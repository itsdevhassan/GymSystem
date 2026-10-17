# Gym Management System (Pakistan) — Complete Feature Document

Oct 17, 2026 · @Aman

## 1. Khulasa aur Scope

Yeh **sirf Pakistan ke gyms** ke liye ek multi-gym SaaS hai. Ek hi platform par sainkron gyms chalenge, lekin har gym ka data alag hoga. Har gym owner ko ek jagah sab kuch milega: members, fees, attendance, staff, salary, inventory, purchasing, product sales, monthly expenses, profit/loss, chat aur reports. Member ki online payment owner ke apne JazzCash / Easypaisa / NayaPay / bank account mein jayegi.

Yeh **functional (feature) document** hai, technical nahi. Is mein har module ka overview, features, kaun kya karega (roles), aur web/mobile par kya milega likha hai. Database, API aur tech stack baad wale technical document mein aayenge.

### 1.1 Kya banega (In scope)

| # | Cheez | Note |
| --- | --- | --- |
| 1 | Super Admin portal (web) + Super Admin mobile app (alag app) | Aap aur aapki team ke liye |
| 2 | Gym web portal + Gym mobile app (owner aur staff) | Har module web aur mobile dono par |
| 3 | Member mobile app + member web portal | Har module web aur mobile dono par |
| 4 | Role-based access, har role ka alag dashboard | Staff ko sirf woh module nazar aaye jis ki permission ho |
| 5 | Module ON/OFF | Super Admin aur owner level par |
| 6 | Theme & branding | Har gym ka logo aur colors |
| 7 | Chat system | Member, trainer, staff, owner, Super Admin support |
| 8 | Payments | Owner apne JazzCash, Easypaisa, NayaPay, bank/Raast details daale; paisa seedha owner ko |
| 9 | Inventory, purchasing, product sales (POS) | Purchase cost, selling price, profit per product |
| 10 | Monthly expenses aur Profit & Loss | Rent, bijli, salaries, sab hisab |
| 11 | Fee vouchers / slips, receipts, salary slips | School jaisa slip system |
| 12 | Urdu + English, PKR, Pakistani timings (gents/ladies, Ramadan) | Pakistan ke mutabiq |

### 1.2 Kya nahi banega (Out of scope)

- **Global / doosre mulk:** sirf Pakistan; currency sirf PKR, phone sirf +92.
- **Subdomain nahi:** har gym ka alag URL (gymname.platform.pk) nahi hoga. Sab ek hi website aur ek hi app se login karenge; system login ke baad khud user ka gym khol dega.
- **Gym ka landing page / public website nahi:** platform har gym ka public page ya online joining website nahi banayega.
- **Custom domain aur white-label app store listing nahi** (abhi ke liye).

### 1.3 Ek line mein product

> "Gym owner ko register, Excel, alag POS software aur WhatsApp groups ki zaroorat nahi — sab kuch is ek app mein, aur paisa seedha uske apne account mein."

### 1.4 Pakistan ke gyms ke masail jo yeh system hal karega

| Masla | Hal |
| --- | --- |
| Fees register mein, bhool jati hain, hisab nahi milta | Auto fee vouchers, reminders, defaulter list |
| Members gayab ho jate hain | Absent alerts, expiry reminders |
| Owner nahi hota to staff cash mein gadbad karta hai | Har entry logged, approvals, shift cash closing, alerts |
| Supplements ka stock aur munafa pata nahi chalta | Inventory + purchasing + POS + product-wise profit |
| Mahine ka asal munafa pata nahi | Monthly expenses + Profit & Loss report |
| Trainer commission par jhagra | Auto commission calculation |
| Ladies/gents alag timings | Gender-wise slots aur plans |
| Biometric machine aur software alag | ZKTeco wagera integration |
| Light chali jaye to kaam ruk jaye | Offline mode |
| Member se baat WhatsApp groups mein bikhri hui | App ke andar chat |

## 2. Apps aur Portals

System mein **teen hisse** hain: Super Admin, Gym, aur Member. Har hissa **web aur mobile dono** par chalega. Har module jo web par hai, wohi mobile app mein bhi hoga. Bara data-entry wala kaam (bulk import, lambi reports) web par asaan rahega, lekin mobile par bhi available hoga.

| # | App / Portal | Kaun use karega | Platform |
| --- | --- | --- | --- |
| 1 | **Super Admin Web Portal** | Aap (platform owner), aapki support/sales/finance team | Web |
| 2 | **Super Admin Mobile App** (alag app) | Aap aur aapki team | Android + iOS |
| 3 | **Gym Web Portal** | Gym owner, partner, manager, receptionist, accountant, store manager, trainers | Web (desktop + tablet) |
| 4 | **Gym Mobile App** (owner + staff) | Wohi gym users; role ke hisab se screens | Android + iOS |
| 5 | **Member Mobile App** | Gym ke members | Android + iOS |
| 6 | **Member Web Portal** | Members (jo laptop par dekhna chahein) | Web |

### 2.1 Login ka tareeqa (subdomain ke baghair)

- Sab ke liye **ek hi website** (misal `app.platform.pk`) aur ek hi set of apps.
- **Gym staff:** phone number + password (ya OTP). Login ke baad system user ka gym aur role khud pehchan kar wohi gym khol deta hai.
- Agar ek staff do gyms/branches mein kaam karta ho to login ke baad **"Gym/Branch select karein"** screen aati hai.
- **Member:** phone number + OTP. Pehli dafa gym ka **Gym Code** daalna ya gym ka QR scan karna hota hai (taake member ghalat gym mein na chala jaye). Uske baad app hamesha usi gym mein khulti hai.
- **Super Admin:** alag login screen, phone/email + password + OTP (2-step) lazmi.
- Super Admin aur gym ke users ki login bilkul alag hai. Gym ka koi user Super Admin portal mein login nahi kar sakta.

### 2.2 Har module web + mobile par

| Module ki qisam | Web par | Mobile par |
| --- | --- | --- |
| Daily kaam (admission, fee, check-in, POS sale, expense entry) | Poora | Poora |
| Approvals (discount, refund, expense, leave) | Poora | Poora (notification se seedha) |
| Reports aur dashboards | Poora, bari tables + Excel/PDF | Poora, mobile-friendly charts + PDF/WhatsApp share |
| Settings (modules, roles, theme, payment accounts) | Poora | Poora |
| Bulk kaam (Excel import, bulk vouchers print) | Poora | Upload/generate mumkin, print web se behtar |
| Chat | Poora | Poora (push notifications ke saath) |

### 2.3 Gym Mobile App: ek app, alag roles

Owner, manager, receptionist, trainer, accountant aur store manager sab **ek hi Gym App** use karte hain. Login ke baad har role ko **apna dashboard** aur **sirf apne permitted modules** nazar aate hain (section 4 aur 5). Is se do alag apps maintain nahi karni partin.

### 2.4 Super Admin App alag kyun

Super Admin ka data (saare gyms, platform ki kamai, support tickets) bohot sensitive hai. Is liye uski app **alag** hai. Yeh gym apps ke saath kabhi mix nahi hoti, aur is mein extra security (OTP, device lock) hai.

## 3. Roles aur Har Role ka Kaam

System mein do qisam ke users hain: **Platform users** (Super Admin side) aur **Gym users** (har gym ke apne). Neeche har role ka kaam likha hai: woh kya karta hai aur kya nahi kar sakta. Yeh default roles hain. Owner inki permissions badal sakta hai ya naya custom role bana sakta hai (section 4).

### 3.1 Platform Roles (Super Admin side)

**1. Super Admin (Platform Owner — aap)**

- Naye gyms register/approve karna, gym ko active, suspend ya band karna.
- Subscription plans banana (Starter/Growth/Pro), price PKR mein, limits (members, branches, staff).
- Har gym ke liye modules ON/OFF karna.
- Gyms ki monthly subscription billing, payment record, overdue gyms ko reminder/suspend.
- Platform support staff ke accounts banana aur unki permissions.
- Saare gyms ko announcement/notification bhejna.
- Support tickets aur gym owners se chat.
- Platform reports: total gyms, active members, monthly kamai, kaunse gyms inactive hain.
- Audit log: platform par kisne kya kiya.
- **Nahi kar sakta:** gym ke member ka paisa receive nahi karta; owner ke payment accounts ki secret details nahi dekh sakta.

**2. Platform Support Agent**

- Gym owners ke tickets aur chat ka jawab, setup mein madad.
- Owner ki ijazat se gym account dekhna ("View as gym"), har dafa log ke saath aur time limit ke andar.
- **Nahi kar sakta:** gym delete karna, plans/prices badalna, billing.

**3. Platform Sales Agent**

- Naye gyms ki leads, demo, trial account banana, trial ko paid mein convert karna.
- Apni laai hui gyms aur commission dekhna.
- **Nahi kar sakta:** gym ka andar ka data dekhna.

**4. Platform Accountant**

- Gyms ki subscription invoices, payments, dues, platform ki kamai ki reports.
- **Nahi kar sakta:** modules/plans badalna, gym ka andar ka data.

### 3.2 Gym Roles

**5. Gym Owner**

