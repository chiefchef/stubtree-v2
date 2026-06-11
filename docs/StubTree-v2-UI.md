# StubTree v2 UI Specification

Version: 1.0 Draft

# 1. Design Philosophy

StubTree should feel:

* Modern
* Fast
* Mobile First
* Professional
* Comparable to Stripe, Shopify, Linear, and modern SaaS platforms

The UI should never feel like legacy ticketing software.

Primary design goals:

* Minimize clicks
* Minimize forms
* Mobile-friendly
* Fast event creation
* Easy ticket purchasing
* Easy QR scanning

---

# 2. Responsive Requirements

Every page must function on:

* Desktop
* Tablet
* Mobile

No desktop-only functionality.

Required mobile workflows:

* Buy Ticket
* View Ticket
* Scan Ticket
* Create Event
* Door Sales
* View Dashboard

---

# 3. Public Homepage

Route:

```text id="49e4xl"
/
```

Sections:

### Header

* StubTree Logo
* Search
* Login
* Create Event

### Hero

* Featured Event Carousel

### Upcoming Events

* Event Cards

### Event Categories

* Concerts
* Festivals
* Community Events
* Sports

### Organizer CTA

Cards:

* Venue
* Promoter
* Artist

### Footer

* About
* Support
* Terms
* Privacy

---

# 4. Event Page

Route:

```text id="afsn0j"
/events/{slug}
```

Sections:

### Poster

Large hero image.

### Event Details

* Name
* Date
* Time
* Venue
* Age Policy

### Description

Formatted content.

### Ticket Types

Cards displaying:

* Name
* Price
* Remaining Inventory

### Promo Code Entry

### Checkout CTA

### Event Updates

### Related Events

---

# 5. Authentication Screens

### Login

* Email
* Password
* Magic Link

### Magic Link Verification

### Forgot Password

### Reset Password

### Create Account

Minimal fields:

* Email

Magic-link-first workflow preferred.

---

# 6. Customer Portal

Route:

```text id="9h1wzs"
/account
```

Sections:

### Upcoming Events

### Past Events

### Tickets

### Orders

### Settings

### Notification Preferences

---

# 7. Ticket Display Screen

Route:

```text id="nsxvwl"
/tickets/{ticketId}
```

Display:

* Event Name
* Ticket Type
* QR Code

Features:

* Swipe between tickets
* Large QR code
* High contrast

Real-time updates:

* Checked In animation
* Auto advance to next ticket

```
```

# 8. Venue Dashboard

Route:

```text
/venues/{venueId}/dashboard
```

Purpose:

Venue-wide operational dashboard.

---

## Dashboard Layout

### Top KPI Row

* Upcoming Events
* Tickets Sold This Month
* Revenue This Month
* Attendance This Month
* Refunds This Month

---

### Upcoming Events Widget

Displays:

* Event Name
* Date
* Tickets Sold
* Revenue

Actions:

* Open Dashboard
* Edit Event
* Sell Tickets

---

### Recent Orders Widget

Displays:

* Customer Name
* Event
* Order Total
* Purchase Date

---

### Marketing Widget

Displays:

* Top Promo Codes
* Top Marketing QR Codes
* Conversion Statistics

---

### Settlement Widget

Displays:

* Pending Settlements
* Finalized Settlements

---

# 9. Event Dashboard

Route:

```text
/events/{eventId}/dashboard
```

Purpose:

Single event command center.

---

## Dashboard Layout

### KPI Row

* Tickets Sold
* Revenue
* Attendance
* Refunds
* Comps

---

### Ticket Breakdown

Cards:

* General Admission
* Rail Chairs
* Tables
* VIP Parking

Displays:

* Sold
* Remaining
* Revenue

---

### Sales Chart

Views:

* Daily
* Weekly
* Monthly

Metrics:

* Revenue
* Ticket Count

---

### Attendance Chart

Displays:

* Checked In
* Remaining
* Attendance Percentage

---

### Promo Code Widget

Displays:

* Code
* Uses
* Revenue

---

### Marketing QR Widget

Displays:

* QR Name
* Scans
* Purchases
* Revenue

---

### Documents Widget

Displays:

* Contracts
* Posters
* Spreadsheets
* Uploaded Files

---

### Artist Widget

Displays:

* Artists
* Billing Order
* Deal Type

