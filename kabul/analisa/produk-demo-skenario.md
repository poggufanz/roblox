# Dokumen Demo Skenario: Kabul 3D Table Experience

## Pendahuluan: Selamat Datang di Meja Kabul

Bayangkan sebuah ruangan virtual di Roblox. Cahaya hangat dari lampu gantung menerangi sebuah meja kayu antik di tengah ruangan. Empat kursi kayu mengelilingi meja, masing-masing dengan pemandangan unik ke pusat permainan. Di tengah meja, tumpukan kartu berselimutkan kain hijau tua, mengundang para pemain untuk mulai bertarung dalam permainan kartu penuh strategi dan intrik.

Ini bukan sekadar permainan kartu biasa. Ini adalah Kabul, sebuah permainan yang menggabungkan memori, psikologi, dan sedikit keberuntungan dalam sebuah pengalaman 3D yang imersif. Dalam dokumen ini, kita akan mengikuti perjalanan dua pemain, Andi dan Budi, saat mereka memainkan satu ronde lengkap Kabul dari awal hingga akhir.

### Karakter

**Andi** - Seorang pemain berpengalaman yang sudah memahami seluk-beluk permainan Kabul. Dia tahu kapan harus bermain aman dan kapan harus mengambil risiko.

**Budi** - Pemain baru yang antusias, masih mempelajari strategi dasar tapi punya naluri bermain yang tajam.

---

## Bab 1: Lobby - Tempat Pertemuan (Fase WAITING)

### Skenario 1.1: Memasuki Dunia Virtual

_Suasana: Sebuah lobby besar dengan dekorasi klasik. Dinding berwarna krem dengan lukisan-lukisan kartu zaman dulu. Beberapa meja virtual terlihat tersebar di ruangan, masing-masing menunjukkan status permainan._

Andi baru saja membuka aplikasi Kabul di Roblox. Avatar-nya, yang mengenakan blazer biru tua dan topi fedora, muncul di lobby utama. Dia melihat sekeliling. Beberapa meja sudah penuh dengan pemain yang sedang bertarung. Suara kartu yang dikocok terdengar samar dari arah salah satu meja.

Di pojok kanan atas layar Andi, sebuah daftar room muncul. Room "Jakarta Night #42" menunjukkan status "WAITING - 1/4 players". Nama host: Budi_Gaming.

**Perspektif Server (Backend):**

```
[Server Log] Player "Andi_Pro" connected
[Server Log] Querying active rooms...
[Server Log] Found 12 active rooms, 3 waiting
[Server Log] Room "Jakarta Night #42" found
[Server Log] Room ID: room_8f3a9b2c
[Server Log] Current players: 1
[Server Log] Host: Budi_Gaming (playerId: p_9921)
[Server Log] Costume: COSTUME_1 (Classic)
```

Andi mengarahkan kursornya ke room tersebut dan mengklik tombol "Join". Layar-nya berubah, menampilkan loading screen dengan animasi kartu yang berputar.

### Skenario 1.2: Masuk ke Room

_Tiga detik kemudian, Andi muncul di sebuah ruangan yang lebih kecil dan intim. Empat kursi kayu mengelilingi sebuah meja bundar. Di seberang meja, Budi sudah duduk menunggu._

Budi melihat Andi muncul. Avatar Budi, yang mengenakan hoodie merah dan sneakers putih, melambaikan tangan virtual.

"Halo!" kata Budi melalui chat box di pojok kiri bawah. "Mau main 2 player aja atau nunggu yang lain?"

Andi melihat sekeliling ruangan. Ada empat kursi, tapi hanya dua yang terisi. Dia membuka mic-nya dan berkata, "Ayo kita mulai aja 2 player. Lebih cepat dan lebih intens."

**Perspektif Server:**

```
[Server Log] Player "Andi_Pro" (p_7742) joining room_8f3a9b2c
[Server Log] Validating room capacity...
[Server Log] Capacity OK: 2/4 players
[Server Log] Adding player to room state
[Server Log] Broadcasting player_joined event
[Server Log] Room state updated:
{
  roomId: "room_8f3a9b2c",
  hostId: "p_9921",
  players: [
    { id: "p_9921", name: "Budi_Gaming", isConnected: true, hand: [] },
    { id: "p_7742", name: "Andi_Pro", isConnected: true, hand: [] }
  ],
  status: "WAITING",
  costume: "COSTUME_1"
}
```

Di layar Andi dan Budi, masing-masing melihat panel pemain di sisi kanan. Panel tersebut menampilkan:

- Budi_Gaming (Host)
- Andi_Pro

Tombol "Start Game" hanya muncul di layar Budi karena dia adalah host.

### Skenario 1.3: Host Memulai Permainan

Budi menggerakkan mouse-nya ke tombol "Start Game" yang berwarna hijau di bagian bawah layar. Tombol tersebut berkilau dengan efek hover. Budi mengkliknya.

Sebuah dialog konfirmasi muncul: "Mulai permainan dengan 2 pemain?"

Budi mengklik "Yes".

**Perspektif Server:**

```
[Server Log] Host p_9921 initiated game start
[Server Log] Validating player count: 2 players (min: 2) ✓
[Server Log] Initializing new game instance...
[Server Log] Creating deck with 54 cards (COSTUME_1)
[Server Log] Shuffling deck...
[Server Log] Dealing 4 cards to each player...
[Server Log] Player p_9921 hand: [hidden x4]
[Server Log] Player p_7742 hand: [hidden x4]
[Server Log] Initializing discard pile...
[Server Log] Top discard: 7♦
[Server Log] Setting phase to MEMORIZE
[Server Log] Memorize phase will end in 3000ms
[Server Log] Broadcasting game_start event
```

---

## Bab 2: Momen Kritis - Fase MEMORIZE

### Skenario 2.1: Cahaya Terfokus

_Tiba-tiba, lampu di ruangan meredup. Satu-satunya sumber cahaya sekarang berasal dari tiga titik: tumpukan deck di tengah, kartu terbuka di discard pile, dan keempat kartu yang terletak di depan masing-masing pemain._

Andi melihat ke meja virtual. Di depannya, empat kartu melayang sedikit di atas permukaan meja, tersusun dalam format 2x2. Namun yang menarik perhatiannya: hanya dua kartu di baris bawah yang terbuka.

Kartu kiri bawah: **A♠** (Ace Sekop)
Kartu kanan bawah: **5♥** (5 Hati)

Dua kartu di atasnya tetap tertutup, menampilkan motif back kartu yang bergambar pola geometris berwarna biru dan emas.

Di seberang meja, Budi juga sedang melihat kartunya sendiri. Dia bisa melihat kartu-kartu Andi, tapi semuanya tertutup. Tidak ada cara untuk mengetahui apa yang sedang dilihat Andi.

**Perspektif Server (Client State untuk Andi):**

```javascript
{
  phase: "MEMORIZE",
  memorizeEndsAt: 1709123456789, // 3 detik dari sekarang
  myHand: [
    { position: 0, display: "A♠", value: 1, visible: true },
    { position: 1, display: "5♥", value: 5, visible: true },
    { position: 2, visible: false }, // Tertutup
    { position: 3, visible: false }  // Tertutup
  ],
  opponents: [
    {
      id: "p_9921",
      name: "Budi_Gaming",
      cardCount: 4,
      hasCalledKabul: false
    }
  ],
  topDiscard: { display: "7♦", value: 7, rank: "7" },
  deckCount: 46
}
```