- Gym ka sab kuch dekh aur badal sakta hai: saari branches, saare modules, saari reports.
- Gym setup: profile, branches, timings, theme/logo, receipt/voucher format.
- **Payment accounts add karna** (JazzCash, Easypaisa, NayaPay, bank, Raast). Yeh kaam sirf owner kar sakta hai, OTP ke saath.
- Staff add karna, roles dena, custom roles aur permissions banana.
- Apne plan ke modules mein se ON/OFF karna.
- Approvals: bara discount, refund, fee maafi, bara expense, salary, stock adjustment.
- Profit & Loss, cash flow, product profit, staff performance dekhna.
- Audit log: kis staff ne kya kiya.

**6. Partner / Co-Owner**

- Owner jaisa access, lekin owner tay karta hai kya kya (misal sirf reports + finance, ya sab kuch).
- Partner profit share report.
- **Default mein nahi kar sakta:** payment accounts badalna, owner ko hatana.

**7. Branch Manager**

- Sirf apni branch: members, fees, attendance, staff duty, expenses, stock, reports.
- Approvals apni limit tak (misal 10% discount tak); us se zyada owner ke paas.
- Daily cash closing check karna, staff ki attendance aur leave approve karna.
- **Nahi kar sakta:** doosri branch ka data, payment accounts, roles/permissions (agar owner na de).

**8. Receptionist / Front Desk**

- Naya member admit karna, enquiry likhna, trial dena.
- Fee collect karna (cash/online confirm), receipt print/WhatsApp, voucher dena.
- Check-in (QR scan, manual), guest entry.
- Membership renew/freeze ki request.
- Apni shift ka cash closing.
- Members se chat (support inbox).
- **Nahi kar sakta:** receipt delete/edit, apni limit se zyada discount, reports ka profit hissa, expenses, staff salary.

**9. Accountant / Cashier**

- Fees aur online payments verify karna (screenshot/TID match).
- Monthly expenses darj karna, vendor/supplier payments.
- Salary run, salary slips.
- Cash book, bank/wallet hisab, Profit & Loss reports.
- **Nahi kar sakta:** members ki medical info, workout/diet, roles badalna.

**10. Store / Inventory Manager**

- Products ka record (supplements, drinks, gym wear), purchase cost aur selling price.
- Suppliers, purchase orders, maal receive karna (stock in).
- POS par product bechna, stock count, damage/expiry entries.
- Low stock aur expiry alerts.
- Product sales aur profit report (agar owner ijazat de).
- **Nahi kar sakta:** membership fees, staff salary, gym ki overall P&L (default).

**11. Sales / Membership Consultant**

- Enquiries/leads, follow-up calls, trial book karna, lead ko member banana.
- Apne targets aur incentive dekhna.
- **Nahi kar sakta:** fees collect (agar owner na de), reports, expenses.

**12. Trainer (Gym Instructor)**

- Apne assigned members ki list, unki attendance aur progress.
- Workout aur diet plan banana/assign karna.
- Body measurements darj karna.
- Apne members se chat.
- Apni attendance, leave request, salary slip aur commission dekhna.
- **Nahi kar sakta:** doosre trainer ke members, fees/finance, members ka phone number (owner chahe to mask).

**13. Personal Trainer (PT)**

- Trainer ke saare kaam + PT clients, session booking, session complete (member OTP/QR se confirm).
- PT commission dekhna.

**14. Nutritionist / Dietitian (optional)**

- Diet plans, consultations, members se chat. Finance ka koi access nahi.

**15. HR / Admin Officer (optional)**

- Staff profiles, documents, shifts, staff attendance, leaves, salary preparation.
- **Nahi kar sakta:** members ka finance, product profit.

**16. Maintenance / Cleaning Staff (limited)**

- Equipment kharabi ke tickets, cleaning checklist tick karna, apni attendance. Mobile app par sirf yahi.

### 3.3 Member side

**17. Member**

- Apna profile, membership status, din baqi, fee vouchers aur receipts.
- Online fee payment (owner ke JazzCash/Easypaisa/NayaPay/bank par) aur screenshot/TID upload.
- Digital member card / QR se check-in.
- Workout aur diet plan, progress, attendance history.
- Class/PT booking, trainer aur gym se chat, complaints/feedback.
- Freeze/renew/upgrade request, store ke products dekhna/order (agar module ON ho).

**18. Guest / Trial Member**

- Trial ke dinon mein limited app access: QR entry, gym timings, plans dekhna, chat with reception.

## 4. Permission System aur Module ON/OFF

Kisi staff ko koi module tabhi nazar aata hai jab **teen sharten** poori hon: (1) woh module gym ke plan mein ho aur ON ho, (2) staff ke role ko us module ki permission mili ho, aur (3) staff us branch ka ho. Ek bhi shart poori na ho to us module ka menu, dashboard widget, notification aur report web aur mobile dono se ghayab ho jata hai.

### 4.1 Module ON/OFF teen levels par

| Level | Kaun karta hai | Misal |
| --- | --- | --- |
| Plan level | Super Admin | Starter plan mein POS nahi hai, to gym ko POS milega hi nahi |
| Gym level | Gym Owner (plan ke andar) | Owner ne Diet module band kar diya kyunki uske paas nutritionist nahi |
| Branch level | Gym Owner | Sauna/locker module sirf us branch mein ON jahan sauna hai |

**Module dependencies:** kuch modules doosre ke baghair nahi chalte. System khud batata hai, misal:

- POS / Product Sales ke liye **Inventory** ON hona zaroori.
- Purchasing ke liye **Inventory** zaroori.
- Payroll ke liye **Staff/HR** zaroori.
- PT ke liye **Trainers** zaroori.
- Profit & Loss ke liye **Expenses** zaroori (aur Inventory ON ho to product profit bhi shamil).

**Module band karne par data delete nahi hota.** Sirf chhup jata hai, aur dobara ON karne par sab wapas aa jata hai.

### 4.2 Permission ki qismein (har module ke andar)

| Permission | Matlab |
| --- | --- |
| View | Dekh sakta hai |
| Create | Naya bana sakta hai (member, sale, expense) |
| Edit | Badal sakta hai |
| Delete | Mita sakta hai (sirf recycle bin mein, owner wapas la sakta hai) |
| Approve | Doosre ki request approve kar sakta hai |
| Export / Print | Excel/PDF nikal ya print kar sakta hai |

**Data scope (kitna data):** sab branches / sirf apni branch / sirf apne records (misal trainer sirf apne members).

**Limits:** misal receptionist max 5% discount de sakta hai, manager 15%, us se zyada owner approve kare. Isi tarah refund aur expense ki limit.

**Field hide:** member ka poora phone number/CNIC chhupana (sirf aakhri 4 digit), product ki **purchase cost** staff se chhupana (sirf selling price dikhe), salary sirf owner/accountant ko.

**Time-based:** staff sirf apni shift ke waqt login kar sake (optional).

### 4.3 Custom roles

Owner naya role bana sakta hai (misal "Night Supervisor" ya "Cafe Boy"), har module ki permission tick kare, aur staff ko assign kare. Ek staff ke paas do roles bhi ho sakte hain (misal Trainer + Receptionist).

### 4.4 Default Role × Module matrix

F = poora access · B = sirf apni branch · A = sirf apne records · V = sirf dekhna · — = koi access nahi. Owner har cell badal sakta hai.

| Module | Owner | Manager | Reception | Accountant | Store | Sales | Trainer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Gym Setup / Settings | F | V | — | — | — | — | — |
| Theme & Branding | F | — | — | — | — | — | — |
| Roles & Permissions | F | — | — | — | — | — | — |
| Payment Accounts | F | — | — | V | — | — | — |
| Members | F | B | B | V | — | B | A |
| Membership Plans | F | V | V | V | — | V | — |
| Enquiry / Leads | F | B | B | — | — | B | — |
| Attendance | F | B | B | — | — | — | A |
| Fees & Vouchers | F | B | B | F | — | — | — |
| Online Payment Verification | F | B | V | F | — | — | — |
| Expenses | F | B | — | F | — | — | — |
| Inventory | F | B | V | V | F | — | — |
| Purchasing | F | B | — | V | F | — | — |
| POS / Product Sales | F | B | B | V | F | — | — |
| Profit & Loss | F | V | — | F | — | — | — |
| Staff / HR | F | B | — | V | — | — | A |
| Payroll / Salary | F | — | — | F | — | — | A |
| Trainers / PT | F | B | V | V | — | — | A |
| Classes | F | B | B | — | — | — | A |
| Workout & Diet | F | B | — | — | — | — | A |
| Progress | F | B | — | — | — | — | A |
| Chat | F | B | B | A | A | A | A |
| Notifications / WhatsApp | F | B | B | — | — | B | A |
| Lockers | F | B | B | — | — | — | — |
| Equipment | F | B | V | — | — | — | V |
| Reports | F | B | A | F | A | A | A |
| Audit Log | F | V | — | — | — | — | — |

### 4.5 Misal: permission kaise kaam karti hai

> Owner ne "Ali" ko Receptionist role diya, aur POS ki permission bhi de di lekin "purchase cost dekhna" band rakha. Ali ke app mein Members, Attendance, Fees aur POS nazar aate hain. POS mein woh protein bech sakta hai aur selling price dekh sakta hai, lekin product ki khareed qeemat, profit, Expenses aur Profit & Loss uske menu mein hain hi nahi.

