# Kitchen Commerce Suite — End-to-End Build Guide

## 1) Product Vision
Build a **kitchen-first commerce platform** with:
- Backoffice web app for kitchen/admin/staff.
- Mobile-optimized public online store opened via QR code.
- WhatsApp-driven ordering, payment updates, and order tracking.
- Openclaw AI agent for customer care + assisted ordering in WhatsApp chat.

---

## 2) Recommended Architecture

## 2.1 Core Applications
1. **Admin Web App (PWA-friendly)**
   - Inventory, sales, debt book, staff, payroll, settings.
2. **Public Storefront (Mobile Web)**
   - Product listing, cart, checkout intent, WhatsApp handoff.
   - Desktop blocked or downgraded view.
3. **API Backend**
   - Auth, products, pricing, orders, payments, customers, reports.
4. **Realtime/Jobs Layer**
   - Event queue for WhatsApp messages, AI responses, receipt notifications, sync jobs.
5. **AI Agent Service (Openclaw integration adapter)**
   - Intent parsing, FAQ answers, order capture, escalation to human.

## 2.2 Suggested Tech Stack
- **Frontend:** Next.js + TypeScript + Tailwind + shadcn/ui.
- **Mobile app:** React Native (Expo) with embedded web views for admin shortcuts where useful.
- **Backend:** NestJS or Fastify (TypeScript).
- **Database:** PostgreSQL.
- **Cache/Queue:** Redis + BullMQ.
- **File storage:** S3-compatible storage for product images/logo.
- **Auth:** JWT + refresh tokens + role-based ACL.
- **Observability:** OpenTelemetry + centralized logs.

---

## 3) Skills / Teams Needed

## 3.1 Product & Delivery Skills
- Product Management
- UX/UI (POS + mobile commerce UX)
- Technical Writing (SOPs, onboarding)
- QA (manual + automated)

## 3.2 Engineering Skills
- Frontend engineer (Next.js/shadcn)
- Mobile engineer (React Native)
- Backend engineer (Node.js + PostgreSQL)
- DevOps engineer (CI/CD, infra, backups)
- Data/reporting engineer (sales/finance analytics)
- AI integration engineer (Openclaw + WhatsApp workflows)

## 3.3 Operations Skills
- Customer success/support workflows
- Store onboarding + training
- Financial operations (tax, payroll, debt collection flows)

---

## 4) Domain Model (High Level)
Create entities for:
- Organization, Shop, User, Role, Permission
- Product, Category, SubCategory, AddOn, Variation
- InventoryLedger, StockAdjustment, ProductTransfer
- Sale, SaleItem, CheckoutSession, Tax, Discount
- Customer, CreditAccount, DepositTransaction, DebtLedger
- TableSection, SectionPricingRule, SectionQRCode, TableOrder
- StaffInvite, Employee, PayrollRun, SalaryPayment
- Settings (Business, Printer, Tax, Troubleshooting)
- WhatsAppThread, MessageEvent, AIAgentSession

---

## 5) Build Roadmap (Start to Finish)

## Phase 0 — Discovery (Week 1)
- Finalize requirements and acceptance criteria for each menu item.
- Define countries/currencies/tax rules.
- Decide WhatsApp provider (Cloud API/BSP).
- Security/privacy requirements and backup policy.

Deliverables:
- PRD, user stories, API contract draft, ERD v1.

## Phase 1 — Foundation (Weeks 2–3)
- Monorepo setup.
- Auth + roles + permissions.
- Organization/shop bootstrap onboarding.
- Settings foundation (business profile, tax base fields).

Deliverables:
- Sign-up/login, invite flow, role checks, audit logging.

## Phase 2 — Product & Inventory (Weeks 4–6)
- Categories/subcategories/add-ons/variations.
- Product CRUD with image upload, barcode flags, tax mapping.
- Product settings: units, price tag config, reset quantity.
- Inventory adjustment + product transfer + history.

Deliverables:
- All Products page, Add Product page, barcode generation page.

## Phase 3 — Sales POS + Checkout (Weeks 7–9)
- Sales page with category filter, search, cart.
- Product option modal and quantity logic.
- Checkout + VAT inclusive/exclusive + discount.
- Attach customer flow (required/optional configurable).
- Credit sales + deposits + due dates.
- Print receipt and pending receipt rules.

Deliverables:
- Quick start actions, checkout pipeline, receipt engine.

## Phase 4 — Customer & Debt Book (Weeks 10–11)
- Customer directory + debt summary.
- Debt book overview (credit, paid, balance, deposits).
- Red/green/orange status markers and reports export.

Deliverables:
- Credit/deposit lifecycle complete.

## Phase 5 — Online Store + QR Table Ordering (Weeks 12–14)
- Mobile-only storefront experience.
- Table section management (Rooftop/Bar/VIP etc).
- Section pricing adjustment (% or amount).
- QR generation per section.
- Order capture + status tracking + approval queue.

Deliverables:
- End-to-end scan QR → order → kitchen approval.

## Phase 6 — WhatsApp + Openclaw Agent (Weeks 15–17)
- WhatsApp message orchestration (order, payment info, status updates).
- Two-way thread mapping with internal order IDs.
- Openclaw assistant intents:
  - Menu inquiry
  - Place order
  - Track order
  - Payment instruction
  - Human handoff
- Escalation rules and transcript logging.

Deliverables:
- AI-assisted order capture through WhatsApp.

