# Dokumen Mapping Database: Firebase ke Roblox DataStore

## Ringkasan Eksekutif

Dokumen ini menjelaskan bagaimana struktur data Firebase yang digunakan dalam aplikasi web Kabul Card Game dipetakan ke sistem penyimpanan Roblox. Pemetaan ini penting karena perbedaan fundamental antara arsitektur web-based (Firebase Realtime Database) dan arsitektur Roblox (server-based dengan DataStoreService).

**Target Pembaca:** Tim pengembang Kabul Card Game yang akan melakukan migrasi dari web ke platform Roblox, dengan pemahaman bahwa pembaca bisa jadi pemula dalam pengembangan game.

**Hasil Akhir:** Setelah membaca dokumen ini, tim akan memahami:

- Perbedaan cara kerja Firebase vs Roblox storage
- Apa data yang perlu disimpan permanen vs data yang hanya ada di memori
- Bagaimana struktur data saat ini dipindahkan ke Roblox
- Kapan data hilang dan kapan data tersimpan

---

## ⭐ Pemahaman Dasar: Firebase vs Roblox Storage

### Analogi Sederhana: Lemari Arsip vs Meja Permainan

⭐ **Untuk Pemula:** Bayangkan perbedaan penyimpanan data Firebase dan Roblox seperti perbedaan antara lemari arsip dan meja permainan.

**Firebase (Lemari Arsip Bersama Online)**

Firebase Realtime Database ibarat lemari arsip besar yang terhubung ke internet. Semua orang bisa membuka lemari ini dari mana saja, melihat isinya, dan mengambil atau menyimpan dokumen. Lemari ini selalu ada, meski semua orang sudah pulang ke rumah. Dokumen di dalamnya tersimpan aman dan bisa diakses kapan saja.

Dalam konteks Kabul web:

- Room adalah "folder" di dalam lemari arsip
- Game state adalah "dokumen" yang selalu diperbarui
- Semua pemain melihat dokumen yang sama secara real-time
- Server Firebase (bukan server kita) yang mengelola lemari ini

**Roblox Server (Meja Permainan Langsung)**

Roblox bekerja dengan cara yang berbeda. Bayangkan sebuah meja permainan di ruangan tertutup. Semua pemain harus datang ke ruangan yang sama (server yang sama) untuk bermain. Kartu-kartu diletakkan di atas meja. Semua orang di ruangan itu bisa melihat kartu, tapi begitu semua orang pergi dan ruangan ditutup, meja dibersihkan dan kartu-kartu diangkat. Tidak ada yang tersisa.

Dalam konteks Kabul Roblox:

- Setiap server Roblox adalah "ruangan" dengan "meja" sendiri
- Game state adalah "kartu-kartu di atas meja" (hanya di memori)
- Pemain harus bergabung ke server yang sama untuk bermain bersama
- Begitu server mati atau semua pemain keluar, data permainan hilang

**Mengapa Perbedaan Ini Penting?**

Perbedaan fundamental ini mempengaruhi bagaimana kita mendesain sistem data:

| Aspek       | Firebase (Web)                  | Roblox Server             |
| ----------- | ------------------------------- | ------------------------- |
| Lokasi data | Cloud (server Google)           | Server game Roblox        |
| Persistensi | Selalu tersimpan                | Hilang saat server mati   |
| Akses       | Dari mana saja                  | Harus di server yang sama |
| Multi-room  | Banyak room dalam satu database | 1 server = 1 room         |
| Biaya       | Berdasarkan penggunaan          | Termasuk dalam platform   |

⭐ **Key Takeaway:** Di Roblox, kita tidak perlu sistem "room" yang kompleks seperti Firebase karena setiap server Roblox secara otomatis ISOLATED dan menjadi satu "room" tersendiri.

---

## Struktur Data Firebase Saat Ini

### Overview Database Kabul Web

Berdasarkan file `FirebaseService.js`, struktur database Firebase Kabul Card Game adalah sebagai berikut:

