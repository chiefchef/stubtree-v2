# StubTree v2 Database Schema

Version: 1.0 Draft

## Database Principles

* PostgreSQL
* snake_case table names
* snake_case column names
* UUID primary keys
* public-facing records use slugs/tokens, not database IDs
* soft deletes where practical
* audit sensitive actions
* store file metadata in the database, not file contents
* store payment/tax/fee snapshots at purchase time

Common columns for most tables:

```sql
id uuid primary key
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

# 1. Platform / Organization Tables

## organizations

Represents an entity that can own events.

Organization types:

* venue
* promoter
* artist
* stubtree

```sql
id uuid primary key

organization_type text not null

name text not null

slug text not null unique

email text null

phone text null

website_url text null

default_stubtree_fee numeric(10,2) not null default 2.00

is_verified boolean not null default false

is_active boolean not null default true

requires_approval boolean not null default false

approved_at timestamptz null

approved_by_user_id uuid null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

Notes:

* Artist organizations default to $1.00 fee.
* Venue organizations default to $2.00 fee.
* Promoter organizations default to $2.00 fee.
* First-time organizers may require approval.

---

## organization_settings

Organization-specific configuration.

```sql
id uuid primary key

organization_id uuid not null references organizations(id)

default_stubtree_fee numeric(10,2) null

default_tax_rate numeric(10,4) null

terms_text text null

support_email text null

support_phone text null

logo_file_id uuid null references files(id)

primary_color text null

secondary_color text null

custom_domain text null

subdomain text null

allow_white_label boolean not null default false

settings_json jsonb null

created_at timestamptz not null
updated_at timestamptz not null
```

Purpose:

* White label support
* Branding
* Fee configuration
* Tax configuration
* Support configuration

---

## users

All authenticated users.

```sql
id uuid primary key

email text not null unique

password_hash text null

first_name text null

last_name text null

phone text null

email_verified_at timestamptz null

phone_verified_at timestamptz null

last_login_at timestamptz null

is_active boolean not null default true

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## organization_users

Links users to organizations.

```sql
id uuid primary key

organization_id uuid not null references organizations(id)

user_id uuid not null references users(id)

is_active boolean not null default true

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## roles

Role templates.

```sql
id uuid primary key

key text not null unique

name text not null

description text null

created_at timestamptz not null
updated_at timestamptz not null
```

Examples:

* stubtree_admin
* venue_admin
* promoter
* artist
* door_staff
* parking_staff
* customer

---

## permissions

Granular permissions.

```sql
id uuid primary key

key text not null unique

name text not null

description text null

created_at timestamptz not null
updated_at timestamptz not null
```

---

## role_permissions

```sql
role_id uuid not null references roles(id)

permission_id uuid not null references permissions(id)

primary key (role_id, permission_id)
```

---

## user_permissions

Direct user permission overrides.

```sql
id uuid primary key

user_id uuid not null references users(id)

permission_id uuid not null references permissions(id)

scope_type text null

scope_id uuid null

allowed boolean not null default true

created_at timestamptz not null
updated_at timestamptz not null
```

Scope Examples:

* organization
* venue
* event
* artist


## organizations

Represents an entity that can own events.

Organization types:

* venue
* promoter
* artist
* stubtree

Key fields:

```sql
id uuid primary key
organization_type text not null
name text not null
slug text not null unique
email text null
phone text null
website_url text null
default_stubtree_fee numeric(10,2) not null default 2.00
is_verified boolean not null default false
is_active boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

Notes:

Artist organizations default to $1.00 fee.

Venue and promoter organizations default to $2.00 fee.

---

## users

All authenticated users.

```sql
id uuid primary key
email text not null unique
password_hash text null
first_name text null
last_name text null
phone text null
email_verified_at timestamptz null
phone_verified_at timestamptz null
last_login_at timestamptz null
is_active boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## organization_users

Links users to organizations.

