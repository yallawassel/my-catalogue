MY CATALOGUE SAAS — PATCHED DEPLOYMENT PACKAGE

This package preserves the existing My Catalogue multi-business SaaS and uses the My Catalogue Supabase project. It does not replace or modify the separate White Pack site.

Included files are named for direct upload:
- admin.html
- index.html (patched)
- saas.js
- orders.html
- driver-tracking.html
- platform-admin.html
- manifest.webmanifest
- icon-192.png
- icon-512.png
- products.json (unchanged source/import backup)
- PLATFORM_ADMIN_GUIDE.sql
- README_PATCHED.txt

Changes in index.html:
1. Blank/null prices are treated as unpriced, not as zero-priced products.
2. WhatsApp checkout opens a temporary tab immediately on the customer's tap, before awaiting the Supabase save. This helps prevent mobile popup blocking.
3. If the Supabase order save fails, the WhatsApp message can still be sent, and the status message explains the online save failure.

Deployment:
1. Download and extract this ZIP.
2. Upload all files to the SAME directory in your My Catalogue GitHub repository, replacing the existing files with the same names.
3. Keep saas.js beside all HTML pages. Keep manifest.webmanifest and both icon PNGs beside index.html.
4. Do not copy these files over the separate White Pack site.
5. Public catalogues use index.html?shop=YOUR-BUSINESS-SLUG unless a verified custom domain is configured.
6. A business must be approved/published by the platform owner before its public catalogue is visible.

Validation:
The inline JavaScript in admin.html, index.html, orders.html, driver-tracking.html, and platform-admin.html passed Node's syntax checker after the patch. This does not prove live Supabase policies, OAuth provider configuration, or GitHub Pages settings are correct; those require testing on the deployed site.

Suggested test order:
- Sign in to admin
- Add/edit one test product
- Import a small JSON file
- Preview the business catalogue using its ?shop=slug link
- Add a product to cart and send a test WhatsApp order
- Confirm the order appears in orders.html
- Test driver sign-in and location sharing