---

# 10. Artist Dashboard

Route:

```text
/artists/{artistId}/dashboard
```

Purpose:

Artist-facing event visibility.

Visibility controlled by permissions.

---

## Dashboard Layout

### KPI Row

* Tickets Sold
* Revenue
* Attendance
* Promo Code Revenue

---

### Events Widget

Displays:

* Upcoming Events
* Past Events

---

### Promo Code Widget

Displays:

* Promo Code Performance
* Revenue
* Ticket Count

---

### Marketing Widget

Displays:

* Marketing QR Performance

---

### Documents Widget

Displays:

* Contracts
* W9
* 1099

---

# 11. Promoter Dashboard

Route:

```text
/promoters/{promoterId}/dashboard
```

Purpose:

Multi-event management.

---

## Dashboard Layout

### KPI Row

* Events
* Revenue
* Attendance
* Marketing Performance

---

### Event List

Displays:

* Event Name
* Date
* Revenue
* Attendance

---

### Marketing Widget

Displays:

* Promo Codes
* Marketing QR Performance

---

### Settlement Widget

Displays:

* Estimated Settlements
* Finalized Settlements

---

# 12. Admin Dashboard

Route:

```text
/admin
```

Purpose:

Platform administration.

---

## Dashboard Layout

### KPI Row

* Total Revenue
* Tickets Sold
* Active Events
* Active Organizations

---

### Approval Queue

Displays:

* Pending Event Approvals
* Pending Organization Approvals

---

### Risk Monitoring

Displays:

* Refund Alerts
* Chargeback Alerts
* Suspicious Activity

---

### Support Queue

Displays:

* Open Tickets
* Assigned Tickets

---

### Email Health

Displays:

* Failed Emails
* Failed SMS

---

### Audit Log Viewer

Displays:

* User
* Action
* Entity
* Timestamp

# 13. Event Creation Wizard

Route:

```text id="7n9wwo"
/events/create
```

Purpose:

Allow a first-time organizer to create and publish an event in less than 10 minutes.

---

## Step 1 - Event Basics

Fields:

* Event Name
* Venue
* Event Date
* Doors Time
* Start Time
* End Time
* Age Policy

Actions:

* Save Draft
* Continue

---

## Step 2 - Artists

Features:

* Add Existing Artist
* Create New Artist
* Billing Order
* Artist Permissions

Actions:

* Back
* Continue

---

## Step 3 - Media

Features:

* Poster Upload
* Additional Images
* Video Links

Fields:

* Event Description

Actions:

* Back
* Continue

---

## Step 4 - Tickets

Features:

* Apply Venue Template
* Create Custom Ticket Type

Fields:

* Price
* Day Of Show Price
* Inventory
* Sales Start
* Sales End

Actions:

* Back
* Continue

---

## Step 5 - Marketing

Fields:

* Facebook Event URL
* Promo Codes
* Marketing QR Codes

Actions:

* Back
* Continue

---

## Step 6 - Documents

Upload:

* Contracts
* PDFs
* Images
* Spreadsheets

Actions:

* Back
* Continue

---

## Step 7 - Review

Displays:

* Event Summary
* Ticket Summary
* Artist Summary

Actions:

* Edit
* Publish

---

# 14. Scanner UI

Route:

```text id="ywz5pi"
/scanner
```

Purpose:

Fast ticket validation.

Optimized for:

* Mobile
* Tablet

---

## Scan Screen

Large Camera View

Displays:

* QR Overlay
* Flashlight Toggle
* Camera Selector

---

## Valid Ticket Screen

Displays:

* Green Success State
* Ticket Type
* Event Name
* Check-In Time

---

## Invalid Ticket Screen

Displays:

* Red Error State
* Error Message

Examples:

* Refunded Ticket
* Duplicate Scan
* Wrong Checkpoint

---

## Manual Lookup

Search:

* Name
* Email
* Phone

---

# 15. Door Sales UI

Route:

```text id="d5oqwv"
/door-sales
```

Purpose:

Sell tickets at the venue.

---

## Ticket Selection

Displays:

* Ticket Types
* Current Prices
* Inventory

---

## Cart

Displays:

* Selected Tickets
* Quantity
* Totals

---

## Payment Options

Buttons:

* Cash
* Stripe Reader
* External

---

## Customer Information

