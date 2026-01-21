---
sidebar_position: 6
---

# Build & Deployment

Panduan lengkap untuk build dan deploy Toko Kita ke production.

## Build Process Overview

Next.js supports multiple deployment strategies:
1. **Vercel** (recommended, zero-config)
2. **Node.js Server** (self-hosted)
3. **Docker** (containerized)
4. **Static Export** (for static hosting)

## Production Build

### Step 1: Prepare for Production

Update environment variables untuk production.

Create `.env.production`:

```env
NEXT_PUBLIC_API_URL=https://api.aerplus.com/api
NEXT_PUBLIC_MAPBOX_TOKEN=pk.your_production_token
NEXT_PUBLIC_APP_URL=https://tokokita.aerplus.com
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX
```

### Step 2: Build Application

```bash
# Using pnpm
pnpm build

# Using npm
npm run build

# Using yarn
yarn build
```

Build output:
```
Route (app)                              Size     First Load JS
┌ ○ /                                   142 B          87.2 kB
├ ○ /login                              1.45 kB        88.5 kB
├ ○ /order                              2.89 kB        90.9 kB
├ ○ /my-order                           1.23 kB        88.2 kB
└ ○ /my-profile                         856 B          87.9 kB

○  (Static)  prerendered as static content
```

Build generates:
- `.next/` folder dengan compiled code
- Static assets di `.next/static/`
- Server code di `.next/server/`

### Step 3: Test Production Build Locally

```bash
# Start production server
pnpm start

# Access at http://localhost:3000
```

### Step 4: Analyze Bundle Size

```bash
# Install analyzer
pnpm add -D @next/bundle-analyzer

# Update next.config.ts
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
});

module.exports = withBundleAnalyzer(nextConfig);

# Run analysis
ANALYZE=true pnpm build
```

Opens bundle visualization di browser.

## Deployment Options

## Option 1: Vercel (Recommended)

### Why Vercel?

- Zero-configuration deployment
- Automatic CI/CD
- Edge network (CDN)
- Preview deployments
- Free SSL
- Environment variables UI
- Analytics
- Made by Next.js creators

### Deploy to Vercel

#### Method 1: GitHub Integration

1. **Push to GitHub**:
   ```bash
   git add .
   git commit -m "Ready for deployment"
   git push origin main
   ```

2. **Import to Vercel**:
   - Go to https://vercel.com
   - Click "New Project"
   - Import GitHub repository
   - Select "toko-kita" repository

3. **Configure**:
   - Framework Preset: Next.js (auto-detected)
   - Root Directory: ./
   - Build Command: `pnpm build` (or `npm run build`)
   - Output Directory: `.next` (default)

4. **Environment Variables**:
   - Add all `NEXT_PUBLIC_*` variables
   - Add secret variables (without NEXT_PUBLIC prefix)

5. **Deploy**:
   - Click "Deploy"
   - Wait ~2-3 minutes
   - Get deployment URL

#### Method 2: Vercel CLI

```bash
# Install Vercel CLI
pnpm add -g vercel

# Login
vercel login

# Deploy
vercel

# Production deployment
vercel --prod
```

### Custom Domain

1. Go to Project Settings → Domains
2. Add custom domain: `tokokita.aerplus.com`
3. Configure DNS:
   ```
   Type: CNAME
   Name: tokokita
   Value: cname.vercel-dns.com
   ```
4. Wait for DNS propagation (~5-10 minutes)

### Continuous Deployment

Vercel automatically deploys:
- **Production**: Pushes to `main` branch
- **Preview**: Pull requests dan other branches

## Option 2: Node.js Server (Self-Hosted)

### Requirements

