# MOOV Theme — Claude Context

## Project
Custom Shopify 2.0 theme for **MOOV** — a premium wall-mounted smart alarm device.

- **Brand:** Dark (black) + Cream two-surface system, amber accent
- **All copy lives in:** `locales/en.default.json` — update strings there, not in `.liquid` directly
- **Alternate product template:** `templates/product.moov.json` → assign to MOOV product in Shopify admin

## Key Files

```
sections/product-moov.liquid     ← main PDP (self-contained: HTML + CSS + JS + schema)
sections/header.liquid           ← site header
sections/footer.liquid           ← site footer
sections/faq.liquid              ← FAQ accordion (used in product.moov.json)
sections/home-reviews-grid.liquid← reviews grid (used in product.moov.json)
templates/product.moov.json      ← MOOV product alternate template
locales/en.default.json          ← all user-facing copy
locales/en.default.schema.json   ← theme editor label translations
config/settings_schema.json      ← global design tokens schema
config/settings_data.json        ← saved global setting values
snippets/css-variables.liquid    ← CSS custom properties output
```

## Design Tokens (CSS Variables)

| Variable | Purpose |
|---|---|
| `--moov-black` | Primary dark surface |
| `--moov-cream` | Primary light surface |
| `--moov-white` | Pure white (`#ffffff`) — use for text on dark surfaces where max brightness is needed (e.g. header nav). Do NOT use `--moov-cream` or hard-code `#fff` in headers — always use this token. |
| `--moov-amber` | Accent / eyebrow / highlights |
| `--moov-border-dark` | Borders on dark surfaces |
| `--moov-border-light` | Borders on cream surfaces |
| `--moov-text-secondary-dark` | Body text on dark |
| `--moov-text-secondary-light` | Body text on cream |
| `--moov-text-tertiary-dark` | Dim/label text on dark |
| `--moov-eyebrow-accent` | Eyebrow colour on cream surfaces |
| `--font-hero` | Display / heading font |
| `--font-body` | Body font |
| `--section-block-pad-y-lg` | Section vertical padding (large) |
| `--section-inline-pad` | Section horizontal padding |
| `--layout-max-width` | Max content width |
| `--header-height` | Used for sticky offset |

## Conventions

- Sections are self-contained: HTML + `{% stylesheet %}` + `{% javascript %}` + `{% schema %}` all in one file
- Use `data-surface="dark"` or `data-surface="cream"` on `<section>` elements
- Buybox option UI is determined by option name: `finish/color/colour` → swatches, `mount/style` → pills, else → `<select>`
- Swatch colours mapped via `data-swatch` CSS attribute (handles: cream, off-white, charcoal, black, white, slate, sand, brass)
- Snippets and blocks must have `{% doc %}` header tags
- Validate schema JSON using `schemas/section.json`

## Current Status

| Area | Status |
|---|---|
| Product page (`product-moov.liquid`) | Complete |
| Buybox (swatches, pills, live price, qty counter) | Complete |
| Client content integration (6-section brief) | In progress |
| How It Works section | Not started |
| Founder Drop section | Not started |

> See `CLAUDE-REFERENCE.md` for pending implementation details, design token deep-dive, and Shopify Liquid patterns.
> See `HANDOFF.md` for full implementation specs (locale keys, code snippets, schema settings).
