---
sidebar_position: 3
---

# Technology Stack

Dokumen ini menjelaskan teknologi, framework, dan library yang digunakan dalam pengembangan Toko Kita.

## Core Framework

### Next.js

- **Version**: 15.0.2
- **Description**: React framework dengan App Router dan Server Components
- **Website**: https://nextjs.org
- **Key Features**:
  - App Router (app directory)
  - Server Components
  - Server Actions
  - Turbopack (development)
  - Built-in optimization
  - API routes
  - Middleware support
- **Why Next.js 15**:
  - Latest features (Server Actions, Partial Prerendering)
  - Better performance dengan Turbopack
  - Improved caching strategies
  - Better SEO dengan SSR
  - File-based routing

### React

- **Version**: 19.0.0-rc (Release Candidate)
- **Description**: JavaScript library untuk building user interfaces
- **Features Used**:
  - React Server Components
  - Client Components ("use client")
  - Hooks (useState, useEffect, custom hooks)
  - Suspense
  - Error boundaries

### TypeScript

- **Version**: 5.x
- **Description**: Typed superset dari JavaScript
- **Target**: ES2017
- **Benefits**:
  - Type safety
  - Better IDE support
  - Catch errors early
  - Self-documenting code
  - Refactoring confidence

## Styling & UI

### Tailwind CSS

- **Version**: 3.4.1
- **Description**: Utility-first CSS framework
- **Website**: https://tailwindcss.com
- **Features**:
  - Utility classes
  - Responsive design
  - Dark mode support
  - Custom theme
  - JIT compilation

### DaisyUI

- **Version**: 4.12.14
- **Description**: Component library untuk Tailwind CSS
- **Website**: https://daisyui.com
- **Components Used**:
  - Button, Card, Badge
  - Modal, Drawer
  - Form elements (input, select, textarea)
  - Toast notifications
  - Hero, Navbar
- **Themes**: bumblebee, light, dark, cupcake, valentine, lofi

### Tailwind CSS Animate

- **Version**: 1.0.7
- **Description**: Animation utilities untuk Tailwind
- **Features**: Pre-built animations (fade, slide, spin, etc)

### Styling Utilities

- **clsx**: 2.1.1 - Conditional className utility
- **class-variance-authority**: 0.7.0 - Component variants
- **tailwind-merge**: 2.5.4 - Merge Tailwind classes

## State Management

### Zustand

- **Version**: 5.0.1
- **Description**: Lightweight state management
- **Website**: https://zustand-demo.pmnd.rs
- **Packages**:
  - `zustand`: Core library
  - `zustand-middleware-computed-state`: 0.1.3 - Computed state
- **Why Zustand**:
  - Minimal boilerplate
  - No providers needed
  - Great TypeScript support
  - Small bundle size (~1KB)
  - Easy to learn

### Stores

Located in `src/store/`:
- `customer.ts`: Customer/user state
- `order.ts`: Order management state
- `outlet.ts`: Outlet/store state

### Immer

- **Version**: 10.1.1
- **Description**: Immutable state updates
- **Usage**: With Zustand untuk simpler state updates

## Data Fetching & Caching

### TanStack Query (React Query)

- **Version**: 5.59.20
- **Package**: `@tanstack/react-query`
- **Website**: https://tanstack.com/query
- **Features**:
  - Data fetching
  - Caching
  - Synchronization
  - Background refetching
  - Pagination
  - Infinite scrolling
  - Optimistic updates
- **Why React Query**:
  - Automatic caching
  - Request deduplication
  - Background updates
  - Error retry logic
  - DevTools

## HTTP Client

### Axios

- **Version**: 1.7.7
- **Description**: Promise-based HTTP client
- **Features**:
  - Request/Response interceptors
  - Automatic JSON transformation
  - Request cancellation
  - Timeout handling
  - Error handling
- **Usage**: API communication dengan Aerplus backend

## UI Components & Libraries

### Radix UI

Headless UI primitives:
- **@radix-ui/react-alert-dialog**: 1.1.2 - Alert dialogs
- **@radix-ui/react-slot**: 1.1.0 - Slot composition

### Headless UI

- **@headlessui/react**: 2.2.0
- **Description**: Unstyled, accessible UI components
- **Components**: Transitions, Dialogs, Menus

### Icon Libraries

#### Remix Icon

- **@remixicon/react**: 4.5.0
- **Description**: Open-source icon library
- **Usage**: Primary icon set

#### Lucide React

- **lucide-react**: 0.460.0
- **Description**: Beautiful & consistent icon set
- **Usage**: Alternative icons

## Maps & Location

### Mapbox GL JS

- **Version**: 3.8.0
- **Package**: `mapbox-gl`
- **Description**: Interactive maps library
- **Features**:
  - Interactive maps
  - Custom markers
  - Geolocation
  - Custom styling
  - Vector tiles
- **Usage**: Location selection, delivery address

### Mapbox Geocoder

- **Version**: 5.0.3
- **Package**: `@mapbox/mapbox-gl-geocoder`
- **Description**: Address search dan geocoding
- **Features**:
  - Address autocomplete
  - Reverse geocoding
  - Place search

## Utilities & Helpers

### Day.js

- **Version**: 1.11.13
- **Description**: Date manipulation library
- **Features**:
  - Date formatting
  - Date parsing
  - Relative time
  - Localization
- **Why Day.js**: Lightweight alternative to Moment.js (2KB vs 67KB)

### Cookies Next

- **Version**: 5.0.2
- **Description**: Cookie handling untuk Next.js
- **Features**:
  - Server-side cookies
  - Client-side cookies
  - Cookie encryption