```sql
id uuid primary key
organization_id uuid not null references organizations(id)
user_id uuid not null references users(id)
is_active boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## roles

Role templates.

```sql
id uuid primary key
key text not null unique
name text not null
description text null
created_at timestamptz not null
updated_at timestamptz not null
```

Examples:

* stubtree_admin
* venue_admin
* promoter
* artist
* door_staff
* parking_staff
* customer

---

## permissions

Granular permissions.

```sql
id uuid primary key
key text not null unique
name text not null
description text null
created_at timestamptz not null
updated_at timestamptz not null
```

Examples:

* event_create
* event_edit
* event_publish
* ticket_manage
* ticket_scan_admission
* ticket_scan_parking
* door_sale_create
* refund_process
* refund_bulk_process
* report_view
* settlement_view
* document_upload
* user_manage

---

## role_permissions

```sql
role_id uuid not null references roles(id)
permission_id uuid not null references permissions(id)
primary key (role_id, permission_id)
```

---

## user_permissions

Direct user permission overrides.

```sql
id uuid primary key
user_id uuid not null references users(id)
permission_id uuid not null references permissions(id)
scope_type text null
scope_id uuid null
allowed boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
```

Scope examples:

* organization
* venue
* event
* artist

---

# 2. Venue / Artist / Promoter Tables

## venues

Physical event locations.

```sql
id uuid primary key
organization_id uuid null references organizations(id)
name text not null
slug text not null unique
address1 text null
address2 text null
city text null
state text null
postal_code text null
country text not null default 'US'
website_url text null
default_capacity integer null
is_verified boolean not null default false
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

Notes:

Temporary venues can be created by artists/promoters.

---

## venue_users

Venue-level access.

```sql
id uuid primary key
venue_id uuid not null references venues(id)
user_id uuid not null references users(id)
is_active boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## artists

Artist/band records.

```sql
id uuid primary key
organization_id uuid null references organizations(id)
name text not null
slug text not null unique
website_url text null
facebook_url text null
instagram_url text null
spotify_url text null
notes text null
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## artist_users

```sql
id uuid primary key
artist_id uuid not null references artists(id)
user_id uuid not null references users(id)
is_active boolean not null default true
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## promoters

Promoter records.

```sql
id uuid primary key
organization_id uuid null references organizations(id)
name text not null
slug text not null unique
website_url text null
notes text null
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

# 3. Event Tables

## event_categories

```sql
id uuid primary key

name text not null

slug text not null unique

sort_order integer not null default 0

is_active boolean not null default true

created_at timestamptz not null
updated_at timestamptz not null
```

Initial Seed Data:

* Concert
* Festival
* Venue Event
* Community Event
* Sports Event

---

## events

Core event table.

```sql
id uuid primary key

organization_id uuid not null references organizations(id)

venue_id uuid not null references venues(id)

name text not null

slug text not null unique

description text null

status text not null default 'draft'

approval_status text not null default 'approved'

event_category_id uuid null references event_categories(id)

poster_file_id uuid null references files(id)

doors_at timestamptz null

starts_at timestamptz not null

ends_at timestamptz not null

published_at timestamptz null

completed_at timestamptz null

archived_at timestamptz null

age_policy text null

terms_text text null

facebook_event_url text null

radio_spend numeric(10,2) null

social_spend numeric(10,2) null

marketing_spend numeric(10,2) null

capacity integer null

requires_approval boolean not null default false

approved_at timestamptz null

approved_by_user_id uuid null references users(id)

created_by_user_id uuid not null references users(id)

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

Statuses:

* draft
* published
* completed
* archived
* cancelled
* postponed
* rescheduled

Approval Status:

* pending
* approved
* rejected

---

## event_artists

```sql
id uuid primary key

event_id uuid not null references events(id)

artist_id uuid not null references artists(id)

billing_order integer null

billing_label text null

deal_type text null

deal_notes text null

guarantee_amount numeric(10,2) null

bonus_terms text null

promo_code_id uuid null

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## event_promoters

```sql
id uuid primary key

event_id uuid not null references events(id)

promoter_id uuid not null references promoters(id)

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## event_ownership_history

Tracks ownership changes.

```sql
id uuid primary key

