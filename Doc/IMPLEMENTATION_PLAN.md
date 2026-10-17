# Gym Management System (Pakistan) — Implementation Plan

Oct 17, 2026 · Status: **v1.0 — approval ke liye** (faisle `DECISIONS.md` mein)

Yeh plan dono documents (Complete Feature Document + Technical Document) parh kar banaya gaya hai. Har task ka format Technical Doc section 12.1 jaisa hai. Estimates ek developer (Aman + Claude) ke hisab se rough hain.

### Development laptop par pehle yeh (Prerequisites)

| Cheez | Version | Check command |
| --- | --- | --- |
| Node.js | LTS (22/24) | `node -v` |
| pnpm | latest | `pnpm -v` (na ho to `npm i -g pnpm`) |
| Git + GitHub account | latest | `git --version` |
| Docker Desktop | latest | `docker compose version` |
| Flutter (stable) + Android Studio / emulator | latest stable | `flutter doctor` |
| Melos | latest | `dart pub global activate melos` |
| VS Code (ESLint, Prettier, Tailwind, Prisma, Flutter extensions) | — | — |

Project folder: `C:\dev\Gym-System` (OneDrive se bahar). `Doc/` folder wahan copy kar lein.

---

## 0. Kaam karne ka tareeqa (Working agreement)

1. **Ek waqt mein ek task.** Har task ke aakhir mein: kya bana, kaise test karein, agla task kya hai — aap ko report.
2. **Har module ka order hamesha same:** `DB migration + seed → Backend API + tests → Web screens → Mobile screens → E2E check`.
3. **Har module "Definition of Done"** (section 9) poora hone par hi agla module.
4. **Koi farq ya confusion** dono documents mein ho to khud faisla nahi — aap se poochunga.
5. **UI quality gate:** har nayi screen par loading / empty / error states, dark mode, Urdu RTL, mobile width (360px) — sab check kar ke hi "done".
6. **Phase ke aakhir mein** demo + aap ka approval, phir agla phase.

---

## 1. Kaam ka bara naqsha (Roadmap)

| Phase | Kya banega | Nateeja |
| --- | --- | --- |
| **Phase 0 — Foundation** | Repo, Docker, Database core, Backend core (auth, tenant, RBAC, modules), Design system, Web core, Mobile core, CI | Teeno realms ka login chal raha ho, khaali dashboards dynamic menu ke saath, theme engine kaam kare |
| **Phase 1 — MVP** | Super Admin, Gym Setup, Theme, Roles & Permissions, Module ON/OFF, Dashboards, Members, Plans, Leads/Admission, Attendance, Fees & Vouchers, Payments, Cash shifts, Expenses, basic Reports, Notifications, Member app, Data import | Ek asli gym poora mahina chala sake |
| **Phase 2 — Business** | Inventory, Purchasing, POS, P&L, Staff/HR, Payroll, Chat, PT, Classes, Workout, Diet, Progress, Lockers, Offline, WhatsApp/SMS | Poora karobari hisab + member experience |
| **Phase 3 — Scale** | Biometric, Multi-branch advanced, Equipment, Complaints, Documents/signature, Loyalty, advanced reports, merchant payments | Bare gyms aur chains |

---

## 2. Phase 0 — Foundation (sab se pehle yeh)

> Is phase mein koi business feature nahi; sirf woh bunyaad jis par har module khara hoga. Yeh sahi bani to baqi modules tez aur ek jaise banenge.

### 2.1 Repo aur Environment

| Task ID | Title | Depends on | Files | Acceptance criteria | Estimate |
| --- | --- | --- | --- | --- | --- |
| P0-ENV-01 | Root setup: git init, `.gitignore`, `.editorconfig`, root `README.md`, `.env.example` policy | — | `/` | Repo bana, secrets ignore, README mein setup steps | 0.5 din |
| P0-ENV-02 | `docker-compose.yml`: MySQL 8 (utf8mb4), Redis, MinIO, Mailpit (dev) | ENV-01 | `/docker-compose.yml` | `docker compose up` se sab services healthy | 0.5 din |
| P0-ENV-03 | Database folder: `erd/`, `schema/`, `seeds/`, `scripts/` + README | ENV-01 | `Database/*` | Structure ready, backup/restore script skeleton | 0.5 din |