### Skenario 2.2: Detik-detik Berharga

"Oke, saya punya Ace dan 5," gumam Andi dalam hati. "Total yang saya tahu: 6 poin. Dua kartu lainnya masih misteri."

Andi mencoba mengingat posisi kartu. Ini penting karena nanti, ketika fase MEMORIZE selesai, semua kartu akan tertutup. Dia harus mengingat bahwa Ace ada di posisi kiri bawah (posisi 0) dan 5 ada di posisi kanan bawah (posisi 1).

Di seberang meja, Budi juga sedang berkonsentrasi. Matanya bergerak cepat antara kartu-kartunya.

Di tengah layar masing-masing pemain, sebuah timer countdown besar muncul: **3... 2... 1...**

Timer itu berdetak dengan suara tik-tok yang terdengar jelas. Efek visual berupa lingkaran merah yang menyusut mengelilingi angka, memberikan tekanan psikologis.

### Skenario 2.3: Transisi ke Fase PLAYING

**Timer mencapai 0.**

_Sebuah efek suara bergema di ruangan: suara kartu yang dibalik secara bersamaan. Cahaya kembali menyinari seluruh ruangan._

Di layar Andi, kedua kartu yang tadi terbuka kini tertutup. Mereka bergabung dengan dua kartu lainnya, membentuk empat kartu misterius di depannya.

Andi sekarang melihat:

- Pos 0: [???] - Dia tahu ini Ace, tapi visualnya tertutup
- Pos 1: [???] - Dia tahu ini 5
- Pos 2: [???] - Tidak diketahui
- Pos 3: [???] - Tidak diketahui

**Perspektif Server:**

```
[Server Log] MEMORIZE phase ended
[Server Log] Transitioning to PLAYING phase
[Server Log] Current turn: p_9921 (Budi_Gaming)
[Server Log] Broadcasting phase_change event
[Server Log] Client state updated - all hands masked
```

Sebuah indikator muncul di atas avatar Budi: "🎯 TURN". Giliran Budi untuk bermain.

---

## Bab 3: Pertarungan Dimulai - Fase PLAYING (Early Game)

### Skenario 3.1: Giliran Pertama Budi

Budi melihat ke meja. Di tengah, ada tumpukan deck (46 kartu tersisa) dan satu kartu terbuka di discard pile: **7♦**.

"Hmmm," Budi berpikir. "7♦. Kalau saya ambil, saya bisa pakai untuk swap. Tapi saya belum tahu nilai kartu saya yang tertutup."

Budi memutuskan untuk mengambil kartu dari deck. Dia menggerakkan kursor ke tumpukan deck, dan sebuah tooltip muncul: "Draw from Deck (46 cards)".

Budi mengklik.

**Animasi 3D:**

Kartu dari atas deck melayang keluar dengan animasi halus. Kartu tersebut berputar di udara, menunjukkan back-nya yang berwarna biru-keemasan, lalu mendarat di area "Drawn Card" di bagian bawah layar Budi. Kartu itu kemudian terbuka dengan efek flip 3D yang smooth.

**Kartu yang diambil Budi: Q♣ (Queen Sekop)**

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Player: p_9921
[Server Log] Source: deck
[Server Log] Card drawn: Q♣ (value: 13, actionType: SEE_AND_SWAP)
[Server Log] State: drawnCard = { playerId: "p_9921", card: Q♣ }
[Server Log] Deck remaining: 45
```

Budi melihat kartu Queen di tangannya. Nilainya 13, sangat tinggi dan buruk untuk skor akhir. Tapi Queen punya ability SEE_AND_SWAP - dia bisa menukar kartu sambil melihat nilai kartu lawan terlebih dahulu.

"Ini berharga," pikir Budi. "Tapi 13 terlalu tinggi untuk disimpan. Saya harus swap atau discard."

### Skenario 3.2: Keputusan Strategis

Budi mempertimbangkan:

- Jika dia swap Q♣ dengan salah satu kartunya, kartu yang diganti masuk ke discard pile
- Tapi dia tidak tahu nilai kartu yang akan diganti (bisa jelek, bisa bagus)
- Atau dia bisa discard Q♣ langsung dan memicu ability SEE_AND_SWAP

Budi memutuskan untuk swap. Dia yakin salah satu dari empat kartunya mungkin lebih tinggi dari 13, jadi lebih baik menukar.

Dia menggerakkan mouse ke kartu Queen di area drawn card, lalu mengklik dan menyeret (drag) ke salah satu kartu tertutup di tangannya - kartu posisi 2 (kanan atas).

**Interaksi 3D:**

Saat Budi hover ke kartu tertutup di tangannya, kartu tersebut sedikit terangkat (lift effect), memberikan feedback visual bahwa kartu itu bisa dipilih. Saat dia drop kartu Queen di sana, sebuah animasi swap terjadi:

1. Kartu Queen bergerak ke posisi kartu target
2. Kartu yang ada di posisi tersebut (tertutup) bergerak ke discard pile
3. Efek partikel kecil muncul saat swap selesai

**Kartu yang dibuang Budi: J♦ (Jack Hati)**

Jack memiliki ability BLIND_SWAP.

**Perspektif Server:**

```
[Server Log] Action: swapCard
[Server Log] Player: p_9921
[Server Log] Hand index: 2
[Server Log] Replaced card: J♦ (value: 11, actionType: BLIND_SWAP)
[Server Log] New card at pos 2: Q♣
[Server Log] Discard pile updated: top = J♦
[Server Log] Resolving ability: BLIND_SWAP
[Server Log] State: pendingAction = {
  type: "SWAPPING_CARDS",
  playerId: "p_9921",
  expiresAt: 1709123461789 // 15 detik timeout
}
```

### Skenario 3.3: Ability Activation - BLIND SWAP

Layar Budi berubah. Sebuah panel muncul dengan judul "🃏 BLIND SWAP - Pilih kartu untuk ditukar". Panel ini menunjukkan instruksi: "Pilih kartu milikmu, lalu pilih kartu lawan. Kartu akan ditukar tanpa terbuka."

Budi melihat keempat kartunya. Dua di bawah (pos 0 dan 1) masih misteri karena dia lupa apa yang dia lihat di fase MEMORIZE. Dua di atas (pos 2 dan 3) juga misteri, tapi dia baru saja menaruh Queen di pos 2.

"Kalau saya tukar Queen yang baru saja saya dapat, itu sia-sia," pikir Budi. "Lebih baik saya tukar salah satu dari tiga kartu lainnya."

Budi mengklik kartu posisi 0 (kiri bawah).

**Animasi 3D:**

Kartu tersebut sedikit menyala dengan aura biru, menandakan sudah dipilih.

**Perspektif Server:**

```
[Server Log] BLIND SWAP step 1: Own card selected
[Server Log] Player: p_9921
[Server Log] Own index: 0
[Server Log] Waiting for target selection...
```

Panel sekarang menunjukkan: "Sekarang pilih kartu lawan."

Di layar Budi, area kartu Andi di seberang meja sekarang memiliki highlight hijau. Keempat kartu Andi terlihat tertutup, tapi sekarang mereka bisa diklik.

Budi mengamati. Dia tidak tahu nilai kartu Andi (tentu saja). Dia harus menebak. "Andi sudah bermain cukup lama, dia mungkin punya kartu bagus. Saya akan ambil risiko."

Budi mengklik kartu posisi 1 milik Andi (kanan bawah).

**Animasi 3D Dramatis:**

Kedua kartu (Budi pos 0 dan Andi pos 1) melayang ke tengah meja. Mereka bertemu di udara, berputar bersama dalam spiral kecil, lalu kembali ke posisi masing-masing. Sepanjang animasi ini, kedua kartu tetap tertutup. Tidak ada yang tahu apa nilai kartu yang ditukar.

**Perspektif Server:**

```
[Server Log] BLIND SWAP step 2: Target card selected
[Server Log] Player: p_9921
[Server Log] Target player: p_7742 (Andi_Pro)
[Server Log] Target index: 1
[Server Log] Executing swap...
[Server Log] Budi's card at pos 0: [moved to Andi pos 1]
[Server Log] Andi's card at pos 1: [moved to Budi pos 0]
[Server Log] Swap completed - NO REVEAL
[Server Log] Clearing pending action
[Server Log] Advancing turn...
```

Di layar Andi, dia hanya melihat animasi kartunya bergerak dan kembali. Dia tidak tahu kartu apa yang sekarang ada di posisi 1 miliknya. Mungkin lebih baik, mungkin lebih buruk. Itulah intrik BLIND SWAP.

**Indikator berganti. Sekarang giliran Andi.**

### Skenario 3.4: Giliran Andi - Reaksi terhadap Swap

Andi melihat indikator "🎯 TURN" muncul di atas avatar-nya. Dia juga melihat kartunya di posisi 1 telah berubah (karena swap dari Budi), tapi dia tidak tahu nilainya.

"Oke, jadi Budi sudah menukar kartu saya," pikir Andi. "Saya harus lebih berhati-hati sekarang."

Andi melihat discard pile. Kartu teratas sekarang adalah **J♦** (Jack Hati) yang baru saja dibuang Budi. Jack memiliki ability BLIND SWAP, tapi sudah terlambat untuk menggunakannya - kartu sudah di discard pile.

"Discard pile menunjukkan apa yang lawan buang," Andi mengingatkan dirinya. "Budi membuang Jack, mungkin dia dapat kartu lebih baik darinya."

Andi memutuskan untuk mengambil dari deck. Dia mengklik tumpukan deck.

**Animasi 3D:**

Kartu melayang dari deck, berputar, dan terbuka di area drawn card.

**Kartu yang diambil Andi: 8♠ (8 Sekop)**

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Player: p_7742
[Server Log] Source: deck
[Server Log] Card drawn: 8♠ (value: 8, actionType: PEEK_SELF)
[Server Log] Deck remaining: 44
```