```
/rooms/{roomId}/
  ├── config/           # Pengaturan room
  │   ├── name          # Nama room
  │   ├── hostId        # ID pembuat room
  │   ├── hostName      # Nama pembuat room
  │   ├── costume       # Kostum yang dipilih
  │   └── createdAt     # Waktu pembuatan
  │
  ├── gameState/        # Status permainan
  │   ├── phase         # WAITING | MEMORIZE | PLAYING | ENDED
  │   ├── currentTurn   # ID pemain yang giliran
  │   ├── turnPhase     # DRAWING | DISCARDING | RESOLVING_ABILITY | ...
  │   ├── abilityState  # State kemampuan kartu yang aktif
  │   ├── topDiscard    # Kartu teratas di discard pile
  │   ├── deckCount     # Jumlah kartu di deck
  │   ├── kabulCaller   # ID pemain yang memanggil KABUL
  │   ├── finalTurnsRemaining # Giliran tersisa setelah KABUL
  │   └── winner        # ID pemenang
  │
  ├── players/{playerId}/  # Data setiap pemain
  │   ├── name          # Nama pemain
  │   ├── isHost        # Apakah host
  │   ├── hand          # Kartu di tangan (array)
  │   ├── cardCount     # Jumlah kartu
  │   ├── score         # Skor
  │   └── hasCalledKabul # Sudah panggil KABUL?
  │
  ├── deck/             # Kartu-kartu di deck (server only)
  ├── discardPile/      # Kartu yang sudah dibuang
  └── private/{playerId}/  # Data privat per pemain
      ├── revealedCard  # Kartu yang sedang diintip
      └── drawnCard     # Kartu yang baru diambil
```

### Penjelasan Setiap Node

**Node `/rooms/{roomId}/config/`**

Node ini menyimpan informasi dasar tentang room. Setiap kali pemain membuat room baru, data ini diinisialisasi. Costume menentukan aturan nilai kartu spesial (Joker dan Red King).

**Node `/rooms/{roomId}/gameState/`**

Ini adalah "jantung" dari game state. Node ini berubah terus-menerus sepanjang permainan. Phase mengontrol alur permainan: dari menunggu pemain (WAITING), ke fase menghafal kartu (MEMORIZE), ke permainan aktif (PLAYING), hingga berakhir (ENDED).

**Node `/rooms/{roomId}/players/{playerId}/`**

Setiap pemain memiliki node sendiri. Hand array menyimpan objek kartu lengkap dengan rank, suit, value, dan display. Security rules Firebase memastikan pemain hanya bisa membaca hand miliknya sendiri.

**Node `/rooms/{roomId}/deck/`**

Node ini menyimpan array kartu yang tersisa di deck. Firebase security rules melarang client membaca node ini, sehingga hanya server (melalui Cloud Functions atau trusted environment) yang bisa melihat isi deck. Ini mencegah cheating.

**Node `/rooms/{roomId}/private/{playerId}/`**

Node privat untuk setiap pemain. Data di sini sementara dan spesifik untuk aksi tertentu, seperti kartu yang sedang diintip selama 3 detik, atau kartu yang baru diambil dari deck.

---

## Mapping ke Roblox: Konsep Fundamental

### 1. Room menjadi Server Instance

⭐ **Konsep Penting:** Di Roblox, kita TIDAK PERLU membuat sistem room.

Di Firebase, kita perlu sistem room karena:

- Satu aplikasi web melayani banyak grup pemain secara bersamaan
- Setiap grup perlu ruang permainan terpisah
- Database terpusat menyimpan semua room

Di Roblox, platform sudah menangani ini:

- Setiap kali pemain memulai game, Roblox membuat server baru
- Server ini sepenuhnya terisolasi dari server lain
- Maksimum pemain per server bisa diatur (untuk Kabul: 4 pemain)
- Tidak ada cara bagi pemain di server A melihat server B

**Implikasi:** Seluruh struktur `/rooms/{roomId}/` di Firebase tidak perlu dipindahkan ke Roblox. Kita langsung menyimpan data di level root server.

**Firebase:**

```
/rooms/roomABC123/gameState/phase
```

**Roblox:**

```
GameState.phase (langsung di server memory)
```

### 2. GameState menjadi Module Table di Server

Di Firebase, gameState adalah node JSON yang di-sync real-time. Di Roblox, kita menggunakan ModuleScript atau table di Script (server) untuk menyimpan state.

**Struktur GameState di Roblox:**

Berdasarkan `KabulGame.js` baris 79-100, struktur state yang sama bisa direpresentasikan sebagai table di Luau:

```
GameState = {
    gameId = "auto-generated",
    phase = "WAITING",
    memorizeEndsAt = nil,

    players = {}, -- table dengan key = Player object
    turnOrder = {}, -- array urutan pemain
    currentTurnIndex = 1,

    deck = {}, -- array kartu
    discardPile = {}, -- array kartu
    topDiscard = nil,

    drawnCard = nil, -- { playerId, card }
    pendingAction = nil, -- untuk ability

    kabulCaller = nil,
    finalTurnsRemaining = 0,
    winner = nil,

    costume = "COSTUME_1"
}
```

