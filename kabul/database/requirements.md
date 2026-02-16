# Database Requirements: Kabul Card Game Roblox

Dokumen ini menjelaskan kebutuhan sistem penyimpanan data untuk Kabul Card Game di platform Roblox. Dokumen mencakup pemahaman mendalam tentang tipe data, struktur penyimpanan, limitasi sistem, dan proses migrasi dari arsitektur Firebase.

---

## 1. Tipe Data: Volatile (Memory) vs Persistent (DataStore)

### 1.1 Konsep Dasar

Roblox menyediakan dua cara berbeda untuk menyimpan data. Memahami perbedaan ini adalah fondasi dari seluruh arsitektur data game.

**Volatile Memory** adalah data yang disimpan di RAM server. Data ini cepat diakses tapi hilang saat server mati atau restart. **Persistent Storage** menggunakan DataStoreService, data tersimpan di cloud Roblox dan bertahan meski server mati.

### 1.2 Data Volatile (Memory Server)

Data volatile hanya ada di memori server selama permainan berlangsung. Begitu server mati atau semua pemain keluar, data ini hilang selamanya.

**Contoh Data Volatile di Kabul:**

| Data                  | Deskripsi                    | Kenapa Volatile?                         |
| --------------------- | ---------------------------- | ---------------------------------------- |
| GameState.phase       | Fase permainan saat ini      | Hanya relevan selama match berlangsung   |
| GameState.currentTurn | Pemain yang sedang giliran   | Berubah terus, tidak perlu diingat       |
| PlayerData.hand       | Kartu di tangan pemain       | Spesifik untuk satu sesi permainan       |
| Deck                  | Susunan kartu di deck        | Random setiap game, tidak perlu disimpan |
| DiscardPile           | Kartu yang sudah dibuang     | Histori sementara, tidak penting         |
| AbilityState          | Status kemampuan kartu aktif | Transien, hanya untuk momen tertentu     |

**Analogi Sederhana:**

Bayangkan data volatile seperti tulisan di papan tulis putih di ruang meeting. Semua orang di ruangan bisa melihat dan membaca tulisan tersebut. Tapi begitu meeting selesai dan ruangan ditutup, petugas kebersihan akan menghapus papan tulis. Tidak ada yang tersisa untuk meeting berikutnya.

### 1.3 Data Persistent (DataStore)

Data persistent disimpan menggunakan DataStoreService. Data ini akan ada meski server mati, pemain keluar, atau bahkan setelah maintenance Roblox.

**Contoh Data Persistent di Kabul:**

| Data                        | Deskripsi         | Kenapa Persistent?       |
| --------------------------- | ----------------- | ------------------------ |
| PlayerStats.wins            | Jumlah kemenangan | Riwayat prestasi pemain  |
| PlayerStats.losses          | Jumlah kekalahan  | Statistik jangka panjang |
| PlayerStats.gamesPlayed     | Total permainan   | Progress karakter        |
| Preferences.soundMuted      | Setting suara     | Preferensi personal      |
| Preferences.favoriteCostume | Kostum favorit    | Konfigurasi user         |

**Analogi Sederhana:**

Data persistent seperti buku catatan yang disimpan di lemari arsip. Setiap kali selesai menulis, buku dimasukkan ke lemari. Bulan depan, tahun depan, kamu masih bisa membuka lemari dan membaca catatan lama.

### 1.4 Perbandingan Karakteristik

| Aspek           | Volatile (Memory)           | Persistent (DataStore) |
| --------------- | --------------------------- | ---------------------- |
| Lokasi          | RAM server                  | Cloud Roblox           |
| Kecepatan akses | Sangat cepat (microseconds) | Lambat (network call)  |
| Persistensi     | Hilang saat server mati     | Bertahan selamanya     |
| Limit ukuran    | Terbatas RAM (GB)           | 4MB per key            |
| Rate limit      | Tidak ada                   | 60 requests/menit      |
| Use case        | Game state real-time        | Stats dan progress     |

---

## 2. Struktur Key DataStore

### 2.1 Format Key

Semua data persistent menggunakan format key yang konsisten:

```
player_{UserId}
```

**Penjelasan:**

- `player_` adalah prefix untuk semua data pemain
- `{UserId}` adalah UserId unik dari Roblox (angka)
- Contoh: `player_123456789`

**Mengapa Format Ini?**

