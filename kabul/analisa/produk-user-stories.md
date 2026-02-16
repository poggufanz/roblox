# Dokumen User Stories Kabul Game

## Daftar Isi

1. [Ringkasan Produk](#ringkasan-produk)
2. [Persona Pengguna](#persona-pengguna)
3. [User Stories Phase 1 (MVP)](#user-stories-phase-1-mvp)
4. [User Stories Phase 2 (Enhancement)](#user-stories-phase-2-enhancement)
5. [Kriteria Prioritas](#kriteria-prioritas)

---

## Ringkasan Produk

Kabul adalah permainan kartu multiplayer untuk 2-4 pemain yang menggabungkan strategi, memori, dan keberuntungan. Pemain bersaing untuk mendapatkan total nilai kartu terendah. Game ini akan diporting dari platform web ke Roblox dengan pengalaman yang lebih interaktif dan immersive.

### Fitur Utama

- Sistem kartu lengkap dengan 54 kartu dan kemampuan spesial
- Mekanik KABUL untuk mengakhiri permainan
- Mekanik SLAP untuk aksi cepat
- Lobby dan sistem room
- Fitur sosial dan statistik pemain

---

## Persona Pengguna

### Persona 1: The Builder (Pemain Kasual)

**Profil:**

- Usia: 10-14 tahun
- Pengalaman: Baru main game kartu atau sudah familiar dengan Roblox
- Tujuan: Have fun dan main bareng teman-teman
- Perilaku: Lebih suka game yang gampang dipahami, gak suka ribet

**Quote:** _"Gue cuma mau main seru-seruan sama temen, gak perlu mikir terlalu keras."_

**Pain Points:**

- Takut kalo aturannya susah
- Gak suka nunggu lama
- Bingung kalo tutorialnya kepanjangan

**Needs:**

- Quick match making
- Tutorial singkat dan interaktif
- Feedback visual yang jelas

---

### Persona 2: The Scripter (Pemain Kompetitif)

**Profil:**

- Usia: 13-18 tahun
- Pengalaman: Sudah pernah main game kartu strategis, paham meta
- Tujuan: Menang dan naik leaderboard
- Perilaku: Suka analisis, cari strategi terbaik, aktif di komunitas

**Quote:** _"Gue harus tahu kapan waktu tepat buat panggil KABUL. Timing itu everything."_

**Pain Points:**

- Kesel kalo ada bug yang merusak strategi
- Butuh statistik detail buat improve
- Gak suka matchmaking yang gak balance

**Needs:**

- Statistik permainan detail
- Leaderboard dan ranking
- Match history
- Spectator mode buat belajar dari pemain lain

---

### Persona 3: The Designer (Pemain Sosial)

**Profil:**

- Usia: 12-16 tahun
- Pengalaman: Main Roblox buat hangout sama teman
- Tujuan: Socialize dan koleksi item keren
- Perilaku: Suka customisasi, aktif chat, ajak teman main

**Quote:** _"Yang penting bisa ngobrol sambil main. Kartu bagus-bagus juga sih plusnya."_

**Pain Points:**

- Gak suka kalo chatnya terbatas
- Butuh cara gampang invite teman
- Pingin avatar dan kartu yang bisa dicustom

**Needs:**

- Chat system yang lancar
- Emoji dan reaksi
- Card skins dan avatar items
- Friend system

---

## User Stories Phase 1 (MVP)

### US-001: Membuat Room Baru

**Sebagai** Builder (pemain kasual),
**Saya ingin** membuat room baru dengan mudah,
**Sehingga** saya bisa langsung main bareng teman-teman tanpa ribet.

#### Acceptance Criteria

**AC1:** Pemain dapat membuat room dengan satu klik tombol "Buat Room"
**AC2:** Room otomatis diberi nama unik (bisa diubah)
**AC3:** Room muncul di lobby list dalam 3 detik
**AC4:** Pemain yang membuat room otomatis jadi host
**AC5:** Room bisa di-set private (dengan password) atau public

#### Detail Implementasi

| Aspek      | Deskripsi                                                          |
| ---------- | ------------------------------------------------------------------ |
| Input      | Klik tombol "Buat Room"                                            |
| Output     | Room terbuat, pemain masuk ke waiting room                         |
| Error Case | Jika server penuh, tampilkan pesan "Server penuh, coba lagi nanti" |

---

### US-002: Join Room dari Lobby

**Sebagai** Builder (pemain kasual),
**Saya ingin** melihat daftar room yang tersedia dan join,
**Sehingga** saya bisa cepet main tanpa nunggu teman online.

#### Acceptance Criteria

**AC1:** Lobby menampilkan list room dengan info: nama room, jumlah pemain, status
**AC2:** Pemain bisa filter room berdasarkan status (waiting/playing/private)
**AC3:** Pemain bisa search room berdasarkan nama
**AC4:** Klik pada room menampilkan detail dan tombol join
**AC5:** Room yang sudah full (4/4) tidak bisa dijoin

#### Detail Implementasi

| Aspek      | Deskripsi                              |
| ---------- | -------------------------------------- |
| Input      | Pilih room dari list                   |
| Output     | Masuk ke waiting room atau pesan error |
| Error Case | Room full: "Room sudah penuh"          |

---

### US-003: Melihat Kartu Saat Fase Memorize

**Sebagai** Builder (pemain kasual),
**Saya ingin** melihat 2 kartu bawah saya saat game mulai,
**Sehingga** saya bisa ingat posisi kartu bagus atau jelek.

#### Acceptance Criteria

**AC1:** Saat game start, semua kartu tertutup dulu
**AC2:** Kartu posisi 0 dan 1 (baris bawah) terbuka selama 3 detik
**AC3:** Kartu posisi 2 dan 3 (baris atas) tetap tertutup
**AC4:** Timer countdown 3 detik ditampilkan besar-besar
**AC5:** Setelah 3 detik, semua kartu otomatis tertutup
**AC6:** Tidak ada interaksi lain yang bisa dilakukan selama fase ini

#### Detail Implementasi

| Aspek      | Deskripsi                        |
| ---------- | -------------------------------- |
| Input      | Game dimulai oleh host           |
| Output     | Kartu posisi 0-1 terbuka 3 detik |
| Error Case | -                                |

---

### US-004: Mengambil Kartu dari Deck

**Sebagai** Builder (pemain kasual),
**Saya ingin** mengambil kartu dari deck saat giliran saya,
**Sehingga** saya bisa dapet kartu baru buat ditukar atau dibuang.

#### Acceptance Criteria

**AC1:** Saat giliran, deck highlight atau glow
**AC2:** Klik pada deck mengambil 1 kartu acak
**AC3:** Kartu yang diambil ditampilkan di tengah layar (hanya pemain yang ambil)
**AC4:** Sound effect "card-draw" diputar saat mengambil
**AC5:** Giliran berpindah ke fase berikutnya (discard)
**AC6:** Deck count berkurang 1

#### Detail Implementasi

| Aspek      | Deskripsi                                  |
| ---------- | ------------------------------------------ |
| Input      | Klik deck saat giliran                     |
| Output     | 1 kartu random masuk ke tangan (temporary) |
| Error Case | Deck habis: acak ulang discard pile        |

---

### US-005: Mengambil Kartu dari Discard Pile

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** ngambil kartu terbuka dari discard pile,
**Sehingga** saya bisa dapet kartu yang saya tahu nilainya, bukan tebak-tebakan.

#### Acceptance Criteria

**AC1:** Discard pile menunjukkan kartu teratas (face-up)
**AC2:** Klik pada discard pile mengambil kartu teratas
**AC3:** Kartu yang diambil masuk ke tangan temporary
**AC4:** Discard pile update menunjukkan kartu berikutnya
**AC5:** Tidak bisa ngambil kalo discard pile kosong

#### Detail Implementasi

| Aspek      | Deskripsi                         |
| ---------- | --------------------------------- |
| Input      | Klik discard pile                 |
| Output     | Kartu teratas masuk ke tangan     |
| Error Case | Discard kosong: disable interaksi |

---

### US-006: Menukar Kartu dengan Tangan

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** nuker kartu yang baru saya ambil dengan kartu di tangan saya,
**Sehingga** saya bisa optimize nilai total kartu saya.

#### Acceptance Criteria

**AC1:** Setelah draw, pemain bisa pilih salah satu kartu di tangan untuk ditukar
**AC2:** Klik pada kartu tangan menukar posisi dengan kartu temporary
**AC3:** Kartu yang ditukar masuk ke discard pile
**AC4:** Kartu di tangan update menunjukkan kartu baru
**AC5:** Sound effect "card-swap" diputar
**AC6:** Jika kartu yang dibuang punya ability, ability aktif

#### Detail Implementasi

| Aspek      | Deskripsi                            |
| ---------- | ------------------------------------ |
| Input      | Klik kartu di tangan setelah draw    |
| Output     | Kartu bertukar, yang lama ke discard |
| Error Case | -                                    |

---

### US-007: Membuang Kartu Tanpa Swap

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** langsung buang kartu yang baru saya ambil tanpa ditukar,
**Sehingga** saya bisa aktifin ability kartu atau buang kartu jelek.

#### Acceptance Criteria

**AC1:** Tombol "Buang" tersedia setelah draw
**AC2:** Klik tombol buang langsung masukkan kartu ke discard pile
**AC3:** Jika kartu ability, masuk ke fase ability resolution
**AC4:** Jika bukan ability card, langsung end turn
**AC5:** Kartu terbuka di discard pile (semua pemain bisa lihat)

#### Detail Implementasi

| Aspek      | Deskripsi             |
| ---------- | --------------------- |
| Input      | Klik tombol "Buang"   |
| Output     | Kartu ke discard pile |
| Error Case | -                     |

---

### US-008: Menggunakan Ability Peek Self (Kartu 7 dan 8)

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** liat salah satu kartu saya sendiri pas buang kartu 7 atau 8,
**Sehingga** saya tahu posisi kartu bagus/jelek buat planning selanjutnya.

#### Acceptance Criteria

**AC1:** Saat buang kartu 7 atau 8, sistem masuk ke fase SELECTING_OWN_CARD
**AC2:** Semua kartu di tangan highlight menunjukkan bisa dipilih
**AC3:** Klik pada kartu membuka kartu tersebut selama 3 detik
**AC4:** Hanya pemain yang pakai ability yang bisa liat kartu
**AC5:** Setelah 3 detik, kartu otomatis tertutup
**AC6:** Giliran berpindah ke pemain berikutnya

#### Detail Implementasi

| Aspek      | Deskripsi                             |
| ---------- | ------------------------------------- |
| Input      | Buang kartu 7/8 → pilih kartu sendiri |
| Output     | Kartu terbuka 3 detik                 |
| Error Case | Timeout 15 detik: ability dicancel    |

---

### US-009: Menggunakan Ability Peek Enemy (Kartu 9 dan 10)

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** intip salah satu kartu lawan pas buang kartu 9 atau 10,
**Sehingga** saya bisa tahu kartu lawan rendah atau tinggi buat strategi swap.

#### Acceptance Criteria

**AC1:** Saat buang kartu 9 atau 10, sistem masuk ke fase SELECTING_TARGET
**AC2:** Highlight pemain lain yang bisa dipilih
**AC3:** Klik pada pemain target menunjukkan kartu-kartunya
**AC4:** Klik pada kartu target membuka kartu tersebut selama 3 detik
**AC5:** Hanya pemain yang pakai ability yang bisa liat
**AC6:** Lawan yang diintip gak tahu kartunya dilihat
**AC7:** Setelah 3 detik, giliran berpindah

#### Detail Implementasi

| Aspek      | Deskripsi                                                  |
| ---------- | ---------------------------------------------------------- |
| Input      | Buang kartu 9/10 → pilih lawan → pilih kartu               |
| Output     | Kartu lawan terbuka 3 detik (hanya buat pemain yang intip) |
| Error Case | Timeout 15 detik: ability dicancel                         |

---

### US-010: Menggunakan Ability Blind Swap (Kartu Jack)

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** tuker 2 kartu tanpa liat nilainya pas buang Jack,
**Sehingga** saya bisa gambling menukar kartu jelek saya dengan kartu lawan.

#### Acceptance Criteria

**AC1:** Saat buang Jack, sistem masuk ke fase BLIND_SWAP
**AC2:** Pemain pilih kartu sendiri dulu
**AC3:** Kemudian pilih target pemain
**AC4:** Kemudian pilih kartu target
**AC5:** Kedua kartu bertukar posisi secara instant
**AC6:** Tidak ada reveal nilai kartu saat swap
**AC7:** Giliran berpindah setelah swap selesai

#### Detail Implementasi

| Aspek      | Deskripsi                                                          |
| ---------- | ------------------------------------------------------------------ |
| Input      | Buang Jack → pilih kartu sendiri → pilih lawan → pilih kartu lawan |
| Output     | 2 kartu bertukar posisi                                            |
| Error Case | Timeout 15 detik di tiap step                                      |

---

### US-011: Menggunakan Ability See and Swap (Kartu Queen dan King)

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** liat dulu 2 kartu sebelum tuker pas buang Queen/King,
**Sehingga** saya bisa decide mau swap atau gak berdasarkan nilai yang kelihatan.

#### Acceptance Criteria

**AC1:** Saat buang Q/K, sistem masuk ke fase SEE_AND_SWAP
**AC2:** Pemain pilih kartu sendiri
**AC3:** Kemudian pilih target pemain dan kartu target
**AC4:** Kedua kartu terbuka dan ditampilkan di UI popup
**AC5:** Pemain bisa pilih Confirm Swap atau Cancel
**AC6:** Jika Confirm, kartu bertukar
**AC7:** Jika Cancel, gak terjadi apa-apa
**AC8:** Giliran berpindah setelah confirm/cancel

#### Detail Implementasi

| Aspek      | Deskripsi                                |
| ---------- | ---------------------------------------- |
| Input      | Buang Q/K → pilih kartu → confirm/cancel |
| Output     | Swap terjadi atau dibatalkan             |
| Error Case | Timeout 15 detik: auto-cancel            |

---

### US-012: Memanggil KABUL

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** panggil KABUL pas yakin nilai kartu saya paling rendah,
**Sehingga** saya bisa end game dan menang.

#### Acceptance Criteria

**AC1:** Tombol "📣 KABUL" tersedia di awal giliran (DRAWING phase)
**AC2:** Klik tombol membuka modal konfirmasi
**AC3:** Konfirmasi akan trigger call KABUL
**AC4:** UI menampilkan indikator "KABUL by [PlayerName]" besar-besar
**AC5:** Sound effect "kabul-call" diputar buat semua pemain
**AC6:** Pemain yang KABUL gak bisa lakukan aksi lagi
**AC7:** Setiap pemain lain dapet satu giliran terakhir
**AC8:** Setelah final turns selesai, game end

#### Detail Implementasi

| Aspek      | Deskripsi                                 |
| ---------- | ----------------------------------------- |
| Input      | Klik tombol KABUL → konfirmasi            |
| Output     | Game masuk final turns                    |
| Error Case | KABUL cuma bisa dipanggil sekali per game |

---

### US-013: Melakukan SLAP

**Sebagai** Builder (pemain kasual),
**Saya ingin** slap kartu dari tangan ke discard kalo rank-nya sama,
**Sehingga** saya bisa kurangi jumlah kartu di tangan dengan cepat.

#### Acceptance Criteria

**AC1:** Selama fase PLAYING (bukan giliran sendiri juga bisa), pemain bisa slap
**AC2:** Long-press atau key binding pada kartu trigger slap
**AC3:** Sistem cek apakah rank kartu cocok dengan top discard
**AC4:** Jika match: kartu berhasil dibuang, jumlah kartu berkurang 1
**AC5:** Jika no match: penalti +1 kartu dari deck
**AC6:** Sound effect berbeda buat success vs fail
**AC7:** Toast notification tampilkan hasil slap

#### Detail Implementasi

| Aspek      | Deskripsi                                    |
| ---------- | -------------------------------------------- |
| Input      | Long-press/key binding pada kartu            |
| Output     | Kartu dibuang (success) atau +1 kartu (fail) |
| Error Case | -                                            |

---

### US-014: Melihat Skor dan Pemenang

**Sebagai** Builder (pemain kasual),
**Saya ingin** liat skor akhir semua pemain pas game selesai,
**Sehingga** saya tahu siapa pemenangnya dan skor saya berapa.

#### Acceptance Criteria

**AC1:** Saat game end, semua kartu otomatis terbuka
**AC2:** Skor tiap pemain dihitung dan ditampilkan
**AC3:** Pemenang (skor terendah) di-highlight dengan efek visual
**AC4:** Ranking 1-4 ditampilkan jelas
**AC5:** Detail per kartu ditampilkan (bisa expand/collapse)
**AC6:** Sound effect "success" buat pemenang, "error" buat yang lain
**AC7:** Tombol "Rematch" tersedia buat host
**AC8:** Tombol "Back to Lobby" tersedia buat semua pemain

#### Detail Implementasi

| Aspek      | Deskripsi                                |
| ---------- | ---------------------------------------- |
| Input      | Game end (setelah KABUL atau deck habis) |
| Output     | Scoreboard dengan ranking                |
| Error Case | -                                        |

---

### US-015: Host Memulai Game

**Sebagai** Builder (pemain kasual),
**Saya ingin** mulai game kalo saya jadi host dan udah ada minimal 2 pemain,
**Sehingga** gak perlu nunggu room penuh kalo teman saya udah ready.

#### Acceptance Criteria

**AC1:** Host punya tombol "Mulai Game" yang disabled kalo kurang dari 2 pemain
**AC2:** Tombol enabled kalo ada 2-4 pemain
**AC3:** Klik tombol trigger countdown 3 detik sebelum game start
**AC4:** Semua pemain lihat countdown yang sama
**AC5:** Game otomatis masuk fase MEMORIZE setelah countdown
**AC6:** Non-host gak punya tombol start (cuma bisa liat "Menunggu host...")

#### Detail Implementasi

| Aspek      | Deskripsi                             |
| ---------- | ------------------------------------- |
| Input      | Host klik "Mulai Game"                |
| Output     | Countdown 3 detik → game start        |
| Error Case | Kurang dari 2 pemain: tombol disabled |

---

### US-016: Memilih Costume/Varian Permainan

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** pilih varian aturan (Classic atau Reversed) pas buat room,
**Sehingga** saya bisa main dengan aturan yang berbeda-beda.

#### Acceptance Criteria

**AC1:** Saat create room, ada dropdown pilihan costume
**AC2:** Pilihan: Classic (default) atau Reversed
**AC3:** Costume info ditampilkan di room list
**AC4:** Deck digenerate sesuai costume yang dipilih
**AC5:** Nilai kartu berbeda berdasarkan costume (misal: Joker = -1 di Classic, 0 di Reversed)
**AC6:** Costume gak bisa diubah setelah room dibuat

#### Detail Implementasi

| Aspek      | Deskripsi                                  |
| ---------- | ------------------------------------------ |
| Input      | Pilih costume saat create room             |
| Output     | Room dibuat dengan aturan costume tersebut |
| Error Case | -                                          |

---

## User Stories Phase 2 (Enhancement)

### US-017: Chat dengan Pemain Lain

**Sebagai** Designer (pemain sosial),
**Saya ingin** chat sama pemain lain di room,
**Sehingga** saya bisa ngobrol, bluff, atau ngasih reaksi pas game berlangsung.

#### Acceptance Criteria

**AC1:** Chat panel slide-in dari sisi kanan layar
**AC2:** Chat bisa dibuka/tutup dengan tombol atau key binding
**AC3:** Histori 50 pesan terakhir disimpan
**AC4:** Auto-scroll ke pesan terbaru
**AC5:** Enter key buat kirim pesan
**AC6:** Pesan muncul real-time tanpa refresh
**AC7:** Filter kata-kata kasar (basic moderation)

#### Detail Implementasi

| Aspek      | Deskripsi                  |
| ---------- | -------------------------- |
| Input      | Ketik pesan → Enter        |
| Output     | Pesan muncul di chat panel |
| Error Case | Pesan kosong: gak dikirim  |

---

### US-018: Mengirim Emoji Reaction

**Sebagai** Designer (pemain sosial),
**Saya ingin** kirim emoji reaction pas momen penting,
**Sehingga** saya bisa ekspresi kaget, senang, atau kesel tanpa ngetik.

#### Acceptance Criteria

**AC1:** Emoji picker tersedia di dekat chat atau sebagai quick button
**AC2:** Minimal 8 emoji: 😂 🔥 💀 😱 😤 👏 🤔 💩
**AC3:** Emoji muncul sebagai bubble di atas avatar pemain
**AC4:** Emoji fade out setelah 3 detik
**AC5:** Cooldown 5 detik antar emoji biar gak spam
**AC6:** Emoji terlihat semua pemain di room

#### Detail Implementasi

| Aspek      | Deskripsi                      |
| ---------- | ------------------------------ |
| Input      | Klik emoji dari picker         |
| Output     | Emoji bubble muncul di avatar  |
| Error Case | Cooldown aktif: tunggu 5 detik |

---

### US-019: Melihat Statistik Pemain

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** liat statistik permainan saya,
**Sehingga** saya bisa track progress dan liat seberapa bagus saya main.

#### Acceptance Criteria

**AC1:** Profile page/menu menampilkan stats pemain
**AC2:** Stats yang ditampilkan:

- Total games played
- Win rate (persentase)
- Total wins/losses
- Average score per game
- Best score (lowest)
- Total KABUL calls (success vs fail)
  **AC3:** Data persisten (tersimpan antar session)
  **AC4:** Stats update otomatis setelah tiap game
  **AC5:** Time filter: All Time, This Month, This Week

#### Detail Implementasi

| Aspek      | Deskripsi                                     |
| ---------- | --------------------------------------------- |
| Input      | Buka profile/stats menu                       |
| Output     | Tampilan statistik lengkap                    |
| Error Case | Data gak tersedia: tampilkan "Belum ada data" |

---

### US-020: Melihat Leaderboard

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** liat leaderboard pemain terbaik,
**Sehingga** saya bisa bandingin skill saya sama yang lain dan ada target buat chase.

#### Acceptance Criteria

**AC1:** Leaderboard menampilkan top 100 pemain
**AC2:** Ranking berdasarkan: Win Rate, Total Wins, atau Best Score
**AC3:** Filter: Global, Friends Only, Weekly
**AC4:** Posisi pemain sendiri selalu ditampilkan (meski gak top 100)
**AC5:** Avatar dan username pemain ditampilkan
**AC6:** Leaderboard refresh setiap jam

#### Detail Implementasi

| Aspek      | Deskripsi                                          |
| ---------- | -------------------------------------------------- |
| Input      | Buka leaderboard menu                              |
| Output     | List ranking pemain                                |
| Error Case | Server error: tampilkan "Gagal memuat leaderboard" |

---

### US-021: Menambahkan Teman

**Sebagai** Designer (pemain sosial),
**Saya ingin** add teman dari pemain yang pernah main bareng,
**Sehingga** saya bisa gampang invite mereka main lagi.

#### Acceptance Criteria

**AC1:** Tombol "Add Friend" tersedia di profile pemain lain
**AC2:** Friend request dikirim dan muncul notifikasi buat target
**AC3:** Target bisa accept atau decline request
**AC4:** Kalo diaccept, teman muncul di friend list
**AC5:** Friend list bisa diakses dari menu utama
**AC6:** Bisa liat status teman (Online, In Game, Offline)
**AC7:** Bisa invite teman langsung ke room

#### Detail Implementasi

| Aspek      | Deskripsi                                         |
| ---------- | ------------------------------------------------- |
| Input      | Klik "Add Friend" → konfirmasi                    |
| Output     | Friend request terkirim                           |
| Error Case | Target gak nerima request: timeout setelah 7 hari |

---

### US-022: Melihat Match History

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** liat riwayat game yang pernah saya mainin,
**Sehingga** saya bisa review game dan liat kapan saya menang/kalah.

#### Acceptance Criteria

**AC1:** Match history menampilkan 50 game terakhir
**AC2:** Info per match:

- Tanggal dan waktu
- Mode (Classic/Reversed)
- Posisi akhir (1st, 2nd, 3rd, 4th)
- Skor akhir
- List pemain yang main
  **AC3:** Bisa filter: All, Wins Only, Losses Only
  **AC4:** Bisa klik match buat liat detail lengkap
  **AC5:** Detail match menunjukkan kartu yang dimiliki tiap pemain

#### Detail Implementasi

| Aspek      | Deskripsi                                        |
| ---------- | ------------------------------------------------ |
| Input      | Buka match history menu                          |
| Output     | List riwayat game                                |
| Error Case | Belum pernah main: tampilkan "Belum ada riwayat" |

---

### US-023: Mengatur Volume dan Audio

**Sebagai** Builder (pemain kasual),
**Saya ingin** atur volume sound effect dan music,
**Sehingga** saya bisa main dengan nyaman, gak ganggu orang lain, atau matiin kalo bosan.

#### Acceptance Criteria

**AC1:** Settings menu punya Audio section
**AC2:** Slider untuk: Master Volume, SFX Volume, Music Volume (0-100%)
**AC3:** Toggle Mute/Unmute untuk masing-masing kategori
**AC4:** Setting persisten (tersimpan antar session)
**AC5:** Preview sound effect saat adjust volume
**AC6:** Default: Master 80%, SFX 100%, Music 50%

#### Detail Implementasi

| Aspek      | Deskripsi                      |
| ---------- | ------------------------------ |
| Input      | Adjust slider atau toggle mute |
| Output     | Volume berubah real-time       |
| Error Case | -                              |

---

### US-024: Melihat Tutorial

**Sebagai** Builder (pemain kasual),
**Saya ingin** liat tutorial cara main pas pertama kali buka game,
**Sehingga** saya gak bingung dan langsung bisa main.

#### Acceptance Criteria

**AC1:** Tutorial otomatis muncul pas pertama kali main
**AC2:** Tutorial bisa di-skip atau diulang dari menu
**AC3:** Tutorial interaktif dengan langkah-langkah:

- Pengenalan tujuan game
- Penjelasan nilai kartu
- Demo fase MEMORIZE
- Demo turn (draw, swap, discard)
- Penjelasan ability cards
- Demo KABUL dan SLAP
  **AC4:** Tiap step ada visual highlight yang jelas
  **AC5:** Bisa balik ke step sebelumnya
  **AC6:** Total durasi tutorial maksimal 5 menit

#### Detail Implementasi

| Aspek      | Deskripsi                                 |
| ---------- | ----------------------------------------- |
| Input      | First launch atau klik "Tutorial" di menu |
| Output     | Interactive tutorial sequence             |
| Error Case | -                                         |

---

### US-025: Menggunakan Card Skins

**Sebagai** Designer (pemain sosial),
**Saya ingin** ganti skin kartu saya dengan desain yang keren,
**Sehingga** tampilan game saya beda sama yang lain dan lebih personal.

#### Acceptance Criteria

**AC1:** Skin shop/inventory menampilkan skin yang dimiliki
**AC2:** Skin bisa dibeli pake in-game currency (kalo ada) atau Robux
**AC3:** Preview skin sebelum apply
**AC4:** Skin categories: Classic, Fantasy, Neon, Minimalist
**AC5:** Skin apply ke semua kartu (bukan per kartu)
**AC6:** Skin terlihat oleh pemain lain
**AC7:** Default skin selalu tersedia (gratis)

#### Detail Implementasi

| Aspek      | Deskripsi                                          |
| ---------- | -------------------------------------------------- |
| Input      | Pilih skin → Apply                                 |
| Output     | Kartu pakai skin baru                              |
| Error Case | Skin belum dibeli: tampilkan harga dan tombol beli |

---

### US-026: Mode Spectator

**Sebagai** Scripter (pemain kompetitif),
**Saya ingin** nonton game temen yang lagi main,
**Sehingga** saya bisa belajar strategi dari pemain bagus sambil nunggu.

#### Acceptance Criteria

**AC1:** Dari lobby, bisa pilih room dan klik "Spectate"
**AC2:** Spectator gak ikut main dan gak menghitung ke slot pemain
**AC3:** Spectator bisa liat semua kartu (seperti mode ENDED)
**AC4:** Bisa chat sama spectator lain
**AC5:** Bisa keluar dari spectator kapan aja
**AC6:** Maksimal 4 spectator per room
**AC7:** Spectator gak bisa ikut rematch

#### Detail Implementasi

| Aspek      | Deskripsi                                   |
| ---------- | ------------------------------------------- |
| Input      | Klik "Spectate" di room                     |
| Output     | Masuk room sebagai spectator                |
| Error Case | Room full spectator: "Slot spectator penuh" |

---

### US-027: Mengirim Gift/Kado ke Teman

**Sebagai** Designer (pemain sosial),
**Saya ingin** kirim gift atau kado ke teman,
**Sehingga** saya bisa kasih hadiah skin atau item ke teman yang udah bantu saya.

#### Acceptance Criteria

**AC1:** Dari friend list, bisa pilih teman dan klik "Kirim Gift"
**AC2:** Pilihan gift: Skin, Emoji Pack, atau Currency
**AC3:** Harga gift ditampilkan (dalam Robux)
**AC4:** Konfirmasi sebelum kirim
**AC5:** Teman nerima notifikasi gift
**AC6:** Teman bisa accept gift dari inventory/notifikasi
**AC7:** Gift history tersimpan

#### Detail Implementasi

| Aspek      | Deskripsi                              |
| ---------- | -------------------------------------- |
| Input      | Pilih teman → Pilih gift → Konfirmasi  |
| Output     | Gift terkirim, teman nerima notifikasi |
| Error Case | Teman gak accept dalam 7 hari: refund  |

---

## Kriteria Prioritas

### Definisi Prioritas

| Prioritas         | Definisi                                              | Timeline    |
| ----------------- | ----------------------------------------------------- | ----------- |
| **P0 (Critical)** | Fitur wajib ada, game gak bisa jalan tanpa ini        | Sprint 1-2  |
| **P1 (High)**     | Fitur penting, pengalaman user berkurang kalo gak ada | Sprint 3-4  |
| **P2 (Medium)**   | Fitur nice-to-have, enhance experience                | Sprint 5-6  |
| **P3 (Low)**      | Fitur optional, bisa ditambah nanti                   | Post-launch |

### Mapping User Stories ke Prioritas

#### P0 - Critical (Phase 1 MVP)

| US ID  | User Story                        | Alasan Prioritas                                    |
| ------ | --------------------------------- | --------------------------------------------------- |
| US-001 | Membuat Room Baru                 | Core functionality, gak bisa main kalo gak ada room |
| US-002 | Join Room dari Lobby              | Core functionality, cara pemain masuk ke game       |
| US-003 | Melihat Kartu Saat Fase Memorize  | Core gameplay mechanic                              |
| US-004 | Mengambil Kartu dari Deck         | Core gameplay mechanic                              |
| US-005 | Mengambil Kartu dari Discard Pile | Core gameplay mechanic                              |
| US-006 | Menukar Kartu dengan Tangan       | Core gameplay mechanic                              |
| US-007 | Membuang Kartu Tanpa Swap         | Core gameplay mechanic                              |
| US-014 | Melihat Skor dan Pemenang         | Game loop gak complete kalo gak ada ini             |
| US-015 | Host Memulai Game                 | Entry point ke gameplay                             |

**Total:** 9 User Stories

#### P1 - High (Phase 1 MVP)

| US ID  | User Story                       | Alasan Prioritas                |
| ------ | -------------------------------- | ------------------------------- |
| US-008 | Menggunakan Ability Peek Self    | Card abilities = depth gameplay |
| US-009 | Menggunakan Ability Peek Enemy   | Card abilities = depth gameplay |
| US-010 | Menggunakan Ability Blind Swap   | Card abilities = depth gameplay |
| US-011 | Menggunakan Ability See and Swap | Card abilities = depth gameplay |
| US-012 | Memanggil KABUL                  | Mekanik unik game ini           |
| US-013 | Melakukan SLAP                   | Mekanik unik game ini           |
| US-016 | Memilih Costume/Varian           | Replayability                   |

**Total:** 7 User Stories

#### P2 - Medium (Phase 2 Enhancement)

| US ID  | User Story                | Alasan Prioritas               |
| ------ | ------------------------- | ------------------------------ |
| US-017 | Chat dengan Pemain Lain   | Social feature penting         |
| US-018 | Mengirim Emoji Reaction   | Quick communication            |
| US-019 | Melihat Statistik Pemain  | Player retention               |
| US-023 | Mengatur Volume dan Audio | Quality of life                |
| US-024 | Melihat Tutorial          | Onboarding pemain baru         |
| US-025 | Menggunakan Card Skins    | Monetization & personalization |

**Total:** 6 User Stories

#### P3 - Low (Phase 2 Enhancement)

| US ID  | User Story                  | Alasan Prioritas       |
| ------ | --------------------------- | ---------------------- |
| US-020 | Melihat Leaderboard         | Competitive feature    |
| US-021 | Menambahkan Teman           | Social network         |
| US-022 | Melihat Match History       | Retention & engagement |
| US-026 | Mode Spectator              | Engagement & learning  |
| US-027 | Mengirim Gift/Kado ke Teman | Monetization           |

**Total:** 5 User Stories

### Ringkasan Distribusi

| Fase                  | P0    | P1    | P2    | P3    | Total  |
| --------------------- | ----- | ----- | ----- | ----- | ------ |
| Phase 1 (MVP)         | 9     | 7     | 0     | 0     | 16     |
| Phase 2 (Enhancement) | 0     | 0     | 6     | 5     | 11     |
| **Total**             | **9** | **7** | **6** | **5** | **27** |

---

## Acceptance Criteria Format

Setiap acceptance criteria menggunakan format berikut:

- **AC#:** [Deskripsi criteria]
  - Given [kondisi awal]
  - When [aksi dilakukan]
  - Then [hasil yang diharapkan]

### Contoh Detail:

**US-004: Mengambil Kartu dari Deck**

**AC1:** Saat giliran, deck highlight atau glow

```
Given: Giliran pemain aktif
When: Phase DRAWING dimulai
Then: Deck model memiliki efek glow/highlight berwarna hijau
```

**AC2:** Klik pada deck mengambil 1 kartu acak

```
Given: Phase DRAWING dan deck tersedia
When: Pemain klik deck
Then: 1 kartu random diambil dan ditampilkan di tengah layar
```

---

## Catatan Implementasi

### Untuk Tim Development

1. **Performance:** Roblox punya limitasi, pastikan semua interaksi responsif (< 200ms)
2. **Network:** Minimalkan RemoteEvent calls, batch updates kalo bisa
3. **Mobile:** Pastikan touch interface works well untuk semua interaksi
4. **Accessibility:** Sediakan visual indicators yang jelas, jangan cuma rely on sound

### Untuk Tim QA

1. **Test Cases:** Buat test case berdasarkan acceptance criteria
2. **Edge Cases:** Perhatikan timeout, disconnect, dan concurrency issues
3. **Platform Testing:** Test di PC dan mobile (tablet minimal)

### Untuk Tim Design

1. **Consistent UI:** Gunakan design system yang sama di semua screen
2. **Feedback Visual:** Tiap aksi harus ada feedback (sound, animation, atau UI update)
3. **Clarity:** Prioritaskan informasi penting, kurangi clutter

---

## Kesimpulan

Dokumen ini mencakup 27 user stories yang menggambarkan seluruh fitur Kabul Game dari perspektif pengguna. Dengan pembagian Phase 1 (MVP) dan Phase 2 (Enhancement), tim development punya roadmap yang jelas buat deliver game yang fun dan playable dulu, baru tambah fitur-fitur sosial dan competitive nanti.

**Total User Stories:** 27 stories
**Phase 1 (MVP):** 16 stories (59%)
**Phase 2 (Enhancement):** 11 stories (41%)

**Distribusi Persona:**

- Builder (Pemain Kasual): 6 stories
- Scripter (Pemain Kompetitif): 12 stories
- Designer (Pemain Sosial): 9 stories

---

_Dokumen ini dibuat sebagai panduan development Kabul Game di platform Roblox. Semua user stories dirancang dengan mempertimbangkan target audience muda dan karakteristik platform Roblox._
