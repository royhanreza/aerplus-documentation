---
sidebar_position: 5
---

# Configuration

Panduan konfigurasi Toko Kita untuk development dan production.

## Environment Variables

### Environment Files

Next.js supports multiple environment files:

```
.env                 # Loaded in all environments
.env.local           # Loaded in all environments, ignored by git
.env.development     # Loaded in development only
.env.production      # Loaded in production only
```

**Priority** (highest to lowest):
1. `.env.local`
2. `.env.[environment]`
3. `.env`

### Required Variables

Create `.env.local`:

```env
# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:8000/api

# Mapbox
NEXT_PUBLIC_MAPBOX_TOKEN=pk.eyJ1...your_token

# App Configuration
NEXT_PUBLIC_APP_NAME=TokoKita
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

### Optional Variables

```env
# Analytics
NEXT_PUBLIC_GA_ID=G-XXXXXXXXXX

# Feature Flags
NEXT_PUBLIC_ENABLE_FEATURE_X=true

# Debug Mode
NEXT_PUBLIC_DEBUG=false

# API Timeout
NEXT_PUBLIC_API_TIMEOUT=30000
```

### Variable Prefixes

**`NEXT_PUBLIC_`** prefix:
- Exposed to browser
- Available di client components
- Embedded di bundle at build time

**No prefix**:
- Server-side only
- Not exposed to browser
- For API keys, secrets

### Access Environment Variables

```typescript
// Client-side (needs NEXT_PUBLIC_ prefix)
const apiUrl = process.env.NEXT_PUBLIC_API_URL;

// Server-side (any variable)
const secretKey = process.env.SECRET_API_KEY;
```

## Next.js Configuration

File: `next.config.ts`

### Basic Configuration

```typescript
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  // Images from external domains
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "**", // Allow all domains (not recommended for production)
      },
      // Better: Specific domains
      {
        protocol: "https",
        hostname: "api.aerplus.com",
      },
      {
        protocol: "https",
        hostname: "cdn.aerplus.com",
      },
    ],
  },

  // TypeScript
  typescript: {
    ignoreBuildErrors: false, // Change to false for production
  },

  // ESLint
  eslint: {
    ignoreDuringBuilds: false,
  },

  // Turbopack (development only)
  // Automatically enabled with --turbopack flag

  // Output (for deployment)
  output: 'standalone', // For Docker deployment
  // output: 'export', // For static export
};

export default nextConfig;
```

### Advanced Configuration

```typescript
const nextConfig: NextConfig = {
  // Redirects
  async redirects() {
    return [
      {
        source: '/old-page',
        destination: '/new-page',
        permanent: true,
      },
    ];
  },

  // Rewrites (proxy)
  async rewrites() {
    return [
      {
        source: '/api/:path*',
        destination: 'https://api.aerplus.com/:path*',
      },
    ];
  },

  // Headers
  async headers() {
    return [
      {
        source: '/:path*',
        headers: [
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
        ],
      },
    ];
  },

  // Experimental features
  experimental: {
    // Enable Server Actions
    serverActions: {
      bodySizeLimit: '2mb',
    },
  },

  // Webpack configuration (advanced)
  webpack: (config, { isServer }) => {
    // Custom webpack config
    return config;
  },
};
```

## Tailwind CSS Configuration

File: `tailwind.config.ts`

```typescript
import type { Config } from "tailwindcss";
import daisyui from "daisyui";

const config: Config = {
  darkMode: ["class"],
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        // Custom colors
        primary: '#F59E0B', // Amber
        secondary: '#10B981', // Green
        accent: '#3B82F6', // Blue
      },
      fontFamily: {
        sans: ['var(--font-geist-sans)', 'system-ui', 'sans-serif'],
        mono: ['var(--font-geist-mono)', 'monospace'],
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem',
      },
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
    },
  },
  plugins: [
    daisyui,
    require("tailwindcss-animate"),
  ],
  daisyui: {
    themes: [
      "light",
      "dark",
      "cupcake",
      "valentine",
      "lofi",
      "bumblebee", // Default theme
    ],
    // Default theme
    base: true,
    styled: true,
    utils: true,
  },
};