event_id uuid not null references events(id)

previous_organization_id uuid not null references organizations(id)

new_organization_id uuid not null references organizations(id)

changed_by_user_id uuid not null references users(id)

notes text null

created_at timestamptz not null
```

---

## event_users

Show-specific user access.

```sql
id uuid primary key

event_id uuid not null references events(id)

user_id uuid not null references users(id)

is_active boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## event_user_permissions

```sql
id uuid primary key

event_id uuid not null references events(id)

user_id uuid not null references users(id)

permission_id uuid not null references permissions(id)

allowed boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null
```

---

## event_documents

```sql
id uuid primary key

event_id uuid not null references events(id)

file_id uuid not null references files(id)

name text not null

document_type text null

is_private boolean not null default true

uploaded_by_user_id uuid not null references users(id)

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## event_updates

Used for:

* Lineup changes
* Weather updates
* Parking updates
* Time changes

```sql
id uuid primary key

event_id uuid not null references events(id)

title text not null

message text not null

send_email boolean not null default false

send_sms boolean not null default false

created_by_user_id uuid not null references users(id)

created_at timestamptz not null

updated_at timestamptz not null
```

---

## featured_events

Admin-controlled homepage placement.

```sql
id uuid primary key

event_id uuid not null references events(id)

slot text null

priority integer not null default 0

starts_at timestamptz null

ends_at timestamptz null

is_active boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## event_page_views

Tracks event traffic.

```sql
id uuid primary key

event_id uuid not null references events(id)

customer_id uuid null references customers(id)

marketing_qr_code_id uuid null references marketing_qr_codes(id)

promo_code_id uuid null references promo_codes(id)

ip_address text null

user_agent text null

referrer text null

created_at timestamptz not null
```


# 4. File Storage

## files

Stores metadata only.

Actual files live in Railway Storage Buckets or another S3-compatible provider.

```sql
id uuid primary key
storage_provider text not null
bucket_name text not null
storage_key text not null
file_name text not null
content_type text null
file_size_bytes bigint null
uploaded_by_user_id uuid null references users(id)
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

# 5. Ticketing Tables

## ticket_kinds

Controls scanner behavior.

```sql id="j1x3ha"
id uuid primary key

key text not null unique

name text not null

description text null

created_at timestamptz not null

updated_at timestamptz not null
```

Examples:

* admission
* parking
* vip
* table
* rail
* addon
* comp

---

## ticket_templates

Reusable venue templates.

```sql id="mn2f8j"
id uuid primary key

venue_id uuid not null references venues(id)

name text not null

is_default boolean not null default false

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## ticket_template_items

```sql id="w6xwqt"
id uuid primary key

ticket_template_id uuid not null references ticket_templates(id)

ticket_kind_id uuid not null references ticket_kinds(id)

name text not null

description text null

price numeric(10,2) not null

day_of_show_price numeric(10,2) null

quantity_available integer null

includes_admission boolean not null default false

max_per_order integer null

sale_start_offset_hours integer null

sale_end_offset_hours integer null

sort_order integer not null default 0

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## ticket_types

Event-specific ticket types.

```sql id="l8r1wv"
id uuid primary key

event_id uuid not null references events(id)

ticket_kind_id uuid not null references ticket_kinds(id)

name text not null

description text null

price numeric(10,2) not null

day_of_show_price numeric(10,2) null

quantity_available integer not null

quantity_sold integer not null default 0

includes_admission boolean not null default false

max_per_order integer null

sale_start_at timestamptz null

sale_end_at timestamptz null

sort_order integer not null default 0

is_visible boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## waitlist_entries

Used when ticket inventory is sold out.

```sql id="h6msd8"
id uuid primary key

event_id uuid not null references events(id)

ticket_type_id uuid null references ticket_types(id)

name text not null

email text not null

phone text null

quantity_requested integer not null default 1

notes text null

created_at timestamptz not null

updated_at timestamptz not null
```

