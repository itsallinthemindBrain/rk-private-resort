# RK Private Resort and Hotels

Marketing site for RK Private Resort and Hotels — a boutique 2-storey family resort in Echague, Isabela, Philippines. Goal: drive booking inquiries via Facebook Messenger.

## Tech Stack

- **Frontend**: HTML5 / CSS3 / Vanilla JS (no framework, no build step)
- **Hosting**: Azure Static Web Apps (Free tier)
- **Auth**: GitHub OAuth via Azure Static Web Apps built-in auth
- **Messenger**: Facebook Customer Chat Plugin (page ID `61559916437724`)
- **CI/CD**: GitHub Actions — lint gates deploy, PR staging environments

## Project Structure

```
rk-private-resort/
├── frontend/
│   ├── index.html                  # Main marketing site (auth-required)
│   ├── login.html                  # GitHub OAuth login (public)
│   ├── css/styles.css              # Glassmorphism design, mobile-first
│   └── assets/
│       ├── js/scripts.js           # Fade-in, hover zoom, smooth scroll, nav toggle
│       └── images/                 # Resort photos (see image list below)
├── .github/workflows/
│   └── azure-static-web-apps-salmon-grass-0f1311e0f.yml  # CI/CD
├── staticwebapp.config.json        # Routes, auth, security headers, CSP
├── package.json                    # Lint scripts + dev dependencies
├── .eslintrc.json                  # ESLint v8 legacy config
├── .htmlhintrc                     # HTMLHint rules
└── .stylelintrc.json               # Stylelint rules
```

## Local Development

```bash
# Serve locally (auth is bypassed — staticwebapp.config.json rules only apply on Azure)
python -m http.server 8000 -d frontend
# then open http://localhost:8000
```

## Linting

```bash
npm install

npm run lint          # Run all linters (HTML + CSS + JS)
npm run lint:html     # HTMLHint on frontend/*.html
npm run lint:css      # Stylelint on frontend/css/**/*.css
npm run lint:js       # ESLint on frontend/assets/js/**/*.js
```

Linting runs automatically in CI before every deploy.

## Design Decisions

- **Color palette**: Yellow `#fbbf24` (warm, welcoming, resort feel) + Blue `#3b82f6` (trust, water/pool theme), used as a radial gradient background.
- **Glassmorphism**: `backdrop-filter: blur(14px)`, `rgba(255,255,255,0.1)` card — premium aesthetic without heavy image assets.
- **Hover scales**: Gallery images `scale(1.1)`, room/amenity images `scale(1.05)` — subtle depth cue on hover.
- **Primary CTA**: Yellow "Message Us to Book" button → Facebook Messenger deep link (`https://m.me/61559916437724`). Yellow accent stands out against the dark glassmorphism card.
- **Persistent chat**: Facebook Customer Chat Plugin floats on all pages — main conversion channel for booking inquiries.
- **Nav**: Sticky semi-transparent navbar with hamburger on mobile; links jump to each section.

## Images

Required in `frontend/assets/images/`:

| File | Used in | Description |
|---|---|---|
| `RKLogo.jpg` | Hero | Resort logo |
| `BigRoom.jpg` | Rooms | Big Family Room — Ground Floor |
| `BigRoom2.jpg` | Rooms | Big Family Room — First Floor |
| `Exterior.jpg` | Gallery | Hotel exterior |
| `Dinning.jpg` | Gallery | Dining area |
| `DeluxeInterior.jpg` | Gallery | Deluxe room interior |
| `FamilyVilla.jpg` | Gallery | Family villa |
| `SwimmingPool.jpg` | Amenities | Swimming pool |
| `Conference.jpg` | Amenities | Conference facilities |

## Content Update Guide

### Updating contact info
Edit `frontend/index.html`:
- **Phone**: update the `<p>` in the Phone `contact-item` AND `href="tel:..."` in the footer
- **Address**: update the `<p>` in the Address `contact-item` AND `streetAddress` in the JSON-LD block
- **Email**: update `href="mailto:..."` in the footer AND `"email"` in the JSON-LD block

### Adding a room
1. Add a room photo to `frontend/assets/images/`
2. Copy one `.room-card` block in `index.html`
3. Update `<img src>`, `<h3>`, `<p>`, and `.feature` spans

### Updating Facebook page ID
The page ID appears in two places in `index.html`:
1. `<div class="fb-customerchat" page_id="...">` — the chat widget
2. `href="https://m.me/..."` in the `.cta-btn` — update the numeric ID in the URL

## CI/CD

| Trigger | Action |
|---|---|
| Push to `main` | Lint then deploy to production |
| PR opened/updated | Lint then deploy to staging environment |
| PR closed | Staging environment torn down |

Linting gates deployment: `npm run lint` runs before the Azure deploy step. A lint failure stops the deploy.

## Required GitHub Secret

| Secret | Used by |
|---|---|
| `AZURE_STATIC_WEB_APPS_API_TOKEN_SALMON_GRASS_0F1311E0F` | App deploy workflow |

## Security

Security headers set globally in `staticwebapp.config.json`:
- `Content-Security-Policy` — strict self-origin; explicitly allows Facebook SDK (`connect.facebook.net`), Messenger chat API (`connect-src`), and Facebook CDN images (`img-src`)
- `Strict-Transport-Security` — 2-year HSTS with preload
- `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy` — disables camera, microphone, geolocation
