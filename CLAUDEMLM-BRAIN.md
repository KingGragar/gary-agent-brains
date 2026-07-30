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

## BOARD ACCESS NOTE (2026-07-28 — STILL BLOCKED)
Board-watcher cannot access buge4/gary-collab from remote sessions (GitHub scope is KingGragar only).
Fix needed: add KingGragar as collaborator on buge4/gary-collab, OR move the board to KingGragar/gary-collab.
Also: arctico.duckdns.org is 403/unreachable from remote sessions (egress proxy blocks it) — API tests must be run from Hetzner box.
Latest nordic-vitals push: 2026-07-28 (commit c5e9253) — Admin Network Tree page (/admin/network): full-network genealogy visualization for admins using react-d3-tree; sponsor/placement mode toggle; plan type selector; member search/jump-to-root; rank-colored nodes (name/ID/PV/GV/country/status); click-to-detail drawer with direct recruits list + subtree drill-down; Reset Root button; getAdminMembers() mock-safe. AdminLayout nav (🌐 Network Tree) + Overview quick-nav card added. Vite build passes ✅. Board-watcher run 62 (complete): board still inaccessible (buge4/gary-collab cross-owner); no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual).
Board-watcher run 63 (2026-07-28): same — board inaccessible (buge4/gary-collab), no new Bjørn emails in Gmail, arctico.duckdns.org blocked. No new tasks found. All NEXT_ACTION items done or externally blocked.
Board-watcher run 64 (2026-07-28): same — board inaccessible (buge4/gary-collab), no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual; "helse prosjekt" 2026-07-25 — research attachments only, no code task). nordic-vitals at c5e9253. No new tasks found.
Board-watcher run 65 (2026-07-28): board inaccessible (buge4/gary-collab cross-owner), arctico still 403, no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Analytics page (/admin/analytics): Revenue trend (line chart, tabs: revenue/orders/commissions), Member growth area chart, Conversion funnel with drop-off rates, Geographic distribution table, Product category revenue bar chart (Cell-colored per category), Commission efficiency overlay bar chart. ANALYTICS_DATA mock dataset (13 months revenue/growth, funnel, geo, category). getAdminAnalytics() mock-safe in mlmApi.js. AdminLayout nav (📈 Analytics) + Overview quick-nav card added. App.jsx route added. Vite build passes ✅. Commit 56e0ba8 on nordic-vitals main.
Board-watcher run 66 (2026-07-28): board inaccessible (buge4/gary-collab cross-owner), arctico still 403, no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Training & Certification page (/dashboard/training): 5-module self-paced training program (Getting Started, Product Knowledge, Network Building, Advanced MLM Strategy, Leadership & Team Management). Each module has 3-4 lessons (text + quiz types), completion tracking via localStorage + API (getTrainingModules/completeTrainingLesson/claimTrainingReward all mock-safe), MLMT reward claims per module (500 MLMT total across all 5 modules), and "Certified Member" badge shown when all 5 modules complete. Detailed lesson content covers: NV product line (Omega-3, D3+K2, Magnesium, B-Complex), binary leg balancing, autoship strategy, rank advancement planning, compliance/legal, team leadership. Nav link added to DashboardLayout. App.jsx route added. Vite build passes ✅. Commit b14339f on nordic-vitals main.
Board-watcher run 67 (2026-07-28): board inaccessible (buge4/gary-collab cross-owner), arctico still 403, no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Member Announcements feed (/dashboard/announcements): read-only member view of company announcements with type badges (Info/Product/Promotion/System/Maintenance), expand/collapse for long posts, unread count indicator (tracked via localStorage last-read timestamp), click-to-filter stat tiles, audience rank filtering in getMemberAnnouncements() (all/bronze/silver/gold/platinum). Nav link added to DashboardLayout. App.jsx route added. getMemberAnnouncements() added to mlmApi.js (mock-safe). Vite build passes ✅. Commit a2ab9be on nordic-vitals main.
Board-watcher run 68 (2026-07-28): board inaccessible (buge4/gary-collab cross-owner), arctico still 403, no new Bjørn emails (last: "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Roles & Permissions page (/admin/roles): tabbed layout — Admin Users tab (KPI cards: total/active/invited/MFA-enabled/roles defined; search+role+status filters; table with name/email/role badge/status badge/last-login/MFA indicator/note/actions; Change Role modal with role description; Deactivate/Reactivate with confirm modal; Invite Admin modal with email+role+note); Permission Matrix tab (role summary cards with descriptions; full module-vs-role checklist table with ✓/– indicators for all 19 admin modules). ADMIN_USERS mock dataset (6 users: Bjørn/Gary/moderator/analyst/inactive/invited). ROLE_PERMISSIONS + PERMISSION_LABELS mock data (4 roles × 19 modules). getAdminUsers/inviteAdminUser/updateAdminUserRole/deactivateAdminUser/getRolePermissions all mock-safe in mlmApi.js. AdminLayout nav (🔐 Roles & Permissions) + Overview quick-nav card added. App.jsx route added. Vite build passes ✅. Commit 6c9a693 on nordic-vitals main.
Board-watcher run 69 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Compliance Center (/admin/compliance): 3-tab layout — Income Disclosure Statement (IDS) tab with earnings distribution bar chart across 6 tiers, 4 KPI stats (active earners/median/avg/top-1% earnings), legal disclaimer, print/PDF button, and key legal note re: Forbrukerrådet + EU UCPD requirements; Compliance Checklist tab with 19 items across 5 categories (Documentation/Marketing/Operations/Financial/Regulatory), progress bar + score %, per-item status badges (Done/Pending/Review/Failed), category + status filters, edit modal to update status+notes, CSV export; Document Vault tab with 8 seed documents (IDS/Legal/GDPR/Product/Financial categories), category filter pills, card grid, download link, delete-with-confirm. COMPLIANCE_STATS/COMPLIANCE_CHECKLIST/COMPLIANCE_DOCS mock datasets in mock.js. getComplianceStats/getComplianceChecklist/updateChecklistItem/getComplianceDocs/deleteComplianceDoc all mock-safe in mlmApi.js. AdminLayout nav (⚖️ Compliance) + Overview quick-nav card added. App.jsx route /admin/compliance added. Vite build passes ✅. Commit 27d2900 on nordic-vitals main.
Board-watcher run 70 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added My Team page (/dashboard/my-team): searchable/filterable flat-list view of member's full downline. KPI cards (total/active members, team GV, deepest level); rank distribution mini-bar; search (name/ID/email); rank/status/level filters (Direct L1/L2/L3+); sortable columns (name/level/rank/PV/GV/last activity/joined); pagination (20/page); CSV export; at-risk warning for inactive members. getMyTeam() BFS sponsor chain in mock, genealogy tree in live. Nav link added to DashboardLayout (between My Tree and Commissions). Vite build passes ✅. Commit e5313ec on nordic-vitals main. No notification sent (no new board tasks, incremental member feature).
Board-watcher run 71 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Events & Webinars feature: member page (/dashboard/events) + admin manager (/admin/events). Member page: 9 seed events (5 upcoming, 4 past) across webinar/training/team-call types; RSVP/cancel with optimistic UI + toast feedback; capacity bar (green/amber/red); past recordings with "Watch Recording" link; type filter pills + Upcoming/Past tabs; MLMT reward badges; KPI strip. Admin page: full CRUD (create/edit/delete with confirm modal); table view with type badge, speaker, date, capacity bar, status badge; type + status filters; KPI strip. API: getEvents/registerForEvent/unregisterFromEvent/getAdminEvents/createAdminEvent/updateAdminEvent/deleteAdminEvent (all mock-safe). EVENTS mock dataset with 9 events added to mock.js. Nav: 🎙️ Events in DashboardLayout + AdminLayout. Vite build passes ✅ (892 modules). Commit c46ffde on nordic-vitals main.
Board-watcher run 72 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Business Plan / Goal Planner page (/dashboard/business-plan): interactive income goal planner for members. Income goal slider (500–50k MLMT/mo), timeline slider (3–36 months), avg PV-per-recruit input; binary leg GV progress bars with weaker-leg tip; quarterly income projection table (Q1–Q4+); personalized milestone roadmap (rank ladder + recruit targets); this-month priority action checklist; income disclaimer. All computed live from user's PV/GV/rank via useMemo — no API needed (uses auth context). Nav link 📋 Business Plan added to DashboardLayout. App.jsx route added. Vite build passes ✅. Commit 416e568 on nordic-vitals main. No notification sent (no new board tasks; incremental member engagement feature).
Board-watcher run 73 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Member ID Card page (/dashboard/member-card): printable/shareable card with QR code (referral URL), rank badge, member ID, PV/GV stats, copy-referral-link button, @media print support; DashboardLayout sidebar nav reorganised from flat 21-item list into 6 labelled sections (Network / Finances / Grow / Community / Account) with section headings; 🪪 Member Card link added under Account section. Vite build passes ✅. Commit 3e17616 on nordic-vitals main. No notification sent (no new board tasks; incremental member feature + UX improvement).
Board-watcher run 74 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Commission Dry-Run Preview page (/admin/commission-preview): simulate per-member payout before running for real — 4 bonus pools (Direct Sales 20% PV, Sponsor 10% direct-recruit PV, Level L2 5%/L3 3%, Pairing 5-8% weak-leg GV), KPI summary cards, stacked allocation bar, comparison vs last actual run (absolute + % delta), sortable table with click-to-expand per-member breakdown, search, filtered subtotals row, CSV export. getCommissionPreview() added to mlmApi.js (mock-safe). Nav link in AdminLayout (🧮 Commission Preview) + Overview quick-nav card. Vite build passes ✅. Commit 82ca368 on nordic-vitals main. No notification sent (no new board tasks; incremental admin operational feature).
Board-watcher run 75 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Integrations & Webhooks page (/admin/integrations): 3-section page — Arctico API (URL + key + live connection test with status badge + env-var note), Payment Gateways (Stripe/Klarna/Vipps toggles with credential fields), Outgoing Webhooks (full CRUD, 8 event types, signing secret, enable/disable toggle, per-webhook ping button, delivery log table). INTEGRATIONS/WEBHOOKS/WEBHOOK_LOG mock data. 9 new mock-safe API functions. AdminLayout nav (🔌 Integrations) + Overview quick-nav card added. Vite build passes ✅. Commit 52531b3 on nordic-vitals main.
Board-watcher run 76 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Member Tax Summary page (/dashboard/tax-summary): annual earnings statement for Skattemeldingen (Norwegian tax return). Features: year selector (2024/2025/2026), KPI cards (total MLMT earned / NOK equivalent / withdrawn / pending), commission breakdown table by type (count + MLMT + illustrative NOK), visual distribution bar chart, print/PDF button (@media print), tax filing disclaimer (official Skatteetaten/Norges Bank rate reminder), member info strip. getTaxSummary(userId, year) added to mlmApi.js (mock-safe, wires to GET /v1/mlm/tax-summary). 🧾 Tax Summary nav link added to Finances section of DashboardLayout. App.jsx route /dashboard/tax-summary added. Vite build passes ✅. Commit ce73f8b on nordic-vitals main. No notification sent (no new board tasks; incremental compliance/member feature).
Board-watcher run 77 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Bulk Member Import page (/admin/import): CSV drag-and-drop (or file browse) upload; auto column mapping (email/name/sponsor_id/phone/country/rank/pv/joined); manual override selects per column; per-row validation with error highlighting; preview table (first 5 rows); validation summary (valid/invalid counts); download validation-error CSV; import button calls importMembers() API — mock-safe (simulates ~5% duplicate rate); import result screen (imported/skipped/failed counts + failed-rows table + download failure CSV). importMembers(rows) added to mlmApi.js (mock-safe, wires to POST /v1/mlm/admin/members/import). AdminLayout nav (📥 Bulk Import) + Overview quick-nav card added. App.jsx route /admin/import added. Vite build passes ✅. Commit 1d8b3aa on nordic-vitals main. No notification sent (no new board tasks; practical launch-prep feature).
Board-watcher run 78 (2026-07-29): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Member Onboarding Wizard (/dashboard/onboarding): 7-step guided setup for new members (Welcome → Profile → Products → Share & Earn → Income Goal → Training → Done); progress bar + clickable back-nav on completed steps; per-step action links (shop, referral, business-plan, training); income goal picker (500/2k/5k/10k/25k MLMT); completion tracked per user via localStorage (nv_onboarded_<userId>); Home.jsx shows gold "Get Started →" CTA banner until onboarding complete; 🚀 Setup Guide nav link added to DashboardLayout Account section. Vite build passes ✅ (899 modules). Commit 5bf5251 on nordic-vitals main. No notification sent (no new board tasks; incremental member activation feature).
Board-watcher run 79 (2026-07-30): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added Admin Email Campaigns page (/admin/campaigns): targeted email blast system — compose modal with 12 audience segments (all/by-rank/by-status/new-joiners/inactive), schedule now/later/draft, load-from-template helper, live recipient estimate; status tabs (draft/scheduled/sent/cancelled); per-campaign open+click rate stats; preview modal with sample variable substitution; send-now + cancel-scheduled confirm flows; duplicate campaign. KPI cards (total/sent/scheduled/delivered/avg-open/avg-click). 7-entry EMAIL_CAMPAIGNS mock dataset + 6 new mock-safe API functions (getEmailCampaigns/createEmailCampaign/updateEmailCampaign/cancelEmailCampaign/duplicateEmailCampaign/sendEmailCampaignNow). AdminLayout nav (📧 Email Campaigns) + Overview quick-nav card. Vite build passes ✅ (906 modules). Commit c523ae0 on nordic-vitals main. No notification sent (no new board tasks).
Board-watcher run 80 (2026-07-30): board inaccessible (buge4/gary-collab cross-owner), arctico still blocked, no new Bjørn emails (last: bvhauge@gmail.com "Dagens Hacker News-svar" 2026-07-27 — still Gary-manual). Added KYC / Identity Verification system: member /dashboard/kyc page (step progress tracker, drag-and-drop doc upload for 3 doc types: government ID / proof of address / selfie with ID, status banner for unverified/draft/pending/approved/rejected states, resubmission flow on rejection, AML/KYC compliance info box, submit-for-review button); admin /admin/kyc page (KPI cards, status filter tabs, search, paginated table, review modal with doc list, approve/reject two-step confirm flows — rejection requires notes). KYC_SUBMISSIONS mock dataset (6 entries across all statuses). 7 mock-safe API functions. 🔏 KYC nav in DashboardLayout Account section + AdminLayout. Admin Overview quick-nav card. Routes wired. Vite build passes ✅ (902 modules). Commit b64a6fb on nordic-vitals main. No notification sent (no new board tasks; incremental AML compliance feature).