export default config;
```

### Custom Theme

```typescript
daisyui: {
  themes: [
    {
      tokokita: {
        "primary": "#F59E0B",
        "secondary": "#10B981",
        "accent": "#3B82F6",
        "neutral": "#3D4451",
        "base-100": "#FFFFFF",
        "info": "#3ABFF8",
        "success": "#36D399",
        "warning": "#FBBD23",
        "error": "#F87272",
      },
    },
  ],
}
```

## TypeScript Configuration

File: `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES2017",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "incremental": true,
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/*": ["./*"]
    }
  },
  "include": ["next-env.d.ts", "**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### Strict Mode Options

```json
{
  "compilerOptions": {
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

## API Configuration

### Axios Instance

File: `src/util/services.ts`

```typescript
import axios from 'axios';
import { getCookie } from 'cookies-next';

const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor (add auth token)
api.interceptors.request.use(
  (config) => {
    const token = getCookie('auth_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);

// Response interceptor (handle errors)
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Redirect to login
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

### Environment-Based Configuration

```typescript
const getApiUrl = () => {
  if (process.env.NODE_ENV === 'production') {
    return 'https://api.aerplus.com/api';
  }
  if (process.env.NODE_ENV === 'staging') {
    return 'https://staging-api.aerplus.com/api';
  }
  return 'http://localhost:8000/api';
};

const api = axios.create({
  baseURL: getApiUrl(),
});
```

## Middleware Configuration

File: `src/middleware.ts`

```typescript
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  const token = request.cookies.get('auth_token');
  const { pathname } = request.nextUrl;

  // Public routes
  const publicPaths = ['/', '/login'];
  const isPublicPath = publicPaths.includes(pathname);

  // Redirect unauthenticated users to login
  if (!token && !isPublicPath) {
    return NextResponse.redirect(new URL('/login', request.url));
  }

  // Redirect authenticated users away from login
  if (token && pathname === '/login') {
    return NextResponse.redirect(new URL('/order', request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: [
    /*
     * Match all request paths except:
     * - api (API routes)
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!api|_next/static|_next/image|favicon.ico).*)',
  ],
};
```

## Zustand Store Configuration

### Store with Persistence

```typescript
import { create } from 'zustand';
import { persist } from 'zustand/middleware';

interface CartStore {
  items: any[];
  addItem: (item: any) => void;
  removeItem: (id: string) => void;
  clear: () => void;
}

export const useCartStore = create<CartStore>()(
  persist(
    (set) => ({
      items: [],
      addItem: (item) =>
        set((state) => ({ items: [...state.items, item] })),
      removeItem: (id) =>
        set((state) => ({
          items: state.items.filter((item) => item.id !== id),
        })),
      clear: () => set({ items: [] }),
    }),
    {
      name: 'cart-storage', // localStorage key
    }
  )
);
```

## React Query Configuration

### Query Client Setup

File: `src/util/provider.tsx`

```typescript
'use client';

import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import { useState } from 'react';

export default function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            staleTime: 60 * 1000, // 1 minute
            gcTime: 5 * 60 * 1000, // 5 minutes (formerly cacheTime)
            retry: 1,
            refetchOnWindowFocus: false,
          },
        },
      })
  );

  return (
    <QueryClientProvider client={queryClient}>
      {children}
      {process.env.NODE_ENV === 'development' && (
        <ReactQueryDevtools initialIsOpen={false} />
      )}
    </QueryClientProvider>
  );
}
```

Usage in layout:

```typescript
// src/app/layout.tsx
import Providers from '@/util/provider';

export default function RootLayout({ children }) {
  return (
    <html lang="id">
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```

## Mapbox Configuration

```typescript
'use client';

import mapboxgl from 'mapbox-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

// Set access token
mapboxgl.accessToken = process.env.NEXT_PUBLIC_MAPBOX_TOKEN!;

// Create map instance
const map = new mapboxgl.Map({
  container: 'map', // container ID
  style: 'mapbox://styles/mapbox/streets-v12',
  center: [106.845599, -6.208763], // Jakarta coordinates
  zoom: 12,
});

// Add marker
new mapboxgl.Marker()
  .setLngLat([106.845599, -6.208763])
  .addTo(map);
```

## PWA Configuration (Optional)

Install `next-pwa`:

```bash
pnpm add next-pwa
```

Update `next.config.ts`:

```typescript
const withPWA = require('next-pwa')({
  dest: 'public',
  disable: process.env.NODE_ENV === 'development',
});

const nextConfig = {
  // ... other config
};

module.exports = withPWA(nextConfig);
```

Create `public/manifest.json`:

```json
{
  "name": "TokoKita",
  "short_name": "TokoKita",
  "description": "Platform belanja untuk kebutuhan harian",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#F59E0B",
  "icons": [
    {
      "src": "/icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

## ESLint Configuration

File: `.eslintrc.json`

```json
{
  "extends": "next/core-web-vitals",
  "rules": {
    "@next/next/no-img-element": "warn",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

## Checklist Konfigurasi

Sebelum production:

- [ ] Set production API URL di environment variables
- [ ] Configure image domains untuk next/image
- [ ] Set `ignoreBuildErrors: false` di next.config.ts
- [ ] Remove debug logs
- [ ] Configure proper error handling
- [ ] Set up analytics (Google Analytics, etc)
- [ ] Configure security headers
- [ ] Test di different browsers
- [ ] Test responsive design
- [ ] Optimize images
- [ ] Configure caching strategies

---

**Selanjutnya**: [Build & Deployment](./build-deployment)