- Node.js 18+ installed
- PM2 atau process manager
- Nginx (recommended) atau Apache
- SSL certificate (Let's Encrypt)

### Step 1: Build Application

```bash
pnpm build
```

### Step 2: Upload to Server

```bash
# Using rsync
rsync -avz --exclude 'node_modules' \
  ./ user@server:/var/www/toko-kita/

# Or using scp
scp -r .next package.json user@server:/var/www/toko-kita/
```

### Step 3: Install Dependencies on Server

```bash
ssh user@server
cd /var/www/toko-kita
npm install --production
```

### Step 4: Start Application

**Using PM2** (recommended):

```bash
# Install PM2
npm install -g pm2

# Start application
pm2 start npm --name "toko-kita" -- start

# Save PM2 config
pm2 save

# Setup startup script
pm2 startup

# Monitor
pm2 status
pm2 logs toko-kita
```

**Using systemd**:

Create `/etc/systemd/system/toko-kita.service`:

```ini
[Unit]
Description=TokoKita Next.js App
After=network.target

[Service]
Type=simple
User=www-data
WorkingDirectory=/var/www/toko-kita
ExecStart=/usr/bin/npm start
Restart=on-failure
Environment=NODE_ENV=production
Environment=PORT=3000

[Install]
WantedBy=multi-user.target
```

Enable dan start:

```bash
sudo systemctl enable toko-kita
sudo systemctl start toko-kita
sudo systemctl status toko-kita
```

### Step 5: Configure Nginx

Create `/etc/nginx/sites-available/toko-kita`:

```nginx
upstream tokokita {
    server 127.0.0.1:3000;
}

server {
    listen 80;
    server_name tokokita.aerplus.com;

    # Redirect to HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name tokokita.aerplus.com;

    ssl_certificate /etc/letsencrypt/live/tokokita.aerplus.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/tokokita.aerplus.com/privkey.pem;

    location / {
        proxy_pass http://tokokita;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_cache_bypass $http_upgrade;
    }

    # Static files caching
    location /_next/static {
        proxy_pass http://tokokita;
        add_header Cache-Control "public, max-age=31536000, immutable";
    }
}
```

Enable site:

```bash
sudo ln -s /etc/nginx/sites-available/toko-kita /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

### Step 6: SSL Certificate

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get certificate
sudo certbot --nginx -d tokokita.aerplus.com

# Auto-renewal (cron)
sudo certbot renew --dry-run
```

## Option 3: Docker Deployment

### Create Dockerfile

```dockerfile
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml* ./
RUN npm install -g pnpm && pnpm install --frozen-lockfile

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV production

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static

USER nextjs

EXPOSE 3000

ENV PORT 3000

CMD ["node", "server.js"]
```

### Update next.config.ts

```typescript
const nextConfig = {
  output: 'standalone',
  // ... other config
};
```

### Build Docker Image

```bash
docker build -t toko-kita .
```

### Run Container

```bash
docker run -p 3000:3000 \
  -e NEXT_PUBLIC_API_URL=https://api.aerplus.com/api \
  -e NEXT_PUBLIC_MAPBOX_TOKEN=pk.your_token \
  toko-kita
```

### Docker Compose

Create `docker-compose.yml`:

```yaml
version: '3.8'

services:
  toko-kita:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - NEXT_PUBLIC_API_URL=https://api.aerplus.com/api
      - NEXT_PUBLIC_MAPBOX_TOKEN=${MAPBOX_TOKEN}
    restart: unless-stopped
```

Run:

```bash
docker-compose up -d
```

## Option 4: Static Export

For purely static sites (no Server Components atau Server Actions):

### Update Configuration

```typescript
// next.config.ts
const nextConfig = {
  output: 'export',
  images: {
    unoptimized: true, // Required for static export
  },
};
```

### Build

```bash
pnpm build
```

Output: `out/` directory dengan static HTML/CSS/JS

### Deploy to Static Hosting

**Netlify**:
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy
netlify deploy --dir=out --prod
```

**GitHub Pages**:
```bash
# Add to package.json
"scripts": {
  "export": "next build && next export"
}

# Deploy
npm run export
# Upload out/ directory to gh-pages branch
```

**AWS S3 + CloudFront**, **Firebase Hosting**, etc.

## Environment Variables in Production

### Vercel

1. Project Settings → Environment Variables
2. Add variables:
   - Production
   - Preview
   - Development
3. Redeploy untuk apply changes

### Self-Hosted

Create `.env.production`:

```env
NEXT_PUBLIC_API_URL=https://api.aerplus.com/api
NEXT_PUBLIC_MAPBOX_TOKEN=pk.production_token
# Do not commit this file
```

Or use environment variables di server:

```bash
# PM2
pm2 start npm --name "toko-kita" -- start \
  --env NEXT_PUBLIC_API_URL=https://api.aerplus.com/api
```

## Performance Optimization

### Image Optimization

```typescript
// Use next/image
import Image from 'next/image';

<Image
  src="/product.jpg"
  alt="Product"
  width={500}
  height={500}
  priority // For above-the-fold images
/>
```

### Code Splitting

```typescript
// Dynamic imports
import dynamic from 'next/dynamic';

const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <p>Loading...</p>,
});
```

### Caching Headers

```typescript
// next.config.ts
async headers() {
  return [
    {
      source: '/_next/static/:path*',
      headers: [
        {
          key: 'Cache-Control',
          value: 'public, max-age=31536000, immutable',
        },
      ],
    },
  ];
}
```

## Monitoring & Analytics

### Vercel Analytics

```bash
pnpm add @vercel/analytics
```

```typescript
// app/layout.tsx
import { Analytics } from '@vercel/analytics/react';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        {children}
        <Analytics />
      </body>
    </html>
  );
}
```

### Google Analytics

```typescript
// app/layout.tsx
export default function RootLayout({ children }) {
  return (
    <html>
      <head>
        <script
          async
          src={`https://www.googletagmanager.com/gtag/js?id=${process.env.NEXT_PUBLIC_GA_ID}`}
        />
        <script
          dangerouslySetInnerHTML={{
            __html: `
              window.dataLayer = window.dataLayer || [];
              function gtag(){dataLayer.push(arguments);}
              gtag('js', new Date());
              gtag('config', '${process.env.NEXT_PUBLIC_GA_ID}');
            `,
          }}
        />
      </head>
      <body>{children}</body>
    </html>
  );
}
```

## Rollback Strategy

### Vercel

- Go to Deployments
- Select previous deployment
- Click "Promote to Production"

### Self-Hosted with Git

```bash
# Revert to previous commit
git revert HEAD
git push origin main

