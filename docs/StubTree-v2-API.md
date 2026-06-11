# StubTree v2 API Specification

Version: 1.0 Draft

## API Standards

Base URL:

```text
/api/v1
```

Authentication:

* JWT Access Token
* Refresh Token
* Magic Link Authentication

Response Format:

```json
{
  "success": true,
  "message": "",
  "data": {}
}
```

Error Format:

```json
{
  "success": false,
  "message": "Validation failed",
  "errors": []
}
```

---

# 1. Authentication Endpoints

## Request Magic Link

POST

```text
/api/v1/auth/magic-link
```

Request:

```json
{
  "email": "user@example.com"
}
```

Response:

```json
{
  "success": true
}
```

---

## Verify Magic Link

POST

```text
/api/v1/auth/verify-magic-link
```

Request:

```json
{
  "token": "..."
}
```

Response:

```json
{
  "accessToken": "...",
  "refreshToken": "..."
}
```

---

## Login

POST

```text
/api/v1/auth/login
```

Request:

```json
{
  "email": "",
  "password": ""
}
```

Response:

```json
{
  "accessToken": "...",
  "refreshToken": "..."
}
```

---

## Refresh Token

POST

```text
/api/v1/auth/refresh
```

---

## Logout

POST

```text
/api/v1/auth/logout
```

---

## Get Current User

GET

```text
/api/v1/auth/me
```

# 2. Customer Endpoints

## Get Customer Profile

GET

```text
/api/v1/customers/me
```

Response:

```json
{
  "id": "",
  "email": "",
  "firstName": "",
  "lastName": "",
  "phone": ""
}
```

---

## Update Customer Profile

PUT

```text
/api/v1/customers/me
```

Request:

```json
{
  "firstName": "",
  "lastName": "",
  "phone": ""
}
```

---

## Get Customer Orders

GET

```text
/api/v1/customers/me/orders
```

Query Parameters:

```text
page
pageSize
status
```

---

## Get Customer Order

GET

```text
/api/v1/customers/me/orders/{orderId}
```

---

## Get Customer Tickets

GET

```text
/api/v1/customers/me/tickets
```

Query Parameters:

```text
eventId
status
```

---

## Get Ticket

GET

```text
/api/v1/customers/me/tickets/{ticketId}
```

---

## Resend Order Confirmation

POST

```text
/api/v1/customers/me/orders/{orderId}/resend
```

---

## Resend Ticket Email

POST

```text
/api/v1/customers/me/tickets/{ticketId}/resend
```

---

## Get Customer Notifications

GET

```text
/api/v1/customers/me/notifications
```

---

## Update Customer Preferences

PUT

```text
/api/v1/customers/me/preferences
```

Request:

```json
{
  "allowEmail": true,
  "allowSms": false,
  "allowMarketing": true
}
```

---

# 3. Public Event Endpoints

## Get Featured Events

GET

```text
/api/v1/events/featured
```

---

## Search Events

GET

```text
/api/v1/events
```

Query Parameters:

```text
search
category
venueId
city
state
startDate
endDate
page
pageSize
```

---

## Get Event By Slug

GET

```text
/api/v1/events/{slug}
```

Example:

```text
/api/v1/events/toadies-2026-08-22
```

---

## Get Event Ticket Types

GET

```text
/api/v1/events/{eventId}/ticket-types
```

---

## Join Event Waitlist

POST

```text
/api/v1/events/{eventId}/waitlist
```

Request:

```json
{
  "ticketTypeId": "",
  "name": "",
  "email": "",
  "phone": "",
  "quantityRequested": 2
}
```

---

## Track Marketing QR Visit

POST

```text
/api/v1/marketing/qr/{slug}
```

---

## Validate Promo Code

POST

```text
/api/v1/promo-codes/validate
```

Request:

```json
{
  "eventId": "",
  "code": ""
}
```

Response:

```json
{
  "valid": true,
  "discountType": "percentage",
  "discountValue": 10
}
```

# 4. Cart & Checkout Endpoints

## Get Active Cart

GET

```text
/api/v1/cart
```

---

## Create Cart

POST

```text
/api/v1/cart
```