⭐ **Perbedaan Penting:** Table di Roblox server hanya ada di RAM. Jika server crash atau restart, semua data hilang. Ini berbeda dengan Firebase yang menyimpan data di disk.

### 3. Players menjadi game.Players Service

Di Firebase, kita menyimpan data pemain secara manual dengan ID unik. Di Roblox, platform menyediakan service `game.Players` yang otomatis mengelola pemain yang terhubung.

**Perbandingan:**

| Firebase                   | Roblox                            |
| -------------------------- | --------------------------------- |
| players/{playerId}/        | game.Players:GetPlayers()         |
| Manual ID generation       | Player.UserId (dari Roblox)       |
| Manual connection tracking | Player.Connected event            |
| Custom data structure      | bisa attach data ke Player object |

**Cara Kerja di Roblox:**

Ketika pemain bergabung ke server:

1. Event `game.Players.PlayerAdded` ter-trigger
2. Kita bisa simpan data pemain dalam table terpisah, menggunakan Player object sebagai key
3. Data hand, score, dan status pemain disimpan dalam struktur terpisah

### 4. Deck menjadi Array di Server Memory

Di Firebase, deck disimpan di database dengan security rules khusus. Di Roblox, karena seluruh logika game berjalan di server, kita bisa menyimpan deck sebagai array/table biasa di server script.

**Contoh struktur deck:**

Berdasarkan `kabulGameState.json` dan `KabulGame.js`, deck adalah array 54 kartu:

- 52 kartu standar (A-K, 4 suit)
- 2 Joker

Di Roblox, ini menjadi:

```
Deck = {
    { rank = "A", suit = "♥", value = 1, display = "A♥" },
    { rank = "A", suit = "♦", value = 1, display = "A♦" },
    -- ... 52 kartu lainnya
    { rank = "Joker", suit = nil, value = -1, display = "🃏" },
    { rank = "Joker", suit = nil, value = -1, display = "🃏" }
}
```

**Keunggulan di Roblox:** Tidak perlu security rules kompleks karena client tidak bisa akses server memory.

### 5. Private Data menjadi Per-Player State

Di Firebase, node `/private/{playerId}/` menyimpan data sensitif sementara. Di Roblox, cara yang sama bisa dilakukan dengan menyimpan data privat dalam table terpisah.

**Struktur Private Data di Roblox:**

```
PlayerPrivateData = {
    [Player1] = {
        revealedCard = nil, -- kartu yang sedang diintip
        drawnCard = nil     -- kartu yang baru diambil
    },
    [Player2] = { ... }
}
```

---

## Data Volatile vs Persistent

⭐ **Konsep Kritis:** Memahami kapan data hilang dan kapan data tersimpan.

### Data Volatile (Hilang Saat Server Mati)

Data volatile adalah data yang hanya ada di memori server dan hilang begitu server mati atau semua pemain keluar.

**Contoh Data Volatile di Kabul:**

| Data          | Contoh                 | Kenapa Volatile?               |
| ------------- | ---------------------- | ------------------------------ |
| Game State    | Phase, turn, deck      | Hanya relevan selama permainan |
| Hand pemain   | Kartu di tangan        | Spesifik untuk satu sesi       |
| Discard pile  | Kartu yang dibuang     | Tidak perlu disimpan           |
| Ability state | Peek, swap in progress | Sementara dan transien         |
| Drawn card    | Kartu yang diambil     | Hanya untuk giliran itu        |

**Analogi:** Seperti menulis di papan tulis. Semua orang di ruangan bisa melihat, tapi begitu ruangan ditutup, papan dibersihkan.

### Data Persistent (Tersimpan Permanen)

Data persistent adalah data yang disimpan menggunakan DataStoreService dan akan ada meski server mati atau pemain keluar.

**Contoh Data Persistent di Kabul:**

| Data         | Contoh                       | Kenapa Persistent?      |
| ------------ | ---------------------------- | ----------------------- |
| Player stats | Wins, losses, games played   | Riwayat pemain          |
| Preferences  | Sound muted, costume favorit | Preferensi personal     |
| Achievement  | Trophy, badge progress       | Progress jangka panjang |

**Analogi:** Seperti menulis di buku catatan dan menyimpannya di lemari. Bisa dibuka lagi kapan saja.

