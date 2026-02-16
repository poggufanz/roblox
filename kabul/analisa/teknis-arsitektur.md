# Dokumen Arsitektur Teknis: Kabul di Roblox

> **Versi**: 1.0  
> **Target Pembaca**: Tim pemula yang ingin mem port game Kabul dari web ke Roblox  
> **Skill Level**: ⭐ Pemula (Level 1-2)

---

## 1. Pendahuluan

### 1.1 Apa yang Akan Kamu Pelajari

Dokumen ini adalah peta jalan lengkap untuk memindahkan game kartu Kabul dari platform web (React + Firebase) ke Roblox. Bayangkan ini seperti pindah rumah: kita membawa semua barang berharga (logika game, aturan, sistem), tapi menata ulang sesuai tata letak rumah baru (struktur Roblox).

### 1.2 Analogi Restoran

Untuk memudahkan, kita akan pakai analogi **restoran** sepanjang dokumen:

| Komponen Web       | Komponen Roblox   | Peran di Restoran                   |
| ------------------ | ----------------- | ----------------------------------- |
| Firebase (Server)  | Script Server     | Dapur (chef yang masak)             |
| React App (Client) | Script Client     | Meja tamu (pelayan antar pesanan)   |
| GameState          | ReplicatedStorage | Menu yang sama untuk semua          |
| RemoteEvents       | RemoteEvent       | Pelayan yang antar pesanan ke dapur |

---

## 2. Struktur Folder Roblox 101 ⭐

### 2.1 Explorer Hierarchy

Di Roblox Studio, kamu akan lihat jendela **Explorer** di sisi kanan. Ini adalah struktur folder utama yang harus kamu pahami:

```
Workspace/                    ⭐⭐ (Sangat Penting)
├── Environment/              # Meja, kursi, lampu, dekorasi
├── Cards/                    # Kartu 3D yang ada di meja
├── Players/                  # Avatar pemain (otomatis)
└── Camera/                   # Kamera pengamat

ServerScriptService/          ⭐⭐⭐ (Paling Penting)
├── GameLogic/                # Logika game utama
├── TurnManager/              # Pengatur giliran
├── DeckManager/              # Pengelola dek kartu
└── AntiCheat/                # Validasi server

ReplicatedStorage/            ⭐⭐ (Sangat Penting)
├── CardTemplates/            # Template kartu (Prefab)
├── GameState/                # State yang terlihat semua
└── SharedConfigs/            # Konfigurasi bersama

StarterPlayer/                ⭐ (Penting)
└── StarterPlayerScripts/
    ├── ClientController/     # Kontrol input pemain
    ├── UIManager/            # UI lokal
    └── CameraManager/        # Kamera pemain

StarterGui/                   ⭐⭐ (Sangat Penting)
├── MainUI/                   # UI utama game
├── CardDisplay/              # Tampilan kartu
└── NotificationUI/           # Notifikasi

Lighting/                     ⭐ (Penting)
└── Pengaturan cahaya dan atmosfer
```

### 2.2 Penjelasan Detail Setiap Folder

#### Workspace (⭐⭐)

Ini adalah **dunia 3D** yang terlihat oleh pemain. Semua objek fisik ada di sini.

**Analisis dari KabulGame.js baris 77-111:**

```javascript
constructor(gameId, costumeKey = "COSTUME_1") {
  this.state = {
    gameId,
    phase: "WAITING",
    memorizeEndsAt: null,
    players: {},
    turnOrder: [],
    deck: [],
    discardPile: [],
    topDiscard: null,
    // ...
  };
}
```

Di Roblox, state ini dipecah menjadi:

- `Workspace.Cards` untuk kartu fisik yang terlihat
- `ReplicatedStorage.GameState` untuk data logika

#### ServerScriptService (⭐⭐⭐)

Ini adalah **otak** game. Hanya server yang bisa baca tulis di sini. Client tidak bisa lihat sama sekali. Ini tempatnya **FirebaseService.js** (baris 85-1425) pindah ke.

**Mapping kode Firebase ke Roblox:**

| FirebaseService.js | Roblox ServerScriptService      |
| ------------------ | ------------------------------- |
| `createRoom()`     | `RoomManager.createRoom()`      |
| `startGame()`      | `GameManager.startGame()`       |
| `_generateDeck()`  | `DeckManager.generateDeck()`    |
| `performAction()`  | `ActionHandler.processAction()` |

