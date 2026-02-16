# Kabul Game Seed Data

Dokumen ini berisi data awal (seed data) untuk sistem permainan Kabul multiplayer. Data ini mencakup definisi lengkap 54 kartu, konfigurasi costume, pengaturan game, dan contoh state awal yang dapat digunakan untuk inisialisasi database atau pengujian sistem.

---

## Daftar Isi

1. [Definisi Kartu](#1-definisi-kartu)
2. [Konfigurasi Costume](#2-konfigurasi-costume)
3. [Konfigurasi Game](#3-konfigurasi-game)
4. [Contoh State Awal](#4-contoh-state-awal)
5. [Penjelasan Field](#5-penjelasan-field)

---

## 1. Definisi Kartu

Permainan Kabul menggunakan total **54 kartu**, terdiri dari 52 kartu standar (4 suit x 13 rank) ditambah 2 kartu Joker. Setiap kartu memiliki atribut value (nilai poin) dan actionType (kemampuan khusus saat dibuang).

### 1.1 Tabel Kartu Lengkap (54 Kartu)

| No  | ID     | Rank  | Suit | Display | Value (Classic) | Value (Reversed) | Action Type  |
| :-: | :----- | :---: | :--: | :-----: | :-------------: | :--------------: | :----------- |
|  1  | A♥     |   A   |  ♥   |   A♥    |        1        |        1         | NONE         |
|  2  | A♦     |   A   |  ♦   |   A♦    |        1        |        1         | NONE         |
|  3  | A♠     |   A   |  ♠   |   A♠    |        1        |        1         | NONE         |
|  4  | A♣     |   A   |  ♣   |   A♣    |        1        |        1         | NONE         |
|  5  | 2♥     |   2   |  ♥   |   2♥    |        2        |        2         | NONE         |
|  6  | 2♦     |   2   |  ♦   |   2♦    |        2        |        2         | NONE         |
|  7  | 2♠     |   2   |  ♠   |   2♠    |        2        |        2         | NONE         |
|  8  | 2♣     |   2   |  ♣   |   2♣    |        2        |        2         | NONE         |
|  9  | 3♥     |   3   |  ♥   |   3♥    |        3        |        3         | NONE         |
| 10  | 3♦     |   3   |  ♦   |   3♦    |        3        |        3         | NONE         |
| 11  | 3♠     |   3   |  ♠   |   3♠    |        3        |        3         | NONE         |
| 12  | 3♣     |   3   |  ♣   |   3♣    |        3        |        3         | NONE         |
| 13  | 4♥     |   4   |  ♥   |   4♥    |        4        |        4         | NONE         |
| 14  | 4♦     |   4   |  ♦   |   4♦    |        4        |        4         | NONE         |
| 15  | 4♠     |   4   |  ♠   |   4♠    |        4        |        4         | NONE         |
| 16  | 4♣     |   4   |  ♣   |   4♣    |        4        |        4         | NONE         |
| 17  | 5♥     |   5   |  ♥   |   5♥    |        5        |        5         | NONE         |
| 18  | 5♦     |   5   |  ♦   |   5♦    |        5        |        5         | NONE         |
| 19  | 5♠     |   5   |  ♠   |   5♠    |        5        |        5         | NONE         |
| 20  | 5♣     |   5   |  ♣   |   5♣    |        5        |        5         | NONE         |
| 21  | 6♥     |   6   |  ♥   |   6♥    |        6        |        6         | NONE         |
| 22  | 6♦     |   6   |  ♦   |   6♦    |        6        |        6         | NONE         |
| 23  | 6♠     |   6   |  ♠   |   6♠    |        6        |        6         | NONE         |
| 24  | 6♣     |   6   |  ♣   |   6♣    |        6        |        6         | NONE         |
| 25  | 7♥     |   7   |  ♥   |   7♥    |        7        |        7         | PEEK_SELF    |
| 26  | 7♦     |   7   |  ♦   |   7♦    |        7        |        7         | PEEK_SELF    |
| 27  | 7♠     |   7   |  ♠   |   7♠    |        7        |        7         | PEEK_SELF    |
| 28  | 7♣     |   7   |  ♣   |   7♣    |        7        |        7         | PEEK_SELF    |
| 29  | 8♥     |   8   |  ♥   |   8♥    |        8        |        8         | PEEK_SELF    |
| 30  | 8♦     |   8   |  ♦   |   8♦    |        8        |        8         | PEEK_SELF    |
| 31  | 8♠     |   8   |  ♠   |   8♠    |        8        |        8         | PEEK_SELF    |
| 32  | 8♣     |   8   |  ♣   |   8♣    |        8        |        8         | PEEK_SELF    |
| 33  | 9♥     |   9   |  ♥   |   9♥    |        9        |        9         | PEEK_ENEMY   |
| 34  | 9♦     |   9   |  ♦   |   9♦    |        9        |        9         | PEEK_ENEMY   |
| 35  | 9♠     |   9   |  ♠   |   9♠    |        9        |        9         | PEEK_ENEMY   |
| 36  | 9♣     |   9   |  ♣   |   9♣    |        9        |        9         | PEEK_ENEMY   |
| 37  | 10♥    |  10   |  ♥   |   10♥   |       10        |        10        | PEEK_ENEMY   |
| 38  | 10♦    |  10   |  ♦   |   10♦   |       10        |        10        | PEEK_ENEMY   |
| 39  | 10♠    |  10   |  ♠   |   10♠   |       10        |        10        | PEEK_ENEMY   |
| 40  | 10♣    |  10   |  ♣   |   10♣   |       10        |        10        | PEEK_ENEMY   |
| 41  | J♥     |   J   |  ♥   |   J♥    |       11        |        11        | BLIND_SWAP   |
| 42  | J♦     |   J   |  ♦   |   J♦    |       11        |        11        | BLIND_SWAP   |
| 43  | J♠     |   J   |  ♠   |   J♠    |       11        |        11        | BLIND_SWAP   |
| 44  | J♣     |   J   |  ♣   |   J♣    |       11        |        11        | BLIND_SWAP   |
| 45  | Q♥     |   Q   |  ♥   |   Q♥    |       12        |        12        | SEE_AND_SWAP |
| 46  | Q♦     |   Q   |  ♦   |   Q♦    |       12        |        12        | SEE_AND_SWAP |
| 47  | Q♠     |   Q   |  ♠   |   Q♠    |       12        |        12        | SEE_AND_SWAP |
| 48  | Q♣     |   Q   |  ♣   |   Q♣    |       12        |        12        | SEE_AND_SWAP |
| 49  | K♥     |   K   |  ♥   |   K♥    |        0        |        -1        | SEE_AND_SWAP |
| 50  | K♦     |   K   |  ♦   |   K♦    |        0        |        -1        | SEE_AND_SWAP |
| 51  | K♠     |   K   |  ♠   |   K♠    |       13        |        13        | SEE_AND_SWAP |
| 52  | K♣     |   K   |  ♣   |   K♣    |       13        |        13        | SEE_AND_SWAP |
| 53  | Joker1 | Joker | null |  Joker  |       -1        |        0         | NONE         |
| 54  | Joker2 | Joker | null |  Joker  |       -1        |        0         | NONE         |

### 1.2 Penjelasan Nilai Kartu

**Sistem Penilaian Kartu:**

|         Rank         | Nilai Dasar | Catatan Khusus                          |
| :------------------: | :---------: | :-------------------------------------- |
|        Joker         |  -1 atau 0  | Bergantung pada costume yang dipilih    |
|       A (Ace)        |      1      | Nilai terendah untuk kartu angka        |
|         2-10         | Face Value  | Sesuai angka pada kartu (2=2, 3=3, dst) |
|       J (Jack)       |     11      | Memiliki kemampuan BLIND_SWAP           |
|      Q (Queen)       |     12      | Memiliki kemampuan SEE_AND_SWAP         |
|  K♥, K♦ (Red Kings)  |  0 atau -1  | King merah, nilai spesial               |
| K♠, K♣ (Black Kings) |     13      | King hitam, nilai tertinggi             |

**Tujuan Permainan:**
Player dengan total nilai kartu di tangan **terendah** memenangkan permainan. Red Kings dan Joker (tergantung costume) adalah kartu premium karena memiliki nilai negatif atau nol.

### 1.3 Penjelasan Kemampuan Kartu (Action Types)

| Action Type      |     Rank      | Deskripsi                  | Mekanisme                                                                                                |
| :--------------- | :-----------: | :------------------------- | :------------------------------------------------------------------------------------------------------- |
| **NONE**         | A, 2-6, Joker | Tidak ada kemampuan khusus | Kartu langsung masuk ke discard pile tanpa efek tambahan                                                 |
| **PEEK_SELF**    |     7, 8      | Intip kartu sendiri        | Player dapat memilih 1 kartu dari tangan sendiri untuk dilihat selama 3 detik                            |
| **PEEK_ENEMY**   |     9, 10     | Intip kartu lawan          | Player dapat memilih 1 kartu dari tangan lawan untuk dilihat selama 3 detik                              |
| **BLIND_SWAP**   |   J (Jack)    | Tukar buta                 | Player menukar 1 kartu dari tangan sendiri dengan 1 kartu dari tangan lawan tanpa melihat kartu tersebut |
| **SEE_AND_SWAP** |     Q, K      | Tukar dengan melihat       | Player melihat 1 kartu lawan, lalu dapat memutuskan untuk menukar atau tidak                             |

**Catatan Penting:** Kemampuan kartu hanya aktif ketika kartu tersebut **dibuang** ke discard pile (baik melalui swap maupun discard langsung).

---

## 2. Konfigurasi Costume

Costume adalah sistem yang memungkinkan variasi aturan dalam permainan Kabul. Saat ini tersedia 2 costume: Classic dan Reversed.

### 2.1 Tabel Costume

| Costume Key | Nama     | Deskripsi               | Joker Value | Red King Value |
| :---------: | :------- | :---------------------- | :---------: | :------------: |
|  COSTUME_1  | Classic  | Joker: -1, Red Kings: 0 |     -1      |       0        |
|  COSTUME_2  | Reversed | Joker: 0, Red Kings: -1 |      0      |       -1       |

### 2.2 Perbandingan Costume

| Aspek                  | Classic (COSTUME_1)    | Reversed (COSTUME_2)       |
| :--------------------- | :--------------------- | :------------------------- |
| **Joker**              | Nilai -1 (sangat baik) | Nilai 0 (cukup baik)       |
| **Red Kings (K♥, K♦)** | Nilai 0 (cukup baik)   | Nilai -1 (sangat baik)     |
| **Strategi**           | Berburu Joker          | Berburu Red Kings          |
| **Kartu Premium**      | Joker (-1)             | Joker (0) + Red Kings (-1) |
| **Tingkat Kesulitan**  | Sedang                 | Sedang                     |

### 2.3 Rekomendasi Pemilihan Costume

| Costume      | Cocok Untuk                                                                                |
| :----------- | :----------------------------------------------------------------------------------------- |
| **Classic**  | Pemain yang menyukai strategi berbasis Joker, lebih banyak kartu bernilai negatif tersedia |
| **Reversed** | Pemain yang menyukai variasi, Red Kings menjadi sangat berharga                            |

**Catatan Teknis:** Costume harus dipilih **sebelum** permainan dimulai (saat phase masih WAITING). Setelah game dimulai, costume tidak dapat diubah.

---

## 3. Konfigurasi Game

Bagian ini berisi konstanta dan pengaturan yang mengatur berjalannya permainan.

### 3.1 Tabel Konstanta Konfigurasi

| Konstanta         | Nilai Default |  Satuan   | Deskripsi                                                                          |
| :---------------- | :-----------: | :-------: | :--------------------------------------------------------------------------------- |
| MEMORIZE_DURATION |     3000      | milidetik | Durasi waktu pemain dapat melihat kartu depan mereka saat awal permainan (3 detik) |
| PEEK_DURATION     |     3000      | milidetik | Durasi waktu pemain dapat melihat kartu saat menggunakan kemampuan PEEK (3 detik)  |
| ACTION_TIMEOUT    |     15000     | milidetik | Batas waktu pemain untuk melakukan aksi swap/blind swap/see and swap (15 detik)    |
| SLAP_PENALTY      |       1       |   kartu   | Jumlah kartu penalty yang diterima pemain saat slap gagal                          |

### 3.2 Tabel Batasan Pemain

| Parameter        | Nilai Minimum | Nilai Maksimum | Rekomendasi | Deskripsi                                                                                      |
| :--------------- | :-----------: | :------------: | :---------: | :--------------------------------------------------------------------------------------------- |
| MIN_PLAYERS      |       2       |       -        |     2-4     | Jumlah minimum pemain untuk memulai game                                                       |
| MAX_PLAYERS      |       -       |       13       |     6-8     | Jumlah maksimum pemain (dibatasi oleh jumlah kartu: 54 kartu / 4 kartu per pemain = 13 pemain) |
| CARDS_PER_PLAYER |       4       |       4        |      4      | Jumlah kartu yang diterima setiap pemain saat awal permainan                                   |

### 3.3 Tabel Phase Permainan

| Phase        |   Kode   | Deskripsi                                                            | Transisi                                                                |
| :----------- | :------: | :------------------------------------------------------------------- | :---------------------------------------------------------------------- |
| **WAITING**  | waiting  | Menunggu pemain bergabung                                            | Otomatis ke MEMORIZE saat game dimulai                                  |
| **MEMORIZE** | memorize | Pemain melihat 2 kartu depan mereka selama MEMORIZE_DURATION         | Otomatis ke PLAYING setelah timer habis                                 |
| **PLAYING**  | playing  | Fase utama permainan, pemain bergantian mengambil dan membuang kartu | Ke ENDED saat Kabul dipanggil dan semua pemain selesai giliran terakhir |
| **ENDED**    |  ended   | Permainan selesai, pemenang ditentukan                               | Manual reset ke WAITING                                                 |

### 3.4 Tabel Pending Action States

| State                           |    Kode     | Pemicu                           | Deskripsi                                      |
| :------------------------------ | :---------: | :------------------------------- | :--------------------------------------------- |
| **CHOOSING_OWN_CARD_TO_PEEK**   |  peek_self  | Membuang kartu 7 atau 8          | Pemain memilih kartu sendiri untuk dilihat     |
| **CHOOSING_ENEMY_CARD_TO_PEEK** | peek_enemy  | Membuang kartu 9 atau 10         | Pemain memilih kartu lawan untuk dilihat       |
| **SWAPPING_CARDS**              | blind_swap  | Membuang kartu Jack              | Pemain menukar kartu buta dengan lawan         |
| **SEE_AND_SWAPPING_CARDS**      |  see_swap   | Membuang kartu Queen atau King   | Pemain melihat lalu menukar kartu dengan lawan |
| **PEEK_RESULT**                 | peek_result | Setelah memilih kartu untuk PEEK | Menampilkan hasil peek selama PEEK_DURATION    |

---

## 4. Contoh State Awal

Bagian ini menunjukkan contoh struktur state yang digunakan dalam sistem Kabul.

### 4.1 Contoh State Server Lengkap

State ini merepresentasikan kondisi game saat sedang berlangsung dengan 3 pemain:

| Field               | Nilai              |    Tipe Data     | Deskripsi                                                                     |
| :------------------ | :----------------- | :--------------: | :---------------------------------------------------------------------------- |
| gameId              | "game_001"         |      string      | ID unik untuk instance game                                                   |
| phase               | "PLAYING"          |      string      | Fase permainan saat ini                                                       |
| memorizeEndsAt      | null               | number/timestamp | Timestamp ketika fase memorize berakhir (null jika tidak dalam fase memorize) |
| currentTurnIndex    | 1                  |      number      | Indeks pemain yang sedang mendapat giliran                                    |
| turnOrder           | ["p1", "p2", "p3"] |      array       | Urutan giliran pemain                                                         |
| kabulCaller         | null               |   string/null    | ID pemain yang memanggil Kabul (null jika belum dipanggil)                    |
| finalTurnsRemaining | 0                  |      number      | Sisa giliran akhir setelah Kabul dipanggil                                    |
| winner              | null               |   string/null    | ID pemain pemenang (null jika game belum selesai)                             |
| costume             | "COSTUME_1"        |      string      | Costume yang digunakan dalam game                                             |
| deckCount           | 38                 |      number      | Sisa kartu di deck                                                            |

### 4.2 Contoh Data Pemain

| Player ID | Nama    | Card Count | Has Called Kabul | Is Connected |
| :-------: | :------ | :--------: | :--------------: | :----------: |
|    p1     | Alice   |     4      |      false       |     true     |
|    p2     | Bob     |     4      |      false       |     true     |
|    p3     | Charlie |     4      |      false       |     true     |

### 4.3 Contoh Struktur Kartu di Tangan

Berikut contoh kartu yang dimiliki Player 1 (Alice):

| Position | ID     | Rank  | Suit | Display | Value | Action Type  | Visible to Owner |
| :------: | :----- | :---: | :--: | :-----: | :---: | :----------- | :--------------: |
|    0     | 5♥     |   5   |  ♥   |   5♥    |   5   | NONE         |      false       |
|    1     | Q♠     |   Q   |  ♠   |   Q♠    |  12   | SEE_AND_SWAP |      false       |
|    2     | Joker1 | Joker | null |  Joker  |  -1   | NONE         |      false       |
|    3     | K♦     |   K   |  ♦   |   K♦    |   0   | SEE_AND_SWAP |      false       |

### 4.4 Contoh Discard Pile

| Urutan | ID  | Rank | Suit | Display | Value |
| :----: | :-- | :--: | :--: | :-----: | :---: |
|  Top   | 7♣  |  7   |  ♣   |   7♣    |   7   |
|  2nd   | A♠  |  A   |  ♠   |   A♠    |   1   |
|  3rd   | 10♥ |  10  |  ♥   |   10♥   |  10   |

### 4.5 Contoh Client State (Masked)

State yang dikirim ke client sudah dimasking untuk menyembunyikan informasi sensitif:

| Field         | Nilai untuk Alice                                                                              | Keterangan                  |
| :------------ | :--------------------------------------------------------------------------------------------- | :-------------------------- |
| gameId        | "game_001"                                                                                     | ID game                     |
| phase         | "PLAYING"                                                                                      | Fase saat ini               |
| myHand        | [{pos:0, vis:false}, {pos:1, vis:false}, {pos:2, vis:false}, {pos:3, vis:false}]               | Kartu sendiri (tersembunyi) |
| peekedCard    | null                                                                                           | Hasil peek terakhir         |
| opponents     | [{id:"p2", name:"Bob", cards:4, kabul:false}, {id:"p3", name:"Charlie", cards:4, kabul:false}] | Info lawan                  |
| topDiscard    | {display:"7♣", value:7, rank:"7"}                                                              | Kartu teratas discard pile  |
| deckCount     | 38                                                                                             | Sisa kartu di deck          |
| currentTurn   | "p2"                                                                                           | Giliran siapa sekarang      |
| isMyTurn      | false                                                                                          | Apakah giliran Alice        |
| drawnCard     | null                                                                                           | Kartu yang sedang diambil   |
| pendingAction | null                                                                                           | Aksi yang sedang menunggu   |
| kabulCaller   | null                                                                                           | Siapa yang Kabul            |
| canSlap       | true                                                                                           | Bisa slap atau tidak        |

### 4.6 Contoh State Saat Membuang Kartu 7

Ketika pemain membuang kartu 7, state pendingAction akan terisi:

| Field     | Nilai                       |
| :-------- | :-------------------------- |
| type      | "CHOOSING_OWN_CARD_TO_PEEK" |
| playerId  | "p1"                        |
| expiresAt | 1699999999999               |

Setelah pemain memilih kartu untuk diintip:

| Field     | Nilai                                                  |
| :-------- | :----------------------------------------------------- |
| type      | "PEEK_RESULT"                                          |
| playerId  | "p1"                                                   |
| targetId  | "p1"                                                   |
| handIndex | 2                                                      |
| card      | {id:"Joker1", rank:"Joker", value:-1, display:"Joker"} |
| expiresAt | 1700000002999                                          |

---

## 5. Penjelasan Field

Bagian ini memberikan penjelasan detail untuk setiap field yang digunakan dalam sistem Kabul.

### 5.1 Field Kartu (Card Object)

| Field          |    Tipe     | Deskripsi                                         | Contoh                            |
| :------------- | :---------: | :------------------------------------------------ | :-------------------------------- |
| **id**         |   string    | Identifikasi unik kartu berdasarkan rank dan suit | "A♥", "10♣", "Joker1"             |
| **rank**       |   string    | Nilai rank kartu                                  | "A", "7", "J", "K", "Joker"       |
| **suit**       | string/null | Simbol suit (♥, ♦, ♠, ♣) atau null untuk Joker    | "♥", "♠", null                    |
| **value**      |   number    | Nilai poin kartu untuk perhitungan skor           | -1, 0, 5, 11, 13                  |
| **actionType** |   string    | Kemampuan khusus saat kartu dibuang               | "PEEK_SELF", "BLIND_SWAP", "NONE" |
| **display**    |   string    | Representasi visual kartu untuk UI                | "A♥", "10♦", "Joker"              |
| **position**   |   number    | Posisi kartu dalam tangan pemain (0-3)            | 0, 1, 2, 3                        |

#### 5.1.1 Penjelasan Detail Field id

Field `id` digunakan sebagai identifier unik untuk setiap kartu dalam deck. Formatnya adalah kombinasi rank dan suit (kecuali Joker yang menggunakan suffix angka).

| Tipe Kartu    | Format ID     | Contoh             |
| :------------ | :------------ | :----------------- |
| Kartu Standar | {rank}{suit}  | "A♥", "10♣", "K♦"  |
| Joker         | Joker{number} | "Joker1", "Joker2" |

#### 5.1.2 Penjelasan Detail Field value

Nilai kartu sangat penting untuk penentuan pemenang. Berikut aturan penentuan nilai:

```
Jika rank == "Joker":
    value = costume.jokerValue
Jika rank == "K":
    Jika suit == "♥" atau suit == "♦":
        value = costume.redKingValue
    Else:
        value = 13
Else:
    value = CARD_VALUES[rank]  // Menggunakan mapping standar
```

**Mapping CARD_VALUES standar:**

| Rank | Value |
| :--: | :---: |
|  A   |   1   |
| 2-10 | 2-10  |
|  J   |  11   |
|  Q   |  12   |
|  K   |  13   |

#### 5.1.3 Penjelasan Detail Field actionType

Field ini menentukan apa yang terjadi ketika kartu dibuang:

| Action Type  |     Rank      | Efek                                              |
| :----------- | :-----------: | :------------------------------------------------ |
| NONE         | A, 2-6, Joker | Tidak ada efek khusus                             |
| PEEK_SELF    |     7, 8      | Pemain dapat melihat 1 kartu dari tangan sendiri  |
| PEEK_ENEMY   |     9, 10     | Pemain dapat melihat 1 kartu dari tangan lawan    |
| BLIND_SWAP   |       J       | Pemain menukar 1 kartu dengan lawan tanpa melihat |
| SEE_AND_SWAP |     Q, K      | Pemain melihat kartu lawan lalu bisa menukar      |

### 5.2 Field Player

| Field              |  Tipe   | Deskripsi                                | Contoh                               |
| :----------------- | :-----: | :--------------------------------------- | :----------------------------------- |
| **id**             | string  | ID unik pemain                           | "p1", "player_123"                   |
| **name**           | string  | Nama display pemain                      | "Alice", "Bob"                       |
| **hand**           |  array  | Array objek kartu di tangan              | [{card1}, {card2}, {card3}, {card4}] |
| **hasCalledKabul** | boolean | Apakah pemain sudah memanggil Kabul      | true, false                          |
| **isConnected**    | boolean | Status koneksi pemain                    | true, false                          |
| **finalScore**     | number  | Skor akhir pemain (setelah game selesai) | 15, 8, -1                            |

#### 5.2.1 Penjelasan Detail Field hand

Field `hand` adalah array yang berisi maksimal 4 kartu. Setiap kartu memiliki struktur lengkap seperti dijelaskan di bagian 5.1.

**Struktur tangan awal:** Setiap pemain menerima 4 kartu saat game dimulai. Posisi kartu (0-3) menentukan lokasi visual di UI:

| Posisi | Lokasi UI      |
| :----: | :------------- |
|   0    | Depan Kiri     |
|   1    | Depan Kanan    |
|   2    | Belakang Kiri  |
|   3    | Belakang Kanan |

**Catatan:** Selama fase MEMORIZE, hanya posisi 0 dan 1 yang terlihat oleh pemiliknya.

#### 5.2.2 Penjelasan Detail Field hasCalledKabul

Field ini menandakan apakah pemain sudah memanggil Kabul. Hanya satu pemain yang bisa memanggil Kabul dalam satu ronde. Setelah Kabul dipanggil:

1. Pemain yang memanggil tidak bisa mengambil giliran lagi
2. Setiap pemain lain mendapat 1 giliran terakhir
3. Game berakhir setelah semua pemain selesai giliran terakhir
4. Pemenang ditentukan berdasarkan skor terendah

### 5.3 Field Game State

| Field                   |    Tipe     | Deskripsi                           | Contoh                                    |
| :---------------------- | :---------: | :---------------------------------- | :---------------------------------------- |
| **gameId**              |   string    | ID unik instance game               | "game_001", "kabul_abc123"                |
| **phase**               |   string    | Fase permainan saat ini             | "WAITING", "MEMORIZE", "PLAYING", "ENDED" |
| **memorizeEndsAt**      | number/null | Timestamp berakhirnya fase memorize | 1699999999999, null                       |
| **players**             |   object    | Map pemain dengan key playerId      | {"p1": {...}, "p2": {...}}                |
| **turnOrder**           |    array    | Urutan giliran pemain               | ["p1", "p2", "p3"]                        |
| **currentTurnIndex**    |   number    | Indeks pemain yang sedang giliran   | 0, 1, 2                                   |
| **deck**                |    array    | Array kartu yang tersisa di deck    | [{card1}, {card2}, ...]                   |
| **discardPile**         |    array    | Array kartu yang sudah dibuang      | [{card1}, {card2}, ...]                   |
| **topDiscard**          | object/null | Kartu teratas di discard pile       | {id:"7♣", rank:"7", ...}                  |
| **drawnCard**           | object/null | Kartu yang sedang diambil pemain    | {playerId:"p1", card:{...}}               |
| **pendingAction**       | object/null | Aksi yang menunggu respon pemain    | {type:"PEEK_SELF", playerId:"p1", ...}    |
| **kabulCaller**         | string/null | ID pemain yang memanggil Kabul      | "p1", null                                |
| **finalTurnsRemaining** |   number    | Sisa giliran setelah Kabul          | 2, 1, 0                                   |
| **winner**              | string/null | ID pemain pemenang                  | "p2", null                                |
| **costume**             |   string    | Costume yang digunakan              | "COSTUME_1", "COSTUME_2"                  |

#### 5.3.1 Penjelasan Detail Field phase

Field ini mengontrol alur permainan:

| Phase    | Deskripsi                    | Durasi                      |
| :------- | :--------------------------- | :-------------------------- |
| WAITING  | Menunggu pemain bergabung    | Tidak terbatas              |
| MEMORIZE | Pemain melihat 2 kartu depan | MEMORIZE_DURATION (3 detik) |
| PLAYING  | Giliran pemain bermain       | Sampai Kabul dipanggil      |
| ENDED    | Permainan selesai            | Manual reset                |

**Transisi Phase:**

1. WAITING → MEMORIZE: Saat `startGame()` dipanggil
2. MEMORIZE → PLAYING: Setelah MEMORIZE_DURATION berlalu
3. PLAYING → ENDED: Saat Kabul dipanggil dan final turns selesai

#### 5.3.2 Penjelasan Detail Field turnOrder dan currentTurnIndex

Sistem giliran menggunakan array turnOrder dan index:

```
currentPlayerId = turnOrder[currentTurnIndex]
```

Untuk pindah ke pemain berikutnya:

```
currentTurnIndex = (currentTurnIndex + 1) % turnOrder.length
```

**Contoh:**

- turnOrder = ["p1", "p2", "p3"]
- currentTurnIndex = 0 → currentPlayerId = "p1"
- currentTurnIndex = 1 → currentPlayerId = "p2"

#### 5.3.3 Penjelasan Detail Field drawnCard

Field ini menyimpan informasi kartu yang sedang diambil oleh pemain dalam gilirannya:

| Sub-field |  Tipe  | Deskripsi                        |
| :-------- | :----: | :------------------------------- |
| playerId  | string | ID pemain yang mengambil kartu   |
| card      | object | Objek kartu lengkap yang diambil |

Kartu ini harus diproses (swap atau discard) sebelum giliran berakhir. Field ini akan null jika pemain belum mengambil kartu atau sudah memprosesnya.

#### 5.3.4 Penjelasan Detail Field pendingAction

Field ini menyimpan aksi yang menunggu input dari pemain, biasanya setelah membuang kartu dengan kemampuan khusus:

| Sub-field    |  Tipe  | Deskripsi                                |
| :----------- | :----: | :--------------------------------------- |
| type         | string | Jenis aksi yang pending                  |
| playerId     | string | ID pemain yang harus merespons           |
| expiresAt    | number | Timestamp batas waktu aksi               |
| targetId     | string | ID target (untuk PEEK_ENEMY)             |
| handIndex    | number | Index kartu (untuk PEEK_RESULT)          |
| card         | object | Kartu yang diintip (untuk PEEK_RESULT)   |
| revealedCard | object | Kartu yang terlihat (untuk SEE_AND_SWAP) |

### 5.4 Field Client State (Masked)

State yang dikirim ke client sudah difilter untuk mencegah cheating:

| Field              |    Tipe     | Deskripsi                                    |
| :----------------- | :---------: | :------------------------------------------- |
| **gameId**         |   string    | ID game                                      |
| **phase**          |   string    | Fase saat ini                                |
| **memorizeEndsAt** | number/null | Waktu berakhir memorize                      |
| **myHand**         |    array    | Kartu pemain (termasuk visibility)           |
| **peekedCard**     | object/null | Hasil peek terakhir                          |
| **opponents**      |    array    | Info lawan (tanpa detail kartu)              |
| **topDiscard**     |   object    | Kartu teratas discard                        |
| **deckCount**      |   number    | Jumlah kartu di deck                         |
| **currentTurn**    |   string    | ID pemain yang giliran                       |
| **isMyTurn**       |   boolean   | Apakah giliran client                        |
| **drawnCard**      | object/null | Kartu yang diambil (hanya jika milik client) |
| **pendingAction**  | object/null | Aksi pending (hanya jika milik client)       |
| **kabulCaller**    | string/null | Siapa yang Kabul                             |
| **canSlap**        |   boolean   | Bisa slap atau tidak                         |

#### 5.4.1 Masking myHand

Kartu pemain di-mask dengan aturan berikut:

- **Selama MEMORIZE:** Posisi 0 dan 1 menampilkan detail lengkap, posisi 2 dan 3 hanya visible:false
- **Setelah MEMORIZE:** Semua posisi hanya menampilkan visible:false
- **Setelah PEEK:** Kartu yang diintip menampilkan detail lengkap selama PEEK_DURATION

#### 5.4.2 Masking Opponents

Informasi lawan dibatasi:

| Field          | Deskripsi              |
| :------------- | :--------------------- |
| id             | ID pemain              |
| name           | Nama pemain            |
| cardCount      | Jumlah kartu di tangan |
| hasCalledKabul | Status Kabul           |

**Detail kartu lawan tidak dikirim** untuk mencegah cheating.

### 5.5 Field Costume Configuration

| Field            |  Tipe  | Deskripsi                |          Classic          |         Reversed          |
| :--------------- | :----: | :----------------------- | :-----------------------: | :-----------------------: |
| **name**         | string | Nama costume             |         "Classic"         |        "Reversed"         |
| **description**  | string | Deskripsi singkat        | "Joker: -1, Red Kings: 0" | "Joker: 0, Red Kings: -1" |
| **jokerValue**   | number | Nilai Joker              |            -1             |             0             |
| **redKingValue** | number | Nilai Red Kings (K♥, K♦) |             0             |            -1             |

---

## 6. Skema Database (Referensi)

Berikut skema database yang direkomendasikan untuk menyimpan data permainan Kabul:

### 6.1 Tabel Games

| Kolom                 |    Tipe     | Nullable |      Default      | Deskripsi                 |
| :-------------------- | :---------: | :------: | :---------------: | :------------------------ |
| game_id               | VARCHAR(36) |    NO    |       UUID        | Primary key               |
| phase                 |    ENUM     |    NO    |     'WAITING'     | Fase permainan            |
| costume               |    ENUM     |    NO    |    'COSTUME_1'    | Costume yang digunakan    |
| current_turn_index    |     INT     |    NO    |         0         | Index giliran saat ini    |
| turn_order            |    JSON     |    NO    |        []         | Array urutan pemain       |
| kabul_caller          | VARCHAR(36) |   YES    |       NULL        | ID pemangil Kabul         |
| final_turns_remaining |     INT     |    NO    |         0         | Sisa giliran akhir        |
| winner_id             | VARCHAR(36) |   YES    |       NULL        | ID pemenang               |
| deck_data             |    JSON     |    NO    |        []         | Data deck (kartu tersisa) |
| discard_pile          |    JSON     |    NO    |        []         | Data discard pile         |
| top_discard           |    JSON     |   YES    |       NULL        | Kartu teratas discard     |
| drawn_card            |    JSON     |   YES    |       NULL        | Kartu yang sedang diambil |
| pending_action        |    JSON     |   YES    |       NULL        | Aksi yang menunggu        |
| created_at            |  TIMESTAMP  |    NO    | CURRENT_TIMESTAMP | Waktu pembuatan           |
| updated_at            |  TIMESTAMP  |    NO    | CURRENT_TIMESTAMP | Waktu update terakhir     |

### 6.2 Tabel Players

| Kolom            |    Tipe     | Nullable |      Default      | Deskripsi            |
| :--------------- | :---------: | :------: | :---------------: | :------------------- |
| player_id        | VARCHAR(36) |    NO    |       UUID        | Primary key          |
| game_id          | VARCHAR(36) |    NO    |         -         | Foreign key ke games |
| name             | VARCHAR(50) |    NO    |         -         | Nama pemain          |
| hand_data        |    JSON     |    NO    |        []         | Data kartu di tangan |
| has_called_kabul |   BOOLEAN   |    NO    |       FALSE       | Status Kabul         |
| is_connected     |   BOOLEAN   |    NO    |       TRUE        | Status koneksi       |
| final_score      |     INT     |   YES    |       NULL        | Skor akhir           |
| joined_at        |  TIMESTAMP  |    NO    | CURRENT_TIMESTAMP | Waktu bergabung      |

### 6.3 Tabel Cards (Master Data)

| Kolom       |    Tipe     | Nullable | Deskripsi                     |
| :---------- | :---------: | :------: | :---------------------------- |
| card_id     | VARCHAR(10) |    NO    | ID kartu (A♥, Joker1, dll)    |
| rank        | VARCHAR(5)  |    NO    | Rank kartu                    |
| suit        | VARCHAR(2)  |   YES    | Suit kartu (NULL untuk Joker) |
| base_value  |     INT     |    NO    | Nilai dasar                   |
| action_type | VARCHAR(20) |    NO    | Kemampuan kartu               |

---

## 7. Catatan Implementasi

### 7.1 Validasi Penting

Saat mengimplementasikan sistem ini, perhatikan validasi berikut:

| Validasi                               | Kapan                     | Error Message                                  |
| :------------------------------------- | :------------------------ | :--------------------------------------------- |
| Minimum 2 pemain                       | Saat startGame            | "Need at least 2 players"                      |
| Costume hanya bisa diubah saat WAITING | Saat setCostume           | "Cannot change costume after game has started" |
| Tidak bisa draw 2x dalam 1 giliran     | Saat drawCard             | "Already drew a card this turn"                |
| Hanya bisa swap saat punya drawnCard   | Saat swapCard             | "No card drawn this turn"                      |
| Tidak bisa Kabul saat punya drawnCard  | Saat callKabul            | "Must discard or swap before calling Kabul"    |
| Tidak bisa swap dengan diri sendiri    | Saat blindSwap/seeAndSwap | "Cannot swap with yourself"                    |
| Validasi index kartu                   | Saat semua operasi kartu  | "Invalid hand index"                           |

### 7.2 Timer dan Timeout

Sistem ini menggunakan beberapa timer:

| Timer          | Durasi  | Trigger              | Action                                |
| :------------- | :-----: | :------------------- | :------------------------------------ |
| Memorize Timer | 3000ms  | Game start           | Auto transition ke PLAYING            |
| Peek Timer     | 3000ms  | Setelah peek         | Auto clear peek dan next turn         |
| Action Timeout | 15000ms | Setelah kartu action | Auto skip action jika tidak merespons |

### 7.3 Security Considerations

| Aspek               | Implementasi                                |
| :------------------ | :------------------------------------------ |
| **Card Visibility** | Selalu mask kartu lawan di client state     |
| **Turn Validation** | Validasi server-side untuk setiap aksi      |
| **Action Timeout**  | Batasi waktu aksi untuk mencegah AFK abuse  |
| **Costume Lock**    | Lock costume setelah game start             |
| **Slap Penalty**    | Penalty untuk slap yang salah mencegah spam |

---

**Dokumen ini merupakan seed data lengkap untuk sistem permainan Kabul. Gunakan sebagai referensi untuk implementasi database, validasi, dan logika permainan.**

_Versi: 1.0_  
_Terakhir Diupdate: Februari 2026_
