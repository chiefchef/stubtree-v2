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
