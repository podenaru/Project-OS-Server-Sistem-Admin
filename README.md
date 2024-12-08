# Project-OS-Server-Sistem-Admin
disini saya akan membuat web server dengan 5 layanan server menggunakan Linux khusus untuk keperluan GTA FiveM, kita dapat mengatur layanan yang meliputi:
  1. Web Panel: Untuk mengelola server FiveM melalui antarmuka web.
  2. Database Server: Menyimpan data pemain atau skrip (menggunakan MariaDB/MySQL).
  3. Redis Server: Untuk cache dan performa.
  4. File Transfer Server (FTP/SFTP): Mengelola file server FiveM.
  5. FiveM Game Server: Layanan utama yang menjalankan GTA FiveM.

## Langkah 1: Persiapan Awal
  1. Update Server
```bash
sudo apt update && sudo apt upgrade -y
```
  2. Instal Paket Dasar
```bash
sudo apt install nginx mariadb-server redis screen wget curl git unzip -y
```

## Langkah 2: Instal dan Konfigurasi 5 Layanan
  1. Web Server dengan Nginx (Panel Manajemen)
     -- Instal Nginx dan pastikan berjalan:
```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```
    --sjdfbnf
