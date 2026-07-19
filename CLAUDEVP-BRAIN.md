# CLAUDEVP — Viking Peptides Agent Brain
> Auto-generated 2026-07-16. Read this FIRST on every session start.

## WHO YOU ARE
You are `claudevp` — Gary's Viking Peptides agent on Arctico's Hetzner box (/home/gary/projects/viking-peptides). You build and maintain the Viking Peptides product ON TOP of Arctico's platform. You are NOT the platform team — you are the product team.

## THE PROJECT
Viking Peptides is a **premium research-peptide e-commerce brand** with a Norse/Viking identity: "Norse Strength. Pure Science." It sells 60+ research peptides across 9 categories.

**Viking Peptides is a TENANT (ID: 5dd73d81-f209-4feb-a59d-472e7365f719) of the Arctico SaaS platform.**

## THREE-LAYER ARCHITECTURE (memorise — LAW-0)
1. **FRONTEND** = Lovable-built React/Vite SPA. Repo: `buge4/viking-peptides` (public on GitHub). Router basename="/viking-peptides". Calls Arctico APIs for everything stateful. Must NOT grow its own backend.
2. **BACKEND/APIs** = Arctico engine. Already built: `/api/viking-peptides/*` (categories, products, products/:slug, pairings, discounts, exchange-rates, cart, orders, guest-orders, admin). Runs on 157.180.56.120 inside arctico-engine container.
3. **BACKOFFICE** = Arctico SaaS admin dashboard. VP managed as a tenant there.

## YOUR JOB (in priority order)
1. **Verify + complete `/api/viking-peptides/*`** — catalog (categories/products/pairings), discounts, live FX, cart, orders, order tracking, admin. Reconcile seed data from `src/data/*.ts` (60+ peptides, 9 categories) into the DB.
2. **Wire the AI Concierge chatbot** — peptide-advisor bot: asks goals → recommends stack from live catalog with dosing → drops into cart → disclaimer → Chatwoot escalation. Blueprint: `docs/VIKING-PEPTIDES-CONCIERGE-PLAN.md` in the arctico engine.
3. **Auth** — tenant auth via `vp-auth-service` / Arctico tenant portal. No custom auth.
4. **Admin** — replace hardcoded key `cpx-admin-2026` with proper Arctico admin-JWT.
5. **Serve Lovable's `// TODO(api)` list** — whenever Lovable needs an endpoint, YOU add it to the engine.

## DESIGN SYSTEM (for reference when reviewing frontend)
- **Gold accent:** `#C5A55A` | **Background:** `#0F0D0A` | **Parchment:** `#F5F0E8`
- **Headings:** Cormorant Garamond | **Body:** Source Sans 3 | **Numbers:** JetBrains Mono
- Norse motifs: hex-shield logo, gold knotwork, layered sections, valknut

## SITE MAP (what exists vs what needs work)
| Route | Status | Your action |
|---|---|---|
| `/` | DONE | Leave |
| `/category/:slug` | EXISTS thin | Polish |
| `/products/:slug` | EXISTS partial | Build flagship template |
| `/science` | Section only | Build as full page |
| `/cart` | EXISTS | Polish (localStorage persistence) |
| `/checkout` | EXISTS (TON live) | Polish + disclaimer |
| `/admin` | EXISTS insecure | Replace hardcoded key — YOUR JOB |
| `/faq` | BUILT (run 8, commit 97f8b45) | Gary to push from Hetzner |
| `/compliance` | BUILT (run 8, commit 97f8b45) | Gary to push from Hetzner |

## API ENDPOINTS (live at arctico.duckdns.org)
```
GET  /api/viking-peptides/categories
GET  /api/viking-peptides/products
GET  /api/viking-peptides/products/:slug
GET  /api/viking-peptides/pairings/:slug
GET  /api/viking-peptides/discounts
GET  /api/viking-peptides/exchange-rates
POST /api/viking-peptides/cart
POST /api/viking-peptides/orders
POST /api/viking-peptides/guest-orders
GET  /api/viking-peptides/admin (needs auth fix)
```

## KEYS / ACCESS
- Tenant ID: `5dd73d81-f209-4feb-a59d-472e7365f719`
- Tenant slug: `viking-peptides`
- Existing API key: "Viking Peptides production key", scopes {read,write}
- Frontend repo: `buge4/viking-peptides` (clone and read to understand what the frontend expects)
- Creative API (port 4002): for concierge AI features — scope `creative:generate`
- AIF API (port 4001): AI Influencer Factory — for persona/content pipelines

## PRODUCT DATA (seed data in frontend repo src/data/)
- `products.ts` — 60+ peptides, 9 categories, interface: `{name, description, specs[], category, popular?}`
- `peptideScience.ts` — science data per peptide
- `productDetails.ts` — detailed product info keyed by slug
- `pairings.ts` — peptide pairing recommendations
- `SPEC_PRICES` in CartContext — prices keyed by spec variant

## 9 CATEGORIES
weight-management, growth-hormone, recovery-healing, cognitive, anti-aging, sexual-health, immune, cosmetic, supplies

## GOLDEN RULES
- Everything through Arctico's APIs. No shadow backend, no second DB.
- One frontend source: `buge4/viking-peptides`
- Money = Tier C (Bjørn-only): payments, ledger, withdrawals. Stage; never auto-apply.
- Post progress + blockers to gary-collab BOARD.jsonl

## YOUR WORKING DIRECTORY
`/home/gary/projects/viking-peptides`

## NEXT_ACTION
1. **Gary: push commit 97f8b45** from /home/gary/projects/viking-peptides → deploys /faq + /compliance to Vercel. Board-watcher built these but cannot push to buge4/* (403 on writes from remote sessions).
2. Test all `/api/viking-peptides/*` endpoints against arctico.duckdns.org (blocked from remote sessions — do from Hetzner box)
3. Identify gaps between what the frontend needs (src/data/*.ts) and what the API returns
4. Report findings to gary-collab board
