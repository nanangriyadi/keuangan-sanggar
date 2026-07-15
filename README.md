<img width="1677" height="781" alt="image" src="https://github.com/user-attachments/assets/ed26d6cd-47b2-42de-b3c6-df94306e1443" />

# Buku Kas Acara — Dashboard Anggaran (Read-Only)

Dashboard responsif yang menampilkan data anggaran acara langsung dari Google
Sheets. Halaman ini **hanya menampilkan data** — tidak ada tombol tambah,
ubah, atau hapus. Semua pengeditan tetap dilakukan di Google Sheets seperti
biasa.

## Struktur

- `Code.gs` — backend (Google Apps Script) yang membaca sheet `Anggaran_acara`
  dan mengubahnya jadi JSON.
- `index.html` — tampilan (Tailwind CSS), mengambil data dari `Code.gs` lalu
  merender ringkasan + tabel transaksi.

## Langkah 1 — Pasang Code.gs di Google Sheets

1. Buka spreadsheet Anda → menu **Extensions > Apps Script**.
2. Hapus kode default di editor, lalu tempel seluruh isi `Code.gs`.
3. Simpan (ikon disket / `Ctrl+S`).
4. Klik **Deploy > New deployment**.
   - Klik ikon gear di samping "Select type" → pilih **Web app**.
   - **Execute as**: Me (akun Anda).
   - **Who has access**: Anyone.
5. Klik **Deploy**, lalu izinkan akses (Authorize access) sesuai akun Google
   Anda.
6. Salin **Web app URL** yang muncul (diakhiri `/exec`). URL inilah yang
   dipakai halaman web untuk mengambil data.

> Setiap kali Anda mengubah `Code.gs`, buka **Deploy > Manage deployments**,
> edit deployment yang ada, lalu pilih versi baru — atau data lama yang akan
> terus tampil.

## Langkah 2 — Sambungkan index.html ke Apps Script

Buka `index.html`, cari baris berikut di bagian bawah file:

```js
const API_URL = "PASTE_URL_WEB_APP_GOOGLE_APPS_SCRIPT_DI_SINI";
```

Ganti dengan URL `/exec` dari Langkah 1, misalnya:

```js
const API_URL = "https://script.google.com/macros/s/AKfycbx.../exec";
```

## Langkah 3 — Unggah ke GitHub & aktifkan GitHub Pages

1. Buat repository baru di GitHub (boleh publik atau privat, GitHub Pages
   tetap bisa dipakai untuk repo publik secara gratis).
2. Unggah `index.html` (dan `README.md` jika mau) ke repository tersebut.
   Lewat browser: **Add file > Upload files**. Lewat terminal:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Dashboard anggaran acara"
   git branch -M main
   git remote add origin https://github.com/USERNAME/NAMA-REPO.git
   git push -u origin main
   ```
3. Di repository, buka **Settings > Pages**.
4. Pada **Source**, pilih branch `main` dan folder `/ (root)`, lalu **Save**.
5. Tunggu 1–2 menit, GitHub akan memberi URL seperti:
   `https://USERNAME.github.io/NAMA-REPO/`

Buka URL tersebut — dashboard akan otomatis memuat data dari Google Sheets
dan menyegarkan diri sendiri setiap 60 detik, tanpa perlu tombol apa pun.

## Catatan penting

- `Code.gs` bersifat read-only: tidak memuat fungsi tambah/ubah/hapus data,
  sehingga data hanya bisa diubah lewat Google Sheets langsung — sesuai
  permintaan.
- Kolom kosong pada tabel Google Sheets (placeholder seperti "Name" atau
  "dd/mm/yyyy") otomatis diabaikan; hanya baris yang benar-benar terisi yang
  dihitung.
- Kategori baru yang ditambahkan di sheet akan otomatis mendapat warna label
  di dashboard, tanpa perlu mengubah kode.
