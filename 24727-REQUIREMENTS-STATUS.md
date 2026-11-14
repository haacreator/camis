# 24727 Requirements Status

Source: `24727 requirements.docx`. Assessment based on the source review of `camis-bn` and `camis-fn`. **Satisfied in source** means the named capability is implemented in the reviewed code. **Partial** means part of the capability is implemented. **Missing** means no usable implementation was found. **Unverified** means results need to be measured or observed.

## System modules

**1 satisfied in source · 6 partial · 1 missing**

| Requirement | Status | Evidence and remaining work |
|---|---|---|
| User Registration & Authentication | Satisfied in source | Frontend calls registration, login and account APIs. Backend uses bcrypt and JWT. Verify all role logins and logout on a working build. |
| Client Data Management | Partial | The database stores client contact and company details. A live screen for updating and deactivating clients and viewing their history is missing. |
| Data Collection | Partial | Backend saves quotes, task updates and interaction records. Connect the staff dashboard forms and show saved records in the timeline. |
| Analytics & Reporting | Partial | SQL analytics, reports and export components exist. Active dashboards still use hard-coded records. Connect the reports to live data. |
| Performance Monitoring | Partial | Backend calculates project health, workload and overdue work. Connect the dashboard and verify automatic refresh. |
| Notification | Missing | Add a persistent inbox and alerts for assignments, project updates and deadlines. |
| User & Role Management | Partial | Four roles and access checks exist. Add admin screens and APIs for user updates, deactivation and role changes. |
| Audit & Logging | Partial | The activity table and admin read API exist. Automatically log actions on the server and replace sample entries in the dashboard. |

## Key features

**1 satisfied in source · 6 partial.** These features overlap with the modules above.

| Requirement | Status | Evidence and remaining work |
|---|---|---|
| Centralized client database | Satisfied in source | PostgreSQL stores client accounts and links them to quotes, projects and interactions. Verify persistence on a clean database. |
| Integrated data collection mechanisms | Partial | Operational records can be written by the API. Complete the dashboard forms and timelines. |
| Analytical dashboards and reports | Partial | Backend queries and export components exist. The routed dashboards still use sample data. |
| Secure role-based access control | Partial | Role checks exist. Verify record ownership and the effect of disabling an account or changing its role. |
| Real-time performance monitoring | Partial | The backend can calculate current snapshots. Add live refresh and measure how long changes take to reach the dashboard. |
| Automated insights generation | Partial | The backend calculates project risk on request. Add scheduled or event-driven insights with reasons and links to the underlying records. |
| Audit trails for transparency | Partial | Activity can be stored and read. Generate trusted events for key actions and show them in the admin audit screen. |

## Specific objectives

| Objective | Status | Evidence needed |
|---|---|---|
| Centralize client data collection and management | Partial | Demonstrate one client record with a live, editable profile and linked history. |
| Track client interactions and service engagements | Partial | Demonstrate a quote becoming a project and a recorded interaction appearing in the client's history. |
| Analyze client behavior and business performance metrics | Partial | Show engagement trends and verify the displayed figures against stored records. |
| Generate analytical reports and dashboards | Partial | Export a filtered report from live data and reconcile it with the displayed dashboard. |
| Improve operational efficiency through data-driven insights | Unverified | Measure time to identify overdue work or prepare a report before and after using CAMIS. |
| Support strategic planning and decision-making | Unverified | Record an example of a manager using CAMIS evidence to make a decision. |
| Ensure secure storage and access to client information | Partial | Verify role and record access, account deactivation and deployment security. |

The HOD's expected outcomes also need evaluation evidence. Improved organization, decision-making, understanding of client behavior, efficiency, infrastructure and service delivery or satisfaction remain **unverified**.

The earlier frontend type check passed and the backend type check failed. End-to-end verification is pending. Fix the backend build first, then use the [project TODO](TODO.md) to close and test the gaps.

## CAMIS MIS baseline