### 2.2 Database core

| Task ID | Title | Depends on | Files | Acceptance criteria | Estimate |
| --- | --- | --- | --- | --- | --- |
| P0-DB-01 | Prisma setup + conventions (BIGINT ids, DECIMAL money, DATETIME(3), soft delete) | ENV-02 | `Backend/prisma/schema.prisma` | Pehli migration MySQL par chal jaye | 0.5 din |
| P0-DB-02 | Platform + Gym core + Identity tables (Tech Doc 4.6) | DB-01 | `schema.prisma`, migration | Tamam tables, FKs, indexes, uniques `(gym_id, ...)` | 1 din |
| P0-DB-03 | Seeds: modules (+depends_on), permissions (`module.action`), system roles + default matrix (Feature Doc 4.4), cities | DB-02 | `Database/seeds/*`, `prisma/seed.ts` | `pnpm db:seed` se poora master data | 1 din |
| P0-DB-04 | `Database/schema/*.sql` reference + ERD (mermaid) | DB-02 | `Database/schema`, `Database/erd` | Migration se match | 0.5 din |

### 2.3 Backend core (`Backend/src/core`)

| Task ID | Title | Depends on | Acceptance criteria | Estimate |
| --- | --- | --- | --- | --- |
| P0-BE-01 | Project setup: TS, ESLint, Prettier, Jest, `config/env.ts` (Zod validate), Pino logger, `server.ts`/`socket.ts`/`worker.ts` entries | DB-01 | Ghalat env par app start na ho; `/health` chale | 1 din |
| P0-BE-02 | HTTP layer: response envelope, `AppError` family, errorHandler, asyncHandler, pagination/sort/filter helpers, requestId, Helmet/CORS/hpp/rate-limit | BE-01 | Section 5.2 ka format har jagah | 1 din |
| P0-BE-03 | Utils: money (`decimal.js`), date (Asia/Karachi), phone (+92 normalize), sequences (`FOR UPDATE`), AES-256-GCM encryption | BE-01 | Unit tests pass | 1 din |
| P0-BE-04 | Auth — 3 realms: platform (password + OTP 2FA), gym (phone + password/OTP), member (gymCode + phone + OTP); JWT access 15m, refresh rotation + family revoke, `token_version`, OTP Redis (5 min, 5 tries block) | BE-02, DB-02 | Login/refresh/logout teeno realms; reuse detection test; rate-limit test | 3 din |
| P0-BE-05 | Tenant context + BaseRepository (`gym_id` auto scope) + `X-Branch-Id` check | BE-04 | Cross-tenant test: Gym A token Gym B ka record 404 | 1.5 din |
| P0-BE-06 | RBAC: `requirePermission`, scope (all/branch/own), role limits, Redis permission cache, field-masking mapper helper | BE-05 | Permission denied tests | 2 din |
| P0-BE-07 | Module engine: effective modules (plan ∧ gym ∧ branch ∧ deps), `requireModule` → `403 MODULE_DISABLED` | BE-06 | Band module ka API 403 | 1 din |
| P0-BE-08 | Bootstrap APIs: `/gym/me/bootstrap`, `/member/me/bootstrap`, `/platform/me/bootstrap` | BE-07 | Response Tech Doc 3.6 jaisa | 1 din |
| P0-BE-09 | Audit log service, Files module (`POST /files`, magic-byte check, MinIO, signed URLs 5 min), Idempotency middleware | BE-05 | Upload/download test, duplicate request same response | 2 din |
| P0-BE-10 | Event bus + BullMQ queues skeleton + Bull Board (admin only) | BE-01 | Sample job chale, retry/backoff | 1 din |
| P0-BE-11 | Socket.IO skeleton: JWT handshake, `/live` + `/chat` namespaces, rooms, Redis adapter | BE-04 | Connect/reject test, room join | 1 din |
| P0-BE-12 | NotificationService (in-app + socket + FCM adapter, quiet hours) + `POST /auth/devices` | BE-10, BE-11 | Test notification teeno channels se | 1.5 din |
| P0-BE-13 | OpenAPI (Zod se generate) `/api/docs` dev mein | BE-02 | Docs khulein | 0.5 din |