Optional:

* Name
* Email
* Phone

---

## Sale Complete

Displays:

* Receipt
* Ticket Delivery Status
* QR Generation Status

---

# 16. Guest List UI

Route:

```text id="7e64g4"
/events/{eventId}/guest-list
```

Purpose:

Replace paper guest lists.

---

## Guest List Grid

Columns:

* Name
* Quantity
* Source
* Status

---

## Add Guest

Fields:

* Name
* Quantity
* Source
* Notes

---

## Check In Guest

Actions:

* Scan QR
* Manual Check-In

---

# 17. Marketing UI

Route:

```text id="p7ixt4"
/events/{eventId}/marketing
```

Purpose:

Manage promo codes and marketing attribution.

---

## Promo Codes

Displays:

* Code
* Uses
* Revenue

Actions:

* Create
* Edit
* Disable

---

## Marketing QR Codes

Displays:

* Name
* Scans
* Purchases
* Revenue

Actions:

* Create QR
* Download QR
* Disable QR

---

## Campaigns

Displays:

* Campaign Name
* Budget
* Revenue
* ROI

---

# 18. Settlement UI

Route:

```text id="a57f6e"
/events/{eventId}/settlement
```

Purpose:

Event reconciliation.

---

## Revenue Section

Displays:

* Online Sales
* Door Cash
* Door Card
* External Sales

---

## Expense Section

Displays:

* Refunds
* Discounts
* Fees
* Taxes

---

## Artist Section

Displays:

* Guarantees
* Bonuses
* Promo Revenue

---

## Settlement Summary

Displays:

* Gross Revenue
* Net Revenue
* Estimated Settlement

Actions:

* Finalize Settlement
* Export

```:::
```

# 19. Mobile Experience

Mobile is a first-class platform.

The following workflows must be fully functional on mobile devices:

* Create Event
* Edit Event
* Buy Tickets
* Display Tickets
* Scan Tickets
* Door Sales
* View Dashboards
* Process Refunds
* Manage Guest Lists

No functionality should require a desktop browser.

---

## Mobile Ticket Display

Requirements:

* Large QR Code
* Maximum screen brightness option
* Swipe between tickets
* Real-time check-in updates

Primary use case:

Customer standing in line entering an event.

---

## Mobile Scanner

Requirements:

* One-handed operation
* Fast scan response
* Audible feedback
* Visual feedback

Response time goal:

Less than 1 second.

---

## Mobile Door Sales

Requirements:

* Fast ticket selection
* Stripe Reader integration
* Cash sales support

Primary use case:

High-volume entry lines.

---

# 20. Homepage Experience

Route:

```text
/
```

Purpose:

Sell tickets and attract new organizers.

---

## Hero Section

Displays:

* Featured Events

Primary CTA:

```text
Create Event
```

Secondary CTA:

```text
Browse Events
```

---

## Featured Events

Admin-controlled.

Displays:

* Poster
* Event Name
* Date
* Venue

---

## Upcoming Events

Displays:

* Event Cards
* Search
* Filters

---

## Organizer Section

Three cards:

### Venues

Create events and manage operations.

### Promoters

Create and market events.

### Artists

Sell tickets directly to fans.

---

## Footer

Links:

* About
* Support
* Terms
* Privacy

---

# 21. Approval Workflow

Purpose:

Prevent abuse while maintaining simple onboarding.

---

## New Organizer Flow

New organizer creates account.

Organizer creates event.

Event enters:

```text
Pending Approval
```

State.

StubTree Admin reviews.

Actions:

* Approve
* Reject
* Request Information

---

## Approved Organizer

Future events may bypass approval.

Approval rules configurable.

---

# 22. White Label Roadmap

Not required for v2.

Architecture must support future implementation.

---

## White Label Features

### Branding

* Logo
* Colors
* Fonts

### Domains

Examples:

```text
josabis.stubtree.com

tickets.josabis.com
```

### Fees

Organization-specific fee structures.

### Stripe

Organization-specific Stripe accounts.

---

# 23. Non-Functional Requirements

## Performance

Page loads:

* Under 2 seconds

API responses:

* Under 500ms typical

Scanner validation:

* Under 1 second

---

## Security

Requirements:

* JWT Authentication
* Refresh Tokens
* HTTPS Only
* Password Hashing
* Audit Logging