---

## ticket_price_history

Tracks pricing changes.

```sql id="ytm7h7"
id uuid primary key

ticket_type_id uuid not null references ticket_types(id)

old_price numeric(10,2) not null

new_price numeric(10,2) not null

changed_by_user_id uuid not null references users(id)

notes text null

created_at timestamptz not null
```


---

# 6. Customer Tables

## customers

Customer profile.

```sql id="plxt8x"
id uuid primary key

user_id uuid null references users(id)

email text not null

first_name text null

last_name text null

phone text null

email_verified_at timestamptz null

phone_verified_at timestamptz null

last_order_at timestamptz null

last_login_at timestamptz null

is_active boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## customer_addresses

Future-proofing for billing and venue-specific use cases.

```sql id="3pjlwm"
id uuid primary key

customer_id uuid not null references customers(id)

address_type text not null

address1 text null

address2 text null

city text null

state text null

postal_code text null

country text not null default 'US'

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

Address Types:

* billing
* mailing

---

## customer_preferences

Notification preferences.

```sql id="5pjrpk"
id uuid primary key

customer_id uuid not null references customers(id)

allow_email boolean not null default true

allow_sms boolean not null default false

allow_marketing boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null
```

---

## customer_login_history

Tracks account access.

```sql id="n4y88r"
id uuid primary key

customer_id uuid not null references customers(id)

ip_address text null

user_agent text null

created_at timestamptz not null
```


---

# 7. Cart Tables

## carts

```sql
id uuid primary key

customer_id uuid not null references customers(id)

event_id uuid not null references events(id)

status text not null

expires_at timestamptz null

created_at timestamptz not null
updated_at timestamptz not null
```

Status:

* active
* abandoned
* converted

---

## cart_items

```sql
id uuid primary key

cart_id uuid not null references carts(id)

ticket_type_id uuid not null references ticket_types(id)

quantity integer not null

unit_price numeric(10,2) not null

created_at timestamptz not null
updated_at timestamptz not null
```

---

# 8. Order Tables

## orders

One Stripe checkout = one order.

```sql id="sjv3tz"
id uuid primary key

order_number text not null unique

customer_id uuid not null references customers(id)

event_id uuid not null references events(id)

status text not null

base_ticket_total numeric(10,2) not null

stubtree_fee_total numeric(10,2) not null

card_fee_total numeric(10,2) not null

tax_total numeric(10,2) not null

discount_total numeric(10,2) not null

grand_total numeric(10,2) not null

promo_code_id uuid null references promo_codes(id)

marketing_campaign_id uuid null references marketing_campaigns(id)

marketing_qr_code_id uuid null references marketing_qr_codes(id)

placed_at timestamptz null

created_at timestamptz not null

updated_at timestamptz not null
```

Status:

* pending
* paid
* refunded
* partially_refunded
* cancelled

---

## order_items

Snapshot of pricing.

```sql id="nskt7v"
id uuid primary key

order_id uuid not null references orders(id)

ticket_type_id uuid not null references ticket_types(id)

ticket_kind_id uuid not null references ticket_kinds(id)

name text not null

quantity integer not null

unit_price numeric(10,2) not null

stubtree_fee numeric(10,2) not null

card_fee numeric(10,2) not null

tax_amount numeric(10,2) not null

discount_amount numeric(10,2) not null

line_total numeric(10,2) not null

created_at timestamptz not null
```

---

## order_status_history

Tracks order lifecycle.

```sql id="g4mj9v"
id uuid primary key

order_id uuid not null references orders(id)

old_status text null

new_status text not null

changed_by_user_id uuid null references users(id)

notes text null

created_at timestamptz not null
```

Examples:

* pending → paid
* paid → refunded
* paid → partially_refunded
* pending → cancelled

```
```

---

# 9. Ticket Tables

## tickets

Most important table in the system.

Each ticket is an individual record.

