# Project-OS-Server-Sistem-Admin
disini saya akan membuat web server dengan 5 layanan server menggunakan Linux khusus untuk keperluan GTA FiveM, kita dapat mengatur layanan yang meliputi:
1. Web Panel: Untuk mengelola server FiveM melalui antarmuka web.
2. Apache2: Web server untuk mendukung layanan dan aplikasi berbasis web.
3. Database Server: Menyimpan data pemain atau skrip (menggunakan MariaDB/MySQL).
4. Redis Server: Untuk cache dan performa.
5. File Transfer Server (FTP/SFTP): Mengelola file server FiveM.
6. FiveM Game Server: Layanan utama yang menjalankan GTA FiveM.
7. SSH ( Secure Shell) :mengakses dan mengelola server secara aman dari jarak jauh

## Spesifikasi
1. Ubuntu Server 22.04.5 LTS (Jammy Jellyfish)
2. RAM 5GB
3. Processor 4 Core
4. Storage 25GB
    
## Langkah 1: Persiapan Awal
1. Update Server
```bash
sudo apt update && sudo apt upgrade -y
```

## Langkah 2: Instal dan Konfigurasi Server Layanan
### 1. Web Server dengan Nginx (Panel Manajemen)
- Instal Apache2 dan pastikan berjalan:
```bash
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
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
wget https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/12092-c9700385dfad9008c8b7888a61d3901451c4dc29/fx.tar.xz
tar -xvf latest.tar.xz
```
- Unduh cfx-server-data:
```bash
mkdir -p ~/fivem/server-data
cd ~/fivem/server-data
git clone https://github.com/citizenfx/cfx-server-data.git
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
sudo ufw allow 80    # Apache2
sudo ufw allow 30120 # FiveM
sudo ufw allow 3306  # MariaDB
sudo ufw allow 6379  # Redis
sudo ufw allow 21    # FTP
sudo ufw enable
```

## Langkah 4: Daftar Server Key
- bikin key server dengan daftar di website https://keymaster.fivem.net/
