# Urutan Baca (Wajib)

## Jalur 1: Urutan "Wajib Baca" (Untuk Semua Anggota Tim)
Baca ini dulu supaya satu visi tentang game apa yang mau dibuat.

- [arsip/referensi-glosarium.md](arsip/referensi-glosarium.md) — Buka di tab terpisah.
	- Kenapa: Ini kamus. Jangan dihafal, tapi biarkan terbuka. Kalau ketemu istilah aneh (seperti `RemoteEvent`, `SurfaceGui`, `ReplicatedStorage`), cek di sini.

- [analisa/produk-demo-skenario.md](analisa/produk-demo-skenario.md) 🎬
	- Kenapa: Ini seperti membaca novel/naskah film. Dokumen ini menceritakan perjalanan Andi & Budi main dari awal sampai akhir.
	- Tujuan: Supaya tim bisa membayangkan: "Oh, nanti jadinya bakal seperti ini lho di layar."

- [analisa/produk-fitur-lengkap.md](analisa/produk-fitur-lengkap.md) 📋
	- Kenapa: Ini daftar belanjaan fitur.
	- Tujuan: Memahami batasan. Mana yang harus dibuat sekarang (Phase 1), mana yang nanti saja (Phase 2).

- [analisa/teknis-arsitektur.md](analisa/teknis-arsitektur.md) 🏗️
	- Kenapa: Ini peta lokasi.
	- Tujuan: Memahami "dapur" Roblox. Di mana naruh kode? Di mana naruh gambar kartu? Bagaimana cara client ngomong ke server? (Ada penjelasan ⭐ untuk pemula).

## Jalur 2: Urutan Berdasarkan Peran (Spesifik)
Setelah membaca Jalur 1, anggota tim bisa mencari dokumen teknis masing-masing.

### Untuk Builder & UI Designer (Non-Coding)
Fokus: Tampilan visual, meja 3D, dan aset gambar.

- [analisa/bisnis-backlog-tim.md](analisa/bisnis-backlog-tim.md) — Cari bagian "Role: Builder" & "Role: Designer". Lihat tugas mingguanmu di sana.
- [analisa/produk-user-stories.md](analisa/produk-user-stories.md) — Baca story yang berhubungan dengan UI/Visual (contoh: "Sebagai pemain, saya ingin melihat kartu saya...").
- [database/seed-data.md](database/seed-data.md) — Lihat tabel 54 Kartu. Ini panduan kamu untuk upload gambar (aset mana untuk kartu mana).

### Untuk Scripter (Programmer)
Fokus: Logika, database, dan koneksi.

- [analisa/teknis-database.md](analisa/teknis-database.md) & [database/requirements.md](database/requirements.md) — Pahami dulu cara simpan data di Roblox (DataStore vs Memory). Beda banget sama Firebase.
- [analisa/teknis-api-events.md](analisa/teknis-api-events.md) ⚡ **(PENTING)** — Ini "kitab suci" komunikasi. Isinya daftar perintah (Event) dari Client ke Server. Kamu akan sering bolak-balik ke sini saat ngoding.
- [database/seed-data.md](database/seed-data.md) — Copy-paste tabel konfigurasi (Timer, Nilai Kartu) dari sini ke dalam script Lua nanti.

### Untuk Project Manager / Ketua Tim
Fokus: Mengatur jadwal dan memastikan arah.

- [analisa/bisnis-backlog-tim.md](analisa/bisnis-backlog-tim.md) 📅 — Ini jadwal kerja kalian selama 8 minggu (4 Sprint). Pantau progress pakai dokumen ini.
- [analisa/bisnis-dampak.md](analisa/bisnis-dampak.md) — Baca ini untuk motivasi tim (potensi user & uang), dan untuk jaga-jaga kalau ada risiko.

## Tips Memulai Hari Ini

- Kumpulkan tim.
- Buka [analisa/produk-demo-skenario.md](analisa/produk-demo-skenario.md) dan baca bareng-bareng (atau salah satu membacakan). Ini cara tercepat menyamakan frekuensi.
- Buka [analisa/bisnis-backlog-tim.md](analisa/bisnis-backlog-tim.md) dan lihat Sprint 1. Bagi tugas siapa yang install Roblox Studio duluan, siapa yang cari aset meja, dll.
- Jalan! 🚀