## 5. Role-wise Dashboards

Login karte hi har role ko **apna dashboard** milta hai. Us par sirf woh widgets hote hain jo us role ke kaam ke hain aur jin modules ki use permission hai. Dashboard web aur mobile dono par same hai (mobile par cards ek ke neeche ek). Owner har role ke dashboard ke widgets ON/OFF kar sakta hai.

| Role | Dashboard par kya nazar aayega | Quick actions |
| --- | --- | --- |
| **Super Admin** | Total gyms (active/trial/suspended), naye gyms is mahine, platform ki monthly kamai, overdue gyms, total members (sab gyms), open support tickets, inactive gyms (churn risk) | Naya gym, plan badlo, announcement, ticket kholo |
| **Gym Owner** | Aaj ki collection (cash/online alag), aaj ke check-ins, abhi gym mein kitne log, naye admissions, expire hone wali memberships (7 din), defaulters aur total baqaya, aaj ki product sale aur munafa, is mahine ke expenses, is mahine ka net profit, pending approvals, low stock, staff present/absent, branch comparison | Approve, expense add, member search, announcement |
| **Partner** | Owner jaisa (jo owner ne diya), khaas taur par finance aur partner profit share | Reports |
| **Branch Manager** | Sirf apni branch: aaj ki collection, check-ins, admissions, expiring, defaulters, staff attendance, cash closing status, low stock, pending approvals (apni limit) | Approve, member search, cash closing check |
| **Receptionist** | Aaj ke check-ins, aaj ke due fees wale members, pending online payment screenshots, aaj ki enquiries/trials, birthdays, meri shift ki collection, unread chats | Naya admission, fee collect, check-in, POS sale |
| **Accountant** | Aaj/mahine ki collection method-wise (cash, JazzCash, Easypaisa, NayaPay, bank), verify hone wali payments, is mahine ke expenses, supplier payables, salary due, cash-in-hand, mahine ka P&L summary | Payment verify, expense add, supplier payment |
| **Store Manager** | Aaj ki product sale, low stock list, expire hone wale products (30 din), pending purchase orders, supplier dues, top selling products, (permission ho to) product profit | POS sale, stock in, naya PO |
| **Sales Consultant** | Aaj ke follow-ups, overdue follow-ups, trials aaj, is mahine ke conversions vs target, meri incentive | Naya lead, call log, trial book |
| **Trainer** | Mere members (aaj aaye/nahi aaye), aaj ke PT sessions/classes, jin ka workout plan purana ho gaya, unread chats, meri attendance, is mahine ki commission | Workout assign, measurement add, session complete |
| **HR / Admin** | Staff present/absent/late, leave requests, certificates expiry, salary run status | Leave approve, attendance theek karo |
| **Maintenance** | Mere open repair tickets, aaj ki cleaning checklist | Ticket close, checklist tick |
| **Member** | Membership status aur din baqi, agli fee kab aur kitni, pending voucher, aaj ka workout, class/PT booking, streak, gym announcements, unread chat | Pay fee, QR card, workout, chat |

### 5.1 Dashboard ke rules

- Har number par tap karne se uski detail list khulti hai (misal "Defaulters 23" → 23 members ki list).
- Date filter: aaj, kal, is hafte, is mahine, custom.
- Branch filter (sirf unke liye jinke paas multi-branch access ho).
- Paise ke numbers (profit, purchase cost) sirf unko jinke paas permission ho; baqi ko woh card nazar hi nahi aata.
- Owner ko roz subah WhatsApp/app notification par pichle din ka summary.

## 6. Modules 1–4

Har module mein yeh likha hai: woh kya karta hai, uske features, kaun kya karega, aur web/mobile par kya milega.

### Module 1: Super Admin (Platform Management)

Yeh aapka control room hai. Is se aap saare gyms, unke plans, modules aur billing chalate hain. Yeh **Super Admin web portal** aur **alag Super Admin app** dono par hai.

**Features:**

- **Gym onboarding:** naya gym banana (naam, owner ka naam, phone, CNIC, city, branches ki tadaad). Owner ko SMS/WhatsApp par login aur Gym Code chala jata hai.
- **Gym approval aur KYC:** owner ka CNIC aur business documents upload, approve/reject.
- **Gym status:** Trial, Active, Overdue, Suspended, Closed. Suspended gym ke users login karein to "subscription khatam" message aata hai, data safe rehta hai.
- **Subscription plans:** Starter/Growth/Pro (ya jitne aap banayein). Har plan mein price (PKR, monthly/yearly), max members, max branches, max staff, kaunse modules, SMS/WhatsApp credits.
- **Free trial:** 14/30 din, trial extend, trial khatam hone se pehle reminder.
- **Gym billing:** har gym ki monthly invoice auto, payment record (aapke apne account mein), due/overdue list, grace period ke baad auto-suspend.
- **Module control:** kisi bhi gym ke liye koi module ON/OFF, plan se hat kar bhi (misal ek gym ko free mein POS dena).
- **Discount/coupon** gyms ki subscription par.
- **Announcements:** saare gyms ya chune hue gyms ko notification/banner ("Naya feature", "Raat 2 baje maintenance").
- **Support:** tickets, priority, status; gym owners se chat (Chat module).
- **View as gym:** support ke liye owner ki ijazat se gym dekhna, har dafa audit log mein.
- **Platform staff:** support/sales/accountant accounts aur unki permissions.
- **Platform reports:** total gyms, active members, monthly recurring kamai, naye vs band gyms, sab se zyada use hone wale modules, inactive gyms.
- **Default templates:** default roles, exercise library, Pakistani food list, message templates, voucher designs — naye gym ko yahin se milte hain.
- **Data export:** gym chhorne par uska data Excel mein dena.

**Kaun kya karega:** Super Admin sab kuch · Support Agent tickets/chat/view-as-gym · Sales Agent leads/trials · Platform Accountant gym billing/reports.

**Web vs Mobile:** dono par poora. App par khaas: naye gym ki notification, ticket ka jawab, gym suspend/activate, aaj ki kamai.

### Module 2: Gym Setup & Settings

Naya gym pehli dafa login karta hai to **setup wizard** chalta hai. Baad mein yahi Settings se badla ja sakta hai.

**Features:**

- **Gym profile:** naam, logo, address, city, phone, WhatsApp, email, NTN/STRN (optional), Google Maps location.
- **Branches:** har branch ka naam, address, phone, manager, capacity (kitne log ek waqt mein).
- **Gym type:** Gents, Ladies, Mixed, ya Gents/Ladies alag timings.
- **Timings:** din-wise, branch-wise; gents/ladies shifts (misal ladies 10 AM–2 PM); subah/shaam sessions.
- **Ramadan mode:** Ramadan ki alag timings ek click se ON/OFF, members ko auto announcement.
- **Holidays:** Eid, 14 August, 9 Muharram wagera; gym band ho to membership ke din auto barhane ka option.
- **Facilities:** cardio, weights, ladies hall, sauna, steam, lockers, cafe — branch-wise.
- **Number series:** member ID (GYM-0001), receipt no., voucher no., invoice no. ka format.
- **Tax (optional):** sales tax ON/OFF, % (provincial rate owner khud daale), receipt par tax dikhana.
- **Language:** Urdu ya English default; har user apni language khud chun sake.
- **Payment methods:** kaunse tareeqe allow hain (cash, JazzCash, Easypaisa, NayaPay, bank, Raast, card machine, cheque). Accounts ki detail Module 9 mein.
- **Rules:** grace period (expiry ke baad kitne din entry), late fee, freeze rules, fee due day.
- **Terms & conditions / gym rules** jo admission par member accept kare.
- **Receipt/voucher footer** (misal "Fee non-refundable hai").

**Kaun kya karega:** sirf Owner (Manager ko view, agar owner de).

**Web vs Mobile:** dono par poora; setup wizard mobile par bhi chal jata hai.

### Module 3: Theme & Branding

Har gym apna logo aur rang set karta hai, aur woh gym ke web portal, gym app, member app, receipts, vouchers aur salary slips sab par lag jata hai.

**Features:**

- **Logo upload:** main logo, chhota icon. System check karta hai file sahi format (PNG/JPG/SVG) aur size mein ho.
- **Colors:** primary aur secondary color; system khud light aur dark versions bana deta hai.
- **Ready-made themes:** 8–10 themes (Classic Blue, Dark Iron, Gold, Ladies Rose, Green Fit...) ek click mein.
- **Dark / Light mode:** har user apni marzi se (system setting ke mutabiq ya manual).
- **Contrast check:** agar chuna hua rang par text parhna mushkil ho to warning.
- **Live preview:** save se pehle web, mobile, receipt aur voucher ka preview.
- **Slip designs:** receipt, voucher, member card, salary slip par logo, header aur footer text.
- **Member app branding:** member jab apne gym mein login karta hai to app usi gym ke logo aur rang mein nazar aati hai.
- **Platform theme:** Super Admin portal aur login screens ka default theme Super Admin set karta hai.
- **Purani theme par wapas** jana (undo).

**Kaun kya karega:** Owner (theme set) · Super Admin (platform default theme) · har user (sirf dark/light aur font size).

**Web vs Mobile:** dono par; theme ek dafa set karo, sab jagah lag jata hai.

### Module 4: Members Management