Request:

```json
{
  "eventId": ""
}
```

---

## Get Cart

GET

```text
/api/v1/cart/{cartId}
```

---

## Add Cart Item

POST

```text
/api/v1/cart/{cartId}/items
```

Request:

```json
{
  "ticketTypeId": "",
  "quantity": 2
}
```

---

## Update Cart Item

PUT

```text
/api/v1/cart/{cartId}/items/{cartItemId}
```

Request:

```json
{
  "quantity": 4
}
```

---

## Remove Cart Item

DELETE

```text
/api/v1/cart/{cartId}/items/{cartItemId}
```

---

## Apply Promo Code

POST

```text
/api/v1/cart/{cartId}/promo-code
```

Request:

```json
{
  "code": "TOADIES"
}
```

---

## Remove Promo Code

DELETE

```text
/api/v1/cart/{cartId}/promo-code
```

---

## Get Cart Pricing

GET

```text
/api/v1/cart/{cartId}/pricing
```

Response:

```json
{
  "baseTicketTotal": 100.00,
  "stubtreeFeeTotal": 8.00,
  "cardFeeTotal": 4.20,
  "taxTotal": 9.26,
  "discountTotal": 10.00,
  "grandTotal": 111.46
}
```

---

## Create Checkout Session

POST

```text
/api/v1/checkout/create-session
```

Request:

```json
{
  "cartId": ""
}
```

Response:

```json
{
  "orderId": "",
  "checkoutUrl": ""
}
```

---

## Get Order

GET

```text
/api/v1/orders/{orderId}
```

---

## Get Order Tickets

GET

```text
/api/v1/orders/{orderId}/tickets
```

---

# 5. Stripe Endpoints

## Stripe Webhook

POST

```text
/api/v1/stripe/webhook
```

Events:

* checkout.session.completed
* payment_intent.succeeded
* payment_intent.payment_failed
* charge.refunded

---

## Get Checkout Session

GET

```text
/api/v1/stripe/sessions/{sessionId}
```

---

## Get Payment

GET

```text
/api/v1/payments/{paymentId}
```

---

## Get Order Payment

GET

```text
/api/v1/orders/{orderId}/payment
```

---

# 6. Ticket Endpoints

## Get Ticket By Token

GET

```text
/api/v1/tickets/token/{ticketToken}
```

Example:

```text
/api/v1/tickets/token/ABCD1234EFGH5678
```

---

## Get Ticket QR

GET

```text
/api/v1/tickets/{ticketId}/qr
```

---

## Get Ticket History

GET

```text
/api/v1/tickets/{ticketId}/history
```

---

## Void Ticket

POST

```text
/api/v1/tickets/{ticketId}/void
```

---

## Restore Ticket

POST

```text
/api/v1/tickets/{ticketId}/restore
```

---

# 7. Scanner Endpoints

## Validate Ticket

POST

```text
/api/v1/scanner/validate
```

Request:

```json
{
  "ticketToken": "",
  "checkpointId": ""
}
```

Response:

```json
{
  "valid": true,
  "ticketStatus": "active",
  "scanResult": "valid"
}
```

---

## Check In Ticket

POST

```text
/api/v1/scanner/check-in
```

Request:

```json
{
  "ticketToken": "",
  "checkpointId": ""
}
```

---

## Undo Check In

POST

```text
/api/v1/scanner/undo-check-in
```

Request:

```json
{
  "ticketId": ""
}
```

---

## Get Checkpoints

GET

```text
/api/v1/scanner/checkpoints
```

---

## Get Scan History

GET

```text
/api/v1/scanner/scans
```

Query Parameters:

```text
eventId
checkpointId
ticketId
page
pageSize
```

# 8. Venue Management Endpoints

## Get Venue Dashboard

GET

```text id="tt8xik"
/api/v1/venues/{venueId}/dashboard
```

---

## Get Venue Users

GET

```text id="yfcvsz"
/api/v1/venues/{venueId}/users
```

---

## Create Venue User

POST

```text id="sz8s5o"
/api/v1/venues/{venueId}/users
```

Request:

