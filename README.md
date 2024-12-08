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
- Instal Nginx dan pastikan berjalan:
```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
```
- Buat folder web root untuk panel:
```bash
sudo mkdir -p /var/www/fivem-panel
sudo chown -R $USER:$USER /var/www/fivem-panel
```
- Edit konfigurasi Nginx:
```bash
sudo nano /etc/nginx/sites-available/fivem
```
- Tambahkan:
```bash
server {
    listen 80;
    server_name yourdomain.com;

    root /var/www/fivem-panel;
    index index.html index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```
- Aktifkan konfigurasi:
```bash
sudo ln -s /etc/nginx/sites-available/fivem /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```