# Rebuild and deploy
```

### Docker

```bash
# Keep previous images tagged
docker tag toko-kita:latest toko-kita:v1.0.0

# Rollback
docker stop toko-kita
docker run -d --name toko-kita toko-kita:v1.0.0
```

## Deployment Checklist

Before deployment:

### Code
- [ ] All features tested
- [ ] No console.log di production
- [ ] Error handling implemented
- [ ] Loading states added
- [ ] TypeScript errors resolved
- [ ] ESLint warnings fixed

### Configuration
- [ ] Production environment variables set
- [ ] API URLs configured
- [ ] Mapbox token updated
- [ ] Image domains configured
- [ ] Security headers added

### Performance
- [ ] Images optimized
- [ ] Bundle size checked
- [ ] Lighthouse score &gt;90
- [ ] Core Web Vitals optimized

### Security
- [ ] HTTPS enabled
- [ ] CORS configured
- [ ] CSP headers set
- [ ] Sensitive data not exposed

### Testing
- [ ] Tested di multiple browsers
- [ ] Tested di mobile devices
- [ ] Tested slow network
- [ ] Error scenarios tested

## Post-Deployment

### Monitor

- Check application logs
- Monitor error rates
- Track performance metrics
- Review user feedback

### Update DNS

- Point domain to deployment
- Verify SSL certificate
- Test from different locations

### Documentation

- Update README dengan deployment info
- Document environment variables
- Create runbook untuk common issues

---

**Congratulations!** Toko Kita sekarang sudah live di production.
