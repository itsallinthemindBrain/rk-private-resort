# RK Private Resort — Marketing Site

## What This Project Does
Business marketing website for RK Private Resort and Hotels. Deployed to Azure Static Web Apps via GitHub Actions.

## Tech Stack
| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML5, CSS3, JavaScript |
| Hosting | Azure Static Web Apps |
| Auth | **None — fully public site** (no login required) |
| CI/CD | GitHub Actions |

## Project Structure
```
rk-private-resort/
├── frontend/
│   ├── index.html
│   ├── login.html
│   ├── css/styles.css
│   └── assets/
│       ├── images/    # room/gallery/amenity photos; all use loading="lazy"
│       ├── js/scripts.js
│       └── profile.jpg
└── staticwebapp.config.json
```

## What scripts.js Does
- `IntersectionObserver` scroll-triggered fade-in (targets `.section` elements)
- Image hover zoom (`mouseenter` / `mouseleave` on `.gallery-image`, `.room-image`, `.amenity-image`)
- Smooth anchor scrolling (`a[href^="#"]`)
- Lazy-load class (`loaded`) applied via `IntersectionObserver` on images
- Hamburger nav toggle (`initNavToggle`) — toggles `.nav-open`, manages `aria-expanded`; closes on outside click

## Design Rules
| Element | Rule |
|---|---|
| Gallery images hover | `scale(1.1)` → reset to `scale(1)` on mouse leave |
| Room/amenity images hover | `scale(1.05)` → reset to `scale(1)` on mouse leave |
| Yellow accent | `#fbbf24` |
| Blue primary | `#3b82f6` |
| Text | White |

**CSS class name prefixes**: `.hero-*`, `.room-*`, `.amenity-*`, `.gallery-*`, `.section-*`

## Third-Party Integrations
- Facebook Messenger chat widget — CSP allows `connect.facebook.net` and `frame-src https://www.facebook.com`

## Linting (ESLint v8)
```bash
npm install
npm run lint          # HTML + CSS + JS
npm run lint:html
npm run lint:css
npm run lint:js
```

## CI/CD
- Push to `main` → auto-deploy to production
- Pull requests → get a temporary staging environment
- Lint runs **before** deploy; failure blocks the deploy

## Auth Behavior
- **No authentication required** — the site is fully public
- Any visitor can access all pages without logging in
- `login.html` and `/.auth/*` routes exist in `staticwebapp.config.json` but the main route `/*` is open to anonymous users

## Mobile Layout Rules
- Mobile-first; minimum viewport 375px (iPhone SE)
- Breakpoints: 768px (tablet), 1024px (desktop)
- Touch targets: at least 44px
- No horizontal scrolling
