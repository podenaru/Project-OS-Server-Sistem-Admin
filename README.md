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
### 1. Web Server dengan Nginx (Panel Manajemen)
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

### 2. Database Server (MariaDB/MySQL)
- Instal MariaDB:
```bash
sudo apt install mariadb-server -y
```
- Amankan instalasi:
```bash
sudo mysql_secure_installation
```
- Buat database untuk server FiveM:
```bash
sudo mysql -u root -p
CREATE DATABASE fivem_db;
CREATE USER 'fivem_user'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON fivem_db.* TO 'fivem_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 3. Redis Server
- Instal Redis:
```bash
sudo apt install redis -y
sudo systemctl start redis
sudo systemctl enable redis
```
- Verifikasi Redis berjalan:
```bash
redis-cli ping
```

### 4. FTP/SFTP Server (ProFTPD)
- Instal ProFTPD:
```bash
sudo apt install proftpd -y
sudo systemctl start proftpd
sudo systemctl enable proftpd
```
- Tambahkan pengguna FTP untuk server FiveM:
```bash
sudo adduser fivemftp
sudo usermod -aG www-data fivemftp
```

### 5. FiveM Game Server
- Buat direktori server:
```bash
mkdir -p ~/fivem/server
cd ~/fivem/server
```
- Unduh dan ekstrak FiveM:
```bash
wget https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/latest.tar.xz
tar -xvf latest.tar.xz
```
- Unduh cfx-server-data:
```bash
cd ~
git clone https://github.com/citizenfx/cfx-server-data.git fivem-data
```
- Buat file server.cfg:
```bash
nano ~/fivem-data/server.cfg
```
- Contoh isi:
```bash
sv_hostname "Nama Server Anda"
sv_maxclients 32
sv_licenseKey "KEY_LISENSI_ANDA"

endpoint_add_tcp "0.0.0.0:30120"
endpoint_add_udp "0.0.0.0:30120"

ensure mapmanager
ensure chat
ensure spawnmanager
ensure sessionmanager
ensure scoreboard
```
- Jalankan server FiveM:
```bash
cd ~/fivem/server
screen -S fivem ./run.sh +exec ~/fivem-data/server.cfg
```

## Langkah 3: Konfigurasi Firewall
- Buka port yang diperlukan:
```bash
sudo ufw allow 80    # Nginx
sudo ufw allow 30120 # FiveM
sudo ufw allow 3306  # MariaDB
sudo ufw allow 6379  # Redis
sudo ufw allow 21    # FTP
sudo ufw enable
```