- UserId dari Roblox sudah unik secara global
- Prefix memudahkan debugging dan identifikasi
- Format sederhana, mudah dibaca

### 2.2 Struktur Value

Data yang tersimpan di setiap key memiliki struktur berikut:

```lua
{
    stats = {
        wins = 0,           -- number: jumlah menang
        losses = 0,         -- number: jumlah kalah
        gamesPlayed = 0,    -- number: total permainan
        kabulCalls = 0,     -- number: berapa kali panggil KABUL
        perfectWins = 0     -- number: menang tanpa kalah sekali pun
    },
    preferences = {
        soundEnabled = true,        -- boolean: suara aktif
        musicVolume = 0.5,          -- number: 0.0 - 1.0
        sfxVolume = 0.7,            -- number: 0.0 - 1.0
        preferredCostume = 1,       -- number: 1, 2, atau 3
        cardBackStyle = "classic"   -- string: style kartu
    },
    achievements = {
        firstWin = false,           -- boolean: sudah menang pertama kali
        tenWins = false,            -- boolean: 10 kemenangan
        kabulMaster = false,        -- boolean: menang dengan KABUL 10x
        speedDemon = false          -- boolean: menang dalam 5 menit
    },
    metadata = {
        createdAt = 1672531200,     -- number: timestamp pertama kali main
        lastPlayed = 1675209600,    -- number: timestamp terakhir main
        version = 1                 -- number: versi struktur data
    }
}
```

### 2.3 Contoh Data Real

```lua
-- Key: player_987654321
local playerData = {
    stats = {
        wins = 42,
        losses = 38,
        gamesPlayed = 80,
        kabulCalls = 15,
        perfectWins = 3
    },
    preferences = {
        soundEnabled = true,
        musicVolume = 0.8,
        sfxVolume = 0.6,
        preferredCostume = 2,
        cardBackStyle = "fantasy"
    },
    achievements = {
        firstWin = true,
        tenWins = true,
        kabulMaster = false,
        speedDemon = true
    },
    metadata = {
        createdAt = 1704067200,
        lastPlayed = 1735689600,
        version = 1
    }
}
```

---

## 3. Limitasi DataStore

### 3.1 Rate Limit

DataStoreService memiliki batasan request yang ketat:

| Tipe Request | Limit                   |
| ------------ | ----------------------- |
| GetAsync     | 60 per menit per server |
| SetAsync     | 60 per menit per server |
| UpdateAsync  | 60 per menit per server |
| RemoveAsync  | 60 per menit per server |

**Implikasi:**

- Tidak bisa menyimpan data setiap detik
- Harus ada strategi caching dan batching
- Simpan stats hanya saat game selesai, bukan setiap giliran

**Contoh Buruk (Jangan Dilakukan):**

```lua
-- JANGAN BEGINI
while gameRunning do
    -- Simpan setiap 5 detik
    wait(5)
    DataStore:SetAsync(key, data)  -- Akan hit limit cepat!
end
```

**Contoh Baik:**

```lua
-- LAKUKAN BEGINI
-- Simpan hanya saat game selesai
function onGameEnd(winner)
    local stats = loadStats(winner.UserId)
    stats.wins += 1
    stats.gamesPlayed += 1
    saveStats(winner.UserId, stats)  -- Sekali saja per game
end
```

### 3.2 Ukuran Data

Setiap key di DataStore maksimal 4MB. Ini sebenarnya sangat besar untuk data pemain.

**Estimasi Ukuran:**

```lua
-- Struktur data kabul sekitar 500-800 bytes
-- Dengan 4MB, bisa menyimpan ribuan record
-- Tapi tetap perlu efisiensi
```

**Best Practice:**

- Jangan simpan data yang bisa dihitung ulang
- Gunakan kompresi jika perlu
- Hapus data lama yang tidak perlu

### 3.3 Throttling dan Failures

DataStore bisa gagal karena:

- Throttling saat traffic tinggi
- Maintenance Roblox
- Network issues

**Solusi:**

```lua
local function saveWithRetry(userId, data, maxRetries)
    maxRetries = maxRetries or 3

    for i = 1, maxRetries do
        local success, errorMsg = pcall(function()
            DataStore:SetAsync("player_" .. userId, data)
        end)

        if success then
            return true
        end

        -- Exponential backoff
        wait(math.pow(2, i))
    end

    return false
end
```

### 3.4 Data Loss Protection

Roblox merekomendasikan menggunakan `UpdateAsync` daripada `SetAsync` untuk mencegah race condition:

