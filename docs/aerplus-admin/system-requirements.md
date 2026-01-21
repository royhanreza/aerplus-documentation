---
sidebar_position: 2
---

# System Requirements

Dokumen ini menjelaskan persyaratan sistem minimum yang diperlukan untuk menjalankan Aerplus Admin (Back-End).

## Server Requirements

### Operating System
- **Linux**: Ubuntu 20.04 LTS atau lebih baru, CentOS 7+, atau distribusi Linux lainnya
- **Windows**: Windows Server 2016 atau lebih baru (untuk development)
- **macOS**: macOS 10.15 atau lebih baru (untuk development)

### Web Server
- **Apache**: 2.4 atau lebih baru (dengan mod_rewrite enabled)
- **Nginx**: 1.18 atau lebih baru

### PHP Requirements
- **PHP Version**: 8.0.2 atau lebih baru (disarankan PHP 8.1+)
- **PHP Extensions**:
  - BCMath
  - Ctype
  - cURL
  - DOM
  - Fileinfo
  - JSON
  - Mbstring
  - OpenSSL
  - PDO
  - Tokenizer
  - XML
  - Zip

### Database
- **MySQL**: 8.0 atau lebih baru
- **MariaDB**: 10.6 atau lebih baru

### Cache & Queue
- **Redis**: 6.0 atau lebih baru (untuk cache dan queue)
- **Memcached**: 1.6 atau lebih baru (opsional, alternatif Redis)

## Development Requirements

### Composer
- **Composer**: 2.0 atau lebih baru

### Node.js & NPM
- **Node.js**: 16.x atau lebih baru
- **NPM**: 8.x atau lebih baru (atau Yarn/Pnpm)

### Git
- **Git**: 2.20 atau lebih baru

## Hardware Requirements

### Minimum (Development)
- **CPU**: 2 cores
- **RAM**: 4 GB
- **Storage**: 20 GB free space
- **Network**: Internet connection untuk dependency installation

### Recommended (Production)
- **CPU**: 4+ cores
- **RAM**: 8 GB atau lebih
- **Storage**: 50 GB+ free space (SSD recommended)
- **Network**: Stable internet connection

### High Traffic (Production)
- **CPU**: 8+ cores
- **RAM**: 16 GB atau lebih
- **Storage**: 100 GB+ free space (SSD required)
- **Network**: High bandwidth connection

## Software Dependencies

### Required
- **Laravel Framework**: 9.19
- **PHP Composer**: Untuk dependency management
- **Redis Server**: Untuk queue dan cache
- **MySQL/MariaDB**: Database server

### Optional
- **Supervisor**: Untuk queue worker management
- **PM2**: Untuk process management (alternatif)
- **Docker**: Untuk containerization (opsional)

## Browser Requirements (untuk Admin Panel)

### Supported Browsers
- **Chrome**: Versi terbaru
- **Firefox**: Versi terbaru
- **Safari**: Versi terbaru
- **Edge**: Versi terbaru

### Minimum Resolution
- **Desktop**: 1280x720
- **Tablet**: 1024x768

## Environment Variables

Pastikan server Anda dapat mengatur environment variables berikut:
- `APP_ENV`
- `APP_DEBUG`
- `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`
- `REDIS_HOST`, `REDIS_PORT`, `REDIS_PASSWORD`
- `QUEUE_CONNECTION`

## Checklist Sebelum Instalasi

Sebelum memulai instalasi, pastikan:

- [ ] PHP 8.0.2+ terinstall dengan semua extension yang diperlukan
- [ ] MySQL/MariaDB 8.0+ terinstall dan berjalan
- [ ] Redis 6.0+ terinstall dan berjalan
- [ ] Composer 2.0+ terinstall
- [ ] Node.js 16+ dan NPM terinstall
- [ ] Web server (Apache/Nginx) dikonfigurasi dengan benar
- [ ] File permissions untuk storage dan cache folder sudah diset dengan benar

## Verifikasi Requirements

Untuk memverifikasi bahwa sistem Anda memenuhi requirements, jalankan perintah berikut:

```bash
# Cek versi PHP
php -v

# Cek extension PHP
php -m

# Cek versi Composer
composer --version

# Cek versi Node.js
node -v

# Cek versi NPM
npm -v

# Cek MySQL
mysql --version

# Cek Redis
redis-cli --version
```

---

**Selanjutnya**: [Technology Stack](./technology-stack)