#### ReplicatedStorage (⭐⭐)

Tempat penyimpanan **yang terlihat oleh semua**. Client bisa baca, tapi hanya Server yang bisa tulis (kecuali ada mekanisme khusus).

Fungsi dari `getClientState()` di baris 272-311 KabulGame.js akan menghasilkan data yang disimpan di sini:

```javascript
getClientState(playerId) {
  return {
    myHand: this._maskHand(player, playerId, isMemorize),
    opponents: this._getOpponentsView(playerId),
    topDiscard: this.state.topDiscard,
    // ...
  };
}
```

Di Roblox, hasil return ini akan disimpan di `ReplicatedStorage.GameState` dan otomatis sinkron ke semua client.

---

## 3. Arsitektur Client-Server ⭐⭐⭐

### 3.1 Diagram Alur Komunikasi

```
┌─────────────────────────────────────────────────────────────────┐
│                        SERVER (DataModel)                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ RoomManager  │  │ TurnManager  │  │   ActionValidator    │  │
│  │ - createRoom │  │ - nextTurn   │  │ - validateDraw()     │  │
│  │ - joinRoom   │  │ - skipPlayer │  │ - validateSwap()     │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
│           │                │                    │               │
│           ▼                ▼                    ▼               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                  GameState (Authoritative)               │   │
│  │  { phase, currentTurn, players, deck, discardPile }      │   │
│  └─────────────────────────────────────────────────────────┘   │
│           │                                                    │
│           │ Replicate (otomatis)                               │
│           ▼                                                    │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              ReplicatedStorage.GameState                 │   │
│  │         (Dibaca semua client, hanya server tulis)        │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
           │
           │ RemoteEvents (Request/Response)
           ▼
┌─────────────────────────────────────────────────────────────────┐
│                      CLIENT (LocalPlayer)                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │InputHandler  │  │  UIManager   │  │   CameraManager      │  │
│  │- onClickCard │  │ - showHand() │  │ - focusOnTable()     │  │
│  │- onDrawDeck  │  │ - updateUI() │  │ - focusOnDiscard()   │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
│           │                │                    │               │
│           ▼                ▼                    ▼               │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                  Local State (Prediction)                │   │
│  │         (Optimistik update untuk respons cepat)          │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

### 3.2 Alur Satu Giliran Lengkap

Mari ikuti alur dari FirebaseService.js `performAction()` (baris 579-609) dan petakan ke Roblox:

**Contoh: Pemain mengambil kartu dari Deck**

```
┌─────────┐     ┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Client │────▶│ RemoteEvent │────▶│   Server     │────▶│  Validation │
│ Click   │     │ DrawCard    │     │  menerima    │     │  cek turn   │
│ Deck    │     │ :FireServer │     │  request     │     │  cek phase  │
└─────────┘     └─────────────┘     └──────────────┘     └──────┬──────┘
                                                                │
                              ┌───────────────────────────────────┘
                              ▼
┌─────────┐     ┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Client │◀────│ RemoteEvent │◀────│   Server     │◀────│   Update    │
│ terima  │     │ CardDrawn   │     │  kirim hasil │     │  GameState  │
│ kartu   │     │ :FireClient │     │  ke client   │     │  + deck -1  │
└─────────┘     └─────────────┘     └──────────────┘     └─────────────┘
```

**Mapping ke kode web:**

```javascript
// FirebaseService.js baris 613-694
async _drawCard(roomId, playerId, source) {
  // 1. Validasi turn (server authoritative)
  if (gameState.currentTurn !== playerId) {
    throw new Error('Not your turn');
  }

  // 2. Ambil kartu dari deck
  const drawnCard = deck.shift();

  // 3. Update state
  await set(deckRef, deck);
  await update(gameStateRef, { deckCount: deck.length });

  // 4. Kirim ke client (via Firebase onValue listener)
  return { success: true, card: drawnCard };
}
```

Di Roblox, ini menjadi:

```
ServerScriptService/DeckManager
├── Fungsi: DrawCard(playerId)
├── Validasi: cek turn, cek deck > 0
├── Aksi: ambil kartu, update ReplicatedStorage
└── Kirim: RemoteEvent ke client spesifik
```

---

## 4. RemoteEvents vs Firebase ⭐⭐⭐

### 4.1 Perbandingan Konsep

| Aspek          | Firebase (Web)           | Roblox RemoteEvents             |
| -------------- | ------------------------ | ------------------------------- |
| **Model**      | Publish-Subscribe        | Request-Response + Event        |
| **Listener**   | `onValue(ref, callback)` | `RemoteEvent.OnClientEvent`     |
| **Kirim Data** | `update(ref, data)`      | `RemoteEvent:FireServer(data)`  |
| **Keamanan**   | Security Rules           | Server validation wajib         |
| **Realtime**   | Ya, otomatis             | Ya, dengan overhead lebih kecil |

### 4.2 Mapping Spesifik dari Kode Web

#### A. Room Management (FirebaseService.js baris 93-205)

```javascript
// WEB - Membuat room
async createRoom(roomName, hostId, hostName) {
  const newRoomRef = push(ref(this.db, 'rooms'));
  const roomId = newRoomRef.key;
  await set(newRoomRef, { /* data room */ });
  return { success: true, roomId };
}