Gym ke har member ka poora record: profile, membership, fees, attendance, progress aur history ek jagah.

**Features:**

- **Profile:** naam, father/husband name, gender, date of birth, phone (+92 validation), WhatsApp, email, CNIC (optional, masked), address, area, emergency contact, blood group, profession, photo.
- **Quick admission:** sirf naam, phone, plan — 30 second mein; baqi detail baad mein.
- **Photo:** webcam ya mobile camera se.
- **Member ID aur QR card:** auto ID, QR wala digital card (member app mein) aur print wala card.
- **Health info:** bimari, injury, allergy, dawai, PAR-Q sawalat (sirf owner/manager/assigned trainer dekhein).
- **Goal:** weight loss, muscle gain, fitness, rehab.
- **Status:** Active, Expired, Frozen, Trial, Suspended, Blacklisted, Left.
- **Freeze:** safar, bimari, Ramadan — tareekh se tareekh, wajah, din auto barhte hain, freeze fee optional, manager approval.
- **Renew / Upgrade / Downgrade / Re-admission.**
- **Branch transfer** aur plan transfer (doosre member ko, owner approval se).
- **Family / couple / group membership:** ek package mein kai members, ek bill.
- **Corporate members:** company ke employees, company ke naam bill.
- **Tags:** VIP, Student, Defaulter, Regular, Ladies, Police/Army discount wagera.
- **Timeline:** admission, har payment, visits, freeze, trainer change, complaints — tareekh-war.
- **Notes aur documents:** CNIC copy, medical report, signed form.
- **Duplicate check:** same phone/CNIC par warning.
- **Blacklist:** banned member entry na kar sake, doosri branch mein bhi.
- **Bulk actions:** bulk message, bulk freeze (misal Eid band), bulk trainer assign, Excel export.
- **Birthday wish** auto WhatsApp/app par.
- **Referral source:** Facebook, Instagram, dost, banner, walk-in.
- **Search:** naam, phone, ID, CNIC se fori search.

**Kaun kya karega:** Receptionist admission/edit (apni branch) · Manager sab + freeze approve + blacklist · Owner sab + delete · Trainer apne members ka profile aur health (phone masked) · Accountant sirf payment history · Member apna profile dekhe aur kuch fields (photo, email, emergency contact) badle.

**Web vs Mobile:** dono par poora. Mobile par camera se photo aur CNIC scan asaan.

## 7. Modules 5–8

### Module 5: Membership Plans & Packages

Gym ke saare packages yahan bante hain. Har member kisi plan par hota hai, aur usi plan se uski fee aur entry ke rules tay hote hain.

**Features:**

- **Plan types:** monthly, 3 mahine, 6 mahine, yearly, daily pass, weekly pass, visits wala (misal 12 visits), sessions wala.
- **Fee hisse:** admission fee (ek dafa) + monthly fee + locker + trainer fee + add-ons.
- **Entry rules:** kaunsi branch, kaunsa waqt (sirf subah / sirf shaam), kaunsi facility (sirf cardio, sauna shamil), din mein kitni dafa.
- **Gender plans:** ladies plan, gents plan.
- **Khaas plans:** student, couple, family, corporate, senior, police/army, off-peak (dopahar sasta).
- **Add-ons:** personal training, diet plan, locker, towel, sauna, supplements bundle.
- **Discounts/offers:** % ya fixed, tareekh tak, coupon code, referral discount, Eid/Ramadan/New Year/14 August offers.
- **Installments:** fee qiston mein (misal yearly 3 qiston mein), har qist ki due date aur reminder.
- **Auto-renewal reminder** expiry se pehle.
- **Grace period:** expiry ke baad kitne din entry chalegi.
- **Pro-rata:** mahine ke beech admission par baqi dinon ki fee.
- **Price change:** nayi price sirf naye members par ya sab par — owner chune.
- **Freeze rules:** saal mein kitni dafa, max kitne din, freeze fee.
- **Trial / guest pass:** 1–3 din free ya paid trial.
- **Plan band karna:** purana plan naye members ko na dikhe, purane members chalte rahein.

**Kaun kya karega:** Owner plans banaye/badle · Manager/Receptionist/Sales sirf dekhein aur member ko assign karein · Member app mein plans aur prices dekh kar upgrade/renew request bheje.

**Web vs Mobile:** dono par poora.

### Module 6: Enquiry, Leads & Admission

Jo log gym ke baare mein poochte hain (walk-in, call, Facebook, WhatsApp) unka record, follow-up, trial aur phir admission.

**Features:**

- **Enquiry form:** naam, phone, gender, interest (weight loss, bodybuilding), source, budget, preferred timing.
- **Lead stages:** New → Contacted → Trial Booked → Trial Done → Joined / Lost.
- **Follow-up reminders:** call/WhatsApp/visit ki tareekh; overdue par alert.
- **Trial pass** de kar trial ke din ki attendance track.
- **Lost reason:** mehnga, door, timing, ladies trainer nahi.
- **Ek click mein member banana:** lead ka data admission form mein khud bhar jata hai.
- **Admission flow:** profile → plan → fee (cash/online) → terms accept (signature) → photo → receipt + member card + welcome WhatsApp.
- **Sales targets:** har sales staff ka monthly target aur achievement.
- **Source report:** kaunsa source zyada members laya.

**Kaun kya karega:** Receptionist/Sales enquiry aur follow-up · Manager leads assign aur report · Owner sab reports.

**Web vs Mobile:** dono par; mobile se seedha call/WhatsApp button.

### Module 7: Attendance & Entry Control

Member aur guest ki entry record karna, aur fee na dene walon ko rokna.

**Features:**

- **Check-in ke tareeqe:** member app ka QR (har 30 second badalta, screenshot kaam na kare), print card ka QR/barcode, fingerprint/face machine (ZKTeco wagera), receptionist manual (naam/phone/ID se).
- **Entry rules:** fee due / membership expire / frozen / blacklisted ho to entry block, ya sirf warning (owner chune).
- **Timing rules:** ladies time mein gents entry block, plan ki timing ke bahar warning.
- **Photo pop-up:** check-in par reception screen par member ki photo, taake koi doosra card use na kare.
- **Live count:** abhi gym mein kitne log hain; capacity full hone par alert.
- **Check-out** (optional): workout ka time.
- **Absent alert:** 7/15/30 din se nahi aaya to trainer/reception ko alert aur member ko "Hum aapko miss kar rahe hain" message.
- **Guest entry:** guest ka naam, phone, kis member ke saath, guest fee.
- **Reports:** roz ki attendance, member-wise, peak hours, gym kab sab se bhara hota hai.
- **Offline:** internet na ho to entry device par save, baad mein sync.

**Kaun kya karega:** Receptionist check-in aur guest entry · Trainer apne members ki attendance dekhe · Manager/Owner reports aur rules · Member apna QR aur attendance history.

**Web vs Mobile:** dono par; reception tablet par kiosk-jaisi badi screen, mobile camera se QR scan.

### Module 8: Fees, Vouchers, Receipts & Slips

School jaisa fee system: har mahine member ka **fee voucher (challan)** banta hai, member pay karta hai, aur payment confirm hote hi **receipt** banti hai.

**Features:**

- **Auto monthly vouchers:** har member ke liye uske plan ke hisab se (misal har mahine ki 1 tareekh ya joining date par).
- **Voucher par:** gym ka logo/naam, voucher no., member naam/ID, plan, mahina, line items (fee, locker, PT, pichla baqaya, discount), total, **due date**, **due date ke baad amount** (late fee ke saath), payment ke tareeqe (owner ke JazzCash/Easypaisa/NayaPay number, bank IBAN, Raast ID), QR code.
- **Print mode:** A4 par 3 copies (Gym copy, Member copy, Bank copy) ya 1 copy; thermal receipt 58/80mm.
- **Digital mode:** member app mein voucher, WhatsApp par PDF.
- **Fee counter screen:** member search → baqaya nazar aaye → amount → method → receipt print/WhatsApp. Pura kaam 15–20 second.
- **Partial payment** aur **advance payment** (agle mahinon ki).
- **Split payment:** aadha cash, aadha JazzCash.
- **Discount:** reason likhna lazmi; limit se zyada ho to approval.
- **Late fee:** fixed, % ya per day, max limit ke saath.
- **Voucher status:** Unpaid → Partially Paid → Paid, ya Overdue, ya Cancelled/Waived (approval se).
- **Defaulter list:** kitne din se baqi (7/15/30+), total baqaya, ek click mein sab ko reminder.
- **Refund / credit note:** approval workflow ke saath.
- **Receipt edit/delete band:** ghalti ho to "cancel receipt" request, owner approve kare, sab log mein.
- **Shift cash closing:** receptionist shift khatam par cash gin kar daale; system batata hai kitna hona chahiye, kami/ziyadti report, manager/owner ko alert.
- **Wallet / advance balance:** member ka bacha hua paisa wallet mein, agli fee ya store mein use.
- **Other slips:** admission slip, installment schedule, member card, salary slip (Module 16), POS receipt (Module 13), supplier payment slip.
- **Bulk:** ek click mein sab vouchers generate, bulk print, bulk WhatsApp.
- **Verify QR:** receipt/voucher par QR scan kar ke asli hone ka check.

