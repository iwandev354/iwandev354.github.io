# iwandev354.github.io
# Pancasila dan Teknologi yang Bertanggung Jawab

Situs statis tentang **fungsi dan kedudukan Pancasila dalam penggunaan teknologi secara bertanggung jawab**.

- **Bahasa pemrograman:** Python (generator situs statis)
- **Database:** PostgreSQL (menyimpan seluruh konten)
- **Hasil akhir:** file HTML/CSS biasa di folder `dist/`, bisa dihosting di mana saja (GitHub Pages, Netlify, Nginx, dll.)

## Cara kerja

```
PostgreSQL  --->  build.py (Python + Jinja2)  --->  dist/*.html
 (konten)          membaca data, mengisi template      (situs statis)
```

Konten tidak ditulis langsung di HTML. Untuk mengubah isi situs, ubah data di database lalu jalankan `build.py` lagi.

## Struktur proyek

```
pancasila-tech/
├── schema.sql        # struktur tabel
├── seed.sql          # isi konten awal (bisa Anda edit)
├── db_init.py        # membuat tabel + mengisi data
├── build.py          # menghasilkan situs statis ke dist/
├── config.py         # pembaca DATABASE_URL / .env
├── templates/        # template Jinja2 (base, index, sila, dll.)
├── static/style.css  # tampilan
├── requirements.txt
└── dist/             # hasil build (dibuat otomatis)
```

## Cara menjalankan

1. **Pasang dependensi**
   ```bash
   python -m venv .venv
   source .venv/bin/activate        # Windows: .venv\Scripts\activate
   pip install -r requirements.txt
   ```

2. **Buat database PostgreSQL**
   ```bash
   createdb -U postgres pancasila_tech
   # atau lewat psql:  CREATE DATABASE pancasila_tech;
   ```

3. **Atur koneksi.** Salin `.env.example` menjadi `.env`, lalu sesuaikan:
   ```
   DATABASE_URL=postgresql://USER:PASSWORD@localhost:5432/pancasila_tech
   ```

4. **Isi database**
   ```bash
   python db_init.py
   ```
   Perintah ini aman dijalankan berulang (tabel lama dihapus dan dibuat ulang).

5. **Bangun situs**
   ```bash
   python build.py
   ```

6. **Lihat hasilnya**
   ```bash
   python -m http.server -d dist 8000
   ```
   Buka http://localhost:8000

## Halaman yang dihasilkan

| File | Isi |
|---|---|
| `index.html` | Beranda dan ringkasan lima sila |
| `kedudukan-fungsi.html` | Kedudukan dan fungsi Pancasila |
| `sila-1.html` ... `sila-5.html` | Makna, contoh penerapan, dan studi kasus tiap sila |
| `studi-kasus.html` | Seluruh studi kasus ilustratif |
| `panduan.html` | Panduan praktis harian |
| `referensi.html` | Dasar hukum dan bacaan |

## Mengubah konten

Contoh menambah studi kasus:

```sql
INSERT INTO studi_kasus (sila_nomor, judul, situasi, tinjauan, tindakan)
VALUES (2, 'Judul kasus', 'Situasi...', 'Tinjauan...', 'Tindakan...');
```

Lalu jalankan ulang `python build.py`. Jika memakai `db_init.py`, data akan kembali ke isi `seed.sql`, jadi simpan perubahan permanen di `seed.sql` atau edit langsung database tanpa menjalankan `db_init.py` lagi.

## Catatan

- Font memakai Google Fonts; tanpa internet, tampilan tetap berjalan dengan font bawaan sistem.
- Periksa teks resmi peraturan pada peraturan.go.id sebelum dikutip untuk keperluan formal.
