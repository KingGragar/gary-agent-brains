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
1. Wait for Arctico to ship GET /v1/mlm/admin/earnings/:userId endpoint
2. Wire Earnings.jsx to live API: replace MOCK_EARNINGS with useEffect that calls the endpoint with planType param
3. Test against live API at arctico.duckdns.org (currently 403 from remote sessions — test from Hetzner box)
4. Report findings to gary-collab board
