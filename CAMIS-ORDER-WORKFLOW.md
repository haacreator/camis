# CAMIS Cart and Order Workflow

Adding a service saves a cart selection in this browser. Checkout creates a pending order using server-side catalog prices. The payment is made to the Creative Saako Flutterwave merchant account. Verified payment marks the order Awaiting Brief and creates one financial record, even if verification is repeated. If the browser misses the return, open Orders and use Check Flutterwave payment.

Run the backend migration against your development database before starting the updated backend. From the backend folder, use `npm run migrate`, then `npm run build` and `npm start`. Apply the migration after importing your existing service catalog so it can seed example Graphic Design packages. Basic, Standard and Premium have draft prices calculated from the company's Graphic Design base price. Review those prices, deliverables, revision limits and turnaround times before using them with customers. Configure matching Flutterwave sandbox keys for the existing frontend and backend settings when testing.

After payment, the client submits a brief. A manager assigns staff and sets a deadline. The assigned staff member posts a photo and submits it for manager review. The manager releases the delivery. The client can approve it or request a revision within the package allowance. For multi-item orders, the current workflow uses one brief and one assignment, with the lowest included revision allowance across the selected packages.

The Orders screen shows missed deadlines and work with no staff update for 48 hours. Managers can reassign the same order without another customer payment. Order participants can message each other and attach PNG, JPEG or WebP photos up to 500 KB. The current conversation displays the most recent 25 messages. Photos in the conversation are visible to the order participants when posted. The review action controls the delivery status.

## Next implementation requirements

- Use a private asset store with authorized downloads for source files, PDFs, videos and full-resolution photos. Separate internal proofs from client-visible deliveries. Track deliverable versions.
- Store order line items independently when each service needs a separate brief, assigned staff member, deadline or revision allowance. Link the order and its line items to projects and task templates so fulfillment feeds the MIS dashboards.
- Add firm-approved add-ons and tier details for the remaining service lines, with prices and scope recorded on each sale.
- Add scheduled reminders and manager notifications. Confirm review time limits and cancellation/refund policy with Creative Saako. Track cancellation, refund request and verified refund as separate events before enabling them.
- Add the two AI features from the revised guide. Ground the storefront chatbot in the real catalog and approved policies and scope order lookups to the signed-in buyer. Add a structured brief summarizer with package scope checks and client confirmation before manager assignment. Customer-to-staff order messaging is already part of the workspace.
- Connect a verified provider webhook or scheduled payment reconciliation, and test a complete Flutterwave sandbox payment before using production keys.

Verification for this change used a disposable PostgreSQL database and mocked provider verification. Backend TypeScript compilation and the frontend production build passed.