**Kaun kya karega:** Receptionist fee collect, receipt, voucher print · Accountant online payments verify, refunds process, daily collection report · Manager discount/cancel approve (limit tak), cash closing check · Owner sab approvals, settings · Member voucher dekhe, pay kare, receipt download.

**Web vs Mobile:** dono par; bulk print web se behtar, collection aur WhatsApp receipt mobile se bhi.

## 8. Module 9: Payments — Paisa Seedha Owner ke Account Mein

Gym owner apne **JazzCash, Easypaisa, NayaPay, bank account (IBAN) aur Raast ID** system mein khud add karta hai. Member jo bhi online fee de, woh seedha **owner ke account** mein jati hai. Platform (aap) member ka paisa kabhi apne paas nahi rakhte. Platform sirf yeh record karta hai ke payment hui, aur receipt banata hai.

### 8.1 Do tareeqe (owner chune, dono ek saath bhi chal sakte hain)

|  | Tareeqa 1: Direct Transfer + Proof | Tareeqa 2: Merchant Account (Auto Confirm) |
| --- | --- | --- |
| Owner kya daalta hai | Apna JazzCash/Easypaisa/NayaPay number aur account title, bank IBAN + bank naam, Raast ID | Apne JazzCash/Easypaisa **merchant account** ki details (Merchant ID, keys) jo provider deta hai |
| Member kaise pay karta hai | App mein voucher kholta hai, owner ka number/IBAN copy karta hai, apni wallet/bank app se transfer karta hai, phir **screenshot + Transaction ID (TID)** upload karta hai | App mein "Pay" dabata hai, provider ka payment page khulta hai, wahin pay karta hai |
| Confirm kaise hota hai | Accountant/receptionist screenshot aur apni wallet/bank app mein aaya hua paisa match kar ke **Approve** karta hai | Provider khud system ko confirm bhejta hai; receipt auto ban jati hai |
| Kab best | Har gym ke liye, chhota ho ya bara; koi extra setup nahi | Jin gyms ke paas merchant account ho aur payments zyada hon |
| Kharcha | Normal transfer charges (member ke) | Provider ke merchant charges (owner ke) |

Note: NayaPay aur doosre wallets ke liye Tareeqa 1 hamesha chalega. Tareeqa 2 sirf un providers ke saath jo merchant integration dete hain. Technical document mein har provider ki availability confirm ki jayegi.

### 8.2 Owner payment account kaise add karta hai

1. **Settings → Payment Accounts → Add Account.**
2. **Type chunein:** JazzCash, Easypaisa, NayaPay, Bank Account, Raast ID (ya merchant account).
3. **Detail bharein:**
   - Wallet: mobile number (03XX-XXXXXXX validation) aur account title (jis naam par account hai).
   - Bank: bank naam (list se), account title, IBAN (PK + 22 characters validation), branch (optional).
   - Raast: Raast ID / registered mobile number.
   - Merchant: Merchant ID, Store ID, keys (provider ke diye hue).
4. **Branch chunein:** poore gym ke liye ya sirf kisi branch ke liye (har branch ka alag account ho sakta hai).
5. **Owner ke phone par OTP** — bina OTP account save nahi hota.
6. **Default set karein:** misal member ko pehle JazzCash dikhe.
7. Account **Active** ho jata hai aur member ke voucher aur app mein nazar aane lagta hai.

### 8.3 Security rules (staff ki gadbad rokne ke liye)

- Payment account **sirf Owner** add/badal/hata sakta hai, aur har dafa OTP lazmi. Koi staff, manager ya Super Admin yeh nahi kar sakta.
- Account badalte hi owner ko **WhatsApp + SMS alert** ("Aapka JazzCash number badla gaya").
- Merchant keys save hone ke baad **dobara nazar nahi aatin** (sirf aakhri 4 characters).
- Har tabdeeli audit log mein: kisne, kab, kya.
- Staff member ko apna personal number nahi de sakta, kyunki voucher aur app mein sirf owner ke saved accounts aate hain.

### 8.4 Payment verification (Tareeqa 1)

- Member ka upload: screenshot, TID, amount, tareekh, kis account mein bheja.
- Voucher **Pending Verification** ho jata hai; accountant/receptionist ko notification.
- **Duplicate check:** ek TID dobara use ho to system rok deta hai.
- **Amount check:** voucher amount se kam ho to partial payment ke taur par darj.
- Verifier Approve kare to receipt + WhatsApp + membership extend. Reject kare to wajah likhni lazmi, aur member ko message jata hai.
- Report: kis staff ne kitni payments verify kin, kitni pending hain (24 ghante se zyada pending ho to owner ko alert).

### 8.5 Doosre payment tareeqe

- **Cash** counter par (shift closing ke saath).
- **Card machine (POS terminal):** amount aur slip number darj.
- **Cheque:** cheque no., bank, tareekh; clear hone par confirm.
- **Wallet balance** (member ka advance).

### 8.6 Reports

- Method-wise collection: cash, JazzCash, Easypaisa, NayaPay, bank, Raast, card.
- Account-wise: kis account mein kitna aaya (owner apni wallet/bank statement se milaa sake).
- Pending verifications, rejected payments, refunds.

**Kaun kya karega:** Owner accounts add/manage · Accountant aur (permission ho to) Receptionist verify · Manager report dekhe · Member pay + proof upload · Super Admin sirf yeh dekh sakta hai ke gym ne payment account lagaya hai ya nahi, details nahi.

**Web vs Mobile:** dono par; member mobile se screenshot upload, accountant mobile notification se seedha approve.

## Diagrams: Payment aur Profit ka Flow

&#91;embedded content: online fee payment · 6 steps, owner ke account mein\]

Member owner ke account mein khud transfer karta hai. Staff apni wallet/bank app mein aaya hua paisa dekh kar member ke proof se match karta hai, phir approve karta hai.

&#91;embedded content: inventory se profit tak · khareed, stock, sale, P&L\]

Khareeda hua maal stock mein average cost par jata hai. Har sale ka munafa us cost se nikalta hai, aur mahine ke aakhir mein fees ki kamai jor kar aur expenses nikaal kar asal (net) munafa aata hai.

## 9. Modules 10–14: Expenses, Inventory, Purchasing, Product Sales aur Profit & Loss

Yeh paanch modules mil kar gym ka **poora karobari hisab** dete hain. Kitna maal kis qeemat par khareeda, kitne mein becha, har product par kitna munafa hua, mahine ke kharche kitne the, aur aakhir mein gym ne **asal munafa** kitna kamaya.

### Module 10: Expenses (Monthly Kharche)

**Features:**

- **Expense entry:** tareekh, category, amount, kis ko diya (vendor), kahan se diya (cash, bank, JazzCash, Easypaisa, NayaPay), branch, note, **bill/receipt ki photo**.
- **Categories (default, owner badal sake):** rent, bijli, gas, pani, internet/phone, salaries (payroll se auto), trainer commission (auto), maintenance/repair, cleaning, marketing/ads, equipment khareed, generator/UPS fuel, stationery, tea/refreshment, taxes/fees, misc.
- **Recurring expenses:** rent har mahine 1 tareekh, internet 10 tareekh — system khud entry bana deta hai (ya reminder), amount badal sakta hai (bijli ka bill).
- **Petty cash:** chhote kharchon ke liye alag cash, top-up aur hisab.
- **Approval:** limit se bara expense (misal 10,000 se upar) manager/owner approve kare.
- **Budget:** har category ka monthly budget aur asal kharcha, budget se upar jaane par alert.
- **Vendors:** bijli wala, cleaner company, mechanic — kis ka kitna dena baqi hai.
- **Expense edit/delete** sirf approval se, sab log mein.
- **Reports:** category-wise, branch-wise, mahine ka muqabla (is mahine vs pichla), sab se bara kharcha.

**Kaun kya karega:** Accountant entry aur vendor payments · Manager apni branch ki entry aur approval (limit tak) · Owner sab + bari approvals · Receptionist sirf petty cash entry (agar permission ho).

### Module 11: Inventory (Stock Management)

Gym jo bhi cheez bechta hai ya store karta hai (supplements, protein, drinks, gym wear, accessories, cafe ka saman) uska record.

**Product record:**

- Naam, category (protein, pre-workout, vitamins, drinks, snacks, clothing, accessories), brand, flavour/size (variants), barcode (scan ya system ka banaya), photo.
- **Purchase cost** (khareed qeemat) — sirf permission walon ko nazar aaye.
- **Selling price** (bikri qeemat), member price (members ke liye kam), aur system khud **margin %** dikhata hai.
- Unit (piece, box, scoop, bottle), minimum stock level (is se kam par alert).
- **Batch aur expiry date** (supplements ke liye zaroori).
- Branch-wise stock.

**Stock movements (har harkat ka record):**

| Movement | Stock par asar | Kahan se aata hai |
| --- | --- | --- |
| Purchase / Stock In | + | Module 12 (maal receive) |
| Sale | − | Module 13 (POS) |
| Sales Return | + | Customer ne wapas kiya |
| Purchase Return | − | Supplier ko wapas |
| Damage / Expired / Wastage | − | Store manager entry, approval se |
| Branch Transfer | − ek branch, + doosri | Branch se branch |
| Stock Count Adjustment | + / − | Physical ginti ke baad farq |
| Internal use | − | Gym ne khud istemal kiya (misal cleaning saman) |

