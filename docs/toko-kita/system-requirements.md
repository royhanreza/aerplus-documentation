---
sidebar_position: 2
---

# System Requirements

Dokumen ini menjelaskan persyaratan sistem untuk development dan deployment Toko Kita.

## Development Requirements

### Operating System

Next.js development dapat dilakukan di:
- **Windows**: Windows 10 atau lebih baru
- **macOS**: macOS 10.15 (Catalina) atau lebih baru
- **Linux**: Ubuntu 20.04+ atau distribusi Linux modern

### Node.js & Package Manager

- **Node.js**: 18.x atau lebih baru (recommended: 20.x LTS atau 22.x)
- **Package Managers** (pilih salah satu):
  - **npm**: 8.x+ (included dengan Node.js)
  - **yarn**: 1.22+ (optional)
  - **pnpm**: 8.x+ (recommended, faster & efficient)
  - **bun**: 1.x+ (optional, fastest)

Verify installation:
```bash
node --version
npm --version
# or
pnpm --version
```

Install Node.js:
- Download dari https://nodejs.org
- Or use nvm (Node Version Manager):
  ```bash
  # Install nvm
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
  
  # Install Node.js
  nvm install 20
  nvm use 20
  ```

### IDE / Code Editor

**Pilihan 1: Visual Studio Code** (Recommended)
- VS Code latest version
- Extensions:
  - ESLint
  - Prettier
  - Tailwind CSS IntelliSense
  - ES7+ React/Redux/React-Native snippets
  - TypeScript

**Pilihan 2: WebStorm**
- WebStorm 2023.1 atau lebih baru
- Built-in Next.js support

**Pilihan 3: Other Editors**
- Sublime Text dengan plugins
- Vim/Neovim dengan LSP

### Git

- **Git**: 2.20 atau lebih baru

## Browser Requirements

### Development

- **Chrome/Edge**: Latest version (recommended untuk DevTools)
- **Firefox**: Latest version
- **Safari**: Latest version (untuk testing di macOS)

### Testing

Test di berbagai browsers:
- Chrome (desktop & mobile)
- Safari (iOS)
- Firefox
- Edge

## Hardware Requirements

### Minimum (Development)

- **CPU**: Dual-core processor
- **RAM**: 8 GB
- **Storage**: 10 GB free space
- **Internet**: Required untuk package downloads

### Recommended (Development)

- **CPU**: Quad-core processor atau lebih (untuk fast builds)
- **RAM**: 16 GB atau lebih
- **Storage**: 20 GB+ free space (SSD highly recommended)
- **Internet**: Stable connection (untuk fast package installs)

## Network Requirements

### Development

- **Internet Connection**: Required untuk:
  - npm package downloads
  - API testing
  - External services (Mapbox, etc)
  - Documentation

### Runtime (End Users)

- **Internet**: Required (web application)
- **Bandwidth**: Minimal 1 Mbps (recommended 3+ Mbps)
- **Latency**: &lt;200ms untuk optimal experience

## Optional Tools

### API Testing

- **Postman**: API testing tool
- **Insomnia**: Alternative API client
- **Thunder Client**: VS Code extension

### Version Control

- **GitHub Desktop**: GUI untuk Git (optional)
- **GitKraken**: Advanced Git client (optional)

### DevTools

- **React DevTools**: Browser extension untuk React debugging
- **Redux DevTools**: Jika menggunakan Redux (not used in this project)

### Design Tools

- **Figma**: Untuk view design mockups
- **Adobe XD**: Alternative design tool

## Environment Setup Check

Verify your development environment:

```bash
# Check Node.js
node --version
# Expected: v20.x.x or v22.x.x

# Check npm
npm --version
# Expected: 10.x.x or higher

# Check pnpm (if using)
pnpm --version
# Expected: 8.x.x or higher

# Check Git
git --version
# Expected: 2.20.0 or higher
```

## Project Dependencies Check

After cloning repository:

```bash
# Install dependencies
npm install
# or
pnpm install

# Check for outdated packages
npm outdated
# or
pnpm outdated

# Check for vulnerabilities
npm audit
# or
pnpm audit
```

## Next.js Specific Requirements

### Turbopack (Development)

- Included dengan Next.js 15
- Faster development builds
- Automatic dengan `next dev --turbopack`

### Production Build Requirements

- Node.js 18+ untuk build
- Sufficient RAM untuk build process (minimum 2GB free)
- Fast CPU untuk faster builds

## Backend Requirements

Toko Kita memerlukan akses ke:

- **Aerplus Backend API**: REST API endpoints
- **Authentication Service**: Login dan token management
- **Mapbox API**: Untuk maps dan geocoding (requires API key)
- **Payment Gateway**: Payment processing (jika applicable)

## Minimum Browser Support (End Users)

### Desktop Browsers

- **Chrome**: 90+
- **Firefox**: 88+
- **Safari**: 14+
- **Edge**: 90+

### Mobile Browsers

- **Chrome Mobile**: 90+
- **Safari iOS**: 14+
- **Samsung Internet**: 14+

### Features Required

- ES2017+ support
- CSS Grid
- Flexbox
- localStorage
- Service Workers (untuk PWA)

## Screen Resolutions

### Mobile

- **Minimum**: 320px width
- **Optimal**: 375px - 428px width
- **Supported**: Portrait orientation (primary)

### Tablet

- **Optimal**: 768px - 1024px width
- **Supported**: Portrait dan landscape

### Desktop

- **Minimum**: 1024px width
- **Optimal**: 1280px - 1920px width
- **Maximum**: No limit (responsive)

## Checklist Sebelum Development

Pastikan Anda telah:

- [ ] Install Node.js 18+ (recommended 20.x LTS)
- [ ] Install package manager (npm/pnpm/yarn)
- [ ] Install Git
- [ ] Install VS Code atau IDE pilihan
- [ ] Install browser untuk testing (Chrome/Firefox)
- [ ] Setup Mapbox account dan API key
- [ ] Configure API endpoint access ke Aerplus backend
- [ ] Install React DevTools extension
- [ ] Configure Git user dan email

## Environment Variables Required

Before running the app:

- `NEXT_PUBLIC_API_URL`: Backend API base URL
- `NEXT_PUBLIC_MAPBOX_TOKEN`: Mapbox API token
- Other environment-specific variables

## Verifikasi Installation

```bash
# Clone repository
git clone <repository-url> toko-kita
cd toko-kita

# Install dependencies
pnpm install

# Create .env.local file
cp .env.example .env.local

# Edit .env.local dengan your configuration

# Run development server
pnpm dev

# Open browser
# Navigate to http://localhost:3000
```

Expected output:
```
   ▲ Next.js 15.0.2
   - Local:        http://localhost:3000
   - Turbopack (experimental) enabled

 ✓ Ready in 1.5s
```

## Performance Expectations

### Development Server

- **Cold Start**: 3-10 seconds
- **Hot Reload**: &lt;1 second (dengan Turbopack)
- **Full Rebuild**: 5-15 seconds

### Production Build

- **Build Time**: 30 seconds - 2 minutes (depends on hardware)
- **Output Size**: 500KB - 2MB (compressed)

---

**Selanjutnya**: [Technology Stack](./technology-stack)
