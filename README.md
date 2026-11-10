# USPS Quick Pickup — phone app

An independent iPhone/Android web app that requests **free USPS Carrier Pickup** for prepaid **USPS Ground Advantage** packages through the official USPS Carrier Pickup v3 API.

**Status:** Source code and Cloudflare configuration are ready for deployment. **Live USPS booking has NOT been tested or completed**. You need approved production USPS API access. This app cannot create a USPS acceptance scan at home; the carrier must collect and scan the packages.

## Deploy from GitHub to Cloudflare Workers

1. Open https://dash.cloudflare.com/ and go to **Workers & Pages → Create application → Import a repository**.
2. Select GitHub and choose `Red-Winged-King/usps-quick-pickup`. If it is missing, refresh/reconnect the repository list or update Cloudflare's GitHub App repository permissions.
3. Choose branch `main`, **Root directory** `/` (leave blank if the form accepts blank), **Build command** blank, **Deploy command** `npx wrangler deploy`.
4. The Worker name must be `usps-quick-pickup` to match `wrangler.toml`. Click **Save and Deploy**.
5. In Cloudflare's Worker **Settings → Variables and Secrets**, create **three Secrets**:
   - `USPS_CLIENT_ID` — Consumer Key from the USPS Developer Portal.
   - `USPS_CLIENT_SECRET` — Consumer Secret from the USPS Developer Portal.
   - `APP_ACCESS_KEY` — your own *long private password* (at least 16 characters), **never added to GitHub**.
6. Confirm you are using **approved USPS Carrier Pickup v3 production access**, not only a USPS.com login. Add a **USPS_PACKAGE_TYPE** plain-text variable only if the default `USPS_GROUND_ADVANTAGE` is rejected and USPS verifies a different package-type value.
7. Open your Worker’s `https://...workers.dev` URL on iPhone in Safari. Complete the one-time form with your **own** pickup address/contact details, where you leave packages, carrier instructions, and `APP_ACCESS_KEY`. Tap **Save setup**.
8. Safari → Share → **Add to Home Screen**. Next time, enter the number of Ground Advantage packages, **combined weight in pounds**, then schedule the earliest available free pickup.

You only get an actual confirmation number if USPS responds with one. Keep the confirmation email. The app attempts to prevent duplicate requests on one phone, but this is not a guarantee across devices; verify USPS before retrying after a timeout.

## Privacy/security

- Never commit personal addresses, phone numbers, USPS credentials, access keys, or order details.
- USPS secrets live in Cloudflare (server-side). Your address, contact details and access key are saved in the browser local storage on your phone; anyone with access to the unlocked phone might access them.
- The Worker uses `APP_ACCESS_KEY` to restrict the scheduling endpoints, and does not expose USPS credentials in HTML.
- For multi-user/public deployment, add stronger authentication and server-side rate limiting.

## Repository files

- `worker.js`: complete Worker with mobile UI and Carrier Pickup API integration.
- `wrangler.toml`: Cloudflare deployment configuration.
- `README.md`: these setup instructions.

Official docs:
- https://developers.usps.com/carrierpickupv3
- https://developers.usps.com/getting-started
- https://developers.cloudflare.com/workers/ci-cd/builds/

Not affiliated with USPS or Depop.
