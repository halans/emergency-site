# Implementation Plan

A lightweight, performance-optimized, and highly accessible Hugo starter kit for emergency information websites.

## Design Philosophy

> **Emergency-First Design**: Every decision prioritizes getting critical information to people in crisis—poor connectivity, screen readers, stressed users, damaged infrastructure.

### Core Principles

1. **< 14KB First Load** — Fits in first TCP roundtrip
2. **Zero JavaScript Required** — Progressive enhancement only
3. **WCAG AAA Accessibility** — 7:1+ contrast, keyboard navigation
4. **Offline-First** — Service worker caching
5. **Semantic HTML** — Proper structure for assistive tech

---

## Project Structure

```
hugo-emergency-site/
├── src/                          # Hugo source code
│   ├── archetypes/               # Content templates
│   │   ├── updates.md
│   │   ├── faq.md
│   │   └── prepkit.md
│   ├── content/                  # Markdown content
│   │   ├── _index.md
│   │   ├── offline.md
│   │   ├── updates/
│   │   ├── faq/
│   │   └── prepkit/
│   ├── themes/emergency/         # Portable theme
│   │   ├── assets/css/
│   │   │   └── critical.css      # Inlined styles
│   │   ├── layouts/
│   │   │   ├── _default/
│   │   │   ├── partials/
│   │   │   ├── 404.html
│   │   │   └── index.html
│   │   ├── static/
│   │   │   ├── icon.svg
│   │   │   ├── manifest.json
│   │   │   └── sw.js
│   │   └── README.md
│   └── hugo.toml                 # Site configuration
├── docs/                         # Documentation
├── cloudflare.toml               # Deployment config
├── .gitignore
└── README.md
```

---

## Theme Architecture

The theme in `src/themes/emergency/` is portable:

1. Used as part of this starter kit (default)
2. Installed as Git submodule in another Hugo site
3. Copied directly to another site's `themes/` directory

### CSS Inlining

To prevent editor reformatting of Hugo syntax, CSS inlining uses a separate partial:

```
head.html → inline-css.html → critical.css
```

---

## Accessibility Checklist

| Feature | Implementation |
|---------|----------------|
| Skip Link | First focusable element |
| Landmarks | `<header>`, `<nav>`, `<main>`, `<footer>` |
| Headings | Logical h1→h6 hierarchy |
| Color Contrast | 7:1 minimum (AAA) |
| Focus Indicators | 3px visible outline |
| Screen Reader | ARIA labels, live regions |
| Keyboard Nav | All elements reachable |
| Reduced Motion | `prefers-reduced-motion` support |

---

## Performance Budget

| Metric | Target | Actual |
|--------|--------|--------|
| First Load (gzip) | < 14KB | ~3KB |
| Critical CSS | < 10KB | ~10KB |
| JavaScript | 0KB required | 0KB |
| Total Site | < 500KB | ~230KB |

---

## Verification

```bash
# Build and check size
cd src && hugo --minify
gzip -c public/index.html | wc -c

# Check CSS is inlined
grep -o '<style>.*</style>' public/index.html | wc -c

# Run dev server
hugo server
```

### Manual Testing

- Screen reader (VoiceOver)
- Keyboard-only navigation
- Slow 3G throttling
- Offline mode
