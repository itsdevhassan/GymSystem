# Decisions Log

Oct 17, 2026 · @Aman

Technical Document section 13 ke sawalon ke jawab. Naya faisla ho to yahan add karein (tareekh ke saath).

## Environment

| # | Faisla |
| --- | --- |
| E1 | Development doosre laptop par hogi (jahan Flutter, Docker wagera installed hain). |
| E2 | Project OneDrive se bahar rahega: `C:\dev\Gym-System` (code ka backup GitHub par). |
| E3 | Team: sirf Aman + Claude. Tasks ek ke baad ek, sequence mein. |
| E4 | Platform naam/domain abhi tay nahi — placeholder `gymsystem.pk`, sab config/env se badla ja sakega. |

## Technical (T1–T26) — sab defaults qabool

| # | Faisla |
| --- | --- |
| T1/T2 | Backend aur Frontend TypeScript |
| T3 | Prisma |
| T4 | Redis haan |
| T5 | VPS + Docker, Pakistan ke qareeb region, DB alag server (provider baad mein) |
| T6 | S3-compatible storage (local par MinIO) |
| T7 | Domain baad mein (E4) |
| T8 | Ek monorepo, GitHub |
| T9 | Teen alag React apps + shared packages (pnpm) |
| T10 | Teen alag Flutter apps + shared packages (Melos) |
| T11 | Tailwind + shadcn/ui |
| T12 | Drift |
| T13 | Ek Firebase project; web push Phase 2 |
| T14 | WhatsApp/SMS adapter interface; provider baad mein |
| T15 | PDFKit |
| T16 | Bluetooth ESC/POS 80mm |
| T17 | ZKTeco (Phase 3) |
| T18 | BIGINT auto-increment |
| T19 | P&L cash basis |
| T20 | SSL pinning / root detection Phase 3 |
| T21 | Sentry haan |
| T22 | GitHub Actions; mobile builds pehle manual |
| T23 | Android 8 (API 26), iOS 13 |
| T24 | Urdu UI mein bhi English digits |
| T25 | Sirf Aman (E3) |
| T26 | Har module: Backend → Web → Mobile, phir agla module |

## Feature (F1–F29) — sab defaults qabool

| # | Faisla |
| --- | --- |
| F1 | Phase 1 = Feature Doc 13.2 ka Phase 1 |
| F2 | Plans: Starter / Growth / Pro; prices baad mein (Super Admin se set) |
| F3 | Trial 14 din, Growth plan ke modules |
| F4 | Gym subscription fee manual; Super Admin record kare |
| F5 | Phase 1 mein sirf Direct Transfer + Proof; merchant Phase 3 |
| F6 | Verify: accountant + owner; receptionist optional permission |
| F7 | Voucher owner setting; default joining date par |
| F8 | Late fee owner setting; default band |
| F9 | Fee due par default warning (block owner setting se) |
| F10 | Grace 3 din |
| F11 | Discount limits: receptionist 5%, manager 15% |
| F12 | Sales tax sirf % option, default band |
| F13 | Member gym selection: Gym Code + QR + SMS invite (teeno) |
| F14 | Member app store order Phase 2 |
| F15 | Member–member chat band; ladies rule owner setting |
| F16 | Chat history 1 saal |
| F17 | Staff kai gyms mein (ek login, gym select) |
| F18 | Weighted average cost |
| F19 | Negative stock nahi (owner setting) |
| F20 | Purchase cost: owner, accountant, store manager |
| F21 | Salary 1–5 tareekh |
| F22 | Commission rules owner set kare, default koi nahi |
| F23 | Offline Phase 2 |
| F24 | Phase 1 reports: collection, defaulters, attendance, expenses, members |
| F25 | Daily summary Phase 1 app notification, WhatsApp Phase 2 |
| F26 | View as gym: har dafa ijazat, 1 ghanta |
| F27 | Import: Excel/CSV template |
| F28 | Logo/theme sirf owner; support view-only |
| F29 | Ramadan/holiday din auto barhana owner setting, default nahi |
| F30 | Koi extra feature abhi nahi (aage aaye to yahan add) |

## UI

| # | Faisla |
| --- | --- |
| U1 | Style: **Modern minimal** (Stripe/Linear jaisa) — zyada safed jagah, halki borders, soft shadows, saaf typography; gym ka theme color upar se lagega |