These project requirements adapt the MIS functions of operational data capture, management reporting and decision support to Creative Saako. The HOD module totals above apply to the original brief. [OpenStax MIS reference](https://openstax.org/books/introduction-business/pages/13-3-management-information-systems)

| ID | CAMIS shall | Current status | Acceptance check |
|---|---|---|---|
| MIS-01 | Maintain a searchable client and service register with linked quote, project and interaction history. | Partial. Database and list queries exist. | Edit a client, filter by service and reload the saved history. |
| MIS-02 | Manage requests, approvals, assignments, deadlines, task updates and client-visible progress. | Partial. Backend routes exist. | Complete the workflow across client, manager and staff accounts using one project. |
| MIS-03 | Show live management KPIs and let users drill down to contributing records. | Partial. API calculations exist and active dashboards still use samples. | Update a task and verify that progress, workload and overdue figures agree with PostgreSQL after refresh. |
| MIS-04 | Provide routine reports, on-demand filters, and exception views for overdue or at-risk work. | Partial. Income queries, project health and export components exist. | Filter by date and service, export the result and match it to the current dashboard and source totals. |
| MIS-05 | Record billing and balances and show them to authorized users. | Partial. Financial records and client queries exist. | Record an invoice and authorized payment in a test environment, then reconcile due, paid and outstanding amounts. |
| MIS-06 | Enforce role and record permissions, validate inputs, and maintain an audit trail. | Partial. Role gates and activity storage exist. | Verify two clients see their own records, staff access is scoped, and a quote approval is logged with actor and time. |
| MIS-07 | Support reproducible setup, backup and recovery. | Unverified. Schema and migration script exist. | Build on a clean environment, restore a backup to a separate database and verify the sample workflow records. |

## Automation requirements

| ID | CAMIS shall | Current status | Acceptance check |
|---|---|---|---|
| AUTO-01 | Recalculate project progress after a task changes and show the new value to authorized users. | Partial. The staff task API already recalculates progress within its transaction. | Complete one assigned task and verify both the manager and client views show the saved progress. |
| AUTO-02 | Create an in-app notification on task assignment and relevant project status changes. | Missing. | Assign a task and verify one unread notification appears for the correct recipient and can be marked read. |
| AUTO-03 | Scan for deadline exceptions on a schedule and maintain an Attention Needed queue with a reason, owner and source record. | Partial. Project health is calculated on request. | A seeded overdue project appears, repeated scans produce one open item, and completion resolves it. |
| AUTO-04 | Generate a weekly management summary of new requests, completed projects, overdue items and workload. | Missing. | Run the scheduled summary on known data and verify totals, date window and one saved summary per week. |

## AI requirements

These are project additions. The HOD's brief requires automated insights. For CAMIS, AI can explain an existing insight and suggest a follow-up for the manager to review. The requirements below use evaluation and human oversight practices described in the [NIST AI Risk Management Framework](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/).

| ID | CAMIS shall | Current status | Acceptance check |
|---|---|---|---|
| AI-01 | Let authorized managers and admins request a labeled explanation and one suggested action for a project risk alert. | Missing. | Open an overdue alert and verify the explanation agrees with the displayed deadline, task count and source record. |
| AI-02 | Use approved operational fields as evidence and keep any decision or record change under the user's control. | Missing. | Verify that contacts and private notes stay out of the AI request, unauthorized roles are denied and generating an explanation leaves project records unchanged. |
| AI-03 | Show a rule-based explanation when the model is unavailable or its output fails validation. | Missing. | Simulate a timeout and invalid response. The alert remains usable and the UI displays its underlying evidence. |
| AI-04 | Evaluate explanations against a rule-based baseline and record factual agreement, usefulness and response time. | Missing. | Run and document overdue, due soon, completed and missing-data cases using a fictional dataset. |

Build the live role workflow and management reports first. Then demonstrate an overdue item appearing automatically, a manager opening its evidence and AI explanation, and the resolution appearing in the report and audit trail.

## Revised cart and fulfillment requirements

This section records the revised cart scope and supersedes the earlier AI-01 through AI-04 project proposal. The HOD's original requirements still apply.

| ID | Requirement | Status and evidence |
|---|---|---|
| CART-01 | Use Creative Saako's existing catalog and show package prices, deliverables, turnaround and revision limits. | Graphic Design tiers implemented with draft terms. Add-ons and tier details for other service lines are pending. |
| CART-02 | Save a priced order before payment, verify the provider reference, and open the brief only after verification. | Passed in disposable PostgreSQL using a mocked Flutterwave response. Live sandbox checkout remains unverified. |
| CART-03 | Let clients, managers and assigned staff communicate inside an order and attach reference images. | API workflow passed with role-scoped messaging and an image attachment. Full-resolution files, PDFs and videos remain pending. |
| CART-04 | Support brief submission, assignment, reassignment, delivery, revisions and client approval. | Core order actions implemented. Linking paid orders to MIS projects and task templates remains pending. |
| CART-05 | Flag missed deadlines and 48 hours without staff updates, notify management, and track cancellation and refunds according to company policy. | Order flags and reassignment implemented. Scheduled notifications, policy approval and refund processing are pending. |
| AI-CART-01 | Provide a customer chatbot grounded in the actual catalog and approved policies, with customer-scoped order lookup and human handoff. | Pending. Test package prices and policy answers, cross-account access, unsupported questions and handoff. |
| AI-CART-02 | Summarize customer briefs into structured goals, deliverables, constraints, scope flags and questions, then require client confirmation before assignment. | Pending. Test field accuracy, missing information, out-of-scope requests and client corrections. |

See the [cart and order workflow guide](CAMIS-ORDER-WORKFLOW.md) for migration, sandbox testing and delivery limitations.