Andi melihat 8♠. Nilainya 8, cukup rendah. Tapi ability-nya adalah PEEK_SELF - jika dibuang, dia bisa melihat salah satu kartunya sendiri.

Andi memutuskan untuk membuang 8♠ langsung. Dia klik tombol "Discard".

**Perspektif Server:**

```
[Server Log] Action: discardDrawn
[Server Log] Player: p_7742
[Server Log] Discarded: 8♠
[Server Log] Resolving ability: PEEK_SELF
[Server Log] State: pendingAction = {
  type: "CHOOSING_OWN_CARD_TO_PEEK",
  playerId: "p_7742",
  expiresAt: 1709123465000 // 15 detik
}
```

### Skenario 3.5: Ability PEEK_SELF

Panel muncul di layar Andi: "👁️ PEEK - Pilih kartu milikmu untuk dilihat (3 detik)"

Andi mempertimbangkan. Dia tahu posisi 0 adalah Ace (1 poin) dari fase MEMORIZE. Pos 1 sekarang berisi kartu dari Budi (nilai tidak diketahui). Pos 2 dan 3 adalah misteri.

"Saya harus peek kartu yang paling tidak saya kenal," pikir Andi. "Posisi 1 sudah berubah karena swap Budi, jadi saya akan lihat itu."

Andi mengklik kartu posisi 1 miliknya.

**Animasi 3D:**

Kartu tersebut terangkat, lalu dengan animasi flip yang smooth, kartu terbuka menghadap ke Andi (dan hanya Andi).

**Kartu terbuka: K♣ (King Sekop)**

**Nilai: 13** (Black King - nilai tertinggi/terburuk!)

"Sial!" Andi hampir berteriak. "Budi memberi saya kartu jelek! King Sekop, 13 poin!"

Tapi Andi tidak bisa melakukan apa-apa. Kartu tersebut tetap terbuka selama 3 detik, memberinya waktu untuk memproses informasi ini.

Di layar Budi, kartu Andi posisi 1 tetap tertutup. Dia tidak tahu bahwa Andi baru saja melihat nilainya.

**Perspektif Server (Client State Andi):**

```javascript
peekedCard: {
  position: 1,
  display: "K♣",
  value: 13,
  targetPlayerId: "p_7742",
  expiresAt: 1709123468000 // 3 detik
}
```

Setelah 3 detik, kartu kembali tertutup. Tapi Andi sekarang tahu: dia punya King 13 poin di posisi 1. Itu sangat buruk untuk skor akhir.

"Saya harus buang kartu itu secepatnya," Andi bertekad.

Giliran berpindah kembali ke Budi.

---

## Bab 4: Memanfaatkan Informasi (Mid Game)

### Skenario 4.1: Budi Mengintai

Discard pile sekarang menunjukkan **8♠** di atas. Budi melihat ini.

"Andi membuang 8," pikir Budi. "Dia bisa peek kartu sendiri. Mungkin dia sedang mengumpulkan informasi."

Budi mengambil dari deck.

**Kartu yang diambil: 9♥ (9 Hati)**

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Player: p_9921
[Server Log] Card drawn: 9♥ (value: 9, actionType: PEEK_ENEMY)
```

Budi melihat 9♥. Nilainya 9, cukup rendah. Tapi ability-nya PEEK_ENEMY - bisa mengintip kartu lawan.

Budi memutuskan untuk discard 9♥ untuk memicu ability.

**Perspektif Server:**

```
[Server Log] Action: discardDrawn
[Server Log] Discarded: 9♥
[Server Log] Resolving ability: PEEK_ENEMY
```

Panel muncul: "👁️ PEEK ENEMY - Pilih kartu lawan untuk dilihat (3 detik)"

Budi mengamati kartu-kartu Andi. Dia tidak tahu mana yang berharga. Tapi dia ingat posisi 1 adalah kartu yang dia swap ke Andi. Dia penasaran kartu apa yang Andi miliki di sana.

Budi mengklik kartu posisi 1 milik Andi.

**Animasi 3D:**

Kartu tersebut terbuka, tapi hanya untuk Budi.

**Kartu terbuka untuk Budi: K♣ (King Sekop)**

"Haha!" Budi tersenyum dalam hati. "Andi dapat King! Itu 13 poin! Dan dia tahu itu sekarang karena saya lihat dia pakai ability PEEK_SELF sebelumnya."

Budi sekarang punya informasi berharga: Andi punya kartu tinggi di posisi 1. Ini bisa berguna nanti.

**Perspektif Server:**

```
[Server Log] PEEK_ENEMY executed
[Server Log] Player p_9921 peeked p_7742's card at index 1
[Server Log] Card revealed to p_9921 only: K♣ (value: 13)
[Server Log] p_7742 is NOT notified of this peek
```

Giliran kembali ke Andi.

### Skenario 4.2: Strategi Andi - Mengurangi Kerugian

Andi melihat discard pile sekarang: **9♥** di atas. Dia tahu Budi bisa peek kartu lawan dengan 9 atau 10.

"Budi mungkin sudah melihat kartu saya," pikir Andi khawatir. "Apalagi King saya di posisi 1."

Andi mengambil dari deck.

**Kartu: Joker 🃏**

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Player: p_7742
[Server Log] Card drawn: Joker (value: 0, actionType: NONE)
```

