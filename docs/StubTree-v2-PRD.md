# StubTree v2 Product Requirements Document

Version: 1.0 (Draft)

## Executive Summary

StubTree v2 is a complete rebuild of the StubTree ticketing platform.

The goal is to create a modern, mobile-first ticketing and event management platform focused on live entertainment while maintaining enough flexibility to support additional event types in the future.

The initial production customer is Josabi's in Helotes, Texas.

The platform must support:

- Venues
- Promoters
- Bands / Artists
- Customers

StubTree should provide:

- Online ticket sales
- Stripe-hosted checkout
- QR ticketing
- Door sales
- Parking management
- Customer accounts
- Reporting
- Settlements
- Marketing tools
- Event management

The long-term vision is to become a SaaS platform that is easier to use than Eventbrite while offering lower fees and more flexibility.

## Product Philosophy

StubTree is built by event organizers for event organizers.

The platform should feel:

- Fast
- Modern
- Simple
- Mobile-first
- Professional

A first-time user should be able to create and publish an event in under 10 minutes.

StubTree should differentiate itself from competitors through:

- Lower fees
- Better user experience
- Better support
- Faster feature development
- Modern reporting

## Initial Market Focus

Primary Market:

- Concerts
- Music Festivals
- Tribute Bands
- Original Bands
- Venue Events
- Promoter Events

Future Market:

- Sports
- Community Events
- Classes
- Workshops
- Fundraisers

## Technology Stack

Frontend:
- React
- Next.js
- TypeScript
- Tailwind CSS
- shadcn/ui

Backend:
- .NET 9
- ASP.NET Core
- Entity Framework Core

Database:
- PostgreSQL

Payments:
- Stripe Checkout Sessions
- Stripe Terminal / Stripe Reader

Hosting:
- Railway

Storage:
- Railway Storage Buckets

Realtime:
- SignalR

## Organization Model

Events are owned by Organizations.

Organization Types:

### Venue

Examples:

* Josabi's
* The Hall
* Sam's Burger Joint

Capabilities:

* Create events
* Manage staff
* Manage door operations
* Manage parking operations
* Manage reporting
* Manage settlements
* Manage users

Default StubTree Fee:

$2.00 per ticket

---

### Promoter

Examples:

* Texas Sound & Events

Capabilities:

* Create events
* Link venues
* Link artists
* Create promo codes
* Access promoter reporting
* Manage marketing

Default StubTree Fee:

$2.00 per ticket

---

### Artist / Band

Examples:

* San & Tone Pilots
* FooVana
* Toadies

Capabilities:

* Create events
* Sell tickets
* Create promo codes
* Access artist dashboards
* Upload W9 documents
* Access assigned event reporting

Default StubTree Fee:

$1.00 per ticket

Bands receive a lower fee because they are the primary growth channel for the platform.

---

## Event Ownership

Events are owned by an Organization.

An event may be owned by:

* Venue
* Promoter
* Artist

Examples:

Josabi's creates an event:

* Organization Type = Venue

Texas Sound & Events creates an event:

* Organization Type = Promoter

San & Tone Pilots creates an event:

* Organization Type = Artist

The owner controls the event.

The owner may invite other users and organizations to participate.

---

## Venue Creation

Venues may exist independently of events.

Examples:

* Josabi's
* The Hall
* Sam's Burger Joint

A promoter or artist may create a temporary venue if the venue does not already exist.

Venue records should support:

* Name
* Address
* Website
* Capacity
* Notes
* Verification Status

---

## Artists

Artists are first-class entities.

Artists may:

* Appear in multiple events
* Have multiple users
* Upload W9 documents
* Access event reporting
* Access event documents

Artists must never see:

* Customer names
* Customer emails
* Customer phone numbers
* Individual order details

Artist access is controlled by permissions.

---

## Artist Permissions

Artist permissions are configurable on a per-event basis.

Examples:

* View Ticket Sales
* View Revenue
* View Promo Codes
* View Ticket Breakdown
* View Check-In Count
* View Settlement Estimate
* View Marketing QR Statistics
* View Own Contract
* Upload W9
* Download 1099

The venue or event owner controls these permissions.

Roles are templates only.

Permissions are the actual security model.

---

## User Roles

Initial Roles:

* StubTree Admin
* Venue Admin
* Promoter
* Artist
* Door Staff
* Parking Staff
* Customer

Users may have multiple roles.

Example:

A user may be:

* Door Staff
* Parking Staff

with a single login.

Role assignments should support both venue-level access and event-level access.

## Event Lifecycle

Event Statuses:

* Draft
* Published
* Completed
* Archived
* Cancelled
* Postponed
* Rescheduled

### Draft

The event is not publicly visible.

Characteristics:

* Editable
* Not searchable
* Ticket sales unavailable
* Internal only

### Published

The event is publicly visible.

Characteristics:

* Searchable
* Purchasable
* Marketing active
* Ticket sales controlled by ticket sale dates and inventory

### Completed

The event has occurred.

Characteristics:

* Ticket sales closed
* Check-in complete
* Settlement active
* Reporting active
* Refunds still possible by authorized users

### Archived

Historical state.

Characteristics:

* Hidden from normal dashboards
* Searchable
* Reportable
* Historical records preserved

Recommended archive timing:

Approximately one week after event completion.

### Cancelled

The event will not occur.

Characteristics:

* Ticket sales closed
* Refund workflows available
* Customer notifications required

### Postponed

The event date is unknown.

### Rescheduled

The event date has changed.

Customer notifications should be supported for both postponed and rescheduled events.

---

## Event Creation Workflow

Goal:

A first-time organizer should be able to create and publish an event in less than 10 minutes.

### Step 1 - Event Basics

Fields:

* Event Name
* Venue
* Event Date
* Doors Time
* Start Time
* End Time
* Age Policy

### Step 2 - Artists

* Add artists
* Add artist users
* Configure artist permissions
* Configure artist deal information

### Step 3 - Media

* Poster Upload
* Additional Images
* Video Links
* Event Description

### Step 4 - Tickets

Apply venue template.

Configure:

* Price
* Day-of-show price
* Inventory
* Sale start date
* Sale end date
* Maximum per order

### Step 5 - Marketing

* Facebook Event URL
* Promo Codes
* Marketing QR Codes
* Radio Spend
* Social Spend
* Marketing Notes

### Step 6 - Documents

Upload:

* Contracts
* PDFs
* Spreadsheets
* Posters
* Images
* Event Files

### Step 7 - Review

Requirements before publish:

* Poster
* Description
* At least one ticket type

Preview page available.

### Step 8 - Publish

Event becomes publicly available.

First event from a new organizer requires approval.

---

## Ticket Templates

Venues should be able to create reusable ticket templates.

Initial Josabi's template:

* General Admission
* Rail Chair for 1
* Table for 2
* Table for 4-6
* Table for 10
* VIP Parking

Ticket templates reduce event creation time.

---

## Ticket Types

Each event may contain multiple ticket types.

Ticket Type Fields:

* Name
* Description
* Price
* Day-of-show price
* Quantity available
* Sale start date
* Sale end date
* Maximum per order
* Visibility
* Ticket Kind
* Includes Admission

Examples:

### General Admission

Includes Admission = True

### Rail Chair

Includes Admission = True

### Table

Includes Admission = False

### VIP Parking

Includes Admission = False

Customers should receive a warning when purchasing tables without admission tickets.

The system should not force admission purchases.

---

## Capacity Philosophy

StubTree v2 does not rely on rigid venue capacity enforcement.

Inventory is controlled primarily through ticket quantities.

Examples:

500 General Admission

50 Rail Chairs

25 Tables

100 Parking Passes

Each ticket type sells independently.

Store:

* Venue Capacity (optional)
* Event Capacity (optional)

for reporting and future expansion.

---

## Waitlists

If all public ticket inventory is sold out:

Customers may join a waitlist.

Fields:

* Name
* Email
* Phone (optional)
* Quantity Requested
* Ticket Type Requested
* Notes

No automatic purchasing required in v2.

Waitlist exists primarily for lead capture and demand tracking.

## Customer Accounts

StubTree does not support guest checkout.

Every purchaser must have a StubTree account.

The primary goal is to eliminate mistyped email addresses and improve ticket delivery reliability.

### Account Creation Flow

1. Customer enters email address.
2. StubTree sends a magic link.
3. Customer clicks the magic link.
4. StubTree verifies the email.
5. If the account does not exist:

   * Create account.
