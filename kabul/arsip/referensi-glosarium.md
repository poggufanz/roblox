# Referensi dan Glosarium Kabul Card Game

## Daftar Isi

1. [Glosarium Istilah Roblox](#glosarium-istilah-roblox)
2. [Glosarium Istilah Game Kabul](#glosarium-istilah-game-kabul)
3. [Glosarium Istilah Bisnis](#glosarium-istilah-bisnis)
4. [Link Referensi Resmi Roblox](#link-referensi-resmi-roblox)
5. [Sumber Asset Gratis](#sumber-asset-gratis)
6. [Troubleshooting untuk Pemula](#troubleshooting-untuk-pemula)
7. [Sumber Belajar](#sumber-belajar)

---

## Glosarium Istilah Roblox

### A

**Anti-Cheat**
Sistem yang mencegah pemain curang. Di Roblox, ini dilakukan dengan menempatkan semua logika penting di server. Client hanya mengirim input, server yang menentukan hasilnya.

### B

**BillboardGui**
Elemen UI yang menempel pada objek 3D dan selalu menghadap ke kamera. Berguna untuk menampilkan nama pemain, skor, atau status di atas karakter.

### C

**ClickDetector**
Komponen yang membuat objek 3D bisa diklik oleh pemain. Saat pemain mengklik objek, event tertentu akan ter-trigger.

**Client**
Game yang berjalan di perangkat pemain (PC, HP, tablet). Client bertanggung jawab menampilkan gambar, suara, dan menerima input dari pemain.

**Concurrent**
Jumlah pemain yang online dan bermain di waktu yang sama. Contoh: "Game ini punya 500 concurrent players" artinya ada 500 pemain sedang main bersamaan.

### D

**DataStoreService**
Sistem penyimpanan data permanen di Roblox. Berguna untuk menyimpan statistik pemain, achievement, atau pengaturan yang harus ingat meski pemain sudah keluar dari game.

**Decal**
Gambar 2D yang bisa ditempelkan pada permukaan objek 3D. Biasanya digunakan untuk tekstur, poster, atau gambar pada dinding.

**Developer Product**
Item yang bisa dibeli pemain berulang kali menggunakan Robux. Cocok untuk currency, power-up, atau item konsumabel.

**DevEx (Developer Exchange)**
Program penukaran Robux menjadi uang nyata (dalam USD). Developer bisa mencairkan Robux yang dihasilkan dari game mereka.

### F

**Frame**
Elemen UI dasar di Roblox yang berfungsi sebagai wadah untuk elemen lain seperti tombol, teks, atau gambar. Mirip div di HTML.

**FireServer()**
Fungsi untuk mengirim data dari client ke server melalui RemoteEvent. Client memanggil ini untuk memberitahu server apa yang pemain lakukan.

**FireAllClients()**
Fungsi untuk mengirim data dari server ke semua client yang terhubung. Berguna untuk memperbarui status game agar semua pemain melihat perubahan yang sama.

**FireClient()**
Fungsi untuk mengirim data dari server ke satu client tertentu. Berguna untuk mengirim data privat seperti kartu di tangan pemain.

### G

**Game Pass**
Produk satu kali beli yang memberikan fitur atau akses permanen. Contoh: VIP access, skin eksklusif, atau fitur premium.

**GameState**
Kondisi atau status game saat ini. Berisi informasi seperti fase game (WAITING, PLAYING, ENDED), giliran siapa, skor, dan data penting lainnya.

### H

**Hand**
Kartu-kartu yang dimiliki oleh pemain di tangan mereka. Dalam Kabul, setiap pemain punya 4 kartu di hand.

### I

**ImageLabel**
Elemen UI untuk menampilkan gambar. Bisa digunakan untuk menampilkan kartu, ikon, atau gambar lainnya di interface.

### L

**Lighting**
Service di Roblox yang mengatur pencahayaan dalam game. Termasuk pengaturan waktu (siang/malam), jenis cahaya, dan efek atmosfer.

**LocalScript**
Script yang berjalan di client (perangkat pemain). Digunakan untuk UI, input handling, dan efek visual yang hanya perlu dilihat oleh pemain tersebut.

### M

**MAU (Monthly Active Users)**
Jumlah pemain unik yang bermain dalam satu bulan. Metrik penting untuk mengukur popularitas game.

**Matchmaking**
Sistem yang mencarikan lawan atau tim untuk pemain. Di Roblox, ini ditangani secara otomatis oleh platform.

**MeshPart**
Objek 3D dengan bentuk kompleks yang dibuat menggunakan model 3D dari luar Roblox. Lebih detail daripada Part biasa.

**ModuleScript**
Script yang berisi fungsi atau data yang bisa dipakai oleh script lain. Mirip library atau helper file. Tidak berjalan sendiri, tapi dipanggil oleh Script atau LocalScript.

### O

**OnClientEvent**
Event yang ter-trigger saat client menerima data dari server. Client menggunakan ini untuk menangani update yang dikirim server.

**OnServerEvent**
Event yang ter-trigger saat server menerima data dari client. Server menggunakan ini untuk menangani aksi yang dilakukan pemain.

### P

**Part**
Blok dasar pembangun dunia di Roblox. Bisa berbentuk kotak, bola, silinder, atau papan. Semua objek 3D di Roblox terbuat dari Part atau MeshPart.

**Player**
Representasi pemain di dalam game. Berisi informasi seperti nama, ID, karakter (avatar), dan data spesifik pemain tersebut.

**ProximityPrompt**
Elemen interaksi yang muncul saat pemain mendekati objek tertentu. Menampilkan tombol dengan teks seperti "Ambil", "Buka", atau "Gunakan".

### R

**RemoteEvent**
Jembatan komunikasi antara client dan server. Memungkinkan client mengirim pesan ke server dan sebaliknya. Ini fondasi utama multiplayer di Roblox.

**ReplicatedStorage**
Tempat penyimpanan yang terlihat oleh client dan server. Bagian dari isi ReplicatedStorage bisa dibaca oleh client, tapi hanya server yang bisa menulis (mengubah) data di sini.

**Robux**
Mata uang virtual Roblox. Pemain membeli Robux dengan uang nyata, lalu bisa memakainya untuk membeli item di game. Developer menerima Robux dari pembelian dan bisa menukarnya melalui DevEx.

**Room**
Ruang permainan terpisah di mana sekelompok pemain bermain bersama. Dalam konteks Kabul web, room adalah tempat 2-4 pemain bermain. Di Roblox, satu server otomatis menjadi satu room.

### S

**ScreenGui**
Elemen UI yang muncul di layar pemain. Berisi semua interface seperti tombol, teks, panel, dan elemen interaktif lainnya.

**Script**
Script yang berjalan di server. Tempat logika game, aturan, dan validasi keamanan diletakkan. Client tidak bisa melihat isi Script di ServerScriptService.

**Server**
Komputer Roblox yang menjalankan logika game. Server mengatur aturan, menyimpan data game, dan memastikan semua pemain melihat kondisi yang sama.

**ServerScriptService**
Folder khusus di Roblox untuk menyimpan Script server. Script di sini hanya berjalan di server dan tidak terlihat oleh client.

**SLAP**
Mekanik spesial di game Kabul. Pemain bisa menampar (slap) kartu dari tangan mereka ke discard pile jika rank kartu cocok dengan kartu teratas.

**Sound**
Objek untuk memutar audio. Bisa berupa efek suara (SFX) atau musik latar (BGM).

**SoundService**
Service yang mengelola semua audio dalam game. Bisa digunakan untuk mengatur volume global atau efek suara khusus.

**StarterGui**
Folder untuk menyimpan ScreenGui yang otomatis muncul saat pemain masuk game. UI dasar seperti HUD ditempatkan di sini.

**StarterPlayer**
Folder yang berisi script dan pengaturan default untuk pemain. Script di StarterPlayerScripts akan otomatis berjalan saat pemain masuk.

**SurfaceGui**
Elemen UI yang menempel pada permukaan objek 3D. Solusi sempurna untuk membuat kartu 3D yang bisa menampilkan gambar dan teks.

### T

**TextButton**
Tombol di UI yang bisa diklik dan berisi teks. Bisa diberi fungsi saat ditekan menggunakan event MouseButton1Click.

**TextChatService**
Service bawaan Roblox untuk sistem chat. Sudah termasuk fitur moderasi dan tidak perlu dibuat dari nol.

**TextLabel**
Elemen UI untuk menampilkan teks. Bisa digunakan untuk judul, instruksi, atau informasi apapun yang perlu ditampilkan dalam bentuk tulisan.

**TweenService**
Sistem animasi bawaan Roblox untuk membuat pergerakan halus. Bisa menganimasikan posisi, rotasi, ukuran, warna, dan properti lainnya secara smooth.

### W

**Workspace**
Dunia 3D tempat semua objek fisik berada. Apapun yang terlihat dalam game (meja, kartu, avatar pemain) ada di Workspace.

---

## Glosarium Istilah Game Kabul

### A

**Ability**
Kemampuan khusus kartu yang aktif saat dibuang. Kartu 7 dan 8 punya ability PEEK_SELF, kartu 9 dan 10 punya PEEK_ENEMY, Jack punya BLIND_SWAP, Queen dan King punya SEE_AND_SWAP.

**Ace (A)**
Kartu dengan nilai 1. Salah satu kartu terbaik karena nilainya sangat rendah.

### B

**Black Kings (K♠, K♣)**
King Sekop dan King Keriting. Nilainya 13, yang merupakan nilai tertinggi (terburuk) dalam game.

**BLIND_SWAP**
Ability kartu Jack. Pemain menukar satu kartu dari tangan mereka dengan kartu lawan secara buta (tanpa melihat nilainya).

**Blue Ocean**
Istilah bisnis untuk pasar tanpa kompetisi. Kabul adalah Blue Ocean karena belum ada game kartu serupa di Roblox.

### C

**Card Count**
Jumlah kartu yang dimiliki pemain. Normalnya 4 kartu, tapi bisa berkurang jika berhasil SLAP atau bertambah jika gagal SLAP.

**Client State**
Status game yang dilihat oleh satu pemain tertentu. Client State berbeda untuk setiap pemain karena informasi sensitif (seperti kartu lawan) disembunyikan.

**Concurrent Players**
Jumlah pemain yang sedang online dan bermain di waktu yang sama.

**Costume**
Varian aturan nilai kartu. Costume 1 (Classic): Joker = -1, Red Kings = 0. Costume 2 (Reversed): Joker = 0, Red Kings = -1.

### D

**DAU (Daily Active Users)**
Jumlah pemain unik yang bermain dalam satu hari.

**Deck**
Tumpukan kartu yang belum dibagikan atau diambil. Di awal game, deck berisi 54 kartu.

**Discard Pile**
Tumpukan kartu yang sudah dibuang oleh pemain. Kartu teratas discard pile selalu terlihat oleh semua pemain.

**Draw**
Aksi mengambil kartu dari deck atau discard pile.

### E

**ENDED**
Fase game saat permainan sudah selesai. Semua kartu terbuka dan skor dihitung untuk menentukan pemenang.

### F

**Face-down**
Kondisi kartu tertutup (bagian belakang terlihat). Pemain tidak bisa melihat nilai kartu yang face-down.

**Face-up**
Kondisi kartu terbuka (nilai terlihat). Kartu di discard pile selalu face-up.

**Final Turns**
Giliran terakhir yang diberikan kepada pemain lain setelah seseorang memanggil KABUL. Setiap pemain (kecuali yang memanggil KABUL) mendapat satu giliran terakhir.

### G

**Game Loop**
Siklus bermain yang berulang. Di Kabul: DRAW → ACTION → ABILITY (jika ada) → NEXT TURN.

### H

**Hand**
Kartu-kartu yang dimiliki pemain. Di Kabul, setiap pemain memiliki 4 kartu di hand yang disusun dalam format 2x2.

**Host**
Pemain yang membuat room dan punya hak khusus seperti memulai game.

### J

**Joker**
Kartu spesial tanpa suit. Nilainya tergantung costume: -1 di Classic, 0 di Reversed.

### K

**KABUL**
Aksi spesial untuk mengakhiri game. Pemain yang memanggil KABUL yakin mereka punya skor terendah. Setelah KABUL dipanggil, pemain lain mendapat satu giliran terakhir sebelum game berakhir.

### L

**Lobby**
Area tunggu sebelum game dimulai. Pemain bisa membuat room baru atau join room yang sudah ada.

### M

**Masking**
Proses menyembunyikan informasi sensitif. Contoh: kartu lawan ditampilkan sebagai "tertutup" meski server tahu nilai sebenarnya.

**MEMORIZE**
Fase awal game saat pemain diberi waktu 3 detik untuk melihat 2 kartu bawah mereka sebelum kartu tertutup.

### P

**PEEK_ENEMY**
Ability kartu 9 dan 10. Pemain bisa melihat salah satu kartu milik lawan selama 3 detik. Lawan tidak tahu kartunya diintip.

**PEEK_SELF**
Ability kartu 7 dan 8. Pemain bisa melihat salah satu kartu milik mereka sendiri selama 3 detik.

**PLAYING**
Fase utama permainan saat pemain bergiliran mengambil dan membuang kartu.

### Q

**Queen (Q)**
Kartu dengan nilai 12 (tinggi). Punya ability SEE_AND_SWAP saat dibuang.

### R

**Rank**
Angka atau huruf pada kartu: A, 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K.

**Red Kings (K♥, K♦)**
King Hati dan King Wajik. Nilainya -1 (terbaik) di Costume Classic, atau 0 di Costume Reversed.

**Retention**
Persentase pemain yang kembali bermain setelah main pertama kali. Retention tinggi berarti pemain suka game dan kembali lagi.

**Room**
Ruang permainan tempat 2-4 pemain bermain bersama. Setiap room punya game state sendiri.

### S

**Score**
Total nilai kartu di tangan pemain. Pemenang adalah pemain dengan skor terendah.

**SEE_AND_SWAP**
Ability kartu Queen dan King. Pemain memilih satu kartu miliknya dan satu kartu lawan, kedua kartu terbuka, lalu pemain bisa memutuskan apakah akan menukar atau tidak.

**Server-Authoritative**
Arsitektur di mana server memiliki otoritas penuh. Semua validasi dan logika penting dijalankan di server, bukan di client.

**SLAP**
Mekanik spesial di mana pemain bisa membuang kartu dari tangan langsung ke discard pile tanpa menunggu giliran, asalkan rank kartu cocok dengan top discard.

**Suit**
Jenis kartu: Sekop (♠), Hati (♥), Wajik (♦), Keriting (♣).

**Swap**
Aksi menukar kartu yang baru diambil dengan salah satu kartu di tangan.

### T

**Top Discard**
Kartu teratas di discard pile. Kartu ini selalu terlihat oleh semua pemain.

**Turn**
Giliran pemain untuk melakukan aksi. Setiap turn terdiri dari fase DRAWING dan DISCARDING.

**Turn Phase**
Fase dalam satu giliran: DRAWING (saat pemain harus ambil kartu), DISCARDING (saat pemain harus buang kartu), RESOLVING_ABILITY (saat ability kartu diproses).

### V

**Value**
Nilai numerik kartu. Ace = 1, 2-10 = face value, Jack = 11, Queen = 12, King = 13 (atau -1 untuk Red Kings di Classic).

### W

**WAITING**
Fase awal saat room baru dibuat dan menunggu pemain lain join sebelum game bisa dimulai.

**Winner**
Pemain dengan skor terendah saat game berakhir.

---

## Glosarium Istilah Bisnis

**ARPU (Average Revenue Per User)**
Rata-rata pendapatan yang dihasilkan dari setiap pemain. Dihitung dari total revenue dibagi jumlah pemain.

**Blue Ocean**
Strategi bisnis yang fokus pada pasar tanpa kompetisi, bukan bersaing di pasar yang sudah ramai (Red Ocean). Kabul adalah Blue Ocean karena belum ada card game serupa di Roblox.

**Conversion Rate**
Persentase pemain yang melakukan pembelian dari total pemain. Contoh: 1000 pemain, 50 yang beli = 5% conversion rate.

**Game Pass**
Produk sekali beli di Roblox yang memberikan fitur atau akses permanen. Developer mendapat 70% dari harga Game Pass.

**MAU (Monthly Active Users)**
Jumlah pemain unik yang bermain dalam satu bulan. Metrik penting untuk mengukur ukuran dan pertumbuhan game.

**Monetization**
Cara game menghasilkan uang. Di Roblox bisa melalui Game Pass, Developer Products, atau Premium Payouts.

**Premium Payouts**
Pembayaran otomatis dari Roblox ke developer berdasarkan waktu bermain pemain Roblox Premium. Tidak memerlukan pembelian aktif dari pemain.

**Retention**
Persentase pemain yang kembali bermain setelah sesi pertama. Day 1 Retention = pemain yang kembali hari berikutnya. Day 7 Retention = pemain yang kembali setelah seminggu.

**Robux**
Mata uang virtual Roblox. Pemain beli Robux pakai uang nyata, developer dapat Robux dari penjualan, lalu bisa ditukar jadi dollar via DevEx.

**Whales**
Istilah untuk pemain yang menghabiskan banyak uang di game. Mereka mungkin hanya 5-10% dari pemain tapi menghasilkan sebagian besar revenue.

---

## Link Referensi Resmi Roblox

### Dokumentasi Utama

| Topik                            | Link                                                                     |
| -------------------------------- | ------------------------------------------------------------------------ |
| **Roblox Creator Documentation** | https://create.roblox.com/docs                                           |
| **Server-Client Communication**  | https://create.roblox.com/docs/scripting/server-client-communication     |
| **RemoteEvents Guide**           | https://create.roblox.com/docs/scripting/events/remote-events            |
| **ReplicatedStorage**            | https://create.roblox.com/docs/scripting/data-storage/replicated-storage |
| **DataStoreService**             | https://create.roblox.com/docs/scripting/data-storage/data-stores        |
| **TweenService Animation**       | https://create.roblox.com/docs/scripting/animation/tweening              |
| **SurfaceGui**                   | https://create.roblox.com/docs/ui/surface-guis                           |
| **Security Best Practices**      | https://create.roblox.com/docs/scripting/security                        |
| **Player Events**                | https://create.roblox.com/docs/scripting/players/player-events           |
| **TextChatService**              | https://create.roblox.com/docs/chat                                      |

### API Reference

| Class                 | Link                                                                      |
| --------------------- | ------------------------------------------------------------------------- |
| **RemoteEvent**       | https://create.roblox.com/docs/reference/engine/classes/RemoteEvent       |
| **ReplicatedStorage** | https://create.roblox.com/docs/reference/engine/classes/ReplicatedStorage |
| **DataStoreService**  | https://create.roblox.com/docs/reference/engine/classes/DataStoreService  |
| **TweenService**      | https://create.roblox.com/docs/reference/engine/classes/TweenService      |
| **SurfaceGui**        | https://create.roblox.com/docs/reference/engine/classes/SurfaceGui        |
| **ScreenGui**         | https://create.roblox.com/docs/reference/engine/classes/ScreenGui         |
| **ProximityPrompt**   | https://create.roblox.com/docs/reference/engine/classes/ProximityPrompt   |
| **ClickDetector**     | https://create.roblox.com/docs/reference/engine/classes/ClickDetector     |

### Monetization

| Topik                  | Link                                                                  |
| ---------------------- | --------------------------------------------------------------------- |
| **Game Pass**          | https://create.roblox.com/docs/assist/monetization/game-passes        |
| **Developer Products** | https://create.roblox.com/docs/assist/monetization/developer-products |
| **Premium Payouts**    | https://create.roblox.com/docs/assist/monetization/premium-payouts    |
| **DevEx Program**      | https://developer.roblox.com/en-us/devex                              |

---

## Sumber Asset Gratis

### Roblox Creator Store

Creator Store adalah marketplace resmi Roblox tempat developer bisa menemukan asset gratis dan berbayar.

**Link:** https://www.roblox.com/catalog?Category=9

**Kategori Asset Gratis:**

| Kategori    | Gunakan Untuk                 | Contoh Pencarian                               |
| ----------- | ----------------------------- | ---------------------------------------------- |
| **Decals**  | Gambar kartu, tekstur meja    | "playing cards", "poker table", "card pattern" |
| **Meshes**  | Model kartu 3D, meja, kursi   | "table", "chair", "card"                       |
| **Audio**   | Efek suara kartu, musik latar | "card flip", "card shuffle", "casino"          |
| **Plugins** | Alat bantu development        | "building tools", "animation"                  |

**Tips Mencari Asset:**

1. Gunakan filter "Free" untuk hanya menampilkan asset gratis
2. Periksa rating dan jumlah penggunaan untuk memastikan kualitas
3. Baca deskripsi untuk memastikan lisensi sesuai kebutuhan
4. Download asset ke Inventory lalu import ke Studio

### Asset Buat Sendiri

**Untuk Kartu:**

Buat SurfaceGui dengan komponen:

- **RankLabel**: TextLabel untuk angka/huruf (A, K, Q, J, 10, dll)
- **SuitLabel**: TextLabel untuk simbol (♠ ♥ ♦ ♣)
- **CenterArt**: ImageLabel untuk gambar kartu

**Warna Standar Kartu:**

- Merah untuk Hati (♥) dan Wajik (♦)
- Hitam untuk Sekop (♠) dan Keriting (♣)
- Putih untuk background kartu
- Biru tua untuk pattern belakang kartu

**Ukuran Kartu:**

- Ukuran fisik di Roblox: 2 x 0.05 x 3 studs (lebar x tebal x tinggi)
- Canvas SurfaceGui: 200 x 300 pixels
- Font untuk rank: Bold, ukuran 24-32

---

## Troubleshooting untuk Pemula

### Masalah Umum dan Solusi

#### 1. Script Tidak Berjalan

**Gejala:** Script tidak melakukan apa-apa saat game dimainkan.

**Penyebab Umum:**

- Script diletakkan di tempat yang salah
- Nama script atau fungsi salah
- Error syntax di script

**Solusi:**

- Script server harus di ServerScriptService
- LocalScript harus di StarterGui atau StarterPlayerScripts
- Buka Output window (View → Output) untuk lihat error

#### 2. RemoteEvent Tidak Berfungsi

**Gejala:** Client mengirim data tapi server tidak merespons.

**Penyebab Umum:**

- RemoteEvent belum di-create di ReplicatedStorage
- Nama RemoteEvent berbeda antara client dan server
- Server belum connect ke OnServerEvent

**Solusi:**

```lua
-- Pastikan RemoteEvent ada di ReplicatedStorage
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local remoteEvent = ReplicatedStorage:WaitForChild("NamaRemoteEvent")

-- Di server
remoteEvent.OnServerEvent:Connect(function(player, data)
    -- Handle event
end)

-- Di client
remoteEvent:FireServer(data)
```

#### 3. Kartu Tidak Muncul di UI

**Gejala:** SurfaceGui tidak terlihat pada objek 3D.

**Penyebab Umum:**

- Adornee tidak di-set
- Face salah
- CanvasSize terlalu kecil

**Solusi:**

- Pastikan SurfaceGui.Adornee = partKartu
- Pastikan SurfaceGui.Face sesuai (Enum.NormalId.Top atau Front)
- Atur CanvasSize dengan benar (contoh: {200, 0}, {300, 0})

#### 4. Game State Tidak Sinkron

**Gejala:** Pemain melihat kondisi yang berbeda.

**Penyebab Umum:**

- Update hanya dikirim ke satu client
- Client mengubah data langsung tanpa melalui server
- Race condition saat multiple update

**Solusi:**

- Selalu gunakan FireAllClients() untuk update yang harus dilihat semua orang
- Jangan pernah ubah ReplicatedStorage dari client
- Server harus single source of truth

#### 5. DataStore Error

**Gejala:** Stats pemain tidak tersimpan.

**Penyebab Umum:**

- DataStore belum di-enable untuk game
- Rate limit terlampaui (60 req/menit)
- Key format salah

**Solusi:**

- Enable DataStore di Game Settings
- Gunakan pcall untuk handle error
- Simpan data hanya saat perlu (jangan tiap detik)

```lua
local DataStoreService = game:GetService("DataStoreService")
local statsStore = DataStoreService:GetDataStore("PlayerStats")

-- Selalu gunakan pcall
local success, result = pcall(function()
    return statsStore:GetAsync("Player_" .. player.UserId)
end)

if not success then
    warn("DataStore error:", result)
    return nil
end
```

#### 6. Lag atau Lagging

**Gejala:** Game patah-patah atau lambat.

**Penyebab Umum:**

- Terlalu banyak RemoteEvent calls
- Script berat berjalan tiap frame
- Terlalu banyak part tanpa optimization

**Solusi:**

- Batch multiple updates dalam satu RemoteEvent
- Gunakan Heartbeat/RenderStepped dengan bijak
- Enable StreamingEnabled untuk map besar
- Gunakan part sederhana, hindari union kompleks

#### 7. Mobile Compatibility

**Gejala:** Game tidak bisa dimainkan di HP.

**Penyebab Umum:**

- UI terlalu kecil untuk touch screen
- Belum ada touch input handling
- Control PC-only (keyboard/mouse)

**Solusi:**

- Buat UI versi mobile lebih besar
- Gunakan Touch events, bukan hanya Click
- Pertimbangkan ProximityPrompt untuk interaksi
- Test di berbagai ukuran layar

---

## Sumber Belajar

### YouTube Channels

**Bahasa Indonesia:**

| Channel                    | Konten                                              | Link                                       |
| -------------------------- | --------------------------------------------------- | ------------------------------------------ |
| **Roblox Indonesia**       | Tutorial dasar Roblox Studio dalam Bahasa Indonesia | Search: "Roblox Indonesia Tutorial"        |
| **TUTORIAL ROBLOX STUDIO** | Berbagai tutorial scripting dan building            | Search: "Tutorial Roblox Studio Indonesia" |

**Bahasa Inggris:**

| Channel              | Konten                                           | Link                                      |
| -------------------- | ------------------------------------------------ | ----------------------------------------- |
| **AlvinBlox**        | Scripting tutorial dari beginner sampai advanced | https://www.youtube.com/@AlvinBlox        |
| **TheDevKing**       | Roblox development lengkap                       | https://www.youtube.com/@TheDevKing       |
| **ScriptingWithRob** | Lua scripting untuk Roblox                       | https://www.youtube.com/@ScriptingWithRob |
| **PeasFactory**      | Game design dan scripting                        | https://www.youtube.com/@PeasFactory      |
| **Roblox Creator**   | Official Roblox channel                          | https://www.youtube.com/@Roblox           |

**Playlist Rekomendasi:**

- "Roblox Scripting Tutorial" by AlvinBlox (mulai dari nol)
- "How to Make a Roblox Game" by TheDevKing (complete guide)
- "Lua for Roblox" (bahasa pemrograman dasar)

### Forum dan Komunitas

| Platform                     | Gunakan Untuk                        | Link                                   |
| ---------------------------- | ------------------------------------ | -------------------------------------- |
| **DevForum Roblox**          | Tanya jawab teknis, update platform  | https://devforum.roblox.com            |
| **r/robloxgamedev**          | Diskusi komunitas Reddit             | https://www.reddit.com/r/robloxgamedev |
| **Roblox Developer Discord** | Chat real-time dengan developer lain | Search: "Roblox Developer Discord"     |
| **Scripting Helpers**        | Q&A scripting                        | https://scriptinghelpers.org           |

### Kursus Online

| Platform                         | Kursus                    | Link                           |
| -------------------------------- | ------------------------- | ------------------------------ |
| **Roblox Creator Documentation** | Dokumentasi resmi lengkap | https://create.roblox.com/docs |
| **Roblox Education**             | Tutorial terstruktur      | https://education.roblox.com   |

### Tools dan Resources

| Tool                           | Gunakan Untuk                      | Link                                      |
| ------------------------------ | ---------------------------------- | ----------------------------------------- |
| **Roblox Studio**              | IDE utama untuk bikin game         | Download dari roblox.com                  |
| **Roblox API Reference**       | Dokumentasi semua class dan method | https://create.roblox.com/docs/reference  |
| **Roblox Creator Marketplace** | Asset, plugin, dan tools           | https://www.roblox.com/catalog?Category=9 |
| **Hex Color Picker**           | Pilih warna untuk UI               | Search: "hex color picker"                |
| **Lua Learning**               | Belajar bahasa Lua                 | https://lualearning.com                   |

### Buku dan Ebook

**Gratis:**

- "Programming in Lua" (First Edition) - Available free online
- Roblox Creator Documentation - Lengkap dan gratis

### Tips Belajar untuk Pemula

1. **Mulai dari Tutorial Resmi**
   - Roblox Creator Documentation punya learning path terstruktur
   - Ikuti langkah demi langkah, jangan skip dasar

2. **Praktik Setiap Hari**
   - Scripting itu skill, perlu latihan rutin
   - Target: minimal 30 menit sehari

3. **Bikin Project Kecil Dulu**
   - Jangan langsung bikin game besar
   - Mulai dari: obby sederhana, clicker game, atau tycoon

4. **Join Komunitas**
   - Tanya di DevForum kalau stuck
   - Baca solusi orang lain

5. **Pelajari dari Game Lain**
   - Main game populer, amati mekaniknya
   - Pikirkan: "Bagaimana mereka bikin ini?"

6. **Simpan Snippet Code**
   - Buat library code yang sering dipakai
   - Contoh: sistem save/load, UI animation, dll

---

## Cheat Sheet Cepat

### Struktur Script Roblox

```
ServerScriptService/           -- Script server
├── GameManager.server.lua
├── CardManager.server.lua
└── Network/
    └── RemoteEvents/          -- Semua RemoteEvent di sini

StarterPlayerScripts/          -- LocalScript client
├── ClientController/
├── UIManager.local.lua
└── CameraManager.local.lua

ReplicatedStorage/             -- Shared storage
├── Shared/
│   ├── Config.lua             -- ModuleScript
│   └── Utils.lua
└── Assets/
    └── CardImages/

StarterGui/                    -- UI
└── MainUI/
    ├── ScreenGui
    └── Frame components
```

### Pattern Dasar RemoteEvent

```lua
-- 1. Create RemoteEvent di ReplicatedStorage
-- (Lakukan ini sekali di Studio)

-- 2. Server Script
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local myEvent = ReplicatedStorage:WaitForChild("MyRemoteEvent")

myEvent.OnServerEvent:Connect(function(player, data)
    -- Validasi
    if not data then return end

    -- Proses
    print(player.Name .. " mengirim: " .. tostring(data))

    -- Kirim response ke semua client
    myEvent:FireAllClients("Update untuk semua")
end)

-- 3. LocalScript (Client)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local myEvent = ReplicatedStorage:WaitForChild("MyRemoteEvent")

-- Kirim ke server
myEvent:FireServer("Hello Server!")

-- Terima dari server
myEvent.OnClientEvent:Connect(function(message)
    print("Dari server: " .. message)
end)
```

### Pattern DataStore

```lua
local DataStoreService = game:GetService("DataStoreService")
local statsStore = DataStoreService:GetDataStore("PlayerStats")

-- Load data
local function loadStats(player)
    local success, data = pcall(function()
        return statsStore:GetAsync("Player_" .. player.UserId)
    end)

    if success and data then
        return data
    else
        return { wins = 0, losses = 0 }
    end
end

-- Save data
local function saveStats(player, data)
    local success, err = pcall(function()
        statsStore:SetAsync("Player_" .. player.UserId, data)
    end)

    if not success then
        warn("Gagal simpan: " .. tostring(err))
    end
end
```

---

## Penutup

Dokumen ini berisi referensi lengkap untuk pengembangan Kabul Card Game di Roblox. Gunakan glosarium untuk memahami istilah, ikuti link referensi untuk dokumentasi detail, dan manfaatkan sumber belajar untuk meningkatkan skill.

**Ingat:**

- Server adalah boss. Semua validasi penting harus di server.
- Client hanya untuk tampilan dan input.
- Test sering, deploy pelan-pelan.
- Komunitas Roblox sangat membantu, jangan ragu bertanya.

Selamat membangun!

---

_Dokumen ini dibuat untuk tim Kabul Card Game - Februari 2026_
