# Moodle Real Environment

Folder ini berisi konfigurasi Docker Compose untuk menjalankan environment pengembangan Moodle secara lokal. Konfigurasi pada folder ini merupakan adaptasi dari konfigurasi yang ada pada folder `/home/sandy/Documents/KP SABI/moodle`.

## Prasyarat
- **Docker** dan **Docker Compose** telah terinstall di sistem.
- Source code Moodle harus berada di dalam folder ini (`/home/sandy/Documents/KP SABI/moodle_real`), karena path ini diatur sebagai document root (`/var/www/html`) untuk container webserver.

## Detail Konfigurasi
Konfigurasi utama diatur melalui file `.env` dan `docker-compose.yml`:
- **Web Server**: Apache dengan PHP 8.3 (dapat diakses pada port **8000**)
- **Database**: PostgreSQL 16
  - **Host**: `db`
  - **Database Name**: `moodle`
  - **Database User**: `moodle`
  - **Database Password**: `m@0dl3ing`
- **Testing Tools**: Dilengkapi dengan container `exttests` dan `selenium` (Firefox) untuk keperluan pengujian Moodle.

## Langkah-langkah Penggunaan (Step-by-step)

### 1. Persiapkan Source Code Moodle
Pastikan source code Moodle sudah berada di dalam folder ini. Jika folder ini masih kosong (hanya berisi file konfigurasi), Anda perlu menyalin atau meng-clone Moodle ke folder ini.
```bash
# Contoh untuk melakukan clone Moodle:
# git clone -b MOODLE_404_STABLE git://git.moodle.org/moodle.git .
```

### 2. Jalankan Container Moodle
Gunakan Docker Compose untuk membangun dan menjalankan seluruh environment di background (detached mode).
```bash
docker compose up -d --build
```
Proses ini akan mengunduh image yang diperlukan (jika belum ada) dan menjalankan container `webserver`, `db`, `exttests`, dan `selenium`.

### 3. Cek Status Container
Pastikan seluruh container (terutama `webserver` dan `db`) berjalan dengan status **Up**.
```bash
docker compose ps
```

### 4. Akses Web Moodle
Buka web browser dan akses alamat berikut untuk membuka Moodle:
[http://localhost:8000](http://localhost:8000)

### 5. Instalasi Moodle (Untuk Penggunaan Pertama Kali)
Jika ini adalah pertama kalinya Anda menjalankan environment dan belum melakukan instalasi Moodle, Anda akan diarahkan ke halaman instalasi web. Saat diminta memasukkan konfigurasi database, gunakan detail berikut:
- **Type**: PostgreSQL (`pgsql`)
- **Database host**: `db`
- **Database name**: `moodle`
- **Database user**: `moodle`
- **Database password**: `m@0dl3ing`
- **Database port**: `5432` (biarkan kosong/default)

*(Alternatif: Anda juga bisa menjalankan instalasi melalui CLI di dalam container `webserver`)*.

### 6. Menghentikan Environment
Jika Anda sudah selesai bekerja dan ingin menghentikan Moodle beserta database-nya, jalankan perintah berikut:
```bash
docker compose down
```

*(Opsional)* Jika Anda ingin menghapus semua data/container sepenuhnya termasuk volume database (misalnya ingin mereset instalasi dari awal), gunakan:
```bash
docker compose down -v
```
