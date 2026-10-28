# Trajectory Store — resellable e‑commerce starter

A single‑vendor e‑commerce product with a themeable storefront, an admin console, a payment gateway, a lightweight CRM, and a mobile app. Branding (store name, colours, logo, currency, region) is driven from the database so the whole thing can be re‑skinned per client.

## Apps

| App | Stack | Port | What it is |
|-----|-------|------|------------|
| `apps/web` | Next.js | 3000 | Customer storefront + admin console (`/admin`) + CRM (`/admin/customers`) |
| `apps/api` | NestJS + Prisma (PostgreSQL) | 3001 | REST API: catalogue, cart, orders, payments, store settings, admin |
| `apps/mobile` | Expo / React Native | 8081 | Customer storefront (shares the branding engine) |
| `apps/mobile-sdk57` | Expo / React Native | — | Alternative mobile variant |

## Getting started

Requires Node.js and a local PostgreSQL instance.

```bash
# API
cd apps/api
cp .env.example .env            # then fill in DATABASE_URL etc.
npm install
npm run db:generate
npm run db:migrate
npm run db:seed:catalog         # optional: demo products
PORT=3001 npm run start:dev     # (PORT is also set in .env)

# Web (second terminal)
cd apps/web
npm install
npm run dev                     # http://localhost:3000

# Mobile (third terminal)
cd apps/mobile
npm install
EXPO_PUBLIC_API_URL=http://<your-lan-ip>:3001 npx expo start
```

## Features

- **Branding engine** — store name, theme colours, logo, currency and region come from `StoreSettings`; storefront, admin, footer and mobile all read it.
- **Catalogue** — products, brands, categories, variants (SKU/model/price/stock), images, specifications.
- **Cart & checkout** — guest checkout with stock reservation.
- **Payments** — Razorpay (test mode) with server‑side order creation and signature verification; falls back to "pay later" when keys are unset.
- **Admin console** — branding, product management, order status, and a **CRM** with customer lifetime value and segments.

## Configuration

Secrets live in `apps/api/.env` (git‑ignored). See `apps/api/.env.example` for the full list, including optional `RAZORPAY_KEY_ID` / `RAZORPAY_KEY_SECRET`.
