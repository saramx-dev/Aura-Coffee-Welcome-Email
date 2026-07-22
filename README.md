# Aura Coffee — Welcome Email

A coded HTML welcome email for Aura Coffee's waitlist, built with MJML and automated through a live Klaviyo flow. Part of the broader [Aura Coffee waitlist landing page](your-main-repo-link-here) project.

---

## Overview

When someone joins the Aura Coffee waitlist, this email sends automatically — confirming their signup and setting expectations for what's coming next (sourcing notes, roast profile, and tasting breakdown, arriving one week before the seasonal release).

**Trigger:** Added to "Aura Coffee Waitlist" list in Klaviyo
**Flow:** Aura Coffee Welcome Series → Email #1 (Day 0)

---

## Features

- ✉️ Built with **MJML** — compiles to email-client-safe, table-based HTML
- 🎨 Matches the main landing page's design tokens — same typography, colors, and spacing scale
- 🌓 **Three-layer dark mode support:**
  - `color-scheme` / `supported-color-schemes` meta tags for client-level signaling
  - `@media (prefers-color-scheme: dark)` overrides for background, text, headline, and caption colors
  - Logo swap (light/dark variant) synced to the active color scheme
  - Outlook-specific MSO conditional comment forcing light mode, since Outlook's rendering engine doesn't support `@media` queries
- 👤 **Personalization** — greets subscribers by first name with a graceful fallback: `{{ first_name|default:'there' }}`
- 🔗 **Real, working unsubscribe link** via Klaviyo's `{% unsubscribe_link %}` tag
- ♿ Accessible structure — semantic content hierarchy, sufficient color contrast (verified WCAG AA) across both light and dark variants
- 📋 Compliant footer — sender identification, transparency messaging, unsubscribe link

---

## Tech Stack

| Layer | Technology |
|---|---|
| Templating | MJML |
| Automation | Klaviyo Flows |
| Personalization / Logic | Klaviyo template tags (`{{ }}`, `{% %}`) |
| Editor | VS Code + MJML extension |

---

## Project Structure

```
├── index.mjml       # Source template
├── index.html       # Compiled output (pasted into Klaviyo's HTML editor)
└── README.md
```

---

## How Dark Mode Works Here

Email clients don't support dark mode consistently — some respect `prefers-color-scheme`, some auto-invert colors on their own, and Outlook ignores the concept entirely. This template handles all three cases deliberately:

1. **Well-behaved clients** (Apple Mail, some Gmail contexts) — read the `@media (prefers-color-scheme: dark)` block and swap background, text, and accent colors to pre-selected, contrast-checked alternatives.
2. **Logo swap** — two `<mj-image>` elements are stacked (`.logo-dark` / `.logo-light`), toggled via `display: none/block` inside the same media query, so the correct logo variant shows depending on mode.
3. **Outlook desktop** — since it can't read `@media` queries at all, an MSO conditional comment (`<!--[if mso]>`) forces the light-mode palette explicitly, sidestepping Outlook's unpredictable auto-dark behavior entirely.

All dark-mode colors were checked against WCAG AA contrast requirements before being locked in.

---

## Local Development

1. Edit `index.mjml`
2. Compile using the MJML VS Code extension ("MJML: Export to HTML"), or the MJML CLI:
   ```bash
   npx mjml index.mjml -o index.html
   ```
3. Preview `index.html` directly in a browser for a quick check
4. Paste the compiled HTML into Klaviyo's HTML email editor to test with real merge tags and send a live preview

**Note:** Klaviyo template tags (`{{ first_name }}`, `{% unsubscribe_link %}`) only resolve when sent through Klaviyo — local browser previews will show the raw tag text, which is expected.

---

## Author

**Sara**

- Twitter: [@saramx_dev](https://x.com/saramx_dev)  
- LinkedIn: [Sara Mohamed](https://www.linkedin.com/in/saramx-dev/)

---

## License

This project is available for personal and portfolio use. Contact the author for commercial licensing inquiries.