// WEB - Join room
async joinRoom(roomId, playerId, playerName) {
  await update(ref(this.db, `rooms/${roomId}/players/${playerId}`), {
    name: playerName,
    hand: [],
    cardCount: 0,
  });
}
```

**Roblox Equivalent:**

```
ServerScriptService/RoomManager
├── RemoteEvent: CreateRoomRequest
├── RemoteEvent: JoinRoomRequest
├── RemoteEvent: RoomUpdated (broadcast ke semua)
└── Struktur Data di ReplicatedStorage:
    Rooms/
    └── {roomId}/
        ├── config/
        ├── gameState/
        └── players/
```

#### B. Game State Sync (FirebaseService.js baris 480-568)

```javascript
// WEB - Listen ke perubahan room
listenToRoom(roomId, playerId, callback) {
  const gameStateListener = onValue(gameStateRef, async (snapshot) => {
    const gameState = snapshot.val();
    await emitCurrentState(gameState);
  });
}

// WEB - Masking data pemain lain
_maskPlayers(players, myPlayerId) {
  const masked = {};
  for (const [pid, player] of Object.entries(players)) {
    masked[pid] = {
      ...player,
      hand: pid === myPlayerId
        ? playerHand  // Lihat sendiri
        : playerHand.map(() => ({ hidden: true }))  // Tutup orang lain
    };
  }
  return masked;
}
```

**Roblox Equivalent:**

```
ReplicatedStorage/GameState (Server-only write)
├── Public/
│   ├── currentTurn: string
│   ├── phase: string
│   ├── topDiscard: CardData
│   └── players/
│       └── {playerId}/
│           ├── name: string
│           ├── cardCount: number
│           └── hasCalledKabul: boolean
│
└── Private/ (Folder per player, script access only)
    └── {playerId}/
        └── hand: CardData[]
```

**Perbedaan Penting:**

Di Roblox, **tidak ada Firebase Security Rules**. Kamu harus buat sistem masking manual di server. Jangan pernah taruh data kartu pemain di ReplicatedStorage langsung.

---

## 5. Keamanan Server-Authoritative ⭐⭐⭐⭐

### 5.1 Prinsip Fundamental

**GOLDEN RULE**: Client hanya boleh **mengirim intent** (niat), bukan **hasil**. Server yang menentukan hasilnya.

**Analogi Restoran:**

- **SALAH**: Client bilang "Saya sudah bayar 100 ribu"
- **BENAR**: Client bilang "Saya pesan nasi goreng", Server yang cek harga dan proses pembayaran

### 5.2 Contoh Implementasi dari Kode Web

**FirebaseService.js baris 613-640: Validasi Draw Card**

```javascript
async _drawCard(roomId, playerId, source) {
  // 1. SELALU validasi turn di server
  if (gameState.currentTurn !== playerId) {
    throw new Error('Not your turn');
  }

  // 2. SELALU validasi phase
  if (gameState.turnPhase !== TURN_PHASE.DRAWING) {
    throw new Error('Invalid turn phase');
  }

  // 3. Baru proses aksi
  const drawnCard = deck.shift();
  // ... update state
}
```

**Roblox Equivalent Pattern:**

```
ServerScriptService/ActionValidator
├── Fungsi: CanDrawCard(playerId)
│   ├── Cek: Apakah playerId == currentTurn?
│   ├── Cek: Apakah phase == "DRAWING"?
│   ├── Cek: Apakah deckCount > 0?
│   └── Return: boolean (boleh/tidak)
│
└── Fungsi: ValidateAction(actionType, playerId, payload)
    ├── Switch berdasarkan actionType
    ├── Panggil validator spesifik
    └── Throw error jika invalid
