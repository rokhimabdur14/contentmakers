---
name: bantuinaja-feed
description: >-
  Produce Instagram feed content for bantuinaja.id (Jogja on-demand helper service) in the approved
  house style. Use whenever the user asks for new bantuinaja posts, carousels, a monthly feed plan,
  a loker/recruitment post, or a redesign/re-render of existing bantuinaja slides. Covers the source
  files, the "Biru komersial" design system and color palette, photo/cutout rules, the HTML→PNG
  render pipeline, the per-post folder format, and the checks to run before delivering.
---

# bantuinaja.id — Feed Instagram

Skill ini merangkum keputusan yang sudah disetujui pemilik (Oktober 2026). Ikuti aturan di sini
sebelum membuat atau mengubah desain bantuinaja — jangan kembali ke gaya yang sudah ditolak.

## 1. Baca dulu
- `bantuinaja/brand-profile.md` — positioning, suara, guardrail (tanpa "joki tugas", Teman Jalan
  wajib Strict Platonic, angka website seperti "25.000+ tugas" tidak dipakai tanpa bukti).
- `bantuinaja/content-pillars.md` — 4 pillar.
- `bantuinaja/design/palet-warna.md` — palet resmi (diambil dari kode website).
- `bantuinaja/design/broll/SUMBER-FOTO.md` — sumber & lisensi semua foto.
- Contoh jadi: `bantuinaja/november-2026/` (13 postingan + cadangan + loker).

## 2. Gaya desain: "Biru komersial"
Referensi: `G:\Shared drives\Akselera Edtech\REREFENSI MOTION\FEED\` (3 JPG).

| Elemen | Aturan |
|---|---|
| Latar | Biru cerah gradasi `#2290E6 → #0B6DB8 → #00558E → #003F72` bergantian dengan putih kebiruan `#F3F8FD`. Lingkaran garis tipis + kilau kecil sebagai dekorasi. |
| Font | **Poppins** saja (judul 800, isi 500–600). **Dilarang font serif/latin/script/miring.** |
| Judul | Kata kunci diberi **kotak sorotan** (`<mark>`): kotak putih di latar biru, kotak biru di latar putih. |
| Tokoh | Orang **Indonesia** tanpa latar, di sisi kanan; teks rata kiri. Tanpa tokoh → isi rata tengah. |
| Checklist | Pil putih + ikon centang biru (`#2680BE → #00558E`). |
| Harga / estimasi | Kartu putih (latar biru) atau biru (latar putih), label huruf kapital kecil. |
| Header | Tab logo putih menggantung di atas ("bantuinaja.id · JOGJA") + nomor slide untuk carousel. |
| Footer | Bar kontak: `bantuinaja.id` · `@bantuinajaofficial` · WA `0856-0134-3247` (+ "Geser →" di carousel). |
| Slide langkah | Foto berbingkai putih (sedikit miring) + lencana nomor (01, H-7, …). |
| Komposisi | Isi seimbang vertikal (spacer atas & bawah sama), tidak menumpuk di atas. |

Gaya yang **sudah ditolak** (jangan dipakai lagi): playful neo-brutal (stiker emoji, garis tebal,
bayangan keras), warna kuning di feed, aksen serif miring, foto potret generik yang tidak terkait isi.

## 3. Warna — hanya dari palet
Semua warna di template harus ada di `palet-warna.md`. Inti: Biru Utama `#00558E`, Navy `#002248`,
Navy Gradasi `#003865`, Biru Terang `#2680BE`, Biru Muda `#D9ECFC`, Putih Es `#F3F8FD`,
Slate 500 `#64748B` (teks keterangan), Slate 200 `#E2E8F0` (garis). Bayangan/transparansi pakai
rgba dari Navy / Navy Gradasi / Biru Utama. **Kuning `#FDB827` ada di website tapi tidak dipakai di feed.**

## 4. Foto
- Pakai foto dari **website** (`broll/web/`, Unsplash) atau **Pixabay**. Orang harus berkonteks
  Indonesia (cek visual: hijab, peci, blangkon, pasar tradisional). Catat setiap foto baru di `SUMBER-FOTO.md`.
- Tokoh tanpa latar ada di `broll/cutout/`. Untuk tokoh baru: cari Pixabay dengan `?colors=transparent`,
  atau hapus latar foto yang sudah ada dengan `rembg` (model `isnet-general-use`), lalu **crop ke batas tubuh**
  (PNG Pixabay membawa ruang transparan lebar → posisi meleset kalau tidak di-crop).
- Setiap foto orang diberi label kecil **"Foto ilustrasi"**; jangan pernah dipasangkan dengan nama/testimoni pelanggan.
  Postingan testimoni tanpa wajah.
- Model berhijab yang sama dipakai di banyak postingan — ganti dengan foto mitra asli bila sudah ada (dengan izin).

## 5. Template & render
- Semua desain ada di `bantuinaja/design/feed.html`:
  - `POSTS` = isi tiap postingan (`id`, `theme: "blue" | "white"`, `slides[]` berisi HTML).
  - Helper `header(count)` dan `foot(catatan, "Geser →")`.
  - `LAYOUT[slideId]` = tata letak per slide: `person` (tokoh kanan, `h`, `x`), `bottom` (tokoh tengah bawah),
    `group` (beberapa tokoh berjajar), `frame` (foto berbingkai, `fh`), `full` (foto penuh + gradasi biru),
    `lineup` / `lineupFill` (jajaran foto mitra).
- Render: jalankan `python -m http.server 8765` di folder `design/`, buka `feed.html` dengan Playwright
  (viewport 1200×1500), tunggu `document.fonts.ready`, lalu screenshot tiap `section.slide`
  (1080×1350) ke folder postingan: `post.png` atau `slide-N.png`. `file://` diblokir — wajib lewat server lokal.
- Matikan server & hapus folder `.playwright-mcp` setelah selesai.

## 6. Format folder bulanan
`bantuinaja/<bulan-tahun>/NN_YYYY-MM-DD_Hari_Kategori_Judul/` berisi gambar, `caption.txt`
(siap tempel, hashtag maks. 5) dan `info.md` (jadwal, jam, kategori, tujuan, alt text, catatan).
Plus `README.md` (kalender, jenis postingan, jam posting, checklist) dan `preview-grid-akhir-<bulan>.png`.
- Ritme: 3×/minggu — **Senin 19.00, Rabu 12.00, Jumat 16.00 WIB** (titik awal, sesuaikan dengan Insights).
- Kategori: Akuisisi, Sales (≤ ±30%), Layanan pelanggan, Freelance (rekrut mitra).
- Link daftar mitra di bio: **`https://bantuin-amber.vercel.app/#daftar-freelance`** (versi tanpa `#` = 404).

## 7. Cek sebelum menyerahkan
1. Tidak ada teks yang menabrak bar kontak / keluar slide (cek `getBoundingClientRect` isi vs batas bawah).
2. Tidak ada wajah tokoh tertutup teks atau kartu — lihat hasil render ukuran penuh, bukan hanya thumbnail.
3. Warna yang tampil (computed style semua elemen di slide) hanya dari palet.
4. Tidak ada font serif (`getComputedStyle(...).fontFamily`).
5. Buat preview grid (sampul tiap postingan, terbaru di kiri atas) dan periksa ritme biru/putih.
6. Klaim yang belum dikonfirmasi pemilik (promo, testimoni, estimasi pendapatan mitra, nomor WA) ditulis
   di `info.md` sebagai catatan, bukan disembunyikan.