```json id="1e4r5z"
{
  "email": "",
  "firstName": "",
  "lastName": "",
  "roles": [
    "door_staff"
  ]
}
```

---

## Update Venue User

PUT

```text id="muhzkr"
/api/v1/venues/{venueId}/users/{userId}
```

---

## Disable Venue User

POST

```text id="0djlwm"
/api/v1/venues/{venueId}/users/{userId}/disable
```

---

## Get Venue Ticket Templates

GET

```text id="3k77or"
/api/v1/venues/{venueId}/ticket-templates
```

---

## Create Ticket Template

POST

```text id="kvtks6"
/api/v1/venues/{venueId}/ticket-templates
```

---

## Update Ticket Template

PUT

```text id="2f7t7p"
/api/v1/ticket-templates/{ticketTemplateId}
```

---

## Delete Ticket Template

DELETE

```text id="l0nk2w"
/api/v1/ticket-templates/{ticketTemplateId}
```

---

# 9. Event Management Endpoints

## Create Event

POST

```text id="xq1wly"
/api/v1/events
```

---

## Get Event

GET

```text id="72k1v0"
/api/v1/events/{eventId}
```

---

## Update Event

PUT

```text id="b2jz8n"
/api/v1/events/{eventId}
```

---

## Publish Event

POST

```text id="2fy0zn"
/api/v1/events/{eventId}/publish
```

---

## Archive Event

POST

```text id="4ozhn4"
/api/v1/events/{eventId}/archive
```

---

## Add Artist To Event

POST

```text id="dwj42g"
/api/v1/events/{eventId}/artists
```

---

## Remove Artist From Event

DELETE

```text id="j91kg5"
/api/v1/events/{eventId}/artists/{artistId}
```

---

## Add Promoter To Event

POST

```text id="0v0m9j"
/api/v1/events/{eventId}/promoters
```

---

## Add Event User

POST

```text id="b2igyu"
/api/v1/events/{eventId}/users
```

---

## Update Event User Permissions

PUT

```text id="6mukm8"
/api/v1/events/{eventId}/users/{userId}/permissions
```

---

## Upload Event Document

POST

```text id="hht2vz"
/api/v1/events/{eventId}/documents
```

---

## Delete Event Document

DELETE

```text id="9gqog4"
/api/v1/events/{eventId}/documents/{documentId}
```

---

## Create Event Update

POST

```text id="mw8i7d"
/api/v1/events/{eventId}/updates
```

---

## Get Event Updates

GET

```text id="jtvdly"
/api/v1/events/{eventId}/updates
```

---

## Get Event Dashboard

GET

```text id="tm56m0"
/api/v1/events/{eventId}/dashboard
```

---

# 10. Artist Endpoints

## Create Artist

POST

```text id="h91omx"
/api/v1/artists
```

---

## Update Artist

PUT

```text id="t9dw3f"
/api/v1/artists/{artistId}
```

---

## Get Artist

GET

```text id="7sjjko"
/api/v1/artists/{artistId}
```

---

## Get Artist Dashboard

GET

```text id="mlh2wv"
/api/v1/artists/{artistId}/dashboard
```

---

## Upload W9

POST

```text id="0qkzk4"
/api/v1/artists/{artistId}/w9
```

---

## Upload Tax Document

POST

```text id="vq61gm"
/api/v1/artists/{artistId}/tax-documents
```

---

## Download 1099

GET

```text id="jlwmw0"
/api/v1/artists/{artistId}/1099/{taxYear}
```

---

## Get Artist Events

GET

```text id="ih6x9h"
/api/v1/artists/{artistId}/events
```

# 11. Marketing Endpoints

## Create Promo Code

POST

```text id="qx1x2f"
/api/v1/events/{eventId}/promo-codes
```

---

## Update Promo Code

PUT

```text id="8vw5jl"
/api/v1/promo-codes/{promoCodeId}
```

---

## Delete Promo Code

DELETE

```text id="u3whi0"
/api/v1/promo-codes/{promoCodeId}
```

---

## Get Promo Codes

GET

```text id="h0mdr8"
/api/v1/events/{eventId}/promo-codes
```

---

## Get Promo Code Reporting

