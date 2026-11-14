# CAMIS requirements and thesis audit

Reviewed 14 November 2026: `D:\fypcamis\doc\24727 requirements.docx`, `fypi24727.docx`, backend `camis-bn`, and frontend `camis-fn`. This is a source audit of those folders. The frontend passed TypeScript checking; the backend did not. The database, deployed instance, browser workflows and Flutterwave sandbox were not exercised, so no item is certified as passing end-to-end.

**HOD module count: 1 of 8 has a connected implementation in source, 6 are partial, and 1 is missing.** Connected means the relevant UI calls its backend; partial means APIs, data structures, or screens exist but the workflow is incomplete. The HOD's seven key features are counted separately because they overlap with the modules.

| HOD module | Status | Evidence and gap |
|---|---|---|
| User Registration & Authentication | Connected | `AuthContext.tsx` calls register/login/me; `auth.routes.ts`, bcrypt and JWT middleware back them. Repair backend compilation; verify role login, logout, and disabled-account behavior. |
| Client Data Management | Partial | `users` stores clients with contact/company fields. Backend lists and creates users; admin client queries exist in `mis.routes.ts`. A live client management screen with updates, deactivation and scoped history is missing. |
| Data Collection | Partial | Quote, task and interaction writes exist, including staff interaction POST and transactional task updates. Routed dashboards lack working collection forms and a live timeline; prove persisted inputs appear after reload. |
| Analytics & Reporting | Partial | `mis.routes.ts` has SQL KPIs, revenue trend, service distribution, service-client and grouped income queries. Report components and exports exist, but the routed role dashboards use static arrays; client engagement and project progress reporting need live datasets. |
| Performance Monitoring | Partial | Manager project health, staff capacity, overdue and project status queries exist. No verified live dashboard or bounded refresh; current health calculation is a deterministic rule. |
| Notification | Missing | No persistent inbox, delivery API, or scheduled reminders found. The profile displays a placeholder. |
| User & Role Management | Partial | Four roles and authorization gates exist. `users.routes.ts` exposes GET/POST for admin/manager; no admin lifecycle UI or mounted user update/deactivate routes there. Confirm who may assign privileged roles. |
| Audit & Logging | Partial | `activity` table and admin read API exist. The write handler consumes request body, and the UI displays sample events. Generate events server-side from authenticated actions and preserve audit history. |

| HOD key feature | Status |
|---|---|
| Centralized client database | Present in schema and backend queries; validate persistence on a clean database. |
| Integrated data collection | Partial: workflow writes exist; dashboard inputs and data flow need connecting. |
| Analytical dashboards and reports | Partial: APIs and export UI exist, but live role dashboards are not wired. |
| Secure role-based access control | Partial: role gates exist; ownership, effective revocation and cross-role access must be tested. |
| Real-time performance monitoring | Partial: snapshot queries exist; automatic refresh and freshness evidence are absent. |
| Automated insights generation | Partial: server derives project health on demand. No persisted actionable insights or generation schedule found. |
| Audit trails | Partial: storage and routes exist; dependable event capture and readout are missing. |

The HOD's objectives and expected outcomes describe what to demonstrate and evaluate. Count them as outcomes only once you have evidence, for example shorter report preparation time in a timed comparison or partner feedback on finding a client history. The brief calls for automated insights, not explicitly AI. Treat AI as an extension unless your supervisor confirms otherwise.

## Thesis requirements Chapter 2

Of the **8 functional requirements: 2 have source evidence of implementation, 5 are partial, 1 is missing**. Of the **7 nonfunctional requirements: 2 have partial evidence and 5 remain unverified**. These are separate denominators from the HOD modules.

| Thesis | Status | What to prove or finish |
|---|---|---|
| REQ 1 Authentication | Implemented in source | Login, session identity, protected route and valid/invalid credentials for all four roles. |
| REQ 2 User lifecycle and permissions | Partial | Add edit, deactivate and authorized role assignment; enforce disabled accounts on existing sessions. |
| REQ 3 Client CRUD and history | Partial | Add scoped history and management; agree whether archive/anonymization satisfies delete without losing project or audit history. |
| REQ 4 Requests/projects/deliverables | Partial | Wire client request, authorized approval, staff/task assignment, dates, status and deliverables. Thesis describes staff approval; the MIS accept route is admin/manager. Confirm responsibility. |
| REQ 5 Centralized operational data | Implemented in source | Nine relational tables and SQL reads/writes; prove clean migration and persistence after restart. |
| REQ 6 Interactive analytics | Partial | Replace static data; add live client engagement, project health and workload with drill-down. |
| REQ 7 Automated/custom reports | Partial | SQL grouped income and service-client reports exist; add project and client activity reports, validated filters and weekly summaries if approved. |
| REQ 8 Communications and alerts | Missing | Persist assignments, project updates and deadline reminders in an inbox. An interaction log is not a messaging inbox; confirm whether two-way messaging is required. |
| REQ 9 Common operations around 5 seconds | Unverified | Record p95 per operation and error count on a defined normal workload. |
| REQ 10 50 concurrent users | Unverified | Run a representative 50-user mixed workload against a disposable deployment; document duration, dataset, p95 and errors. |
| REQ 11 Scalability | Unverified | Explain modular design and validate with a larger seeded dataset plus measured query plans. |
| REQ 12 Backup and recovery | Unverified | Demonstrate backup and restore into a separate database; record restore time and integrity checks. |
| REQ 13 Security | Partial | bcrypt and JWT/RBAC exist. Verify ownership, payment integrity, session handling, and deployment TLS and storage controls. |
| REQ 14 Browser compatibility | Unverified | Test Chrome, Firefox and Edge, and the OS targets stated in the thesis; record versions and results. |
| REQ 15 Maintainability | Partial | Modules, Swagger and TypeScript exist; fix compilation, consolidate overlapping business logic and add setup/test runbooks. |

### What makes the current website look like a prototype

`camis-fn/src/App.tsx` routes the four roles through wrapper components to `src/components/dashboard/RoleDashboard.tsx`. That file builds KPIs, tables and exports from bundled arrays of users, projects, invoices, tasks, interactions and activity. Treat those records as potentially sensitive; remove personal/contact data from the production bundle, use fictional seeded accounts, and make client scoping a backend rule derived from authentication. Existing `src/services/*Service.ts` and the report/task/manager components are useful starting points, but their presence does not mean the active pages use them.

The backend type-check failed on missing bcrypt declarations, optional fields and Express route parameter types. Fix before relying on `dist`; a previously generated build does not establish current reproducibility. Check payment initialization, verification, and webhook against stored obligations and a unique provider transaction before enabling real money. The payment routes expose initialize/verify without auth middleware and verification uses browser-supplied expected values. The current checkout is not evidence of reconciled invoice payments. See the acceptance checks in the [TODO](TODO.md).
