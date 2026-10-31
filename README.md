# ADMIN SD DIGITAL – halaman GitHub Pages

Folder ini berisi halaman penaut (wrapper) agar aplikasi Google Apps Script (GAS) bisa dibuka dari alamat
`https://NAMAANDA.github.io/NAMAREPO/` dan dipasang sebagai aplikasi di layar HP atau laptop.

```
index.html               halaman penaut + tombol pasang aplikasi
manifest.webmanifest     identitas aplikasi (nama, ikon, warna)
sw.js                    service worker (cangkang halaman)
icons/                   ikon 192, 512, dan maskable
```

## A. Persiapan di Apps Script (wajib, sekali saja)
1. Tempel `Code.gs` dan `Index.html` terbaru. `doGet` sudah memakai `ALLOWALL` agar aplikasi boleh ditanam.
2. **Terapkan > Kelola deployment > ikon pensil**:
   - Jalankan sebagai: **Saya**
   - Siapa yang memiliki akses: **Siapa saja** (halaman login Google tidak bisa ditanam di iframe)
   - Versi: **Versi baru** > **Deploy**
3. Salin **URL aplikasi web** yang berakhiran `/exec`.

## B. Pasang di GitHub Pages
1. Buka `index.html`, ganti nilai `GAS_URL` dengan URL dari langkah A.3.
2. Buat repositori baru di GitHub (publik), lalu unggah **isi folder ini** ke root repositori
   (`index.html`, `manifest.webmanifest`, `sw.js`, folder `icons`).
3. **Settings > Pages > Build and deployment**: Source = *Deploy from a branch*, Branch = `main`, folder = `/ (root)`.
4. Tunggu sekitar 1 menit, lalu buka `https://NAMAANDA.github.io/NAMAREPO/`.
5. Di Chrome atau Edge akan muncul tombol **Pasang** (pasang aplikasi sungguhan). Di iPhone: Bagikan > Tambah ke Layar Utama.

## C. (Disarankan) Perketat situs yang boleh menanam aplikasi
Di `Index.html` (Apps Script) ada daftar `ASAL`. Ganti `'github.io'` dengan `'NAMAANDA.github.io'`
supaya hanya halaman Anda yang boleh menanam aplikasi. Jika memakai domain sendiri di GitHub Pages, tambahkan domainnya.

## D. (Opsional) Sinkronkan kode GAS dengan GitHub memakai clasp
```bash
npm install -g @google/clasp
clasp login
clasp clone SCRIPT_ID            # SCRIPT_ID ada di Pengaturan Proyek Apps Script
git init && git add . && git commit -m "kode awal"
git remote add origin https://github.com/NAMAANDA/NAMAREPO-KODE.git
git push -u origin main          # simpan kode di GitHub
clasp push                       # kirim perubahan dari komputer ke Apps Script
```
Jangan pernah menyimpan kata sandi atau API key di repositori. `GEMINI_API_KEY` tersimpan di Properti Skrip, bukan di kode.
Sebaiknya gunakan repositori **privat** untuk kode GAS.

## Pemecahan masalah
- **Layar "Aplikasi tidak merespons"**: pastikan akses "Siapa saja", versi deployment terbaru, dan `GAS_URL` benar.
- **Pesan "tidak diizinkan ditampilkan di situs ini"**: tambahkan domain Anda ke daftar `ASAL` di `Index.html`.
- **Perlu login ulang setiap refresh**: wajar jika browser memblokir penyimpanan pihak ketiga di dalam iframe.
- Perubahan di GitHub baru terlihat setelah 1–2 menit; tekan Ctrl+Shift+R untuk memuat ulang cache.