GET

```text id="iijfrn"
/api/v1/promo-codes/{promoCodeId}/reporting
```

---

## Create Marketing Campaign

POST

```text id="b2k3jb"
/api/v1/events/{eventId}/marketing-campaigns
```

---

## Update Marketing Campaign

PUT

```text id="fwmw0t"
/api/v1/marketing-campaigns/{campaignId}
```

---

## Get Marketing Campaigns

GET

```text id="9dyknd"
/api/v1/events/{eventId}/marketing-campaigns
```

---

## Create Marketing QR Code

POST

```text id="ozf8x0"
/api/v1/events/{eventId}/marketing-qr-codes
```

---

## Update Marketing QR Code

PUT

```text id="84ecgh"
/api/v1/marketing-qr-codes/{marketingQrCodeId}
```

---

## Delete Marketing QR Code

DELETE

```text id="7e8n4x"
/api/v1/marketing-qr-codes/{marketingQrCodeId}
```

---

## Get Marketing QR Codes

GET

```text id="1c20pb"
/api/v1/events/{eventId}/marketing-qr-codes
```

---

## Get Marketing QR Reporting

GET

```text id="l4jj17"
/api/v1/marketing-qr-codes/{marketingQrCodeId}/reporting
```

---

# 12. Refund Endpoints

## Create Full Refund

POST

```text id="0rujdz"
/api/v1/refunds/full
```

---

## Create Partial Refund

POST

```text id="fx3fqo"
/api/v1/refunds/partial
```

---

## Create Custom Refund

POST

```text id="l61x7f"
/api/v1/refunds/custom
```

---

## Get Refund

GET

```text id="e0a0fh"
/api/v1/refunds/{refundId}
```

---

## Get Event Refunds

GET

```text id="22m8zl"
/api/v1/events/{eventId}/refunds
```

---

## Create Refund Batch

POST

```text id="7w5g35"
/api/v1/events/{eventId}/refund-batches
```

---

## Get Refund Batch

GET

```text id="5nn1zq"
/api/v1/refund-batches/{refundBatchId}
```

---

## Get Refund Batch Items

GET

```text id="gkl7nm"
/api/v1/refund-batches/{refundBatchId}/items
```

---

## Execute Refund Batch

POST

```text id="cfn0rf"
/api/v1/refund-batches/{refundBatchId}/execute
```

---

# 13. Guest List Endpoints

## Get Guest List

GET

```text id="m8k0kt"
/api/v1/events/{eventId}/guest-list
```

---

## Create Guest List Entry

POST

```text id="wvjlwm"
/api/v1/events/{eventId}/guest-list
```

---

## Update Guest List Entry

PUT

```text id="u63pd8"
/api/v1/guest-list/{guestListEntryId}
```

---

## Delete Guest List Entry

DELETE

```text id="vgsq53"
/api/v1/guest-list/{guestListEntryId}
```

---

## Generate Comp Ticket

POST

```text id="j96s6j"
/api/v1/guest-list/{guestListEntryId}/generate-ticket
```

---

## Check In Guest

POST

```text id="fksg3z"
/api/v1/guest-list/{guestListEntryId}/check-in
```

---

# 14. Support Endpoints

## Create Support Request

POST

```text id="3ylkr6"
/api/v1/support
```

---

## Get Support Request

GET

```text id="f6fjlwm"
/api/v1/support/{supportRequestId}
```

---

## Get Support Requests

GET

```text id="x6s0wf"
/api/v1/support
```

---

## Assign Support Request

POST

```text id="p7ng9v"
/api/v1/support/{supportRequestId}/assign
```

---

## Resolve Support Request

POST

```text id="e25v7g"
/api/v1/support/{supportRequestId}/resolve
```

---

## Close Support Request

POST

```text id="90hjlwm"
/api/v1/support/{supportRequestId}/close
```

# 15. Reporting Endpoints

## Venue Dashboard

GET

```text
/api/v1/venues/{venueId}/dashboard
```

---

## Venue Revenue Report

GET

```text
/api/v1/venues/{venueId}/reports/revenue
```

---

## Venue Attendance Report

GET