- **Usage**: Authentication tokens, user preferences

### JWT

- **jsonwebtoken**: 9.0.2
- **Description**: JSON Web Token implementation
- **Usage**: Token verification, user authentication

## UI Feedback & Notifications

### React Hot Toast

- **Version**: 2.4.1
- **Description**: Toast notifications
- **Features**:
  - Beautiful toasts
  - Customizable
  - Accessible
  - Promise-based

### React Toastify

- **Version**: 10.0.6
- **Description**: Alternative toast library
- **Features**: More customization options

## Interactive Components

### React Rating

- **@smastrom/react-rating**: 1.5.0
- **Description**: Star rating component
- **Features**:
  - Customizable stars
  - Half-star support
  - Read-only mode
- **Usage**: Product reviews, order ratings

### React Intersection Observer

- **Version**: 9.13.1
- **Description**: Detect element visibility
- **Features**:
  - Lazy loading
  - Infinite scroll
  - Scroll animations
- **Usage**: Image lazy loading, infinite product list

### React Simple Image Viewer

- **Version**: 1.2.2
- **Description**: Image lightbox viewer
- **Features**:
  - Full-screen images
  - Zoom
  - Navigation
- **Usage**: Product image gallery

## Development Tools

### ESLint

- **Version**: 8.x
- **Package**: `eslint` + `eslint-config-next`
- **Description**: JavaScript/TypeScript linter
- **Config**: Next.js recommended rules
- **Usage**: Code quality, best practices

### PostCSS

- **Version**: 8.x
- **Description**: CSS transformations
- **Usage**: Process Tailwind CSS

## Project Structure

```
src/
├── app/                    # Next.js App Router
│   ├── page.tsx           # Home page
│   ├── layout.tsx         # Root layout
│   ├── globals.css        # Global styles
│   ├── login/             # Login page
│   ├── order/             # Order flow
│   │   ├── page.tsx       # Product selection
│   │   ├── address.tsx    # Address selection
│   │   ├── payment-method.tsx
│   │   └── product.tsx
│   ├── my-order/          # Order history
│   │   ├── page.tsx       # Order list
│   │   ├── status.tsx     # Order status
│   │   └── detail/        # Order detail
│   │       └── [orderId]/
│   │           ├── page.tsx
│   │           ├── payment.tsx
│   │           └── review.tsx
│   ├── my-profile/        # User profile
│   │   ├── page.tsx
│   │   └── edit-location/
│   └── address/           # Address management
│       └── edit/
├── store/                 # Zustand stores
│   ├── customer.ts
│   ├── order.ts
│   └── outlet.ts
├── util/                  # Utilities
│   ├── provider.tsx       # Context providers
│   ├── services.ts        # API services
│   └── util.ts            # Helper functions
├── interface.ts           # TypeScript interfaces
└── middleware.ts          # Next.js middleware
```

## Routing

### App Router

Next.js 15 menggunakan App Router (file-based routing):
- `app/page.tsx` → `/`
- `app/login/page.tsx` → `/login`
- `app/order/page.tsx` → `/order`
- `app/my-order/detail/[orderId]/page.tsx` → `/my-order/detail/:orderId`

### Dynamic Routes

- `[orderId]`: Dynamic segment
- `[...slug]`: Catch-all segment

### Layouts

- `layout.tsx`: Nested layouts
- Shared UI across routes
- Persistent state

## Middleware

File: `src/middleware.ts`
- Authentication check
- Route protection
- Redirects
- Header modification

## API Integration

### Service Layer

File: `src/util/services.ts`
- Centralized API calls
- Axios instance configuration
- Request/Response interceptors
- Error handling

### Authentication

- Token storage via cookies
- Automatic token injection
- Token refresh logic

## Styling Architecture

### Tailwind Configuration

File: `tailwind.config.ts`
- Custom colors
- DaisyUI themes
- Custom spacing
- Border radius
- Animations

### Global Styles

File: `src/app/globals.css`
- CSS custom properties
- Tailwind directives
- Global resets

## Performance Optimization

### Built-in Next.js Optimizations

- **Image Optimization**: `next/image` component
- **Font Optimization**: `next/font`
- **Code Splitting**: Automatic per route
- **Lazy Loading**: Dynamic imports
- **Static Generation**: For static pages
- **Server Components**: Default di App Router

### Custom Optimizations

- React Query caching
- Intersection Observer untuk lazy loading
- Zustand untuk efficient re-renders
- Memoization dengan useMemo/useCallback

## TypeScript Configuration

File: `tsconfig.json`
- **Target**: ES2017
- **Module**: ESNext
- **JSX**: preserve (Next.js handles)
- **Strict mode**: enabled
- **Path aliases**: `@/*` maps to root

## Build & Bundler

### Turbopack

- Next.js 15 development bundler
- Faster than Webpack
- Automatic dengan `--turbopack` flag
- Incremental compilation

### Production Build

- Webpack-based (stable)
- Optimizations:
  - Minification
  - Tree shaking
  - Code splitting
  - Image optimization

## Environment Support

### Browsers

- Modern browsers (ES2017+)
- No IE11 support
- Progressive enhancement

### Devices

- Mobile-first approach
- Responsive design
- Touch-friendly UI
- PWA-ready

## Third-party Services

### Required

- **Aerplus Backend API**: REST API
- **Mapbox**: Maps dan geocoding

### Optional

- **Analytics**: Google Analytics, etc
- **Error Tracking**: Sentry, etc
- **Performance Monitoring**: Vercel Analytics

---

**Selanjutnya**: [Development Setup](./development-setup)
