# KodePromo — Mesin Voucher Diskon REST API & E-Commerce Demo

Repository Tugas Besar 1 Mata Kuliah Komputasi Awan / DevOps (Studi Kasus **K08 — KodePromo**).

## Struktur Repository
- `api/` — Aplikasi utama KodePromo REST API berbasis PHP & Laravel (pusat seluruh logika dan aturan bisnis voucher).
- `ecommerce-demo/` — Aplikasi toko online sederhana sebagai client pemanggil API KodePromo.
- `docs/` — Dokumentasi kontrak API (OpenAPI), Postman Collection, dan diagram arsitektur/pipeline.
- `docker-compose.yml` — Orkestrasi container service (`api`, `db`, `ecommerce-demo`).
- `Jenkinsfile` — Pipeline CI/CD otomatis (Checkout, Test, Build Image, Push Image, Approval, Deploy, Smoke Test).

## Tim Pengembang & Pembagian Peran
- **Tech Lead / Backend:** Mubarak Subkhi Muhidin
- **DevOps Engineer:** ahmad nur abid
- **Container Engineer:** gevira
- **QA Engineer & Tech Writer:** lian

## Fitur Utama
1. Manajemen Kampanye Promosi (`CRUD /api/v1/campaigns`) dan Laporan Penggunaan (`/api/v1/campaigns/{id}/report`).
2. Manajemen Kode Voucher (`CRUD /api/v1/vouchers`).
3. Validasi Voucher (`POST /api/v1/vouchers/validate`) dan Redeem Voucher (`POST /api/v1/vouchers/redeem`) dengan aturan bisnis BR-01 s.d. BR-09.
4. Health Check Service & Database (`GET /health`).
5. Aplikasi E-Commerce Demo untuk simulasi keranjang belanja, validasi kode promo, dan checkout.

# KodePromo

**KodePromo** adalah layanan REST API voucher dan promosi berbasis Laravel (`api/`) yang terintegrasi dengan aplikasi demo e-commerce berbasis PHP (`ecommerce-demo/`) untuk pengelolaan kampanye promo, voucher, dan penukaran kode.

## Arsitektur & Komponen Proyek

- **Backend REST API (`api/`)**: Dibangun menggunakan Laravel, berjalan di `http://127.0.0.1:8000`, dan terhubung ke database relasional MySQL/MariaDB untuk menyimpan data merchant, kampanye, voucher, serta riwayat penukaran.
- **Frontend Demo E-Commerce (`ecommerce-demo/`)**: Aplikasi demo PHP yang dijalankan menggunakan PHP built-in server di `http://127.0.0.1:8085`, menyediakan halaman toko/checkout (`index.php`) serta halaman monitoring/administrasi (`monitor.php`) yang berkomunikasi dengan REST API menggunakan header `X-API-Key`.

## Prasyarat

- **PHP** (disarankan versi 8.2/8.3+) beserta ekstensi PDO MySQL
- **Composer**
- **MySQL / MariaDB** (misalnya melalui Laragon atau XAMPP)
- **Git**

## Panduan Instalasi & Menjalankan di Lokal

### 1. Clone Repository

Clone repositori ke direktori lokal lalu masuk ke folder proyek:

```bash
git clone <https://github.com/KiMiL056/kodepromo.git>
cd KodePromo-

### 2. Konfigurasi Database & Backend API

cd api
composer install
cp .env.example .env
php artisan key:generate

php artisan migrate
php artisan db:seed
php artisan serve --host=127.0.0.1 --port=8000

### 3. Menjalankan Frontend Demo

cd ecommerce-demo
php -S 127.0.0.1:8085

http://127.0.0.1:8085/index.php