### 2.4 Design System (UI ki bunyaad — clean aur smooth UI yahin se aayegi)

> UI ko "clean, smooth aur professional" banane ke liye **design pehle, screens baad mein.** Ek hi design language teeno web apps aur teeno mobile apps mein.

| Task ID | Title | Acceptance criteria | Estimate |
| --- | --- | --- | --- |
| P0-DS-01 | Design tokens: colors (primary/secondary + semantic success/warning/danger/info), neutral scale, spacing 4/8/12/16/24/32, radius, shadows, typography scale (Inter + Noto Nastaliq Urdu), motion (durations/easing) | Ek `tokens` source jis se web CSS variables aur Flutter `AppTheme` dono banen | 1 din |
| P0-DS-02 | Theme engine: gym primary/secondary se light/dark shades auto, contrast (AA) check, 8–10 presets (Classic Blue, Dark Iron, Gold, Ladies Rose, Green Fit...) | Theme badalte hi poori app bina reload badle | 1 din |
| P0-DS-03 | UI review page (web: `/dev/ui-kit`, mobile: UI Kit screen) — har component har state mein, light/dark, Urdu/English | Aap ek jagah poora design dekh kar approve kar sakein | 0.5 din |

**UI principles (har screen par lagu):**
- Saaf whitespace, ek screen par ek primary action, clear hierarchy (title → filters → content).
- Smooth: skeleton loaders (spinner kam), 150–250ms transitions, optimistic updates, page-level code splitting, no layout jump.
- Har list: search + filters + pagination + empty state with action.
- Har form: inline validation, required `*`, save par loading button, kamyabi par toast, danger action par ConfirmDialog.
- Status hamesha rang + text dono se (green active, red expired, amber due) — sirf rang par bharosa nahi.
- Touch targets ≥ 40px web / 48px mobile; keyboard shortcuts reception/POS par.
- Responsive 360px → 1920px; tables mobile par cards.

### 2.5 Web core (`Frontend/`)

| Task ID | Title | Depends on | Acceptance criteria | Estimate |
| --- | --- | --- | --- | --- |
| P0-FE-01 | pnpm monorepo: `apps/super-admin`, `apps/gym-portal`, `apps/member-portal`, `packages/config` (eslint/tsconfig/tailwind preset) | ENV-01 | Teeno apps `pnpm dev` se chalein | 0.5 din |
| P0-FE-02 | `packages/ui` part 1: Button, IconButton, Badge, Avatar, Tooltip, Spinner, Skeleton, Card, Modal, Drawer, DropdownMenu, Toast, Alert, ConfirmDialog, EmptyState, ErrorState | DS-01 | Dark + RTL + keyboard; tests | 2 din |
| P0-FE-03 | `packages/ui` part 2 (forms): TextField, PhoneField, CnicField, MoneyField, NumberField, Select, SearchSelect, DatePicker, DateRange, Switch, Checkbox, Radio, FileUpload, FormSection, FormActions | FE-02 | RHF + Zod ke saath integrate | 2 din |
| P0-FE-04 | `packages/ui` part 3 (data/layout): DataTable (server pagination, sort, column toggle, bulk select, mobile cards), FilterBar, SearchInput, StatCard, StatusBadge, PageHeader, Tabs, Sidebar, Topbar, BranchSwitcher, ThemeToggle, LanguageToggle, NotificationBell, chart cards | FE-03 | List page pattern ka demo | 2.5 din |
| P0-FE-05 | `packages/api-client` (Axios, refresh queue, error mapping, query keys), `packages/auth` (store, guards, `<Can/>`, `<RequirePermission/>`), `packages/i18n` (ur/en, RTL), `packages/utils`, `packages/realtime` | BE-08 | 401 par ek hi refresh; RTL switch | 2 din |
| P0-FE-06 | Layouts: AppLayout (collapsible sidebar + topbar), AuthLayout, ReceptionLayout; dynamic menu `bootstrap` se | FE-04, FE-05 | Menu permissions/modules ke hisab se | 1 din |
| P0-FE-07 | Login screens teeno apps: Super Admin (password + OTP), Gym (phone + password/OTP, gym/branch select), Member (gym code + OTP) | FE-06 | E2E login teeno | 1.5 din |

