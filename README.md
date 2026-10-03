# The Gagrafa — Booking Website, Guest Portal & Admin Dashboard

"Live Luxuriously, Stay Serenely." A booking platform for The Gagrafa, a luxury
serviced apartment complex in Spintex / Community 20, Accra.

## What's in this scaffold

This is a working Next.js 15 (App Router) + TypeScript + Tailwind + Supabase
starting point, built to match the full brief. Given the size of the brief
(public site + guest portal + admin dashboard + payments + notifications),
it's organized into three tiers of completeness so you can see exactly
what's ready to run and what's next:

**Fully implemented**
- Database schema with RLS policies, an availability function that prevents
  double-booking, an auto-profile-on-signup trigger, and seed data for the
  five real Gagrafa unit types (`supabase/schema.sql`)
- Design system: navy/charcoal + champagne-gold palette, Playfair Display +
  Inter type, reusable button/card classes (`tailwind.config.ts`, `globals.css`)
- Public site: homepage, residences listing, residence detail page with a
  live price calculator, header/footer, responsive layout
- Booking flow: 3-step (dates & unit → guest details → review & pay), an
  API route that checks availability server-side and creates the booking
  with a generated `GAG-YYYY-XXXX` code, and a success page
- Guest portal: booking-code lookup, stay summary, check-in/Wi-Fi/house-rules
  info cards, and a working service-request form wired to its own API route
- Admin dashboard shell: sidebar nav for all sections, an overview page with
  live counts (today's check-ins/outs, month revenue, pending requests), and
  a bookings table page reading real data
- Paystack webhook route: verifies the signature and marks bookings paid —
  ready for you to drop in your secret key

**Stubbed (page exists, ready for you or Claude Code to fill in)**
- Admin: Units CRUD, Guests list, Service Requests queue, Payments table,
  Reviews moderation, Settings
- About, Contact, FAQ, Privacy, Terms static pages

**Integration points marked with `// TODO`, not yet wired up**
- Paystack transaction initialization (client → `/api/bookings` → Paystack)
- Resend confirmation emails
- WhatsApp notifications (Twilio or WhatsApp Business API — provider is a
  config choice, both need a business-verified sender number)
- Supabase Storage image upload for units
- Auth (magic link / phone) for the guest portal and admin login — RLS
  policies are already written for `profiles.role` ('guest' | 'admin' | 'staff')

This isn't a small brief — it's a full three-surface product. Rather than
generate hundreds of files of unreviewed, unrunnable code in one pass, this
scaffold prioritizes getting the architecture, schema, and design system
exactly right, with the highest-value flows working end to end against a
real database. **For finishing the stubbed admin CRUD screens and wiring
the three integrations, Claude Code (desktop or terminal) is a much better
fit than chat** — it can run `npm install`, start the dev server, iterate
against real errors, and work through the remaining pages file by file.

## Setup

1. **Supabase**
   - Create a project at supabase.com
   - Run `supabase/schema.sql` in the SQL editor (creates tables, RLS, seed data)
   - Copy your Project URL, anon key, and service role key into `.env.local`

2. **Environment variables**
   ```
   cp .env.example .env.local
   ```
   Fill in Supabase, Paystack, Resend, and WhatsApp provider keys.

3. **Install & run**
   ```
   npm install
   npm run dev
   ```

4. **First admin user**
   Sign up normally, then in the Supabase table editor set that user's
   `profiles.role` to `admin`.

5. **Paystack**
   - Add your public key to `NEXT_PUBLIC_PAYSTACK_PUBLIC_KEY`
   - Add your secret key to `PAYSTACK_SECRET_KEY`
   - Point a Paystack webhook at `/api/webhooks/paystack`

## Project structure

```
src/app/                  Public site, booking flow, guest portal, admin
src/app/api/              Bookings, service requests, Paystack webhook
src/components/site/      Public + guest-portal components
src/lib/supabase/         Browser, server, and service-role clients
src/lib/types.ts          Shared types + booking code/nights helpers
supabase/schema.sql       Full schema, RLS policies, seed data
```