```sql id="0g4hcd"
id uuid primary key

order_id uuid not null references orders(id)

order_item_id uuid not null references order_items(id)

event_id uuid not null references events(id)

ticket_type_id uuid not null references ticket_types(id)

ticket_kind_id uuid not null references ticket_kinds(id)

customer_id uuid not null references customers(id)

ticket_token text not null unique

status text not null

issued_at timestamptz not null

first_viewed_at timestamptz null

last_viewed_at timestamptz null

checked_in_at timestamptz null

refunded_at timestamptz null

voided_at timestamptz null

created_at timestamptz not null

updated_at timestamptz not null
```

Status:

* active
* scanned
* refunded
* voided

---

## ticket_history

Tracks ticket lifecycle changes.

```sql id="4baf70"
id uuid primary key

ticket_id uuid not null references tickets(id)

old_status text null

new_status text not null

changed_by_user_id uuid null references users(id)

notes text null

created_at timestamptz not null
```

Examples:

* active → scanned
* active → refunded
* active → voided
* scanned → active (scan reversal)

---

## ticket_views

Tracks customer ticket access.

```sql id="b6ph3v"
id uuid primary key

ticket_id uuid not null references tickets(id)

customer_id uuid null references customers(id)

ip_address text null

user_agent text null

created_at timestamptz not null
```


# 10. Payment Tables

## payments

Represents successful payment transactions.

```sql
id uuid primary key

order_id uuid not null references orders(id)

provider text not null

provider_payment_id text not null

provider_charge_id text null

amount numeric(10,2) not null

currency text not null default 'usd'

status text not null

paid_at timestamptz null

created_at timestamptz not null
updated_at timestamptz not null
```

Status:

* pending
* succeeded
* failed
* refunded
* partially_refunded

---

## stripe_checkout_sessions

Tracks Stripe-hosted checkout sessions.

```sql
id uuid primary key

order_id uuid not null references orders(id)

stripe_session_id text not null unique

stripe_url text null

expires_at timestamptz null

completed_at timestamptz null

created_at timestamptz not null
updated_at timestamptz not null
```

---

## stripe_events

Stores webhook history.

```sql
id uuid primary key

stripe_event_id text not null unique

event_type text not null

payload jsonb not null

processed_at timestamptz null

created_at timestamptz not null
```

Purpose:

* Idempotency
* Auditing
* Troubleshooting

---

# 11. Refund Tables

## refunds

```sql
id uuid primary key

order_id uuid not null references orders(id)

payment_id uuid not null references payments(id)

refund_amount numeric(10,2) not null

refund_reason text null

refund_type text not null

stripe_refund_id text null

approved_by_user_id uuid null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
```

Refund Types:

* full
* partial
* custom

---

## refund_items

```sql
id uuid primary key

refund_id uuid not null references refunds(id)

ticket_id uuid null references tickets(id)

amount numeric(10,2) not null

created_at timestamptz not null
```

---

## refund_batches

Bulk event refunds.

```sql
id uuid primary key

event_id uuid not null references events(id)

name text not null

customer_message text not null

refund_type text not null

status text not null

created_by_user_id uuid not null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
```

Status:

* pending
* processing
* completed
* failed

---

## refund_batch_items

```sql
id uuid primary key

refund_batch_id uuid not null references refund_batches(id)

order_id uuid not null references orders(id)

refund_id uuid null references refunds(id)

status text not null

error_message text null

created_at timestamptz not null
updated_at timestamptz not null
```

# 12. Scanner Tables

## scanner_checkpoints

Defines where tickets can be scanned.

Examples:

* Front Gate
* VIP Entrance
* Parking Lot

```sql
id uuid primary key

event_id uuid not null references events(id)

name text not null

ticket_kind_id uuid null references ticket_kinds(id)

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## ticket_scans

Every scan attempt should be recorded.

```sql
id uuid primary key

ticket_id uuid not null references tickets(id)

checkpoint_id uuid not null references scanner_checkpoints(id)

scanner_user_id uuid not null references users(id)

scan_result text not null

notes text null