```

### 5.3 Anti-Cheat Checklist

Berikut checklis yang **WAJIB** diimplementasikan, diambil dari pola FirebaseService.js:

| Aksi       | Validasi Server                            | Referensi Kode Web |
| ---------- | ------------------------------------------ | ------------------ |
| Draw Card  | Cek turn, cek phase, cek deck              | Baris 631-634      |
| Swap Card  | Cek turn, cek phase, cek handIndex valid   | Baris 716-719      |
| Call Kabul | Cek turn == playerId, cek phase == DRAWING | Baris 1247-1248    |
| Peek Card  | Cek abilityState.activePlayer == playerId  | Baris 882-884      |
| Blind Swap | Cek target != self, cek index valid        | Baris 577-588      |

### 5.4 Struktur Folder Keamanan

```
ServerScriptService/
├── AntiCheat/
│   ├── TurnValidator      # Cek giliran valid
│   ├── PhaseValidator     # Cek fase game valid
│   ├── ActionLogger       # Log semua aksi untuk audit
│   └── RateLimiter        # Batasi request per detik
│
└── GameLogic/
    └── SELALU panggil AntiCheat sebelum proses
```

---

## 6. Penanganan Disconnect dan Reconnect ⭐⭐⭐

### 6.1 Masalah yang Harus Dihadapi

Dari `App.jsx` baris 65-84, kita lihat handling leave room:

```javascript
const handleLogout = async () => {
  const roomMatch = location.pathname.match(/^\/room\/(.+)$/);
  if (roomMatch) {
    const roomId = roomMatch[1];
    const firebase = getFirebaseService();
    await firebase.leaveRoom(roomId, player.id);
    firebase.stopListening(roomId);
  }
};
```

Dan dari `FirebaseService.js` baris 211-261:

```javascript
async leaveRoom(roomId, playerId) {
  // Jika game sedang berlangsung dan hanya 1 pemain tersisa
  if (gamePhase === 'MEMORIZE' || gamePhase === 'PLAYING') {
    const remainingPlayers = playerIds.filter(pid => pid !== playerId);

    if (remainingPlayers.length === 1) {
      // Pemain tersisa otomatis menang
      await update(gameStateRef, {
        phase: 'ENDED',
        winner: winnerId,
      });
    }
  }
}
```

### 6.2 Roblox Player Events

Roblox punya event bawaan untuk handle disconnect:

```
Players.PlayerRemoving
└── Dipanggil saat player keluar (disconnect/crash/leave)

Players.PlayerAdded
└── Dipanggil saat player masuk (join baru/reconnect)
```

### 6.3 Strategi Penanganan

#### A. Grace Period (Toleransi Waktu)

```
Konfigurasi:
├── DISCONNECT_GRACE_PERIOD = 60 detik
├── Jika reconnect dalam 60 detik: lanjutkan game
└── Jika lebih dari 60 detik: anggap leave permanen
```

#### B. Data Persistence

```
DataStoreService (penyimpanan persisten Roblox)
├── Simpan state game setiap turn
├── Key: "Kabul_Room_{roomId}_State"
└── Jika server crash + buat server baru:
    └── Load state terakhir, invite pemain rejoin
```

#### C. Handling Berdasarkan Fase Game

| Fase     | Disconnect Pemain   | Sisa Pemain      |
| -------- | ------------------- | ---------------- |
| WAITING  | Hapus dari room     | Lanjut tunggu    |
| MEMORIZE | Pause 30 detik      | Tunggu reconnect |
| PLAYING  | Tandai disconnected | Skip turn-nya    |
| ENDED    | Tidak masalah       | -                |

---

## 7. Pertimbangan Platform ⭐⭐

### 7.1 Mobile vs PC

Roblox berjalan di berbagai platform. Game Kabul harus nyaman dimainkan di semuanya.

#### Perbedaan Input

| Aspek          | PC (Desktop) | Mobile (Touch)      |
| -------------- | ------------ | ------------------- |
| **Kamera**     | Mouse drag   | Swipe dengan 2 jari |
| **Klik Kartu** | Mouse click  | Tap                 |
| **Drag Kartu** | Hold + drag  | Long press + drag   |
| **Zoom**       | Scroll wheel | Pinch (2 jari)      |
| **UI Size**    | Normal       | 1.5x lebih besar    |

#### Solusi dari Kode Web

Dari `App.jsx`, UI game web sudah responsif:

```jsx
<div className="bg-background-light dark:bg-background-dark min-h-screen flex flex-col font-display">
  {/* UI yang adaptif */}