6. If the account exists:

   * Log the customer in.
7. Return customer to original cart.
8. Continue checkout.

Existing users may also log in using:

* Email
* Password

### Customer Profile

Customers should have:

* First Name
* Last Name
* Email
* Phone (optional)

Email is required.

Phone is optional.

Phone should be validated if supplied.

---

## Customer Portal

Customers should be able to:

### Upcoming Events

View future ticket purchases.

### Past Events

View historical purchases.

### My Tickets

Access active QR codes.

### Account Settings

Manage account information.

### Ticket Resend

Resend tickets to themselves.

### Refund Status

View refunded tickets.

---

## Stripe Checkout Philosophy

StubTree should not collect credit card information.

StubTree should not store credit card information.

StubTree should not process card fields directly.

All online payments must occur on Stripe-hosted pages.

### Checkout Flow

Customer builds cart.

StubTree creates:

* Pending Order

StubTree creates:

* Stripe Checkout Session

Customer is redirected to Stripe.

Stripe collects payment.

Stripe webhook confirms payment.

StubTree marks order as paid.

StubTree generates tickets.

StubTree delivers tickets.

### Benefits

* Reduced PCI scope
* Simpler compliance
* Improved security
* Faster implementation

---

## Ticket Lifecycle

Ticket Statuses:

* Pending
* Paid
* Issued
* Scanned
* Refunded
* Voided

### Pending

Order exists.

Payment not confirmed.

### Paid

Stripe payment confirmed.

### Issued

Ticket generated.

QR available.

### Scanned

Customer checked in.

### Refunded

Refund completed.

Ticket invalid.

### Voided

Manually disabled.

Ticket invalid.

---

## Ticket Ownership

In v2:

Tickets belong to the purchasing customer.

Example:

John purchases:

* 4 General Admission
* 1 Parking

All tickets belong to John.

Attendee assignment is out of scope for v2.

However:

Tickets must be modeled as individual records so attendee assignment can be added later.

---

## QR Ticket Strategy

Every ticket receives a unique QR code.

QR codes are generated after payment confirmation.

One ticket equals one QR code.

One order may contain many tickets.

### Examples

Order:

4 General Admission

Generates:

4 tickets

4 QR codes

### Ticket URL

Preferred format:

```text
/t/{ticket_token}
```

Ticket tokens should not expose database IDs.

---

## Ticket Display Experience

Customers should be able to:

* View ticket
* Swipe between tickets
* View QR codes easily on mobile

Large QR codes should be optimized for scanning.

### Real-Time Scan Experience

When a ticket is scanned:

Customer portal should update instantly.

Show:

✓ Checked In

Then automatically advance to the next unscanned ticket.

Recommended technology:

SignalR

---

## Ticket Kinds

Ticket Kind is different from Ticket Type.

Examples:

### Admission

Used for venue entry.

### Parking

Used for parking access.

### VIP

Used for VIP checkpoints.

### Add-On

Used for upgrades and extras.

### Comp

Used for guest lists and promotional tickets.

Ticket Kind controls scanner behavior.

---

## Scanner Permissions

Door Staff:

* Admission tickets

Parking Staff:

* Parking tickets

Admins:

* All ticket kinds

If the wrong ticket type is scanned:

Display:

"Valid Ticket - Wrong Checkpoint"

---

## Marketing QR Codes

Marketing QR codes are separate from ticket QR codes.

Purpose:

Drive traffic and measure conversions.

Examples:

* Band QR
* Promoter QR
* Flyer QR
* Poster QR
* Radio QR

Marketing QR Codes should track:

* Scans
* Clicks
* Purchases
* Revenue
* Conversion Rate

These QR codes should link to:

```text
/events/{slug}
```

not directly to tickets.

---

## Promo Codes

Promo Codes support:

* Discounts
* Artist attribution
* Promoter attribution
* Radio station campaigns
* Marketing campaigns

Discounts may be:

5%
10%
15%
20%
25%
30%

Reporting should include:

* Uses
* Revenue
* Tickets sold
* Discount totals
* Conversion performance

## Reporting Philosophy

Reporting is a core feature of StubTree.

The goal is to provide organizers with significantly better visibility into event performance than competing platforms.

