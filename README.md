# INVENTORYPRO - Sistem Manajemen Persediaan Barang

[![PHP Version](https://img.shields.io/badge/PHP-7.4%20%7C%208.x-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://www.php.net/)
[![MySQL](https://img.shields.io/badge/MySQL-InnoDB-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)](https://www.chartjs.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

**INVENTORYPRO** adalah aplikasi web manajemen inventaris dan pergudangan modern berbasis PHP & MySQL yang dirancang khusus untuk memonitor stok produk, pencatatan barang masuk & keluar (*stock in/out*), rekam jejak audit (*audit trail*), visualisasi statistik, serta pembuatan laporan persediaan barang.

---

## 🌟 Fitur Unggulan

### 1. Role-Based Access Control (RBAC) & Multi-User
- **Administrator**: Memiliki hak akses penuh untuk mengelola master produk (CRUD), melihat valuasi total aset gudang, ringkasan nilai per kategori, dan mencetak laporan persediaan resmi.
- **Staff Gudang**: Akses operasional untuk melakukan mutasi barang masuk (*Stock In*), barang keluar (*Stock Out*), dan memantau riwayat transaksi serta stok persediaan.

### 2. Dashboard Interaktif & Analitik Real-Time
- Kartu metrik agregat: Total Produk, Peringatan Stok Kritis (*Low Stock*), Total Barang Masuk, Total Barang Keluar, dan Total Transaksi.
- **Valuasi Inventaris (IDR)** otomatis (*khusus Admin*).
- **Grafik Interaktif Arus Barang 7 Hari Terakhir** menggunakan Chart.js (perbandingan volume In vs Out harian).
- Tabel mutasi terkini (*recent activity*) dan daftar peringatan produk dengan stok menipis (*critical alerts*).
- Ringkasan statistik dan valuasi per kategori produk (*khusus Admin*).

### 3. Manajemen Master Produk
- Tambah, ubah, dan hapus data produk (SKU unik, Nama, Kategori, Harga Satuan, Stok Fisik, Batas Minimum Stok).
- Pencarian cerdas berbasis SKU maupun Nama Produk.
- Filter berdasarkan Kategori dan Status Stok (*Aman*, *Kritis/Menipis*, *Habis/Kosong*).
- Fitur pengurutan data (*Sorting*) dan *Pagination*.

### 4. Transaksi Barang Masuk & Keluar (ACID Compliant)
- **Barang Masuk (Stock In)**: Menambah kuantitas stok produk dan mencatat histori transaksi.
- **Barang Keluar (Stock Out)**: Mengurangi stok dengan validasi ketat (*mencegah stok bernilai negatif*).
- Menggunakan **Database Transaction** (`BEGIN`, `COMMIT`, `ROLLBACK`) dan **Row-Level Locking** (`SELECT ... FOR UPDATE`) untuk mencegah *race condition* pada saat mutasi stok bersamaan.

### 5. Riwayat Audit & Cetak Laporan (Print-Ready)
- Log lengkap setiap transaksi: Tanggal/Waktu, Kode SKU, Nama Produk, Tipe Transaksi, Jumlah, Penanggung Jawab (User), dan Catatan.
- Filter riwayat berdasarkan rentang waktu: **Semua**, **Harian**, **Mingguan**, **Bulanan**, dan **Tahunan**.
- Filter tipe mutasi (**IN** / **OUT**) dan pencarian kata kunci.
- Fitur **Cetak Laporan / Export PDF** khusus Admin yang ramah cetak (*print-optimized CSS*) lengkap dengan tanda tangan pengesahan dan rekapitulasi data.

---

## 🛡️ Aspek Keamanan (Security)

Aplikasi ini dibangun dengan menerapkan standar keamanan web:
- **Prepared Statements (PDO)**: Mencegah celah kerentanan *SQL Injection*.
- **CSRF Protection**: Verifikasi token *Cross-Site Request Forgery* pada setiap pengiriman formulir POST.
- **XSS Sanitization**: Proteksi *Cross-Site Scripting* melalui fungsi *escaping* `htmlspecialchars()`.
- **Password Hashing**: Menggunakan algoritma enkripsi standar PHP `PASSWORD_DEFAULT` (Bcrypt).
- **Session Security**: Penerapan `session_regenerate_id()` saat login untuk menghindari *Session Fixation*.
- **Sorting Whitelisting**: Membatasi parameter sorting hanya pada kolom yang diizinkan untuk mencegah injeksi query dinamis.

---

## 🛠️ Teknologi yang Digunakan

| Komponen | Teknologi |
| :--- | :--- |
| **Backend** | PHP (Native PDO) |
| **Database** | MySQL / MariaDB (InnoDB Engine) |
| **Frontend Styling** | Tailwind CSS (CDN) |
| **Icons & Typography** | FontAwesome 6, Google Fonts (*Plus Jakarta Sans*) |
| **Visualisasi Data** | Chart.js |
| **Server Rekomendasi**| Apache (XAMPP / Laragon / LAMP) |

---

## 📁 Struktur Direktori

```text
inventory-app/
├── config/
│   └── database.php          # Konfigurasi koneksi database PDO
├── database/
│   └── schema.sql            # Struktur tabel, relasi, dan data awal (seeder)
├── helpers/
│   ├── auth.php              # Helper otentikasi & pengecekan role (RBAC)
│   ├── escape.php            # Helper XSS protection & CSRF token generator
│   └── validation.php        # Helper validasi & sanitasi input data
├── public/
│   ├── products/
│   │   ├── create.php        # Form tambah produk baru (Admin)
│   │   ├── delete.php        # Logika hapus produk (Admin)
│   │   ├── edit.php          # Form ubah data produk (Admin)
│   │   └── index.php         # Katalog master produk & filter
│   ├── stock/
│   │   ├── history.php       # Riwayat log mutasi stok
│   │   ├── in.php            # Form pencatatan barang masuk
│   │   ├── out.php           # Form pencatatan barang keluar
│   │   └── print.php         # Cetak laporan resmi / print view (Admin)
│   ├── dashboard.php         # Halaman utama & analitik
│   ├── login.php             # Form autentikasi pengguna
│   ├── logout.php            # Proses keluar dari sistem
│   └── reset.php             # Skrip reset password default
└── views/
    ├── header.php            # Layout navigasi atas, sidebar & styling
    └── footer.php            # Penutup layout & script pendukung
