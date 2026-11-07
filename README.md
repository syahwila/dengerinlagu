# DENGERINLAGU — All-in-One Music Portal
> **"Semua Musik, Satu Tempat."**  
> Domain Target: [https://dengerinlagu.co-id.id](https://dengerinlagu.co-id.id)

---

## 🎵 Ringkasan Arsitektur Proyek
- **Frontend Produksi**: React 19 + TypeScript + Tailwind CSS (dikompilasi ke file statis di `/dist`).
- **Backend Produksi**: Native PHP 8.2+ REST API dengan PDO MySQL, Secure Sessions, dan CSRF protection.
- **Database Engine**: MySQL 8.0+ / MariaDB 10.5+ (Engine InnoDB, Charset `utf8mb4`, Collation `utf8mb4_unicode_ci`).
- **Web Server**: Apache 2.4+ dengan mod_rewrite (`.htaccess`).
- **Hosting Target**: cPanel Shared Hosting standar (tanpa dependensi server Node.js permanen).

---

## 📂 Struktur Berkas Proyek

```text
dengerinlagu/
├── dist/                     <- Berkas statis hasil kompilasi React frontend
│   ├── assets/               <- Berkas JS, CSS, gambar terkompilasi
│   └── index.html            <- Entry point SPA utama
├── backend/                  <- Native PHP 8.2 REST API
│   ├── api/
│   │   └── index.php         <- Master REST API Router (/api/*)
│   ├── config/
│   │   ├── database.php      <- Koneksi PDO MySQL
│   │   └── app.php           <- Pengaturan CORS, Header keamanan, & Limits
│   ├── includes/
│   │   ├── Auth.php          <- PHP Secure Session & RBAC
│   │   ├── Response.php      <- JSON Response formatter standar
│   │   └── RateLimiter.php   <- Pembatas laju permintaan login
│   ├── templates/
│   │   └── seo_ssr.php       <- Server-Side HTML Renderer untuk Googlebot / Medsos
│   └── uploads/              <- Folder berkas media yang dapat ditulis
├── database/
│   ├── schema.sql            <- Skema 21 tabel relasional MySQL lengkap
│   └── seed.sql              <- Data peragaan awal (Lagu, Artis, Lirik, Chart, Berita)
├── deployment/
│   ├── .htaccess             <- Aturan rewrite Apache, HTTPS, & Cache
│   └── install.php           <- Wizard instalasi web otomatis
├── .htaccess                 <- Salinan .htaccess di root public_html
├── install.php               <- Entry installer root
├── package.json
├── vite.config.ts
└── README.md
```

---

## 🚀 Panduan Instalasi Lengkap di cPanel Shared Hosting

### Langkah 1: Kompilasi Frontend
Jalankan perintah ini di lingkungan pengembangan untuk mengompilasi frontend:
```bash
npm run build
```
Hasil kompilasi akan otomatis dibuat di folder `/dist`.

### Langkah 2: Buat Database MySQL di cPanel
1. Masuk ke **cPanel** akun hosting Anda.
2. Buka menu **MySQL Databases** atau **MySQL Database Wizard**.
3. Buat database baru (misal: `cpaneluser_dengerin`).
4. Buat user MySQL baru (misal: `cpaneluser_dbuser`) dan buat password yang kuat.
5. Hubungkan user ke database dengan mencentang **ALL PRIVILEGES**.

### Langkah 3: Unggah Berkas ke `public_html`
1. Buka **cPanel File Manager** dan buka folder `public_html` (atau folder subdomain Anda).
2. Pastikan opsi **Show Hidden Files (dotfiles)** telah diaktifkan di File Manager Settings.
3. Unggah seluruh isi folder berikut:
   - Salin isi dari `/dist` langsung ke root `public_html` (`index.html`, folder `assets/`).
   - Salin folder `backend/` ke `public_html/backend/`.
   - Salin folder `database/` ke `public_html/database/`.
   - Salin berkas `.htaccess` dan `install.php` ke root `public_html`.
4. Berikan izin tulis (chmod 755 atau 775) pada folder `backend/config` dan `backend/uploads`.

### Langkah 4: Jalankan Setup Wizard Otomatis
Buka browser dan akses tautan installer:
```text
https://dengerinlagu.co-id.id/install.php
```
1. Wizard akan memeriksa versi PHP (minimal PHP 8.2) dan ekstensi `pdo_mysql`.
2. Masukkan nama database, username database, dan password MySQL dari Langkah 2.
3. Tentukan nama dan password untuk akun **Super Administrator**.
4. Klik **Mulai Instalasi Database**. Wizard akan secara otomatis mengimpor `schema.sql`, `seed.sql`, dan menulis file `backend/config/database.php`.

### Langkah 5: Pengamanan Pasca-Instalasi (PENTING)
Setelah instalasi selesai:
1. Hapus file `install.php` dan `deployment/install.php` dari File Manager cPanel.
2. Masuk ke Admin Dashboard via tombol **Admin Panel** di pojok kanan atas dengan akun Super Admin Anda:
   - **Username**: `admin` (atau email yang Anda daftarkan)
   - **Password**: Password yang Anda masukkan saat instalasi (Default: `AdminPass123!`)

---

## 🛡️ Fitur Keamanan yang Diimplementasikan
1. **Password Hashing**: Menggunakan algoritma `PASSWORD_BCRYPT` native PHP.
2. **PDO Prepared Statements**: Mencegah serangan SQL Injection pada seluruh kueri database.
3. **Session Hardening**: Cookie sesi dikonfigurasi dengan flag `HttpOnly`, `SameSite=Lax`, dan `Secure` (HTTPS).
4. **Proteksi File Sensitif**: Konfigurasi Apache `.htaccess` memblokir akses langsung ke berkas `.env`, `*.sql`, `backend/config/`, dan `.git`.
5. **SEO Server-Side Crawling**: Googlebot, Bingbot, dan bot preview WhatsApp/Telegram diarahkan ke `backend/templates/seo_ssr.php` untuk merender lirik dan metadata Schema.org secara server-side tanpa butuh JavaScript.

---

## ⚖️ Kepatuhan Lisensi & Hak Cipta
- Seluruh lirik lagu yang disajikan adalah konten resmi berlisensi dan dilengkapi atribusi pencipta lagu/komposer.
- Tidak ada scraping lirik tanpa izin atau penyediaan file MP3 bajakan.
- Audio pratinjau menggunakan sampel berlisensi resmi dan embed resmi (Spotify/YouTube legal embeds).

---

*© 2024 DENGERINLAGU — Semua Musik, Satu Tempat.*