### Penjelasan Detail: Kapan Data Hilang vs Tersimpan

**Skenario 1: Game Berlangsung Normal**

1. Pemain A dan B bergabung ke server
2. Game dimulai, state tersimpan di memori server
3. Pemain bermain selama 10 menit
4. Game selesai, pemenang ditentukan
5. **Stats pemenang DISIMPAN ke DataStore** (persistent)
6. **Game state DIHAPUS dari memori** (volatile)

**Skenario 2: Server Crash di Tengah Game**

1. Pemain A, B, C sedang bermain
2. Tiba-tiba server crash (jaringan error, bug, dll)
3. Semua pemain terputus
4. **Seluruh game state HILANG** (volatile)
5. **Stats tidak berubah** karena belum disimpan
6. Pemain harus mulai game baru di server lain

**Skenario 3: Pemain Disconnect dan Reconnect**

1. Pemain A bermain, kemudian disconnect (wifi mati)
2. Game berjalan tanpa pemain A
3. Jika pemain A reconnect dalam waktu singkat:
   - Server masih aktif
   - Game state masih ada
   - Pemain A bisa lanjut bermain
4. Jika pemain A reconnect terlalu lama:
   - Server mungkin sudah mati (tidak ada pemain)
   - Game state hilang
   - Pemain A harus cari game baru

---

## DataStoreService untuk Stats

### Apa Itu DataStoreService?

DataStoreService adalah layanan bawaan Roblox untuk menyimpan data persistent. Ini pengganti Firebase untuk data yang perlu tersimpan lama.

**Karakteristik DataStoreService:**

| Karakteristik | Detail                |
| ------------- | --------------------- |
| Batas ukuran  | 4MB per key           |
| Rate limit    | 60 requests per menit |
| Scope         | Per game (universe)   |
| Access        | Async (perlu await)   |
| Key format    | String unik           |

### Struktur DataStore untuk Kabul

Berdasarkan `PlayerStats.js`, kita perlu menyimpan stats sederhana:

**Key Format:** `PlayerStats_{UserId}`

**Value Structure:**

```
{
    wins = 0,
    losses = 0,
    gamesPlayed = 0,
    lastUpdated = timestamp
}
```

**Contoh penggunaan:**

Ketika game selesai dan pemain A menang:

1. Load data dari DataStore: `GetAsync("PlayerStats_12345")`
2. Update wins += 1, gamesPlayed += 1
3. Save kembali: `SetAsync("PlayerStats_12345", data)`

### Best Practices DataStore

**1. Retry Logic**

DataStore bisa gagal. Selalu gunakan pcall (protected call) dan retry mechanism:

- Coba simpan data
- Jika gagal, tunggu dan coba lagi
- Maksimal 3-5 kali percobaan

**2. Batching**

Jangan simpan data terlalu sering:

- Simpan stats hanya saat game selesai
- Jangan simpan setiap giliran
- Gunakan cache di memory, simpan periodik

**3. Data Validation**

Selalu validasi data sebelum simpan:

- Pastikan wins dan losses adalah angka
- Pastikan tidak ada nilai negatif
- Pastikan gamesPlayed = wins + losses

---

## Diagram Perbandingan Lengkap

### Tabel Mapping Firebase ke Roblox

| Firebase Path                        | Roblox Equivalent           | Tipe Data | Volatile/Persistent |
| ------------------------------------ | --------------------------- | --------- | ------------------- |
| `/rooms/{id}/config/`                | Tidak perlu (server = room) | -         | -                   |
| `/rooms/{id}/gameState/phase`        | `GameState.phase`           | String    | Volatile            |
| `/rooms/{id}/gameState/currentTurn`  | `GameState.currentTurn`     | Player    | Volatile            |
| `/rooms/{id}/gameState/turnPhase`    | `GameState.turnPhase`       | String    | Volatile            |
| `/rooms/{id}/gameState/abilityState` | `GameState.abilityState`    | Table     | Volatile            |
| `/rooms/{id}/gameState/topDiscard`   | `GameState.topDiscard`      | Table     | Volatile            |
| `/rooms/{id}/gameState/deckCount`    | `#Deck` (length operator)   | Number    | Volatile            |
| `/rooms/{id}/gameState/kabulCaller`  | `GameState.kabulCaller`     | Player    | Volatile            |
| `/rooms/{id}/players/{id}/`          | `PlayerData[Player]`        | Table     | Volatile            |
| `/rooms/{id}/players/{id}/hand`      | `PlayerData[Player].hand`   | Array     | Volatile            |
| `/rooms/{id}/players/{id}/name`      | `Player.DisplayName`        | String    | Built-in            |
| `/rooms/{id}/deck/`                  | `Deck` array                | Array     | Volatile            |
| `/rooms/{id}/discardPile/`           | `DiscardPile` array         | Array     | Volatile            |
| `/rooms/{id}/private/{id}/`          | `PrivateData[Player]`       | Table     | Volatile            |
| `/rooms/{id}/chat/`                  | `TextChatService`           | -         | Built-in            |
| N/A (localStorage)                   | `DataStoreService`          | API       | Persistent          |

