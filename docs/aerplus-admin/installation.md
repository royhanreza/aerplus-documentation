---
sidebar_position: 4
---

# Installation

Panduan lengkap untuk menginstal dan setup Aerplus Admin (Back-End) di server Anda.

## Prerequisites

Pastikan Anda telah memenuhi semua [System Requirements](./system-requirements) sebelum melanjutkan instalasi.

## Step 1: Clone Repository

Clone repository Aerplus Admin ke direktori server Anda:

```bash
git clone <repository-url> aerplus-admin
cd aerplus-admin
```

## Step 2: Install PHP Dependencies

Install semua PHP dependencies menggunakan Composer:

```bash
composer install
```

Jika Anda menginstall di production, gunakan:

```bash
composer install --no-dev --optimize-autoloader
```

## Step 3: Install Node Dependencies

Install JavaScript dependencies:

```bash
npm install
# atau
yarn install
# atau
pnpm install
```

## Step 4: Environment Configuration

Copy file environment example:

```bash
cp .env.example .env
```

Edit file `.env` dan sesuaikan konfigurasi berikut:

```env
APP_NAME="Aerplus Admin"
APP_ENV=production
APP_KEY=
APP_DEBUG=false
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=aerplus_db
DB_USERNAME=your_username
DB_PASSWORD=your_password

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

QUEUE_CONNECTION=redis

CACHE_DRIVER=redis
SESSION_DRIVER=redis
```

Generate application key:

```bash
php artisan key:generate
```

## Step 5: Database Setup

### Create Database

Buat database baru di MySQL:

```sql
CREATE DATABASE aerplus_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Run Migrations

Jalankan database migrations:

```bash
php artisan migrate
```

### Seed Database (Opsional)

Jika ada seeder untuk data awal:

```bash
php artisan db:seed
```

Atau untuk development dengan dummy data:

```bash
php artisan db:seed --class=DatabaseSeeder
```

## Step 6: Storage & Cache Setup

Buat symbolic link untuk storage:

```bash
php artisan storage:link
```

Set permissions untuk storage dan cache:

```bash
chmod -R 775 storage bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache
```

*Catatan: Sesuaikan `www-data` dengan user web server Anda*

## Step 7: Build Assets

Compile frontend assets menggunakan Vite:

Untuk production:

```bash
npm run build
```

Untuk development dengan Hot Module Replacement (HMR):

```bash
npm run dev
```

**Catatan**: Vite akan otomatis watch perubahan file dan melakukan hot reload saat development.

## Step 8: Cache Configuration

Optimize Laravel untuk production dengan cache configuration:

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

## Step 9: Laravel Horizon Setup

Aerplus Admin menggunakan Laravel Horizon untuk monitoring dan management queue. Setup Horizon:

### Publish Horizon Assets

```bash
php artisan horizon:install
```

### Publish Horizon Config (jika diperlukan)

```bash
php artisan vendor:publish --tag=horizon-config
```

### Run Migrations untuk Horizon

```bash
php artisan migrate
```

Horizon akan menggunakan database untuk menyimpan monitoring data.

## Step 10: Queue Worker Setup

Aerplus Admin menggunakan Laravel Horizon untuk queue management. Setup Horizon:

### Install Supervisor (Required)

Install Supervisor jika belum terinstall:

```bash
# Ubuntu/Debian
sudo apt-get install supervisor

# CentOS/RHEL
sudo yum install supervisor
```

### Menggunakan Laravel Horizon (Recommended)

Horizon menyediakan dashboard untuk monitoring queue. Akses Horizon di:

```
http://your-domain/horizon
```

**Catatan**: Pastikan untuk mengatur authentication untuk Horizon di production (edit `app/Providers/HorizonServiceProvider.php`).

Jalankan Horizon:

```bash
php artisan horizon
```

Untuk production, gunakan Supervisor untuk menjalankan Horizon:

```bash
sudo nano /etc/supervisor/conf.d/aerplus-horizon.conf
```

Tambahkan konfigurasi:

```ini
[program:aerplus-horizon]
process_name=%(program_name)s
command=php /path/to/aerplus-admin/artisan horizon
autostart=true
autorestart=true
user=www-data
redirect_stderr=true
stdout_logfile=/path/to/aerplus-admin/storage/logs/horizon.log
stopwaitsecs=3600
```

Reload supervisor:

```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start aerplus-horizon
```

### Manual Queue Worker (Alternatif)

Jika tidak menggunakan Horizon, gunakan queue worker manual:

```bash
php artisan queue:work redis
```

## Step 11: Web Server Configuration

### Apache Configuration

Pastikan mod_rewrite enabled:

```bash
sudo a2enmod rewrite
```

Buat virtual host configuration:

```apache
<VirtualHost *:80>
    ServerName aerplus-admin.local
    DocumentRoot /path/to/aerplus-admin/public

    <Directory /path/to/aerplus-admin/public>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/aerplus-error.log
    CustomLog ${APACHE_LOG_DIR}/aerplus-access.log combined
</VirtualHost>
```

Enable site dan restart Apache:

```bash
sudo a2ensite aerplus-admin.conf
sudo systemctl restart apache2
```

### Nginx Configuration

Buat Nginx configuration:

```nginx
server {
    listen 80;
    server_name aerplus-admin.local;
    root /path/to/aerplus-admin/public;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    index index.php;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

Reload Nginx:

```bash
sudo nginx -t
sudo systemctl reload nginx
```

## Step 12: Schedule Setup (Cron Job)

Laravel scheduler memerlukan cron job. Tambahkan ke crontab:

```bash
crontab -e
```

Tambahkan baris berikut:

```cron
* * * * * cd /path/to/aerplus-admin && php artisan schedule:run >> /dev/null 2>&1
```

## Step 13: Verify Installation

### Check Application Status

Akses aplikasi di browser:

```
http://your-server-ip
```

Atau jika menggunakan domain:

```
http://aerplus-admin.local
```

### Check Queue Status

Verifikasi queue worker berjalan:

```bash
sudo supervisorctl status
```

### Check Logs

Periksa log untuk error:

```bash
tail -f storage/logs/laravel.log
```

## Troubleshooting

### Permission Issues

Jika ada permission error:

```bash
sudo chown -R www-data:www-data storage bootstrap/cache
sudo chmod -R 775 storage bootstrap/cache
```

### Cache Issues

Clear semua cache:

```bash
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

### Database Connection Error

Pastikan:
- Database sudah dibuat
- Credentials di `.env` benar
- MySQL service berjalan
- User database memiliki privileges yang cukup

### Redis Connection Error

Pastikan:
- Redis service berjalan
- Redis host dan port di `.env` benar
- Redis password (jika ada) sudah diset

## Development vs Production

### Development

- `APP_DEBUG=true` di `.env`
- Jangan cache config, route, view
- Gunakan `npm run watch` untuk auto-compile
- Queue worker bisa dijalankan manual

### Production

- `APP_DEBUG=false` di `.env`
- Cache semua config, route, view
- Build assets dengan `npm run production`
- Setup supervisor untuk queue workers
- Setup proper file permissions
- Enable HTTPS/SSL

## Next Steps

Setelah instalasi selesai:

1. **[Configuration](./configuration)** - Konfigurasi aplikasi sesuai kebutuhan
2. Buat user admin pertama
3. Setup backup database
4. Konfigure monitoring dan logging

---

**Selanjutnya**: [Configuration](./configuration)
