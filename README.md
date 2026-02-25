# InternalToolKit: Enterprise Operations Monorepo

[![Vercel Deployment](https://img.shields.io/badge/Vercel-Deployed-black?logo=vercel)](https://internal-tool-kit-web.vercel.app)
[![Node.js](https://img.shields.io/badge/Node.js-LTS-339933?logo=node.js)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-Strict-3178C6?logo=typescript)](https://www.typescriptlang.org/)

InternalToolKit is a high-signal, production-ready monorepo centralizing core business operations: fleet management, shift scheduling, and real-time team communication. Built with an "Edge-First" philosophy, it prioritizes sub-100ms latency, strict type-safety, and operational resilience.

## 🏗 Architecture & Design Patterns

The platform is architected as a **Contract-Driven Monorepo** to ensure maximum reliability across distributed services.

### Core Architecture
- **Monorepo Engine:** Managed via `pnpm workspaces` for atomic dependency management.
- **Contract-First API:** Shared `Zod` schemas between `apps/web` (Next.js) and `apps/api` (Cloudflare Workers) to guarantee 100% type-safety at the networking layer.
- **Edge Data Layer:** Optimized for global delivery using Cloudflare's edge network for the API, paired with a pooled PostgreSQL instance for high-concurrency relational data.
- **State Strategy:** Hybrid approach using **React Server Components (RSC)** for data hydration and **Zustand** for interactive, optimistic UI updates in the dashboard.

### Tech Stack
- **Frontend:** Next.js 14+ (App Router), Tailwind CSS, Framer Motion, Lucide.
- **Backend:** Cloudflare Workers (Edge Functions), Node.js.
- **Database:** PostgreSQL (Supabase/Neon), Prisma ORM, Knex.js (Migrations).
- **Validation/Types:** Zod, TypeScript (Strict Mode).
- **Tooling:** Vitest, Playwright, ESLint, Prettier.

---

## 🚀 Key Modules

### 1. Fleet Management Pipeline (V2)
A real-time vehicle logistics engine featuring a robust state machine for vehicle status tracking (Available → In Use → Maintenance → Incident). Optimized with PostgreSQL indexes for fast lookups across thousands of assets.

### 2. Workforce Shift Planner
An interactive timeline interface built with CSS Grid and windowed rendering. It includes real-time conflict detection and automated shift-overlap validation at the database level.

### 3. Unified Ops Inbox
A centralized hub for operational tasks and internal team chat. Features "Workspaces" isolation, ensuring data privacy and role-based access control (RBAC) across different departments.

---

## 🛠 Engineering Standards

- **Security:** Session management via HTTP-only cookies, strict RBAC middleware, and input sanitization via Zod.
- **Performance:** Cursor-based pagination for large datasets, SVG-based iconography, and optimized bundle sizes.
- **Observability:** Centralized logging and health endpoints (`/api/health`) for database and worker heartbeat monitoring.
- **Resilience:** Circuit breaker patterns for AI provider fallbacks and schema-fallback banners for graceful degradation.

---

## 🚦 Local Development

### Prerequisites
- Node.js LTS & pnpm
- A running PostgreSQL instance (or Supabase project)

### Setup
1. **Clone & Install:**
   ```bash
   git clone https://github.com/kostasuser01gr/InternalToolKit-Portfolio
   pnpm install
   ```

2. **Environment Configuration:**
   Copy `.env.example` in `apps/web` and `apps/api` and fill in the required fields:
   ```env
   DATABASE_URL="postgresql://..."
   SESSION_SECRET="your-32-char-secret"
   NEXT_PUBLIC_API_URL="http://localhost:8787"
   ```

3. **Database Setup:**
   ```bash
   pnpm --filter @internal-toolkit/web db:migrate:dev
   pnpm --filter @internal-toolkit/web db:seed
   ```

4. **Run Workspace:**
   ```bash
   pnpm dev
   ```

---

## 🧪 Testing & QA

Run the full suite of unit and E2E tests:
```bash
# Unit tests
pnpm test:unit

# E2E Smoke tests (Playwright)
pnpm test:e2e
```

---

## 📐 Trade-offs & Decisions

- **Auth Strategy:** Chose a PIN-based rapid-access flow for internal staff, prioritizing speed of entry in high-pressure environments over multi-factor authentication for standard roles.
- **API Choice:** Selected Cloudflare Workers over standard Node.js servers to ensure minimal latency for field staff accessing the app from mobile devices in varied connectivity zones.
- **DB Migration Logic:** Implemented a custom migration script (`migrate-deploy.mjs`) to handle zero-downtime schema updates during Vercel deployments.

---

## 📄 License
MIT. Created by **Kostas** (@kostasuser01gr).
