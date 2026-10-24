# My Catalogue — Multi-business SaaS starter

This package turns the White Pack feature set into a separate multi-business catalogue platform. It uses the **My Catalogue Supabase project** (`mkodburhxdrvfacytovb.supabase.co`) and does not use or alter the White Pack Supabase project.

## Included

- `admin.html` — account registration/sign-in, create a business profile, logo/brand settings, product CRUD, category, description/specifications, barcode/SKU, two price units, optimized WebP photo uploads, JSON import/export, custom-domain requests.
- `index.html` — public branded catalogue, categories, product search, image zoom, multiple unit prices, cart, customer/delivery form, optional GPS location, database order creation and WhatsApp checkout.
- `orders.html` — business-scoped order dashboard, search, status/payment updates, delivery map link, 10-second refresh.
- `quotation.html` — product picker, client-specific prices/quantities, discount/fee, print-to-PDF, private cloud quotation archive and JSON backup.
- `driver-tracking.html` — staff/driver login, opt-in GPS sharing and location map links.
- `saas.js` — shared Supabase client, domain/slug resolver, image optimizer.
- `SAAS_MIGRATION.sql` — new isolated `mc_saas_*` schema and RLS policies.
- `PLATFORM_ADMIN_GUIDE.sql` — SQL examples for manual activation, domain approval and driver assignment.

## Important: do not overwrite White Pack

Keep these files in a **new repository** such as `my-catalogue-saas`. They are not drop-in replacements for White Pack. The new platform uses its own tables (`mc_saas_*`) and image bucket (`mc-saas-images`). Existing White Pack orders, products and accounts remain separate.

## Setup order

1. Back up the intended My Catalogue Supabase project.
2. Open Supabase → SQL Editor and run `SAAS_MIGRATION.sql` once. Do not run it in the White Pack Supabase project.
3. In Supabase → Authentication, configure Site URL and Redirect URLs for the deployed app. Keep email confirmation enabled for production.
4. Upload the HTML files and `saas.js` together to a new repository/deployment. All files must share the same directory so relative links work.
5. Open `admin.html`, create an account, confirm the email if requested, sign in, and create a business profile.
6. In Supabase SQL Editor, review the new business and manually publish it after approval, using the example in `PLATFORM_ADMIN_GUIDE.sql`. Until then, the public catalogue stays private.
7. Add products and photos. Test the catalogue URL in an incognito/private browser and place a test order.
8. Test cross-business isolation with two separate accounts: verify each account only sees its own products, orders, quotations and domains.

## Custom domains — hosting requirement

The public page resolves a verified custom domain from `mc_saas_custom_domains`, or a platform-hosted slug URL such as `index.html?shop=cedar-bakery`. A custom domain is not activated just because it is entered in the admin. The platform operator must validate DNS and activate the domain row.

**Standard GitHub Pages is not suitable for serving many independent customer custom domains from one project.** Use a hosting setup that supports multiple custom domains on one deployment (for example Cloudflare Pages with custom domains configured for that project, or an equivalent edge/proxy setup). Configure the domain's DNS and HTTPS at the hosting provider first. Then review the requested domain and token and mark it verified/active using the operator-only SQL example. Do not mark a domain verified merely because a customer entered it.

The `verification_token` is generated for a future DNS TXT verification workflow; this starter does not automatically query DNS or configure custom domains at the hosting provider. Domain verification/hosting hookup is a manual platform-operator step in this version.

## Manual business approval

New businesses are created unpublished (`is_published=false`). A platform operator reviews each business, then publishes it using the SQL in `PLATFORM_ADMIN_GUIDE.sql`. A trigger blocks business owners from changing their own publication status, owner ID, or catalogue slug through the browser API.

## Security notes

- Browser code contains only the Supabase publishable key. Never place a `service_role`/secret key in any HTML, JS, JSON, or public repository.
- RLS policies scope all product, order, quotation, domain, and image writes to the authenticated business membership.
- Public visitors can read active products only for published businesses and can insert an order only for a published business. They cannot read private orders or quotations.
- Customer order insertion is intentionally anonymous, so production deployments should add rate limiting / bot protection (for example a Cloudflare Turnstile + server-side verification flow) before marketing the platform broadly.
- Driver accounts need to be assigned the `driver` role by a trusted operator. The UI does not grant staff access to itself.
- Supabase Storage image objects are public-read because product photos must display in the public catalogue. Upload/update/delete is restricted to authenticated business members by business-ID folder.
- SQL Editor actions are privileged. Treat `PLATFORM_ADMIN_GUIDE.sql` as an operator-only guide.

## Current limitations to finish before commercial launch

- Domain verification and hosting-provider custom-domain attachment are manual, not automated.
- Billing/subscription collection is not included; account approvals and payments are manual as requested.
- Team member invitation/role management and automated password-recovery UX need further polish.
- This package is a functional starter; complete end-to-end tests and RLS review in a staging Supabase project before production use.
