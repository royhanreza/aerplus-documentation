---
sidebar_position: 4
---

# Development Setup

Panduan lengkap untuk setup development environment Toko Kita dengan Next.js 15.

## Prerequisites

Pastikan Anda telah memenuhi semua [System Requirements](./system-requirements) sebelum melanjutkan.

## Step 1: Install Node.js

### Option 1: Direct Download

Download dari https://nodejs.org (recommended LTS version 20.x)

### Option 2: Using NVM (Recommended)

```bash
# Install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Reload shell
source ~/.bashrc  # or ~/.zshrc

# Install Node.js 20
nvm install 20
nvm use 20
nvm alias default 20

# Verify
node --version  # Should show v20.x.x
npm --version   # Should show 10.x.x
```

## Step 2: Install Package Manager (Optional)

### Using pnpm (Recommended)

```bash
# Install pnpm globally
npm install -g pnpm

# Verify
pnpm --version
```

**Why pnpm?**
- Faster than npm/yarn
- Efficient disk space usage
- Strict dependency management
- Better monorepo support

### Alternative: yarn

```bash
npm install -g yarn
yarn --version
```

## Step 3: Clone Repository

```bash
git clone <repository-url> toko-kita
cd toko-kita
```

## Step 4: Install Dependencies

```bash
# Using pnpm (recommended)
pnpm install

# Using npm
npm install

# Using yarn
yarn install
```

This will install all packages dari `package.json`:
- Next.js 15
- React 19 RC
- TypeScript
- Tailwind CSS + DaisyUI
- Zustand, React Query
- Axios
- Mapbox
- Dan dependencies lainnya

## Step 5: Environment Variables

### Create Environment File

```bash
cp .env.example .env.local
```

### Configure Environment Variables

Edit `.env.local`:

```env
# API Configuration
NEXT_PUBLIC_API_URL=http://localhost:8000/api

# Mapbox
NEXT_PUBLIC_MAPBOX_TOKEN=your_mapbox_token_here

# App Configuration
NEXT_PUBLIC_APP_NAME=TokoKita
NEXT_PUBLIC_APP_URL=http://localhost:3000

# Optional: Analytics
NEXT_PUBLIC_GA_ID=your_google_analytics_id
```

**Important**:
- Variables prefixed dengan `NEXT_PUBLIC_` are exposed ke browser
- Other variables hanya available di server-side
- Never commit `.env.local` to git

### Get Mapbox Token

1. Sign up di https://account.mapbox.com/auth/signup/
2. Create access token
3. Copy token ke `.env.local`

## Step 6: Run Development Server

### Using Turbopack (Recommended)

```bash
# With pnpm
pnpm dev

# With npm
npm run dev

# With yarn
yarn dev
```

This runs `next dev --turbopack` (configured di `package.json`)

### Without Turbopack

```bash
next dev
```

### Expected Output

```
   ▲ Next.js 15.0.2
   - Local:        http://localhost:3000
   - Turbopack (experimental) enabled

 ✓ Ready in 1.5s
```

## Step 7: Open Application

Navigate to http://localhost:3000 in your browser.

Expected:
- Home page dengan "TokoKita" branding
- "Masuk" button
- Responsive design

## Step 8: Install VS Code Extensions (Optional)

Recommended extensions:

```json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "dsznajder.es7-react-js-snippets",
    "formulahendry.auto-rename-tag",
    "christian-kohler.path-intellisense"
  ]
}
```

Create `.vscode/extensions.json` dengan content di atas.

### VS Code Settings

Create `.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ]
}
```

## Development Workflow

### Project Structure Overview

```
toko-kita/
├── src/
│   ├── app/              # Pages & routes
│   ├── store/            # Zustand stores
│   ├── util/             # Utilities & services
│   └── interface.ts      # TypeScript types
├── public/               # Static assets
├── lib/                  # Utility functions
├── .env.local            # Environment variables
├── next.config.ts        # Next.js configuration
├── tailwind.config.ts    # Tailwind configuration
└── package.json          # Dependencies
```

### Hot Reload

Next.js supports Fast Refresh:
- Edit files dan save
- Changes reflect immediately (usually &lt;1 second)
- State is preserved
- Error overlay shows compilation errors

### TypeScript

TypeScript is configured dan enabled by default:
- Type checking during development
- IntelliSense di VS Code
- Build-time error catching

Check types manually:

```bash
# Type check
npx tsc --noEmit

# Watch mode
npx tsc --noEmit --watch
```

## Common Development Tasks

### Add New Page

Create file di `src/app/`:

```tsx
// src/app/about/page.tsx
export default function AboutPage() {
  return (
    <div>
      <h1>About Page</h1>
    </div>
  );
}
```