No credit card data stored.

---

## Reliability

Requirements:

* Retryable email delivery
* Retryable SMS delivery
* Retryable refund processing

---

## Scalability

Initial target:

* Josabi's

Future target:

* Multiple Venues
* Multiple Promoters
* Multiple Artists

Architecture should support SaaS growth without major redesign.

---

# 24. Initial Release Scope

Included:

* Customer Accounts
* Magic Links
* Event Management
* Ticket Sales
* Stripe Checkout
* QR Tickets
* Scanner
* Door Sales
* Promo Codes
* Marketing QR Codes
* Reporting
* Settlements
* Refunds
* Guest Lists
* Artist W9 Management

Not Included:

* Ticket Transfers
* Ticket Resale
* Connected Stripe Accounts
* White Label Branding
* Venue Custom Domains
* Offline Scanner Mode
* Automated Artist Payouts

---

# 25. Success Criteria

A new organizer can:

1. Create Account
2. Create Event
3. Upload Poster
4. Configure Tickets
5. Publish Event
6. Sell Tickets

In under 10 minutes.

A customer can:

1. Discover Event
2. Purchase Ticket
3. Receive Ticket
4. Enter Event

In under 3 minutes.

The platform should provide a better user experience than Eventbrite while maintaining lower fees and greater flexibility.

# 26. Design System

## Color Philosophy

Primary goals:

* High contrast
* Accessible
* Professional
* Event-focused

Avoid:

* Heavy gradients
* Excessive animations
* Cluttered dashboards

---

## Typography

Primary Font:

```text id="l52hwu"
Inter
```

Fallback:

```text id="3gxrtm"
System Sans
```

Guidelines:

* Large headings
* Readable body text
* Mobile-friendly sizing

---

## Buttons

Primary Button:

Used for:

* Buy Tickets
* Publish Event
* Checkout
* Save

Secondary Button:

Used for:

* Cancel
* Back
* View Details

Danger Button:

Used for:

* Refund
* Delete
* Void Ticket

---

## Cards

Used for:

* Events
* Tickets
* Dashboard Widgets
* Reports

All cards should support:

* Mobile layouts
* Hover states (desktop)
* Touch interactions (mobile)

---

## Tables

Used for:

* Orders
* Refunds
* Guest Lists
* Settlements

Requirements:

* Sorting
* Filtering
* Pagination

Mobile:

* Convert to stacked layout

---

## Forms

Requirements:

* Inline validation
* Mobile-friendly inputs
* Autosave where appropriate

---

# 27. Navigation Structure

## Public Navigation

```text id="m6kg0j"
Home
Events
Create Event
Login
```

---

## Customer Navigation

```text id="6b2kh3"
Dashboard
Tickets
Orders
Settings
Logout
```

---

## Organizer Navigation

```text id="9z7drq"
Dashboard
Events
Marketing
Reports
Settlements
Users
Settings
```

---

## Admin Navigation

```text id="49qv9f"
Dashboard
Approvals
Organizations
Events
Support
Risk Alerts
Audit Logs
System Health
```

---

# 28. Release Milestones

## Phase 1

Foundation

* Authentication
* Organizations
* Events
* Ticket Types
* PostgreSQL Schema

---

## Phase 2

Ticketing

* Cart
* Checkout
* Stripe
* Orders
* Tickets

---

## Phase 3

Operations

* Scanner
* Door Sales
* Guest Lists
* Refunds

---

## Phase 4

Reporting

* Venue Dashboard
* Event Dashboard
* Artist Dashboard
* Promoter Dashboard

---

## Phase 5

Growth Features

* Marketing QR Codes
* Promo Codes
* Advanced Reporting
* White Label Foundation

---

# 29. Open Future Features

Future backlog:

* Ticket Transfers
* Ticket Marketplace
* Reserved Seating
* SMS Marketing
* Email Campaign Builder
* Automated Artist Payouts
* Connected Stripe Accounts
* Venue Custom Domains
* Mobile App
* Offline Scanning
* AI Marketing Assistant
* AI Event Creation Assistant

---

# 30. PRD Completion Status

This document represents the approved foundation for StubTree v2.

The PRD, Database Schema, API Specification, and UI Specification collectively serve as the source of truth for implementation.

Changes to business logic should be reflected in these documents before implementation.