## Phase 7 — Staff, Payroll, Advanced Settings (Weeks 18–20)
- Staff invites, role matrix, permission toggles.
- Employee records + payroll runs + tax + commission.
- Printer settings and troubleshooting reset controls.

Deliverables:
- Admin operations parity across requested modules.

## Phase 8 — Hardening & Launch (Weeks 21–22)
- Load testing, security testing, backup restore drills.
- UAT with 1–3 pilot kitchens.
- Documentation + training + rollout checklist.

Deliverables:
- Production launch readiness signoff.

---

## 6) UX Blueprint by Menu

## 6.1 Header Quick Actions
- Record Sales
- Add Product
- Adjust Stock

## 6.2 Overview
- Sales totals: day/week/month/year.
- Order totals: day/week/month/year.
- Revenue, expenses, recent transactions.

## 6.3 Products Module
Submenu:
- All Products
- Add-ons
- Product Transfer
- Product Settings
- Generate Barcode
- Product History

All Products table columns:
- Product (+ category subtitle)
- Quantity
- Purchase price
- Selling price
- Profit
- Actions (opens shadcn Sheet)

Sheet actions:
- Manage info
- Quantity/pricing adjustment
- Derived values:
  - purchase value = quantity × purchase price
  - sales value = quantity × selling price
- Product history

## 6.4 Sales + Checkout
- Product list with filters and search.
- Cart with print receipt / proceed checkout.
- Confirm checkout modal with subtotal/tax/total.
- Payment methods: cash/POS/transfer/multiple.
- Credit mode with deposit and due date.

## 6.5 Customers
- List with debt/total order, search, filters.
- Add customer modal with WhatsApp flag and credit limit.

## 6.6 Debt Book
- Counters: total customers, credit records, paid, balance.
- Filters: all/cleared/uncleared.
- Color coding:
  - debt = red
  - amount paid = green
  - deposit = orange

## 6.7 Online Store (Public)
- Mobile-first only, desktop disabled.
- QR entry with section-aware pricing/catalog.

## 6.8 Table Order
- Section setup and product mapping.
- QR generate per section.
- Order management and pending approvals.

## 6.9 My Staff
- Tabs: Staffs, Pending Requests, Roles.
- Preset roles: Manager, Cashier, Sales Person + custom permissions.

## 6.10 Payroll
- Employee list + status + bulk upload.
- Salary config (hourly/weekly/biweekly/monthly/quarterly/annual).
- Payroll runs and actions (pause/deduction/top-up).

## 6.11 Settings
- Profile, transfer shop ownership, onboarding new owner.
- Business settings + logo.
- Advanced toggles and troubleshooting reset controls.
- Tax settings and printer settings.

---

## 7) WhatsApp Integration Design

## 7.1 Core Flows
1. Customer places order from web store → order summary sent via WhatsApp.
2. Customer messages business first → AI collects order details.
3. Payment instructions and status confirmation sent on thread.
4. Human takeover command routes to staff dashboard.

## 7.2 Message Templates
- Order received
- Awaiting approval
- Approved + payment instruction
- Payment received
- Order ready / delivered

## 7.3 Data Sync Rules
- Every WhatsApp thread maps to customer + order(s).
- Idempotency keys for webhook retries.
- Store all inbound/outbound event logs.

---

## 8) Openclaw AI Agent Integration

## 8.1 Agent Responsibilities
- Answer FAQs (hours, location, menu, pricing basics)
- Guide ordering (item, quantity, add-ons, confirmation)
- Track order status
- Escalate to human when confidence is low

## 8.2 Guardrails
- Never finalize payment without explicit confirmation.
- Never alter price outside rules from backend.
- Always summarize order before submit.
- Log all agent actions for audit.

## 8.3 Required Agent Skills
- Intent classification
- Entity extraction (item, qty, section, customer info)
- Dialogue state tracking
- Tool calling to backend APIs
- Handoff trigger policy

---

## 9) Security, Compliance, and Reliability
- RBAC everywhere.
- Audit logs for sensitive actions (price, stock, transfers, credits).
- Backups daily + point-in-time recovery.
- Encrypt PII at rest and in transit.
- Rate limiting + webhook signature validation.

---

## 10) QA Checklist (Must Pass Before Launch)
- Product CRUD + stock ledger correctness.
- Checkout totals accuracy (tax/discount/credit/deposit).
- Debt book math and color states.
- QR section pricing correctness.
- WhatsApp webhook reliability and retries.
- AI order placement regression suite.
- Printer format compatibility (thermal + A4).

---

## 11) Suggested API Modules
- /auth
- /users, /roles, /permissions
- /shops, /settings
- /products, /categories, /addons, /barcodes
- /inventory, /transfers
- /sales, /checkout, /payments
- /customers, /credits, /deposits
- /table-sections, /qr, /orders
- /whatsapp/webhooks, /ai-agent
- /staff, /payroll
- /reports

---

## 12) Implementation Tips
- Build calculations in backend first; UI consumes computed totals.
- Use feature flags for settings toggles.
- Use event-driven architecture for notifications and printing.
- Start with one currency/tax profile, then generalize.
- Prioritize mobile performance: image compression, pagination, caching.

---

## 13) Definition of Done (Platform)
- Kitchen can manage catalog, stock, sales, credits, staff, payroll, settings.
- Customer can scan QR, browse section-specific menu, order, and track via WhatsApp.
- Openclaw agent can support customer care and capture structured order intent.
- Finance/admin can reconcile receipts, debt book, and reports reliably.