Andi hampir melompat dari kursinya. Joker! Nilainya 0 (di COSTUME_1 Classic). Ini kartu terbaik kedua setelah Red Kings yang bernilai -1.

"Ini kesempatan saya!" Andi berpikir cepat. "Saya harus swap Joker dengan King saya!"

Andi mengklik Joker dan menyeretnya ke kartu posisi 1 (yang berisi King♣).

**Animasi Swap:**

Joker bergerak ke posisi 1. King♣ bergerak ke discard pile.

**Perspektif Server:**

```
[Server Log] Action: swapCard
[Server Log] Player: p_7742
[Server Log] Hand index: 1
[Server Log] Replaced: K♣ (value: 13)
[Server Log] New card at pos 1: Joker (value: 0)
[Server Log] Discard pile: top = K♣
[Server Log] King discarded - ability SEE_AND_SWAP triggered
```

Panel muncul untuk Andi: "👁️ SEE AND SWAP - Lihat dan tukar kartu"

"Luar biasa!" pikir Andi. "King yang saya buang memicu ability SEE_AND_SWAP! Saya bisa melihat dan menukar!"

### Skenario 4.3: SEE_AND_SWAP - Keuntungan Ganda

Andi sekarang harus memilih:

1. Kartu miliknya sendiri untuk ditukar
2. Kartu lawan untuk ditukar
3. Setelah memilih, kedua kartu akan terbuka
4. Dia bisa memutuskan apakah akan swap atau cancel

Andi melihat kartunya:

- Pos 0: Ace♠ (1 poin) - Bagus, jangan tukar
- Pos 1: Joker🃏 (0 poin) - Sangat bagus, jangan tukar
- Pos 2: ???
- Pos 3: ???

"Saya akan tukar salah satu kartu misteri saya dengan kartu Budi," pikir Andi.

Andi memilih kartu posisi 2 miliknya.

**Perspektif Server:**

```
[Server Log] SEE_AND_SWAP step 1: Own card selected
[Server Log] Index: 2
```

Panel meminta: "Pilih kartu lawan."

Andi mengamati kartu-kartu Budi. Dia ingat bahwa Budi mendapat Queen♣ sebelumnya dan menyimpannya. Dia juga ingat Budi melakukan BLIND SWAP dan menukar sesuatu.

"Saya akan ambil kartu posisi 3 Budi," pikir Andi. "Itu kartu yang belum terjamah sejak awal."

Andi mengklik kartu posisi 3 milik Budi.

**Animasi 3D Dramatis:**

Kedua kartu (Andi pos 2 dan Budi pos 3) melayang ke tengah meja. Kali ini, berbeda dengan BLIND SWAP, kedua kartu TERBUKA dengan animasi flip yang megah.

**Kartu Andi pos 2: 10♦ (10 Hati)** - Nilai 10
**Kartu Budi pos 3: 3♠ (3 Sekop)** - Nilai 3

Panel sekarang menunjukkan kedua kartu dengan jelas:

```
┌─────────────────────────┐
│  👁️ SEE AND SWAP        │
├───────────┬─────────────┤
│  YOUR CARD│ ENEMY CARD  │
│   10♦     │    3♠       │
│  Value:10 │  Value:3    │
└───────────┴─────────────┘
    [CONFIRM SWAP] [CANCEL]
```

Andi melihat pilihan: kartunya sendiri bernilai 10, kartu Budi bernilai 3. Jika dia swap, dia akan mendapat kartu lebih rendah (bagus!) dan memberikan kartu tinggi ke Budi.

Tapi tunggu, itu berarti Budi akan mendapat kartu 10 yang buruk. Tapi di Kabul, tujuan adalah memiliki skor TERENDAH. Jadi Andi harus swap!

Andi mengklik "CONFIRM SWAP".

**Perspektif Server:**

```
[Server Log] SEE_AND_SWAP confirmed
[Server Log] Swapping:
[Server Log]   Andi's pos 2: 10♦ → Budi's pos 3
[Server Log]   Budi's pos 3: 3♠ → Andi's pos 2
[Server Log] Swap completed
```

**Animasi Final:**

Kedua kartu kembali ke posisi baru mereka dengan animasi slide smooth. Kartu 3♠ sekarang di tangan Andi (pos 2), dan kartu 10♦ sekarang di tangan Budi (pos 3).

Di layar Budi, dia hanya melihat kartunya bergerak. Dia tidak tahu nilai kartu yang baru masuk ke posisi 3 miliknya. Tapi dia tahu Andi baru saja melakukan SEE_AND_SWAP, yang berarti Andi tahu persis nilai kedua kartu.

"Andi pasti dapat kartu bagus dari saya," Budi menebak dengan benar.

---

## Bab 5: Persaingan Memanas (Late Game)

### Skenario 5.1: Budi Merespons

Discard pile sekarang menunjukkan **K♣** (King Sekop) di atas. Budi melihat ini dengan heran.

"Andi membuang King!" pikir Budi. "Berarti dia dapat kartu lebih baik darinya. Mungkin Joker atau Red King."

Budi mengambil dari deck.