created_at timestamptz not null
```

Results:

* valid
* duplicate
* refunded
* voided
* wrong_checkpoint

---

# 13. Marketing Tables

## promo_codes

Supports:

* Discounts
* Artist attribution
* Promoter attribution
* Radio campaigns
* Marketing campaigns

```sql id="l5md1f"
id uuid primary key

event_id uuid not null references events(id)

code text not null

name text null

description text null

discount_type text null

discount_value numeric(10,2) null

max_uses integer null

uses_count integer not null default 0

starts_at timestamptz null

ends_at timestamptz null

is_active boolean not null default true

created_by_user_id uuid null references users(id)

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

Discount Types:

* percentage
* fixed_amount

---

## promo_code_redemptions

```sql id="76q8t6"
id uuid primary key

promo_code_id uuid not null references promo_codes(id)

order_id uuid not null references orders(id)

customer_id uuid not null references customers(id)

discount_amount numeric(10,2) not null

created_at timestamptz not null
```

---

## marketing_campaigns

Groups marketing assets together.

Examples:

* Radio Campaign
* Facebook Campaign
* Band Promotion
* Flyer Campaign

```sql id="m80iq8"
id uuid primary key

event_id uuid not null references events(id)

name text not null

campaign_type text null

notes text null

budget_amount numeric(10,2) null

starts_at timestamptz null

ends_at timestamptz null

created_by_user_id uuid null references users(id)

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## marketing_qr_codes

Separate from ticket QR codes.

Used for:

* Bands
* Promoters
* Flyers
* Posters
* Radio campaigns

```sql id="gk59rw"
id uuid primary key

event_id uuid not null references events(id)

marketing_campaign_id uuid null references marketing_campaigns(id)

name text not null

slug text not null unique

destination_url text not null

promo_code_id uuid null references promo_codes(id)

notes text null

created_by_user_id uuid null references users(id)

is_active boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null

deleted_at timestamptz null
```

---

## marketing_qr_events

Tracks usage.

```sql id="93l5st"
id uuid primary key

marketing_qr_code_id uuid not null references marketing_qr_codes(id)

customer_id uuid null references customers(id)

order_id uuid null references orders(id)

ip_address text null

user_agent text null

referrer text null

created_at timestamptz not null
```

---

## marketing_attribution

Tracks which campaign generated an order.

```sql id="bnglzm"
id uuid primary key

order_id uuid not null references orders(id)

marketing_campaign_id uuid null references marketing_campaigns(id)

marketing_qr_code_id uuid null references marketing_qr_codes(id)

promo_code_id uuid null references promo_codes(id)

created_at timestamptz not null
```


---

# 14. Door Sales / POS

## door_sales

```sql
id uuid primary key

event_id uuid not null references events(id)

customer_id uuid null references customers(id)

sale_type text not null

payment_method text not null

status text not null

subtotal numeric(10,2) not null

fees_total numeric(10,2) not null

tax_total numeric(10,2) not null

grand_total numeric(10,2) not null

created_by_user_id uuid not null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
```

Sale Types:

* ticket
* comp
* manual_external

Payment Methods:

* cash
* stripe_reader
* external

---

## door_sale_items

```sql
id uuid primary key

door_sale_id uuid not null references door_sales(id)

ticket_type_id uuid null references ticket_types(id)

quantity integer not null

unit_price numeric(10,2) not null

line_total numeric(10,2) not null

created_at timestamptz not null
```

---

## cash_reconciliations

```sql
id uuid primary key

event_id uuid not null references events(id)

expected_cash_total numeric(10,2) not null

actual_cash_total numeric(10,2) not null

difference_amount numeric(10,2) not null

notes text null

created_by_user_id uuid not null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
```

# 15. Guest List / Comp Tables

## guest_list_entries

Future replacement for paper guest lists.

Supports:

* Band Guests
* Radio Winners
* VIP Guests
* Owner Guests
* Promoter Guests

```sql
id uuid primary key

event_id uuid not null references events(id)

source text null

name text not null

email text null

phone text null

