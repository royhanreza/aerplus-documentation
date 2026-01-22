---
sidebar_position: 7
---

# Configuration

Panduan konfigurasi Aerplus Admin (Back-End) setelah instalasi.

## Environment Configuration

File `.env` adalah file konfigurasi utama untuk aplikasi. Berikut adalah penjelasan konfigurasi penting:

### Application Settings

```env
APP_NAME="Aerplus Admin"
APP_ENV=production
APP_KEY=base64:your-generated-key
APP_DEBUG=false
APP_URL=https://admin.aerplus.com
APP_TIMEZONE=Asia/Jakarta
APP_LOCALE=id
APP_FALLBACK_LOCALE=en
APP_FAKER_LOCALE=id_ID
```

**Penjelasan:**
- `APP_NAME`: Nama aplikasi
- `APP_ENV`: Environment (local, staging, production)
- `APP_KEY`: Encryption key (generate dengan `php artisan key:generate`)
- `APP_DEBUG`: Enable/disable debug mode (false untuk production)
- `APP_URL`: URL aplikasi
- `APP_TIMEZONE`: Timezone aplikasi
- `APP_LOCALE`: Bahasa default
- `APP_FALLBACK_LOCALE`: Bahasa fallback jika translation tidak tersedia

### Database Configuration

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=aerplus_db
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

**Tips:**
- Gunakan koneksi terpisah untuk read dan write jika menggunakan replication
- Pastikan user database memiliki privileges yang cukup
- Gunakan strong password untuk production

### Redis Configuration

```env
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

REDIS_CLIENT=phpredis
REDIS_DB=0
```

**Untuk Cache:**
```env
CACHE_DRIVER=redis
CACHE_PREFIX=aerplus_cache
```

**Untuk Session:**
```env
SESSION_DRIVER=redis
SESSION_LIFETIME=120
```

**Untuk Queue:**
```env
QUEUE_CONNECTION=redis
```

### Mail Configuration

```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="noreply@aerplus.com"
MAIL_FROM_NAME="${APP_NAME}"
```

**Contoh untuk Gmail:**
```env
MAIL_MAILER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_ENCRYPTION=tls
```

### File Storage

```env
FILESYSTEM_DISK=local
```

**Untuk S3 (Production):**
```env
FILESYSTEM_DISK=s3
AWS_ACCESS_KEY_ID=your-key
AWS_SECRET_ACCESS_KEY=your-secret
AWS_DEFAULT_REGION=ap-southeast-1
AWS_BUCKET=aerplus-storage
AWS_USE_PATH_STYLE_ENDPOINT=false
```

**Catatan**: Aerplus Admin menggunakan `league/flysystem-aws-s3-v3` untuk S3 integration.

## Application Configuration Files

### config/app.php

Konfigurasi utama aplikasi. Biasanya tidak perlu diubah langsung karena sudah menggunakan environment variables.

### config/database.php

Konfigurasi database connections. Bisa menambahkan multiple connections jika diperlukan.

### config/cache.php

Konfigurasi caching. Default menggunakan Redis.

### config/queue.php

Konfigurasi queue. Pastikan menggunakan Redis untuk production.

### config/filesystems.php

Konfigurasi file storage. Bisa menambahkan disk baru untuk storage yang berbeda.

## Security Configuration

### Password Requirements

Edit `config/auth.php` untuk mengatur password requirements:

```php
'passwords' => [
    'users' => [
        'provider' => 'users',
        'table' => 'password_resets',
        'expire' => 60,
        'throttle' => 60,
    ],
],
```

### Session Configuration

Edit `config/session.php`:

```php
'lifetime' => env('SESSION_LIFETIME', 120),
'expire_on_close' => false,
'encrypt' => false,
'secure' => env('SESSION_SECURE_COOKIE', true), // HTTPS only
'http_only' => true,
'same_site' => 'lax',
```

### CORS Configuration

Edit `config/cors.php` untuk API:

```php
'paths' => ['api/*', 'sanctum/csrf-cookie'],
'allowed_methods' => ['*'],
'allowed_origins' => ['https://your-frontend-domain.com'],
'allowed_headers' => ['*'],
'exposed_headers' => [],
'max_age' => 0,
'supports_credentials' => true,
```

## Performance Configuration

### Cache Configuration

Enable caching untuk production:

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

### Queue Configuration

Setup multiple queue workers untuk performa lebih baik. Edit supervisor config:

```ini
numprocs=4  # Jumlah worker processes
```

### Database Optimization

Enable query caching di `config/database.php`:

```php
'mysql' => [
    // ...
    'options' => [
        PDO::ATTR_EMULATE_PREPARES => true,
    ],
],
```

## Feature Flags

Gunakan environment variables untuk enable/disable features:

```env
FEATURE_EXPORT_ENABLED=true
FEATURE_IMPORT_ENABLED=true
FEATURE_NOTIFICATION_ENABLED=true
```

Kemudian di code:

```php
if (config('features.export_enabled')) {
    // Export feature code
}
```

## Third-Party Integrations

### Payment Gateway

```env
PAYMENT_GATEWAY=midtrans
MIDTRANS_SERVER_KEY=your-server-key
MIDTRANS_CLIENT_KEY=your-client-key
MIDTRANS_IS_PRODUCTION=false
```

### SMS Gateway

```env
SMS_PROVIDER=twilio
TWILIO_SID=your-sid
TWILIO_TOKEN=your-token
TWILIO_FROM=+1234567890
```

### Other Services

Tambahkan konfigurasi sesuai kebutuhan integrasi Anda.

## Logging Configuration

Edit `config/logging.php` untuk mengatur logging:

```php
'channels' => [
    'daily' => [
        'driver' => 'daily',
        'path' => storage_path('logs/laravel.log'),
        'level' => env('LOG_LEVEL', 'debug'),
        'days' => 14,
    ],
],
```

Set di `.env`:

```env
LOG_CHANNEL=daily
LOG_LEVEL=error  # Production: error, Development: debug
```

## Maintenance Mode

Enable maintenance mode:

```bash
php artisan down
```

Dengan message:

```bash
php artisan down --message="Sedang maintenance"
```

Allow specific IP:

```bash
php artisan down --allow=127.0.0.1
```

Disable maintenance mode:

```bash
php artisan up
```

## Backup Configuration

### Database Backup

Setup cron job untuk backup otomatis:

```bash
# Backup database setiap hari jam 2 pagi
0 2 * * * mysqldump -u username -p password aerplus_db > /backup/aerplus_$(date +\%Y\%m\%d).sql
```

### File Backup

Backup storage folder:

```bash
# Backup storage setiap hari
0 3 * * * tar -czf /backup/storage_$(date +\%Y\%m\%d).tar.gz /path/to/storage
```

## Monitoring Configuration

### Laravel Horizon (Queue Monitoring)

Aerplus Admin menggunakan Laravel Horizon untuk monitoring dan management queue. Horizon sudah terinstall sebagai dependency.

#### Horizon Configuration

Edit `config/horizon.php` untuk mengatur:

```php
'environments' => [
    'production' => [
        'supervisor-1' => [
            'connection' => 'redis',
            'queue' => ['default'],
            'balance' => 'auto',
            'processes' => 10,
            'tries' => 3,
            'timeout' => 60,
        ],
    ],
],
```

#### Horizon Authentication

Untuk production, pastikan Horizon dilindungi dengan authentication. Edit `app/Providers/HorizonServiceProvider.php`:

```php
protected function gate()
{
    Gate::define('viewHorizon', function ($user) {
        return in_array($user->email, [
            'admin@aerplus.com',
        ]);
    });
}
```

#### Access Horizon Dashboard

Akses Horizon dashboard di:

```
http://your-domain/horizon
```

Dashboard ini menampilkan:
- Queue metrics dan statistics
- Job status dan failures
- Worker status
- Throughput dan wait times

## Environment-Specific Configuration

### Local Development

```env
APP_ENV=local
APP_DEBUG=true
LOG_LEVEL=debug
```

### Staging

```env
APP_ENV=staging
APP_DEBUG=false
LOG_LEVEL=info
```

### Production

```env
APP_ENV=production
APP_DEBUG=false
LOG_LEVEL=error
SESSION_SECURE_COOKIE=true
```

## Configuration Best Practices

1. **Jangan commit `.env` file** - Gunakan `.env.example` sebagai template
2. **Gunakan environment variables** - Jangan hardcode values di config files
3. **Cache config di production** - Gunakan `php artisan config:cache`
4. **Rotate logs regularly** - Setup log rotation untuk menghindari disk penuh
5. **Secure sensitive data** - Gunakan encryption untuk sensitive configuration
6. **Document custom configs** - Dokumentasikan konfigurasi custom yang dibuat

## Verifying Configuration

Test konfigurasi setelah perubahan:

```bash
# Test database connection
php artisan tinker
>>> DB::connection()->getPdo();

# Test Redis connection
>>> Redis::ping();

# Test mail configuration
php artisan tinker
>>> Mail::raw('Test', function($msg) { $msg->to('test@example.com')->subject('Test'); });
```

## Troubleshooting Configuration

### Config Not Updating

Clear config cache:

```bash
php artisan config:clear
```

### Environment Variables Not Loading

Pastikan:
- File `.env` ada di root project
- Tidak ada whitespace di sekitar `=`
- Restart web server setelah perubahan

### Cache Issues

Clear semua cache:

```bash
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear
```

---

**Kembali ke**: [Introduction](./intro)