### Diagram Arsitektur Firebase (Web)

```
┌─────────────────────────────────────────────────────────────┐
│                    FIREBASE CONSOLE                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              REALTIME DATABASE                       │   │
│  │  ┌──────────────────────────────────────────────┐  │   │
│  │  │ /rooms                                        │  │   │
│  │  │   ├─ roomABC123/                              │  │   │
│  │  │   │   ├─ config/ (name, host, costume)        │  │   │
│  │  │   │   ├─ gameState/ (phase, turn, etc)        │  │   │
│  │  │   │   ├─ players/                             │  │   │
│  │  │   │   │   ├─ player1/ (hand, score, etc)      │  │   │
│  │  │   │   │   └─ player2/ (hand, score, etc)      │  │   │
│  │  │   │   ├─ deck/ (array kartu)                  │  │   │
│  │  │   │   ├─ discardPile/ (array kartu)           │  │   │
│  │  │   │   └─ private/ (data privat per player)    │  │   │
│  │  │   └─ roomXYZ789/ (room lain)                 │  │   │
│  │  └──────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
         ▲                              ▲
         │                              │
┌────────┴────────┐            ┌────────┴────────┐
│   PEMAIN A      │            │   PEMAIN B      │
│  (Web Browser)  │◄──────────►│  (Web Browser)  │
│                 │   SYNC     │                 │
└─────────────────┘            └─────────────────┘
```

### Diagram Arsitektur Roblox

```
┌─────────────────────────────────────────────────────────────┐
│                  ROBLOX SERVER                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │              SERVER MEMORY (RAM)                     │   │
│  │  ┌──────────────────────────────────────────────┐  │   │
│  │  │ GameState                                     │  │   │
│  │  │   ├─ phase = "PLAYING"                        │  │   │
│  │  │   ├─ currentTurn = PlayerA                    │  │   │
│  │  │   ├─ deck = {array 54 kartu}                  │  │   │
│  │  │   └─ ...                                      │  │   │
│  │  ├──────────────────────────────────────────────┤  │   │
│  │  │ PlayerData[PlayerA]                           │  │   │
│  │  │   ├─ hand = {kartu A♥, 7♠, ...}               │  │   │
│  │  │   ├─ score = 15                               │  │   │
│  │  │   └─ hasCalledKabul = false                   │  │   │
│  │  └──────────────────────────────────────────────┘  │   │
│  │                                                      │   │
│  │  ┌──────────────────────────────────────────────┐  │   │
│  │  │ DATASTORESERVICE (Persistent)                │  │   │
│  │  │   Key: "PlayerStats_12345"                   │  │   │
│  │  │   Value: {wins: 10, losses: 5, ...}          │  │   │
│  │  └──────────────────────────────────────────────┘  │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
         ▲                              ▲
         │      REMOTE EVENTS           │
┌────────┴────────┐            ┌────────┴────────┐
│   PEMAIN A      │◄──────────►│   PEMAIN B      │
│  (Roblox App)   │            │  (Roblox App)   │
│                 │            │                 │
└─────────────────┘            └─────────────────┘
```

### Diagram Alur Data: Giliran Bermain

**Firebase (Web):**

```
Pemain A klik "Draw Card"
         │
         ▼
┌──────────────────┐
│ Client kirim     │
│ request ke       │
│ Firebase         │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Firebase update  │
│ gameState di DB  │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Semua client     │
│ ter-notifikasi   │
│ perubahan        │
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
Pemain A  Pemain B
melihat   melihat
update    update
```

**Roblox:**

```
Pemain A klik "Draw Card"
         │
         ▼
┌──────────────────┐
│ Client fire      │
│ RemoteEvent      │
│ ke Server        │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Server proses    │
│ logika game      │
│ (update GameState)│
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Server fire      │
│ RemoteEvent      │
│ ke semua client  │
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
Pemain A  Pemain B
melihat   melihat
update    update
```

