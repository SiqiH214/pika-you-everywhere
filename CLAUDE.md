# CLAUDE.md — Pika: You, Everywhere

## Project Overview

Static marketing website for **Pika AI Self** — a product that creates AI personas living across messaging platforms (iMessage, Slack, Discord, X, etc.). The site showcases the concept and features individual AI Self profile pages.

**Live URL**: https://pika-you-everywhere.vercel.app
**Deployment**: Vercel (static hosting, no build step)

## Tech Stack

- **Pure HTML/CSS/JS** — no frameworks, no build tools, no package.json
- **No external dependencies** — everything is self-contained
- **Vanilla JavaScript** — Intersection Observer API for scroll animations, scroll-snap carousels
- **CSS Custom Properties** for theming
- **Inline styles** — all CSS lives inside `<style>` tags in each HTML file (no external stylesheets)

## File Structure

```
/
├── index.html              # Main landing page (~2,866 lines)
├── og-image.html           # Open Graph image generator (58 lines)
├── 47.html                 # AI Self profile: 47 (Siqi's)
├── ava.html                # AI Self profile: Ava Purple
├── cami.html               # AI Self profile: Cami
├── darius.html             # AI Self profile: Darius
├── jessica.html            # AI Self profile: Jessica
├── jessie.html             # AI Self profile: Jessie
├── raccoon.html            # AI Self profile: Raccoon 2.0
├── semi.html               # AI Self profile: Semi
├── shiro.html              # AI Self profile: Shiro
├── fonts/
│   ├── Telka-Extended-Regular.otf   # weight 400
│   ├── Telka-Extended-Medium.otf    # weight 500
│   └── Telka-Extended-Bold.otf      # weight 700
├── favicon.ico
├── og-preview.png
├── *-avatar.png/jpg        # Profile avatars for each AI Self
├── *-1.png through *-4.png # Gallery images per AI Self
├── *-video-*.mp4           # Video content (raccoon, shiro)
├── *-thumb-*.png           # Video thumbnails
└── *-logo.svg              # Platform icons (slack, discord, x, etc.)
```

## Page Architecture

### index.html (Landing Page)

The main page has these sections in order:

1. **Nav** — Fixed header with Pika wordmark SVG + "Create Yours" CTA
2. **Hero** — Full viewport, animated blob background, staggered text entrance
3. **iMessage Born Section** — Messages integration showcase
4. **Carousel Section** — Horizontal scroll-snap use-case cards
5. **Living AI Selves** — Fan/arc layout of clickable AI Self profile cards
6. **Multi-Platform Section** — Grid of platform pills
7. **Skills Section** — Accordion-style categories with 4-column grid
8. **Statement Section** — CTA text block
9. **Final CTA** — Login button

Key JS class: `ScrollingChatAnimator` manages animated chat message sequences with typing indicators.

### Profile Pages (ava.html, darius.html, etc.)

All profile pages follow the same template structure (~170-235 lines each, minified CSS):

1. **Nav** — Fixed, with back link to index.html
2. **Hero** — Avatar + name + role + owner badge
3. **Where I Live** — Platform cards (Slack, X, Discord, etc.)
4. **Examples** — 4-column image/video gallery
5. **Skills** — Numbered list with descriptions and use cases
6. **Footer** — Navigation back to main page

Exception: `47.html` (436 lines) is more elaborate with expanded styling.

## Design System

### CSS Variables

```css
--pika-key: #CFC3FF        /* Purple accent */
--pika-key-50: rgba(207, 195, 255, 0.5)
--pika-blob: #E2DCEE        /* Light purple */
--pika-black: #0D0D0D       /* Near-black text */
--pika-white: #FFFFFF
--pika-eggshell: #FDF7EF    /* Warm off-white background */
--pika-cream: #F8F4ED
--pika-warm: #6E6D6B        /* Warm gray for secondary text */
--work: #A8E6CF             /* Green tag */
--life: #FFD3B6             /* Peach tag */
--emotion: #FFAAA5          /* Salmon tag */
```