quantity integer not null default 1

ticket_kind_id uuid not null references ticket_kinds(id)

notes text null

created_by_user_id uuid null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

---

## comp_tickets

Comp tickets are first-class tickets.

They generate QR codes and appear in reporting.

```sql
id uuid primary key

event_id uuid not null references events(id)

guest_list_entry_id uuid null references guest_list_entries(id)

ticket_id uuid null references tickets(id)

created_by_user_id uuid not null references users(id)

created_at timestamptz not null
```

---

# 16. Notification Tables

# 16. Notification Tables

## email_messages

Tracks outbound email.

```sql
id uuid primary key

customer_id uuid null references customers(id)

event_id uuid null references events(id)

order_id uuid null references orders(id)

to_email text not null

subject text not null

body_template_key text null

status text not null

provider_message_id text null

provider_response jsonb null

sent_at timestamptz null

delivered_at timestamptz null

opened_at timestamptz null

clicked_at timestamptz null

failed_at timestamptz null

created_at timestamptz not null

updated_at timestamptz not null
```

Status:

* queued
* sent
* delivered
* opened
* clicked
* failed
* bounced

---

## sms_messages

Tracks outbound SMS.

```sql
id uuid primary key

customer_id uuid null references customers(id)

event_id uuid null references events(id)

order_id uuid null references orders(id)

to_phone text not null

message text not null

status text not null

provider_message_id text null

provider_response jsonb null

sent_at timestamptz null

delivered_at timestamptz null

failed_at timestamptz null

created_at timestamptz not null

updated_at timestamptz not null
```

Status:

* queued
* sent
* delivered
* failed

---

## magic_links

Supports passwordless login.

```sql
id uuid primary key

user_id uuid null references users(id)

email text not null

token_hash text not null

purpose text not null

return_url text null

expires_at timestamptz not null

used_at timestamptz null

created_at timestamptz not null
```

Purposes:

* login
* verification
* password_reset

---

## notification_templates

```sql
id uuid primary key

template_key text not null unique

channel text not null

subject text null

body text not null

template_variables jsonb null

is_active boolean not null default true

created_at timestamptz not null

updated_at timestamptz not null
```

Examples:

* magic_link
* ticket_delivery
* refund_notice
* event_update
* bulk_refund
* password_reset


---

# 17. Support Tables

## support_requests

```sql
id uuid primary key

customer_id uuid null references customers(id)

event_id uuid null references events(id)

order_id uuid null references orders(id)

ticket_id uuid null references tickets(id)

category text not null

status text not null

subject text not null

message text not null

assigned_to_user_id uuid null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

Categories:

* refund_request
* ticket_issue
* event_question
* general_inquiry

Status:

* open
* assigned
* waiting_customer
* resolved
* closed

# 18. Settlement Tables

## event_settlements

Stores event-level settlement and reconciliation totals.

```sql
id uuid primary key

event_id uuid not null references events(id)

status text not null

online_sales_total numeric(10,2) not null default 0

door_cash_total numeric(10,2) not null default 0

door_card_total numeric(10,2) not null default 0

external_sales_total numeric(10,2) not null default 0

refund_total numeric(10,2) not null default 0

discount_total numeric(10,2) not null default 0

stubtree_fee_total numeric(10,2) not null default 0

card_fee_total numeric(10,2) not null default 0

tax_total numeric(10,2) not null default 0

estimated_net_total numeric(10,2) not null default 0

notes text null

created_at timestamptz not null
updated_at timestamptz not null
```

Status:

* draft
* reviewed
* finalized

---

## settlement_line_items

Manual or calculated settlement adjustments.

```sql
id uuid primary key

event_settlement_id uuid not null references event_settlements(id)

line_type text not null

description text not null

amount numeric(10,2) not null

created_by_user_id uuid null references users(id)

created_at timestamptz not null
```

Line Types:

* income
* expense
* adjustment
* artist_payment
* promoter_payment
* venue_adjustment


---

# 19. Artist Tax / Payout Tables

## artist_tax_documents

Stores W9 and 1099 document references.

```sql
id uuid primary key

