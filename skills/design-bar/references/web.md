# Web: websites, landing pages, web apps

This is how the design-bar laws are obeyed in a browser. Default stack is
whatever the project already uses; when there is a choice for a marketing
site, prefer a static generator (Astro) with zero client JS by default and
islands only where interaction needs them.

## Platform baseline

- **Semantic HTML first**: `<header>`, `<nav>`, `<main>`, `<section>`,
  `<article>`, `<footer>`, real `<button>` and `<a>` (a link navigates, a
  button acts, never a `div onClick`). One `<h1>` per page, logical
  heading nesting, landmarks and `aria-label` where the structure needs it.
- **Design tokens on `:root`**: colors, type scale, spacing scale, radii,
  shadows, motion durations. Dark mode via `prefers-color-scheme` and an
  explicit `data-theme` override; `color-scheme: light dark` on `:root` so
  form controls and scrollbars follow.
- **Mobile first, fluid**: base styles for 360 px, then `min-width`
  breakpoints (roughly 640, 768, 1024, 1280). Fluid type with `clamp()`, a
  max content width (about 65–75 characters for prose, 1200–1280 px for
  layouts), 16 px side gutters on phones, never a horizontal scroll.
- **Fonts**: one or two families, self-hosted or Google Fonts with
  `font-display: swap`, subsets, preloaded for the hero. System stack is a
  valid choice. Tabular numerals via `font-variant-numeric: tabular-nums`.
- **Images**: `<img>` with explicit `width`/`height` (or `aspect-ratio`)
  so nothing shifts, `loading="lazy"` below the fold, `fetchpriority="high"`
  on the LCP image, modern formats (AVIF/WebP) with sensible `srcset`.
- **Icons**: one set (Lucide, Phosphor, Heroicons…) as inline SVG, sized
  from the type scale, `aria-hidden` when decorative.

## Native fidelity laws, web edition

1. **Native controls over rebuilt ones**: `<select>`, `<input type=date>`,
   `<dialog>`, `<details>`, `popover`, native checkbox and radio styled
   with `accent-color`. Rebuild only when the design genuinely diverges,
   and then keep keyboard behavior and focus identical to the native one.
2. **Focus is visible.** `:focus-visible` rings on every interactive
   element, in the accent or a high-contrast ring, never `outline: none`
   without a replacement.
3. **Hover is not a state the product depends on.** Everything reachable
   on hover is reachable on tap and keyboard.
4. **Tap targets ≥ 44 px** on touch, ≥ 24 px with spacing on pointer.
5. **Forms**: labels above fields (never placeholder-as-label), the right
   `type`, `inputmode` and `autocomplete`, validate on blur or submit,
   inline errors beneath the field that stay until fixed, one submit.
6. **Scroll is the platform's.** No scroll-jacking, no hijacked wheel, no
   full-page snap unless the content is genuinely slides. Sticky headers
   shrink, not jump.
7. **Text is selectable, links are links** (right-click, middle-click and
   copy-address all work), the browser back button always works, and
   every state a user might share has a URL.

## Layout grammar for marketing pages

Sections, in the order the references show (verify on Mobbin with
`search_sections`), each with one job:

1. **Hero**: one headline (under 10 words), one sub-line, one primary CTA
   (a secondary text link at most), one visual. Above the fold at 360 px
   the headline and CTA are visible.
2. **Proof**: logo bar, a number, or one quote. Immediately after the hero.
3. **How it works / features**: 3 or 4 items, each a verb-led title and one
   sentence. Real product visuals beat icons in tinted circles.
4. **Pricing** (if any): tiers side by side on desktop, stacked on phone,
   one highlighted plan, the toggle (monthly/annual) if it exists is native
   and obvious.
5. **FAQ**: `<details>` elements, 5–8 questions.
6. **Final CTA**: the same primary label as the hero.
7. **Footer**: legal, contact, social, secondary nav; small and quiet.

Content budget per section is part of the spec: headline words, number of
items, one CTA. More than one primary per section is a finding.

## Motion, web specifics

- CSS transitions and `@keyframes` first, the Web Animations API second,
  a JS library (Motion, GSAP) only when a spring or gesture needs it.
- `transform` and `opacity` only; never animate `width`, `height`, `top`,
  `left`, `box-shadow` on anything that repeats.
- Durations 150–300 ms, `cubic-bezier(0.23, 1, 0.32, 1)`; hover feedback
  under 150 ms; section reveals on scroll (IntersectionObserver or
  `animation-timeline: view()`) once, subtle (8–16 px translate + fade),
  never re-triggering on scroll back.
- `@media (prefers-reduced-motion: reduce)`: collapse spatial motion to
  fades or nothing; keep essential state changes.
- View Transitions API for page navigations when the framework supports it
  (Astro does); otherwise no fake page transitions.

## Perceived performance and Core Web Vitals

Budgets to hold on a mid-range phone over 4G, measured with Lighthouse
(Chrome DevTools MCP `lighthouse_audit`) or PageSpeed:

| Metric | Budget |
|---|---|
| LCP | < 2.5 s (target < 1.5 s for a static marketing page) |
| CLS | < 0.05 |
| INP | < 200 ms |
| Initial JS on a marketing page | 0–50 KB; islands only |
| Page weight above the fold | < 500 KB |
| Lighthouse performance / accessibility | ≥ 95 / 100 |

- No layout shift from fonts (size-adjust or matched fallback), images
  (dimensions), or late-injected banners.
- Critical CSS inline for the hero when the framework supports it; the rest
  deferred. No render-blocking third-party scripts in `<head>`.
- Cache static assets with hashed filenames and long `Cache-Control`.

## SEO and sharing (marketing pages)

- `<title>` (under 60 chars), meta description (under 160), canonical,
  Open Graph and Twitter Card with a real 1200×630 image, `lang` on
  `<html>`, `robots.txt`, `sitemap.xml`, JSON-LD (`Organization`,
  `WebSite`, `SoftwareApplication` or `Product` where it applies).
- Text in HTML, not in images. Headings carry the page's argument.

## Verification loop, web

1. Run the dev server, open it in a real browser (agent-browser or Chrome
   DevTools MCP), at **360 × 800, 768 × 1024 and 1280 × 800** minimum.
2. Screenshot each width in light and dark and actually look: no
   horizontal scroll, gutters hold, headline wraps by design, images are
   not cropped wrong, nothing overlaps the sticky header.
3. Zoom to 200% (or set root font to 32 px): layout still works.
4. Keyboard walk: Tab through the page; every control reachable, focus
   visible, order logical, `Esc` closes dialogs and menus.
5. Record the motion (scroll reveals, menus, dialogs, hover states) and
   scrub: no pops, no reveal that re-fires, no jank on scroll.
6. Lighthouse on the built output, not the dev server. Fix every
   accessibility finding; hold the budgets above.
7. Throttle CPU 4× and network to Fast 3G once: nothing visibly breaks,
   above-the-fold content appears first.

The full checklist is in [verification.md](verification.md).