```lua
-- Gunakan UpdateAsync untuk keamanan
dataStore:UpdateAsync(key, function(oldData)
    -- oldData adalah data terbaru di server
    -- Transformasi data di sini
    oldData.stats.wins += 1
    return oldData
end)
```

---

## 4. Migrasi Konsep dari Firebase ke Roblox

### 4.1 Firebase vs Roblox: Perbedaan Fundamental

Firebase Realtime Database bekerja dengan model cloud-based. Semua data tersimpan di server Google dan di-sync real-time ke semua client. Roblox menggunakan model server-authoritative. Setiap server Roblox adalah dunia terpisah dengan memori sendiri.

**Perbandingan Arsitektur:**

| Aspek          | Firebase (Web)            | Roblox                         |
| -------------- | ------------------------- | ------------------------------ |
| Lokasi data    | Cloud (global)            | Server instance (regional)     |
| Persistensi    | Semua data tersimpan      | Hanya DataStore yang tersimpan |
| Multi-room     | Banyak room dalam satu DB | 1 server = 1 room otomatis     |
| Real-time sync | Built-in via WebSocket    | RemoteEvents manual            |
| Security       | Security Rules            | Server-side validation         |

### 4.2 Migrasi: Room System

**Di Firebase:**

Firebase memerlukan sistem room manual karena satu aplikasi web melayani banyak grup pemain.

```javascript
// Struktur Firebase
/rooms/
  ├── roomABC123/
  │   ├── config/
  │   ├── gameState/
  │   └── players/
  └── roomXYZ789/
      ├── config/
      ├── gameState/
      └── players/
```

**Di Roblox:**

Roblox tidak perlu sistem room. Setiap server instance secara otomatis menjadi satu room terpisah.

```lua
-- Tidak perlu struktur room
-- Langsung di root server:
GameState = {
    phase = "WAITING",
    players = {},
    deck = {}
}
```

**Implikasi:**

- Tidak ada konsep "room ID" yang bisa dipilih pemain
- Pemain tidak bisa membuat room private dengan password
- Roblox menggunakan matchmaking bawaan untuk memasukkan pemain ke server

### 4.3 Migrasi: Chat System

**Di Firebase:**

```javascript
// Custom implementation
/rooms/{roomId}/chat/
  ├── message1: { sender: "Alice", text: "Hi!", timestamp: 123 }
  └── message2: { sender: "Bob", text: "Hello!", timestamp: 124 }
```

**Di Roblox:**

Gunakan TextChatService bawaan. Tidak perlu implementasi custom.

```lua
-- Gunakan service bawaan Roblox
local TextChatService = game:GetService("TextChatService")

-- Sudah include moderation otomatis
-- Sudah include fitur whisper, team chat
-- Tidak perlu database untuk chat
```

**Keuntungan:**

- Tidak perlu maintenance sistem chat
- Moderasi otomatis dari Roblox
- Optimized untuk platform Roblox

### 4.4 Migrasi: Deck dan Kartu

**Di Firebase:**

Deck disimpan di database dengan security rules khusus agar client tidak bisa cheat.

```javascript
/rooms/{roomId}/
  ├── deck/          // Hanya server yang bisa baca
  ├── discardPile/   // Semua bisa lihat
  └── private/{playerId}/hand  // Hanya pemain yang bisa lihat hand sendiri
```

**Di Roblox:**

Semua data kartu ada di server memory. Client tidak bisa akses langsung.

```lua
-- Di server script (aman, client tidak bisa lihat)
local Deck = {
    { rank = "A", suit = "Hearts", value = 1 },
    { rank = "K", suit = "Spades", value = 10 },
    -- ... 52 kartu + 2 Joker
}

local PlayerHands = {
    [Player1] = { {rank="A", suit="Hearts"}, {rank="7", suit="Clubs"} },
    [Player2] = { {rank="K", suit="Spades"}, {rank="J", suit="Diamonds"} }
}

-- Server kirim hand ke client masing-masing
SendHandToPlayer(Player1, PlayerHands[Player1])
SendHandToPlayer(Player2, PlayerHands[Player2])
```

**Keamanan:**

Lebih aman dari Firebase karena client sama sekali tidak bisa akses server memory. Tidak ada celah untuk memory manipulation.

### 4.5 Migrasi: Game State Synchronization

**Di Firebase:**

Client subscribe ke node gameState. Update otomatis terjadi saat data berubah.