### Easing Curves

```css
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1)
--ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1)
--ease-in-out-expo: cubic-bezier(0.87, 0, 0.13, 1)
--spring: cubic-bezier(0.34, 1.56, 0.64, 1)
```

### Typography

- **Font**: Telka Extended (custom, loaded via `@font-face` from `fonts/` directory)
- **Weights**: 400 (Regular), 500 (Medium), 700 (Bold)
- **Fallback stack**: `-apple-system, system-ui, sans-serif`
- **Fluid sizing**: Uses `clamp()` for responsive text, e.g. `font-size: clamp(32px, 5vw, 48px)`

### Responsive Breakpoints

| Breakpoint | Target |
|------------|--------|
| 768px | Tablet |
| 600px | Phone |
| 500px | Small phone |

Patterns: 4-col grid → 2-col → 1-col. Padding reduces from 60px to 24px.

## Conventions and Patterns

### Class Naming

BEM-inspired but not strict. Uses semantic prefixes:

- Sections: `.hero`, `.selves-section`, `.carousel-section`, `.skills-section`
- Elements: `.hero-title`, `.hero-subtitle`, `.platform-card`, `.skill-item`
- States: `.visible`, `.open`, `.exiting`
- Components: `.chat-*`, `.card-*`, `.fan-*`, `.btn-*`

### Animation Pattern

All scroll-triggered animations use the same approach:

```css
.reveal { opacity: 0; transform: translateY(24px); transition: opacity 0.8s var(--ease-out-expo), transform 0.8s var(--ease-out-expo); }
.reveal.visible { opacity: 1; transform: translateY(0); }
```

Triggered via Intersection Observer in JavaScript. Use `transform` and `opacity` only for GPU-accelerated performance.

### Section Header Pattern

Each section follows a consistent hierarchy:

```html
<div class="section-tag">TAG TEXT</div>
<h2 class="section-title">Title</h2>
<p class="section-desc">Description</p>
```

Section tags: uppercase, small, letter-spaced, warm gray color.

### Button Styles

- `.btn-primary` — Rounded pill, dark background, light text, hover lift
- `.btn-imessage` — iMessage-styled green button

### Profile Page CSS

Profile pages use **minified inline CSS** (single-line rules) as opposed to the formatted CSS in index.html. When editing profile pages, maintain this minified style.

## Development Workflow

### No Build Step

There is no build process. Edit HTML files directly and open them in a browser or deploy to Vercel.

### Deployment

Push to `main` on GitHub. Vercel auto-deploys static files — no configuration needed.

### Adding a New AI Self Profile

1. Copy an existing profile page (e.g., `ava.html`) as a template
2. Update the `<title>` tag: `{Name} — AI Self | Pika`
3. Replace avatar image, name, role, quote, and owner badge
4. Update platform cards in "Where I Live" section
5. Add gallery images (naming: `{name}-1.png` through `{name}-4.png`)
6. Update skills list
7. Add a card linking to the new page in `index.html`'s AI Selves section

### Asset Naming Conventions

- Avatars: `{name}-avatar.png` or `{name}-avatar.jpg`
- Gallery images: `{name}-{n}.png` or `{name}-{n}-{descriptor}.jpg`
- Videos: `{name}-video-{n}.mp4`
- Thumbnails: `{name}-thumb-{n}.png`
- Platform icons: `{platform}-logo.svg`

## Important Notes

- **All assets are in the root directory** — no subdirectories for images/videos
- **Large media files** — many PNGs are 1-5 MB; consider optimizing before adding new ones
- **No .gitignore** — all files are tracked
- **Inline CSS only** — do not create external `.css` files; keep styles in `<style>` tags within each HTML file
- **No JS frameworks** — keep JavaScript vanilla; avoid adding npm dependencies
- **Preserve the grain overlay** — `body::before` adds a subtle texture overlay on index.html
- **Intersection Observer** is used for all scroll animations — do not use scroll event listeners
- **Profile pages are self-contained** — each HTML file includes its own complete CSS; do not extract shared stylesheets
