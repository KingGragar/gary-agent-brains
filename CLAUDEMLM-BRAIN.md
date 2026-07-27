# CLAUDEMLM — MLM Agent Brain
> Auto-generated 2026-07-16. Read this FIRST on every session start.

## WHO YOU ARE
You are `claudemlm` — Gary's MLM agent on Arctico's Hetzner box (/home/gary/projects/mlm). You own the Nordic Vitals MLM frontend — a React/Vite dashboard that visualises and interacts with Arctico's MLM engine. You are a TENANT-SIDE product builder, not a platform engineer.

## THE PROJECT
**Nordic Vitals** is the MLM member/admin frontend for Arctico's MLM engine. It is a React 18 + Vite SPA deployed on Vercel. It connects to the live Arctico MLM API at `https://arctico.duckdns.org`.

- **Vercel URL:** https://nordic-vitals.vercel.app
- **GitHub repo:** KingGragar/nordic-vitals
- **Working dir on box:** /home/gary/projects/mlm
- **Local dev:** /tmp/nordic-vitals/ (on Gary's Mac)

## THE ARCTICO MLM ENGINE
One reusable engine powering every MLM plan: binary, unilevel, matrix, forced matrix, breakaway, board/cycle, monoline. All via a SINGLE `/v1/mlm/*` API — plan type is a parameter, not separate endpoints.

**MLMT token:** 100M supply, ledger-only testnet token. All balances in MLMT, not NOK/USD.

## WHAT'S BEEN BUILT (your starting point)
| Feature | Status | Notes |
|---|---|---|
| Binary tree visualisation | ✅ LIVE | Uses react-d3-tree, calls GET /v1/mlm/genealogy/tree/:root |
| Wallet page | ✅ LIVE | Shows MLMT balances, calls getUserTransactions() |
| Admin/Reports | ✅ LIVE | MLMT token summary from /admin/summary |
| Earnings Dashboard | ✅ built | MOCK data — endpoint not yet shipped by Arctico |
| 12 Marp presentations | ✅ pushed | gary-collab/mlm-demo/presentations/output/ (HTML+PDF) |

## API ENDPOINTS (live at arctico.duckdns.org)
```
# Genealogy
POST /v1/mlm/genealogy/enroll          — enroll member
GET  /v1/mlm/genealogy/node/:id        — get node by UUID (NOT integer)
GET  /v1/mlm/genealogy/node-by-user/:userId
GET  /v1/mlm/genealogy/tree/:rootId?tree=placement&depth=10  — flat nodes[]
GET  /v1/mlm/genealogy/upline/:id?tree=placement

# Volume
POST /v1/mlm/volume/event              — post volume event

# Transactions
GET  /v1/mlm/transactions/user/:userId
GET  /v1/mlm/transactions/admin        — all transactions

# Admin
GET  /v1/mlm/admin/summary             — token summary (total_supply, holders, total_bonus_paid)
GET  /v1/mlm/admin/transactions
# PENDING (not yet live):
# GET /v1/mlm/admin/earnings/:userId
```

## TREE VISUALISATION (key implementation detail)
- ROOT_ID = `efbb8d0e-b5a5-4a15-bcc6-2f07b980ca64`
- API returns flat `nodes[]` with `placement_parent_id` — nested client-side
- Colours: gold=#c9a84c (root), blue=#3b82f6 (L leg), green=#22c55e (R leg)
- Click node → side drawer with details
- Library: react-d3-tree

## API CLIENT (src/api/mlmApi.js)
Key exports: `getTree(rootNodeId, {tree, depth})`, `getNode(id)`, `getNodeByUser(userId)`, `getUserTransactions(userId)`, `getAdminSummary()`, `getAdminTransactions()`, `postVolumeEvent()`, `enrollMember()`

Falls back to mock data if `VITE_MLM_API_URL` not set.

## TECH STACK
- React 18 + Vite
- react-router-dom (routes: /, /login, /join, /dashboard/*, /admin/*)
- react-d3-tree (binary tree)
- recharts (earnings charts)
- qrcode.react (QR codes in wallet)
- VITE_MLM_API_URL + VITE_MLM_API_KEY env vars (set in Vercel)

## OPEN TASKS (wire these next)
1. ~~**Plan type switch**~~ ✅ DONE (2026-07-18 run 2 + run 4): plan_type selector live in Tree.jsx (calls API with plan_type param) and Earnings.jsx. Binary-specific sections (Leg Balance, rank requirements) now conditional on selected plan type. Commits: e67114e, 7565738 on main.
2. **Wire earnings endpoint** — replace mock in Earnings.jsx when GET /v1/mlm/admin/earnings/:userId ships from Arctico
3. **Vercel deploy** — after any code changes, Gary runs `vercel --prod` in /tmp/nordic-vitals/ (remote agents can push code, but can't deploy to Vercel)

## DEMO PRESENTATIONS (already done — do not redo)
12 presentations built in Marp (dark/glass theme), HTML+PDF:
- master-overview, binary, unilevel, forced-matrix, breakaway, monoline, board-cycle, slot-ladder, social-circle, override-pool, package-webshop, earnings-dashboard
All in gary-collab/mlm-demo/presentations/output/

## MLM PLAN CATALOG (all via same /v1/mlm/* endpoints)
Binary, Unilevel, Matrix/Forced Matrix, Breakaway, Board/Cycle, Monoline, Slot-Ladder (demo-only), plus support systems: Social Circle, Override+Pool, Package/Webshop, Earnings Dashboard

## SAFETY (X-factor cap)
Total paid out ≤ X% of turnover, always. Points → real payouts only through capped weekly run. Mathematically impossible to overpay.

## GOLDEN RULES
- All money paths stage-only until Bjørn signs off
- Pay-to-enter plan types (Slot-Ladder, Board/Cycle) are demo_only in code — never flip without legal review
- Use /v1/mlm/* API only. No direct DB access.
- Post progress to gary-collab BOARD.jsonl

## NEXT_ACTION
1. ~~NOK→MLMT currency sweep~~ ✅ DONE (runs 10+11)
2. ~~UX: commission tabs filtering, orders empty/loading states, wallet pending balance~~ ✅ DONE (run 22, commit 7b23974)
3. Wait for Arctico to ship GET /v1/mlm/admin/earnings/:userId endpoint
4. ~~Wire Earnings.jsx to live API~~ ✅ DONE (2026-07-25 board-watcher audit): useEffect + getEarnings() already wired in Earnings.jsx and mlmApi.js. MOCK_EARNINGS is the initial state / fallback — correct design since endpoint not live yet.
5. Test against live API at arctico.duckdns.org (currently 403 from remote sessions — test from Hetzner box)
6. Report findings to gary-collab board
7. ~~Commission Calculator~~ ✅ DONE (run 49, commit 4673a34): /dashboard/calculator with Binary/Unilevel/Breakaway tabs, slider inputs, live commission breakdown bars, rank table.
8. ~~Admin Member Detail~~ ✅ DONE (run 50, commit c21fc62): Members.jsx upgraded to full tabbed detail panel (Profile/Commissions/Downline/Actions). Profile tab: name/email/phone/country/sponsor/PV/GV/joined + admin note (persisted via addMemberNote). Commissions tab: recent commission history + orders table. Downline tab: direct recruit list with combined GV total. Actions tab: status toggle with confirm, manual rank override with confirm, password reset email, contact details. CSV export now includes email/phone/country. Row click opens panel. getMemberDetail/updateMemberStatus/setMemberRank/addMemberNote all mock-safe.

## BOARD ACCESS NOTE (2026-07-27 — STILL BLOCKED)
Board-watcher cannot access buge4/gary-collab from remote sessions (GitHub scope is KingGragar only).
Fix needed: add KingGragar as collaborator on buge4/gary-collab, OR move the board to KingGragar/gary-collab.
Also: arctico.duckdns.org is 403/unreachable from remote sessions (egress proxy blocks it) — API tests must be run from Hetzner box.
Latest nordic-vitals push: 2026-07-27 (commit 4abc8fd) — Promo Codes system: /admin/promos (KPI cards, create modal, activate/deactivate, delete, search/filter/pagination), Checkout.jsx promo input + discount line + adjusted total, validatePromoCode/getAdminPromos/createPromoCode/togglePromoCode/deletePromoCode mock-safe in mlmApi.js, PROMO_CODES mock dataset (6 codes), AdminLayout Promo Codes nav, App.jsx route, Admin Overview quick-nav card. Board-watcher run 56: board still inaccessible, no new Bjorn tasks.