Automatically available di `/about`

### Create API Route

```tsx
// src/app/api/hello/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  return NextResponse.json({ message: 'Hello World' });
}
```

Available di `/api/hello`

### Add New Component

```tsx
// src/components/Button.tsx
interface ButtonProps {
  label: string;
  onClick?: () => void;
}

export default function Button({ label, onClick }: ButtonProps) {
  return (
    <button onClick={onClick} className="btn btn-primary">
      {label}
    </button>
  );
}
```

### Create Zustand Store

```typescript
// src/store/example.ts
import { create } from 'zustand';

interface ExampleStore {
  count: number;
  increment: () => void;
  decrement: () => void;
}

export const useExampleStore = create<ExampleStore>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));
```

Usage:

```tsx
'use client';
import { useExampleStore } from '@/store/example';

export default function Counter() {
  const { count, increment, decrement } = useExampleStore();
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

### Add API Service

```typescript
// src/util/services.ts
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
});

export const getProducts = async () => {
  const response = await api.get('/products');
  return response.data;
};

export const createOrder = async (data: any) => {
  const response = await api.post('/orders', data);
  return response.data;
};
```

### Use React Query

```tsx
'use client';
import { useQuery } from '@tanstack/react-query';
import { getProducts } from '@/util/services';

export default function ProductList() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['products'],
    queryFn: getProducts,
  });
  
  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error loading products</div>;
  
  return (
    <div>
      {data?.map((product: any) => (
        <div key={product.id}>{product.name}</div>
      ))}
    </div>
  );
}
```

## Debugging

### React DevTools

Install browser extension:
- Chrome: https://chrome.google.com/webstore
- Firefox: https://addons.mozilla.org

Features:
- Component tree
- Props inspection
- State inspection
- Performance profiling

### Console Logging

```typescript
// Development only
if (process.env.NODE_ENV === 'development') {
  console.log('Debug info:', data);
}
```

### VS Code Debugger

Create `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    }
  ]
}
```

### Error Overlay

Next.js shows errors in development:
- Compilation errors
- Runtime errors
- Click to open file in editor

## Linting & Formatting

### Run ESLint

```bash
# Check for issues
pnpm lint

# Fix auto-fixable issues
pnpm lint --fix
```

### Format Code (if Prettier configured)

```bash
# Format all files
npx prettier --write .

# Check formatting
npx prettier --check .
```

## Testing

### Manual Testing Checklist

- [ ] All pages load without errors
- [ ] Navigation works
- [ ] Forms submit correctly
- [ ] API calls work
- [ ] Responsive design on mobile
- [ ] Browser console has no errors

### Test Different Devices

Chrome DevTools:
1. Open DevTools (F12)
2. Toggle device toolbar (Ctrl+Shift+M)
3. Select device (iPhone, iPad, etc)
4. Test UI dan functionality

## Troubleshooting

### Port Already in Use

```bash
# Kill process on port 3000
# macOS/Linux
lsof -ti:3000 | xargs kill -9

# Windows
netstat -ano | findstr :3000
taskkill /PID <PID> /F

# Or use different port
pnpm dev -- --port 3001
```

### Module Not Found

```bash
# Clean install
rm -rf node_modules
rm pnpm-lock.yaml  # or package-lock.json
pnpm install
```

### TypeScript Errors

```bash
# Restart TypeScript server di VS Code
# Command Palette (Ctrl+Shift+P)
# TypeScript: Restart TS Server
```

### Cache Issues

```bash
# Clear Next.js cache
rm -rf .next
pnpm dev
```

### Build Errors

```bash
# Clean build
rm -rf .next
rm -rf out
pnpm build
```

## Performance Tips

### Development

- Use Turbopack untuk faster builds
- Keep dev server running (hot reload faster)
- Close unnecessary applications
- Use SSD untuk faster file operations

### Code Organization

- Keep components small
- Use React.memo untuk expensive components
- Lazy load heavy components
- Optimize images dengan next/image

## Git Workflow

### Initial Setup

```bash
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### Branch Strategy

```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes
git add .
git commit -m "Add new feature"

# Push to remote
git push origin feature/new-feature

# Create Pull Request di GitHub/GitLab
```

### Commit Messages

Follow conventional commits:
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `style:` Formatting
- `refactor:` Code restructuring
- `test:` Tests
- `chore:` Maintenance

Example:
```bash
git commit -m "feat: add product search functionality"
git commit -m "fix: resolve cart calculation bug"
```

## Next Steps

Setelah development setup selesai:

1. **[Configuration](./configuration)** - Configure app settings
2. Explore codebase
3. Implement features
4. Test thoroughly
5. Prepare untuk build production

---

**Selanjutnya**: [Configuration](./configuration)