---

## Mapping Konsep Spesifik

### 1. Room Management

**Firebase:**

- Pemain membuat room dengan `createRoom()`
- Room disimpan di `/rooms/{roomId}/`
- Pemain lain join dengan `joinRoom(roomId)`
- Bisa ada banyak room aktif bersamaan

**Roblox:**

- Pemain tekan "Play" di halaman game Roblox
- Roblox otomatis assign ke server (existing atau baru)
- 1 server = 1 "room" dengan max 4 pemain
- Tidak ada konsep room ID yang bisa dipilih

**Perbedaan Fundamental:**

| Aspek           | Firebase           | Roblox                   |
| --------------- | ------------------ | ------------------------ |
| Cara buat room  | API call           | Tekan Play               |
| Pilih room      | Bisa (dari list)   | Tidak bisa (auto-assign) |
| Invite teman    | Share roomId       | Share server link        |
| Max room        | Unlimited          | Unlimited servers        |
| Pemain per room | 4 (app constraint) | 4 (Roblox setting)       |

### 2. Game State Synchronization

**Firebase:**

```javascript
// Client subscribe ke changes
onValue(gameStateRef, (snapshot) => {
  const gameState = snapshot.val();
  updateUI(gameState);
});
```

Semua client otomatis ter-update saat data di DB berubah.

**Roblox:**

```
-- Server mengirim update ke client
GameStateUpdateEvent:FireAllClients(gameState)

-- Client menerima dan update UI
GameStateUpdateEvent.OnClientEvent:Connect(function(gameState)
    updateUI(gameState)
end)
```

Server yang mengontrol kapan dan apa yang dikirim ke client.

### 3. Player Hand Security

**Firebase:**

- Hand pemain disimpan di database
- Security rules memastikan pemain hanya bisa baca hand sendiri
- Server bisa baca semua hand (untuk logika game)

**Roblox:**

- Hand pemain disimpan di server memory (table)
- Client tidak bisa akses server memory langsung
- Server mengirim hand spesifik ke client yang bersangkutan saja
- Lebih aman karena tidak ada "client-side validation" yang bisa di-hack

### 4. Chat System

**Firebase:**

- Custom implementation dengan `/rooms/{id}/chat/`
- Push message baru ke array
- Subscribe untuk real-time update

**Roblox:**

- Gunakan `TextChatService` bawaan Roblox
- Tidak perlu implementasi custom
- Sudah termasuk moderation otomatis
- Bisa di-disable jika tidak diperlukan

---

## Analogi "Lemari Arsip" vs "Meja"

⭐ **Penjelasan untuk Non-Programmer**

### Lemari Arsip (Firebase)

Bayangkan Firebase seperti lemari arsip kantor yang besar dan canggih:

**Karakteristik Lemari Arsip:**

- **Sentral**: Ada di satu tempat pusat (cloud)
- **Persistent**: Dokumen tersimpan bahkan saat tidak ada yang membuka
- **Multi-access**: Banyak orang bisa akses dari lokasi berbeda
- **Organized**: Setiap dokumen punya ID dan lokasi spesifik
- **Backup**: Tidak hilang meski listrik mati

**Dalam Konteks Kabul:**

- Setiap room adalah folder di lemari
- Setiap game state adalah dokumen di folder
- Semua pemain melihat dokumen yang sama
- Server Firebase adalah "penjaga lemari"

### Meja Permainan (Roblox Server)

Bayangkan Roblox server seperti meja permainan di ruangan privat:

**Karakteristik Meja:**

- **Lokal**: Ada di satu ruangan tertentu (server)
- **Volatile**: Dibersihkan saat semua orang pergi
- **Single-location**: Harus datang ke ruangan untuk bermain
- **Fisik**: Kartu nyata di atas meja (di memory)
- **No backup**: Hilang saat game selesai

**Dalam Konteks Kabul:**

- Setiap server adalah ruangan dengan meja sendiri
- Kartu-kartu di atas meja adalah game state
- Pemain harus masuk ruangan untuk bermain
- Server Roblox adalah "ruangan permainan"

### Kapan Menggunakan Masing-Masing?

**Gunakan Lemari Arsip (DataStore) untuk:**

- Stats pemain (wins, losses)
- Achievement dan progress
- Preferensi setting
- Data yang perlu tahan lama

