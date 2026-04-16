# Luxury Property Website Generator — Prompt for Cursor Opus 4.6

Copy the entire prompt below into a new Cursor chat with Opus 4.6. Place your video file in the project folder before running.

---

## PROMPT — COPY EVERYTHING BELOW THIS LINE

I have one video file in this folder for **6 E Gate Road, Wainscott, NY 11975** (MLS #391487). I need you to build a world-class luxury property listing website from it. Follow every step below precisely.

### STEP 1: Probe the video

Use `ffprobe` to get the video's resolution, duration, frame rate, and codec. Report the results before continuing.

### STEP 2: Extract stills with FFMPEG

Create a `stills/` directory. Use FFMPEG to extract 17 high-quality stills at evenly distributed intervals throughout the video. Scale them to 1920px wide. Use `-q:v 2` for JPEG quality. Use the `select` filter to pick specific frame numbers spread across the full duration — don't just use fps-based extraction. Name them `frame_001.jpg` through `frame_017.jpg`.

### STEP 3: Compress the video for web

Use FFMPEG to create `hero-video.mp4` — a web-optimized version of the source video:
- Scale to 1920x1080
- H.264 High profile, level 4.1
- CRF 28, preset `slow`
- Strip all audio (`-an`)
- Add `-movflags +faststart` for progressive loading
- Target should be under 50MB

### STEP 4: View every still

Open and examine every single extracted still (`frame_001.jpg` through `frame_017.jpg`) so you understand what the video shows — the property exterior, interior rooms, pool, landscaping, lifestyle shots, etc. You need this context to write accurate alt text and choose which images go where.

### STEP 5: Research the property

Search the web for **6 E Gate Road, Wainscott, NY 11975 MLS #391487** to find:
- Price, bedrooms, bathrooms, square footage, lot size, year built
- Key interior features (kitchen, bathrooms, flooring, ceilings)
- Key exterior features (pool, deck, garage, landscaping)
- Any sustainability or luxury features
- The listing agent/brokerage

### STEP 6: Research local dining

Search the web for the top 4 restaurants near Wainscott / East Hampton / the Hamptons. For each, find:
- Restaurant name
- Cuisine type / vibe
- A 1-2 sentence description

These will be used in the Dining section (no images needed — text-only cards).

### STEP 7: Build the website

Create a single self-contained `index.html` file. Use the 26 Alewive Brook site (https://github.com/boardc3/alewive) as the exact structural template. The site must include ALL of the following sections in this order:

#### 7a. Architecture & Design System
- Google Fonts: Cormorant Garamond (serif) + Inter (sans)
- No Leaflet.js — there is no interactive map
- CSS custom properties for all colors with WCAG AA contrast ratios:
  - `--body-text` ~7:1 on ivory for body copy
  - `--gold-text` ~5.5:1 on ivory for accent labels on light backgrounds
  - `--gold-on-dark` ~6.5:1 on dark for accent labels on dark backgrounds
  - `--gold` for decorative use only (markers, borders, backgrounds)
- All body text `font-weight: 400` minimum (never 300)
- All text-on-image overlays must have sufficient gradient darkening

#### 7b. Accessibility (build this in from the start)
- Skip-to-content link as first element in `<body>`
- `<main id="main-content">` wrapping all content sections
- `<nav>` with `aria-label="Main navigation"`
- `<footer>` with `role="contentinfo"`
- Every `<section>` gets `aria-label` describing its purpose
- All decorative SVG icons: `aria-hidden="true"`
- All decorative image breaks: `role="presentation"`
- `*:focus-visible` with visible blue focus ring (`3px solid #4A90D9`)
- `:focus:not(:focus-visible)` removes outline for mouse clicks
- `@media (prefers-reduced-motion: reduce)` — disable all animations, transitions, hide autoplay video
- Hamburger button: `aria-label`, `aria-expanded`, spans get `aria-hidden="true"`
- Do NOT include Leaflet.js CSS or JS (no interactive map)

#### 7c. Hero Section
- `<video>` element as the sole hero background with:
  - `autoplay muted playsinline loop preload="auto"`
  - `poster="stills/frame_XXX.jpg"` (use the best exterior/hero still)
  - `<source src="hero-video.mp4" type="video/mp4">`
  - `object-fit: cover` filling the viewport
  - `aria-hidden="true"`
- Dark gradient overlay on top for text contrast
- Property name, location, tagline, and stats (beds/baths/sqft/acres)
- Scroll hint at bottom (aria-hidden)
- JS: `const prefersReducedMotion = ...` MUST be declared BEFORE any code that references it. Then call `heroVideo.play().catch(() => {})` as a belt-and-suspenders backup. If reduced motion is preferred, pause and hide the video.
- Hero has `background: #0d0d0d` as fallback while poster loads

#### 7d. Intro / Residence Section
- Two-column grid: text left (eyebrow, title, 2 paragraphs of property description), image right
- Use the best daytime exterior still
- Descriptive alt text on the image

#### 7e. Photo Gallery
- Dark background (`#0d0d0d`)
- 12-column CSS grid with 7 images in 3 rows:
  - Row 1: `col 1/8` + `col 8/13` (7+5)
  - Row 2: `col 1/5` + `col 5/9` + `col 9/13` (4+4+4)
  - Row 3: `col 1/7` + `col 7/13` (6+6)
- Each image: `tabindex="0"`, `role="listitem"`, `aria-label` with "press Enter to enlarge"
- Keyboard support: Enter/Space opens lightbox
- Choose 7 stills that show variety: exterior, living room, kitchen, bedroom, bathroom, lifestyle, pool/outdoor

#### 7f. Full-Width Image Breaks
- 3 total, placed between sections (after gallery, after amenities, after dining)
- `role="presentation"`, descriptive alt text, `height: 65vh`, `object-fit: cover`

#### 7g. Amenities Section
- 8 amenity cards in `repeat(4, 1fr)` grid (NOT auto-fit)
- Each card: SVG icon (`aria-hidden`), h3 title, paragraph description
- Tailor amenities to what this specific property actually has

#### ~~7h. Attractions Section~~ — REMOVED
Do NOT include an attractions/explore section with image cards. Skip this entirely.

#### 7i. Dining Section
- Light background (`#f5f2ee`)
- 4 restaurant cards in `repeat(4, 1fr)` grid
- Each: h3 name, cuisine type span, description paragraph
- Use `<article>` elements

#### ~~7j. Interactive Map Section~~ — REMOVED
Do NOT include an interactive map. No Leaflet, no map sidebar, no POI data, no map-related CSS or JS.

#### 7k. Getting Here Section
- 4 travel cards in `repeat(4, 1fr)` grid
- From Manhattan, By Train, By Air, By Jitney (adjust if property warrants different options)

#### 7l. CTA Section
- Dark background, centered
- "Begin Your [Location] Story" heading
- Realtor info block: **Richard Mike Moran** · Real Estate Sales Person, Douglas Elliman Real Estate · (212) 579-1919 (phone number linked with `tel:`)
- "Request Information" mailto link to **mike.moran@elliman.com** styled as button

#### 7m. Footer
- Dark background, property address, copyright
- Credit line: "Listing by Douglas Elliman Real Estate · Richard Mike Moran · (212) 579-1919"

#### 7n. Lightbox
- `role="dialog"`, `aria-modal="true"`, `aria-label="Image viewer"`
- Close (×), Previous (‹), Next (›) buttons — all 44px+ touch targets
- `aria-live="polite"` counter ("3 of 7")
- Dynamic alt text pulled from source gallery image
- Focus trap (Tab cycles within lightbox buttons)
- Escape closes, ArrowLeft/ArrowRight navigates
- Restores focus to triggering element on close

#### 7o. Responsive Breakpoint (max-width: 900px)
- Nav links hidden, hamburger shown
- All grids collapse: amenities → 2col, dining → 1col, travel → 2col
- Gallery: 2-column with full-width hero images

### STEP 8: Open the site locally for testing

Run `open index.html` to launch the site in the default browser. Verify:
1. The hero video plays automatically with the poster image visible while it loads
2. All 7 gallery images display and the lightbox opens on click/Enter
3. All 3 full-width image breaks load
4. All 6 attraction card images load (curl each Unsplash URL to confirm HTTP 200)
5. The interactive map renders with all markers and the sidebar works
6. Category filter buttons show/hide markers correctly
7. Clicking a POI in the sidebar flies the map to that location
8. The lightbox keyboard navigation works (Escape, ArrowLeft, ArrowRight)
9. Tab through the entire page to confirm focus indicators are visible
10. The page looks correct on a narrow browser window (responsive layout)

Do NOT initialize git, push to GitHub, or deploy anywhere. We will test locally first and deploy later when I tell you to.

### KEY LESSONS LEARNED (avoid these mistakes)

1. **Never reference a `const` before its declaration** — JavaScript's temporal dead zone will crash the entire script silently. Declare `prefersReducedMotion` BEFORE the hero video code.
2. **Always add `autoplay` alongside `muted`** on background videos — browsers require both attributes together.
3. **Always verify Unsplash URLs** with curl before using them — they go dead frequently.
4. **Use explicit grid columns** (`repeat(4, 1fr)`) not `auto-fit` with `minmax` — auto-fit creates incomplete rows at certain viewport widths.
5. **Use `font-weight: 400`** minimum for body text, never 300 — thin fonts fail contrast checks even when the color ratio passes.
6. **Hero video should be simple** — just a `<video>` element with `autoplay muted playsinline loop`, `object-fit: cover`, and a `poster` fallback. No opacity tricks, no JS fade-in detection, no hidden states.
7. **The `.gitignore` should exclude the original large video** but NOT the compressed `hero-video.mp4`.
8. **GitHub Pages needs a workflow file** when `build_type=workflow` — create the deploy.yml in `.github/workflows/`.