All dashboards should be mobile-friendly and accessible on:

* Desktop
* Tablet
* Mobile

Users should be able to quickly understand event performance without exporting spreadsheets.

---

## Venue Dashboard

Venue users should have access to venue-wide reporting.

Examples:

### Overview

* Upcoming Events
* Events This Month
* Tickets Sold This Month
* Revenue This Month
* Refund Requests
* Recent Orders

### Event Summary

For each event:

* Tickets Sold
* Revenue
* Attendance
* Refunds
* Comps
* Parking Sold
* Tables Sold

### Trends

* Daily Sales
* Weekly Sales
* Revenue Trends
* Ticket Velocity

### Marketing Performance

* Promo Code Performance
* Marketing QR Performance
* Campaign Attribution

---

## Event Dashboard

Every event should have its own dashboard.

### Event Overview

* Event Name
* Date
* Venue
* Status

### Sales

* Tickets Sold
* Revenue
* Refunds
* Discounts
* Taxes
* Fees

### Attendance

* Tickets Scanned
* Tickets Remaining
* Attendance Percentage

### Ticket Breakdown

Examples:

* General Admission
* Rail Chairs
* Tables
* VIP Parking

### Sales Trends

* Daily Sales
* Hourly Sales
* Sales Velocity

### Marketing

* Promo Codes
* Marketing QR Codes
* Facebook Event Link
* Campaign Spend

### Documents

* Contracts
* Posters
* Settlement Files
* Event Documents

---

## Artist Dashboard

Artists only see information allowed by permissions.

Examples:

### Allowed Metrics

* Ticket Sales
* Revenue
* Promo Code Usage
* Attendance
* Marketing QR Performance
* Settlement Estimates

### Restricted Data

Artists may never see:

* Customer Names
* Customer Emails
* Customer Phones
* Individual Orders

Artist permissions are configurable by the event owner.

---

## Promoter Dashboard

Promoters can view events they own or are assigned to.

Examples:

* Event Sales
* Event Revenue
* Marketing Performance
* Promo Code Usage
* Attendance
* Settlement Estimates

Promoters should have a broader view than artists but less visibility than venue admins.

---

## StubTree Admin Dashboard

Platform-wide visibility.

Examples:

### Revenue

* Platform Revenue
* Total Tickets Sold
* Total Orders

### Operations

* Refund Queue
* Support Queue
* Failed Emails
* Failed SMS

### Events

* Upcoming Events
* Recently Published Events
* Top Performing Events

### Risk Monitoring

* High Refund Rates
* Excessive Chargebacks
* Suspicious Accounts
* Potential Venue Conversion Alerts

---

## Promo Code Reporting

Promo code reporting is a first-class feature.

Each code should track:

* Uses
* Revenue
* Discount Amount
* Tickets Sold
* Conversion Rate

Examples:

```text
TOADIES
75 Uses
$2,400 Revenue
```

```text
RADIO
32 Uses
$960 Revenue
```

---

## Marketing QR Reporting

Marketing QR codes should track:

* Scans
* Clicks
* Purchases
* Revenue

Examples:

Band QR:

* 500 Scans
* 80 Purchases

Promoter QR:

* 300 Scans
* 50 Purchases

Poster QR:

* 200 Scans
* 15 Purchases

This allows organizers to measure which marketing efforts actually generate sales.

---

## Settlement Reporting

Settlement reporting should support event reconciliation.

### Revenue Sources

* Online Sales
* Door Cash
* Door Card
* Manual External Sales

### Adjustments

* Refunds
* Discounts
* Comps

### Fees

* StubTree Fees
* Card Fees
* Taxes

### Totals

* Gross Revenue
* Net Revenue

### Artist Information

* Guarantees
* Bonus Structures
* Promo Code Revenue

Settlement reporting should display:

1. Raw numbers
2. Estimated settlements

Manual overrides may be added later.

---

## Refund Reporting

Track:

* Refund Count
* Refund Amount
* Refund Reason
* Refund Type

Examples:

* Full Refund
* Partial Refund
* Bulk Refund

---

## Event History

Historical reporting should remain available after events are archived.

Users should be able to review:

* Historical sales
* Attendance
* Marketing performance
* Settlement data
* Ticket trends

for any historical event.

Archived events remain searchable and reportable.