**Kartu: 7♣ (7 Sekop)**

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Card: 7♣ (value: 7, actionType: PEEK_SELF)
```

Budi melihat 7♣. Nilainya 7, cukup baik. Ability PEEK_SELF bisa berguna untuk mengumpulkan informasi.

Budi memutuskan untuk swap 7♣ dengan kartu posisi 0 miliknya.

**Perspektif Server:**

```
[Server Log] Action: swapCard
[Server Log] Replaced card at pos 0: ??? → discard
[Server Log] New card: 7♣
[Server Log] Resolving ability: PEEK_SELF
```

Panel muncul untuk Budi: "Pilih kartu untuk dilihat."

Budi memilih kartu posisi 3 miliknya - yang baru saja didapat dari swap dengan Andi.

**Kartu terbuka untuk Budi: 10♦ (10 Hati)**

"Buset!" Budi hampir mengumpat. "Andi kasih saya kartu 10! Tinggi banget!"

Budi sekarang tahu posisi 3 miliknya berisi 10. Itu buruk untuk skor akhir. Dia harus buang itu secepatnya.

Giliran kembali ke Andi.

### Skenario 5.2: SLAP! - Momen Mendebarkan

Ini giliran Andi. Dia melihat discard pile: **7♣** di atas.

Andi melihat ke tangannya. Dia punya:

- Pos 0: Ace♠ (1)
- Pos 1: Joker🃏 (0)
- Pos 2: 3♠ (3) - dari swap sebelumnya
- Pos 3: ??? (belum diketahui)

"Tunggu," Andi tiba-tiba teringat. "Saya punya 7 juga! Di posisi 3!"

Andi melihat kartu tertutup di posisi 3. Dia lupa nilainya dari fase MEMORIZE, tapi dia punya feeling itu adalah 7.

Ini saatnya untuk SLAP!

Andi mengarahkan kursor ke kartu posisi 3, lalu menekan dan menahan tombol mouse (long-press selama 500ms).

**Animasi 3D SLAP:**

Kartu tersebut melesat ke tengah meja dengan kecepatan tinggi, meninggalkan jejak partikel berkilauan. Kartu tersebut mendarat di discard pile di atas 7♣.

**Perspektif Server:**

```
[Server Log] Action: SLAP
[Server Log] Player: p_7742
[Server Log] Hand index: 3
[Server Log] Card at index 3: 7♦ (rank: 7)
[Server Log] Top discard rank: 7
[Server Log] MATCH! Rank sama (7 = 7)
[Server Log] Result: SUCCESS
[Server Log] Card 7♦ removed from hand
[Server Log] Added to discard pile
[Server Log] Andi hand count: 3 cards
```

**Suara efek:** _SLAP!_ (suara kartu ditampar keras ke meja)

Di layar, teks besar muncul: "🎯 SLAP MATCH! Kartu berhasil dibuang!"

Andi berhasil! Dia mengurangi jumlah kartunya dari 4 menjadi 3. Ini sangat menguntungkan karena skor akhir adalah total nilai semua kartu di tangan. Semakin sedikit kartu, semakin kecil potensi skor.

Budi melihat ini dari seberang meja. Dia hanya melihat animasi SLAP dan kartu Andi berkurang. Tapi dia tidak tahu nilai kartu yang dibuang.

"Andi berhasil SLAP," pikir Budi. "Dia punya kartu match dengan discard pile. Berarti dia punya 7."

Informasi ini berguna. Budi sekarang tahu Andi mungkin punya kartu lain yang tidak terlalu tinggi.

### Skenario 5.3: Keputusan Besar - Memanggil KABUL

Beberapa giliran berlalu. Permainan berlanjut dengan saling swap dan discard.

Sekarang giliran Andi lagi. Dia melihat ke tangannya (sekarang 3 kartu):

- Pos 0: Ace♠ (1)
- Pos 1: Joker🃏 (0)
- Pos 2: 3♠ (3)

Total yang dia ketahui: 1 + 0 + 3 = 4 poin.

"Ini sangat rendah!" pikir Andi. "Saya yakin Budi punya total lebih tinggi dari 4."

Ini saatnya memanggil KABUL.

Andi menggerakkan kursor ke tombol "📣 KABUL" yang ada di samping tombol Draw. Tombol tersebut berwarna emas dan berkilau. Dia mengkliknya.

**Dialog Konfirmasi:**

```
┌─────────────────────────────┐
│  📣 PANGGIL KABUL?          │
│                             │
│  Memanggil KABUL akan       │
│  mengakhiri ronde.          │
│  Semua pemain mendapat      │
│  1 giliran terakhir.        │
│                             │
│  [YA, PANGGIL KABUL] [BATAL]│
└─────────────────────────────┘
```

Andi mengklik "YA, PANGGIL KABUL".

**Perspektif Server:**

```
[Server Log] ╔════════════════════════════════════╗
[Server Log] ║      K A B U L  C A L L E D !      ║
[Server Log] ╚════════════════════════════════════╝
[Server Log] Player: p_7742 (Andi_Pro)
[Server Log] Setting kabulCaller = "p_7742"
[Server Log] Final turns remaining: 1 (2 players - 1)
[Server Log] Player p_7742 hand LOCKED
[Server Log] Broadcasting KABUL_CALL event
```

**Efek Visual Dramatis:**

_Sebuah klakson terdengar: BRAAAK!_

Cahaya kuning keemasan menyinari avatar Andi. Teks besar muncul di tengah layar kedua pemain:

```
╔═══════════════════════════════════════╗
║                                       ║
║   📣  A N D I _ P R O  M E M A N G G I L  ║
║                                       ║
║           K A B U L ! ! !             ║
║                                       ║
║     1 Giliran Terakhir untuk Budi     ║
║                                       ║
╚═══════════════════════════════════════╝
```

Avatar Andi mendapatkan ikon gembok emas di atas kepalanya, menandakan tangannya terkunci dan tidak bisa diubah lagi.

Budi melihat ini dengan kaget. "Andi sudah KABUL! Dia yakin skornya rendah. Saya harus maksimalkan giliran terakhir ini!"

---

## Bab 6: Final Turn - Detik-detik Terakhir

### Skenario 6.1: Budi Bertarung untuk Bertahan

Layar Budi menunjukkan indikator merah besar: "⚠️ FINAL TURN - Lakukan yang terbaik!"

Budi melihat tangannya:

- Pos 0: 7♣ (7)
- Pos 1: ???
- Pos 2: Q♣ (13) - yang dia simpan sejak awal
- Pos 3: 10♦ (10) - yang dia dapat dari Andi

Total yang dia ketahui: 7 + 13 + 10 = 30. Sangat tinggi!

"Saya harus segera buang kartu tinggi ini," Budi panik.

Budi mengambil dari deck.

**Kartu: A♥ (Ace Hati)**

**Perspektif Server:**

```
[Server Log] Action: drawCard (FINAL TURN)
[Server Log] Player: p_9921
[Server Log] Card: A♥ (value: 1)
```

"Ace! Nilainya 1!" Budi gembira. "Saya akan swap ini dengan Queen saya!"

Budi swap A♥ dengan kartu posisi 2 (Q♣).

**Animasi Swap:**

Queen♣ bergerak ke discard pile. Ace♥ mengambil posisi 2.

**Perspektif Server:**

```
[Server Log] Action: swapCard
[Server Log] Replaced: Q♣ (value: 13)
[Server Log] New card: A♥ (value: 1)
[Server Log] Queen discarded - ability SEE_AND_SWAP triggered
```

Panel muncul untuk Budi: "SEE AND SWAP - Lihat dan tukar."

Budi harus bertindak cepat. Dia memilih kartu posisi 3 miliknya (10♦) untuk ditukar.

"Saya akan tukar 10 ini dengan kartu Andi," pikir Budi.

Budi memilih kartu posisi 1 milik Andi.

**Animasi Reveal:**

Kedua kartu terbuka:

- Budi pos 3: 10♦ (10)
- Andi pos 1: Joker🃏 (0)

"JOKER!" Budi mata melotot. "Andi punya Joker! Nilainya 0!"

Panel menunjukkan:

```
YOUR CARD: 10♦ (10)
ENEMY CARD: Joker🃏 (0)
```

Tentu saja Budi akan swap! Dia akan memberikan 10 ke Andi dan mengambil Joker 0!

Budi klik "CONFIRM SWAP".

**Perspektif Server:**

```
[Server Log] SEE_AND_SWAP confirmed
[Server Log] Swapping:
[Server Log]   Budi's pos 3: 10♦ → Andi's pos 1
[Server Log]   Andi's pos 1: Joker🃏 → Budi's pos 3
```

Tapi tunggu! Ingat bahwa Andi sudah memanggil KABUL!

**Perspektif Server:**

```
[Server Log] WARNING: Target player p_7742 has called KABUL
[Server Log] Target hand is LOCKED
[Server Log] Swap proceeding anyway (valid mechanic)
[Server Log] Andi's hand updated despite being locked
```

Jadi swap berhasil! Budi berhasil merebut Joker dari Andi!

Tapi ada twist: karena Andi sudah KABUL, tangannya seharusnya terkunci. Tapi ability SEE_AND_SWAP masih bisa menukar kartu ke tangannya. Ini adalah strategi terakhir yang bisa dilakukan lawan.

**Perspektif Server - Final Turn Complete:**

```
[Server Log] Final turn completed for p_9921
[Server Log] Final turns remaining: 0
[Server Log] ╔════════════════════════════════════╗
[Server Log] ║         G A M E   E N D E D        ║
[Server Log] ╚════════════════════════════════════╝
[Server Log] Calculating final scores...
```

---

## Bab 7: Akhir Permainan - Pengungkapan

### Skenario 7.1: Semua Kartu Terbuka

_Suasana ruangan berubah. Cahaya menjadi lebih terang dan hangat. Semua kartu di meja, baik milik Andi maupun Budi, mulai berputar dan terbuka satu per satu dengan animasi dramatis._

Layar kedua pemain menunjukkan hasil akhir:

**Tangan Andi (Andi_Pro):**

- Pos 0: Ace♠ = **1 poin**
- Pos 1: 10♦ = **10 poin** (dari swap Budi terakhir)
- Pos 2: 3♠ = **3 poin**

**Total Andi: 1 + 10 + 3 = 14 poin**

**Tangan Budi (Budi_Gaming):**

- Pos 0: 7♣ = **7 poin**
- Pos 1: ??? = **? poin**
- Pos 2: A♥ = **1 poin**
- Pos 3: Joker🃏 = **0 poin** (dari swap Andi)

**Ternyata pos 1 Budi adalah 2♣ = 2 poin**

**Total Budi: 7 + 2 + 1 + 0 = 10 poin**

**Perspektif Server:**

```
[Server Log] Final Score Calculation:
[Server Log] ─────────────────────────────────
[Server Log] Player: Andi_Pro (p_7742)
[Server Log]   Ace♠: 1
[Server Log]   10♦: 10
[Server Log]   3♠: 3
[Server Log]   TOTAL: 14
[Server Log]
[Server Log] Player: Budi_Gaming (p_9921)
[Server Log]   7♣: 7
[Server Log]   2♣: 2
[Server Log]   A♥: 1
[Server Log]   Joker🃏: 0
[Server Log]   TOTAL: 10
[Server Log] ─────────────────────────────────
[Server Log] WINNER: Budi_Gaming with 10 points!
[Server Log] Difference: 4 points
```

### Skenario 7.2: Pemenang Diumumkan

Layar menunjukkan:

```
╔═══════════════════════════════════════════╗
║                                           ║
║         🏆  P E M E N A N G  🏆          ║
║                                           ║
║         B U D I _ G A M I N G            ║
║                                           ║
║           Skor: 10 poin                  ║
║                                           ║
║    (Andi_Pro: 14 poin)                   ║
║                                           ║
╚═══════════════════════════════════════════╝
```

Avatar Budi diberi mahkota emas virtual dan konfetti berjatuhan di sekitarnya. Avatar Andi bertepuk tangan dengan animasi sportif.

Budi tertawa gembira di mikrofonnya. "Yes! Strategi terakhir berhasil! Ambil Joker-nya!"

Andi tertawa pasrah. "Haha, saya kira sudah aman dengan KABUL. Ternyata SEE_AND_SWAP masih bisa ganggu saya."

### Skenario 7.3: Refleksi dan Rematch

Layar menunjukkan opsi:

- [PLAY AGAIN] - Hanya untuk host (Budi)
- [BACK TO LOBBY]

Budi mengklik "PLAY AGAIN".

**Perspektif Server:**

```
[Server Log] Host p_9921 initiated rematch
[Server Log] Resetting game state...
[Server Log] Keeping players: [p_9921, p_7742]
[Server Log] Reinitializing deck...
[Server Log] Setting phase to MEMORIZE
[Server Log] Broadcasting game_restart event
```

Layar kembali ke fase MEMORIZE. Permainan baru dimulai.

Andi berkata, "Oke, kali ini saya tidak akan gegabah panggil KABUL. Saya tunggu sampai benar-benar aman."

Budi tersenyum. "Saya juga punya strategi baru. Ayo kita lihat siapa yang menang kali ini!"

---

## Edge Cases dan Skenario Khusus

### Edge Case 1: Disconnect Saat Giliran

**Skenario:**

Andi sedang dalam fase memilih ability. Tiba-tiba, koneksi internetnya putus. Avatar-nya membeku. Layar Budi menunjukkan indikator "⚠️ Andi_Pro - Connection Lost".

**Perspektif Server:**

```
[Server Log] Connection lost detected for p_7742
[Server Log] Player marked as disconnected
[Server Log] isConnected = false
[Server Log] Pending action timeout: 15 seconds
[Server Log] If no reconnection, auto-skip action
```

Setelah 15 detik tanpa reconnection:

```
[Server Log] Timeout reached for p_7742
[Server Log] Auto-skipping pending action
[Server Log] Advancing turn to next player
```

Jika Andi reconnect dalam waktu singkat:

```
[Server Log] Player p_7742 reconnected
[Server Log] isConnected = true
[Server Log] Syncing current state...
[Server Log] Game continues normally
```

**Implementasi di Roblox:**

Gunakan `PlayerRemoving` event untuk mendeteksi disconnect. Simpan state pemain di server selama 30 detik untuk memungkinkan reconnect.

### Edge Case 2: SLAP Timing - Race Condition

**Skenario:**

Discard pile menunjukkan **5♥**. Kedua pemain, Andi dan Budi, masing-masing punya 5 di tangan mereka.

Budi melakukan SLAP duluan dengan menekan E + klik kartu.

Andi, 50ms kemudian, juga melakukan SLAP.

**Perspektif Server:**

```
[Server Log] SLAP received from p_9921 at timestamp: 1709123501000
[Server Log] SLAP received from p_7742 at timestamp: 1709123501050
[Server Log] Processing first SLAP (Budi)...
[Server Log] Match confirmed for p_9921
[Server Log] Card removed from Budi's hand
[Server Log] Discard pile updated: new top = 5♦
[Server Log] Processing second SLAP (Andi)...
[Server Log] ERROR: Top discard changed!
[Server Log] Andi's card rank: 5
[Server Log] New top discard rank: 5 (different card)
[Server Log] Actually... still match!
[Server Log] But Budi's SLAP already consumed the slot
[Server Log] Result: Only first SLAP processed
```

Solusi: Implementasi server-side queue untuk SLAP. Hanya SLAP pertama yang diproses dalam window 100ms.

**Kode Implementasi (Konsep):**

```lua
-- Roblox ServerScript
local slapQueue = {}
local SLAP_WINDOW_MS = 100