</div>
```

Di Roblox, gunakan:

```
StarterGui/MainUI
├── ScaleUI/              # Untuk PC
│   └── Frame size: 0.3 layar
└── MobileUI/             # Untuk Mobile
    └── Frame size: 0.8 layar

ScreenGui/
└── UIAspectRatioConstraint  # Jaga proporsi
```

### 7.2 Optimasi Performa

#### Rendering Kartu

```
Workspace/Cards/
├── Gunakan MeshParts (bukan Unions) untuk kartu
├── Limit: Maksimal 100 kartu di workspace sekaligus
└── Pooling: Reuse kartu yang sudah tidak terlihat

Optimasi:
├── StreamingEnabled = true (hanya render yang terlihat)
├── Kartu jauh dari kamera: transparan/non-collide
└── Texture kartu: 512x512 maksimal
```

#### Network Optimization

```
ReplicatedStorage/
├── Update GameState: Maksimal 10 kali/detik
├── RemoteEvents: Batch multiple actions
└── Jangan kirim data kartu lawan (sudah di-mask)
```

---

## 8. Asset Meja dan Environment ⭐⭐

### 8.1 Konsep "3D Table Presentation"

Game ini adalah **pengalaman sosial di meja**. Desain harus:

- Membuat pemain merasa duduk di meja kartu
- Kartu terlihat jelas dari sudut pandang pemain
- Atmosfer nyaman untuk bermain lama

### 8.2 Rekomendasi Asset

#### Meja (Table)

| Komponen     | Rekomendasi                  | Alasan                  |
| ------------ | ---------------------------- | ----------------------- |
| **Base**     | Part rounded 8x4 stud        | Cukup untuk 4 pemain    |
| **Material** | Wood (Oak/Maple)             | Klasik, nyaman di mata  |
| **Height**   | 3.5 stud                     | Standar meja kartu      |
| **Surface**  | Smooth, no friction          | Kartu bisa digesek      |
| **Color**    | Coklat tua (RGB: 86, 66, 54) | Warna meja poker klasik |

#### Kursi (Chair)

```
Model: Kursi makan standar Roblox
Modifikasi:
├── Height: 2.5 stud (lebih rendah dari meja)
├── Position: 6 stud dari pusat meja
├── Rotation: Menghadap pusat (0°, 90°, 180°, 270°)
└── Jumlah: 4 kursi (game 2-4 pemain)
```

#### Dekorasi Environment

```
Workspace/Environment/
├── Lighting/
│   ├── Lampu gantung di atas meja
│   ├── Color: Warm white (RGB: 255, 247, 230)
│   └── Range: Cukup menerangi meja saja
│
├── Room/
│   ├── Dinding dengan tekstur wallpaper
│   ├── Jendela (opsional, untuk atmosfer)
│   └── Lantai kayu
│
└── Props/
    ├── Cangkir kopi di pojok meja
    ├── Tumpukan chip poker (dekorasi)
    └── Asbak (untuk atmosfer klasik)
```

### 8.3 Tata Letak Kartu di Meja

```
                 [DECK]
                   ▲
         ┌─────────┴─────────┐
         │                   │
    [P1 POS]             [P2 POS]
         │                   │
    [P4 POS]             [P3 POS]
         │                   │
         └─────────┬─────────┘
                   ▼
              [DISCARD]

Jarak kartu dari pusat meja: 2.5 stud
Tinggi kartu melayang: 0.2 stud (untuk efek 3D)
Rotation kartu: Menghadap ke pemain (untuk kelihatan)
```

---

## 9. Membuat Kartu 3D dengan SurfaceGui ⭐⭐⭐

### 9.1 Konsep SurfaceGui

**SurfaceGui** adalah UI Roblox yang "menempel" di permukaan 3D. Ini solusi sempurna untuk kartu 3D.

**Analogi**: SurfaceGui seperti stiker digital yang bisa kamu tempel di permukaan objek.

### 9.2 Struktur Kartu 3D

```
Workspace/Cards/CardTemplate (Model)
├── CardBase (MeshPart)
│   ├── Size: 2 x 0.05 x 3 (lebar x tebal x tinggi)
│   ├── Material: SmoothPlastic
│   └── Color: Putih (depan), Biru tua (belakang)
│
├── FaceSurface (SurfaceGui)
│   ├── Adornee: CardBase
│   ├── Face: Enum.NormalId.Top
│   ├── SizingMode: PixelsPerStud
│   └── CanvasSize: {200, 300}
│   │
│   └── Content (Frame)
│       ├── Corner (UICorner, radius: 10)
│       ├── RankLabel (TextLabel) - "A", "K", "Q", dll
│       ├── SuitLabel (TextLabel) - "♠", "♥", "♦", "♣"
│       └── CenterArt (ImageLabel) - Gambar kartu
│
└── BackSurface (SurfaceGui)
    ├── Adornee: CardBase
    ├── Face: Enum.NormalId.Bottom
    └── Content: Pattern belakang kartu (bunga-bunga biru)
