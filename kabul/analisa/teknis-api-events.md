# Dokumen Mapping API dan Events: Kabul Web ke Roblox

> **Versi**: 1.0  
> **Target Pembaca**: Developer Roblox yang akan memport game Kabul  
> **Skill Level**: ⭐⭐ Menengah (Level 2-3)  
> **Word Count**: ~6500 kata

---

## Daftar Isi

1. [Pendahuluan](#1-pendahuluan)
2. [Analogi Tombol Bel: Memahami RemoteEvent](#2-analogi-tombol-bel-memahami-remoteevent)
3. [Mapping Lengkap: Web ke Roblox](#3-mapping-lengkap-web-ke-roblox)
4. [Flow Chart Action Utama](#4-flow-chart-action-utama)
5. [Client vs Server: Siapa Trigger, Siapa Handle](#5-client-vs-server-siapa-trigger-siapa-handle)
6. [Detail Setiap Action](#6-detail-setiap-action)
7. [Event Broadcast dan State Sync](#7-event-broadcast-dan-state-sync)
8. [Error Handling dan Validasi](#8-error-handling-dan-validasi)
9. [Referensi Cepat](#9-referensi-cepat)

---

## 1. Pendahuluan

### 1.1 Apa yang Dokumen Ini Tawarkan

Dokumen ini adalah peta lengkap yang menunjukkan **setiap action** dari sistem web Kabul (React + Firebase) dan bagaimana menerjemahkannya ke sistem Roblox menggunakan RemoteEvents. Bayangkan ini seperti kamus bilingual: satu bahasa untuk web, satu bahasa untuk Roblox.

### 1.2 Perbedaan Fundamental

| Aspek                | Web (Firebase)                   | Roblox (RemoteEvents)                  |
| -------------------- | -------------------------------- | -------------------------------------- |
| **Model Komunikasi** | Publish-Subscribe (Real-time DB) | Request-Response + Event               |
| **Client ke Server** | `update(ref, data)`              | `RemoteEvent:FireServer(args)`         |
| **Server ke Client** | `onValue(ref, callback)`         | `RemoteEvent.OnClientEvent`            |
| **Broadcast**        | Otomatis ke semua subscriber     | `FireAllClients()` atau `FireClient()` |
| **Validasi**         | Security Rules                   | Server Script manual                   |

### 1.3 Dari Mana Data Ini Diambil

Semua mapping dalam dokumen ini berasal dari analisis kode sumber:

- **FirebaseService.js** (baris 570-1425): Fungsi `performAction()` dan semua handler action
- **GameRoomPage.jsx** (baris 180-265): Cara client memanggil action
- **teknis-arsitektur.md**: Konsep arsitektur yang sudah dipaparkan sebelumnya

---

## 2. Analogi Tombol Bel: Memahami RemoteEvent

### 2.1 Cerita Tombol Bel

Bayangkan kamu tinggal di rumah besar dengan banyak kamar. Setiap kamar punya **tombol bel** yang terhubung ke **ruang pengaturan pusat** (dapur).

**Skenario 1: Client Kirim Permintaan**

```
[Kamar Tamu - Client]          [Dapur - Server]
       │                               │
       │  "Ding-Dong!"                 │
       │  (Tombol Bel Ditekan)         │
       │──────────────▶                │
       │                               │
       │                               │  "Ada tamu di kamar 3
       │                               │   minta air minum"
       │                               │  (Proses permintaan)
       │                               │
       │  "Air minum datang!"          │
       │◀──────────────                │
       │  (Pelayan antar)              │
```

**Penjelasan:**

- **Tombol Bel** = RemoteEvent yang Client Fire ke Server
- **Dapur** = ServerScriptService yang memproses logika
- **Pelayan** = Server mengirim hasil kembali ke Client

**Skenario 2: Server Kirim Notifikasi**

```
[Dapur - Server]               [Semua Kamar - Clients]
       │                               │
       │  "Makan malam siap!"          │
       │  (Pengumuman ke semua)        │
       │──────────────▶────────────────┤
       │                               │
       │                       [Kamar 1] "Wah, makan!"
       │                       [Kamar 2] "Wah, makan!"
       │                       [Kamar 3] "Wah, makan!"
```

**Penjelasan:**

- **Pengumuman** = Server menggunakan `FireAllClients()`
- **Semua kamar mendengar** = Setiap client menerima event yang sama

### 2.2 Kenapa Analogi Ini Penting

Di Roblox, **Client tidak bisa langsung mengubah GameState**. Client hanya bisa:

1. Menekan "tombol bel" (FireServer)
2. Memberi tahu apa yang diinginkan
3. Menunggu Server memproses
4. Menerima hasil dari Server

Ini mirip dengan Firebase di mana client tidak bisa langsung ubah data sembarangan (ada Security Rules). Bedanya, di Roblox **aturan keamanan ditulis manual di Server Script**.

### 2.3 Tiga Tipe RemoteEvent

```
┌─────────────────────────────────────────────────────────────┐
│                    TIPE REMOTEEVENT                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. CLIENT ──────▶ SERVER (Request)                         │
│     └─ Client minta sesuatu, Server proses                  │
│     └─ Contoh: "Saya mau draw kartu"                        │
│                                                             │
│  2. SERVER ──────▶ CLIENT (Response/Targeted)               │
│     └─ Server kirim ke client spesifik                      │
│     └─ Contoh: "Ini kartu yang kamu draw"                   │
│                                                             │
│  3. SERVER ──────▶ ALL CLIENTS (Broadcast)                  │
│     └─ Server kasih tahu semua player                       │
│     └─ Contoh: "Player A sudah draw kartu"                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Mapping Lengkap: Web ke Roblox

### 3.1 Tabel Master Mapping

Berikut tabel lengkap yang memetakan **setiap action** dari `performAction()` di FirebaseService.js:

| No  | Web Action           | Firebase Function                     | Roblox RemoteEvent        | Arah | Skill  |
| --- | -------------------- | ------------------------------------- | ------------------------- | ---- | ------ |
| 1   | DRAW_DECK            | `_drawCard()` (baris 613)             | `Request:DrawCard`        | C→S  | ⭐⭐   |
| 2   | DRAW_DISCARD         | `_drawCard()` (baris 613)             | `Request:DrawCard`        | C→S  | ⭐⭐   |
| 3   | SWAP_CARD            | `_swapCard()` (baris 698)             | `Request:SwapCard`        | C→S  | ⭐⭐   |
| 4   | DISCARD_DRAWN        | `_discardDrawn()` (baris 759)         | `Request:DiscardCard`     | C→S  | ⭐⭐   |
| 5   | SLAP_MATCH           | `_slapMatch()` (baris 1175)           | `Request:SlapCard`        | C→S  | ⭐⭐   |
| 6   | CALL_KABUL           | `_callKabul()` (baris 1240)           | `Request:CallKabul`       | C→S  | ⭐⭐   |
| 7   | SELECT_OWN_CARD      | `_selectOwnCard()` (baris 868)        | `Request:SelectOwnCard`   | C→S  | ⭐⭐⭐ |
| 8   | SELECT_ENEMY_CARD    | `_selectEnemyCard()` (baris 920)      | `Request:SelectEnemyCard` | C→S  | ⭐⭐⭐ |
| 9   | SELECT_TARGET_PLAYER | `_seeSwapSelectTarget()` (baris 1061) | `Request:SelectTarget`    | C→S  | ⭐⭐⭐ |
| 10  | CONFIRM_SWAP         | `_confirmSwap()` (baris 1129)         | `Request:ConfirmSwap`     | C→S  | ⭐⭐⭐ |
| 11  | SKIP_ABILITY         | `_skipAbility()` (baris 1267)         | `Request:SkipAbility`     | C→S  | ⭐⭐   |

**Keterangan Arah:**

- **C→S**: Client to Server (Request)
- **S→C**: Server to Client (Response)
- **S→All**: Server to All Clients (Broadcast)

### 3.2 Mapping Room Management

Selain action gameplay, ada juga operasi manajemen room:

| No  | Operasi     | Firebase Function              | Roblox Equivalent    | Arah  | Skill  |
| --- | ----------- | ------------------------------ | -------------------- | ----- | ------ |
| 1   | Create Room | `createRoom()` (baris 125)     | `Request:CreateRoom` | C→S   | ⭐⭐   |
| 2   | Join Room   | `joinRoom()` (baris 176)       | `Request:JoinRoom`   | C→S   | ⭐⭐   |
| 3   | Leave Room  | `leaveRoom()` (baris 211)      | `Request:LeaveRoom`  | C→S   | ⭐⭐   |
| 4   | Start Game  | `startGame()` (baris 269)      | `Request:StartGame`  | C→S   | ⭐⭐   |
| 5   | Reset Game  | `resetGameState()` (baris 341) | `Request:ResetGame`  | C→S   | ⭐⭐   |
| 6   | Room Update | `listenToRoom()` (baris 485)   | `Event:RoomUpdated`  | S→All | ⭐⭐⭐ |

### 3.3 Mapping State Synchronization

Firebase punya `onValue()` listener yang otomatis sinkron. Di Roblox, perlu dibuat manual:

| Data Firebase               | Roblox Equivalent | Event Name             | Arah  | Skill  |
| --------------------------- | ----------------- | ---------------------- | ----- | ------ |
| `gameState.phase`           | `StringValue`     | `Event:PhaseChanged`   | S→All | ⭐⭐   |
| `gameState.currentTurn`     | `StringValue`     | `Event:TurnChanged`    | S→All | ⭐⭐   |
| `gameState.topDiscard`      | `ObjectValue`     | `Event:DiscardUpdated` | S→All | ⭐⭐   |
| `players/{id}/hand`         | Private folder    | `Event:HandUpdated`    | S→C   | ⭐⭐⭐ |
| `private/{id}/drawnCard`    | Private folder    | `Event:CardDrawn`      | S→C   | ⭐⭐⭐ |
| `private/{id}/revealedCard` | Private folder    | `Event:CardRevealed`   | S→C   | ⭐⭐⭐ |

---

## 4. Flow Chart Action Utama

### 4.1 Flow Draw Card (Ambil Kartu dari Deck)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         FLOW: DRAW CARD                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌───────────┐      ┌──────────────┐      ┌─────────────┐      ┌──────────┐ │
│  │   CLIENT  │      │  RemoteEvent │      │   SERVER    │      │ GameState│ │
│  └─────┬─────┘      └──────┬───────┘      └──────┬──────┘      └────┬─────┘ │
│        │                   │                     │                  │       │
│        │  1. Klik tombol   │                     │                  │       │
│        │     "Draw Deck"   │                     │                  │       │
│        │                   │                     │                  │       │
│        │  2. FireServer()  │                     │                  │       │
│        │──────────────────▶│                     │                  │       │
│        │                   │  3. OnServerEvent   │                  │       │
│        │                   │────────────────────▶│                  │       │
│        │                   │                     │  4. Validasi:    │       │
│        │                   │                     │     - Turn?      │       │
│        │                   │                     │     - Phase?     │       │
│        │                   │                     │     - Deck > 0?  │       │
│        │                   │                     │                  │       │
│        │                   │                     │  5. Ambil kartu  │       │
│        │                   │                     │     dari deck    │       │
│        │                   │                     │                  │       │
│        │                   │                     │  6. Update       │◀──────┤
│        │                   │                     │     GameState    │       │
│        │                   │                     │                  │       │
│        │                   │  7. FireClient()    │                  │       │
│        │                   │◀────────────────────│                  │       │
│        │  8. Terima data   │                     │                  │       │
│        │     kartu         │                     │                  │       │
│        │◀──────────────────│                     │                  │       │
│        │                   │                     │                  │       │
│        │  9. Tampilkan     │                     │                  │       │
│        │     kartu di UI   │                     │                  │       │
│        │                   │                     │                  │       │
│        │                   │  10. FireAllClients()│                 │       │
│        │                   │◀────────────────────│                  │       │
│        │  11. Update       │                     │                  │       │
│        │      deck count   │                     │                  │       │
│        │      di semua UI  │                     │                  │       │
│        │                   │                     │                  │       │
└────────┴───────────────────┴─────────────────────┴──────────────────┴───────┘
```

**Mapping ke Kode Web:**

```javascript
// FirebaseService.js baris 613-694
async _drawCard(roomId, playerId, source) {
  // Langkah 4: Validasi
  if (gameState.currentTurn !== playerId) {
    throw new Error('Not your turn');
  }
  if (gameState.turnPhase !== TURN_PHASE.DRAWING) {
    throw new Error('Invalid turn phase');
  }

  // Langkah 5: Ambil kartu
  const drawnCard = deck.shift();

  // Langkah 6: Update state
  await set(deckRef, deck);
  await update(gameStateRef, {
    deckCount: deck.length,
    turnPhase: TURN_PHASE.DISCARDING
  });

  // Langkah 7 & 10: Firebase otomatis broadcast
  return { success: true, card: drawnCard };
}
```

### 4.2 Flow Swap Card (Tukar Kartu)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         FLOW: SWAP CARD                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  CLIENT                           SERVER                        GAMESTATE  │
│    │                                │                              │       │
│    │  1. Klik kartu di tangan      │                              │       │
│    │     yang mau ditukar          │                              │       │
│    │                                │                              │       │
│    │  2. FireServer(handIndex)     │                              │       │
│    │───────────────────────────────▶                              │       │
│    │                                │  3. Validasi turn & phase    │       │
│    │                                │  4. Ambil drawnCard dari     │       │
│    │                                │     private data             │       │
│    │                                │  5. Ambil card lama dari     │       │
│    │                                │     hand[index]              │       │
│    │                                │  6. Tukar posisi             │       │
│    │                                │  7. Card lama ke discard     │◀──────┤
│    │                                │  8. Update hand player       │◀──────┤
│    │                                │  9. Clear drawnCard private  │       │
│    │                                │                              │       │
│    │                                │  10. Cek ability kartu       │       │
│    │                                │      yang dibuang?           │       │
│    │                                │                              │       │
│    │      ┌──────────────────────────────────────┐                │       │
│    │      │  YES: Initiate Ability Flow          │                │       │
│    │      │  (Lihat flow chart 4.3)              │                │       │
│    │      └──────────────────────────────────────┘                │       │
│    │                                │                              │       │
│    │      ┌──────────────────────────────────────┐                │       │
│    │      │  NO: Advance Turn                    │                │       │
│    │      │  - Update currentTurn                │◀───────────────┤       │
│    │      │  - Set phase = DRAWING               │                │       │
│    │      │  - FireAllClients TurnChanged        │                │       │
│    │      └──────────────────────────────────────┘                │       │
│    │                                │                              │       │
│    │  11. Terima confirm swap      │                              │       │
│    │◀───────────────────────────────                              │       │
│    │  12. Animasi kartu            │                              │       │
│    │                                │                              │       │
└────┴────────────────────────────────┴──────────────────────────────┴───────┘
```

**Mapping ke Kode Web:**

```javascript
// FirebaseService.js baris 698-757
async _swapCard(roomId, playerId, handIndex) {
  // Validasi
  if (gameState.currentTurn !== playerId) throw new Error('Not your turn');
  if (gameState.turnPhase !== TURN_PHASE.DISCARDING) throw new Error('Must draw first');

  const drawnCard = privateData.drawnCard;
  const replacedCard = player.hand[handIndex];

  // Swap
  const newHand = [...player.hand];
  newHand[handIndex] = drawnCard;

  // Discard card lama
  await runTransaction(discardRef, (pile) => {
    pile.push(replacedCard);
    return pile;
  });

  // Update hand
  await update(playerRef, { hand: newHand });

  // Clear drawn card
  await set(privateRef, { drawnCard: null });

  // Cek ability
  const ability = CARD_ABILITIES[replacedCard.rank];
  if (ability) {
    return this._initiateAbility(roomId, playerId, ability, replacedCard);
  }

  // No ability - advance turn
  return this._advanceTurn(roomId, playerId);
}
```

### 4.3 Flow Card Ability (7/8/9/10/J/Q/K)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    FLOW: CARD ABILITY (Multi-Step)                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  STEP 1: Initiate Ability                                          │   │
│  │  Trigger: Setelah discard kartu dengan ability                      │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                       │
│                                    ▼                                       │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  STEP 2: Set AbilityState                                          │   │
│  │  Server set:                                                       │   │
│  │  - turnPhase = RESOLVING_ABILITY                                   │   │
│  │  - abilityState = { type, activePlayer, step }                     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                    │                                       │
│                    ┌───────────────┼───────────────┐                       │
│                    ▼               ▼               ▼                       │
│          ┌──────────────┐ ┌──────────────┐ ┌──────────────┐               │
│          │  PEEK_SELF   │ │  PEEK_ENEMY  │ │ BLIND_SWAP   │               │
│          │   (7 atau 8) │ │  (9 atau 10) │ │    (J)       │               │
│          └──────┬───────┘ └──────┬───────┘ └──────┬───────┘               │
│                 │                │                │                        │
│                 ▼                ▼                ▼                        │
│          ┌──────────────┐ ┌──────────────┐ ┌──────────────┐               │
│          │ Step 2a:     │ │ Step 2a:     │ │ Step 2a:     │               │
│          │ Select own   │ │ Select target│ │ Select own   │               │
│          │ card index   │ │ player       │ │ card         │               │
│          └──────┬───────┘ └──────┬───────┘ └──────┬───────┘               │
│                 │                │                │                        │
│                 ▼                ▼                ▼                        │
│          ┌──────────────┐ ┌──────────────┐ ┌──────────────┐               │
│          │ Step 3:      │ │ Step 2b:     │ │ Step 2b:     │               │
│          │ Reveal card  │ │ Select enemy │ │ Select target│               │
│          │ to player    │ │ card index   │ │ player       │               │
│          │ (3 detik)    │ └──────┬───────┘ └──────┬───────┘               │
│          └──────────────┘        │                │                        │
│                                  ▼                ▼                        │
│                           ┌──────────────┐ ┌──────────────┐               │
│                           │ Step 3:      │ │ Step 2c:     │               │
│                           │ Reveal card  │ │ Select target│               │
│                           │ to player    │ │ card index   │               │
│                           │ (3 detik)    │ └──────┬───────┘               │
│                           └──────────────┘        │                        │
│                                                   ▼                        │
│                                            ┌──────────────┐               │
│                                            │ Step 3:      │               │
│                                            │ Execute swap │               │
│                                            │ (blind)      │               │
│                                            └──────────────┘               │
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │  STEP 4: Complete Ability                                          │   │
│  │  - Clear abilityState                                              │   │
│  │  - Advance turn                                                    │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Mapping ke Kode Web (Peek Self - 7/8):**

```javascript
// FirebaseService.js baris 868-916
async _selectOwnCard(roomId, playerId, handIndex) {
  // Validasi ability
  if (gameState.abilityState?.activePlayer !== playerId) {
    throw new Error('Not your ability turn');
  }
  if (gameState.abilityState?.type !== 'PEEK_SELF') {
    throw new Error('Wrong ability type');
  }

  const card = player.hand[handIndex];

  // Reveal ke private node (hanya player ini)
  await set(privateRef, {
    revealedCard: {
      position: handIndex,
      rank: card.rank,
      suit: card.suit,
      value: card.value,
      display: card.display,
      expiresAt: Date.now() + 3000, // 3 detik
    },
  });

  // Complete ability
  await update(gameStateRef, {
    turnPhase: TURN_PHASE.WAITING,
    abilityState: null,
  });

  // Schedule cleanup
  setTimeout(async () => {
    await set(privateRef, { revealedCard: null });
    await this._advanceTurn(roomId, playerId);
  }, 3000);
}
```

---

## 5. Client vs Server: Siapa Trigger, Siapa Handle

### 5.1 Prinsip Server-Authoritative

**Aturan Emas**: Client hanya boleh **mengirim niat** (intent), bukan **memutuskan hasil**.

| Client Boleh          | Client Tidak Boleh              |
| --------------------- | ------------------------------- |
| Klik tombol "Draw"    | Langsung tambah kartu ke tangan |
| Klik kartu untuk swap | Langsung tukar posisi kartu     |
| Klik "Call Kabul"     | Langsung ubah game phase        |
| Pilih target player   | Langsung lihat kartu lawan      |

### 5.2 Daftar Lengkap: Trigger vs Handler

#### A. Room Management

| Action      | Trigger (Client)                    | Handler (Server)       | Validasi Server                         | Skill  |
| ----------- | ----------------------------------- | ---------------------- | --------------------------------------- | ------ |
| Create Room | Host klik "Create"                  | `RoomManager.Create()` | Cek unique room ID                      | ⭐⭐   |
| Join Room   | Player klik "Join"                  | `RoomManager.Join()`   | Cek kapasitas, cek phase WAITING        | ⭐⭐   |
| Leave Room  | Player klik "Leave" atau disconnect | `RoomManager.Leave()`  | Cek sisa player, end game jika 1 player | ⭐⭐⭐ |
| Start Game  | Host klik "Start"                   | `GameManager.Start()`  | Cek host, cek min 2 player, cek phase   | ⭐⭐   |

#### B. Gameplay Actions

| Action        | Trigger (Client)         | Handler (Server)            | Validasi Server                                                       | Skill  |
| ------------- | ------------------------ | --------------------------- | --------------------------------------------------------------------- | ------ |
| Draw Deck     | Player klik deck         | `DeckManager.Draw()`        | Cek turn, cek phase DRAWING, cek deck > 0                             | ⭐⭐   |
| Draw Discard  | Player klik discard pile | `DeckManager.DrawDiscard()` | Cek turn, cek phase DRAWING, cek pile > 0                             | ⭐⭐   |
| Swap Card     | Player klik kartu tangan | `CardManager.Swap()`        | Cek turn, cek phase DISCARDING, cek index valid, cek drawnCard exists | ⭐⭐   |
| Discard Drawn | Player klik "Discard"    | `CardManager.Discard()`     | Cek turn, cek phase DISCARDING, cek drawnCard exists                  | ⭐⭐   |
| Slap Match    | Player klik "Slap"       | `ActionManager.Slap()`      | Cek kartu match dengan top discard                                    | ⭐⭐⭐ |
| Call Kabul    | Player klik "KABUL!"     | `GameManager.CallKabul()`   | Cek turn, cek phase DRAWING, cek belum pernah call                    | ⭐⭐   |

#### C. Ability Actions

| Action                 | Trigger (Client)          | Handler (Server)                | Validasi Server                                          | Skill  |
| ---------------------- | ------------------------- | ------------------------------- | -------------------------------------------------------- | ------ |
| Select Own Card (Peek) | Player klik kartu sendiri | `AbilityManager.PeekSelf()`     | Cek abilityState.type == PEEK_SELF, cek activePlayer     | ⭐⭐⭐ |
| Select Target Player   | Player klik player lain   | `AbilityManager.SelectTarget()` | Cek abilityState.type, cek target != self                | ⭐⭐⭐ |
| Select Enemy Card      | Player klik kartu lawan   | `AbilityManager.PeekEnemy()`    | Cek abilityState.type == PEEK_ENEMY, cek target valid    | ⭐⭐⭐ |
| Confirm Swap           | Player klik "Confirm"     | `AbilityManager.ConfirmSwap()`  | Cek abilityState.type == SEE_AND_SWAP, cek indices valid | ⭐⭐⭐ |
| Skip Ability           | Player klik "Skip"        | `AbilityManager.Skip()`         | Cek abilityState.activePlayer == playerId                | ⭐⭐   |

### 5.3 Diagram Interaksi Client-Server

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                  DIAGRAM INTERAKSI CLIENT-SERVER                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         CLIENT SIDE                                │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────────┐  │   │
│  │  │ InputHandler │  │   UIManager  │  │   Prediction System      │  │   │
│  │  │              │  │              │  │   (Optimistic UI)        │  │   │
│  │  │ - onClick()  │  │ - showHand() │  │   - Preview changes      │  │   │
│  │  │ - onHover()  │  │ - animate()  │  │   - Rollback if fail     │  │   │
│  │  └──────┬───────┘  └──────┬───────┘  └──────────────────────────┘  │   │
│  │         │                 │                                        │   │
│  │         └────────┬────────┘                                        │   │
│  │                  │                                                 │   │
│  │         ┌────────▼────────┐                                        │   │
│  │         │  ClientBridge   │                                        │   │
│  │         │  (RemoteEvents) │                                        │   │
│  │         └────────┬────────┘                                        │   │
│  └──────────────────┼─────────────────────────────────────────────────┘   │
│                     │                                                     │
│                     │ FireServer(action, payload)                         │
│                     ▼                                                     │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         SERVER SIDE                                │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────────┐  │   │
│  │  │   Validator  │  │   Executor   │  │   Broadcaster            │  │   │
│  │  │              │  │              │  │                          │  │   │
│  │  │ - Cek turn   │  │ - Update DB  │  │   - FireClient()         │  │   │
│  │  │ - Cek phase  │  │ - Swap cards │  │   - FireAllClients()     │  │   │
│  │  │ - Cek rules  │  │ - Calc score │  │   - Update ReplicatedStorage│ │   │
│  │  └──────┬───────┘  └──────┬───────┘  └──────────────────────────┘  │   │
│  │         │                 │                                        │   │
│  │         └────────┬────────┘                                        │   │
│  │                  │                                                 │   │
│  │         ┌────────▼────────┐                                        │   │
│  │         │  ServerBridge   │                                        │   │
│  │         │  (RemoteEvents) │                                        │   │
│  │         └────────┬────────┘                                        │   │
│  └──────────────────┼─────────────────────────────────────────────────┘   │
│                     │                                                     │
│                     │ OnClientEvent(result)                                 │
│                     ▼                                                     │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                         CLIENT SIDE                                │   │
│  │  ┌──────────────┐  ┌──────────────┐                                 │   │
│  │  │   Response   │  │   Update UI  │                                 │   │
│  │  │   Handler    │  │              │                                 │   │
│  │  │              │  │ - Show card  │                                 │   │
│  │  │ - Success?   │  │ - Animate    │                                 │   │
│  │  │ - Error msg  │  │ - Play sound │                                 │   │
│  │  └──────────────┘  └──────────────┘                                 │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 6. Detail Setiap Action

### 6.1 Action: DRAW_DECK dan DRAW_DISCARD ⭐⭐

**FirebaseService.js Reference**: Baris 613-694

**Deskripsi**: Player mengambil kartu dari deck atau discard pile.

**Web Code**:

```javascript
// GameRoomPage.jsx baris 193-201
const handleDrawDeck = () => {
  play('card-flip');
  handleAction(ACTION.DRAW_DECK);
};

const handleDrawDiscard = () => {
  play('card-flip');
  handleAction(ACTION.DRAW_DISCARD);
};
```

**Roblox Mapping**:

| Komponen             | Detail                                                                                                                                                                          |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RemoteEvent Request  | `ReplicatedStorage.Remotes.DrawCardRequest`                                                                                                                                     |
| Payload              | `{ source: "deck" \| "discard" }`                                                                                                                                               |
| Handler              | `ServerScriptService.DeckManager.DrawCard(player, source)`                                                                                                                      |
| Validasi             | - `gameState.currentTurn == player.UserId`<br>- `gameState.turnPhase == "DRAWING"`<br>- Jika source="deck": `deckCount > 0`<br>- Jika source="discard": `discardPile.Count > 0` |
| Response (ke caller) | `{ success: true, card: CardData }` atau `{ success: false, error: string }`                                                                                                    |
| Broadcast            | `Event:DeckCountChanged` (ke semua)                                                                                                                                             |

**Flow Detail**:

1. Client klik deck/discard pile
2. Client `FireServer("DrawCardRequest", { source })`
3. Server validasi turn dan phase
4. Server ambil kartu dari source
5. Server simpan ke `private[playerId].drawnCard`
6. Server update `gameState.turnPhase = "DISCARDING"`
7. Server `FireClient(player, "CardDrawn", cardData)`
8. Server `FireAllClients("DeckCountChanged", newCount)`

### 6.2 Action: SWAP_CARD ⭐⭐

**FirebaseService.js Reference**: Baris 698-757

**Deskripsi**: Player menukar kartu yang baru di-draw dengan kartu di tangan.

**Web Code**:

```javascript
// GameRoomPage.jsx baris 215-218
const handleSwapCard = (handIndex) => {
  play('card-deal');
  handleAction(ACTION.SWAP_CARD, { handIndex });
};
```

**Roblox Mapping**:

| Komponen            | Detail                                                                                                                                   |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| RemoteEvent Request | `ReplicatedStorage.Remotes.SwapCardRequest`                                                                                              |
| Payload             | `{ handIndex: number }`                                                                                                                  |
| Handler             | `ServerScriptService.CardManager.SwapCard(player, handIndex)`                                                                            |
| Validasi            | - `currentTurn == player.UserId`<br>- `turnPhase == "DISCARDING"`<br>- `private[playerId].drawnCard != nil`<br>- `handIndex` valid (0-3) |
| Response            | `{ success: true, ability: string \| null }`                                                                                             |
| Broadcast           | - `Event:HandUpdated` (ke player)<br>- `Event:DiscardUpdated` (ke semua)<br>- Jika ada ability: `Event:AbilityStarted`                   |

**Flow Detail**:

1. Client klik kartu di tangan
2. Client `FireServer("SwapCardRequest", { handIndex })`
3. Server validasi
4. Server ambil `drawnCard` dari private
5. Server ambil kartu lama dari `hand[handIndex]`
6. Server swap: `hand[handIndex] = drawnCard`
7. Server kirim kartu lama ke discard pile
8. Server clear `private[playerId].drawnCard`
9. Server cek `CARD_ABILITIES[discardedCard.rank]`
10. Jika ada ability: initiate ability flow
11. Jika tidak: `_advanceTurn()`

### 6.3 Action: DISCARD_DRAWN ⭐⭐

**FirebaseService.js Reference**: Baris 759-798

**Deskripsi**: Player membuang kartu yang baru di-draw (tanpa swap).

**Web Code**:

```javascript
// GameRoomPage.jsx baris 203-206
const handleDiscardDrawn = () => {
  play('card-flip');
  handleAction(ACTION.DISCARD_DRAWN);
};
```

**Roblox Mapping**:

| Komponen            | Detail                                                 |
| ------------------- | ------------------------------------------------------ |
| RemoteEvent Request | `ReplicatedStorage.Remotes.DiscardCardRequest`         |
| Payload             | `{}` (tidak perlu parameter)                           |
| Handler             | `ServerScriptService.CardManager.DiscardDrawn(player)` |
| Validasi            | Sama dengan SWAP_CARD                                  |
| Response            | `{ success: true, ability: string \| null }`           |

**Perbedaan dengan Swap**:

- Kartu drawn langsung ke discard (bukan swap)
- Hand player tidak berubah
- Tetap cek ability dari kartu yang di-discard

### 6.4 Action: SLAP_MATCH ⭐⭐⭐

**FirebaseService.js Reference**: Baris 1175-1236

**Deskripsi**: Player menepuk kartu di tangan yang cocok dengan top discard (special move).

**Web Code**:

```javascript
// GameRoomPage.jsx baris 220-223
const handleSlap = (handIndex) => {
  play('card-slap');
  handleAction(ACTION.SLAP_MATCH, { handIndex });
};
```

**Roblox Mapping**:

| Komponen            | Detail                                                             |
| ------------------- | ------------------------------------------------------------------ |
| RemoteEvent Request | `ReplicatedStorage.Remotes.SlapCardRequest`                        |
| Payload             | `{ handIndex: number }`                                            |
| Handler             | `ServerScriptService.ActionManager.SlapCard(player, handIndex)`    |
| Validasi            | - `handIndex` valid<br>- `hand[handIndex].rank == topDiscard.rank` |
| Response (success)  | `{ success: true, message: "Match!", removed: CardData }`          |
| Response (fail)     | `{ success: false, message: "Wrong! +1 card", penalty: CardData }` |
| Broadcast           | `Event:SlapResult` dengan detail hasil                             |

**Logika Khusus**:

```lua
-- Pseudocode Roblox
local function SlapCard(player, handIndex)
    local card = playerHand[handIndex]
    local topDiscard = discardPile[#discardPile]

    if card.rank == topDiscard.rank then
        -- SUCCESS: Remove card dari hand
        table.remove(playerHand, handIndex)
        table.insert(discardPile, card)
        return { success = true }
    else
        -- FAIL: Draw penalty card
        local penalty = table.remove(deck, 1)
        table.insert(playerHand, penalty)
        return { success = false, penalty = penalty }
    end
end
```

### 6.5 Action: CALL_KABUL ⭐⭐

**FirebaseService.js Reference**: Baris 1240-1263

**Deskripsi**: Player menyerukan "Kabul!" untuk mengakhiri game setelah semua player mendapat 1 giliran lagi.

**Web Code**:

```javascript
// GameRoomPage.jsx baris 208-211
const handleCallKabul = () => {
  play('kabul-call');
  handleAction(ACTION.CALL_KABUL);
};
```

**Roblox Mapping**:

| Komponen            | Detail                                                                                               |
| ------------------- | ---------------------------------------------------------------------------------------------------- |
| RemoteEvent Request | `ReplicatedStorage.Remotes.CallKabulRequest`                                                         |
| Payload             | `{}`                                                                                                 |
| Handler             | `ServerScriptService.GameManager.CallKabul(player)`                                                  |
| Validasi            | - `currentTurn == player.UserId`<br>- `turnPhase == "DRAWING"`<br>- `player.hasCalledKabul == false` |
| Response            | `{ success: true, message: "KABUL! Others get 1 final turn." }`                                      |
| Broadcast           | `Event:KabulCalled` dengan caller ID                                                                 |

**State Changes**:

```lua
-- Server setelah Call Kabul
gameState.kabulCaller = player.UserId
gameState.finalTurnsRemaining = playerCount - 1
player.hasCalledKabul = true
```

### 6.6 Ability Actions ⭐⭐⭐

#### 6.6.1 PEEK_SELF (Kartu 7 atau 8)

**FirebaseService.js Reference**: Baris 868-916

**Deskripsi**: Player melihat salah satu kartu sendiri.

| Komponen            | Detail                                                                                 |
| ------------------- | -------------------------------------------------------------------------------------- |
| RemoteEvent Request | `ReplicatedStorage.Remotes.SelectOwnCardRequest`                                       |
| Payload             | `{ handIndex: number }`                                                                |
| Handler             | `ServerScriptService.AbilityManager.PeekSelf(player, handIndex)`                       |
| Validasi            | - `abilityState.type == "PEEK_SELF"`<br>- `abilityState.activePlayer == player.UserId` |
| Response            | `{ success: true, card: CardData }`                                                    |
| Broadcast           | Tidak ada (private ke caller saja)                                                     |

**Timer Handling**:

```lua
-- Server memberikan 3 detik untuk melihat
FireClient(player, "CardRevealed", cardData, 3)
wait(3)
FireClient(player, "HideRevealed")
AdvanceTurn()
```

#### 6.6.2 PEEK_ENEMY (Kartu 9 atau 10)

**FirebaseService.js Reference**: Baris 920-969

**Deskripsi**: Player melihat salah satu kartu lawan.

| Komponen            | Detail                                                                                                        |
| ------------------- | ------------------------------------------------------------------------------------------------------------- |
| RemoteEvent Request | `ReplicatedStorage.Remotes.SelectEnemyCardRequest`                                                            |
| Payload             | `{ targetPlayerId: string, handIndex: number }`                                                               |
| Handler             | `ServerScriptService.AbilityManager.PeekEnemy(player, targetPlayerId, handIndex)`                             |
| Validasi            | - `abilityState.type == "PEEK_ENEMY"`<br>- `targetPlayerId != player.UserId`<br>- Target punya kartu di index |

#### 6.6.3 BLIND_SWAP (Kartu J)

**FirebaseService.js Reference**: Baris 973-1040

**Deskripsi**: Player menukar kartu sendiri dengan kartu lawan tanpa melihat.

**Multi-Step Flow**:

```
Step 1: Select own card
        ↓
Step 2: Select target player
        ↓
Step 3: Select target card
        ↓
Step 4: Execute swap (blind)
        ↓
Step 5: Advance turn
```

| RemoteEvent                  | Arah  | Payload                             |
| ---------------------------- | ----- | ----------------------------------- |
| `Request:SelectOwnCard`      | C→S   | `{ handIndex: number }`             |
| `Event:AbilityStep`          | S→C   | `{ step: "SELECTING_TARGET" }`      |
| `Request:SelectTargetPlayer` | C→S   | `{ targetPlayerId: string }`        |
| `Event:AbilityStep`          | S→C   | `{ step: "SELECTING_TARGET_CARD" }` |
| `Request:SelectEnemyCard`    | C→S   | `{ handIndex: number }`             |
| `Event:SwapExecuted`         | S→All | `{ success: true }`                 |

#### 6.6.4 SEE_AND_SWAP (Kartu Q atau K)

**FirebaseService.js Reference**: Baris 1044-1171

**Deskripsi**: Player melihat kartu lawan, lalu bisa memilih swap atau tidak.

**Multi-Step Flow**:

```
Step 1: Select own card
        ↓
Step 2: Select target player
        ↓
Step 3: Select target card
        ↓
Step 4: REVEAL BOTH cards to player only
        ↓
Step 5: Player Confirm atau Skip
        ↓
Step 6a: If Confirm → Execute swap
Step 6b: If Skip → Advance turn (no swap)
```

| RemoteEvent           | Arah              | Payload                                       |
| --------------------- | ----------------- | --------------------------------------------- |
| `Request:ConfirmSwap` | C→S               | `{ confirm: boolean }`                        |
| `Event:SwapPreview`   | S→C (caller only) | `{ ownCard: CardData, targetCard: CardData }` |

---

## 7. Event Broadcast dan State Sync

### 7.1 Events yang Dibroadcast ke Semua Player

| Event Name               | Trigger              | Data                               | Skill  |
| ------------------------ | -------------------- | ---------------------------------- | ------ |
| `Event:PhaseChanged`     | Game phase berubah   | `{ oldPhase, newPhase }`           | ⭐⭐   |
| `Event:TurnChanged`      | Giliran berpindah    | `{ newTurn: playerId, turnPhase }` | ⭐⭐   |
| `Event:DeckCountChanged` | Kartu deck berkurang | `{ count: number }`                | ⭐⭐   |
| `Event:DiscardUpdated`   | Kartu dibuang        | `{ topCard: CardData }`            | ⭐⭐   |
| `Event:PlayerJoined`     | Player masuk room    | `{ playerId, playerName }`         | ⭐⭐   |
| `Event:PlayerLeft`       | Player keluar        | `{ playerId }`                     | ⭐⭐   |
| `Event:KabulCalled`      | Kabul dipanggil      | `{ callerId, finalTurns }`         | ⭐⭐   |
| `Event:GameEnded`        | Game selesai         | `{ winnerId, scores: {} }`         | ⭐⭐   |
| `Event:SlapResult`       | Hasil slap           | `{ playerId, success, message }`   | ⭐⭐⭐ |
| `Event:AbilityStarted`   | Ability dimulai      | `{ type, activePlayer }`           | ⭐⭐⭐ |
| `Event:SwapExecuted`     | Swap selesai         | `{ playerA, playerB }`             | ⭐⭐⭐ |

### 7.2 Events yang Hanya ke Player Tertentu

| Event Name           | Target       | Trigger         | Data                               | Skill  |
| -------------------- | ------------ | --------------- | ---------------------------------- | ------ |
| `Event:CardDrawn`    | Caller       | Draw berhasil   | `{ card: CardData }`               | ⭐⭐   |
| `Event:HandUpdated`  | Caller       | Hand berubah    | `{ hand: CardData[] }`             | ⭐⭐   |
| `Event:CardRevealed` | Caller       | Peek berhasil   | `{ card: CardData, expiresIn: 3 }` | ⭐⭐⭐ |
| `Event:SwapPreview`  | Caller       | See and Swap    | `{ ownCard, targetCard }`          | ⭐⭐⭐ |
| `Event:TurnStarted`  | Current turn | Giliran dimulai | `{ turnPhase }`                    | ⭐⭐   |
| `Event:Error`        | Caller       | Validasi gagal  | `{ message: string }`              | ⭐⭐   |

### 7.3 State Synchronization Pattern

Di Firebase, state sinkron otomatis via `onValue()`. Di Roblox, perlu pattern khusus:

```lua
-- ServerScriptService/StateManager
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local GameState = ReplicatedStorage:WaitForChild("GameState")

-- Value Objects untuk state yang perlu sync
local currentTurn = GameState:FindFirstChild("CurrentTurn")
local phase = GameState:FindFirstChild("Phase")
local topDiscard = GameState:FindFirstChild("TopDiscard")

-- Function untuk update state
function UpdateGameState(updates)
    if updates.currentTurn then
        currentTurn.Value = updates.currentTurn
    end
    if updates.phase then
        phase.Value = updates.phase
    end
    if updates.topDiscard then
        topDiscard.Value = HttpService:JSONEncode(updates.topDiscard)
    end
end
```

---

## 8. Error Handling dan Validasi

### 8.1 Jenis Error yang Mungkin Terjadi

| Error Code           | Deskripsi                     | Sumber (Web)           |
| -------------------- | ----------------------------- | ---------------------- |
| `NOT_YOUR_TURN`      | Bukan giliran player          | Baris 631, 716, 1247   |
| `INVALID_PHASE`      | Phase game salah              | Baris 634, 717, 773    |
| `DECK_EMPTY`         | Deck habis                    | Baris 622, 646         |
| `DISCARD_EMPTY`      | Discard pile kosong           | Baris 665              |
| `NO_CARD_DRAWN`      | Belum draw kartu              | Baris 718, 774         |
| `INVALID_INDEX`      | Index kartu tidak valid       | Baris 719              |
| `NOT_YOUR_ABILITY`   | Bukan giliran ability         | Baris 882-884          |
| `WRONG_ABILITY_TYPE` | Tipe ability salah            | Baris 885-886, 936-937 |
| `CANNOT_PEEK_SELF`   | Tidak boleh peek diri sendiri | Baris 939-940          |

### 8.2 Error Response Pattern

**Web Pattern**:

```javascript
// FirebaseService.js
try {
  if (gameState.currentTurn !== playerId) {
    throw new Error('Not your turn');
  }
} catch (err) {
  return { success: false, error: err.message };
}
```

**Roblox Equivalent**:

```lua
-- ServerScriptService/ActionValidator
local function ValidateTurn(playerId)
    local currentTurn = GameState.CurrentTurn.Value
    if currentTurn ~= playerId then
        return false, "NOT_YOUR_TURN"
    end
    return true, nil
end

-- Handler
local success, error = ValidateTurn(player.UserId)
if not success then
    RemoteEvents.ErrorEvent:FireClient(player, {
        code = error,
        message = "Bukan giliranmu!"
    })
    return
end
```

### 8.3 Validasi Checklist

Setiap action WAJIB melewati validasi berikut:

```lua
-- Universal Validation Checklist
local function ValidateAction(player, actionType)
    -- 1. Cek player masih di game
    if not PlayersInGame[player.UserId] then
        return false, "NOT_IN_GAME"
    end

    -- 2. Cek game phase (jika perlu)
    if RequiresPlayingPhase[actionType] then
        if GameState.Phase.Value ~= "PLAYING" then
            return false, "INVALID_PHASE"
        end
    end

    -- 3. Cek turn (jika perlu)
    if RequiresTurn[actionType] then
        if GameState.CurrentTurn.Value ~= player.UserId then
            return false, "NOT_YOUR_TURN"
        end
    end

    -- 4. Cek turn phase (jika perlu)
    if RequiresTurnPhase[actionType] then
        local requiredPhase = TurnPhaseRequirements[actionType]
        if GameState.TurnPhase.Value ~= requiredPhase then
            return false, "INVALID_TURN_PHASE"
        end
    end

    -- 5. Cek ability state (jika perlu)
    if RequiresAbility[actionType] then
        local ability = AbilityState.Value
        if not ability or ability.activePlayer ~= player.UserId then
            return false, "NOT_YOUR_ABILITY"
        end
    end

    return true, nil
end
```

---

## 9. Referensi Cepat

### 9.1 Struktur Folder RemoteEvents

```
ReplicatedStorage/
└── Remotes/
    ├── Requests/           (Client → Server)
    │   ├── DrawCardRequest
    │   ├── SwapCardRequest
    │   ├── DiscardCardRequest
    │   ├── SlapCardRequest
    │   ├── CallKabulRequest
    │   ├── SelectOwnCardRequest
    │   ├── SelectEnemyCardRequest
    │   ├── SelectTargetRequest
    │   ├── ConfirmSwapRequest
    │   └── SkipAbilityRequest
    │
    ├── Events/             (Server → Client)
    │   ├── PhaseChanged
    │   ├── TurnChanged
    │   ├── DeckCountChanged
    │   ├── DiscardUpdated
    │   ├── KabulCalled
    │   ├── GameEnded
    │   ├── AbilityStarted
    │   ├── SwapExecuted
    │   ├── SlapResult
    │   └── PlayerJoined/Left
    │
    └── Private/            (Server → Specific Client)
        ├── CardDrawn
        ├── HandUpdated
        ├── CardRevealed
        ├── SwapPreview
        ├── TurnStarted
        └── Error
```

### 9.2 Tabel Mapping Cepat

| Web Action           | Roblox RemoteEvent       | Direction | Skill  |
| -------------------- | ------------------------ | --------- | ------ |
| DRAW_DECK            | `DrawCardRequest`        | C→S       | ⭐⭐   |
| DRAW_DISCARD         | `DrawCardRequest`        | C→S       | ⭐⭐   |
| SWAP_CARD            | `SwapCardRequest`        | C→S       | ⭐⭐   |
| DISCARD_DRAWN        | `DiscardCardRequest`     | C→S       | ⭐⭐   |
| SLAP_MATCH           | `SlapCardRequest`        | C→S       | ⭐⭐⭐ |
| CALL_KABUL           | `CallKabulRequest`       | C→S       | ⭐⭐   |
| SELECT_OWN_CARD      | `SelectOwnCardRequest`   | C→S       | ⭐⭐⭐ |
| SELECT_ENEMY_CARD    | `SelectEnemyCardRequest` | C→S       | ⭐⭐⭐ |
| SELECT_TARGET_PLAYER | `SelectTargetRequest`    | C→S       | ⭐⭐⭐ |
| CONFIRM_SWAP         | `ConfirmSwapRequest`     | C→S       | ⭐⭐⭐ |
| SKIP_ABILITY         | `SkipAbilityRequest`     | C→S       | ⭐⭐   |

### 9.3 State Objects di ReplicatedStorage

```lua
-- StringValue
ReplicatedStorage.GameState.Phase          -- "WAITING", "MEMORIZE", "PLAYING", "ENDED"
ReplicatedStorage.GameState.CurrentTurn    -- Player.UserId
ReplicatedStorage.GameState.TurnPhase      -- "DRAWING", "DISCARDING", etc

-- IntValue
ReplicatedStorage.GameState.DeckCount
ReplicatedStorage.GameState.FinalTurnsRemaining

-- ObjectValue (JSON encoded)
ReplicatedStorage.GameState.TopDiscard
ReplicatedStorage.GameState.AbilityState

-- Folder (per player)
ReplicatedStorage.GameState.Players/
└── {PlayerId}/
    ├── Name (StringValue)
    ├── CardCount (IntValue)
    └── HasCalledKabul (BoolValue)
```

---

## Kesimpulan

Dokumen ini memetakan **seluruh 11 action utama** dari sistem web Kabul ke sistem Roblox RemoteEvents. Ingatlah analogi **tombol bel**: Client hanya bisa menekan bel (kirim request), Server yang menentukan apa yang terjadi (proses dan broadcast).

**Poin Kunci**:

1. Setiap action web memiliki RemoteEvent counterpart di Roblox
2. Validasi WAJIB dilakukan di server (tidak percaya client)
3. State sync menggunakan Value Objects di ReplicatedStorage
4. Ability actions membutuhkan multi-step flow
5. Error handling konsisten dengan code dan message

**Selanjutnya**: Lanjut ke implementasi dengan mengacu pada `teknis-arsitektur.md` untuk struktur folder dan pattern arsitektur.

---

_**Skill Level Legend:**_

- ⭐ = Pemula (Level 1)
- ⭐⭐ = Menengah (Level 2-3)
- ⭐⭐⭐ = Lanjutan (Level 3-4)
- ⭐⭐⭐⭐ = Expert (Level 4-5)

_**Dokumen ini dibuat untuk tim porting Kabul ke Roblox. Update terakhir: Februari 2026**_
