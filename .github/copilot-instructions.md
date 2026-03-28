# RK Private Resort - Workspace Guidelines

A luxury resort website showcasing premium accommodations and amenities with glassmorphism UI design, deployed on Azure Static Web Apps with GitHub Actions CI/CD.

## Code Style

### HTML

- **Semantic HTML5**: Use semantic elements (`<header>`, `<section>`, `<article>`, `<footer>`) over generic `<div>`
- **Meta tags**: Include comprehensive SEO meta tags (description, Open Graph, JSON-LD structured data) for each page
- **Image optimization**: Use `loading="lazy"` on all images and descriptive alt text
- **Accessibility**: Include proper ARIA labels where needed; ensure proper heading hierarchy
- **Class naming**: Use kebab-case with clear section prefixes (e.g., `hero-logo`, `room-card`, `gallery-image`)
- See: [index.html](../../frontend/index.html) and [login.html](../../frontend/login.html) for patterns

### CSS

- **Organizational structure**: Use section markers `/* ===== SECTION NAME ===== */` to divide code into logical blocks
- **Glassmorphism design**: Apply consistent glass effect with `backdrop-filter: blur()`, `background: rgba(255, 255, 255, 0.1)`, and `border: 1px solid rgba(255, 255, 255, 0.18)`
- **Animations**: Use CSS transitions and animations for smooth interactions; prefer `transform` and `opacity` for performance
- **Responsive design**: Mobile-first approach with flexible layouts using CSS Grid and Flexbox
- **Color palette**: Yellow accent (#fbbf24), blue primary (#3b82f6), white text with appropriate opacity
- See: [styles.css](../../frontend/css/styles.css#L1-L100) for established patterns

### JavaScript

- **Vanilla JS only**: No frameworks—use modern ES6+ with `const`/`let`, arrow functions, and template literals
- **IntersectionObserver**: Use for performance-critical tasks like scroll animations, intersection detection, and image loading
- **Event handling**: Use `addEventListener` with named functions; group related listeners in logical functions
- **DOMContentLoaded**: Wrap initialization code in `DOMContentLoaded` event listener
- **Comments**: Clear, concise comments explaining complex logic; console logging for debugging only
- **Selector best practices**: Use `querySelector`/`querySelectorAll` with descriptive class names; cache frequently accessed elements
- See: [scripts.js](../../frontend/assets/js/scripts.js) for reference implementation

## Architecture

### Project Structure

```
frontend/            # Static website files (single app location)
├── index.html       # Main resort showcase page
├── login.html       # Admin login with GitHub OAuth
├── assets/
│   ├── css/         # Main stylesheet
│   ├── js/          # Interactive scripts
│   └── images/      # Hotel/resort photos (16+ images expected)
├── css/             # Alternative CSS location (css/styles.css)
└── js/              # Alternative JS location
api/                 # Backend functions (optional, currently empty)
staticwebapp.config.json  # Azure Static Web Apps routing & auth config
```

### Key Architectural Patterns

**Single-page marketing site**: All content on `index.html` with smooth scrolling navigation

- Hero section with logo and tagline
- Modular sections: Rooms, Gallery, Amenities, Contact
- Footer with quick links

**Authentication**: GitHub OAuth via Azure Static Web Apps

- `/login.html` handles unauthenticated users
- Routes in `staticwebapp.config.json` enforce authentication on root path `/*`
- `.auth/*` endpoints available for OAuth flow

**Responsive layout**: Main `.card` container with max-width 980px, centered on viewport

- Adapts padding and sizing for smaller screens
- Glassmorphic background with backdrop blur effect

## Build and Deployment

### Local Development

No build step required—open `frontend/index.html` directly in browser or use a simple HTTP server:

```bash
# Option 1: Python
python -m http.server 8000 -d frontend

# Option 2: Node.js http-server
npx http-server frontend -p 8000
```

Visit `http://localhost:8000`

### Azure Static Web Apps Deployment

**Trigger**: Automatically deploys on push to `main` branch via GitHub Actions

- Workflow file: [`.github/workflows/azure-static-web-apps-*.yml`](.github/workflows)
- App location: `frontend`
- API location: `api` (optional)
- Output location: `frontend`

**Secrets** (required in GitHub):

- `AZURE_STATIC_WEB_APPS_API_TOKEN_SALMON_GRASS_0F1311E0F` — Azure deployment token

**Live URL**: https://rk-private-resort.azurestaticapps.net/

## Conventions

### Image Assets

- **Location**: `frontend/assets/images/`
- **Naming**: Descriptive, camelCase (e.g., `BigRoom.jpg`, `SwimmingPool.jpg`, `RKLogo.jpg`)
- **Expected images**: Logo, 2 room photos, 4+ amenity/facility photos, gallery images (16+ total)
- **Lazy loading**: All images use `loading="lazy"` for performance
- **Hover effects**: Gallery/room/amenity images trigger scale transform on hover

### Contact Information Fields

Update in `index.html` `<section class="contact">`:

- Address
- Phone: +63 (953) 998-2367
- Facebook Page: RK's Private Resort and Hotels
- Hours: 24/7 Reception, Restaurant: 6:00 AM - 10:00 PM

### SEO & Meta Data

- Update structured data (JSON-LD) in `<head>` for Hotel type
- Update Open Graph tags with current URL and description
- Ensure descriptive alt text on all images for accessibility

## Critical Patterns & Gotchas

### Animation Performance

- Use IntersectionObserver to trigger animations only when elements enter viewport (prevents jank)
- Apply `fadeUp 0.6s ease-out both` animation on `.card` and `.section` for consistent entrance effects
- Transform-based animations (scale, translate) perform better than layout-affecting properties

### Image Zoom Effects

- Gallery images: `scale(1.1)` on hover
- Room/amenity images: `scale(1.05)` on hover in `scripts.js` line ~30
- Must smooth scaling back to `scale(1)` on mouse leave

### Class Naming Patterns

Follow consistent prefixes for related elements:

- `.hero-*` for header section (logo, name, tagline)
- `.room-*` for room cards (image, title, description, features)
- `.amenity-*` for amenities grid
- `.gallery-*` for gallery images
- `.section-*` for section headings and subtitles

### Glassmorphism Base Styling

Every container using glass effect should include:

```css
background: rgba(255, 255, 255, 0.1);
border: 1px solid rgba(255, 255, 255, 0.18);
backdrop-filter: blur(14px);
-webkit-backdrop-filter: blur(14px); /* Safari support */
border-radius: 20px;
```

### No Static Build Required

The frontend is pure HTML/CSS/JS—no build tools, bundlers, or transpilers needed. Files serve directly; keep them optimized and clean.

## Testing & Quality

- **Manual testing**: Verify responsive design on mobile devices (test at 375px, 768px, 1024px viewports)
- **Image optimization**: Compress images to reduce file size without visible quality loss
- **Lighthouse audit**: Aim for >90 on Performance, Accessibility, Best Practices
- **Cross-browser**: Test on Chrome, Safari, Firefox for CSS support (especially backdrop-filter)

## Quick Links

- **Repository**: GitHub (this project)
- **Deployment**: [Azure Static Web Apps Portal](https://portal.azure.com/)
- **Live Site**: https://rk-private-resort.azurestaticapps.net/
- **CI/CD Logs**: GitHub Actions in `.github/workflows`