function processSlap(playerId, cardIndex)
    local timestamp = tick()

    -- Check if there's a pending slap in window
    for _, slap in ipairs(slapQueue) do
        if (timestamp - slap.timestamp) < (SLAP_WINDOW_MS / 1000) then
            -- Reject - someone else slapped first
            return { success = false, reason = "Another player slapped first" }
        end
    end

    -- Add to queue
    table.insert(slapQueue, {
        playerId = playerId,
        cardIndex = cardIndex,
        timestamp = timestamp
    })

    -- Process slap
    return validateAndExecuteSlap(playerId, cardIndex)
end
```

### Edge Case 3: Deck Habis

**Skenario:**

Permainan sudah berjalan lama. Banyak kartu dibuang. Deck menyisakan 1 kartu.

Andi mengambil kartu terakhir.

**Perspektif Server:**

```
[Server Log] Action: drawCard
[Server Log] Deck remaining: 1
[Server Log] Card drawn: 3♦
[Server Log] Deck is now EMPTY
[Server Log] Reshuffling discard pile (except top card)...
[Server Log] New deck created with 45 cards
```

Saat deck habis, sistem otomatis mengambil semua kartu dari discard pile (kecuali kartu teratas), mengocoknya, dan membuat deck baru.

### Edge Case 4: KABUL dengan Kartu Kosong

**Skenario Ekstrem:**

Andi berhasil melakukan SLAP berkali-kali dan mengurangi tangannya menjadi 0 kartu. Lalu dia memanggil KABUL.

**Perspektif Server:**

```
[Server Log] Action: callKabul
[Server Log] Player: p_7742
[Server Log] Checking hand count...
[Server Log] Hand count: 0
[Server Log] Score calculation: 0 (empty hand)
[Server Log] This is the BEST possible score!
```

Dalam kasus ini, Andi hampir pasti menang karena skor 0 adalah yang terendah.

---

## Diagram Alur dan Placeholder Screenshot

### Diagram 1: Alur Game Utama

```
[PLACEHOLDER SCREENSHOT]
File: screenshots/01-lobby-waiting.png
Deskripsi: Tampilan lobby dengan daftar room yang tersedia
```

```
┌─────────────────────────────────────────────────────────────┐
│                      ALUR GAME KABUL                        │
└─────────────────────────────────────────────────────────────┘

    LOBBY
      │
      ▼