**Features:**

- **Low stock alert** aur **expiry alert** (30/15/7 din pehle).
- **Stock count (audit):** mahine mein ek dafa physical ginti, system se farq, approval ke baad adjust.
- **Cost method:** har product ki **average cost** (weighted average) khud calculate — alag qeemat par khareede maal ka ausat. Isi se har sale ka munafa nikalta hai.
- **Stock value:** abhi godown mein kitne paise ka maal hai (cost par aur selling par).
- **Dead stock:** jo product 60/90 din se nahi bika.
- **Cafe / juice bar (optional):** recipe (misal shake = 1 scoop protein + 1 kela + doodh), har shake bikne par ingredients stock se khud kam.

### Module 12: Purchasing (Khareedari)

Suppliers se maal mangwana, receive karna, aur supplier ka hisab rakhna.

**Flow:**

1. **Supplier** add karein: naam, company, phone, address, NTN (optional), payment terms (cash ya udhaar kitne din).
2. **Purchase Order (PO):** kaunse products, kitni quantity, kis qeemat par. PO PDF/WhatsApp supplier ko.
3. **Maal receive (Goods Receipt):** jitna aaya woh darj; kam aaya to farq; batch aur expiry; stock khud barh jata hai.
4. **Purchase bill:** supplier ka bill (photo), total, discount, transport/freight kharcha (product ki cost mein shamil ho sakta hai).
5. **Supplier payment:** poori ya qist mein (cash, bank, wallet); baqaya supplier ke khate mein.
6. **Purchase return:** kharab/expire maal wapas, supplier ke khate mein credit.

**Features:**

- **Supplier ledger:** har supplier ka kitna diya, kitna baqi.
- **Bina PO ke direct purchase** (chhoti khareed ke liye).
- **Price history:** pichli dafa kis qeemat par liya; qeemat barhi to alert.
- **Approval:** bari purchase owner approve kare.
- **Reports:** supplier-wise khareed, product-wise khareed, payable dues.

### Module 13: Product Sales / POS

Gym ke counter par supplements, drinks aur gym wear bechna.

**Features:**

- **POS screen:** product search ya barcode scan, cart, quantity, discount (limit ke saath), total, payment, receipt (thermal print / WhatsApp).
- **Kis ko becha:** walk-in customer ya member (member chuna to member price lagti hai aur sale uski history mein jati hai).
- **Payment:** cash, JazzCash/Easypaisa/NayaPay/bank (owner ke account, Module 9), card, **member wallet**, ya **member ke agle voucher mein add** (udhaar).
- **Member app se order (optional):** member app mein products dekhe, order kare, counter se le.
- **Sales return / exchange:** approval se, stock wapas.
- **Bundles:** misal membership + protein combo.
- **Staff commission:** kis staff ne kitna becha, product sale par commission % (payroll mein jaata hai).
- **Hold sale:** customer ka cart rok kar doosre ko bechna.
- **Har sale ka munafa:** system sale ki qeemat aur product ki average cost se khud nikal leta hai.
- **Offline POS:** internet na ho to bhi sale, baad mein sync.
- **Reports:** roz/mahine ki sale, product-wise, staff-wise, payment method-wise, best sellers.

**Misal (product-wise munafa):**

| Product | Khareeda | Average cost | Becha | Bikri qeemat | Sale amount | Cost of goods | Munafa |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Whey Protein 2 lb | 10 dabbe | Rs 8,000 | 6 dabbe | Rs 10,500 | Rs 63,000 | Rs 48,000 | Rs 15,000 |
| Mineral Water | 120 bottle | Rs 45 | 100 bottle | Rs 80 | Rs 8,000 | Rs 4,500 | Rs 3,500 |
| Gym Gloves | 20 pair | Rs 600 | 8 pair | Rs 1,200 | Rs 9,600 | Rs 4,800 | Rs 4,800 |

Yeh sirf misal hai. Bacha hua maal (4 dabbe protein, 20 bottle, 12 pair) stock value mein rehta hai, munafa mein nahi.

### Module 14: Profit & Loss aur Finance

Gym ka **asal munafa** batata hai: saari kamai minus bechi hui cheezon ki cost minus saare kharche.

**P&L ka hisab:**

```
Kamai (Income)
  Membership fees + admission fees
+ Personal training fees
+ Product sales (POS)
+ Locker / facility / guest fees
+ Late fees aur doosri kamai
= Total Income

− Cost of Goods Sold (bechi hui products ki khareed cost)
= Gross Profit

− Expenses (rent, bijli, salaries, commission, maintenance, marketing ...)
= Net Profit (Asal Munafa)
```

**Misal (ek mahina, ek branch):**

| Hissa | Amount (Rs) |
| --- | --- |
| Membership + admission fees | 850,000 |
| PT fees | 120,000 |
| Product sales | 180,000 |
| Locker/guest/late fees | 20,000 |
| **Total Income** | **1,170,000** |
| Cost of goods sold | (130,000) |
| **Gross Profit** | **1,040,000** |
| Rent | (250,000) |
| Salaries + commission | (380,000) |
| Bijli, gas, internet | (140,000) |
| Maintenance, cleaning, marketing, misc | (70,000) |
| **Net Profit** | **200,000** |

Yeh numbers sirf samjhane ke liye hain.

**Features:**

- **P&L report:** mahine, quarter, saal ya custom tareekhein; branch-wise aur sab branches mila kar.
- **Service-wise munafa:** membership, PT, product sales, cafe alag alag.
- **Cash book:** roz ka cash aana/jana, opening/closing balance.
- **Account-wise balance:** cash, har JazzCash/Easypaisa/NayaPay/bank account mein kitna aaya aur gaya.
- **Receivables:** members ka baqaya. **Payables:** suppliers aur vendors ka dena.
- **Partner profit share:** partners ka hissa % ke hisab se.
- **Owner drawings:** owner ne apne liye kitna nikala (expense nahi, alag record).
- **Month closing:** mahine band karne ke baad purani entries lock (sirf owner unlock kare).
- **Muqabla:** is mahine vs pichla mahina, is saal vs pichla saal; graphs.
- **Excel/PDF export** (accountant/CA ke liye).

**Kaun kya karega:** Owner sab · Partner (agar owner de) P&L aur profit share · Accountant sab reports aur month closing · Manager apni branch ka summary (agar owner de) · Store manager sirf product sales aur product profit (agar permission ho).

**Web vs Mobile (Modules 10–14):** sab dono par. POS mobile/tablet par bhi barcode camera scan ke saath. Bari reports web par table mein, mobile par summary + PDF.

## 10. Modules 15–21

### Module 15: Staff & HR

**Features:**

- **Staff profile:** naam, CNIC, phone, address, role(s), branch(es), joining date, photo, emergency contact, bank/wallet detail (salary ke liye), documents (CNIC copy, certificates).
- **Login account:** staff ko app login aur role dena; staff chhorne par ek click mein login band.
- **Shifts:** subah/shaam/raat, roster, shift swap request.
- **Staff attendance:** fingerprint machine, app check-in (gym ke andar location se), QR, ya manual; late, early leave, overtime khud calculate.
- **Leaves:** casual/sick/annual, request → approve, baqi leaves ka hisab.
- **Tasks:** staff ko kaam dena ("cardio area saaf karo"), checklist, photo proof.
- **Warnings / disciplinary record.**
- **Certificates expiry alert** (trainer certificates).
- **Staff internal announcements** aur chat.

**Kaun kya karega:** Owner/HR sab · Manager apni branch ki attendance/leave approve · Har staff apni attendance, leave request, tasks.

### Module 16: Payroll, Salary Slips & Commission

**Features:**

- **Salary type:** fixed monthly, per day, per hour, per session, sirf commission, ya mixed.
- **Monthly salary run:** system attendance se din, late/absent deductions, overtime, allowances, bonus, commission, advance/loan ki qist, tax (agar ho) jor kar **net salary** nikalta hai.
- **Trainer commission engine:** per member, per PT session, per package sale, per renewal, product sale par %, target slabs (misal 20 members se upar zyada %). Mahine ke aakhir mein auto.
- **Advance / loan:** staff ko advance, har mahine auto kati.
- **Salary slip:** gym logo, staff naam, mahina, earnings aur deductions ki line-by-line detail, net pay, payment tareeqa. PDF, print, WhatsApp, aur staff app mein.
- **Staff ka "Received" tap** app mein (acknowledgement).
- **Salary payment record:** cash/bank/JazzCash/Easypaisa/NayaPay; payment hote hi Expenses mein "Salaries" khud darj.
- **Approval:** salary run owner approve kare.
- **Reports:** mahine ka total payroll, staff-wise, commission report.

**Kaun kya karega:** Accountant/HR salary banaye · Owner approve · Har staff sirf apni slip aur commission dekhe.

### Module 17: Trainers & Personal Training (PT)

**Features:**

