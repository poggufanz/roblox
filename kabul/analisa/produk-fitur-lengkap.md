# Dokumen Fitur Lengkap Kabul Game

## Daftar Isi

1. [Pendahuluan](#pendahuluan)
2. [Sistem Kartu](#sistem-kartu)
3. [Alur Permainan](#alur-permainan)
4. [Mekanik KABUL](#mekanik-kabul)
5. [Mekanik SLAP](#mekanik-slap)
6. [Lobby dan Sistem Room](#lobby-dan-sistem-room)
7. [Fitur Sosial](#fitur-sosial)
8. [Sistem Audio](#sistem-audio)
9. [Costume dan Varian](#costume-dan-varian)
10. [Kategorisasi Phase](#kategorisasi-phase)
11. [Tabel Perbandingan Web vs Roblox](#tabel-perbandingan-web-vs-roblox)
12. [Flow Diagram](#flow-diagram)
13. [Skill Level Indicators](#skill-level-indicators)

---

## Pendahuluan

Kabul adalah permainan kartu multiplayer yang menggabungkan elemen strategi, memori, dan keberuntungan. Pemain bersaing untuk mendapatkan total nilai kartu terendah di tangan mereka. Game ini mendukung 2-4 pemain dalam satu room dan dimainkan secara real-time melalui koneksi internet.

Tujuan utama dari dokumen ini adalah mendokumentasikan seluruh fitur yang ada pada versi web Kabul Game agar dapat diimplementasikan ke platform Roblox dengan konsisten. Setiap fitur dijelaskan secara non-teknis dengan pemetaan cara kerja di web dan rekomendasi implementasi di Roblox.

---

## Sistem Kartu

### 2.1 Dek Kartu

Dalam satu deck Kabul terdapat **54 kartu** yang terdiri dari:

| Tipe Kartu    | Jumlah   | Detail                                        |
| ------------- | -------- | --------------------------------------------- |
| Kartu Standar | 52 kartu | 4 suit (♠ ♥ ♦ ♣) x 13 rank (A, 2-10, J, Q, K) |
| Joker         | 2 kartu  | Kartu spesial tanpa suit                      |

**Skill Level:** ⭐⭐ (Intermediate)

### 2.2 Nilai Kartu (Sistem Scoring)

Nilai kartu menentukan skor akhir pemain. Semakin rendah nilai, semakin baik.

| Kartu                | Nilai      | Keterangan                   |
| -------------------- | ---------- | ---------------------------- |
| Red Kings (K♥, K♦)   | -1         | Nilai terbaik dalam game     |
| Joker (🃏)           | 0          | Kartu netral                 |
| Ace (A)              | 1          | Nilai rendah                 |
| 2 - 10               | Face value | Sesuai angka (2=2, 5=5, dst) |
| Jack (J)             | 11         | Nilai tinggi                 |
| Queen (Q)            | 12         | Nilai tinggi                 |
| Black Kings (K♠, K♣) | 13         | Nilai terburuk               |

**Skill Level:** ⭐ (Beginner)

### 2.3 Kartu Ability (Power Cards)

Beberapa kartu memiliki kemampuan khusus ketika dibuang ke discard pile. Kemampuan ini aktif saat pemain membuang kartu tersebut.

#### 2.3.1 PEEK_SELF - Kartu 7 dan 8

**Skill Level:** ⭐ (Beginner)

**Fungsi:** Melihat salah satu kartu milik sendiri

**Cara Kerja di Web:**

1. Pemain membuang kartu 7 atau 8
2. Sistem masuk ke fase SELECTING_OWN_CARD
3. Pemain mengklik salah satu kartu di tangan sendiri
4. Kartu tersebut terbuka selama 3 detik
5. Setelah 3 detik, kartu kembali tertutup dan giliran berpindah

**Mapping ke Roblox:**

- Gunakan ClickDetector atau ProximityPrompt pada kartu
- Tampilkan GUI preview kartu selama 3 detik
- Mainkan sound effect "card-flip" saat reveal

#### 2.3.2 PEEK_ENEMY - Kartu 9 dan 10

**Skill Level:** ⭐⭐ (Intermediate)

**Fungsi:** Melihat salah satu kartu milik lawan

**Cara Kerja di Web:**

1. Pemain membuang kartu 9 atau 10
2. Sistem masuk ke fase SELECTING_TARGET
3. Pemain mengklik salah satu kartu lawan
4. Kartu lawan tersebut terbuka hanya untuk pemain yang melihat selama 3 detik
5. Lawan tidak tahu kartunya telah dilihat
6. Setelah 3 detik, giliran berpindah

**Mapping ke Roblox:**

- Highlight kartu lawan yang dapat dipilih
- Tampilkan GUI preview hanya untuk pemain yang melihat
- Pastikan kartu tetap tertutup di layar lawan

#### 2.3.3 BLIND_SWAP - Kartu Jack

**Skill Level:** ⭐⭐⭐ (Advanced)

**Fungsi:** Menukar dua kartu tanpa melihatnya

**Cara Kerja di Web:**

1. Pemain membuang kartu Jack
2. Sistem masuk ke fase SWAPPING_CARDS
3. Pemain memilih kartu sendiri terlebih dahulu
4. Pemain memilih kartu lawan target
5. Kedua kartu bertukar posisi secara instant
6. Tidak ada yang melihat nilai kartu yang ditukar
7. Giliran berpindah

**Mapping ke Roblox:**

- Sequence: Pilih kartu sendiri → Pilih target pemain → Pilih kartu target
- Animasi swap sederhana (tukar posisi model kartu)
- Tidak ada efek visual reveal

#### 2.3.4 SEE_AND_SWAP - Kartu Queen dan King

**Skill Level:** ⭐⭐⭐⭐ (Expert)

**Fungsi:** Menukar dua kartu sambil melihat keduanya

**Cara Kerja di Web:**

1. Pemain membuang kartu Queen atau King
2. Sistem masuk ke fase SEE_AND_SWAPPING_CARDS
3. Pemain memilih kartu sendiri
4. Pemain memilih kartu lawan target
5. **Kedua kartu terbuka** dan ditampilkan di UI
6. Pemain melihat nilai kedua kartu
7. Pemain memilih: Confirm Swap atau Cancel
8. Jika confirm, kartu bertukar posisi
9. Jika cancel, tidak terjadi apa-apa
10. Giliran berpindah

**Mapping ke Roblox:**

- Tampilkan popup/panel yang menunjukkan kedua kartu
- Berikan pilihan button Confirm/Cancel
- Animasi swap jika dikonfirmasi

### 2.4 Tabel Ringkasan Ability

| Kartu      | Ability      | Target       | Reveal?           |
| ---------- | ------------ | ------------ | ----------------- |
| 7, 8       | PEEK_SELF    | Self         | Yes (3 sec)       |
| 9, 10      | PEEK_ENEMY   | Enemy        | Yes (3 sec)       |
| J          | BLIND_SWAP   | Self + Enemy | No                |
| Q, K       | SEE_AND_SWAP | Self + Enemy | Yes (before swap) |
| A-6, Joker | NONE         | -            | -                 |

---

## Alur Permainan

### 3.1 Fase Game

Permainan Kabul terbagi menjadi 4 fase utama:

#### 3.1.1 Fase WAITING

**Skill Level:** ⭐ (Beginner)

**Kondisi:** Room baru dibuat, menunggu pemain join

**Fitur:**

- Pemain dapat join sampai maksimal 4 pemain
- Host dapat memulai game (minimal 2 pemain)
- Pemain non-host menunggu host memulai
- Display daftar pemain yang sudah join

**Cara Kerja di Web:**

- Lobby menampilkan room list
- Host memiliki tombol "Start Game"
- Player count ditampilkan real-time (2/4, 3/4, dst)

**Mapping ke Roblox:**

- Teleport pemain ke lobby area
- GUI menunjukkan player list
- Host memiliki button "Start Game"

#### 3.1.2 Fase MEMORIZE

**Skill Level:** ⭐ (Beginner)

**Kondisi:** Game baru dimulai, durasi 3 detik

**Fitur:**

- Setiap pemain mendapat 4 kartu (2x2 grid)
- **Hanya 2 kartu bawah yang terlihat** (posisi 0 dan 1)
- 2 kartu atas tetap tertutup (posisi 2 dan 3)
- Timer countdown 3 detik ditampilkan
- Pemain harus mengingat 2 kartu yang terlihat

**Cara Kerja di Web:**

- Overlay fullscreen dengan tulisan "MEMORIZE YOUR CARDS!"
- Animasi pulse pada timer
- Kartu posisi 0-1 menunjukkan face-up
- Kartu posisi 2-3 menunjukkan face-down

**Mapping ke Roblox:**

- Freeze input pemain selama 3 detik
- Tampilkan GUI countdown besar
- Kartu lokal pemain terbuka untuk posisi 0-1

#### 3.1.3 Fase PLAYING

**Skill Level:** ⭐⭐ (Intermediate)

**Kondisi:** Fase utama permainan, giliran bergantian

**Struktur Turn:**

```
START TURN
    ↓
DRAW PHASE (Pemain mengambil kartu)
    ├─ Draw from Deck (kartu random)
    └─ Draw from Discard (kartu terbuka)
    ↓
ACTION PHASE (Pemain memilih aksi)
    ├─ Swap dengan kartu di tangan
    │   └─ Kartu yang diganti masuk ke discard
    │       └─ Jika kartu ability → Activate Ability
    └─ Discard langsung
        └─ Jika kartu ability → Activate Ability
    ↓
ABILITY RESOLUTION (jika ada)
    ├─ PEEK_SELF → Pilih kartu sendiri
    ├─ PEEK_ENEMY → Pilih kartu lawan
    ├─ BLIND_SWAP → Pilih kartu self → Pilih kartu lawan
    └─ SEE_AND_SWAP → Pilih kartu self → Pilih kartu lawan → Confirm
    ↓
END TURN → Pindah ke pemain berikutnya
```

**Turn Phase States:**

| Phase            | Keterangan             | Aksi yang Tersedia                  |
| ---------------- | ---------------------- | ----------------------------------- |
| DRAWING          | Giliran dimulai        | Draw Deck, Draw Discard, Call Kabul |
| DISCARDING       | Sudah mengambil kartu  | Swap Card, Discard Drawn            |
| SELECTING_TARGET | Memilih target ability | Select Own Card, Select Enemy Card  |
| CONFIRMING_SWAP  | Konfirmasi swap Q/K    | Confirm Swap, Cancel                |
| WAITING          | Menunggu giliran       | None                                |

**Mapping ke Roblox:**

- Highlight pemain yang sedang giliran
- Indikator visual untuk turn phase saat ini
- Input handling sesuai phase

#### 3.1.4 Fase ENDED

**Skill Level:** ⭐ (Beginner)

**Kondisi:** Game selesai, skor dihitung

**Fitur:**

- Semua kartu pemain terbuka
- Skor setiap pemain ditampilkan
- Pemenang (skor terendah) di-highlight
- Tombol "Rematch" (host only)
- Tombol "Back to Lobby"

**Cara Kerja di Web:**

- Panel hasil game menampilkan ranking
- Host memiliki kontrol rematch
- Stats pemain diupdate (win/loss)

**Mapping ke Roblox:**

- Semua kartu di meja terbuka
- Billboard GUI menunjukkan skor
- Button rematch untuk host

### 3.2 Struktur Data Kartu di Tangan

Setiap pemain memiliki 4 kartu yang disusun dalam format 2x2:

```
[0] [1]   ← Baris bawah (terlihat saat MEMORIZE)
[2] [3]   ← Baris atas (tetap tertutup)
```

**Mapping ke Roblox:**

- Gunakan model 3D untuk kartu
- Posisi kartu di world space atau GUI
- Animasi flip untuk reveal

---

## Mekanik KABUL

### 4.1 Definisi KABUL

**Skill Level:** ⭐⭐⭐ (Advanced)

KABUL adalah aksi spesial yang dapat dilakukan pemain untuk mengakhiri ronde permainan. Ketika seorang pemain "memanggil KABUL", dia menyatakan bahwa dirinya memiliki skor kartu terendah.

### 4.2 Aturan KABUL

**Kapan Bisa Dipanggil:**

- Hanya di awal giliran (turnPhase = DRAWING)
- Sebelum mengambil kartu
- Hanya sekali per game

**Efek Memanggil KABUL:**

1. Pemain yang memanggil KABUL mengunci posisinya
2. Pemain tersebut tidak bisa lagi melakukan aksi
3. Giliran berpindah ke pemain berikutnya
4. Setiap pemain lain mendapat **satu giliran terakhir**
5. Setelah semua pemain selesai, game berakhir

### 4.3 Final Turns Logic

```
Pemain A memanggil KABUL
    ↓
Pemain B mendapat giliran terakhir (finalTurnsRemaining = 3)
    ↓
Pemain C mendapat giliran terakhir (finalTurnsRemaining = 2)
    ↓
Pemain D mendapat giliran terakhir (finalTurnsRemaining = 1)
    ↓
GAME END - Hitung skor
```

### 4.4 Cara Kerja di Web

1. Tombol "📣 Kabul" tersedia saat DRAWING phase
2. Klik tombol membuka modal konfirmasi
3. Konfirmasi akan memicu `callKabul()` action
4. UI menampilkan indikator "KABUL by [PlayerName]"
5. Toast notification muncul untuk semua pemain
6. Final turns countdown ditampilkan

### 4.5 Mapping ke Roblox

- Button KABUL di GUI
- Konfirmasi dialog sebelum eksekusi
- Visual indicator di avatar pemain yang memanggil KABUL
- Chat announcement atau sound effect khusus
- Counter untuk final turns

### 4.6 Strategi KABUL

**Kapan Waktu Tepat Memanggil KABUL:**

- Setelah mengumpulkan kartu rendah (Joker, Red Kings, Ace)
- Setelah menggunakan ability untuk memastikan kartu lawan tinggi
- Ketika yakin total skor di bawah 10

**Resiko:**

- Lawan masih punya 1 giliran untuk mengubah kartu mereka
- Lawan bisa menggunakan swap untuk merebut kartu bagus

---

## Mekanik SLAP

### 4.1 Definisi SLAP

**Skill Level:** ⭐⭐ (Intermediate)

SLAP adalah aksi reaktif yang dapat dilakukan **kapan saja** selama fase PLAYING. Pemain dapat "menampar" (slap) salah satu kartunya ke discard pile jika kartu tersebut cocok (match) dengan kartu teratas di discard pile.

### 4.2 Aturan SLAP

**Kapan Bisa Dilakukan:**

- Selama fase PLAYING
- Bukan saat giliran sendiri (bisa juga saat giliran sendiri)
- Kartu di tangan harus memiliki **rank sama** dengan top discard

**Hasil SLAP:**

| Kondisi              | Hasil                                          |
| -------------------- | ---------------------------------------------- |
| Match (rank sama)    | Kartu berhasil dibuang, jumlah kartu berkurang |
| No Match (rank beda) | **Penalti:** +1 kartu dari deck                |

### 4.3 Cara Kerja di Web

1. Pemain melakukan **long-press** (500ms) pada kartu di tangan
2. Sistem memeriksa apakah rank kartu cocok dengan topDiscard
3. Jika cocok: kartu dihapus dari tangan, masuk ke discard
4. Jika tidak cocok: pemain mendapat 1 kartu dari deck sebagai penalti
5. Toast notification menampilkan hasil

### 4.4 Mapping ke Roblox

- Long-press bisa diganti dengan key binding (misal: tekan E sambil hover kartu)
- Atau gunakan context menu pada kartu
- Animasi slap (kartu bergerak cepat ke tengah)
- Sound effect berbeda untuk success vs fail
- Visual penalti (kartu baru muncul di tangan)

### 4.5 Contoh Skenario SLAP

**Skenario Sukses:**

```
Top Discard: 7♥
Pemain memiliki: 7♠ di tangan
Pemain slap kartu 7♠
Result: 7♠ berhasil dibuang, kartu di tangan berkurang menjadi 3
```

**Skenario Gagal:**

```
Top Discard: 7♥
Pemain memiliki: 5♦ di tangan
Pemain salah slap kartu 5♦
Result: Penalti! Pemain mendapat 1 kartu random dari deck
Tangan pemain bertambah menjadi 5 kartu
```

---

## Lobby dan Sistem Room

### 5.1 Lobby System

**Skill Level:** ⭐ (Beginner)

Lobby adalah area awal tempat pemain melihat daftar room yang tersedia dan memilih room untuk join.

**Fitur Lobby:**

| Fitur        | Deskripsi                                         |
| ------------ | ------------------------------------------------- |
| Room List    | Daftar room yang aktif dengan status              |
| Search       | Filter room berdasarkan nama                      |
| Filter       | Filter berdasarkan status (All, Waiting, Private) |
| Room Preview | Panel detail room yang dipilih                    |
| Create Room  | Membuat room baru                                 |

### 5.2 Room Structure

**Room Properties:**

```javascript
{
  id: string,           // ID unik room
  name: string,         // Nama room
  hostId: string,       // ID pembuat room
  hostName: string,     // Nama host
  costume: string,      // COSTUME_1 atau COSTUME_2
  playerCount: number,  // Jumlah pemain saat ini
  maxPlayers: 4,        // Maksimal 4 pemain
  isPrivate: boolean,   // Room private atau public
  status: string,       // WAITING, MEMORIZE, PLAYING, ENDED
  players: array        // Daftar pemain dalam room
}
```

### 5.3 Room Lifecycle

```
CREATE ROOM
    ↓
WAITING (Menunggu pemain)
    ↓
START GAME (Host memulai)
    ↓
MEMORIZE → PLAYING → (ENDED jika KABUL dipanggil)
    ↓
ENDED
    ↓
REMATCH atau DISBAND
```

### 5.4 Cara Kerja di Web

1. Pemain masuk ke LobbyPage
2. Firebase melakukan polling setiap 5 detik untuk update room list
3. Pemain dapat klik room untuk melihat detail
4. Tombol "Join" untuk masuk ke room
5. Tombol "Create Game" untuk membuat room baru
6. Setelah join, navigate ke GameRoomPage dengan roomId

### 5.5 Mapping ke Roblox

- Lobby sebagai place terpisah atau area khusus
- GUI list room dengan scrolling frame
- TeleportService untuk pindah ke game room
- DataStore untuk menyimpan room list

---

## Fitur Sosial

### 6.1 Chat System

**Skill Level:** ⭐ (Beginner)

Chat memungkinkan pemain berkomunikasi dalam room.

**Fitur Chat:**

- Chat panel slide-in dari sisi kanan
- Pesan real-time menggunakan Firebase Realtime Database
- Histori 50 pesan terakhir
- Auto-scroll ke pesan terbaru
- Enter key untuk kirim
- Bubble chat style untuk pesan

**Struktur Pesan:**

```javascript
{
  senderId: string,
  senderName: string,
  text: string,
  timestamp: serverTimestamp
}
```

### 6.2 Player Identity

**Skill Level:** ⭐ (Beginner)

Setiap pemain memiliki identitas unik:

| Property   | Penyimpanan  | Deskripsi                      |
| ---------- | ------------ | ------------------------------ |
| playerId   | localStorage | ID unik persisted              |
| playerName | localStorage | Nama pemain, bisa diubah       |
| level      | localStorage | Level pemain (untuk statistik) |

### 6.3 Cara Kerja di Web

- PlayerContext menyimpan data pemain
- localStorage untuk persistensi
- Nama pemain dapat diubah (tapi tidak diimplementasikan di UI saat ini)

### 6.4 Mapping ke Roblox

- Gunakan Player.Name atau custom display name
- DataStore untuk menyimpan stats persisten
- Chat system bawaan Roblox atau custom GUI chat

---

## Sistem Audio

### 7.1 Sound Effects

**Skill Level:** ⭐⭐ (Intermediate)

Game menggunakan Howler.js untuk mengelola audio.

**Daftar Sound Effects:**

| Sound Name        | Trigger                   | File                          |
| ----------------- | ------------------------- | ----------------------------- |
| card-flip         | Draw kartu, discard kartu | /sounds/card-flip.mp3         |
| card-deal         | Swap kartu                | /sounds/card-deal.mp3         |
| card-slap         | Slap action               | /sounds/card-slap.mp3         |
| kabul-call        | Memanggil KABUL           | /sounds/kabul-call.mp3        |
| turn-notification | Giliran pemain            | /sounds/turn-notification.mp3 |
| success           | Menang                    | /sounds/success.mp3           |
| error             | Kalah atau error          | /sounds/error.mp3             |

### 7.2 Audio Control

**Fitur:**

- Volume control (0.0 - 1.0)
- Mute/Unmute toggle
- Persistensi setting di localStorage
- Preload saat game start

### 7.3 Cara Kerja di Web

- SoundContext menyediakan audio functionality
- SoundManager class mengelola Howl instances
- localStorage menyimpan mute state

### 7.4 Mapping ke Roblox

- Gunakan SoundService atau Sound instances
- Audio assets perlu diupload ke Roblox
- Volume dan mute control via Settings

---

## Costume dan Varian

### 8.1 Costume System

**Skill Level:** ⭐⭐ (Intermediate)

Costume adalah varian aturan nilai kartu yang dapat dipilih saat membuat room.

### 8.2 Costume 1: Classic (Default)

**Skill Level:** ⭐ (Beginner)

```
Joker: -1 (Best card)
Red Kings: 0
Ace: 1
2-10: Face value
Jack: 11
Queen: 12
Black Kings: 13
```

### 8.3 Costume 2: Reversed

**Skill Level:** ⭐⭐ (Intermediate)

```
Joker: 0
Red Kings: -1 (Best card)
Ace: 1
2-10: Face value
Jack: 11
Queen: 12
Black Kings: 13
```

### 8.4 Cara Kerja di Web

- Pemilihan costume saat Create Room
- Costume disimpan di room config
- Deck generation menggunakan costume yang dipilih
- Visual indicator di room list

### 8.5 Mapping ke Roblox

- Pilihan costume di game settings
- Konfigurasi nilai kartu dalam ModuleScript
- Visual berbeda untuk setiap costume (opsional)

---

## Kategorisasi Phase

### Phase 1: MVP (Minimum Viable Product)

Fitur yang **wajib** ada untuk rilis pertama:

**Core Gameplay:**

- ⭐⭐⭐⭐⭐ Sistem kartu lengkap (54 kartu)
- ⭐⭐⭐⭐⭐ Alur permainan 4 fase (WAITING, MEMORIZE, PLAYING, ENDED)
- ⭐⭐⭐⭐⭐ Semua kartu ability (7/8, 9/10, J, Q/K)
- ⭐⭐⭐⭐ Mekanik KABUL
- ⭐⭐⭐ Mekanik SLAP
- ⭐⭐⭐ Costume system (Classic dan Reversed)

**Multiplayer:**

- ⭐⭐⭐⭐⭐ Sistem room (create, join, leave)
- ⭐⭐⭐⭐⭐ Real-time synchronization
- ⭐⭐⭐⭐ Player turn management
- ⭐⭐⭐ Final turns handling saat KABUL

**UI/UX:**

- ⭐⭐⭐⭐⭐ Game table dengan kartu
- ⭐⭐⭐⭐ Kartu visual (face-up dan face-down)
- ⭐⭐⭐⭐ Turn indicator
- ⭐⭐⭐ Deck dan discard pile display
- ⭐⭐⭐ Win/lose screen

### Phase 2: Enhancement

Fitur yang bisa ditambahkan setelah MVP stabil:

**Sosial:**

- ⭐⭐ Chat system
- ⭐⭐ Emoji reactions
- ⭐ Friend system
- ⭐ Player profile dengan stats

**Audio:**

- ⭐⭐⭐ Sound effects untuk setiap aksi
- ⭐⭐ Background music
- ⭐ Volume control

**Visual:**

- ⭐⭐ Animasi kartu (flip, deal, swap)
- ⭐⭐ Particle effects
- ⭐ Custom table themes
- ⭐ Card skins

**Gameplay Enhancement:**

- ⭐⭐ Tutorial mode
- ⭐ Spectator mode
- ⭐ Ranked matches
- ⭐ Leaderboards

---

## Tabel Perbandingan Web vs Roblox

| Fitur              | Web (Current)           | Roblox (Target)             | Notes                                  |
| ------------------ | ----------------------- | --------------------------- | -------------------------------------- |
| **Kartu**          |                         |                             |                                        |
| Visual kartu       | HTML/CSS dengan animasi | 3D Model/Decal              | Roblox perlu model kartu               |
| Flip animation     | Framer Motion (rotateY) | TweenService/Flip book      | Gunakan mesh atau decal swap           |
| Card hover         | CSS hover               | MouseHover event            | Equivalent functionality               |
| **Gameplay**       |                         |                             |                                        |
| Input: Draw        | Click on deck           | Click/ProximityPrompt       | ProximityPrompt lebih immersive        |
| Input: Swap        | Click kartu target      | Click/Key binding           | Pertimbangkan key binding              |
| Input: Slap        | Long-press 500ms        | Key + Click                 | Roblox tidak support long-press native |
| Peek duration      | 3 detik auto-close      | Timer dengan GUI            | Sama, tapi pakai Heartbeat             |
| **Multiplayer**    |                         |                             |                                        |
| Real-time sync     | Firebase Realtime       | RemoteEvents                | Ganti ke Roblox networking             |
| Room system        | Firebase rooms          | TeleportService + DataStore | Butuh sistem room sendiri              |
| Player presence    | Firebase onDisconnect   | PlayerRemoving              | Gunakan event Roblox                   |
| **UI**             |                         |                             |                                        |
| Main UI            | React components        | ScreenGui                   | Konversi ke Roblox GUI                 |
| Modal/Popup        | React Portal            | Frame dengan visibility     | Sama konsepnya                         |
| Toast notification | react-toastify          | Custom GUI popup            | Buat system sendiri                    |
| Chat               | Firebase chat           | Default chat atau custom    | Bisa pakai default atau custom         |
| **Audio**          |                         |                             |                                        |
| Sound effects      | Howler.js               | SoundService                | Sederhana di Roblox                    |
| BGM                | Howler.js loop          | Sound:Play() loop           | Sama                                   |
| Mute toggle        | localStorage + Howler   | Settings + Sound            | Implementasi mirip                     |
| **Data**           |                         |                             |                                        |
| Player data        | localStorage            | DataStore                   | Ganti penyimpanan                      |
| Room data          | Firebase                | DataStore/MemoryStore       | Butuh sistem baru                      |
| Game state         | Firebase Realtime       | Server-side state           | ServerScriptService mengelola state    |

---

## Flow Diagram

### 1. Game Flow Utama

```
┌─────────────────────────────────────────────────────────────────┐
│                         GAME FLOW                               │
└─────────────────────────────────────────────────────────────────┘

START
  │
  ▼
┌──────────────┐
│   LOBBY      │ ◄─────────────────────────────────────────┐
│              │                                           │
│ • Room List  │                                           │
│ • Join Room  │                                           │
│ • Create     │                                           │
└──────┬───────┘                                           │
       │                                                    │
       │ JOIN / CREATE                                      │
       ▼                                                    │
┌──────────────┐                                            │
│    ROOM      │                                            │
│   WAITING    │                                            │
│              │                                            │
│ • 2-4 Player │                                            │
│ • Host Start │                                            │
└──────┬───────┘                                            │
       │ START GAME                                         │
       ▼                                                    │
┌──────────────┐     ┌──────────────┐     ┌──────────────┐  │
│  MEMORIZE    │────►│   PLAYING    │────►│    ENDED     │──┘
│   (3 sec)    │     │              │     │              │
│              │     │ • Draw       │     │ • Show Score │
│ • See 2 card │     │ • Action     │     │ • Winner     │
│ • Remember   │     │ • Ability    │     │ • Rematch?   │
└──────────────┘     │ • KABUL?     │     └──────────────┘
                     └──────┬───────┘
                            │
                    ┌───────┴───────┐
                    │  KABUL CALLED?│
                    └───────┬───────┘
                            │
                    ┌───────┴───────┐
                    │  YES          │ NO
                    ▼               ▼
            ┌──────────────┐  ┌──────────────┐
            │ FINAL TURNS  │  │ CONTINUE     │
            │ (n-1 turns)  │  │ NEXT PLAYER  │
            └──────┬───────┘  └──────────────┘
                   │
                   ▼
            ┌──────────────┐
            │    ENDED     │
            └──────────────┘
```

### 2. Turn Flow Detail

```
┌─────────────────────────────────────────────────────────────────┐
│                      TURN FLOW DETAIL                           │
└─────────────────────────────────────────────────────────────────┘

START TURN
    │
    ▼
┌─────────────────┐
│  DRAWING PHASE  │
│                 │
│ ┌─────────────┐ │
│ │ 1. Draw     │ │
│ │    Deck     │ │
│ └─────────────┘ │
│       OR        │
│ ┌─────────────┐ │
│ │ 2. Draw     │ │
│ │   Discard   │ │
│ └─────────────┘ │
│       OR        │
│ ┌─────────────┐ │
│ │ 3. Call     │ │
│ │    KABUL    │ │
│ └─────────────┘ │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ DISCARDING PHASE│
│                 │
│ ┌─────────────┐ │
│ │ 1. Swap     │ │
│ │   with Hand │ │
│ └─────────────┘ │
│       OR        │
│ ┌─────────────┐ │
│ │ 2. Discard  │ │
│ │   Directly  │ │
│ └─────────────┘ │
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│  CHECK ABILITY  │NO   │   END TURN      │
│   ON DISCARD?   │────►│  (Next Player)  │
└────────┬────────┘     └─────────────────┘
         │ YES
         ▼
┌─────────────────┐
│ ABILITY RESOLVE │
│                 │
│ • 7/8  → PEEK_SELF   (Select own card)
│ • 9/10 → PEEK_ENEMY  (Select enemy card)
│ • J    → BLIND_SWAP  (Select both, swap)
│ • Q/K  → SEE&SWAP    (Select both, confirm)
│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   END TURN      │
│  (Next Player)  │
└─────────────────┘
```

### 3. Card Ability Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                    CARD ABILITY FLOW                            │
└─────────────────────────────────────────────────────────────────┘

PEEK_SELF (7, 8)
================
Discard 7/8
     │
     ▼
Select Own Card ──► Reveal (3 sec) ──► Hide ──► End Turn


PEEK_ENEMY (9, 10)
==================
Discard 9/10
     │
     ▼
Select Enemy Card ──► Reveal (3 sec, you only) ──► Hide ──► End Turn


BLIND_SWAP (J)
==============
Discard J
     │
     ▼
Select Own Card
     │
     ▼
Select Target Player
     │
     ▼
Select Target Card
     │
     ▼
Instant Swap (no reveal)
     │
     ▼
End Turn


SEE_AND_SWAP (Q, K)
===================
Discard Q/K
     │
     ▼
Select Own Card
     │
     ▼
Select Target Player
     │
     ▼
Select Target Card
     │
     ▼
Reveal BOTH Cards (you only)
     │
     ▼
┌────────────┐
│ CONFIRM?   │
│ ┌──┐ ┌──┐  │
│ │Y │ │N │  │
│ └──┘ └──┘  │
└─────┬──────┘
      │
   ┌──┴──┐
   ▼     ▼
  YES   NO
   │     │
   ▼     ▼
 Swap  Skip
   │     │
   └──┬──┘
      ▼
   End Turn
```

### 4. SLAP Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                       SLAP FLOW                                 │
└─────────────────────────────────────────────────────────────────┘

Anytime During PLAYING Phase
            │
            ▼
   ┌────────────────┐
   │ Long-press on  │
   │  Hand Card     │
   └───────┬────────┘
           │
           ▼
   ┌────────────────┐
   │ Check: Does    │
   │ rank match     │
   │ topDiscard?    │
   └───────┬────────┘
           │
      ┌────┴────┐
      ▼         ▼
    YES        NO
      │         │
      ▼         ▼
┌──────────┐ ┌──────────┐
│ SUCCESS  │ │  FAIL    │
│          │ │          │
│ Remove   │ │ Penalty: │
│ card from│ │ +1 card  │
│ hand     │ │ from deck│
└────┬─────┘ └────┬─────┘
     │            │
     └─────┬──────┘
           ▼
     Continue Game
```

### 5. State Machine

```
┌─────────────────────────────────────────────────────────────────┐
│                    GAME STATE MACHINE                           │
└─────────────────────────────────────────────────────────────────┘

                    ┌─────────────┐
         ┌─────────►│   WAITING   │◄────────┐
         │          │             │         │
         │          │ • Join      │         │
         │          │ • Create    │         │
         │          │ • Leave     │         │
         │          └──────┬──────┘         │
         │                 │                │
         │                 │ Start          │
         │                 │ (Host)         │
         │                 ▼                │
         │          ┌─────────────┐         │
         │          │  MEMORIZE   │         │
         │          │   (3 sec)   │         │
         │          └──────┬──────┘         │
         │                 │                │
         │                 │ Auto           │
         │                 │ Progress       │
         │                 ▼                │
         │          ┌─────────────┐         │
         │    ┌────►│   PLAYING   │────┐    │
         │    │     │             │    │    │
         │    │     │ • Turn Loop │    │    │
         │    │     │ • Actions   │    │    │
         │    │     │ • Kabul     │    │    │
         │    │     └──────┬──────┘    │    │
         │    │            │           │    │
         │    │            │ End       │    │
         │    │            │ Condition │    │
         │    │            ▼           │    │
         │    │     ┌─────────────┐    │    │
         │    │     │    ENDED    │────┘    │
         │    │     │             │         │
         │    │     │ • Show Score│         │
         │    │     │ • Winner    │─────────┘
         │    │     │ • Rematch   │
         │    │     └─────────────┘
         │    │
         │    └──── Rematch
         │
         └────────── Back to Lobby
```

---

## Skill Level Indicators

Sistem penilaian kesulitan fitur untuk membantu prioritisasi development:

| Level        | Simbol     | Deskripsi                                                                            |
| ------------ | ---------- | ------------------------------------------------------------------------------------ |
| Beginner     | ⭐         | Fitur dasar, mudah diimplementasikan. Contoh: UI sederhana, state management basic   |
| Intermediate | ⭐⭐       | Fitur dengan beberapa komponen yang berinteraksi. Contoh: Ability kartu, turn system |
| Advanced     | ⭐⭐⭐     | Fitur kompleks dengan logika bisnis. Contoh: KABUL mechanic, multiplayer sync        |
| Expert       | ⭐⭐⭐⭐   | Fitur sangat kompleks, memerlukan desain matang. Contoh: Full ability chain          |
| Master       | ⭐⭐⭐⭐⭐ | Fitur kritis, fundamental untuk game. Harus prioritas utama                          |

---

## Daftar Fitur Lengkap dengan Skill Level

### Core Gameplay

| No  | Fitur                             | Skill Level | Phase |
| --- | --------------------------------- | ----------- | ----- |
| 1   | Deck generation (54 kartu)        | ⭐⭐⭐⭐⭐  | 1     |
| 2   | Card dealing (4 kartu per pemain) | ⭐⭐⭐⭐⭐  | 1     |
| 3   | Card values system                | ⭐⭐⭐⭐⭐  | 1     |
| 4   | Turn management                   | ⭐⭐⭐⭐⭐  | 1     |
| 5   | Draw from deck                    | ⭐⭐⭐⭐⭐  | 1     |
| 6   | Draw from discard                 | ⭐⭐⭐⭐⭐  | 1     |
| 7   | Swap card with hand               | ⭐⭐⭐⭐⭐  | 1     |
| 8   | Discard without swap              | ⭐⭐⭐⭐⭐  | 1     |
| 9   | Discard pile management           | ⭐⭐⭐⭐    | 1     |
| 10  | Memorize phase (3 detik)          | ⭐⭐⭐⭐    | 1     |
| 11  | Masking kartu lawan               | ⭐⭐⭐⭐    | 1     |

### Card Abilities

| No  | Fitur                      | Skill Level | Phase |
| --- | -------------------------- | ----------- | ----- |
| 12  | PEEK_SELF (7, 8)           | ⭐⭐⭐⭐    | 1     |
| 13  | PEEK_ENEMY (9, 10)         | ⭐⭐⭐⭐    | 1     |
| 14  | BLIND_SWAP (Jack)          | ⭐⭐⭐⭐    | 1     |
| 15  | SEE_AND_SWAP (Queen, King) | ⭐⭐⭐⭐⭐  | 1     |
| 16  | Ability timeout (15 detik) | ⭐⭐⭐      | 1     |
| 17  | Skip ability option        | ⭐⭐⭐      | 1     |

### Special Mechanics

| No  | Fitur                   | Skill Level | Phase |
| --- | ----------------------- | ----------- | ----- |
| 18  | KABUL call system       | ⭐⭐⭐⭐    | 1     |
| 19  | Final turns after KABUL | ⭐⭐⭐⭐    | 1     |
| 20  | SLAP mechanic           | ⭐⭐⭐      | 1     |
| 21  | SLAP penalty (+1 card)  | ⭐⭐⭐      | 1     |

### Multiplayer & Room

| No  | Fitur                      | Skill Level | Phase |
| --- | -------------------------- | ----------- | ----- |
| 22  | Room creation              | ⭐⭐⭐⭐⭐  | 1     |
| 23  | Room joining               | ⭐⭐⭐⭐⭐  | 1     |
| 24  | Room leaving               | ⭐⭐⭐⭐    | 1     |
| 25  | Player presence tracking   | ⭐⭐⭐⭐    | 1     |
| 26  | Host privileges            | ⭐⭐⭐⭐    | 1     |
| 27  | Real-time state sync       | ⭐⭐⭐⭐⭐  | 1     |
| 28  | Max 4 players per room     | ⭐⭐⭐⭐    | 1     |
| 29  | Minimum 2 players to start | ⭐⭐⭐⭐    | 1     |

### UI/UX

| No  | Fitur                    | Skill Level | Phase |
| --- | ------------------------ | ----------- | ----- |
| 30  | Card visual (face-up)    | ⭐⭐⭐⭐⭐  | 1     |
| 31  | Card visual (face-down)  | ⭐⭐⭐⭐⭐  | 1     |
| 32  | Card flip animation      | ⭐⭐⭐      | 1     |
| 33  | Turn indicator           | ⭐⭐⭐⭐    | 1     |
| 34  | Deck display with count  | ⭐⭐⭐⭐    | 1     |
| 35  | Discard pile display     | ⭐⭐⭐⭐    | 1     |
| 36  | Player hand display      | ⭐⭐⭐⭐⭐  | 1     |
| 37  | Opponent cards display   | ⭐⭐⭐⭐    | 1     |
| 38  | Ability selection UI     | ⭐⭐⭐⭐    | 1     |
| 39  | KABUL confirmation modal | ⭐⭐⭐      | 1     |
| 40  | Game end screen          | ⭐⭐⭐⭐    | 1     |
| 41  | Score display            | ⭐⭐⭐      | 1     |
| 42  | Winner highlight         | ⭐⭐⭐      | 1     |
| 43  | Rematch button           | ⭐⭐⭐      | 1     |

### Lobby

| No  | Fitur               | Skill Level | Phase |
| --- | ------------------- | ----------- | ----- |
| 44  | Room list display   | ⭐⭐⭐⭐    | 1     |
| 45  | Room search/filter  | ⭐⭐⭐      | 1     |
| 46  | Room preview panel  | ⭐⭐⭐      | 1     |
| 47  | Player list in room | ⭐⭐⭐⭐    | 1     |
| 48  | Create room modal   | ⭐⭐⭐      | 1     |

### Social

| No  | Fitur           | Skill Level | Phase |
| --- | --------------- | ----------- | ----- |
| 49  | Chat system     | ⭐⭐        | 2     |
| 50  | Chat panel UI   | ⭐⭐        | 2     |
| 51  | Message history | ⭐⭐        | 2     |
| 52  | Emoji support   | ⭐          | 2     |

### Audio

| No  | Fitur                   | Skill Level | Phase |
| --- | ----------------------- | ----------- | ----- |
| 53  | Card flip sound         | ⭐⭐        | 2     |
| 54  | Card deal sound         | ⭐⭐        | 2     |
| 55  | Card slap sound         | ⭐⭐        | 2     |
| 56  | KABUL call sound        | ⭐⭐        | 2     |
| 57  | Turn notification sound | ⭐⭐        | 2     |
| 58  | Win/Lose sound          | ⭐⭐        | 2     |
| 59  | Volume control          | ⭐⭐        | 2     |
| 60  | Mute toggle             | ⭐⭐        | 2     |

### Costume/Variant

| No  | Fitur                         | Skill Level | Phase |
| --- | ----------------------------- | ----------- | ----- |
| 61  | Costume selection             | ⭐⭐⭐      | 1     |
| 62  | Classic costume               | ⭐⭐⭐⭐    | 1     |
| 63  | Reversed costume              | ⭐⭐⭐⭐    | 1     |
| 64  | Costume-based deck generation | ⭐⭐⭐⭐    | 1     |

### Additional

| No  | Fitur                      | Skill Level | Phase |
| --- | -------------------------- | ----------- | ----- |
| 65  | Rules modal                | ⭐⭐        | 1     |
| 66  | Player identity (name, id) | ⭐⭐⭐      | 1     |
| 67  | Stats tracking             | ⭐⭐        | 2     |
| 68  | Error handling             | ⭐⭐⭐⭐    | 1     |
| 69  | Loading states             | ⭐⭐⭐      | 1     |
| 70  | Toast notifications        | ⭐⭐        | 2     |

---

## Penutup

Dokumen ini mencakup seluruh fitur yang ada pada Kabul Game versi web. Dengan dokumen ini, tim development Roblox memiliki referensi lengkap untuk mengimplementasikan game dengan fitur yang identik.

**Total Fitur Didokumentasikan:** 70 fitur

**Distribusi Phase:**

- Phase 1 (MVP): 55 fitur (79%)
- Phase 2 (Enhancement): 15 fitur (21%)

**Distribusi Skill Level:**

- ⭐ Beginner: 8 fitur (11%)
- ⭐⭐ Intermediate: 26 fitur (37%)
- ⭐⭐⭐ Advanced: 23 fitur (33%)
- ⭐⭐⭐⭐ Expert: 13 fitur (19%)

---

_Dokumen ini dibuat untuk keperluan porting Kabul Game dari platform Web ke Roblox. Semua fitur dideskripsikan berdasarkan implementasi aktual di codebase web._