┌─────────────┐    Join/Create
│   WAITING   │◄─────────────────────┐
│   (Room)    │                      │
└──────┬──────┘                      │
       │                             │
       │ Host Start                  │ Player Leave
       ▼                             │
┌─────────────┐     3 detik         │
│  MEMORIZE   │────────────────►    │
│  (See 2     │                     │
│   cards)    │                     │
└──────┬──────┘                     │
       │                            │
       ▼                            │
┌─────────────┐                     │
│   PLAYING   │─────────────────────┘
│  (Turn Loop)│    KABUL Called
└──────┬──────┘
       │
       │ End Condition Met
       ▼
┌─────────────┐
│    ENDED    │
│ (Show Score)│
└──────┬──────┘
       │
       ▼
  [Rematch?] ──► Kembali ke MEMORIZE
       │
       ▼
   Back to LOBBY
```

### Diagram 2: State Machine Kartu

```
[PLACEHOLDER SCREENSHOT]
File: screenshots/02-card-states.png
Deskripsi: Visualisasi kartu dalam berbagai state (face-down, face-up, peeked)
```

```
┌─────────────────────────────────────────────────────────────┐
│                   STATE MACHINE KARTU                       │
└─────────────────────────────────────────────────────────────┘

┌──────────────┐
│  FACE DOWN   │◄──────────────────────────────────────────┐
│  (Tertutup)  │                                           │
└──────┬───────┘                                           │
       │                                                    │
       │ Draw / Deal / Swap                                 │
       ▼                                                    │
┌──────────────┐     3 detik / Action complete             │
│   FACE UP    │───────────────────────────────────────────┤
│  (Terbuka)   │                                           │
└──────┬───────┘                                           │
       │                                                    │
       │ PEEK_SELF / PEEK_ENEMY                             │
       ▼                                                    │
┌──────────────┐     3 detik timeout                       │
│   PEEKED     │───────────────────────────────────────────┘
│  (Dilihat    │
│   sementara) │
└──────────────┘
```

### Diagram 3: Layout 3D Table

```
[PLACEHOLDER SCREENSHOT]
File: screenshots/03-3d-table-layout.png
Deskripsi: Tampilan meja 3D dari sudut pandang pemain
```

```
┌─────────────────────────────────────────────────────────────┐
│                LAYOUT MEJA 3D (2 PLAYER)                   │
└─────────────────────────────────────────────────────────────┘

                    ┌─────────────────┐
                    │   BUDI_GAMING   │
                    │    (LAWAN)      │
                    │   [K][K][K][K]  │  ◄── Kartu tertutup
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
    ┌────┴────┐       ┌──────┴──────┐      ┌────┴────┐
    │  DECK   │       │   DISCARD   │      │  SLAP   │
    │  (46)   │       │    7♦       │      │  TIMER  │
    │  [???]  │       │             │      │         │
    └────┬────┘       └─────────────┘      └─────────┘
         │
         │                    ┌─────────────────┐
         │                    │    ANDI_PRO     │
         └────────────────────►     (ANDA)      │
                              │   [?][?][?][?]  │  ◄── 2 terbuka saat MEMORIZE
                              │   A♠   5♥       │
                              └─────────────────┘

LEGENDA:
[K] = Kartu tertutup (Back kartu)
[?] = Kartu unknown (bisa terbuka/tertutup tergantung phase)
A♠ = Ace Sekop (contoh kartu terlihat)
```

### Diagram 4: Sequence SLAP

```
[PLACEHOLDER SCREENSHOT]
File: screenshots/04-slap-sequence.png
Deskripsi: Animasi kartu saat melakukan SLAP ke discard pile
```

```
┌─────────────────────────────────────────────────────────────┐
│              SEQUENCE DIAGRAM - SLAP ACTION                 │
└─────────────────────────────────────────────────────────────┘

Player          Client          Server          GameState
  │               │               │                 │
  │ Long-press    │               │                 │
  │ on Card       │               │                 │
  │──────────────►│               │                 │
  │               │               │                 │
  │               │  slap(cardIdx)│                 │
  │               │──────────────►│                 │
  │               │               │                 │
  │               │               │ Validate match  │
  │               │               │ with topDiscard │
  │               │               │────────────────►│
  │               │               │                 │
  │               │               │◄────────────────│
  │               │               │  Match? (Y/N)   │
  │               │               │                 │
  │               │◄──────────────│                 │
  │               │ Result         │                 │
  │               │                 │                 │
  │◄──────────────│                │                 │
  │ Animation     │               │                 │
  │ + Sound       │               │                 │
  │               │               │                 │
  │ [SUCCESS]     │               │                 │
  │ Card removed  │               │                 │
  │               │               │                 │
  │ [FAIL]        │               │                 │
  │ +1 Card       │               │                 │
  │ Penalty       │               │                 │
```

### Diagram 5: Alur Ability Kartu

```
[PLACEHOLDER SCREENSHOT]
File: screenshots/05-ability-flow.png
Deskripsi: Panel ability dengan pilihan kartu yang bisa diinteraksi
```

```
┌─────────────────────────────────────────────────────────────┐
│              FLOWCHART ABILITY KARTU                        │
└─────────────────────────────────────────────────────────────┘

Discard Card
     │
     ▼
