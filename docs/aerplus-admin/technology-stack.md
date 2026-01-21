---
sidebar_position: 3
---

# Technology Stack

Dokumen ini menjelaskan teknologi dan framework yang digunakan dalam pengembangan Aerplus Admin (Back-End).

## Backend Framework

### Laravel
- **Version**: 9.19
- **Description**: PHP framework modern yang digunakan sebagai core framework
- **Website**: https://laravel.com
- **Documentation**: https://laravel.com/docs/9.x

Laravel dipilih karena:
- Sintaks yang elegan dan mudah dipahami
- Ekosistem yang kaya dengan package dan tools
- Built-in features untuk authentication, authorization, dan queue management
- Active community dan dokumentasi yang lengkap

## Database

### MySQL / MariaDB
- **Version**: 8.0+ / 10.6+
- **Description**: Relational database management system
- **Usage**: Menyimpan semua data aplikasi (users, transactions, products, dll)

### Redis
- **Version**: 6.0+
- **Description**: In-memory data structure store
- **Usage**: 
  - Cache untuk meningkatkan performa
  - Queue management untuk background jobs
  - Session storage

## Frontend (Admin Panel)

### Vue.js
- **Version**: 2.x atau 3.x
- **Description**: Progressive JavaScript framework
- **Usage**: Membangun admin panel interface

### Vite
- **Version**: 2.9+
- **Description**: Next generation frontend build tool
- **Usage**: Compile dan optimize JavaScript dan CSS dengan HMR (Hot Module Replacement)
- **Website**: https://vitejs.dev

## Package Management

### Composer
- **Description**: Dependency manager untuk PHP
- **Usage**: Mengelola PHP packages dan dependencies

### NPM / Yarn / Pnpm
- **Description**: Package manager untuk JavaScript
- **Usage**: Mengelola JavaScript packages dan dependencies

## Key Laravel Packages

### Authentication & Authorization
- **Laravel Sanctum**: API token authentication
- **Laravel Policies**: Authorization policies

### Database & ORM
- **Eloquent ORM**: Built-in ORM dari Laravel
- **Laravel Migrations**: Database version control

### Queue & Jobs
- **Laravel Queue**: Background job processing
- **Laravel Horizon**: Dashboard untuk monitoring queue (required)
- **Version**: 5.33

### File Management
- **Laravel Storage**: File system abstraction
- **Intervention Image**: Image manipulation library
- **Version**: 2.7
- **AWS S3**: File storage untuk production (via league/flysystem-aws-s3-v3)

### Excel Export/Import
- **Maatwebsite Excel**: Export dan import Excel files
- **Version**: 3.1
- **Spatie Simple Excel**: Alternative Excel library
- **Version**: 3.3

### PDF Generation
- **Barryvdh DomPDF**: PDF generation library
- **Version**: 3.0
- **MPDF**: Alternative PDF library
- **Version**: 8.2
- **Carlos Meneses Laravel MPDF**: Laravel wrapper untuk MPDF
- **Version**: 2.1

### DataTables
- **Yajra Laravel DataTables**: Server-side DataTables untuk Laravel
- **Version**: 10.4
- **Usage**: Advanced table features dengan server-side processing

### API & Validation
- **Laravel Validation**: Request validation
- **Laravel Resources**: API resource transformation
- **Laravel Sanctum**: API token authentication
- **Version**: 2.14.1

### Utilities
- **Guzzle HTTP**: HTTP client untuk external API calls
- **Version**: 7.2
- **Riskihajar Terbilang**: Convert number to Indonesian words
- **Version**: 1.2
- **Yield Studio Laravel Expo Notifier**: Push notification untuk mobile apps
- **Version**: 0.0.15

## Development Tools

### Version Control
- **Git**: Source code version control

### Code Quality
- **PHP CS Fixer**: Code style fixer (opsional)
- **PHPStan**: Static analysis tool (opsional)

### Testing
- **PHPUnit**: Unit dan feature testing framework
- **Laravel Testing**: Built-in testing utilities

## Server & Deployment

### Web Server
- **Apache** atau **Nginx**: Web server

### Process Management
- **Supervisor**: Process manager untuk queue workers
- **PM2**: Alternative process manager

### Containerization (Opsional)
- **Docker**: Container platform
- **Docker Compose**: Multi-container Docker applications

## Architecture Pattern

### MVC (Model-View-Controller)
Laravel menggunakan pattern MVC untuk memisahkan:
- **Model**: Business logic dan database interaction
- **View**: Presentation layer (Blade templates)
- **Controller**: Request handling dan coordination

### Repository Pattern
Digunakan untuk abstraksi data access layer, memudahkan testing dan maintenance.

### Service Layer
Business logic yang kompleks dipisahkan ke service classes untuk maintainability.

## Security

### Laravel Security Features
- **CSRF Protection**: Built-in CSRF token protection
- **XSS Protection**: Automatic XSS filtering
- **SQL Injection Protection**: Eloquent ORM dengan parameter binding
- **Password Hashing**: Bcrypt/Argon2 hashing
- **Encryption**: Built-in encryption helpers

## Performance Optimization

### Caching
- **Redis Cache**: Untuk caching queries dan data
- **Laravel Cache**: Cache abstraction layer

### Queue Processing
- **Background Jobs**: Memproses heavy operations secara asynchronous
- **Queue Workers**: Multiple workers untuk parallel processing

### Database Optimization
- **Query Optimization**: Efficient Eloquent queries
- **Database Indexing**: Proper indexing untuk performa
- **Eager Loading**: Mencegah N+1 query problem

## Monitoring & Logging

### Logging
- **Laravel Logging**: Built-in logging system
- **Log Channels**: Multiple log channels (file, database, etc)

### Error Tracking (Opsional)
- **Sentry**: Error tracking dan monitoring
- **Laravel Telescope**: Debug assistant untuk Laravel (development)

## API Integration

### HTTP Client
- **Guzzle HTTP**: HTTP client untuk external API calls
- **Laravel HTTP Client**: Built-in HTTP client wrapper

### API Documentation
- **Swagger/OpenAPI**: API documentation (jika diperlukan)

## Development Environment

### Local Development
- **Laravel Valet**: Development environment untuk macOS
- **Laravel Homestead**: Vagrant box untuk development
- **Laravel Sail**: Docker development environment

---

**Selanjutnya**: [Installation](./installation)