```javascript
// Client mendengarkan perubahan
onValue(gameStateRef, (snapshot) => {
  const gameState = snapshot.val();
  updateUI(gameState);
});
```

**Di Roblox:**

Server mengontrol kapan dan apa yang dikirim ke client menggunakan RemoteEvents.

```lua
-- Server
local GameStateUpdateEvent = Instance.new("RemoteEvent")
GameStateUpdateEvent.Name = "GameStateUpdate"
GameStateUpdateEvent.Parent = ReplicatedStorage

function updateGameState(newState)
    CurrentGameState = newState
    -- Kirim ke semua client
    GameStateUpdateEvent:FireAllClients(newState)
end

-- Client
GameStateUpdateEvent.OnClientEvent:Connect(function(gameState)
    updateUI(gameState)
end)
```

---

## 5. ⭐ Kapan Data Hilang

### 5.1 Skenario Data Volatile Hilang

**Skenario 1: Server Shutdown Normal**

```
1. Game berjalan, semua pemain bermain
2. Game selesai, pemenang ditentukan
3. Stats disimpan ke DataStore (persistent)
4. Semua pemain meninggalkan server
5. Server otomatis shutdown setelah beberapa menit
6. ⭐ GAME STATE HILANG (volatile)
7. ⭐ STATS TERSIMPAN (persistent)
```

**Skenario 2: Server Crash**

```
1. Game sedang berlangsung intens
2. Tiba-tiba server crash (bug, network error)
3. Semua pemain terputus
4. ⭐ SELURUH GAME STATE HILANG
5. ⭐ STATS TIDAK BERUBAH (belum disimpan)
6. Pemain harus mulai game baru
```

**Skenario 3: Maintenance Roblox**

```
1. Roblox melakukan scheduled maintenance
2. Semua server di-restart
3. ⭐ SEMUA GAME STATE HILANG
4. ⭐ STATS TERSIMPAN (di DataStore)
```

### 5.2 Skenario Data Persistent Aman

**Skenario 1: Game Normal**

```
1. Pemain bermain, menang
2. Server simpan stats ke DataStore
3. Pemain keluar
4. Server mati
5. ⭐ STATS TETAP ADA DI CLOUD
6. Pemain login besok, stats masih sama
```

**Skenario 2: Reconnect**

```
1. Pemain A disconnect (wifi mati)
2. Game berlanjut tanpa Pemain A
3. Jika reconnect cepat:
   - Server masih aktif
   - Game state masih ada
   - Pemain A bisa lanjut
4. Jika reconnect lambat:
   - Server sudah mati
   - ⭐ Game state hilang
   - Pemain A cari game baru
```

### 5.3 Tabel Ringkasan: Kapan Data Hilang

| Data            | Kapan Hilang        | Kapan Tersimpan        |
| --------------- | ------------------- | ---------------------- |
| GameState.phase | Server mati         | Selama server aktif    |
| PlayerHand      | Semua pemain keluar | Tidak pernah disimpan  |
| Deck            | Game selesai        | Tidak perlu disimpan   |
| Stats.wins      | Tidak pernah        | Setelah game selesai   |
| Preferences     | Tidak pernah        | Saat user ubah setting |

### 5.4 Best Practice Menghindari Kehilangan Data Penting

**Jangan Simpan Gerakan Kartu ke DataStore**

Ini adalah anti-pattern yang boros dan tidak perlu:

```lua
-- JANGAN LAKUKAN INI
function onPlayerDrawCard(player)
    local card = deck:Draw()
    playerHand:Add(card)

    -- JANGAN SIMPAN SETIAP GERAKAN!
    DataStore:SetAsync("player_" .. player.UserId, {
        currentHand = playerHand:GetCards()
    })
end
```

**Kenapa Ini Buruk:**

- Rate limit akan tercapai dalam beberapa menit
- Data tidak konsisten jika player keluar tengah game
- Memperlambat gameplay karena network call
- Tidak ada manfaat menyimpan state tengah game

**Lakukan Ini Sebaliknya:**

```lua
-- Data volatile di memory saja
function onPlayerDrawCard(player)
    local card = deck:Draw()
    playerHand:Add(card)
    -- Hanya update UI, tidak ke database
end

-- Simpan hanya saat game selesai
function onGameEnd(winner)
    local stats = loadStats(winner.UserId)
    stats.wins += 1
    saveStats(winner.UserId, stats)
end
```