**Gunakan Meja (Server Memory) untuk:**

- Kartu di tangan pemain
- Status giliran
- Deck dan discard pile
- Data yang hanya relevan selama permainan

### Ilustrasi Visual

```
┌──────────────────────────────────────────────────────────────┐
│                    KANTOR PUSAT                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │              LEMARI ARSIP (DataStore)                 │  │
│  │                                                       │  │
│  │  ┌─────────────────────────────────────────────┐     │  │
│  │  │ Player Stats                                 │     │  │
│  │  │  ├─ Pemain A: 10 menang, 5 kalah            │     │  │
│  │  │  ├─ Pemain B: 8 menang, 7 kalah             │     │  │
│  │  │  └─ Pemain C: 15 menang, 3 kalah            │     │  │
│  │  └─────────────────────────────────────────────┘     │  │
│  │                                                       │  │
│  │  [TERSIMPAN PERMANEN - Bisa dibuka kapan saja]       │  │
│  └───────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
                              │
                              │ Internet
                              ▼
┌──────────────────────────────────────────────────────────────┐
│                   RUANGAN PERMAINAN 1                        │
│  ┌───────────────────────────────────────────────────────┐  │
│  │                    MEJA (Server)                      │  │
│  │                                                       │  │
│  │    Pemain A           Pemain B                      │  │
│  │   ┌─────┐            ┌─────┐                        │  │
│  │   │ A♥  │            │ K♠  │   ← Kartu di tangan    │  │
│  │   │ 7♠  │            │ J♦  │     (volatile)         │  │
│  │   └─────┘            └─────┘                        │  │
│  │                                                       │  │
│  │         [Giliran: Pemain A]                          │  │
│  │                                                       │  │
│  │              ┌─────────┐                             │  │
│  │              │ DECK    │                             │  │
│  │              │ 46 kartu│                             │  │
│  │              └─────────┘                             │  │
│  │                                                       │  │
│  │  [HILANG SAAT GAME SELESAI / SERVER TUTUP]           │  │
│  └───────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

---

## Best Practices dan Rekomendasi

### 1. Struktur Folder Roblox

Berdasarkan analisis database mapping, berikut struktur folder yang direkomendasikan di Roblox Studio:

```
ServerScriptService/
├── GameLogic/
│   ├── MainGame.server.lua       # Entry point
│   ├── GameState.lua             # State management
│   ├── CardManager.lua           # Deck, shuffle, deal
│   ├── TurnManager.lua           # Turn rotation
│   └── Scoring.lua               # Score calculation
├── Network/
│   ├── RemoteEvents/             # Semua RemoteEvents
│   └── EventHandlers.server.lua  # Handler functions
└── Data/
    ├── PlayerData.lua            # Volatile player data
    └── StatsStore.lua            # DataStore wrapper

ReplicatedStorage/
├── Shared/
│   ├── CardDefinitions.lua       # CARD_VALUES, CARD_ABILITIES
│   ├── GameConfig.lua            # Constants (duration, etc)
│   └── Utils.lua                 # Helper functions
└── Assets/
    └── CardImages/               # Decals untuk kartu

StarterGui/
└── GameUI/
    ├── MainHUD/                  # HUD utama
    ├── CardDisplay/              # Tampilan kartu
    └── Notification/             # Toast/notifikasi

StarterPlayer/
└── StarterPlayerScripts/
    └── Client/
        ├── InputHandler.lua      # Handle klik/input
        ├── UIManager.lua         # Update UI
        └── CameraController.lua  # Kamera setup
```

### 2. Pattern Data Access

**Server-Side Pattern:**

```
-- Pattern untuk akses data di server
local GameState = {
    phase = "WAITING",
    players = {},
    deck = {}
}

-- Update state
function GameState:SetPhase(newPhase)
    self.phase = newPhase
    -- Notify clients
    RemoteEvents.PhaseChanged:FireAllClients(newPhase)
end

-- Get player data
function GameState:GetPlayerData(player)
    return self.players[player]
end
```

**Client-Side Pattern:**

```
-- Pattern untuk menerima update di client
RemoteEvents.PhaseChanged.OnClientEvent:Connect(function(newPhase)
    -- Update UI
    UIManager:UpdatePhase(newPhase)
end)

-- Request action
function RequestDrawCard()
    RemoteEvents.DrawCardRequest:FireServer()
end
```

### 3. Error Handling

**Untuk DataStore (Persistent Data):**

```
-- Selalu gunakan pcall untuk DataStore
local success, result = pcall(function()
    return DataStore:GetAsync(key)
end)