┌─────────────────────────────────────────────────────────────┐
│                    ABILITY CHECK                            │
└─────────────────────────────────────────────────────────────┘
     │
     ├──────────────────┬──────────────────┬──────────────────┐
     │                  │                  │                  │
     ▼                  ▼                  ▼                  ▼
┌─────────┐      ┌─────────┐      ┌─────────────┐     ┌─────────────┐
│ 7, 8    │      │ 9, 10   │      │     J       │     │   Q, K      │
│PEEK_SELF│      │PEEK_ENEMY      │ BLIND_SWAP  │     │SEE_AND_SWAP │
└────┬────┘      └────┬────┘      └──────┬──────┘     └──────┬──────┘
     │                │                  │                    │
     ▼                ▼                  ▼                    ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐
│ Pilih kartu     │  │ Pilih kartu     │  │ Pilih kartu     │  │ Pilih kartu │
│ sendiri         │  │ lawan           │  │ sendiri         │  │ sendiri     │
└────────┬────────┘  └────────┬────────┘  └────────┬────────┘  └──────┬──────┘
         │                    │                    │                  │
         ▼                    ▼                    ▼                  ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────┐
│ Reveal 3 detik  │  │ Reveal 3 detik  │  │ Pilih kartu     │  │ Pilih kartu │
│ (Hanya kamu)    │  │ (Hanya kamu)    │  │ lawan           │  │ lawan       │
└─────────────────┘  └─────────────────┘  └────────┬────────┘  └──────┬──────┘
                                                   │                  │
                                                   ▼                  ▼
                                            ┌─────────────────┐  ┌─────────────┐
                                            │ SWAP (No reveal)│  │ Reveal BOTH │
                                            └─────────────────┘  └──────┬──────┘
                                                                        │
                                                                        ▼
                                                                 ┌─────────────┐
                                                                 │ Confirm?    │
                                                                 │ [Y]    [N]  │
                                                                 └──────┬──────┘
                                                                        │
                                                              ┌─────────┴─────────┐
                                                              ▼                   ▼
                                                           [YES]               [NO]
                                                              │                   │
                                                              ▼                   ▼
                                                           SWAP               CANCEL
                                                              │                   │
                                                              └─────────┬─────────┘
                                                                        ▼
                                                                  END TURN
```

---

## Detail Teknis Interaksi 3D

### Sistem Input Roblox

```lua
-- Contoh implementasi ClickDetector untuk kartu
local card = workspace.CardModel
local clickDetector = Instance.new("ClickDetector", card)

clickDetector.MouseClick:Connect(function(player)
    -- Kirim event ke server
    local playerId = player.UserId
    local cardPosition = card:GetAttribute("Position") -- 0, 1, 2, atau 3

    -- Validasi di server
    game.ReplicatedStorage.CardClicked:FireServer(playerId, cardPosition)
end)
```

### Animasi Kartu dengan TweenService

```lua
-- Animasi flip kartu
local function flipCard(cardModel, showFace)
    local tweenService = game:GetService("TweenService")

    -- Rotate 90 degrees (hide current face)
    local halfFlip = tweenService:Create(
        cardModel,
        TweenInfo.new(0.15, Enum.EasingStyle.Quad),
        { Rotation = Vector3.new(0, 90, 0) }
    )

    -- Swap face/face-up texture
    halfFlip.Completed:Connect(function()
        if showFace then
            cardModel.Mesh.TextureId = cardModel:GetAttribute("FaceTexture")
        else
            cardModel.Mesh.TextureId = cardModel:GetAttribute("BackTexture")
        end

        -- Rotate back to 0
        local fullFlip = tweenService:Create(
            cardModel,
            TweenInfo.new(0.15, Enum.EasingStyle.Quad),
            { Rotation = Vector3.new(0, 0, 0) }
        )
        fullFlip:Play()
    end)

    halfFlip:Play()
end
```

### Proximity Prompt untuk Deck/Discard

```lua
-- Prompt untuk mengambil kartu dari deck
local deck = workspace.Deck
local prompt = Instance.new("ProximityPrompt", deck)

prompt.ObjectText = "Deck"
prompt.ActionText = "Draw Card"
prompt.KeyboardKeyCode = Enum.KeyCode.E
prompt.HoldDuration = 0
prompt.MaxActivationDistance = 10

prompt.Triggered:Connect(function(player)
    game.ReplicatedStorage.DrawCard:FireServer(player.UserId, "deck")
end)
```

---

## Kesimpulan

Dokumen ini telah menggambarkan satu sesi permainan Kabul lengkap dari perspektif pengalaman pengguna di Roblox. Dari lobby yang ramai, fase MEMORIZE yang tegang, pertarungan strategis di fase PLAYING, hingga momen KABUL yang dramatis dan akhir permainan yang memuaskan.

Kunci dari pengalaman Kabul yang baik adalah:

1. **Immersive 3D Environment** - Meja, kartu, dan animasi harus terasa nyata
2. **Feedback Visual yang Jelas** - Setiap aksi harus memberikan respons visual yang jelas
3. **Sound Design yang Pas** - Efek suara memperkuat setiap momen penting
4. **Smooth Networking** - Sinkronisasi real-time tanpa lag yang mengganggu
5. **Edge Case Handling** - Disconnect, race condition, dan situasi anomali harus ditangani dengan elegan

Dengan mengikuti skenario di atas, tim development dapat membayangkan dan mengimplementasikan pengalaman Kabul yang autentik dan menyenangkan di platform Roblox.

---

## Lampiran: Daftar Placeholder Screenshot

| No  | Filename                                | Deskripsi                               |
| --- | --------------------------------------- | --------------------------------------- |
| 1   | `screenshots/01-lobby-waiting.png`      | Tampilan lobby dengan room list         |
| 2   | `screenshots/02-room-2player.png`       | Room dengan 2 pemain, menunggu start    |
| 3   | `screenshots/03-memorize-phase.png`     | Fase MEMORIZE, 2 kartu terbuka          |
| 4   | `screenshots/04-draw-card.png`          | Animasi mengambil kartu dari deck       |
| 5   | `screenshots/05-swap-animation.png`     | Animasi menukar kartu                   |
| 6   | `screenshots/06-ability-peek.png`       | Panel ability PEEK dengan kartu terbuka |
| 7   | `screenshots/07-ability-blindswap.png`  | BLIND SWAP dengan kartu tertutup        |
| 8   | `screenshots/08-ability-seeandswap.png` | SEE AND SWAP dengan reveal              |
| 9   | `screenshots/09-slap-success.png`       | SLAP berhasil dengan efek visual        |
| 10  | `screenshots/10-slap-fail.png`          | SLAP gagal dengan penalti               |
| 11  | `screenshots/11-kabul-called.png`       | Efek visual KABUL dipanggil             |
| 12  | `screenshots/12-final-turn.png`         | Indikator final turn                    |
| 13  | `screenshots/13-game-ended.png`         | Layar akhir dengan skor                 |
| 14  | `screenshots/14-winner-crown.png`       | Pemenang dengan mahkota emas            |
| 15  | `screenshots/15-3d-table-overview.png`  | Overview meja 3D dari atas              |

---

_Dokumen ini ditulis untuk keperluan development Kabul 3D Table Experience di Roblox. Semua skenario berdasarkan aturan resmi Kabul Game versi web yang telah teruji._

**Word Count Target: ~7500 kata**
