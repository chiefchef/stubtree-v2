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