if not success then
    -- Log error, retry, atau gunakan default
    warn("Failed to load stats:", result)
    return defaultStats
end

return result
```

**Untuk Game State (Volatile Data):**

```
-- Validasi state sebelum operasi
function DrawCard(player)
    if GameState.phase ~= "PLAYING" then
        return false, "Game not in playing phase"
    end

    if GameState.currentTurn ~= player then
        return false, "Not your turn"
    end

    -- Proses draw
    -- ...
end
```

### 4. Migration Checklist

Saat memindahkan fitur dari web ke Roblox, periksa hal berikut:

| Fitur Firebase | Status di Roblox | Action                            |
| -------------- | ---------------- | --------------------------------- |
| Room creation  | Tidak perlu      | Hapus, gunakan server Roblox      |
| Room list      | Tidak perlu      | Hapus, gunakan matchmaking Roblox |
| Player auth    | Built-in         | Gunakan `Player.UserId`           |
| Real-time sync | RemoteEvents     | Implementasi baru                 |
| Chat           | Built-in         | Gunakan TextChatService           |
| Stats storage  | DataStoreService | Implementasi baru                 |
| Security rules | Server authority | Logika di server, bukan client    |

---

## Limitasi dan Pertimbangan

### 1. DataStore Rate Limits

**Batasan:**

- 60 requests per menit per server
- 4MB per key
- Throttling saat traffic tinggi

**Solusi:**

- Batch updates (simpan stats hanya saat game selesai)
- Cache data di memory
- Gunakan queue untuk multiple saves

### 2. Server Lifecycle

**Karakteristik Server Roblox:**

- Server mati saat tidak ada pemain selama beberapa menit
- Server bisa restart kapan saja (maintenance Roblox)
- Tidak ada "graceful shutdown" notification

**Solusi:**

- Simpan progress penting segera
- Jangan andalkan data volatile untuk hal penting
- Implementasi reconnect logic jika perlu

### 3. Latency

**Firebase:**

- Latency rendah karena global CDN
- Sync real-time antar client

**Roblox:**

- Latency tergantung lokasi server
- Network replication overhead

**Solusi:**

- Optimalkan RemoteEvent calls
- Jangan kirim data yang tidak perlu
- Gunakan kompresi untuk data besar

---

## Kesimpulan

Migrasi dari Firebase ke Roblox memerlukan perubahan paradigma fundamental:

1. **Dari Shared Database ke Isolated Servers**: Setiap server Roblox adalah dunia tersendiri, tidak perlu sistem room yang kompleks.

2. **Dari Persistent State ke Volatile Memory**: Game state di Roblox hanya ada di RAM, hilang saat server mati. Ini bukan bug, tapi feature.

3. **Dari Client-Side Security ke Server Authority**: Semua logika kritis harus di server, client hanya mengirim input.

4. **Dari Custom Everything ke Built-in Services**: Gunakan TextChatService, DataStoreService, dan services bawaan Roblox.

⭐ **Analogi Akhir:** Jika Firebase adalah kantor pusat dengan lemari arsip sentral, maka Roblox adalah jaringan ruang meeting privat. Setiap ruang punya meja sendiri, dan apa yang terjadi di meja tetap di meja. Hanya statistik hasil pertemuan yang perlu dilaporkan ke kantor pusat.

---

## Referensi dan Link Terkait

**Dokumen Internal:**

- `analisa/teknis-arsitektur.md` - Arsitektur Roblox lengkap
- `database/seed-data.md` - Data awal kartu dan konfigurasi
- `analisa/teknis-api-events.md` - Pemetaan API/Events

**Referensi Eksternal:**

- Roblox DataStoreService: https://create.roblox.com/docs/cloud-services/data-stores
- Roblox Server-Client Model: https://create.roblox.com/docs/tutorials/scripting/server-client-communication
- Roblox RemoteEvents: https://create.roblox.com/docs/reference/engine/classes/RemoteEvent

**Sumber Kode Referensi:**

- `src/FirebaseService.js:1-37` - Struktur database Firebase
- `src/KabulGame.js:77-111` - Struktur state game
- `src/utils/PlayerStats.js:1-57` - Pattern stats storage

---

_Dokumen ini adalah bagian dari Kabul Card Game Roblox Migration Blueprint. Ditulis dalam Bahasa Indonesia untuk tim pengembang lokal._
