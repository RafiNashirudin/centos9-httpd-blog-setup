# Setup Website Blog dengan HTTPD (Apache) di CentOS 9

Tutorial ini akan memandu Anda untuk menginstal dan mengonfigurasi HTTPD (Apache) di CentOS 9 agar website Blog Anda dapat diakses melalui browser.

## Prasyarat
- CentOS 9 sudah terpasang.
- Akses ke terminal dengan hak akses root atau sudo.
- File aplikasi blog yang sudah siap di-host.

## Langkah-Langkah Instalasi dan Konfigurasi

### 1. Instal HTTPD (Apache)

Jalankan perintah berikut untuk menginstal HTTPD (Apache):

```bash
sudo dnf install httpd -y
```

### 2. Mulai dan Aktifkan Layanan HTTPD

Setelah HTTPD terpasang, mulai layanan HTTPD dan atur agar otomatis berjalan saat booting:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

### 3. Buka Firewall untuk HTTP dan HTTPS

Agar website bisa diakses, buka port 80 (HTTP) dan port 443 (HTTPS) pada firewall:

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

### 4. Atur Direktori untuk Website Blog

- Secara default, Apache menyimpan file website di `/var/www/html`.
- Salin file website Blog Anda ke dalam direktori tersebut.

Contoh:

```bash
sudo cp -r /path/to/your/blog /var/www/html/
```

### 5. Konfigurasi Virtual Host (Opsional)

Jika Anda ingin mengakses website Blog menggunakan domain atau subdomain (misalnya, `blog.yourdomain.com`), buat file konfigurasi virtual host baru di `/etc/httpd/conf.d/`.

Jalankan:

```bash
sudo nano /etc/httpd/conf.d/blog.conf
```

Tambahkan konfigurasi berikut:

```apache
<VirtualHost *:80>
    ServerName blog.yourdomain.com   <-- Ubah IP CentOs 9
    DocumentRoot /var/www/html/blog

    <Directory /var/www/html/blog>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog /var/log/httpd/blog_error.log
    CustomLog /var/log/httpd/blog_access.log combined
</VirtualHost>
```

Simpan dan tutup file, lalu restart Apache:

```bash
sudo systemctl restart httpd
```

### 6. Instal PHP (Jika Diperlukan)

Jika aplikasi blog membutuhkan PHP, instal PHP dan ekstensi terkait dengan perintah berikut:

```bash
sudo dnf install php php-mysqlnd php-json php-xml php-mbstring
```

Restart layanan HTTPD setelah PHP terpasang:

```bash
sudo systemctl restart httpd
```

### 7. Cek Akses Website

- Buka browser dan akses website blog Anda melalui IP server atau domain yang telah dikonfigurasi.
  
  - Jika menggunakan IP server:
    ```
    http://your-server-ip
    ```
  - Jika menggunakan domain:
    ```
    http://blog.yourdomain.com
    ```

### 8. Pengaturan Database (Jika Diperlukan)

Jika website blog menggunakan MySQL atau MariaDB, pastikan database sudah terkonfigurasi dan aplikasi bisa terhubung dengan database.

---

## Troubleshooting

- **Website tidak bisa diakses?**
  - Cek status layanan HTTPD: `sudo systemctl status httpd`.
  - Pastikan firewall sudah dibuka untuk port HTTP dan HTTPS.
  
- **File PHP tidak dieksekusi?**
  - Pastikan PHP sudah terinstal dan layanan HTTPD telah di-restart.

---

Dengan mengikuti langkah-langkah di atas, website blog Anda seharusnya sudah bisa diakses di CentOS 9 menggunakan HTTPD (Apache).