artist_id uuid not null references artists(id)

file_id uuid not null references files(id)

document_type text not null

tax_year integer null

status text not null

uploaded_by_user_id uuid null references users(id)

created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
```

Document Types:

* w9
* 1099

Status:

* missing
* submitted
* approved
* needs_update

---

## artist_payout_estimates

Stores estimated payout information.

```sql
id uuid primary key

event_id uuid not null references events(id)

artist_id uuid not null references artists(id)

deal_type text null

guarantee_amount numeric(10,2) null

bonus_amount numeric(10,2) null

promo_code_amount numeric(10,2) null

estimated_total numeric(10,2) not null default 0

notes text null

created_at timestamptz not null
updated_at timestamptz not null
```

---

# 20. Approval / Risk Tables

## event_approval_requests

Used when new organizers create events.

```sql
id uuid primary key

event_id uuid not null references events(id)

organization_id uuid not null references organizations(id)

status text not null

reviewed_by_user_id uuid null references users(id)

review_notes text null

created_at timestamptz not null
updated_at timestamptz not null
```

Status:

* pending
* approved
* rejected
* needs_info

---

## event_risk_alerts

Examples:

* repeated events at same venue
* excessive refunds
* excessive chargebacks
* unusual ticket volume

```sql
id uuid primary key

event_id uuid null references events(id)

organization_id uuid null references organizations(id)

alert_type text not null

severity text not null

message text not null

status text not null

created_at timestamptz not null
updated_at timestamptz not null
```

Severity:

* low
* medium
* high

Status:

* open
* reviewed
* resolved

---

# 21. Audit Tables

## audit_logs

Tracks sensitive changes.

```sql
id uuid primary key

user_id uuid null references users(id)

entity_type text not null

entity_id uuid null

action text not null

before_data jsonb null

after_data jsonb null

ip_address text null

user_agent text null

created_at timestamptz not null
```

Examples:

* event_updated
* ticket_price_changed
* refund_processed
* bulk_refund_started
* user_disabled
* permission_changed
* settlement_finalized
* ticket_scanned
* scan_reversed

---

# 22. Suggested Indexes

```sql
create index idx_events_slug on events(slug);

create index idx_events_venue_id on events(venue_id);

create index idx_events_organization_id on events(organization_id);

create index idx_ticket_types_event_id on ticket_types(event_id);

create index idx_orders_customer_id on orders(customer_id);

create index idx_orders_event_id on orders(event_id);

create index idx_orders_order_number on orders(order_number);

create index idx_tickets_ticket_token on tickets(ticket_token);

create index idx_tickets_event_id on tickets(event_id);

create index idx_tickets_customer_id on tickets(customer_id);

create index idx_payments_order_id on payments(order_id);

create index idx_stripe_events_stripe_event_id on stripe_events(stripe_event_id);

create index idx_promo_codes_event_id on promo_codes(event_id);

create index idx_marketing_qr_codes_event_id on marketing_qr_codes(event_id);

create index idx_ticket_scans_ticket_id on ticket_scans(ticket_id);

create index idx_ticket_scans_checkpoint_id on ticket_scans(checkpoint_id);

create index idx_email_messages_order_id on email_messages(order_id);

create index idx_support_requests_order_id on support_requests(order_id);
```

---

# 23. Seed Data Requirements

## Roles

* stubtree_admin
* venue_admin
* promoter
* artist
* door_staff
* parking_staff
* customer

## Ticket Kinds

* admission
* parking
* vip
* table
* rail
* addon
* comp

## Default Organizations

* StubTree
* Josabi's
* Texas Sound & Events

## Default Venue

* Josabi's

## Default Ticket Template

Josabi's:

* General Admission
* Rail Chair for 1
* Table for 2
* Table for 4-6
* Table for 10
* VIP Parking for 1 Car

## Notification Templates

* magic_link
* ticket_delivery
* refund_notice
* event_update
* bulk_refund_notice