- **Trainer assignment:** har member ka main trainer (aur backup).
- **Trainer profile:** specialty, timing, gender (ladies ke liye female trainer).
- **PT packages:** 12/24/36 sessions, validity, price.
- **Session booking:** trainer ka calendar, khali slots, double booking rokna.
- **Session complete:** member ke OTP ya QR se confirm (taake fake session na likha jaye).
- **Session notes:** kya karwaya, weights.
- **Cancel/reschedule rules:** late cancel par session kat jaye.
- **Baqi sessions** aur expiry; "sirf 2 sessions bache" reminder.
- **Trainer change request** member ki taraf se.
- **Commission** Module 16 mein khud jati hai.
- **Trainer rating** members dein.

**Kaun kya karega:** Manager assign kare · Trainer/PT apne sessions aur members · Receptionist PT package beche · Member booking aur rating.

### Module 18: Classes & Batches

**Features:**

- **Class types:** Yoga, Zumba, Aerobics, Spinning, CrossFit, HIIT, Boxing, Martial arts, Kids, Ladies-only.
- **Timetable:** hafta-war, instructor, hall, capacity.
- **Booking:** member app se, full ho to waitlist (jagah khali hote hi auto).
- **Class attendance**, no-show par penalty (optional).
- **Fixed batches:** misal 6–7 AM batch, batch-wise members.
- **Instructor change ya class cancel** par booked members ko auto notification.
- **Events/challenges:** 30-day transformation challenge, leaderboard, registration fee.
- **Reports:** kaunsi class zyada bharti hai, instructor-wise.

**Kaun kya karega:** Manager timetable · Instructor attendance · Receptionist booking · Member booking/cancel.

### Module 19: Workout Plans

**Features:**

- **Exercise library:** 500+ exercises, muscle group, equipment, level, video/GIF, Urdu + English naam.
- **Gym apni exercises/videos** add kar sake.
- **Plan builder:** din → exercises → sets/reps/weight/rest.
- **Templates:** beginner, intermediate, advanced, fat loss, muscle gain, ladies, home workout.
- **Assign:** ek member ya bohot saare members ko ek saath.
- **Member app:** aaj ka workout, set tick, weight likhna, rest timer, video.
- **Progress:** personal records, graphs.
- **Printable PDF** (jin members ke paas smartphone nahi).
- **Plan purana hone par reminder** (4–6 hafte baad badlo).

**Kaun kya karega:** Trainer banaye/assign kare · Member follow kare aur log kare · Manager dekhe.

### Module 20: Diet Plans (Pakistani Khana)

**Features:**

- **Pakistani food list:** roti, chawal, daal, sabzi, chicken karahi, biryani, nihari, paratha, anda, doodh, chai, lassi, fruits — calories aur protein/carbs/fat ke saath.
- **Calorie calculator:** umar, wazan, qad aur goal se roz ki calories.
- **Diet plan builder:** nashta, dopahar, snack, raat ka khana.
- **Templates:** weight loss, bulking, vegetarian, sugar (diabetic-friendly), **Ramadan (sehri/iftari)**.
- **Member food log**, paani ka hisab, khane ki photo trainer ko.
- **Supplement plan** (gym store ke products se link).
- **PDF/WhatsApp share.**
- Note: yeh doctor ki salah ki jagah nahi.

**Kaun kya karega:** Trainer/Nutritionist plan banaye · Member follow aur log kare.

### Module 21: Body Measurements & Progress

**Features:**

- Wazan, qad, BMI, body fat %, chest, waist, hips, arms, thighs.
- **Progress photos** (front/side/back), before/after — sirf member aur uska trainer dekhe; chehra blur ka option.
- **Graphs:** wazan aur measurements ka trend.
- **Goal:** target wazan, deadline, badges.
- **Monthly assessment reminder.**
- **Progress report PDF** member ko.

**Kaun kya karega:** Trainer measurements darj kare · Member dekhe aur khud bhi wazan darj kare.

**Web vs Mobile (Modules 15–21):** sab dono par. Trainer ka zyada kaam mobile par (gym floor par).

## 11. Modules 22–28

### Module 22: Chat System

App ke andar real-time chat, taake gym ko alag WhatsApp groups na banane parein aur har baat record mein rahe.

**Chat ki qismein:**

| Chat | Kaun kis se |
| --- | --- |
| Member ↔ Trainer | Member apne assigned trainer se (1-to-1) |
| Member ↔ Gym Support | Member front desk se; kai staff ek shared inbox se jawab dein |
| Staff ↔ Staff | Staff aapas mein, branch groups ("Morning Shift") |
| Owner ↔ Staff | Owner kisi bhi staff ya group se |
| Class / Batch group | "6 AM Zumba" jaise groups |
| Announcement channel | Gym → sab members (sirf ek taraf; members jawab nahi de sakte) |
| Gym Owner ↔ Super Admin Support | Platform support chat |

**Features:**

- Text, emoji, photo, video, PDF, **voice note**, workout/diet plan share.
- Sent / delivered / seen ticks, typing indicator.
- Reply, delete (kuch minute tak), message search, pin.
- Push notification, mute, unread count.
- Support inbox mein conversation staff ko assign karna, internal note (member ko nazar na aaye), "resolved" mark.
- Office time ke baad auto-reply ("Hum subah 9 baje jawab denge").

**Rules aur privacy:**

- Owner tay kare kaun kis se chat kar sakta hai (default: member sirf apne trainer aur support se, doosre members se nahi).
- **Ladies privacy:** ladies members ke saath sirf female staff chat kare (setting); member ka phone number kisi ko nazar nahi aata.
- Report/block button, gandi zaban ka filter.
- Trainer gym chhorde to uski chats gym ke paas rehti hain, trainer ka access band.
- Owner sirf complaint ki soorat mein kisi chat ko dekh sake, aur woh bhi log mein.

**Kaun kya karega:** har role apni allowed chats · Owner rules set kare · Super Admin support chat.

### Module 23: Notifications, WhatsApp & SMS Reminders

**Features:**

- **Channels:** WhatsApp (Business API), SMS, app push notification, in-app notification.
- **Auto messages:** welcome, fee voucher bana, fee due (3 din pehle, due wale din, baad mein), fee mili (receipt), membership expire (7/3/1 din pehle), expire ho gayi, 7 din se absent, birthday, class booked/cancel, plan update, payment verify/reject.
- **Templates:** Urdu, English, Roman Urdu; variables {naam}, {amount}, {due\_date}, {gym\_naam}.
- **Bulk messages:** chuni hui list ko (expired members, defaulters, ladies) — schedule, preview, delivery report.
- **Quiet hours:** raat 10 se subah 8 tak messages na jayen.
- **Credits:** har gym ke SMS/WhatsApp credits; Super Admin sell/allocate kare (aapki extra kamai).
- **Member opt-out** promotional messages se.
- **Owner ko daily summary** subah WhatsApp par.
- **Staff alerts:** approvals pending, follow-ups, low stock, pending payments.

**Kaun kya karega:** Owner templates/rules · Manager/Receptionist bulk messages (permission se) · Super Admin credits.

### Module 24: Lockers & Facilities

**Features:**

- Locker list: number, size, gents/ladies, status (khali/bhara/kharab).
- Locker dena: monthly/daily, fee (voucher mein auto), deposit, chabi/code.
- Expiry alert aur overdue.
- Facility booking: sauna, steam, massage, court — time slot, capacity, fee.
- Towel service (diya/wapas).
- Lost & Found register.
- Chabi/card issue aur wapsi.

**Kaun kya karega:** Receptionist assign/return · Manager settings · Member booking aur apna locker info.

### Module 25: Equipment & Maintenance

**Features:**

- Equipment register: naam, brand, model, serial, khareed tareekh, qeemat, supplier, warranty, branch/zone, photo, QR tag.
- Maintenance schedule aur service reminders.
- **Kharabi ticket:** report → assign (mechanic/maintenance staff) → theek → band; kharcha (Expenses mein auto), kitne din band rahi.
- Member app se report ("Treadmill 3 kharab hai"); machine par QR scan kar ke bhi.
- Warranty expiry alert.
- Cleaning checklist roz, staff sign-off.
- Branch transfer.

**Kaun kya karega:** Maintenance staff tickets · Manager assign aur approve · Owner reports · Member report kare.

### Module 26: Reports & Analytics

Har report Excel/PDF export, print aur WhatsApp share. Har role ko sirf apni permission wali reports nazar aati hain.

| Area | Reports |
| --- | --- |
| Members | Active/expired/frozen, naye admissions, renewals, chhorne wale (churn), plan-wise, gender/umar, referral source |
| Fees | Roz/mahine ki collection, method-wise, staff-wise, defaulters aur aging, discounts, refunds, cancelled receipts, pending verifications |
| Attendance | Roz ki attendance, peak hours heatmap, absent members, guest log |
| Leads | Enquiries, conversion %, source-wise, staff-wise, lost reasons |
| Inventory | Stock in hand, stock value, low stock, expiry, dead stock, stock movements |
| Purchasing | Supplier-wise khareed, payable dues, price history |
| Sales (POS) | Roz/mahine ki sale, product-wise, staff-wise, best sellers, **product-wise munafa** |
| Expenses | Category-wise, branch-wise, budget vs asal, mahine ka muqabla |
| Profit & Loss | P&L, gross/net profit, service-wise munafa, cash book, partner share |
| Staff | Attendance, late/leave, payroll, commission |
| Trainers | Members count, PT sessions, retention, rating |
| Classes | Fill rate, popular classes |
| Equipment | Kharabi, maintenance kharcha |
| Branches | Branch comparison: collection, members, munafa |

