# RK Private Resort — Marketing Site

## What This Project Does
Business marketing website for RK Private Resort and Hotels. Deployed to Azure Static Web Apps via GitHub Actions.

## Tech Stack
| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML5, CSS3, JavaScript |
| Hosting | Azure Static Web Apps |
| Auth | GitHub OAuth via `staticwebapp.config.json` |
| CI/CD | GitHub Actions |

## Project Structure
```
rk-private-resort/
├── frontend/
│   ├── index.html
│   ├── login.html
│   ├── css/styles.css
│   └── assets/
│       ├── images/    # camelCase filenames, all use loading="lazy"
│       └── js/scripts.js
└── staticwebapp.config.json
```

## What scripts.js Does
- Scroll-triggered fade-in animations
- Image hover zoom effects
- Smooth anchor scrolling

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
- GitHub OAuth enforced on all routes except `/login.html` and `/.auth/*`
- Auth is bypassed when running locally

## Mobile Layout Rules
- Mobile-first; minimum viewport 375px
- Breakpoints: 768px (tablet), 1024px (desktop)
- Touch targets: at least 44px
- No horizontal scrolling
