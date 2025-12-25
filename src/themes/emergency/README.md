# Emergency Hugo Theme

A lightweight, performant, and accessible Hugo theme for emergency information websites.

## Features

- ⚡ **< 14KB first load** - Inlined CSS, no external requests
- 🔌 **Offline support** - Service worker for network failures
- ♿ **WCAG AAA accessible** - High contrast, keyboard navigation
- 📱 **Mobile-first** - Works on any device
- 🚀 **Zero JavaScript required** - Progressive enhancement only

## Installation

### As a Git submodule (recommended)

```bash
cd your-hugo-site
git submodule add https://github.com/YOUR_USERNAME/hugo-emergency-site themes/emergency
```

Then add to your `hugo.toml`:

```toml
theme = "emergency"
```

### Copy directly

Copy the `themes/emergency` folder into your Hugo site's `themes/` directory.

## Configuration

Add these parameters to your `hugo.toml`:

```toml
[params]
  description = "Emergency information and updates"
  themeColor = "#B91C1C"
  email = "contact@example.org"
  telephone = "+1 234 567 8900"
  
  # Alert banner (optional)
  alertTitle = "Current Emergency"
  alertMessage = "Important information here"
  alertLevel = "warning"  # info, warning, critical
```

## Content Types

The theme supports three content types:

- `updates/` - Emergency announcements with timestamps and severity
- `faq/` - Frequently asked questions
- `prepkit/` - Emergency preparation checklists

## Customization

Override any template by creating the same file path in your site's `layouts/` directory.

Override CSS by creating `assets/css/critical.css` in your site root.

## License

MIT