```

### 9.3 Mapping dari Data Kartu Web

Dari `KabulGame.js` baris 24-49:

```javascript
export const CARD_VALUES = {
  Joker: 0,
  A: 1,
  2: 2,
  /* ... */ K: 13,
};

export const CARD_ABILITIES = {
  7: 'PEEK_SELF',
  8: 'PEEK_SELF',
  9: 'PEEK_ENEMY',
  10: 'PEEK_ENEMY',
  J: 'BLIND_SWAP',
  Q: 'SEE_AND_SWAP',
  K: 'SEE_AND_SWAP',
};
```

Di Roblox, setiap kartu adalah Instance dengan atribut:

```lua
-- Atribut yang disimpan di Setiap Kartu (CardBase)
CardBase:SetAttribute("Rank", "A")
CardBase:SetAttribute("Suit", "♠")
CardBase:SetAttribute("Value", 1)
CardBase:SetAttribute("Ability", "NONE")
CardBase:SetAttribute("Owner", playerId)  -- Untuk validasi
```

### 9.4 Kartu di Tangan vs Kartu di Meja

#### Kartu di Tangan (Lokal)

```
StarterGui/HandUI (ScreenGui)
└── HandContainer (Frame, Horizontal layout)
    ├── CardSlot1 (Frame)
    │   └── CardViewport (ViewportFrame)
    │       └── Clone dari CardTemplate (3D preview)
    ├── CardSlot2 (Frame)
    ├── CardSlot3 (Frame)
    └── CardSlot4 (Frame)

Fitur:
├── Kartu terlihat dari sisi depan (FaceSurface)
├── Klik untuk select/aktifkan
├── Hover: sedikit naik ke atas
└── Selected: glow effect
```

#### Kartu di Meja (Server)

```
Workspace/TableCards/
├── DrawPile/              # Tumpukan kartu draw
│   └── Stack: 50 kartu menumpuk
│
├── DiscardPile/           # Kartu discard terakhir
│   └── TopCard: Terlihat sisi depan
│
└── PlayerCards/
    └── {playerId}/
        ├── Card1: Face down (belakang terlihat)
        ├── Card2: Face down
        ├── Card3: Face down
        └── Card4: Face down

Rotation: Setiap kartu di-rotate menghadap ke pemainnya
```

### 9.5 Animasi Kartu

```
TweenService untuk animasi halus:

1. Draw Animation:
   ├── Start: DrawPile position
   ├── Mid: Naik ke atas meja
   └── End: Hand player (atau Discard jika langsung buang)

2. Swap Animation:
   ├── Kartu baru: dari Hand ke Table
   ├── Kartu lama: dari Table ke Discard
   └── Concurrent: Dua animasi bersamaan

3. Flip Animation:
   ├── Rotate 90 derajat (samping)
   ├── Ganti SurfaceGui (depan/belakang)
   └── Rotate -90 derajat (selesai)

Duration: 0.3-0.5 detik per animasi
Easing: Enum.EasingStyle.Quad, Enum.EasingDirection.Out
```

---

## 10. Pola Kode Web ke Roblox ⭐⭐⭐

### 10.1 State Management

#### Web (KabulGame.js)

```javascript
class KabulGame {
  constructor() {
    this.state = {
      phase: 'WAITING',
      players: {},
      deck: [],
      // ...
    };
  }