---

## 6. Implementasi DataStore di Roblox

### 6.1 Setup DataStoreService

```lua
local DataStoreService = game:GetService("DataStoreService")
local PlayerDataStore = DataStoreService:GetDataStore("PlayerData")

-- Konstanta
local DATA_VERSION = 1
local MAX_RETRIES = 3
```

### 6.2 Load Data Pemain

```lua
local function loadPlayerData(userId)
    local key = "player_" .. tostring(userId)
    local data = nil

    local success, result = pcall(function()
        return PlayerDataStore:GetAsync(key)
    end)

    if success and result then
        data = result
    else
        -- Data baru atau error, buat default
        data = createDefaultData()
    end

    return data
end

local function createDefaultData()
    return {
        stats = {
            wins = 0,
            losses = 0,
            gamesPlayed = 0,
            kabulCalls = 0,
            perfectWins = 0
        },
        preferences = {
            soundEnabled = true,
            musicVolume = 0.5,
            sfxVolume = 0.7,
            preferredCostume = 1,
            cardBackStyle = "classic"
        },
        achievements = {},
        metadata = {
            createdAt = os.time(),
            lastPlayed = os.time(),
            version = DATA_VERSION
        }
    }
end
```

### 6.3 Save Data Pemain

```lua
local function savePlayerData(userId, data)
    local key = "player_" .. tostring(userId)

    -- Update timestamp
    data.metadata.lastPlayed = os.time()

    local retries = 0
    while retries < MAX_RETRIES do
        local success, errorMsg = pcall(function()
            PlayerDataStore:SetAsync(key, data)
        end)

        if success then
            return true
        end

        retries += 1
        wait(math.pow(2, retries))  -- Exponential backoff
    end

    warn("Failed to save data for user " .. userId)
    return false
end
```

### 6.4 Update Stats Setelah Game

```lua
local function updateStatsAfterGame(userId, isWin, gameData)
    local data = loadPlayerData(userId)

    -- Update stats
    data.stats.gamesPlayed += 1

    if isWin then
        data.stats.wins += 1
    else
        data.stats.losses += 1
    end

    -- Update achievements
    if data.stats.wins == 1 then
        data.achievements.firstWin = true
    end

    if data.stats.wins >= 10 then
        data.achievements.tenWins = true
    end

    -- Simpan
    savePlayerData(userId, data)
end
```

---

## 7. Checklist Implementasi

### 7.1 Volatile Data (Memory)

- [ ] GameState table dengan phase, turn, deck
- [ ] PlayerData table dengan hand, score
- [ ] DiscardPile array
- [ ] AbilityState untuk kemampuan kartu
- [ ] RemoteEvents untuk sync ke client

### 7.2 Persistent Data (DataStore)

- [ ] Key format: `player_{UserId}`
- [ ] Stats: wins, losses, gamesPlayed
- [ ] Preferences: sound, volume, costume
- [ ] Achievements tracking
- [ ] Retry logic untuk failures
- [ ] UpdateAsync untuk race condition prevention

### 7.3 Yang Tidak Perlu Diimplementasi

- [ ] Sistem Room (Roblox handle otomatis)
- [ ] Chat database (gunakan TextChatService)
- [ ] Deck persistence (volatile saja)
- [ ] Hand synchronization (server authority)
- [ ] Real-time database (gunakan RemoteEvents)

---

## 8. Kesimpulan

Sistem database Kabul Card Game di Roblox terdiri dari dua lapisan:

**Lapisan Volatile (Memory Server)** untuk game state real-time. Data ini cepat, responsif, dan hilang saat server mati. Semua data permainan aktif ada di sini.

**Lapisan Persistent (DataStore)** untuk stats dan progress jangka panjang. Data ini tahan lama tapi lebih lambat diakses. Simpan hanya data yang benar-benar perlu bertahan.

**Aturan Emas:**

1. GameState itu VOLATILE. Hilang saat server restart. Ini bukan bug, tapi cara kerja Roblox.
2. Stats itu PERSISTENT. Tersimpan di DataStore dan bertahan selamanya.
3. Jangan simpan setiap gerakan kartu ke DataStore. Itu boros dan tidak perlu.
4. Simpan stats hanya saat game selesai, bukan di tengah permainan.

Dengan memahami perbedaan ini, tim pengembang bisa membuat sistem data yang efisien, aman, dan sesuai dengan arsitektur Roblox.