**Aur features:** apni custom report banana aur save karna · scheduled reports (har subah owner ko) · smart alerts ("Is mahine 80 members expire, 30 ne renew nahi kiya", "Shaam 7–9 rush hota hai").

### Module 27: Multi-Branch Management

**Features:**

- Ek gym ki kai branches; owner ka ek dashboard sab ke liye, ya branch filter.
- Har branch ke apne staff, timings, facilities, stock, expenses, payment account (optional).
- Plans aur prices sab branches ke liye ek jaise ya branch-wise alag.
- **Member ko doosri branch** mein entry (plan allow kare to).
- Branch transfer: member, staff, stock, equipment.
- Branch comparison reports.
- Branch manager sirf apni branch dekhe.

**Kaun kya karega:** Owner sab branches · Manager apni branch.

### Module 28: Complaints & Feedback

**Features:**

- Member complaint (app se), anonymous option; ticket number, status (Open → In progress → Resolved), jawab ka time.
- Suggestion box.
- Feedback/rating: trainer, class, gym ki safai, overall (1–5 star).
- Joining ke 30 din baad aur PT session ke baad auto feedback request.
- Bura feedback ho to manager/owner ko fori alert.
- Staff-wise rating report.

**Kaun kya karega:** Member bheje · Receptionist/Manager jawab de · Owner reports.

**Web vs Mobile (Modules 22–28):** sab dono par.

## 12. Modules 29–34

### Module 29: Documents & Forms

**Features:**

- **Digital admission form** aur gym rules par member ka **signature** (tablet/phone par ungli se).
- **Health declaration (PAR-Q)** digital.
- **Document storage:** member ka CNIC copy, medical report; staff ke documents; gym ke license/fire safety certificates.
- **Expiry alerts:** staff certificates, gym licenses.
- **Minors (18 se kam):** walid/sarparast ki ijazat aur unka phone.

**Kaun kya karega:** Receptionist form sign karwaye · Owner/HR documents · Member apne signed forms dekhe.

### Module 30: Security, Audit Log & Anti-Fraud

Owner ki ghair-maujoodgi mein bhi koi staff hera-pheri na kar sake.

**Features:**

- **Login security:** password + OTP; owner aur Super Admin ke liye 2-step lazmi; app par fingerprint/face lock; device list aur kisi device ko door se logout.
- **Audit log:** har login, add, edit, delete, discount, refund, receipt cancel, stock adjustment, expense, export — kisne, kab, kis device se, purani aur nayi value.
- **Anti-fraud alerts (owner ko fori):**
  - Receipt cancel ya bara discount.
  - Shift cash mein kami.
  - Odd time (raat) login ya entry.
  - Ek din mein bohot zyada refunds/discounts.
  - Stock mein bari kami (damage/adjustment).
  - Payment account badalne ki koshish.
  - Online payment 24 ghante se verify nahi hui.
- **Delete = Recycle bin:** koi record pakka delete nahi hota; owner 30 din tak wapas la sake.
- **Data alag:** ek gym ka data doosre gym ko kabhi nazar nahi aata.
- **Data safety:** roz automatic backup; data encrypted; CNIC aur health info extra protected.
- **Data export:** gym ka data hamesha uska, Excel mein nikal sake.

**Kaun kya karega:** Owner audit log aur alerts · Manager apni branch ka log (agar owner de) · Super Admin platform ka log.

### Module 31: Offline Mode

Load-shedding ya internet na hone par kaam na ruke.

**Features:**

- **Offline mein chalne wale kaam:** member search, check-in, cash fee collection + receipt, POS sale (cash).
- Internet aate hi sab **khud sync**.
- Receipt numbers offline mein bhi duplicate na hon (device code ke saath).
- Reception screen par "Offline — X entries sync hona baqi" ka nishan.
- Member app: workout plan aur member QR card offline bhi chalta hai.

### Module 32: Data Import (Purane Register/Excel se)

Naye gym ka purana data jaldi system mein lana. Yeh Pakistan mein bara selling point hai.

**Features:**

- **Excel/CSV template** download: members, plans, baqaya, products, stock, suppliers, staff.
- Upload → columns match → ghaltiyon ki list (phone ghalat, duplicate) → test run → final import.
- Ghalat import ho to **undo**.
- Super Admin team gym ke liye import kar ke de (paid service ho sakti hai).

**Kaun kya karega:** Owner/Manager import · Super Admin support madad kare.

### Module 33: Loyalty & Referral (Optional)

**Features:**

- **Referral:** member dost ko laaye to dono ko reward (free din, discount).
- **Points:** waqt par fee, attendance streak, referral par points; points se discount ya products.
- **Badges/streaks:** "30 din streak", "100 visits".
- **Member of the Month**, leaderboards (ladies/privacy opt-out ke saath).
- **Birthday reward.**

**Kaun kya karega:** Owner rules · Member points dekhe/use kare.

### Module 34: Help & Support (Gym ↔ Platform)

**Features:**

- Gym owner/staff app ke andar **support ticket** aur **chat** Super Admin team se.
- Help center: Urdu video tutorials, FAQs, "yeh kaise karein" guides.
- Naye gym ke liye **onboarding checklist** (logo lagaya? plans banaye? payment account add kiya? staff add kiya?).
- **Naya feature** aane par in-app message.
- Demo/sandbox data: naya owner seekhne ke liye sample members aur sales, phir ek click mein saaf.

**Kaun kya karega:** Gym Owner/staff ticket/chat · Support Agent jawab · Super Admin announcements.

**Web vs Mobile (Modules 29–34):** sab dono par.

## 13. Gyms ke liye Subscription Plans aur Development Phases

### 13.1 Plans (PKR, monthly ya yearly)

Prices aap market research ke baad tay karein. Neeche sirf yeh likha hai ke kis plan mein kya hoga.

| Plan | Kis ke liye | Modules | Limits (misal) |
| --- | --- | --- | --- |
| **Starter** | Chhota gym, 1 branch | Members, Plans, Enquiry/Admission, Attendance (QR/manual), Fees & Vouchers, Payments (owner accounts), Expenses, basic Reports, WhatsApp/SMS reminders, Theme, Chat (member–gym) | \~250 members, 5 staff |
| **Growth** | Medium gym | Starter + Inventory, POS/Product Sales, Purchasing, Profit & Loss, Staff/HR, Payroll + salary slips, Trainers/PT, Classes, Workout, Diet, Progress, Lockers, full Chat | \~1,000 members, 2 branches |
| **Pro** | Bara gym / chain | Growth + Multi-branch, Biometric machines, Equipment, Offline mode, Loyalty, advanced Reports, custom roles, audit alerts | Unlimited members, 10 branches |

**Add-ons:** extra WhatsApp/SMS credits, extra branch, biometric machine setup, purane data ka import service, training.

### 13.2 Development Phases

| Phase | Kya banega | Kyun pehle |
| --- | --- | --- |
| **Phase 1 (MVP)** | Super Admin web + app, Gym setup, Theme, Roles & Permissions, Module ON/OFF, role-wise dashboards, Members, Plans, Enquiry/Admission, Attendance (QR + manual), Fees & Vouchers, Payments (owner accounts + proof verification), Expenses, basic Reports, reminders, Member app, Data import | Gym chalane ke liye bunyadi cheezein; is se pehle gyms on-board ho sakte hain |
| **Phase 2** | Inventory, Purchasing, POS, Profit & Loss, Staff/HR, Payroll + salary slips, Chat, Trainers/PT, Workout, Diet, Progress, Classes, Lockers, Offline mode | Karobari hisab aur members ka experience |
| **Phase 3** | Biometric machines, Multi-branch, Equipment, Complaints/Feedback, Documents/signature, Loyalty/Referral, advanced Reports, merchant (auto-confirm) payments | Bare gyms aur chains |

Har phase ke aakhir mein 3–5 asli gyms par test, unka feedback, phir agla phase.

## 14. Faisle jo abhi lene hain

Technical document se pehle in sawalon ke jawab chahiye. Har ek ke saath meri recommendation hai.

| # | Sawal | Recommendation |
| --- | --- | --- |
| 1 | Online payment: pehle sirf Direct Transfer + Proof, ya merchant auto-confirm bhi? | Phase 1 mein Direct Transfer + Proof (har gym ke liye chalta hai); merchant integration Phase 3 mein |
| 2 | Product cost ka tareeqa | Weighted average cost (asaan aur kaafi accurate) |
| 3 | Member pehli dafa gym kaise chune: Gym Code ya QR? | Dono; receptionist admission par member ko invite SMS bhi bheje |
| 4 | WhatsApp: official Business API? | Haan, official API; unofficial tareeqe se number ban ho sakta hai |
| 5 | Kaunsi biometric machines pehle? | ZKTeco (Pakistan mein sab se aam) |
| 6 | Tax (sales tax) Phase 1 mein? | Sirf % daalne ka option; FBR/PRA integration baad mein, zaroorat par |
| 7 | Plans ki prices | 10–15 gym owners se baat kar ke tay karein |
| 8 | Chat mein member–member baat? | Default band; sirf class groups mein owner ON kare |
| 9 | Super Admin app mein kya kya | Sab Super Admin features; khaas zor notifications, tickets aur gym activate/suspend par |