  getClientState(playerId) {
    // Return filtered state untuk satu pemain
  }
}
```

#### Roblox Equivalent

```
ServerScriptService/GameStateManager
├── ReplicatedStorage/GameState (Folder)
│   ├── phase: StringValue
│   ├── currentTurn: StringValue
│   ├── deckCount: IntValue
│   └── players (Folder)
│       └── {playerId} (Folder)
│           ├── name: StringValue
│           ├── cardCount: IntValue
│           └── hasCalledKabul: BoolValue
│
└── PrivateState (ModuleScript, Server-only)
    ├── deck: {CardData}[]
    ├── playerHands: { [playerId]: CardData[] }
    └── discardPile: CardData[]
```

### 10.2 Action Handling

#### Web (FirebaseService.js baris 570-609)

```javascript
async performAction(roomId, playerId, actionType, payload = {}) {
  switch (actionType) {
    case ACTION.DRAW_DECK:
      return this._drawCard(roomId, playerId, 'deck');
    case ACTION.SWAP_CARD:
      return this._swapCard(roomId, playerId, payload.handIndex);
    // ...
  }
}
```

#### Roblox Equivalent

```
ServerScriptService/ActionDispatcher
├── RemoteEvent: ActionRequest
│   └── Parameters: (actionType: string, payload: table)
│
└── Fungsi: ProcessAction(player, actionType, payload)
    ├── Validasi player
    ├── Switch actionType:
    │   ├── "DRAW_DECK" -> panggil DeckManager.Draw()
    │   ├── "SWAP_CARD" -> panggil CardManager.Swap()
    │   └── ...
    └── Return: Result (success/error)
```

### 10.3 Turn Management

#### Web (FirebaseService.js baris 1284-1321)

```javascript
async _advanceTurn(roomId, currentPlayerId) {
  const players = Object.keys(playersSnap.val());
  const currentIndex = players.indexOf(currentPlayerId);
  let nextIndex = (currentIndex + 1) % players.length;
  let nextPlayer = players[nextIndex];

  // Skip Kabul caller
  if (nextPlayer === gameState.kabulCaller) {
    nextIndex = (nextIndex + 1) % players.length;
    nextPlayer = players[nextIndex];
  }

  await update(gameStateRef, {
    currentTurn: nextPlayer,
    turnPhase: TURN_PHASE.DRAWING,
  });
}
```

#### Roblox Equivalent

```
ServerScriptService/TurnManager
├── Data:
│   ├── turnOrder: {playerId}[]
│   ├── currentIndex: number
│   └── kabulCaller: playerId?
│
└── Fungsi: NextTurn()
    ├── Hitung nextIndex
    ├── Cek skip kabulCaller
    ├── Update ReplicatedStorage.currentTurn
    └── FireClient: TurnStarted (ke pemain yang dapat giliran)
```

---

## 11. Audio di Roblox ⭐⭐

### 11.1 Mapping dari SoundContext.jsx

Dari `SoundContext.jsx` baris 28-36:

```javascript
const soundFiles = {
  'card-flip': '/sounds/card-flip.mp3',
  'card-deal': '/sounds/card-deal.mp3',
  'card-slap': '/sounds/card-slap.mp3',
  'kabul-call': '/sounds/kabul-call.mp3',
  'turn-notification': '/sounds/turn-notification.mp3',
  success: '/sounds/success.mp3',
  error: '/sounds/error.mp3',
};
```

### 11.2 Roblox Audio Setup

```
ReplicatedStorage/Audio/
├── SFX/ (Sound instances)
│   ├── CardFlip (Sound)
│   │   ├── SoundId: rbxassetid://...
│   │   └── Volume: 0.5
│   ├── CardDeal (Sound)
│   ├── CardSlap (Sound)
│   ├── KabulCall (Sound)
│   ├── TurnNotification (Sound)
│   ├── Success (Sound)
│   └── Error (Sound)
│
└── BGM/ (Background Music)
    └── LoungeMusic (Sound, Loop = true)
```

### 11.3 Audio Player (Client)

```
StarterPlayerScripts/AudioManager
├── Function: PlaySFX(soundName)
│   ├── Ambil Sound dari ReplicatedStorage
│   ├── Clone ke SoundService
│   └── Play()
│
├── Function: PlayTurnNotification()
│   └── Hanya untuk local player yang dapat giliran
│
└── Function: SetMuted(boolean)
    └── Loop semua Sound, set Volume = 0
