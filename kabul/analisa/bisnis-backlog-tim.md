# Dokumen Backlog Tim: Kabul Roblox

> **Versi**: 1.0  
> **Total Sprint**: 4 Sprint (8 Minggu)  
> **Target**: Phase 1 MVP (16 User Stories)  
> **Bahasa**: Indonesia

---

## Daftar Isi

1. [Pengenalan Tim](#pengenalan-tim)
2. [Peran dalam Tim](#peran-dalam-tim)
3. [Sprint 1: Setup dan Dasar](#sprint-1-setup-dan-dasar)
4. [Sprint 2: Kartu dan Deck](#sprint-2-kartu-dan-deck)
5. [Sprint 3: Game Logic](#sprint-3-game-logic)
6. [Sprint 4: Multiplayer dan Polish](#sprint-4-multiplayer-dan-polish)
7. [Ringkasan dan Statistik](#ringkasan-dan-statistik)

---

## Pengenalan Tim

Selamat datang di tim pengembangan Kabul untuk Roblox. Dokumen ini adalah peta jalan lengkap selama 8 minggu ke depan. Setiap sprint dirancang khusus untuk memberikan quick win di awal, memastikan tim pemula tetap termotivasi dan melihat hasil nyata sejak minggu pertama.

### Prinsip Dasar Tim

1. **Quick Win First**: Sprint 1 dirancang agar tim melihat hasil konkret dalam 2 minggu pertama
2. **Learning by Doing**: Setiap task punya level skill yang jelas, memungkinkan tim belajar sambil mengerjakan
3. **Kolaborasi**: Tidak ada yang bekerja sendirian, setiap fitur membutuhkan minimal 2 peran
4. **Iteratif**: Setiap sprint berakhir dengan sesi playtest internal

### Timeline Overview

```
Minggu 1-2  │████████████████████│ Sprint 1: Setup dan Dasar
Minggu 3-4  │████████████████████│ Sprint 2: Kartu dan Deck
Minggu 5-6  │████████████████████│ Sprint 3: Game Logic
Minggu 7-8  │████████████████████│ Sprint 4: Multiplayer dan Polish
```

---

## Peran dalam Tim

Setiap anggota tim memiliki peran spesifik dengan tanggung jawab yang jelas. Peran ini sengaja dibuat agar tim dengan skill berbeda bisa berkontribusi maksimal.

### Builder (Arsitek Dunia)

**Tanggung Jawab Utama:**

- Membangun environment 3D di Roblox Studio
- Menata meja, kursi, dan objek fisik
- Mengatur pencahayaan dan atmosfer
- Memastikan visual game nyaman di mata

**Skill yang Dibutuhkan:**

- Kreativitas dan sense of space
- Pemahaman dasar Roblox Studio
- Kesabaran dalam detailing

**Tools:**

- Roblox Studio (Part, Mesh, Lighting)
- Toolbox (asset gratis)

---

### Scripter (Otak Game)

**Tanggung Jawab Utama:**

- Menulis semua kode Lua untuk logika game
- Mengatur komunikasi Client-Server
- Implementasi sistem game loop
- Debugging dan optimasi performa

**Skill yang Dibutuhkan:**

- Dasar pemrograman (variabel, function, if-else)
- Logika berpikir sistematis
- Kemauan belajar dari error

**Tools:**

- Roblox Studio Script Editor
- Roblox Developer Console
- Dokumentasi Roblox API

---

### Tester (Penjaga Kualitas)

**Tanggung Jawab Utama:**

- Membuat test case berdasarkan user stories
- Melakukan playtest rutin setiap sprint
- Mendokumentasikan bug dengan jelas
- Memverifikasi fitur sebelum dianggap selesai

**Skill yang Dibutuhkan:**

- Attention to detail
- Kemampuan mereproduksi bug
- Komunikasi yang jelas
- Pemahaman aturan game Kabul

**Tools:**

- Spreadsheet untuk bug tracking
- Screen recorder untuk dokumentasi
- Discord untuk komunikasi bug

---

### Designer (Pengalaman Pengguna)

**Tanggung Jawab Utama:**

- Merancang UI/UX yang intuitif
- Membuat prototype dan wireframe
- Mendesain kartu dan visual element
- Memastikan konsistensi visual

**Skill yang Dibutuhkan:**

- Sense of aesthetics
- Empati terhadap pengguna
- Pemahaman prinsip UI/UX dasar
- Kemampuan menggunakan Figma (opsional)

**Tools:**

- Roblox UI Editor
- Figma atau Canva (untuk konsep)
- Reference dari game lain

---

## Legend Skill Level

| Level    | Simbol   | Deskripsi                                   | Siapa yang Bisa                |
| -------- | -------- | ------------------------------------------- | ------------------------------ |
| Pemula   | ⭐       | Task dasar, bisa dipelajari dalam 1-2 hari  | Semua anggota tim              |
| Menengah | ⭐⭐     | Butuh pemahaman konsep, bisa dalam 3-5 hari | Yang sudah lalui task pemula   |
| Lanjutan | ⭐⭐⭐   | Kompleks, butuh pengalaman                  | Scripter/Builder berpengalaman |
| Expert   | ⭐⭐⭐⭐ | Arsitektur sistem, butuh pemahaman mendalam | Lead Developer                 |

---

## Sprint 1: Setup dan Dasar

**Periode**: Minggu 1-2  
**Fokus**: Infrastruktur dasar dan quick win  
**Deliverable**: Room yang bisa di-create dan di-join, environment 3D dasar

### Goals Sprint 1

1. Tim bisa membuat room baru dan join room
2. Environment 3D dasar terbangun (meja, kursi, pencahayaan)
3. Struktur folder Roblox Studio tersusun rapi
4. Semua anggota tim nyaman dengan workflow dasar

### Motivasi Sprint 1

Ini adalah sprint paling penting untuk tim pemula. Di akhir Sprint 1, kalian akan melihat room yang bisa dibuat dan dijoin. Bayangkan betapa kerennya melihat teman-teman masuk ke room yang kalian buat sendiri. Quick win ini akan membangun confidence tim untuk sprint-sprint berikutnya.

---

### Task 1.1: Setup Struktur Folder Roblox Studio

**ID**: T1.1  
**Assignee**: Scripter (dipandu oleh semua tim)  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: US-001, US-002

**Deskripsi:**
Membuat struktur folder dasar di Roblox Studio sesuai dengan arsitektur yang telah ditentukan. Ini seperti menata lemari sebelum memasukkan pakaian.

**Step-by-Step Detail:**

1. **Buka Roblox Studio**, buat place baru bernama "KabulGame"
2. **Buat folder di Workspace:**
   - Klik kanan Workspace → Insert Object → Folder
   - Nama: Environment
   - Ulangi untuk: Cards, Players, Camera
3. **Buat folder di ServerScriptService:**
   - Klik kanan ServerScriptService → Insert Object → Folder
   - Nama: GameLogic
   - Ulangi untuk: RoomManager, DeckManager, AntiCheat
4. **Buat folder di ReplicatedStorage:**
   - Klik kanan ReplicatedStorage → Insert Object → Folder
   - Nama: CardTemplates
   - Ulangi untuk: GameState, SharedConfigs
5. **Buat folder di StarterGui:**
   - Klik kanan StarterGui → Insert Object → Folder
   - Nama: MainUI
   - Ulangi untuk: CardDisplay, NotificationUI

**Deliverable:**

- Screenshot struktur folder yang sudah tersusun
- Semua folder sudah ada di tempat yang benar

**Kriteria Selesai:**

- [ ] Semua folder di Workspace sudah dibuat
- [ ] Semua folder di ServerScriptService sudah dibuat
- [ ] Semua folder di ReplicatedStorage sudah dibuat
- [ ] Semua folder di StarterGui sudah dibuat

---

### Task 1.2: Membangun Meja Kartu 3D

**ID**: T1.2  
**Assignee**: Builder (dibantu Designer)  
**Estimasi Waktu**: 6 jam (bisa dibagi 2-3 hari)  
**Skill Level**: ⭐ Pemula  
**User Story**: US-001 (indirect - membuat environment room)

**Deskripsi:**
Meja adalah pusat perhatian game ini. Meja yang nyaman dilihat akan membuat pengalaman bermain lebih enjoyable. Task ini akan mengajarkan dasar-dasar building di Roblox.

**Step-by-Step Detail:**

1. **Buat base meja:**
   - Insert → Part
   - Ubah Size menjadi 16 x 0.5 x 10 (lebar x tebal x panjang)
   - Material: Wood (Oak)
   - Color: Coklat tua (RGB: 86, 66, 54)
   - Position: 0, 3, 0
   - Anchor: true (penting, agar tidak jatuh)

2. **Buat kaki meja (4 kaki):**
   - Duplicate part base, ubah size: 1 x 3 x 1
   - Posisikan di 4 sudut meja:
     - Kaki 1: -7, 1.5, -4.5
     - Kaki 2: 7, 1.5, -4.5
     - Kaki 3: -7, 1.5, 4.5
     - Kaki 4: 7, 1.5, 4.5
   - Color: sama dengan base (coklat tua)

3. **Buat border meja (opsional tapi recommended):**
   - Part dengan size: 16 x 0.3 x 0.5
   - Posisikan di pinggir meja sebagai border
   - Color: sedikit lebih gelap dari base

4. **Setting collisions:**
   - Pilih semua part meja
   - Properties → CanCollide: true
   - Properties → CanTouch: true

**Deliverable:**

- Meja lengkap dengan 4 kaki di Workspace
- Screenshot dari berbagai sudut

**Kriteria Selesai:**

- [ ] Base meja berukuran 16x0.5x10
- [ ] 4 kaki meja terpasang di sudut
- [ ] Material dan color konsisten
- [ ] Meja tidak jatuh (anchored)

**Tips dari Tim:**
Gunakan plugin "Building Tools by F3X" untuk memudahkan positioning. Plugin ini gratis di Roblox Toolbox.

---

### Task 1.3: Membuat Kursi untuk Pemain

**ID**: T1.3  
**Assignee**: Builder  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: US-001

**Deskripsi:**
Empat kursi yang mengelilingi meja, masing-masing menghadap ke pusat. Kursi ini akan menentukan posisi pemain saat bermain.

**Step-by-Step Detail:**

1. **Buat model kursi dasar:**
   - Base kursi: Part 4 x 0.2 x 4
   - Sandaran: Part 4 x 3 x 0.2
   - Dudukan: Part 3.8 x 0.2 x 3.8
   - Material: Wood
   - Color: sedikit lebih terang dari meja

2. **Posisikan 4 kursi di sekitar meja:**
   - Kursi 1 (Utara): Position 0, 1.1, -8, Rotation 0, 0, 0
   - Kursi 2 (Timur): Position 10, 1.1, 0, Rotation 0, 90, 0
   - Kursi 3 (Selatan): Position 0, 1.1, 8, Rotation 0, 180, 0
   - Kursi 4 (Barat): Position -10, 1.1, 0, Rotation 0, -90, 0

3. **Tambahkan Seat object:**
   - Klik kanan kursi → Insert Object → Seat
   - Seat akan memungkinkan pemain duduk otomatis

4. **Buat model kursi jadi Template:**
   - Group semua part kursi (Ctrl + G)
   - Rename: ChairTemplate
   - Duplicate untuk 4 kursi

**Deliverable:**

- 4 kursi mengelilingi meja
- Setiap kursi menghadap ke pusat meja
- Kursi bisa diduduki (ada Seat)

**Kriteria Selesai:**

- [ ] 4 kursi terpasang di posisi yang benar
- [ ] Semua kursi menghadap ke pusat meja
- [ ] Kursi memiliki Seat object
- [ ] Proporsi kursi nyaman dilihat

---

### Task 1.4: Setup Pencahayaan dan Atmosfer

**ID**: T1.4  
**Assignee**: Builder (dibantu Designer)  
**Estimasi Waktu**: 3 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: US-001

**Deskripsi:**
Pencahayaan yang tepat akan membuat game terasa nyaman dan profesional. Task ini mengajarkan penggunaan Lighting service di Roblox.

**Step-by-Step Detail:**

1. **Setting Lighting properties:**
   - Klik Lighting di Explorer
   - Technology: ShadowMap (untuk bayangan realistis)
   - Brightness: 2 (tidak terlalu terang)
   - Ambient: RGB 120, 120, 120 (cahaya sekitar)

2. **Tambahkan PointLight di atas meja:**
   - Insert → Part (kecil, 1x1x1)
   - Position: 0, 8, 0 (di atas meja)
   - Transparency: 1 (tidak terlihat)
   - Insert Object → PointLight
   - PointLight properties:
     - Brightness: 1
     - Color: RGB 255, 247, 230 (warm white)
     - Range: 20

3. **Tambahkan lampu tambahan di sudut:**
   - Duplicate lampu pusat
   - Posisikan di 4 sudut ruangan
   - Range: 15 (lebih kecil)
   - Brightness: 0.5

4. **Setting Time of Day:**
   - Lighting → ClockTime: 14 (siang hari)
   - GeographicLatitude: 0

5. **Tambahkan ColorCorrection (opsional):**
   - Insert Object → ColorCorrectionEffect
   - Contrast: 0.1
   - Saturation: 0.1
   - TintColor: RGB 255, 255, 245 (sedikit warm)

**Deliverable:**

- Pencahayaan yang nyaman di mata
- Tidak ada area yang terlalu gelap
- Atmosfer hangat dan mengundang

**Kriteria Selesai:**

- [ ] Lighting Technology: ShadowMap
- [ ] PointLight di atas meja dengan warm white
- [ ] Pencahayaan merata di seluruh meja
- [ ] Tidak ada bayangan yang mengganggu

---

### Task 1.5: Membuat Script Room Manager (Server)

**ID**: T1.5  
**Assignee**: Scripter  
**Estimasi Waktu**: 8 jam (bisa dibagi 2-3 hari)  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-001, US-002

**Deskripsi:**
Script ini adalah jantung dari sistem room. Dia mengatur pembuatan room, join room, dan sinkronisasi data antar pemain. Ini adalah task pertama yang melibatkan scripting serius.

**Step-by-Step Detail:**

1. **Buat ModuleScript RoomManager:**
   - ServerScriptService/RoomManager → Insert Object → ModuleScript
   - Nama: RoomManager

2. **Struktur kode dasar:**

   ```lua
   local RoomManager = {}
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local Players = game:GetService("Players")

   -- Folder untuk menyimpan room data
   local RoomsFolder = Instance.new("Folder")
   RoomsFolder.Name = "Rooms"
   RoomsFolder.Parent = ReplicatedStorage

   -- Function untuk membuat room baru
   function RoomManager.createRoom(hostPlayer, roomName)
       local roomId = tostring(math.random(1000, 9999))
       local roomFolder = Instance.new("Folder")
       roomFolder.Name = roomId
       roomFolder.Parent = RoomsFolder

       -- Buat config
       local config = Instance.new("Folder")
       config.Name = "Config"
       config.Parent = roomFolder

       local nameValue = Instance.new("StringValue")
       nameValue.Name = "RoomName"
       nameValue.Value = roomName or "Room " .. roomId
       nameValue.Parent = config

       local hostValue = Instance.new("StringValue")
       hostValue.Name = "HostId"
       hostValue.Value = hostPlayer.UserId
       hostValue.Parent = config

       -- Buat folder players
       local playersFolder = Instance.new("Folder")
       playersFolder.Name = "Players"
       playersFolder.Parent = roomFolder

       return roomId
   end

   -- Function untuk join room
   function RoomManager.joinRoom(player, roomId)
       local room = RoomsFolder:FindFirstChild(roomId)
       if not room then
           return false, "Room tidak ditemukan"
       end

       local playersFolder = room:FindFirstChild("Players")
       if #playersFolder:GetChildren() >= 4 then
           return false, "Room sudah penuh"
       end

       -- Tambahkan player ke room
       local playerData = Instance.new("Folder")
       playerData.Name = tostring(player.UserId)
       playerData.Parent = playersFolder

       local nameValue = Instance.new("StringValue")
       nameValue.Name = "PlayerName"
       nameValue.Value = player.Name
       nameValue.Parent = playerData

       return true, "Berhasil join room"
   end

   return RoomManager
   ```

3. **Buat Script utama di ServerScriptService:**
   - Insert Object → Script
   - Nama: MainServer

   ```lua
   local RoomManager = require(script.Parent.RoomManager.RoomManager)
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   -- RemoteEvents
   local createRoomEvent = Instance.new("RemoteEvent")
   createRoomEvent.Name = "CreateRoom"
   createRoomEvent.Parent = ReplicatedStorage

   local joinRoomEvent = Instance.new("RemoteEvent")
   joinRoomEvent.Name = "JoinRoom"
   joinRoomEvent.Parent = ReplicatedStorage

   -- Handler untuk create room
   createRoomEvent.OnServerEvent:Connect(function(player, roomName)
       local roomId = RoomManager.createRoom(player, roomName)
       createRoomEvent:FireClient(player, roomId)
   end)

   -- Handler untuk join room
   joinRoomEvent.OnServerEvent:Connect(function(player, roomId)
       local success, message = RoomManager.joinRoom(player, roomId)
       joinRoomEvent:FireClient(player, success, message)
   end)
   ```

**Deliverable:**

- ModuleScript RoomManager yang bisa create dan join room
- RemoteEvents untuk komunikasi Client-Server
- Data room tersimpan di ReplicatedStorage

**Kriteria Selesai:**

- [ ] Script bisa membuat room baru
- [ ] Script bisa handle join room
- [ ] Room data tersimpan dengan struktur yang benar
- [ ] Validasi jumlah pemain (max 4)

**Tips dari Tim:**
Jika stuck, ingat analogi restoran: Server adalah dapur, client adalah meja tamu. RoomManager adalah sistem reservasi meja.

---

### Task 1.6: Membuat UI Lobby Sederhana

**ID**: T1.6  
**Assignee**: Designer (dibantu Scripter)  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: US-001, US-002

**Deskripsi:**
UI sederhana yang memungkinkan pemain membuat room baru atau melihat daftar room yang tersedia. Ini adalah quick win yang akan membuat tim merasa puas melihat hasilnya.

**Step-by-Step Detail:**

1. **Buat ScreenGui di StarterGui:**
   - StarterGui → Insert Object → ScreenGui
   - Nama: LobbyUI

2. **Buat frame utama:**
   - LobbyUI → Insert Object → Frame
   - Nama: MainFrame
   - Size: 0.5, 0, 0.6, 0 (50% lebar, 60% tinggi layar)
   - Position: 0.25, 0, 0.2, 0 (tengah layar)
   - BackgroundColor3: RGB 40, 40, 40 (gelap)
   - BorderSizePixel: 0

3. **Tambahkan UICorner untuk rounded corners:**
   - MainFrame → Insert Object → UICorner
   - CornerRadius: 0, 20

4. **Buat judul:**
   - MainFrame → Insert Object → TextLabel
   - Nama: Title
   - Text: "KABUL LOBBY"
   - Size: 1, 0, 0.15, 0
   - BackgroundTransparency: 1
   - TextColor3: RGB 255, 255, 255
   - TextScaled: true
   - Font: Enum.Font.GothamBold

5. **Buat tombol "Buat Room":**
   - MainFrame → Insert Object → TextButton
   - Nama: CreateButton
   - Text: "+ Buat Room Baru"
   - Size: 0.8, 0, 0.1, 0
   - Position: 0.1, 0, 0.2, 0
   - BackgroundColor3: RGB 0, 170, 0 (hijau)
   - TextColor3: RGB 255, 255, 255
   - Insert Object → UICorner

6. **Buat list room (Frame kosong untuk sekarang):**
   - MainFrame → Insert Object → Frame
   - Nama: RoomList
   - Size: 0.9, 0, 0.6, 0
   - Position: 0.05, 0, 0.35, 0
   - BackgroundColor3: RGB 60, 60, 60
   - Insert Object → UICorner

7. **Script sederhana untuk tombol:**
   - Buat LocalScript di CreateButton:

   ```lua
   local button = script.Parent
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local createRoomEvent = ReplicatedStorage:WaitForChild("CreateRoom")

   button.MouseButton1Click:Connect(function()
       createRoomEvent:FireServer("Room " .. tostring(math.random(100, 999)))
   end)
   ```

**Deliverable:**

- UI Lobby yang nyaman dilihat
- Tombol "Buat Room" yang fungsional
- Area untuk menampilkan daftar room

**Kriteria Selesai:**

- [ ] Frame utama berada di tengah layar
- [ ] Tombol "Buat Room" memiliki warna yang menarik
- [ ] UI responsif di berbagai ukuran layar
- [ ] Tombol create room mengirim event ke server

---

### Task 1.7: Integrasi UI dengan Room Manager

**ID**: T1.7  
**Assignee**: Scripter (dibantu Designer)  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-001, US-002

**Deskripsi:**
Menghubungkan UI Lobby dengan RoomManager sehingga pemain benar-benar bisa membuat room dan melihat daftar room.

**Step-by-Step Detail:**

1. **Buat LocalScript untuk listen room list:**
   - StarterGui/LobbyUI → Insert Object → LocalScript
   - Nama: LobbyController

   ```lua
   local Players = game:GetService("Players")
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local player = Players.LocalPlayer

   local RoomsFolder = ReplicatedStorage:WaitForChild("Rooms")
   local RoomListFrame = script.Parent.MainFrame.RoomList

   -- Function untuk update room list
   local function updateRoomList()
       -- Hapus semua child di RoomList kecuali UIListLayout
       for _, child in ipairs(RoomListFrame:GetChildren()) do
           if child:IsA("TextButton") then
               child:Destroy()
           end
       end

       -- Buat tombol untuk setiap room
       for _, room in ipairs(RoomsFolder:GetChildren()) do
           local button = Instance.new("TextButton")
           button.Name = room.Name
           button.Size = UDim2.new(1, -10, 0, 50)
           button.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
           button.TextColor3 = Color3.fromRGB(255, 255, 255)
           button.Text = room.Config.RoomName.Value .. " (" .. #room.Players:GetChildren() .. "/4)"
           button.Parent = RoomListFrame

           button.MouseButton1Click:Connect(function()
               local joinEvent = ReplicatedStorage:WaitForChild("JoinRoom")
               joinEvent:FireServer(room.Name)
           end)
       end
   end

   -- Update saat ada perubahan
   RoomsFolder.ChildAdded:Connect(updateRoomList)
   RoomsFolder.ChildRemoved:Connect(updateRoomList)

   -- Update awal
   updateRoomList()
   ```

2. **Tambahkan UIListLayout:**
   - RoomList → Insert Object → UIListLayout
   - Padding: 0, 5
   - SortOrder: Enum.SortOrder.Name

3. **Handle response dari server:**

   ```lua
   local joinRoomEvent = ReplicatedStorage:WaitForChild("JoinRoom")

   joinRoomEvent.OnClientEvent:Connect(function(success, message)
       if success then
           -- Sembunyikan lobby UI
           script.Parent.MainFrame.Visible = false
           print("Berhasil join room!")
       else
           -- Tampilkan error
           local errorLabel = Instance.new("TextLabel")
           errorLabel.Text = message
           errorLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
           errorLabel.Size = UDim2.new(1, 0, 0, 30)
           errorLabel.Parent = script.Parent.MainFrame

           task.wait(3)
           errorLabel:Destroy()
       end
   end)
   ```

**Deliverable:**

- Room list otomatis update saat ada room baru
- Klik pada room akan mengirim join request
- Feedback sukses/error ditampilkan ke pemain

**Kriteria Selesai:**

- [ ] Room list terupdate otomatis
- [ ] Klik room berhasil mengirim join request
- [ ] Error message muncul jika gagal join
- [ ] UI lobby hilang saat berhasil join

---

### Task 1.8: Testing dan Bug Hunt Sprint 1

**ID**: T1.8  
**Assignee**: Tester (semua tim ikut)  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: Semua US Sprint 1

**Deskripsi:**
Sesi testing menyeluruh untuk Sprint 1. Semua anggota tim akan mencoba membuat room, join room, dan memastikan semuanya berjalan lancar.

**Test Cases:**

1. **Test Case TC-001: Membuat Room**
   - Step: Klik tombol "Buat Room Baru"
   - Expected: Room muncul di list, player menjadi host
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

2. **Test Case TC-002: Join Room**
   - Step: Player B klik room yang dibuat Player A
   - Expected: Player B masuk ke room yang sama
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

3. **Test Case TC-003: Room Penuh**
   - Step: Coba join room yang sudah ada 4 pemain
   - Expected: Error message "Room sudah penuh"
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

4. **Test Case TC-004: Visual Environment**
   - Step: Masuk ke game, lihat sekeliling
   - Expected: Meja, kursi, dan pencahayaan terlihat nyaman
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

**Deliverable:**

- Spreadsheet test case dengan hasil testing
- List bug yang ditemukan (jika ada)
- Screenshot/video dari environment

**Kriteria Selesai:**

- [ ] Semua test case dijalankan
- [ ] Dokumentasi bug lengkap dengan repro steps
- [ ] Environment terlihat profesional dan nyaman

---

## Sprint 2: Kartu dan Deck

**Periode**: Minggu 3-4  
**Fokus**: Sistem kartu 3D dan mekanik dasar  
**Deliverable**: Kartu 3D yang bisa di-generate, shuffle, dan ditampilkan di meja

### Goals Sprint 2

1. Template kartu 3D dengan SurfaceGui berhasil dibuat
2. Sistem deck bisa generate 54 kartu lengkap
3. Kartu bisa di-shuffle dengan algoritma Fisher-Yates
4. Kartu bisa ditampilkan di posisi meja (draw pile, discard pile)

### Motivasi Sprint 2

Sekarang kalian sudah punya room yang fungsional. Di Sprint 2, kalian akan melihat kartu-kartu muncul di atas meja. Bayangkan kepuasan melihat deck kartu yang kalian buat sendiri, lengkap dengan animasi shuffle. Ini adalah momen di mana game mulai terasa seperti game kartu sungguhan.

---

### Task 2.1: Membuat Template Kartu 3D

**ID**: T2.1  
**Assignee**: Builder (dibantu Designer)  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-003, US-004, US-005

**Deskripsi:**
Membuat model kartu 3D yang bisa digunakan untuk semua 54 kartu (13 rank x 4 suit + 2 joker). Kartu akan menggunakan SurfaceGui untuk menampilkan nilai dan gambar.

**Step-by-Step Detail:**

1. **Buat base kartu:**
   - Workspace → Insert Object → Part
   - Nama: CardBase
   - Size: 2, 0.05, 3 (lebar x tebal x tinggi)
   - Material: SmoothPlastic
   - Color: RGB 255, 255, 255 (putih)
   - Anchored: true

2. **Buat SurfaceGui untuk sisi depan (Face):**
   - CardBase → Insert Object → SurfaceGui
   - Nama: FaceSurface
   - Face: Enum.NormalId.Top
   - SizingMode: PixelsPerStud
   - CanvasSize: 200, 300

3. **Buat UI untuk sisi depan:**
   - FaceSurface → Insert Object → Frame
   - Nama: Content
   - Size: 1, 0, 1, 0
   - BackgroundColor3: RGB 255, 255, 255
   - Insert Object → UICorner (radius: 10)
   - Content → Insert Object → TextLabel (rank kiri atas)
   - Nama: RankLabel
   - Position: 0.05, 0, 0.05, 0
   - Size: 0.2, 0, 0.15, 0
   - Text: "A"
   - TextScaled: true
   - Content → Insert Object → TextLabel (suit)
   - Nama: SuitLabel
   - Position: 0.4, 0, 0.35, 0
   - Size: 0.2, 0, 0.3, 0
   - Text: "♠"
   - TextScaled: true
   - TextColor3: RGB 0, 0, 0 (hitam untuk spade/club)

4. **Buat SurfaceGui untuk sisi belakang:**
   - CardBase → Insert Object → SurfaceGui
   - Nama: BackSurface
   - Face: Enum.NormalId.Bottom
   - CanvasSize: 200, 300
   - BackSurface → Insert Object → Frame
   - BackgroundColor3: RGB 0, 50, 100 (biru tua)
   - Insert Object → UICorner
   - Tambahkan pattern atau texture sederhana

5. **Tambahkan atribut kartu:**
   - CardBase → Properties → Attributes
   - Klik + tambah:
     - Rank: "A" (string)
     - Suit: "Spades" (string)
     - Value: 1 (number)
     - Ability: "NONE" (string)

6. **Jadikan Template:**
   - Klik kanan CardBase → Save to Roblox
   - Atau duplicate dan simpan di ReplicatedStorage/CardTemplates

**Deliverable:**

- Model kartu 3D dengan SurfaceGui
- Sisi depan menampilkan rank dan suit
- Sisi belakang dengan pattern biru
- Atribut kartu tersimpan

**Kriteria Selesai:**

- [ ] Kartu berukuran 2x0.05x3
- [ ] SurfaceGui depan menampilkan rank dan suit
- [ ] SurfaceGui belakang menampilkan pattern
- [ ] Atribut Rank, Suit, Value tersimpan

---

### Task 2.2: Membuat Script Deck Manager

**ID**: T2.2  
**Assignee**: Scripter  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-004

**Deskripsi:**
Script untuk mengenerate 54 kartu lengkap (Ace sampai King untuk 4 suit, plus 2 Joker), mengacak deck, dan mengelola draw pile.

**Step-by-Step Detail:**

1. **Buat ModuleScript DeckManager:**
   - ServerScriptService/DeckManager → ModuleScript
   - Nama: DeckManager

2. **Struktur kode:**

   ```lua
   local DeckManager = {}
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   local CardTemplate = ReplicatedStorage:WaitForChild("CardTemplates"):WaitForChild("CardBase")

   -- Definisi kartu
   local SUITS = {"Spades", "Hearts", "Diamonds", "Clubs"}
   local RANKS = {"A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"}
   local ABILITIES = {
       ["7"] = "PEEK_SELF",
       ["8"] = "PEEK_SELF",
       ["9"] = "PEEK_ENEMY",
       ["10"] = "PEEK_ENEMY",
       ["J"] = "BLIND_SWAP",
       ["Q"] = "SEE_AND_SWAP",
       ["K"] = "SEE_AND_SWAP",
   }

   -- Function untuk generate deck
   function DeckManager.generateDeck()
       local deck = {}

       -- Generate kartu reguler
       for _, suit in ipairs(SUITS) do
           for _, rank in ipairs(RANKS) do
               local card = {
                   Rank = rank,
                   Suit = suit,
                   Value = rank == "A" and 1 or tonumber(rank) or 10,
                   Ability = ABILITIES[rank] or "NONE"
               }
               table.insert(deck, card)
           end
       end

       -- Tambahkan 2 Joker
       table.insert(deck, {Rank = "Joker", Suit = "None", Value = 0, Ability = "NONE"})
       table.insert(deck, {Rank = "Joker", Suit = "None", Value = 0, Ability = "NONE"})

       return deck
   end

   -- Function untuk shuffle (Fisher-Yates)
   function DeckManager.shuffle(deck)
       for i = #deck, 2, -1 do
           local j = math.random(i)
           deck[i], deck[j] = deck[j], deck[i]
       end
       return deck
   end

   -- Function untuk spawn kartu fisik
   function DeckManager.spawnCard(cardData, position, parent)
       local card = CardTemplate:Clone()
       card:SetAttribute("Rank", cardData.Rank)
       card:SetAttribute("Suit", cardData.Suit)
       card:SetAttribute("Value", cardData.Value)
       card:SetAttribute("Ability", cardData.Ability)

       -- Update visual
       local faceSurface = card:FindFirstChild("FaceSurface")
       if faceSurface then
           local content = faceSurface:FindFirstChild("Content")
           if content then
               local rankLabel = content:FindFirstChild("RankLabel")
               local suitLabel = content:FindFirstChild("SuitLabel")
               if rankLabel then rankLabel.Text = cardData.Rank end
               if suitLabel then
                   suitLabel.Text = cardData.Suit == "Spades" and "♠" or
                                   cardData.Suit == "Hearts" and "♥" or
                                   cardData.Suit == "Diamonds" and "♦" or
                                   cardData.Suit == "Clubs" and "♣" or "🃏"
                   -- Warna suit
                   if cardData.Suit == "Hearts" or cardData.Suit == "Diamonds" then
                       suitLabel.TextColor3 = Color3.fromRGB(200, 0, 0)
                   else
                       suitLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
                   end
               end
           end
       end

       card.Position = position
       card.Parent = parent
       return card
   end

   return DeckManager
   ```

**Deliverable:**

- ModuleScript DeckManager lengkap
- Bisa generate 54 kartu
- Bisa shuffle deck
- Bisa spawn kartu fisik dengan data yang benar

**Kriteria Selesai:**

- [ ] Deck berisi 54 kartu
- [ ] Setiap kartu memiliki Rank, Suit, Value yang benar
- [ ] Shuffle menggunakan algoritma Fisher-Yates
- [ ] Kartu fisik menampilkan rank dan suit dengan benar

---

### Task 2.3: Menempatkan Kartu di Meja

**ID**: T2.3  
**Assignee**: Builder (dibantu Scripter)  
**Estimasi Waktu**: 5 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-004, US-005

**Deskripsi:**
Menempatkan deck kartu di posisi meja dan membuat discard pile. Kartu akan ditumpuk dengan posisi yang sedikit acak agar terlihat natural.

**Step-by-Step Detail:**

1. **Buat folder untuk kartu di meja:**
   - Workspace/Cards/DrawPile (Folder)
   - Workspace/Cards/DiscardPile (Folder)

2. **Tentukan posisi:**
   - Draw Pile: Position 0, 3.5, -4 (di atas meja, sedikit ke belakang)
   - Discard Pile: Position 0, 3.5, 4 (di depan meja)

3. **Spawn deck ke meja:**

   ```lua
   local function spawnDeckToTable()
       local deck = DeckManager.generateDeck()
       deck = DeckManager.shuffle(deck)

       local drawPileFolder = workspace.Cards.DrawPile
       local startPos = Vector3.new(0, 3.5, -4)

       for i, cardData in ipairs(deck) do
           -- Posisi sedikit acak agar terlihat natural
           local offsetX = math.random(-5, 5) / 100
           local offsetZ = math.random(-5, 5) / 100
           local offsetY = (i - 1) * 0.02 -- Tumpuk ke atas

           local pos = startPos + Vector3.new(offsetX, offsetY, offsetZ)
           local rotation = CFrame.Angles(0, math.random(-10, 10) * math.pi / 180, 0)

           local card = DeckManager.spawnCard(cardData, pos, drawPileFolder)
           card.CFrame = CFrame.new(pos) * rotation
       end

       return #deck
   end
   ```

4. **Test spawn deck:**
   - Jalankan script dan lihat 54 kartu muncul di atas meja
   - Pastikan kartu tertumpuk rapi tapi tidak terlalu rapi (natural)

**Deliverable:**

- 54 kartu tertata di draw pile
- Kartu tersusun dengan posisi sedikit acak
- Setiap kartu bisa dilihat (untuk debug)

**Kriteria Selesai:**

- [ ] 54 kartu muncul di meja
- [ ] Kartu tersusun di draw pile
- [ ] Posisi tumpukan terlihat natural
- [ ] Rotasi kartu sedikit acak

---

### Task 2.4: Membuat Kartu Tangan (Hand UI)

**ID**: T2.4  
**Assignee**: Designer (dibantu Scripter)  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: US-003

**Deskripsi:**
Membuat UI untuk menampilkan 4 kartu di tangan pemain. Kartu di tangan akan ditampilkan di bagian bawah layar dan bisa diklik.

**Step-by-Step Detail:**

1. **Buat ScreenGui untuk hand:**
   - StarterGui → Insert Object → ScreenGui
   - Nama: HandUI

2. **Buat container untuk kartu:**
   - HandUI → Insert Object → Frame
   - Nama: HandContainer
   - Size: 0.6, 0, 0.25, 0 (60% lebar, 25% tinggi)
   - Position: 0.2, 0, 0.7, 0 (di bagian bawah)
   - BackgroundTransparency: 1

3. **Tambahkan UIListLayout:**
   - HandContainer → Insert Object → UIListLayout
   - FillDirection: Horizontal
   - HorizontalAlignment: Center
   - Padding: 0, 10

4. **Buat template slot kartu:**
   - HandContainer → Insert Object → Frame
   - Nama: CardSlotTemplate
   - Size: 0, 120, 1, -20
   - BackgroundColor3: RGB 60, 60, 60
   - Insert Object → UICorner (radius: 10)
   - Insert Object → UIAspectRatioConstraint (AspectRatio: 0.67)
   - Visible: false (jadi template)

5. **Buat 4 slot kartu:**
   - Duplicate CardSlotTemplate 4x
   - Rename: CardSlot1, CardSlot2, CardSlot3, CardSlot4
   - Visible: true

6. **Tambahkan label untuk kartu:**
   - Setiap CardSlot → Insert Object → TextLabel
   - Nama: CardLabel
   - Size: 1, 0, 1, 0
   - BackgroundTransparency: 1
   - Text: "?" (placeholder)
   - TextScaled: true
   - TextColor3: RGB 255, 255, 255

7. **Script untuk update hand:**
   - HandUI → Insert Object → LocalScript

   ```lua
   local Players = game:GetService("Players")
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local player = Players.LocalPlayer

   local slots = {
       script.Parent.HandContainer.CardSlot1,
       script.Parent.HandContainer.CardSlot2,
       script.Parent.HandContainer.CardSlot3,
       script.Parent.HandContainer.CardSlot4,
   }

   -- Function untuk update tampilan kartu
   local function updateHand(handData)
       for i, slot in ipairs(slots) do
           local card = handData[i]
           local label = slot:FindFirstChild("CardLabel")

           if card then
               if card.hidden then
                   label.Text = "🂠"
                   label.TextColor3 = Color3.fromRGB(100, 100, 255)
               else
                   label.Text = card.Rank .. " " .. (card.Suit == "Spades" and "♠" or
                                                    card.Suit == "Hearts" and "♥" or
                                                    card.Suit == "Diamonds" and "♦" or
                                                    card.Suit == "Clubs" and "♣")
                   -- Warna berdasarkan suit
                   if card.Suit == "Hearts" or card.Suit == "Diamonds" then
                       label.TextColor3 = Color3.fromRGB(200, 0, 0)
                   else
                       label.TextColor3 = Color3.fromRGB(0, 0, 0)
                   end
               end
           else
               label.Text = "?"
               label.TextColor3 = Color3.fromRGB(150, 150, 150)
           end
       end
   end

   -- Listen ke perubahan game state
   local gameState = ReplicatedStorage:WaitForChild("GameState")
   -- Update logic akan ditambahkan di Sprint 3
   ```

**Deliverable:**

- UI hand dengan 4 slot kartu di bagian bawah layar
- Slot bisa menampilkan nilai kartu atau tertutup
- Layout responsif dan nyaman dilihat

**Kriteria Selesai:**

- [ ] 4 slot kartu muncul di bagian bawah layar
- [ ] Slot memiliki ukuran yang proporsional
- [ ] Placeholder "?" muncul di slot kosong
- [ ] Kartu tertutup ditandai dengan ikon 🂠

---

### Task 2.5: Implementasi Animasi Kartu Dasar

**ID**: T2.5  
**Assignee**: Scripter  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-004, US-006

**Deskripsi:**
Membuat animasi dasar untuk kartu: draw (mengambil kartu), discard (membuang kartu), dan swap (menukar kartu). Animasi akan menggunakan TweenService.

**Step-by-Step Detail:**

1. **Buat ModuleScript AnimationManager:**
   - ServerScriptService/AnimationManager → ModuleScript

2. **Struktur kode:**

   ```lua
   local AnimationManager = {}
   local TweenService = game:GetService("TweenService")

   -- Config untuk animasi
   local ANIMATION_CONFIG = {
       Draw = {
           Duration = 0.5,
           EasingStyle = Enum.EasingStyle.Quad,
           EasingDirection = Enum.EasingDirection.Out,
       },
       Discard = {
           Duration = 0.4,
           EasingStyle = Enum.EasingStyle.Quad,
           EasingDirection = Enum.EasingDirection.In,
       },
       Swap = {
           Duration = 0.6,
           EasingStyle = Enum.EasingStyle.Quad,
           EasingDirection = Enum.EasingDirection.InOut,
       },
   }

   -- Function untuk animate draw
   function AnimationManager.animateDraw(card, targetPosition, targetRotation)
       local config = ANIMATION_CONFIG.Draw

       local tweenInfo = TweenInfo.new(
           config.Duration,
           config.EasingStyle,
           config.EasingDirection
       )

       local targetCFrame = CFrame.new(targetPosition) * targetRotation

       local tween = TweenService:Create(card, tweenInfo, {
           Position = targetPosition,
           Orientation = targetRotation
       })

       tween:Play()
       return tween
   end

   -- Function untuk animate discard
   function AnimationManager.animateDiscard(card, discardPosition)
       local config = ANIMATION_CONFIG.Discard

       local tweenInfo = TweenInfo.new(
           config.Duration,
           config.EasingStyle,
           config.EasingDirection
       )

       -- Flip kartu saat discard
       local flipTween = TweenService:Create(card, tweenInfo, {
           Orientation = Vector3.new(180, card.Orientation.Y, card.Orientation.Z)
       })

       flipTween:Play()

       -- Pindahkan ke discard pile
       task.wait(config.Duration * 0.5)
       local moveTween = TweenService:Create(card, tweenInfo, {
           Position = discardPosition
       })
       moveTween:Play()

       return moveTween
   end

   -- Function untuk animate swap
   function AnimationManager.animateSwap(card1, card2, midPoint)
       local config = ANIMATION_CONFIG.Swap

       -- Kartu 1 ke midpoint
       local tween1 = TweenService:Create(card1, TweenInfo.new(config.Duration * 0.5), {
           Position = midPoint
       })

       -- Kartu 2 ke posisi kartu 1
       local tween2 = TweenService:Create(card2, TweenInfo.new(config.Duration), {
           Position = card1.Position
       })

       tween1:Play()
       task.wait(config.Duration * 0.5)
       tween2:Play()

       -- Kartu 1 ke posisi kartu 2
       local tween3 = TweenService:Create(card1, TweenInfo.new(config.Duration * 0.5), {
           Position = card2.Position
       })
       tween3:Play()

       return tween3
   end

   return AnimationManager
   ```

**Deliverable:**

- Animasi draw yang smooth
- Animasi discard dengan flip
- Animasi swap untuk tukar kartu

**Kriteria Selesai:**

- [ ] Animasi draw berjalan dalam 0.5 detik
- [ ] Animasi discard dengan flip kartu
- [ ] Animasi swap smooth dan terlihat natural
- [ ] Semua animasi menggunakan TweenService

---

### Task 2.6: Testing dan Bug Hunt Sprint 2

**ID**: T2.6  
**Assignee**: Tester (semua tim ikut)  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐ Pemula  
**User Story**: Semua US Sprint 2

**Deskripsi:**
Testing menyeluruh untuk sistem kartu yang telah dibuat.

**Test Cases:**

1. **Test Case TC-005: Generate Deck**
   - Step: Jalankan script generate deck
   - Expected: 54 kartu muncul dengan rank dan suit yang benar
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

2. **Test Case TC-006: Shuffle Deck**
   - Step: Shuffle deck 3x, bandingkan urutan
   - Expected: Urutan berbeda setiap kali
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

3. **Test Case TC-007: Visual Kartu**
   - Step: Lihat kartu dari berbagai sudut
   - Expected: SurfaceGui terlihat jelas
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

4. **Test Case TC-008: Animasi Draw**
   - Step: Jalankan animasi draw
   - Expected: Kartu bergerak smooth ke target
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

**Deliverable:**

- Test result spreadsheet
- Video/screenshot animasi kartu
- List bug yang ditemukan

---

## Sprint 3: Game Logic

**Periode**: Minggu 5-6  
**Fokus**: Logika permainan lengkap  
**Deliverable**: Game yang bisa dimainkan dari awal sampai akhir

### Goals Sprint 3

1. Sistem giliran (turn management) berfungsi
2. Semua aksi (draw, swap, discard, ability) bekerja
3. Mekanik KABUL dan SLAP diimplementasi
4. Game bisa berakhir dengan kalkulasi skor

### Motivasi Sprint 3

Ini adalah sprint paling menantang tapi paling rewarding. Di akhir Sprint 3, kalian akan bisa memainkan game Kabul lengkap dari awal sampai akhir. Bayangkan sensasi memanggil "KABUL" dan melihat game berakhir dengan kalkulasi skor. Ini adalah momen di mana semua kerja keras kalian berbuah.

---

### Task 3.1: Membuat Turn Manager

**ID**: T3.1  
**Assignee**: Scripter  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-004, US-015

**Deskripsi:**
Mengelola giliran pemain, fase game (DRAWING, SWAPPING, DISCARDING), dan transisi antar pemain.

**Step-by-Step Detail:**

1. **Buat ModuleScript TurnManager:**

   ```lua
   local TurnManager = {}
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   -- Phase enum
   local PHASES = {
       WAITING = "WAITING",
       MEMORIZE = "MEMORIZE",
       DRAWING = "DRAWING",
       SWAPPING = "SWAPPING",
       DISCARDING = "DISCARDING",
       ABILITY = "ABILITY",
       ENDED = "ENDED"
   }

   -- Current state
   local currentState = {
       phase = PHASES.WAITING,
       currentTurn = nil,
       turnOrder = {},
       turnStartTime = nil,
       kabulCaller = nil,
   }

   -- Function untuk set turn order
   function TurnManager.setTurnOrder(players)
       currentState.turnOrder = {}
       for _, player in ipairs(players) do
           table.insert(currentState.turnOrder, player.UserId)
       end
       currentState.currentTurn = currentState.turnOrder[1]
   end

   -- Function untuk next turn
   function TurnManager.nextTurn()
       local currentIndex = table.find(currentState.turnOrder, currentState.currentTurn)
       if not currentIndex then return end

       local nextIndex = currentIndex % #currentState.turnOrder + 1
       local nextPlayer = currentState.turnOrder[nextIndex]

       -- Skip kabul caller di final turns
       if currentState.kabulCaller and nextPlayer == currentState.kabulCaller then
           nextIndex = nextIndex % #currentState.turnOrder + 1
           nextPlayer = currentState.turnOrder[nextIndex]
       end

       currentState.currentTurn = nextPlayer
       currentState.phase = PHASES.DRAWING
       currentState.turnStartTime = tick()

       -- Update ReplicatedStorage
       local gameState = ReplicatedStorage:FindFirstChild("GameState")
       if gameState then
           gameState:SetAttribute("CurrentTurn", nextPlayer)
           gameState:SetAttribute("Phase", PHASES.DRAWING)
       end

       return nextPlayer
   end

   -- Function untuk set phase
   function TurnManager.setPhase(phase)
       currentState.phase = phase
       local gameState = ReplicatedStorage:FindFirstChild("GameState")
       if gameState then
           gameState:SetAttribute("Phase", phase)
       end
   end

   -- Function untuk call Kabul
   function TurnManager.callKabul(playerId)
       currentState.kabulCaller = playerId
       -- Semua pemain lain dapat satu giliran terakhir
   end

   -- Function untuk get current state
   function TurnManager.getState()
       return table.clone(currentState)
   end

   return TurnManager
   ```

**Deliverable:**

- TurnManager yang bisa mengatur urutan giliran
- Phase management berfungsi
- Skip logic untuk kabul caller

**Kriteria Selesai:**

- [ ] Turn order bisa di-set dengan benar
- [ ] Next turn berpindah ke pemain berikutnya
- [ ] Phase bisa diubah dan tersimpan
- [ ] Kabul caller di-skip di final turns

---

### Task 3.2: Implementasi Draw dan Discard

**ID**: T3.2  
**Assignee**: Scripter  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-004, US-005, US-007

**Deskripsi:**
Mengimplementasikan aksi mengambil kartu dari deck atau discard pile, dan membuang kartu.

**Step-by-Step Detail:**

1. **Extend DeckManager dengan draw:**

   ```lua
   -- Function untuk draw dari deck
   function DeckManager.drawFromDeck(deck)
       if #deck == 0 then
           return nil, "Deck habis"
       end
       return table.remove(deck, 1), nil
   end

   -- Function untuk draw dari discard
   function DeckManager.drawFromDiscard(discardPile)
       if #discardPile == 0 then
           return nil, "Discard pile kosong"
       end
       return table.remove(discardPile), nil
   end

   -- Function untuk discard
   function DeckManager.discard(card, discardPile)
       table.insert(discardPile, card)
       return true
   end
   ```

2. **Buat ActionHandler:**

   ```lua
   local ActionHandler = {}
   local TurnManager = require(script.Parent.TurnManager)
   local DeckManager = require(script.Parent.DeckManager)

   -- Validasi aksi
   local function validateAction(playerId, actionType)
       local state = TurnManager.getState()

       -- Cek turn
       if state.currentTurn ~= playerId then
           return false, "Bukan giliranmu"
       end

       -- Cek phase
       if actionType == "DRAW" and state.phase ~= "DRAWING" then
           return false, "Bukan fase mengambil kartu"
       end

       if actionType == "DISCARD" and state.phase ~= "DISCARDING" then
           return false, "Bukan fase membuang kartu"
       end

       return true, nil
   end

   -- Handler untuk draw
   function ActionHandler.handleDraw(playerId, source)
       local valid, error = validateAction(playerId, "DRAW")
       if not valid then
           return false, error
       end

       local card, error
       if source == "DECK" then
           card, error = DeckManager.drawFromDeck(gameDeck)
       else
           card, error = DeckManager.drawFromDiscard(discardPile)
       end

       if not card then
           return false, error
       end

       -- Simpan kartu sementara di tangan player
       temporaryCards[playerId] = card

       -- Pindah ke phase SWAPPING
       TurnManager.setPhase("SWAPPING")

       return true, card
   end

   -- Handler untuk discard
   function ActionHandler.handleDiscard(playerId, handIndex)
       local valid, error = validateAction(playerId, "DISCARD")
       if not valid then
           return false, error
       end

       local tempCard = temporaryCards[playerId]
       if not tempCard then
           return false, "Tidak ada kartu untuk dibuang"
       end

       -- Masukkan ke discard pile
       DeckManager.discard(tempCard, discardPile)

       -- Clear temporary
       temporaryCards[playerId] = nil

       -- Cek ability
       if tempCard.Ability ~= "NONE" then
           TurnManager.setPhase("ABILITY")
           return true, {action = "ABILITY", ability = tempCard.Ability}
       end

       -- Next turn
       TurnManager.nextTurn()
       return true, {action = "NEXT_TURN"}
   end

   return ActionHandler
   ```

**Deliverable:**

- Draw dari deck dan discard pile berfungsi
- Discard kartu berfungsi
- Validasi turn dan phase bekerja

**Kriteria Selesai:**

- [ ] Draw kartu mengurangi deck count
- [ ] Validasi mencegah draw saat bukan giliran
- [ ] Discard memindahkan kartu ke discard pile
- [ ] Ability card trigger phase ABILITY

---

### Task 3.3: Implementasi Swap Kartu

**ID**: T3.3  
**Assignee**: Scripter (dibantu Designer)  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-006

**Deskripsi:**
Mengimplementasikan mekanik menukar kartu yang baru di-draw dengan kartu di tangan.

**Step-by-Step Detail:**

1. **Tambahkan handler swap:**

   ```lua
   function ActionHandler.handleSwap(playerId, handIndex)
       local valid, error = validateAction(playerId, "SWAP")
       if not valid then
           return false, error
       end

       local tempCard = temporaryCards[playerId]
       if not tempCard then
           return false, "Tidak ada kartu untuk ditukar"
       end

       local playerHand = playerHands[playerId]
       if not playerHand or not playerHand[handIndex] then
           return false, "Index kartu tidak valid"
       end

       -- Tukar kartu
       local oldCard = playerHand[handIndex]
       playerHand[handIndex] = tempCard

       -- Buang kartu lama
       DeckManager.discard(oldCard, discardPile)

       -- Clear temporary
       temporaryCards[playerId] = nil

       -- Cek ability dari kartu yang dibuang
       if oldCard.Ability ~= "NONE" then
           TurnManager.setPhase("ABILITY")
           return true, {action = "ABILITY", ability = oldCard.Ability, cardIndex = handIndex}
       end

       -- Next turn
       TurnManager.nextTurn()
       return true, {action = "NEXT_TURN"}
   end
   ```

2. **UI untuk select kartu:**
   - Saat phase SWAPPING, highlight kartu di hand
   - Klik kartu akan trigger swap
   - Tombol "Buang Saja" untuk discard tanpa swap

**Deliverable:**

- Swap kartu berfungsi
- UI untuk memilih kartu di hand
- Kartu yang ditukar masuk ke discard

**Kriteria Selesai:**

- [ ] Klik kartu di hand trigger swap
- [ ] Kartu yang ditukar pindah ke discard
- [ ] Tombol "Buang Saja" tersedia
- [ ] Ability card trigger setelah swap

---

### Task 3.4: Implementasi Ability Cards

**ID**: T3.4  
**Assignee**: Scripter  
**Estimasi Waktu**: 10 jam  
**Skill Level**: ⭐⭐⭐⭐ Expert  
**User Story**: US-008, US-009, US-010, US-011

**Deskripsi:**
Mengimplementasikan 4 ability cards: Peek Self (7/8), Peek Enemy (9/10), Blind Swap (Jack), See and Swap (Queen/King).

**Step-by-Step Detail:**

1. **Buat AbilityManager:**

   ```lua
   local AbilityManager = {}
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   -- State untuk ability
   local abilityState = {
       activePlayer = nil,
       abilityType = nil,
       step = 0,
       selectedCard = nil,
       selectedTarget = nil,
   }

   -- Handler untuk Peek Self (7/8)
   function AbilityManager.handlePeekSelf(playerId, cardIndex)
       abilityState.activePlayer = playerId
       abilityState.abilityType = "PEEK_SELF"

       -- Reveal kartu ke pemain
       local hand = playerHands[playerId]
       local card = hand[cardIndex]

       -- Kirim ke client untuk reveal
       ReplicatedStorage.AbilityReveal:FireClient(
           game.Players:GetPlayerByUserId(playerId),
           {
               cardIndex = cardIndex,
               cardData = card,
               duration = 3
           }
       )

       -- Tunggu 3 detik
       task.wait(3)

       -- Hide lagi dan lanjutkan
       AbilityManager.endAbility()
   end

   -- Handler untuk Peek Enemy (9/10)
   function AbilityManager.handlePeekEnemy(playerId, targetId, cardIndex)
       abilityState.activePlayer = playerId
       abilityState.abilityType = "PEEK_ENEMY"

       local targetHand = playerHands[targetId]
       local card = targetHand[cardIndex]

       -- Reveal ke pemain yang pakai ability
       ReplicatedStorage.AbilityReveal:FireClient(
           game.Players:GetPlayerByUserId(playerId),
           {
               targetId = targetId,
               cardIndex = cardIndex,
               cardData = card,
               duration = 3
           }
       )

       -- Target tidak tahu kartunya dilihat
       task.wait(3)
       AbilityManager.endAbility()
   end

   -- Handler untuk Blind Swap (Jack)
   function AbilityManager.handleBlindSwap(playerId, ownIndex, targetId, targetIndex)
       local ownHand = playerHands[playerId]
       local targetHand = playerHands[targetId]

       -- Swap tanpa reveal
       ownHand[ownIndex], targetHand[targetIndex] =
           targetHand[targetIndex], ownHand[ownIndex]

       -- Animasi swap
       AnimationManager.animateSwap(
           ownHand[ownIndex],
           targetHand[targetIndex],
           midPoint
       )

       AbilityManager.endAbility()
   end

   -- Handler untuk See and Swap (Q/K)
   function AbilityManager.handleSeeAndSwap(playerId, ownIndex, targetId, targetIndex, confirm)
       local ownHand = playerHands[playerId]
       local targetHand = playerHands[targetId]

       -- Reveal kedua kartu
       ReplicatedStorage.AbilityReveal:FireClient(
           game.Players:GetPlayerByUserId(playerId),
           {
               ownCard = ownHand[ownIndex],
               targetCard = targetHand[targetIndex],
               waitForConfirm = true
           }
       )

       -- Tunggu confirm dari client
       -- Kalau confirm = true, swap. Kalau false, cancel.
       if confirm then
           ownHand[ownIndex], targetHand[targetIndex] =
               targetHand[targetIndex], ownHand[ownIndex]
       end

       AbilityManager.endAbility()
   end

   function AbilityManager.endAbility()
       TurnManager.nextTurn()
       abilityState = {
           activePlayer = nil,
           abilityType = nil,
           step = 0,
           selectedCard = nil,
           selectedTarget = nil,
       }
   end

   return AbilityManager
   ```

**Deliverable:**

- Semua 4 ability berfungsi
- UI reveal untuk peek abilities
- UI confirm/cancel untuk see and swap

**Kriteria Selesai:**

- [ ] Peek Self reveal kartu sendiri 3 detik
- [ ] Peek Enemy reveal kartu lawan 3 detik
- [ ] Blind Swap tukar kartu tanpa reveal
- [ ] See and Swap reveal dulu, baru confirm

---

### Task 3.5: Implementasi KABUL dan SLAP

**ID**: T3.5  
**Assignee**: Scripter  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-012, US-013

**Deskripsi:**
Mengimplementasikan mekanik KABUL (mengakhiri game) dan SLAP (membuang kartu dengan cepat).

**Step-by-Step Detail:**

1. **Implementasi KABUL:**

   ```lua
   function ActionHandler.handleKabul(playerId)
       local state = TurnManager.getState()

       -- Validasi
       if state.phase ~= "DRAWING" then
           return false, "Hanya bisa KABUL di awal giliran"
       end

       if state.currentTurn ~= playerId then
           return false, "Bukan giliranmu"
       end

       -- Set kabul caller
       TurnManager.callKabul(playerId)

       -- Broadcast ke semua pemain
       ReplicatedStorage.KabulCalled:FireAllClients({
           callerId = playerId,
           callerName = game.Players:GetPlayerByUserId(playerId).Name
       })

       -- Play sound effect
       -- (akan diimplementasi di Sprint 4)

       -- Set final turns
       TurnManager.setFinalTurns()

       return true, "KABUL berhasil dipanggil!"
   end
   ```

2. **Implementasi SLAP:**
   ```lua
   function ActionHandler.handleSlap(playerId, handIndex)
       local state = TurnManager.getState()

       -- Bisa slap kapan saja saat PLAYING
       if state.phase ~= "PLAYING" and state.phase ~= "DRAWING" then
           return false, "Tidak bisa slap saat ini"
       end

       local hand = playerHands[playerId]
       local card = hand[handIndex]
       local topDiscard = discardPile[#discardPile]

       -- Cek match
       if card.Rank == topDiscard.Rank then
           -- Success! Buang kartu
           DeckManager.discard(card, discardPile)
           table.remove(hand, handIndex)

           return true, {
               success = true,
               message = "SLAP berhasil!"
           }
       else
           -- Fail! Ambil 1 kartu dari deck sebagai penalti
           local penaltyCard, _ = DeckManager.drawFromDeck(gameDeck)
           if penaltyCard then
               table.insert(hand, penaltyCard)
           end

           return true, {
               success = false,
               message = "SLAP gagal! +1 kartu penalty"
           }
       end
   end
   ```

**Deliverable:**

- Tombol KABUL yang fungsional
- Final turns setelah KABUL dipanggil
- Mekanik SLAP dengan validasi rank

**Kriteria Selesai:**

- [ ] KABUL bisa dipanggil di awal giliran
- [ ] Semua pemain dapat notifikasi KABUL
- [ ] SLAP match berhasil membuang kartu
- [ ] SLAP no-match memberikan penalty

---

### Task 3.6: Kalkulasi Skor dan End Game

**ID**: T3.6  
**Assignee**: Scripter (dibantu Tester)  
**Estimasi Waktu**: 5 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: US-014

**Deskripsi:**
Mengimplementasikan perhitungan skor akhir dan menentukan pemenang berdasarkan nilai kartu terendah.

**Step-by-Step Detail:**

1. **Buat ScoreManager:**

   ```lua
   local ScoreManager = {}

   -- Nilai kartu berdasarkan costume
   local CARD_VALUES = {
       Joker = 0,
       A = 1,
       ["2"] = 2,
       ["3"] = 3,
       ["4"] = 4,
       ["5"] = 5,
       ["6"] = 6,
       ["7"] = 7,
       ["8"] = 8,
       ["9"] = 9,
       ["10"] = 10,
       J = 10,
       Q = 10,
       K = 10,
   }

   function ScoreManager.calculateHandValue(hand)
       local total = 0
       for _, card in ipairs(hand) do
           local value = CARD_VALUES[card.Rank] or 10
           total = total + value
       end
       return total
   end

   function ScoreManager.calculateFinalScores(playerHands)
       local scores = {}

       for playerId, hand in pairs(playerHands) do
           table.insert(scores, {
               playerId = playerId,
               playerName = game.Players:GetPlayerByUserId(playerId).Name,
               hand = hand,
               score = ScoreManager.calculateHandValue(hand)
           })
       end

       -- Sort by score (ascending)
       table.sort(scores, function(a, b)
           return a.score < b.score
       end)

       return scores
   end

   function ScoreManager.endGame()
       local scores = ScoreManager.calculateFinalScores(playerHands)

       -- Broadcast hasil
       ReplicatedStorage.GameEnded:FireAllClients({
           scores = scores,
           winner = scores[1]
       })

       -- Update phase
       TurnManager.setPhase("ENDED")

       -- Reveal semua kartu
       for playerId, hand in pairs(playerHands) do
           ReplicatedStorage.RevealAllCards:FireAllClients({
               playerId = playerId,
               hand = hand
           })
       end

       return scores
   end

   return ScoreManager
   ```

**Deliverable:**

- Perhitungan skor akurat
- Ranking pemain dari skor terendah
- Reveal semua kartu saat game end

**Kriteria Selesai:**

- [ ] Skor dihitung berdasarkan nilai kartu
- [ ] Ranking diurutkan dari terendah
- [ ] Semua kartu ter-reveal saat game end
- [ ] Winner ditandai dengan jelas

---

### Task 3.7: Testing End-to-End Sprint 3

**ID**: T3.7  
**Assignee**: Tester (semua tim ikut)  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: Semua US Sprint 3

**Deskripsi:**
Testing menyeluruh untuk seluruh game loop dari awal sampai akhir.

**Test Cases:**

1. **Test Case TC-009: Game Loop Lengkap**
   - Step: Main game dari start sampai end
   - Expected: Semua fase berjalan lancar
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

2. **Test Case TC-010: Ability Cards**
   - Step: Test masing-masing ability card
   - Expected: Ability berfungsi sesuai desain
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

3. **Test Case TC-011: KABUL Flow**
   - Step: Panggil KABUL, main final turns
   - Expected: Game end setelah final turns
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

4. **Test Case TC-012: Skor Calculation**
   - Step: End game, cek skor
   - Expected: Skor akurat, ranking benar
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

**Deliverable:**

- Laporan testing lengkap
- Video gameplay dari awal sampai akhir
- List bug critical yang harus di-fix

---

## Sprint 4: Multiplayer dan Polish

**Periode**: Minggu 7-8  
**Fokus**: Fitur multiplayer lengkap dan polish  
**Deliverable**: Game yang polished dan siap untuk soft launch

### Goals Sprint 4

1. Multiplayer berfungsi dengan baik (2-4 pemain)
2. Sound effects dan audio diimplementasi
3. Handle disconnect dan reconnect
4. UI polish dan feedback visual
5. Mobile optimization

### Motivasi Sprint 4

Ini adalah sprint terakhir. Di akhir Sprint 4, kalian akan punya game yang bisa dimainkan bersama teman-teman. Bayangkan pertama kali kalian main 4 pemain lengkap, saling berinteraksi, memanggil KABUL, dan merayakan kemenangan. Ini adalah momen di mana Kabul Roblox benar-benar hidup.

---

### Task 4.1: Implementasi RemoteEvents Lengkap

**ID**: T4.1  
**Assignee**: Scripter  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐⭐ Expert  
**User Story**: Semua multiplayer US

**Deskripsi:**
Menyusun semua RemoteEvents yang dibutuhkan untuk komunikasi Client-Server yang robust.

**Step-by-Step Detail:**

1. **Buat semua RemoteEvents:**

   ```lua
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   -- Events dari Client ke Server
   local clientToServer = {
       "CreateRoom",
       "JoinRoom",
       "LeaveRoom",
       "StartGame",
       "DrawCard",
       "SwapCard",
       "DiscardCard",
       "CallKabul",
       "SlapCard",
       "UseAbility",
       "SelectTarget",
       "ConfirmAction",
   }

   -- Events dari Server ke Client
   local serverToClient = {
       "RoomCreated",
       "RoomJoined",
       "RoomLeft",
       "GameStarted",
       "TurnChanged",
       "PhaseChanged",
       "CardDrawn",
       "CardSwapped",
       "CardDiscarded",
       "KabulCalled",
       "AbilityActivated",
       "AbilityReveal",
       "SlapResult",
       "GameEnded",
       "ErrorMessage",
   }

   -- Buat semua events
   for _, eventName in ipairs(clientToServer) do
       local event = Instance.new("RemoteEvent")
       event.Name = eventName
       event.Parent = ReplicatedStorage
   end

   for _, eventName in ipairs(serverToClient) do
       local event = Instance.new("RemoteEvent")
       event.Name = eventName
       event.Parent = ReplicatedStorage
   end
   ```

2. **Setup handlers di server:**
   ```lua
   -- Main Event Handler
   local function setupEventHandlers()
       ReplicatedStorage.DrawCard.OnServerEvent:Connect(function(player, source)
           local success, result = ActionHandler.handleDraw(player.UserId, source)
           if success then
               ReplicatedStorage.CardDrawn:FireClient(player, result)
           else
               ReplicatedStorage.ErrorMessage:FireClient(player, result)
           end
       end)

       -- Setup handlers untuk semua events lainnya...
   end
   ```

**Deliverable:**

- Semua RemoteEvents tersedia
- Handler untuk setiap event
- Error handling yang proper

**Kriteria Selesai:**

- [ ] Semua RemoteEvents dibuat
- [ ] Server handlers ter-setup
- [ ] Error handling bekerja
- [ ] No memory leaks dari events

---

### Task 4.2: Implementasi Sound Effects

**ID**: T4.2  
**Assignee**: Scripter (dibantu Designer)  
**Estimasi Waktu**: 4 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: Semua US (enhancement)

**Deskripsi:**
Menambahkan sound effects untuk aksi-aksi penting: draw card, swap, discard, KABUL, slap, dan lainnya.

**Step-by-Step Detail:**

1. **Import audio assets:**
   - ReplicatedStorage/Audio/ (Folder)
   - Upload atau cari audio dari Roblox Toolbox
   - Sounds yang dibutuhkan:
     - CardDraw
     - CardSwap
     - CardDiscard
     - KabulCall
     - SlapSuccess
     - SlapFail
     - TurnNotification
     - GameWin
     - GameLose

2. **Buat AudioManager:**

   ```lua
   local AudioManager = {}
   local ReplicatedStorage = game:GetService("ReplicatedStorage")
   local SoundService = game:GetService("SoundService")

   local sounds = {}

   function AudioManager.init()
       local audioFolder = ReplicatedStorage:WaitForChild("Audio")

       for _, sound in ipairs(audioFolder:GetChildren()) do
           if sound:IsA("Sound") then
               sounds[sound.Name] = sound
           end
       end
   end

   function AudioManager.playSound(soundName, parent)
       local sound = sounds[soundName]
       if not sound then
           warn("Sound not found: " .. soundName)
           return
       end

       local clone = sound:Clone()
       clone.Parent = parent or SoundService
       clone:Play()

       -- Destroy setelah selesai
       clone.Ended:Connect(function()
           clone:Destroy()
       end)
   end

   function AudioManager.playForPlayer(player, soundName)
       local sound = sounds[soundName]
       if sound then
           ReplicatedStorage.PlaySound:FireClient(player, soundName)
       end
   end

   function AudioManager.playForAll(soundName)
       local sound = sounds[soundName]
       if sound then
           ReplicatedStorage.PlaySound:FireAllClients(soundName)
       end
   end

   return AudioManager
   ```

3. **Integrasi dengan actions:**

   ```lua
   -- Saat draw card
   AudioManager.playForPlayer(player, "CardDraw")

   -- Saat KABUL dipanggil
   AudioManager.playForAll("KabulCall")

   -- Saat slap
   AudioManager.playForAll(slapSuccess and "SlapSuccess" or "SlapFail")
   ```

**Deliverable:**

- Semua sound effects tersedia
- AudioManager berfungsi
- Sounds dipicu di aksi yang tepat

**Kriteria Selesai:**

- [ ] Semua sounds import ke game
- [ ] AudioManager bisa play sounds
- [ ] Sounds dipicu saat aksi terjadi
- [ ] Volume levels seimbang

---

### Task 4.3: Handle Disconnect dan Reconnect

**ID**: T4.3  
**Assignee**: Scripter  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐⭐ Expert  
**User Story**: US-014, US-015 (enhancement)

**Deskripsi:**
Mengimplementasikan penanganan disconnect dengan grace period dan penentuan pemenang otomatis jika pemain tersisa hanya satu.

**Step-by-Step Detail:**

1. **Handle PlayerRemoving:**

   ```lua
   local Players = game:GetService("Players")
   local ReplicatedStorage = game:GetService("ReplicatedStorage")

   -- Track disconnected players
   local disconnectedPlayers = {}
   local DISCONNECT_GRACE_PERIOD = 60 -- detik

   Players.PlayerRemoving:Connect(function(player)
       local state = TurnManager.getState()

       -- Cek apakah player sedang di dalam game
       if state.phase ~= "WAITING" and state.phase ~= "ENDED" then
           -- Tandai sebagai disconnected
           disconnectedPlayers[player.UserId] = {
               disconnectTime = tick(),
               hand = playerHands[player.UserId],
           }

           -- Cek sisa pemain
           local activePlayers = 0
           for _, p in ipairs(Players:GetPlayers()) do
               if not disconnectedPlayers[p.UserId] then
                   activePlayers = activePlayers + 1
               end
           end

           -- Kalau tinggal 1 pemain, dia menang
           if activePlayers == 1 then
               -- Cari pemain yang tersisa
               for _, p in ipairs(Players:GetPlayers()) do
                   if not disconnectedPlayers[p.UserId] then
                       ScoreManager.endGame()
                       break
                   end
               end
           else
               -- Skip turn pemain yang disconnect
               if state.currentTurn == player.UserId then
                   TurnManager.nextTurn()
               end
           end
       end
   end)
   ```

2. **Handle reconnect (opsional untuk MVP):**
   ```lua
   Players.PlayerAdded:Connect(function(player)
       -- Cek apakah player sebelumnya disconnect
       if disconnectedPlayers[player.UserId] then
           local disconnectData = disconnectedPlayers[player.UserId]
           local timeSinceDisconnect = tick() - disconnectData.disconnectTime

           if timeSinceDisconnect <= DISCONNECT_GRACE_PERIOD then
               -- Reconnect berhasil, restore data
               playerHands[player.UserId] = disconnectData.hand
               disconnectedPlayers[player.UserId] = nil

               -- Notify client
               ReplicatedStorage.PlayerReconnected:FireClient(player)
           end
       end
   end)
   ```

**Deliverable:**

- Disconnect handling berfungsi
- Grace period 60 detik
- Auto-win jika tersisa 1 pemain

**Kriteria Selesai:**

- [ ] Disconnect terdeteksi
- [ ] Data pemain disimpan selama grace period
- [ ] Turn skip jika pemain disconnect di gilirannya
- [ ] Game end jika tersisa 1 pemain

---

### Task 4.4: UI Polish dan Feedback Visual

**ID**: T4.4  
**Assignee**: Designer (dibantu Scripter)  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐ Menengah  
**User Story**: Semua US (enhancement)

**Deskripsi:**
Menambahkan polish visual: hover effects, glow saat giliran, notifikasi toast, dan animasi transisi.

**Step-by-Step Detail:**

1. **Hover effects untuk kartu:**

   ```lua
   -- Saat mouse hover di kartu
   card.MouseEnter:Connect(function()
       TweenService:Create(card, TweenInfo.new(0.2), {
           Position = card.Position + Vector3.new(0, 0.2, 0)
       }):Play()
   end)

   card.MouseLeave:Connect(function()
       TweenService:Create(card, TweenInfo.new(0.2), {
           Position = originalPosition
       }):Play()
   end)
   ```

2. **Glow effect saat giliran:**
   - Buat Highlight object untuk pemain yang sedang giliran
   - Highlight.FillColor = Color3.fromRGB(0, 255, 0)
   - Highlight.OutlineColor = Color3.fromRGB(0, 200, 0)

3. **Toast notifications:**
   - StarterGui/ToastUI (ScreenGui)
   - Frame untuk menampilkan pesan singkat
   - Auto-hide setelah 3 detik

4. **Turn indicator:**
   - UI besar yang menampilkan "YOUR TURN!"
   - Muncul saat giliran pemain
   - Hilang setelah aksi pertama

**Deliverable:**

- Hover effects di kartu
- Glow saat giliran
- Toast notifications
- Turn indicator

**Kriteria Selesai:**

- [ ] Kartu naik saat di-hover
- [ ] Highlight saat giliran
- [ ] Toast muncul untuk notifikasi
- [ ] Turn indicator jelas dan eye-catching

---

### Task 4.5: Mobile Optimization

**ID**: T4.5  
**Assignee**: Designer (dibantu Scripter)  
**Estimasi Waktu**: 6 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: Semua US (platform support)

**Deskripsi:**
Mengoptimasi game agar nyaman dimainkan di perangkat mobile dengan touch interface.

**Step-by-Step Detail:**

1. **Touch targets yang lebih besar:**
   - Kartu: minimal 100x150 pixels
   - Tombol: minimal 80x80 pixels
   - Spacing antar elemen: minimal 10 pixels

2. **Mobile UI Layout:**

   ```lua
   -- Deteksi mobile
   local UserInputService = game:GetService("UserInputService")
   local isMobile = UserInputService.TouchEnabled

   if isMobile then
       -- Scale UI lebih besar
       HandContainer.Size = UDim2.new(0.9, 0, 0.3, 0)
       -- Dan seterusnya...
   end
   ```

3. **Touch gestures:**
   - Tap untuk klik
   - Long press untuk SLAP
   - Swipe untuk navigasi (jika ada)

4. **Performance optimization:**
   - StreamingEnabled = true
   - Texture size maksimal 512x512
   - Part count dikurangi di mobile

**Deliverable:**

- UI responsif di mobile
- Touch targets yang nyaman
- Performance optimal di mobile

**Kriteria Selesai:**

- [ ] UI scalable di mobile
- [ ] Touch targets cukup besar
- [ ] Long press untuk SLAP berfungsi
- [ ] Frame rate stabil di mobile

---

### Task 4.6: Final Testing dan Bug Fix

**ID**: T4.6  
**Assignee**: Tester (semua tim ikut)  
**Estimasi Waktu**: 8 jam  
**Skill Level**: ⭐⭐⭐ Lanjutan  
**User Story**: Semua US

**Deskripsi:**
Testing menyeluruh final untuk seluruh game, termasuk stress test dengan 4 pemain dan berbagai edge cases.

**Test Cases Final:**

1. **Test Case TC-013: 4 Players Full Game**
   - Step: Main dengan 4 pemain lengkap
   - Expected: Game berjalan lancar tanpa lag
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

2. **Test Case TC-014: Mobile Testing**
   - Step: Test di perangkat mobile
   - Expected: Game playable dengan nyaman
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

3. **Test Case TC-015: Disconnect Testing**
   - Step: Disconnect pemain saat game berlangsung
   - Expected: Game handle dengan baik
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

4. **Test Case TC-016: Edge Cases**
   - Deck habis saat draw
   - Slap berulang kali
   - Multiple ability dalam satu game
   - Actual: [diisi saat testing]
   - Status: [PASS/FAIL]

**Deliverable:**

- Laporan testing final
- List semua bug yang di-fix
- Video showcase game

**Kriteria Selesai:**

- [ ] Semua P0 user stories berfungsi
- [ ] Game playable dengan 4 pemain
- [ ] Tidak ada bug critical
- [ ] Mobile testing passed

---

## Ringkasan dan Statistik

### Total Tasks

| Sprint                           | Jumlah Tasks | Estimasi Total Waktu |
| -------------------------------- | ------------ | -------------------- |
| Sprint 1: Setup dan Dasar        | 8 tasks      | 39 jam               |
| Sprint 2: Kartu dan Deck         | 6 tasks      | 33 jam               |
| Sprint 3: Game Logic             | 7 tasks      | 49 jam               |
| Sprint 4: Multiplayer dan Polish | 6 tasks      | 38 jam               |
| **TOTAL**                        | **27 tasks** | **159 jam**          |

### Distribusi Task per Peran

| Peran    | Sprint 1 | Sprint 2 | Sprint 3 | Sprint 4 | Total |
| -------- | -------- | -------- | -------- | -------- | ----- |
| Builder  | 3        | 2        | 0        | 0        | 5     |
| Scripter | 2        | 2        | 5        | 4        | 13    |
| Designer | 1        | 2        | 1        | 2        | 6     |
| Tester   | 1        | 1        | 1        | 1        | 4     |

### Skill Level Distribution

| Level           | Jumlah Tasks | Persentase |
| --------------- | ------------ | ---------- |
| ⭐ Pemula       | 10 tasks     | 37%        |
| ⭐⭐ Menengah   | 11 tasks     | 41%        |
| ⭐⭐⭐ Lanjutan | 5 tasks      | 18%        |
| ⭐⭐⭐⭐ Expert | 2 tasks      | 4%         |

### Timeline Summary

```
Minggu 1-2  [████████████████████] Sprint 1: Setup dan Dasar
            - Struktur folder
            - Environment 3D
            - Room create/join
            - Quick win: Room berfungsi!

Minggu 3-4  [████████████████████] Sprint 2: Kartu dan Deck
            - Kartu 3D template
            - Deck system
            - Animasi kartu
            - Quick win: Kartu muncul di meja!

Minggu 5-6  [████████████████████] Sprint 3: Game Logic
            - Turn management
            - Draw/swap/discard
            - Abilities
            - KABUL dan SLAP
            - Quick win: Game playable!

Minggu 7-8  [████████████████████] Sprint 4: Multiplayer dan Polish
            - RemoteEvents
            - Sound effects
            - Handle disconnect
            - UI polish
            - Mobile optimization
            - Quick win: Game polished!
```

### Pencapaian Milestone

#### Akhir Sprint 1

- [x] Room bisa dibuat dan di-join
- [x] Environment 3D lengkap (meja, kursi, pencahayaan)
- [x] Tim confident dengan Roblox Studio

#### Akhir Sprint 2

- [x] Kartu 3D berfungsi
- [x] Deck 54 kartu lengkap
- [x] Animasi draw/swap/discard smooth
- [x] UI hand untuk pemain

#### Akhir Sprint 3

- [x] Game loop lengkap berfungsi
- [x] Semua ability cards bekerja
- [x] KABUL dan SLAP diimplementasi
- [x] Skor calculation akurat

#### Akhir Sprint 4

- [x] Multiplayer 4 pemain lancar
- [x] Sound effects lengkap
- [x] Handle disconnect robust
- [x] UI polished dan responsif
- [x] Mobile optimized

### Tips untuk Tim

1. **Komunikasi adalah Kunci**
   - Gunakan Discord atau platform serupa untuk komunikasi harian
   - Daily standup 15 menit untuk sync progress
   - Jangan ragu bertanya jika stuck

2. **Version Control**
   - Simpan backup regular dari place Roblox
   - Dokumentasikan perubahan besar
   - Publish ke Roblox setiap akhir sprint

3. **Learning Resources**
   - Roblox Developer Hub: https://create.roblox.com/docs
   - Roblox Developer Forum untuk tanya jawab
   - YouTube tutorials untuk teknik spesifik

4. **Jaga Semangat**
   - Rayakan setiap quick win!
   - Main game yang sudah dibuat secara regular
   - Ingat: setiap bug yang di-fix adalah pembelajaran

---

## Kesimpulan

Dokumen ini adalah peta jalan lengkap 8 minggu untuk membangun Kabul di Roblox. Dengan 27 task yang terstruktur rapi, tim pemula sekalipun bisa mengikuti dan melihat hasil nyata setiap 2 minggu.

### Key Success Factors

1. **Quick Wins**: Setiap sprint punya deliverable yang bisa dilihat dan dirasakan
2. **Skill Progression**: Dari pemula sampai expert, setiap anggota ada task yang sesuai
3. **Clear Ownership**: Setiap task punya assignee yang jelas
4. **Measurable Progress**: Time estimates dan acceptance criteria yang jelas

### Next Steps

1. **Tim Meeting**: Diskusikan peran masing-masing anggota tim
2. **Setup Environment**: Install Roblox Studio dan buat group place
3. **Start Sprint 1**: Mulai dengan Task 1.1
4. **Celebrate**: Rayakan setiap milestone yang tercapai!

Selamat membangun Kabul Roblox. Ingat: setiap baris kode yang kalian tulis membawa game ini lebih dekat ke tangan pemain. Have fun dan happy coding!

---

_Dokumen ini dibuat untuk tim pengembangan Kabul Roblox. Update sesuai progress aktual tim._
