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
event_category text null
poster_file_id uuid null
doors_at timestamptz null
starts_at timestamptz not null
ends_at timestamptz not null
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
file_id uuid not null
name text not null
document_type text null
is_private boolean not null default true
uploaded_by_user_id uuid not null references users(id)
created_at timestamptz not null
updated_at timestamptz not null
deleted_at timestamptz null
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
