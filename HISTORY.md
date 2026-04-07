# Change History

## 2026-04-07
**Changed:** Security hardening — removed CSP `unsafe-inline`, added Google Analytics 4, expanded `.gitignore`
**Reason:** Security audit found `'unsafe-inline'` in both `script-src` and `style-src`, defeating XSS protection. Fixed by extracting Facebook SDK inline script to `assets/js/facebook-sdk.js`. Added Google Analytics 4 infrastructure (`assets/js/analytics.js`) for daily visitor tracking — replace `G-XXXXXXXXXX` with real Measurement ID from Google Analytics console. Expanded `.gitignore` to cover `.env`, secrets, and credentials.

## 2026-04-07 (2)
**Changed:** Fixed CI lint failure on new JS files
**Reason:** ESLint `no-undef` errors on `analytics.js` (`dataLayer` global) and `facebook-sdk.js` (`FB` global). Fixed by using `window.gtag`/`window.dataLayer` in analytics.js and adding `/* global FB */` declaration in facebook-sdk.js.

## 2026-04-07 (3)
**Changed:** Corrected CLAUDE.md — site is fully public, no authentication required
**Reason:** CLAUDE.md incorrectly stated GitHub OAuth was enforced. Verified via incognito browser test that the site is publicly accessible to all visitors without any login.

## 2026-04-06
**Changed:** Cloned and set up on new laptop
**Reason:** New machine setup; npm dependencies installed.