```text
/api/v1/venues/{venueId}/reports/attendance
```

---

## Venue Marketing Report

GET

```text
/api/v1/venues/{venueId}/reports/marketing
```

---

## Event Sales Report

GET

```text
/api/v1/events/{eventId}/reports/sales
```

---

## Event Attendance Report

GET

```text
/api/v1/events/{eventId}/reports/attendance
```

---

## Event Marketing Report

GET

```text
/api/v1/events/{eventId}/reports/marketing
```

---

## Event Settlement Report

GET

```text
/api/v1/events/{eventId}/reports/settlement
```

---

## Artist Dashboard

GET

```text
/api/v1/artists/{artistId}/dashboard
```

---

## Artist Revenue Report

GET

```text
/api/v1/artists/{artistId}/reports/revenue
```

---

## Artist Promo Code Report

GET

```text
/api/v1/artists/{artistId}/reports/promo-codes
```

---

## Artist Attendance Report

GET

```text
/api/v1/artists/{artistId}/reports/attendance
```

---

## Promoter Dashboard

GET

```text
/api/v1/promoters/{promoterId}/dashboard
```

---

## Promoter Revenue Report

GET

```text
/api/v1/promoters/{promoterId}/reports/revenue
```

---

## Promoter Marketing Report

GET

```text
/api/v1/promoters/{promoterId}/reports/marketing
```

---

# 16. Settlement Endpoints

## Get Event Settlement

GET

```text
/api/v1/events/{eventId}/settlement
```

---

## Create Settlement

POST

```text
/api/v1/events/{eventId}/settlement
```

---

## Update Settlement

PUT

```text
/api/v1/settlements/{settlementId}
```

---

## Finalize Settlement

POST

```text
/api/v1/settlements/{settlementId}/finalize
```

---

## Get Settlement Line Items

GET

```text
/api/v1/settlements/{settlementId}/line-items
```

---

## Create Settlement Line Item

POST

```text
/api/v1/settlements/{settlementId}/line-items
```

---

## Update Settlement Line Item

PUT

```text
/api/v1/settlement-line-items/{lineItemId}
```

---

## Delete Settlement Line Item

DELETE

```text
/api/v1/settlement-line-items/{lineItemId}
```

---

# 17. File Endpoints

## Upload File

POST

```text
/api/v1/files
```

---

## Get File

GET

```text
/api/v1/files/{fileId}
```

---

## Delete File

DELETE

```text
/api/v1/files/{fileId}
```

---

## Download File

GET

```text
/api/v1/files/{fileId}/download
```

---

## Upload Event Document

POST

```text
/api/v1/events/{eventId}/documents
```

---

## Upload Artist Tax Document

POST

```text
/api/v1/artists/{artistId}/documents
```

---

# 18. Admin Endpoints

## Admin Dashboard

GET

```text
/api/v1/admin/dashboard
```

---

## Get Pending Event Approvals

GET

```text
/api/v1/admin/event-approvals
```

---

## Approve Event

POST

```text
/api/v1/admin/event-approvals/{approvalId}/approve
```

---

## Reject Event

POST

```text
/api/v1/admin/event-approvals/{approvalId}/reject
```

---

## Get Risk Alerts

GET

```text
/api/v1/admin/risk-alerts
```

---

## Resolve Risk Alert

POST

```text
/api/v1/admin/risk-alerts/{riskAlertId}/resolve
```

---

## Get Audit Logs

GET

```text
/api/v1/admin/audit-logs
```

---

## Get Platform Statistics

GET

```text
/api/v1/admin/platform-statistics
```

---

## Get Failed Emails

GET

```text
/api/v1/admin/emails/failed
```

---

## Get Failed SMS

GET

```text
/api/v1/admin/sms/failed
```

---

# 19. Health & System Endpoints

## Health Check

GET

```text
/api/v1/health
```

---

## Version

GET

```text
/api/v1/version
```

---

## Current User Permissions

GET

```text
/api/v1/me/permissions
```

---

## Current User Organizations

GET

```text
/api/v1/me/organizations
```

---

## Current User Events

GET

```text
/api/v1/me/events
```
