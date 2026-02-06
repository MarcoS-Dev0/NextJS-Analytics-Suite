<div align="center">

# 📊 NextJS-Analytics-Suite

**Real-time sales analytics dashboard — dark mode, blazing fast, beautifully charted.**

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen?style=flat-square)](https://github.com/) [![Next.js](https://img.shields.io/badge/Next.js-14.1-000000?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org) [![React](https://img.shields.io/badge/React-18.2-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev) [![TypeScript](https://img.shields.io/badge/TypeScript-5.3-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org) [![License: MIT](https://img.shields.io/badge/license-MIT-yellow?style=flat-square)](LICENSE) [![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com) [![Vercel](https://img.shields.io/badge/deploy-Vercel-000?style=flat-square&logo=vercel&logoColor=white)](https://vercel.com)

<br/>

<img src="public/screenshots/dashboard-dark.png" alt="Dashboard Preview" width="780"/>

<br/>

*An admin dashboard for e-commerce operators who need real-time visibility into revenue, conversion funnels, product performance, and customer behavior — all in a sleek dark-mode interface powered by SSR and WebSockets.*

</div>

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Architecture](#architecture)
- [Theming](#theming)
- [Testing](#testing)
- [Deployment](#deployment)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

NextJS-Analytics-Suite is a **self-hosted analytics dashboard** designed to plug into any e-commerce backend via REST or WebSocket. It provides pre-built views for the metrics that matter most — revenue trends, average order value, conversion rates, top products, geographic distribution, and cohort retention — rendered as interactive, animated charts with sub-second data refresh.

Built with the **Next.js 14 App Router**, the dashboard leverages React Server Components for initial payload optimization and client-side Recharts for smooth, interactive visualizations. The dark-mode-first design system is built entirely on Tailwind CSS with CSS custom properties, making theme customization trivial.

---

## Features

### Analytics Views
- **Revenue Dashboard** — line/area charts with daily, weekly, and monthly aggregation; YoY comparison overlay; goal tracking with threshold indicators
- **Conversion Funnel** — step-by-step funnel visualization (visit → cart → checkout → purchase) with drop-off percentages and segmentation by traffic source
- **Product Performance** — ranked bar charts, treemaps, and sortable data tables for SKU-level revenue, units sold, return rate, and margin
- **Customer Insights** — cohort retention heatmap, LTV distribution histogram, RFM segmentation scatter plot, and new vs. returning customer split
- **Geographic Heatmap** — interactive choropleth map with drill-down by country/region, powered by D3-geo projections
- **Real-Time Feed** — live order stream via WebSocket with auto-scrolling transaction log and running daily total

### UI / UX
- **Dark Mode First** — full dark palette with optional light mode toggle; respects `prefers-color-scheme` on first load
- **Responsive Layout** — adaptive grid from mobile (single column) to ultrawide (4-column) with collapsible sidebar navigation
- **Animated Charts** — smooth enter/update/exit transitions via Recharts + Framer Motion; skeleton loaders during data fetch
- **Command Palette** — `⌘K` global search to jump between views, filter date ranges, or trigger exports
- **Keyboard Navigation** — full `aria-*` accessibility; tab-through for all interactive elements

### Data & Performance
- **Server-Side Rendering** — critical metrics rendered on the server via RSC for instant first paint; hydrated with client interactivity
- **Incremental Static Regeneration** — summary pages (daily/weekly) cached and revalidated on a 60-second interval
- **Optimistic UI** — date range and filter changes reflect immediately with local state while the API resolves
- **Data Export** — one-click CSV/PDF export for any chart or table; scheduled email reports via cron endpoint

---

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Framework | Next.js 14 (App Router) | SSR, RSC, routing, API routes |
| UI Library | React 18 | Component architecture |
| Language | TypeScript 5.3 | Type safety across the stack |
| Styling | Tailwind CSS 3.4 | Utility-first styling + dark mode |
| Charts | Recharts 2.x | Composable chart components |
| Maps | D3-geo + TopoJSON | Geographic visualizations |
| Animations | Framer Motion 11 | Chart transitions + page animations |
| State | Zustand 4.x | Lightweight client state management |
| Data Fetching | TanStack Query 5 | Cache, dedup, background refresh |
| WebSocket | Socket.IO Client | Real-time order feed |
| Auth | NextAuth.js 5 | Admin session management |
| Testing | Vitest + Testing Library | Unit + integration tests |
| E2E | Playwright | Cross-browser E2E flows |
| Linting | ESLint + Prettier | Code quality enforcement |

---

## Project Structure

```
NextJS-Analytics-Suite/
├── .github/
│   └── workflows/
│       ├── ci.yml                    # Lint + test + type-check
│       └── preview.yml               # Vercel preview deployment
├── public/
│   ├── screenshots/
│   │   ├── dashboard-dark.png
│   │   └── dashboard-light.png
│   └── favicon.ico
├── src/
│   ├── app/
│   │   ├── layout.tsx                # Root layout (providers, sidebar)
│   │   ├── page.tsx                  # Dashboard home (revenue overview)
│   │   ├── loading.tsx               # Global skeleton fallback
│   │   ├── (dashboard)/
│   │   │   ├── revenue/
│   │   │   │   └── page.tsx          # Revenue deep-dive
│   │   │   ├── funnel/
│   │   │   │   └── page.tsx          # Conversion funnel analysis
│   │   │   ├── products/
│   │   │   │   └── page.tsx          # Product performance tables
│   │   │   ├── customers/
│   │   │   │   └── page.tsx          # Cohort + RFM analytics
│   │   │   ├── geo/
│   │   │   │   └── page.tsx          # Geographic heatmap
│   │   │   └── live/
│   │   │       └── page.tsx          # Real-time order feed
│   │   ├── api/
│   │   │   ├── auth/[...nextauth]/
│   │   │   │   └── route.ts          # NextAuth handler
│   │   │   ├── metrics/
│   │   │   │   ├── revenue/route.ts  # Revenue aggregation endpoint
│   │   │   │   ├── funnel/route.ts   # Funnel data endpoint
│   │   │   │   └── products/route.ts # Product metrics endpoint
│   │   │   └── export/
│   │   │       └── route.ts          # CSV/PDF export handler
│   │   └── globals.css               # Tailwind base + CSS variables
│   ├── components/
│   │   ├── charts/
│   │   │   ├── RevenueChart.tsx      # Area/line chart with toggles
│   │   │   ├── FunnelChart.tsx       # Step funnel visualization
│   │   │   ├── ProductBar.tsx        # Horizontal ranked bar chart
│   │   │   ├── CohortHeatmap.tsx     # Retention heatmap grid
│   │   │   ├── GeoMap.tsx            # D3 choropleth projection
│   │   │   └── LiveTicker.tsx        # Real-time order scroll
│   │   ├── layout/
│   │   │   ├── Sidebar.tsx           # Collapsible nav sidebar
│   │   │   ├── TopBar.tsx            # Search, theme toggle, user menu
│   │   │   └── CommandPalette.tsx    # ⌘K modal
│   │   ├── ui/
│   │   │   ├── Card.tsx              # Metric card with sparkline
│   │   │   ├── DataTable.tsx         # Sortable, paginated table
│   │   │   ├── Skeleton.tsx          # Loading placeholder
│   │   │   ├── Badge.tsx             # Status/tag badge
│   │   │   └── DateRangePicker.tsx   # Calendar range selector
│   │   └── providers/
│   │       ├── ThemeProvider.tsx      # Dark/light mode context
│   │       ├── QueryProvider.tsx      # TanStack Query client
│   │       └── SocketProvider.tsx     # WebSocket connection manager
│   ├── hooks/
│   │   ├── useMetrics.ts             # TanStack Query wrappers
│   │   ├── useLiveOrders.ts          # WebSocket subscription hook
│   │   ├── useDebounce.ts            # Input debounce utility
│   │   └── useMediaQuery.ts          # Responsive breakpoint hook
│   ├── lib/
│   │   ├── api-client.ts             # Typed fetch wrapper
│   │   ├── formatters.ts             # Currency, date, number formatters
│   │   ├── constants.ts              # Color palettes, breakpoints
│   │   └── auth.ts                   # NextAuth configuration
│   ├── stores/
│   │   ├── filterStore.ts            # Date range + segment filters
│   │   └── uiStore.ts               # Sidebar state, command palette
│   └── types/
│       ├── metrics.ts                # Revenue, funnel, product types
│       ├── orders.ts                 # Order + line item types
│       └── api.ts                    # API response wrappers
├── tests/
│   ├── unit/
│   │   ├── components/
│   │   │   ├── RevenueChart.test.tsx
│   │   │   └── DataTable.test.tsx
│   │   └── hooks/
│   │       └── useMetrics.test.ts
│   ├── integration/
│   │   └── dashboard.test.tsx
│   └── e2e/
│       ├── navigation.spec.ts
│       └── export.spec.ts
├── tailwind.config.ts
├── next.config.mjs
├── tsconfig.json
├── vitest.config.ts
├── playwright.config.ts
├── package.json
├── .env.example
└── README.md
```

---

## Getting Started

### Prerequisites

- Node.js ≥ 18.17 (LTS)
- pnpm ≥ 8.x (recommended) or npm
- A running backend API that exposes metrics endpoints (or use the built-in mock server)

### 1. Clone & Install

```bash
git clone https://github.com/yourusername/NextJS-Analytics-Suite.git
cd NextJS-Analytics-Suite

# Install dependencies
pnpm install
```

### 2. Configure Environment

```bash
cp .env.example .env.local
# Edit .env.local — see Environment Variables below
```

### 3. Start Development Server

```bash
# With mock data (no backend required)
pnpm dev:mock

# With live backend
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000). The dashboard hot-reloads on file changes.

### 4. Build for Production

```bash
pnpm build
pnpm start
```

---

## Environment Variables

| Variable | Description | Default |
|----------|------------|---------|
| `NEXT_PUBLIC_API_BASE_URL` | Backend API base URL | `http://localhost:8000/api/v1` |
| `NEXT_PUBLIC_WS_URL` | WebSocket server URL | `ws://localhost:8000/ws` |
| `NEXTAUTH_URL` | Canonical app URL | `http://localhost:3000` |
| `NEXTAUTH_SECRET` | Session encryption secret | *required* |
| `AUTH_PROVIDER` | OAuth provider (`github`, `google`, `credentials`) | `credentials` |
| `GITHUB_ID` | GitHub OAuth app ID | `""` |
| `GITHUB_SECRET` | GitHub OAuth app secret | `""` |
| `MOCK_DATA` | Enable mock data mode | `false` |
| `REVALIDATE_INTERVAL` | ISR revalidation in seconds | `60` |

---

## Architecture

```
┌────────────────────────────────────────────────────┐
│                    Browser                          │
│  ┌──────────────────────────────────────────────┐  │
│  │             Next.js App Shell                 │  │
│  │  ┌────────┐  ┌────────────┐  ┌────────────┐ │  │
│  │  │Sidebar │  │ Chart View │  │  TopBar     │ │  │
│  │  │  Nav   │  │ (Recharts) │  │ ⌘K + Theme  │ │  │
│  │  └────────┘  └─────┬──────┘  └────────────┘ │  │
│  │                     │                         │  │
│  │         ┌───────────┴───────────┐            │  │
│  │         │  TanStack Query Cache │            │  │
│  │         └───────────┬───────────┘            │  │
│  └─────────────────────┼────────────────────────┘  │
│                        │                            │
└────────────────────────┼────────────────────────────┘
               ┌─────────┴─────────┐
               │   REST / WebSocket │
               └─────────┬─────────┘
        ┌────────────────┼────────────────┐
   ┌────▼────┐     ┌─────▼─────┐    ┌────▼────┐
   │ Metrics │     │  Auth API  │    │  WS     │
   │   API   │     │ (NextAuth) │    │ Server  │
   └─────────┘     └───────────┘    └─────────┘
```

Data flows through three channels. **REST** for paginated, filterable metric queries (revenue, products, funnels) cached by TanStack Query with stale-while-revalidate. **WebSocket** for the real-time order feed, managed by a singleton Socket.IO client in React context. **ISR** for pre-rendered summary pages that revalidate on a configurable interval, ensuring fast TTFB even under load.

---

## Theming

The dark mode system is built on CSS custom properties scoped to `[data-theme="dark"]` and `[data-theme="light"]` on the `<html>` element. Tailwind's `darkMode: 'class'` strategy is used in combination.

```css
/* globals.css */
:root {
  --bg-primary:    #0a0a0f;
  --bg-secondary:  #12121a;
  --bg-card:       #1a1a28;
  --text-primary:  #e4e4e7;
  --text-secondary:#a1a1aa;
  --accent:        #6366f1;
  --accent-hover:  #818cf8;
  --success:       #22c55e;
  --warning:       #eab308;
  --danger:        #ef4444;
  --border:        #27272a;
  --chart-1:       #6366f1;
  --chart-2:       #8b5cf6;
  --chart-3:       #a78bfa;
  --chart-4:       #c4b5fd;
  --chart-5:       #22c55e;
}
```

To customize the palette, override these variables in `globals.css` or pass a custom config to `ThemeProvider`. All chart colors reference these tokens, so the entire dashboard rebrands with a single variable change.

---

## Testing

```bash
# Unit + integration tests
pnpm test

# Watch mode
pnpm test:watch

# Coverage report
pnpm test:coverage

# E2E tests (requires running dev server)
pnpm test:e2e

# Type checking
pnpm type-check
```

---

## Deployment

### Vercel (Recommended)

```bash
# One-command deploy
npx vercel --prod
```

Set environment variables in the Vercel dashboard under **Settings → Environment Variables**.

### Docker

```bash
docker build -t analytics-suite .
docker run -p 3000:3000 --env-file .env.local analytics-suite
```

### Static Export

For CDN deployment without a Node.js server:

```bash
# next.config.mjs → output: 'export'
pnpm build
# Deploy ./out to any static host (Cloudflare Pages, Netlify, S3+CloudFront)
```

> **Note:** Static export disables API routes and ISR. Use this only if your metrics API is fully external.

---

## Roadmap

- [x] Revenue + Funnel + Product dashboards
- [x] Dark mode with theme toggle
- [x] Real-time order feed via WebSocket
- [x] CSV/PDF export
- [ ] Embeddable chart widgets (iframe + SDK)
- [ ] Custom dashboard builder (drag-and-drop layout)
- [ ] Anomaly detection alerts (Z-score based)
- [ ] Mobile app (React Native with shared chart components)

---

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feat/new-chart-type`)
3. Write tests for new functionality
4. Ensure `pnpm test && pnpm lint && pnpm type-check` all pass
5. Commit with conventional commits (`git commit -m 'feat: add bubble chart component'`)
6. Open a Pull Request against `main`

---

## License

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for details.

---

<div align="center">
  <sub>Crafted for developers who believe dashboards should be as fast as the data they display.</sub>
</div>