### 2.6 Mobile core (`MobileApp/`)

| Task ID | Title | Depends on | Acceptance criteria | Estimate |
| --- | --- | --- | --- | --- |
| P0-MB-01 | Melos monorepo: `apps/gym_app`, `apps/member_app`, `apps/super_admin_app`, `packages/core`, `packages/ui_kit`; flavors dev/staging/prod | ENV-01 | Teeno apps emulator par chalein | 1 din |
| P0-MB-02 | `core`: NetworkClient (Dio + Auth/Refresh/Error interceptors), SecureStorageService, AuthService, SessionService, ThemeService, ConnectivityService, BaseController/PaginatedController, middlewares, translations | BE-08 | Login → bootstrap → theme apply | 3 din |
| P0-MB-03 | `ui_kit`: AppTheme builder, buttons, inputs (phone/CNIC/money/OTP), cards, lists (paginated + shimmer), feedback (empty/error/snackbar/confirm), PermissionGate/ModuleGate | DS-01 | UI Kit screen par sab widgets | 3 din |
| P0-MB-04 | PushService (FCM, channels, deep link) + SocketService | MB-02, BE-12 | Test push tap par screen khule | 1 din |
| P0-MB-05 | Login flows teeno apps + dynamic bottom nav | MB-03 | Role ke hisab se tabs | 1.5 din |

### 2.7 CI

| Task ID | Title | Acceptance criteria | Estimate |
| --- | --- | --- | --- |
| P0-CI-01 | GitHub Actions: lint → typecheck → test → build (path filters: Backend / Frontend / MobileApp) | PR par sab green | 0.5 din |

**Phase 0 total (rough):** ~45–50 din-kaam.

---

## 3. Phase 1 — MVP (module-wise)

Har module ke andar 4 sub-tasks hote hain: **`-DB` (migration + permissions seed) → `-BE` (API + tests) → `-FE` (web screens) → `-MB` (mobile screens)**. Neeche har module ka scope aur ahem screens.