```

---

## 12. Referensi Dokumentasi Roblox ⭐

### 12.1 Link Dokumentasi Penting

| Topik                       | URL                                                                      |
| --------------------------- | ------------------------------------------------------------------------ |
| **Server-Client Model**     | https://create.roblox.com/docs/scripting/server-client-communication     |
| **RemoteEvents**            | https://create.roblox.com/docs/scripting/events/remote-events            |
| **ReplicatedStorage**       | https://create.roblox.com/docs/scripting/data-storage/replicated-storage |
| **SurfaceGui**              | https://create.roblox.com/docs/ui/surface-guis                           |
| **TweenService**            | https://create.roblox.com/docs/scripting/animation/tweening              |
| **DataStoreService**        | https://create.roblox.com/docs/scripting/data-storage/data-stores        |
| **Player Events**           | https://create.roblox.com/docs/scripting/players/player-events           |
| **Best Practices Security** | https://create.roblox.com/docs/scripting/security                        |

### 12.2 Tutorial Rekomendasi

1. **Official Roblox Creator Documentation** - Dokumentasi lengkap dengan contoh
2. **Roblox Developer Forum** - Komunitas aktif untuk tanya jawab
3. **DevHub Learning Paths** - Jalur belajar terstruktur

---

## 13. Checklist Implementasi ⭐⭐⭐

### Phase 1: Setup Dasar (⭐)

- [ ] Buat struktur folder di ReplicatedStorage
- [ ] Setup ServerScriptService modules
- [ ] Buat meja 3D dasar
- [ ] Setup kamera pengamat
- [ ] Test join/leave pemain

### Phase 2: Kartu 3D (⭐⭐)

- [ ] Buat template kartu dengan SurfaceGui
- [ ] Buat sistem deck (generate, shuffle)
- [ ] Implementasi draw/discard animation
- [ ] Tampilkan kartu di tangan pemain
- [ ] Tampilkan kartu di meja (face down)

### Phase 3: Game Logic (⭐⭐⭐)

- [ ] Implementasi turn management
- [ ] Validasi aksi (server authoritative)
- [ ] Handle kartu ability (7/8, 9/10, J, Q/K)
- [ ] Implementasi Kabul call
- [ ] Score calculation dan end game

### Phase 4: Polish (⭐⭐)

- [ ] Tambah sound effects
- [ ] Optimasi untuk mobile
- [ ] Handle disconnect/reconnect
- [ ] UI feedback untuk semua aksi
- [ ] Bug fix dan testing

---

## 14. Kesimpulan

Memindahkan Kabul dari web ke Roblox adalah proses yang melibatkan **pemahaman ulang arsitektur**, bukan sekadar copy-paste kode. Poin-poin kunci:

1. **Firebase** menjadi **ServerScriptService** dengan ReplicatedStorage
2. **React State** menjadi **Value Objects** di ReplicatedStorage
3. **HTTP Requests** menjadi **RemoteEvents**
4. **Server authoritative** adalah **wajib**, bukan pilihan
5. **SurfaceGui** adalah solusi kartu 3D yang tepat

Mulai dari yang kecil: buat meja dan satu kartu yang bisa di-draw. Setelah itu, bangun sistem turn dan validasi. Terakhir, polish dengan audio dan animasi.

**Selamat membangun!**

---

## Appendix A: Skill Level Legend

| Level    | Simbol   | Deskripsi                                     |
| -------- | -------- | --------------------------------------------- |
| Pemula   | ⭐       | Konsep dasar, wajib dipahami semua tim        |
| Menengah | ⭐⭐     | Memerlukan pemahaman Roblox dasar             |
| Lanjutan | ⭐⭐⭐   | Butuh pengalaman scripting Roblox             |
| Expert   | ⭐⭐⭐⭐ | Kompleks, perlu pemahaman arsitektur mendalam |

---

## Appendix B: Glossary

| Istilah                 | Penjelasan                                |
| ----------------------- | ----------------------------------------- |
| **Client**              | Game yang berjalan di perangkat pemain    |
| **Server**              | Komputer Roblox yang mengatur logika game |
| **RemoteEvent**         | Jembatan komunikasi Client <-> Server     |
| **ReplicatedStorage**   | Penyimpanan yang terlihat oleh semua      |
| **SurfaceGui**          | UI yang menempel di permukaan 3D          |
| **TweenService**        | Sistem animasi bawaan Roblox              |
| **ServerScriptService** | Folder script yang hanya jalan di server  |
| **MeshPart**            | Objek 3D dengan bentuk kompleks           |
| **Prefab**              | Template objek yang bisa di-clone         |

---

_Dokumen ini dibuat untuk tim Kabul Roblox. Update terakhir: Februari 2026_
