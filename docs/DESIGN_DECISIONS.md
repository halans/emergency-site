# Design Decisions

Explains the reasoning behind key architectural choices for the Emergency Site Kit.

## Problem Statement

During emergencies, traditional websites fail:

1. **Traffic spikes** overwhelm dynamic servers
2. **Poor connectivity** makes heavy pages unusable
3. **Damaged infrastructure** limits bandwidth
4. **Stressed users** struggle with complex interfaces
5. **Accessibility needs** are heightened

## Core Philosophy: Rule of Least Power

Use the least powerful technology suitable for the purpose:

- **HTML over JavaScript** — HTML works without JS execution
- **CSS over JS animations** — More resilient
- **Static over dynamic** — Survives traffic spikes
- **Simple over complex** — Less to break

---

## Decision: < 14KB First Load

### Why 14KB?

The initial TCP congestion window is ~14KB. Content within this limit renders in a single network roundtrip.

| Connection | RTT | Time to Render |
|------------|-----|----------------|
| Good 4G | 50ms | 50ms |
| Slow 3G | 400ms | 400ms |
| Poor 2G | 2000ms | 2000ms |

Exceeding 14KB doubles these times minimum.

### How We Achieve It

- Inline CSS (no extra requests)
- System fonts (no downloads)
- No images in critical path
- Minimal HTML
- Zero JavaScript

---

## Decision: Zero JavaScript Requirement

JavaScript fails more often than expected:

- Blocked by firewalls
- Disabled for accessibility
- Parse errors break bundles
- Mobile power saving

**Emergency information must work without JavaScript.**

We use JS only for progressive enhancement (service worker).

---

## Decision: WCAG AAA Color Contrast

### Why AAA (7:1), Not AA (4.5:1)?

- **Outdoor reading** — Sunlight washes out screens
- **Stressed cognition** — High contrast reduces load
- **Dirty screens** — Dust/rain affects visibility
- **Battery saver** — Dim screens need more contrast

### Color Palette

| Color | Hex | Use |
|-------|-----|-----|
| Emergency | #B91C1C | Danger, critical |
| Warning | #D97706 | Caution, attention |
| Info | #1E40AF | General, resolved |
| Safe | #047857 | Positive status |
| Text | #111827 | Main content |
| Focus | #2563EB | Keyboard focus |

---

## Decision: Service Worker Strategy

### Tiered Caching

1. **Network-First (HTML)** — Always tries fresh content first
2. **Cache-First (Assets)** — Icons, CSS cached
3. **Offline Fallback** — Dedicated page when offline

---

## Decision: Hugo as Static Generator

### Why Hugo?

- **Speed** — Builds in milliseconds
- **No dependencies** — Single binary
- **Safe templates** — Prevents injection
- **Asset pipeline** — Built-in processing

### Alternatives Considered

- 11ty — Requires Node.js
- Jekyll — Slower, requires Ruby
- Gatsby/Next — Too heavy
- Plain HTML — Hard to maintain

---

## Decision: Cloudflare Pages Deployment

### Why Cloudflare?

- Global CDN
- Free tier
- DDoS protection
- Zero cold start
- Git push to deploy

---

## Decision: Severity Levels

Updates use three severity levels:

| Level | Use Case | Visual |
|-------|----------|--------|
| `info` | Resolved, general | Blue pill, grey border |
| `warning` | Developing situation | Amber pill, amber border |
| `critical` | Immediate danger | Red pill, red border |

---

## Decision: Timezone Configuration

Timestamps include a configurable timezone label (default: AEDT) because:

- Emergency sites often serve local audiences
- Users need to know currency of information
- Build timestamp shows when site was last published

---

## Future Considerations

- **Multi-language** — Hugo i18n support ready
- **SMS version** — Plain text API endpoint
- **Print optimization** — Already has print stylesheet

---

## References

- [Rule of Least Power - W3C](https://www.w3.org/2001/tag/doc/leastPower.html)
- [WCAG 2.1 Guidelines](https://www.w3.org/TR/WCAG21/)
- [Everyone has JavaScript, right?](https://www.kryogenix.org/code/browser/everyonehasjs.html)