| # | Task ID prefix | Module | Ahem kaam | Web screens | Mobile screens |
| --- | --- | --- | --- | --- | --- |
| 1 | P1-SA | **Super Admin** | Gyms CRUD + status (trial/active/overdue/suspended/closed), KYC docs, subscription plans + plan_modules, gym module override, platform invoices + manual payment record, auto-suspend job, announcements, support tickets, platform users/roles, "View as gym" (owner ijazat + 1 ghanta + audit), platform dashboard | Dashboard, Gyms list/detail/create, Plans, Invoices, Tickets, Announcements, Users | Dashboard, Gyms, Tickets, Billing, More |
| 2 | P1-GS | **Gym Setup** | Setup wizard, gym profile, branches + timings (gents/ladies, Ramadan), holidays, number series, tax %, rules (grace, late fee, due day), T&C, footer | Wizard, Settings tabs | Wizard, Settings |
| 3 | P1-TH | **Theme & Branding** | Logo/icon upload, colors, presets, contrast warning, live preview (web/mobile/receipt/voucher), undo | Theme page + preview | Theme screen |
| 4 | P1-RP | **Roles & Permissions + Module ON/OFF** | Custom roles, permission matrix editor (scope + limits), staff multiple roles, owner module toggle (deps warning), branch modules, `permissions:changed` socket | Roles list, Permission matrix, Modules | Same (simplified) |
| 5 | P1-ST0 | **Staff basic** (login ke liye zaroori) | Staff add, roles/branches assign, login on/off, force logout | Staff list/form | Staff list |
| 6 | P1-DB | **Role-wise Dashboards** | `/gym/dashboard` widgets role ke hisab se, date/branch filter, clickable cards, owner widget toggle | Owner, Manager, Reception, Accountant dashboards | Same (cards stacked) |
| 7 | P1-MP | **Membership Plans** | Plan types (duration/visits/sessions/daily), admission fee, add-ons, access rules, gender, discounts/coupons, installments, pro-rata, grace, freeze rules, price versioning (`plan_snapshot`) | Plans list/form | Plans list |
| 8 | P1-MM | **Members** | Profile (CNIC encrypted), quick admission, photo (webcam/camera), health info (restricted), tags, status, freeze/unfreeze (approval), renew/upgrade, transfer, blacklist, timeline, notes/docs, duplicate check, bulk actions, search | List, Detail (tabs), Form, Freeze dialog | List, Detail, Admission wizard |
| 9 | P1-LD | **Enquiry/Leads & Admission** | Lead stages, follow-ups + overdue alerts, trial pass, lost reasons, convert to member (prefill), admission flow (profile → plan → fee → terms → photo → receipt) | Leads kanban/list, Admission wizard | Leads, call/WhatsApp buttons |
| 10 | P1-AT | **Attendance** | Dynamic QR (30 sec signed token), manual check-in, entry rules (block/warning), gender timing rules, photo pop-up (socket), live count + capacity, check-out, guest visits, absent alerts job | Reception screen (kiosk), Live, History | QR scanner, Manual check-in |
| 11 | P1-FV | **Fees & Vouchers** | Auto monthly vouchers job (idempotent), voucher items, late fee rules, status flow, defaulters + aging, fee counter screen, partial/advance/split, discount + approval, cancel/waive approval, voucher/receipt PDF (A4 3-copy + thermal), verify QR page, bulk generate/print | Vouchers, Fee counter, Defaulters | Fee collect (≤3 taps), Vouchers |
| 12 | P1-PY | **Payments** | Owner payment accounts (OTP, validation, encryption, alerts), member proof upload, TID duplicate check, verify/reject queue (socket + push), allocations, refunds + approval, wallet, 24h pending alert | Payment accounts, Verification queue, Payments list, Refunds | Verification (push se seedha approve) |
| 13 | P1-CS | **Cash shifts** | Open/close shift, expected vs counted, difference alert, approval | Cash shift screen | Cash shift |
| 14 | P1-EX | **Expenses** | Categories, entry + bill photo, vendors, recurring (job), petty cash, approval limits, budgets + alert | Expenses list/form, Categories, Recurring | Expense add (camera) |
| 15 | P1-NT | **Notifications** | Notification center, templates (ur/en/roman), auto reminders jobs (fee due, expiry, absent, birthday), owner daily summary (app) | Notification center, Templates | Notifications |
| 16 | P1-RE | **Basic Reports** | Collection (method/staff-wise), defaulters, attendance, expenses, members; Excel/PDF export (queue) | Reports hub | Reports (summary + PDF share) |
| 17 | P1-MA | **Member app + portal** | Home (membership card, din baqi), QR card, vouchers + pay + proof upload, receipts, attendance, profile, plans, renew/freeze request, notifications | Member portal | Member app |
| 18 | P1-IM | **Data Import** | Excel/CSV templates, upload → column map → errors → preview → commit → rollback | Import wizard | Upload only |
| 19 | P1-AU | **Audit log + anti-fraud alerts (basic)** | Audit viewer, recycle bin (30 din restore), fraud alerts (bara discount, cash kami, receipt cancel) | Audit log, Recycle bin | Alerts |
| 20 | P1-QA | **Phase 1 hardening** | E2E (login, admission, fee collect, payment verify), performance, security checklist (Tech Doc 11.1), staging deploy, 3–5 gyms pilot | — | — |

**Phase 1 order ki wajah:** Super Admin pehle (gym banane ke liye) → Setup/Theme/Roles (gym chalane ke liye) → Plans → Members → Leads → Attendance → Fees → Payments → Cash → Expenses → Notifications → Reports → Member app → Import.

---

## 4. Phase 2 — Business modules

| # | Prefix | Module | Note |
| --- | --- | --- | --- |
| 1 | P2-INV | Inventory | `InventoryService` (weighted avg, `FOR UPDATE`), batches/FEFO, stock counts, transfers, low stock/expiry jobs |
| 2 | P2-PUR | Purchasing | Suppliers + ledger, PO (+approval, PDF), GRN (landed cost), supplier payments, returns, price history |
| 3 | P2-POS | POS | Barcode-focused screen, cart, member price, discount limits, split payment, wallet/add-to-voucher, hold sale, returns, thermal receipt |
| 4 | P2-PL | Profit & Loss / Finance | P&L (cash basis), cash book, account-wise, receivables/payables, drawings, partners, month closing lock, daily summaries job |
| 5 | P2-HR | Staff & HR (full) | Profiles, shifts/roster, staff attendance, leaves, tasks, certificates |
| 6 | P2-PR | Payroll | Salary structures, run (attendance se), commission engine, advances, payslip PDF, acknowledge, auto expense |
| 7 | P2-CH | Chat | Socket chat (`message:send` ack, read receipts, typing, presence), support inbox + assign, voice notes, rules (ladies privacy), FCM fallback |
| 8 | P2-PT | Trainers & PT | Assignments, PT packages, calendar booking, OTP/QR confirm, rating |
| 9 | P2-CL | Classes | Timetable, booking + waitlist, attendance, challenges |
| 10 | P2-WO | Workout | Exercise library seed (ur/en), plan builder (drag-drop), assign, member log + rest timer, PDF |
| 11 | P2-DT | Diet | Pakistani foods seed, calorie calculator, builder (sehri/iftari), assign, food log |
| 12 | P2-PG | Progress | Measurements, photos (private), graphs, report PDF |
| 13 | P2-LK | Lockers & Facilities | Lockers, assignment (voucher mein fee), facility bookings |
| 14 | P2-OF | Offline mode | Drift cache + outbox, `/gym/sync`, offline receipt numbers, sync banner |
| 15 | P2-WA | WhatsApp/SMS | Provider adapters, credits, bulk messages, opt-out, delivery logs |

## 5. Phase 3 — Scale modules

Biometric (ZKTeco adapter), Multi-branch advanced (transfers, comparison), Equipment & maintenance, Complaints & feedback, Documents & signature (PAR-Q, minors), Loyalty & referral, Advanced/custom/scheduled reports, Merchant auto-confirm (provider adapter + webhook + reconciliation), SSL pinning / root detection, Web reception PWA offline.

---

## 6. Ek module kaise banega (template — misal: Members)

```
1. DB      : Prisma models + migration, permissions seed (members.view/create/edit/delete/freeze/...)
2. Backend : modules/members/{routes, controller, service, repository, schemas, mapper, types, events}.ts
             + tests: happy path, validation, permission denied, cross-tenant, module disabled
3. Web     : features/members/{pages, components, api, schemas.ts, types.ts}
             List (PageHeader → FilterBar → DataTable → Drawer) · Detail (tabs) · Form
4. Mobile  : modules/members/{bindings, controllers, views, widgets, data/{models, services, repositories}}
5. Check   : Urdu RTL, dark mode, 360px, loading/empty/error, audit entries, OpenAPI updated
```

---

## 7. Testing aur Quality

- Backend: Jest unit (paisa, stock, payroll, permissions) 80%+; Supertest integration har endpoint.
- Har module mein **cross-tenant** aur **permission denied** test lazmi.
- Web: Vitest + RTL (components), Playwright E2E (ahem flows).
- Mobile: controller/repository unit, widget tests, ek integration flow.
- CI green ke baghair merge nahi.

## 8. Risks aur note

| Risk | Hal |
| --- | --- |
| OneDrive sync `node_modules`/build folders ko lock kar deta hai | Faisla E2: project `C:\dev\Gym-System` mein |
| Platform naam/domain abhi tay nahi | Sab URLs/naam env + config se; baad mein ek jagah badlega |
| WhatsApp/SMS provider, hosting provider abhi tay nahi | Adapter interface; Phase 2 / deploy se pehle faisla |
| Scope bohot bara (34 modules × 6 apps) | Phase-wise, har phase ke baad pilot gyms se feedback |
| Payments par fraud | Approval, audit, TID unique, owner alerts — Phase 1 se |
| Urdu Nastaliq rendering (PDF + mobile) | Font embed + early test (Phase 0 DS-01) |

## 9. Definition of Done (har module)

- [ ] Migration + permissions seed
- [ ] API + Zod + permission/module checks
- [ ] Unit + integration tests (cross-tenant + permission denied)
- [ ] OpenAPI update
- [ ] Web: list/detail/form, loading/empty/error, ur + en, dark, responsive
- [ ] Mobile: same states, RTL, dark
- [ ] Dashboard widgets (agar ho)
- [ ] Notifications/socket events (agar ho)
- [ ] Audit logging
- [ ] README/CHANGELOG
