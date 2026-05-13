# MOOV Theme v1 — Developer Handoff

**Project:** Moovtech Shopify Theme
**Theme name:** moovtech-theme-v1
**Branch:** main
**Last updated:** 2026-05-12

---

## Overview

Custom Shopify 2.0 theme built for MOOV — a premium wall-mounted smart alarm device. The theme uses a dark/cream two-surface design system with amber accent. All copy is driven through `locales/en.default.json` for easy content updates without touching code.

---

## Current Status

| Area | Status |
|---|---|
| Product page section (`product-moov.liquid`) | ✅ Complete — committed |
| Buybox interactions (swatches, pills, live price, counter) | ✅ Complete — committed |
| Selected colour/option label display | ✅ Complete — committed |
| Client content integration (6-section brief) | ⚠️ In progress — interrupted |
| How It Works section (new) | ❌ Not started |
| Founder Drop section (new) | ❌ Not started |
| Locale strings for client brief | ⚠️ Partial (4 of ~14 strings updated, uncommitted) |

---

## Product Page Architecture

The MOOV product page uses a custom alternate template:

```
templates/product.moov.json        ← assign this template to the MOOV product in Shopify admin
sections/product-moov.liquid       ← the entire PDP lives here (self-contained)
locales/en.default.json            ← all user-facing copy
```

### Template section order (`product.moov.json`)

```json
order: ["shop_main", "reviews_grid", "faq_section"]
```

- **shop_main** — `product-moov` section (the full PDP, see below)
- **reviews_grid** — `home-reviews-grid` section with 3 hardcoded reviews
- **faq_section** — `faq` section with 5 hardcoded FAQs

### Section order inside `product-moov.liquid`

| # | Class | Surface | Status |
|---|---|---|---|
| 00 | `.shop-crumbs` | dark | ✅ Built |
| 01 | `.shop-hero` | dark | ✅ Built |
| 02 | `.shop-buy` | dark | ✅ Built |
| 03 | `.shop-specs` | cream | ✅ Built |
| 04 | `.shop-box` | cream | ✅ Built — *remove per client brief* |
| 05 | `.shop-features` | dark | ✅ Built |
| 06 | `.shop-pairs` | cream | ✅ Built — *remove per client brief* |
| 07 | `.shop-cta` | dark | ✅ Built |

---

## Buybox Interactions (section 02)

All JavaScript is inside the `{% javascript %}` tag in `product-moov.liquid`.

### Option UI logic
Options are driven by `product.options_with_values`. The option name determines the UI:

| Option name contains | Renders as |
|---|---|
| `finish`, `color`, `colour` | Colour swatch circles |
| `mount`, `style` | Pill buttons |
| anything else | `<select>` dropdown |

Swatch colours are mapped via CSS `data-swatch` attributes. Known handles:
`cream`, `off-white`, `charcoal`, `black`, `white`, `slate`, `sand`, `brass`

To add a new colour, add a CSS rule in the stylesheet block:
```css
.shop-buy__swatch[data-swatch="your-handle"] .shop-buy__swatch-inner { background: #hexcode; }
```

### Variant resolution
Variants are embedded as JSON inside the form (no fetch calls):
```html
<script type="application/json" id="shop-variants-{{ section.id }}">
  {{ product.variants | json }}
</script>
```
On swatch/pill/select change → JS finds the matching variant → updates hidden `input[name="id"]` → updates price display + button label.

### Live price in button
Submit button label format: `Add to cart — $129`
Qty stepper multiplies: `Add to cart — $258` (×2)
`data-cta-label` on the button holds the base label text.

### Image counter
Overlay on the stage image: `01 / 04`. Updates on thumbnail click.

---

## Pending Work: Client Content Integration

The client sent a 6-section product page brief. The mapping and remaining tasks are:

### What still needs to be done

#### 1. Locale strings — update in `locales/en.default.json`

These keys exist but still need their values updated under `sections.shop`:

| Key | Target value |
|---|---|
| `features_eyebrow` | `"Why MOOV"` |
| `features_heading` | `"Built for better mornings."` |
| `feature_1_title` | `"Gets you out of bed"` |
| `feature_1_body` | `"Designed around physical movement instead of another tap from bed."` |
| `feature_2_title` | `"Reduces morning distractions"` |
| `feature_2_body` | `"Optional Focus Mode helps block distracting apps during your morning routine."` |
| `feature_3_title` | `"Builds morning momentum"` |
| `feature_3_body` | `"Start the day with more intention, movement, and control."` |
| `cta_heading` | `"Better mornings start with movement."` |
| `cta_sub` | `"Reserve your place in the first 500 for $1."` |

These new keys need to be added:

```json
"reservation_note": "Your $1 reservation secures priority access to the first MOOV Founder Drop.",
"hiw_eyebrow": "How it works",
"hiw_step_1_title": "Set your alarm",
"hiw_step_1_body": "Set your wake-up time directly in the MOOV app.",
"hiw_step_2_title": "Get moving",
"hiw_step_2_body": "Move to your MOOV device to begin your morning.",
"hiw_step_3_title": "Stay focused",
"hiw_step_3_body": "Optional Focus Mode helps reduce distracting app usage after waking up.",
"hiw_link": "Learn more about how it works",
"founder_eyebrow": "The Founder Drop",
"founder_heading": "The original MOOV release.",
"founder_body": "The first MOOV Founder Drop will be limited to 500 Ivory White devices. Created for early supporters who believe mornings deserve more intention.",
"founder_limit_badge": "Limited to 500 units"
```

Note: `eyebrow_default`, `heading_default`, `description_default`, and `cta_primary_default` were already updated (uncommitted — run `git diff locales/en.default.json` to see).

---

#### 2. Buybox — add reservation note

After the `.shop-buy__cta-group` div (before the perks list), add:

```liquid
{%- assign reservation_note = section.settings.reservation_note | strip -%}
{%- if reservation_note != blank -%}
  <p class="shop-buy__reservation">{{ reservation_note | escape }}</p>
{%- endif -%}
```

CSS to add inside `{% stylesheet %}`:
```css
.shop-buy__reservation {
  margin: 0;
  font-family: var(--font-body);
  font-size: var(--text-xs);
  font-style: italic;
  color: var(--moov-text-tertiary-dark);
  text-align: center;
}
```

Schema setting to add (after the `product_image` image_picker):
```json
{
  "type": "header",
  "content": "Reservation note"
},
{
  "type": "text",
  "id": "reservation_note",
  "label": "Reservation note",
  "default": "Your $1 reservation secures priority access to the first MOOV Founder Drop."
}
```

---

#### 3. Replace In the Box + Pairs With with How It Works + Founder Drop

**Remove** the HTML blocks for sections 04 (`.shop-box`) and 06 (`.shop-pairs`).

**Insert** two new sections between the buybox close tag and the specs section:

**How It Works** (dark surface):
```liquid
{%- comment -%}── 03 HOW IT WORKS ──{%- endcomment -%}
<section class="shop-hiw full-width" data-surface="dark">
  <div class="shop-hiw__inner">
    <p class="shop-hiw__eyebrow">{{ 'sections.shop.hiw_eyebrow' | t }}</p>
    <div class="shop-hiw__steps">
      {%- for i in (1..3) -%}
        {%- assign _kt = 'hiw_step_' | append: i | append: '_title' -%}
        {%- assign _kb = 'hiw_step_' | append: i | append: '_body' -%}
        {%- assign step_title = section.settings[_kt] | default: 'sections.shop.' | append: _kt | t -%}
        {%- assign step_body  = section.settings[_kb] | default: 'sections.shop.' | append: _kb | t -%}
        <div class="shop-hiw__step">
          <span class="shop-hiw__num">0{{ i }}</span>
          <h4 class="shop-hiw__title">{{ step_title | escape }}</h4>
          <p class="shop-hiw__body">{{ step_body | escape }}</p>
        </div>
      {%- endfor -%}
    </div>
    {%- if section.settings.hiw_url != blank -%}
      <a href="{{ section.settings.hiw_url }}" class="shop-hiw__link">
        {{ 'sections.shop.hiw_link' | t }}
        <svg width="12" height="12" viewBox="0 0 12 12" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <line x1="1" y1="6" x2="10" y2="6"/><polyline points="6.5,2.5 10,6 6.5,9.5"/>
        </svg>
      </a>
    {%- endif -%}
  </div>
</section>
```

**Founder Drop** (cream surface):
```liquid
{%- comment -%}── 04 FOUNDER DROP ──{%- endcomment -%}
{%- assign founder_img = section.settings.founder_image -%}
<section class="shop-founder full-width" data-surface="cream">
  <div class="shop-founder__inner">
    <div class="shop-founder__copy">
      <p class="shop-founder__limit">{{ 'sections.shop.founder_limit_badge' | t }}</p>
      <p class="shop-founder__eyebrow">{{ 'sections.shop.founder_eyebrow' | t }}</p>
      <h2 class="shop-founder__heading">{{ 'sections.shop.founder_heading' | t }}</h2>
      <p class="shop-founder__body">{{ 'sections.shop.founder_body' | t }}</p>
    </div>
    <div class="shop-founder__media">
      {%- if founder_img != blank -%}
        {{ founder_img | image_url: width: 900 | image_tag: loading: 'lazy', class: 'shop-founder__img', alt: 'MOOV Founder Drop' }}
      {%- else -%}
        <div class="shop-founder__placeholder"></div>
      {%- endif -%}
    </div>
  </div>
</section>
```

**CSS to add** inside `{% stylesheet %}`:
```css
/* HIW */
.shop-hiw {
  background: var(--moov-black);
  color: var(--moov-cream);
  padding: var(--section-block-pad-y-lg) var(--section-inline-pad);
  border-top: 1px solid var(--moov-border-dark);
}
.shop-hiw__inner { display: flex; flex-direction: column; gap: clamp(36px, 5vw, 56px); }
.shop-hiw__eyebrow {
  margin: 0;
  font-family: var(--font-body);
  font-size: var(--text-2xs);
  font-weight: 500;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--moov-text-tertiary-dark);
}
.shop-hiw__steps {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0;
  border-top: 1px solid var(--moov-border-dark);
}
.shop-hiw__step {
  display: flex;
  flex-direction: column;
  gap: 14px;
  padding: 32px 28px 40px;
  border-right: 1px solid var(--moov-border-dark);
}
.shop-hiw__step:last-child { border-right: none; }
.shop-hiw__num {
  font-family: var(--font-body);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--moov-amber);
}
.shop-hiw__title {
  margin: 0;
  font-family: var(--font-hero);
  font-weight: 500;
  font-size: var(--text-heading-md);
  line-height: 1.05;
  letter-spacing: -0.01em;
  text-transform: uppercase;
  color: var(--moov-cream);
}
.shop-hiw__body {
  margin: 0;
  font-size: var(--text-sm);
  line-height: 1.65;
  color: var(--moov-text-secondary-dark);
}
.shop-hiw__link {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-family: var(--font-body);
  font-size: var(--text-sm);
  font-weight: 500;
  color: var(--moov-text-secondary-dark);
  text-decoration: none;
  transition: color 0.15s ease;
}
.shop-hiw__link:hover { color: var(--moov-cream); }
@media (max-width: 860px) {
  .shop-hiw__steps { grid-template-columns: 1fr; }
  .shop-hiw__step { border-right: none; }
  .shop-hiw__step:not(:last-child) { border-bottom: 1px solid var(--moov-border-dark); }
}

/* Founder Drop */
.shop-founder {
  background: var(--moov-cream);
  color: var(--moov-black);
  padding: var(--section-block-pad-y-lg) var(--section-inline-pad);
  border-top: 1px solid var(--moov-border-light);
}
.shop-founder__inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: clamp(40px, 6vw, 80px);
  align-items: center;
}
.shop-founder__copy { display: flex; flex-direction: column; gap: 18px; }
.shop-founder__limit {
  display: inline-flex;
  align-self: flex-start;
  padding: 5px 14px;
  border: 1px solid var(--moov-amber);
  border-radius: 999px;
  font-family: var(--font-body);
  font-size: var(--text-2xs);
  font-weight: 500;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--moov-amber);
  margin: 0;
}
.shop-founder__eyebrow {
  margin: 0;
  font-family: var(--font-body);
  font-size: var(--text-2xs);
  font-weight: 500;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--moov-eyebrow-accent);
}
.shop-founder__heading {
  margin: 0;
  font-family: var(--font-hero);
  font-weight: 500;
  font-size: var(--text-heading-xl);
  line-height: 1.05;
  letter-spacing: -0.02em;
  text-transform: uppercase;
  color: var(--moov-black);
  max-width: 18ch;
  text-wrap: balance;
}
.shop-founder__body {
  margin: 0;
  font-size: var(--text-md);
  line-height: 1.7;
  color: var(--moov-text-secondary-light);
  max-width: 42ch;
}
.shop-founder__media {
  aspect-ratio: 3 / 4;
  border-radius: var(--radius-card, 16px);
  overflow: hidden;
  background: color-mix(in srgb, var(--moov-black) 8%, var(--moov-cream));
}
.shop-founder__img { width: 100%; height: 100%; object-fit: cover; display: block; }
.shop-founder__placeholder { width: 100%; height: 100%; }
@media (max-width: 860px) {
  .shop-founder__inner { grid-template-columns: 1fr; }
  .shop-founder__media { aspect-ratio: 4 / 3; order: -1; }
}
```

Also add `.shop-hiw__inner` and `.shop-founder__inner` to the shared max-width selector at the top of the stylesheet:
```css
.shop-crumbs__inner,
.shop-hero__inner,
.shop-buy__inner,
.shop-hiw__inner,       /* ← add */
.shop-founder__inner,   /* ← add */
.shop-specs__inner,
...
```

**Schema settings to add** (after the `product_image` image_picker, before the `header_specs` block):

```json
{ "type": "header", "content": "Reservation note" },
{ "type": "text", "id": "reservation_note", "label": "Reservation note text",
  "default": "Your $1 reservation secures priority access to the first MOOV Founder Drop." },
{ "type": "header", "content": "How it works" },
{ "type": "text", "id": "hiw_step_1_title", "label": "Step 1 title", "default": "Set your alarm" },
{ "type": "text", "id": "hiw_step_1_body",  "label": "Step 1 body",  "default": "Set your wake-up time directly in the MOOV app." },
{ "type": "text", "id": "hiw_step_2_title", "label": "Step 2 title", "default": "Get moving" },
{ "type": "text", "id": "hiw_step_2_body",  "label": "Step 2 body",  "default": "Move to your MOOV device to begin your morning." },
{ "type": "text", "id": "hiw_step_3_title", "label": "Step 3 title", "default": "Stay focused" },
{ "type": "text", "id": "hiw_step_3_body",  "label": "Step 3 body",  "default": "Optional Focus Mode helps reduce distracting app usage after waking up." },
{ "type": "url",  "id": "hiw_url",          "label": "Learn more link URL" },
{ "type": "header", "content": "Founder Drop" },
{ "type": "image_picker", "id": "founder_image", "label": "Founder Drop image" }
```

**Update spec schema defaults** (specs 1–4 updated, 5–9 cleared to blank so rows hide):

| Setting ID | Default value |
|---|---|
| `spec_1_dt` | `Battery` |
| `spec_1_dd` | `Up to 4 months` |
| `spec_2_dt` | `Mounting` |
| `spec_2_dd` | `Magnetic wall mount + 3M Command Strip` |
| `spec_3_dt` | `Charging` |
| `spec_3_dd` | `USB-C` |
| `spec_4_dt` | `Compatibility` |
| `spec_4_dd` | `iOS & Android` |
| `spec_5_dt` through `spec_9_dt` | `""` (blank) |
| `spec_5_dd` through `spec_9_dd` | `""` (blank) |

---

## Theme Editor — Key Settings

To configure the product page in the Shopify theme editor:

1. Go to **Online Store → Themes → Customize**
2. Navigate to a product page using the `product.moov` template
3. Click **Shop** section in the left panel

### Commonly updated settings
- **Eyebrow / Heading / Description** — hero text
- **Tagline** — italic line below the heading in the buybox
- **Price / Compare-at price** — display override (falls back to live variant price)
- **Perk 1–3** — trust icons below the buy button (truck / clock / shield)
- **Reservation note** — small italic text below CTA *(once added)*
- **Specs 1–9** — label + value pairs for the specs table
- **Founder Drop image** — upload via Shopify Files *(once added)*

---

## Uncommitted Changes

Run `git diff` to see current working tree changes. As of handoff:

```
locales/en.default.json  — 4 locale strings partially updated for client brief
                           (eyebrow_default, heading_default, description_default, cta_primary_default)
```

The remaining client content integration (sections 3–6 above) was in progress when work paused.

---

## File Map

```
sections/
  product-moov.liquid      ← main PDP section (self-contained: HTML + CSS + JS + schema)
  faq.liquid               ← FAQ accordion used in product.moov.json
  home-reviews-grid.liquid ← reviews grid used in product.moov.json
  header.liquid            ← site header
  footer.liquid            ← site footer

templates/
  product.moov.json        ← alternate template: assign to MOOV product in admin
  product.json             ← default product template
  index.json               ← homepage

locales/
  en.default.json          ← all copy — update this file, not the liquid directly
  en.default.schema.json   ← theme editor label translations

config/
  settings_schema.json     ← global design tokens (colours, fonts, radius, spacing)
  settings_data.json       ← saved global setting values
```

---

## Design Tokens (global CSS variables)

Set in **Online Store → Themes → Customize → Theme settings**.

| Variable | Purpose |
|---|---|
| `--moov-black` | Primary dark surface |
| `--moov-cream` | Primary light surface |
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
| `--section-block-pad-y-md` | Section vertical padding (medium) |
| `--section-inline-pad` | Section horizontal padding |
| `--layout-max-width` | Max content width |
| `--header-height` | Header height (used for sticky offset) |

---

## Shopify Admin — One-time Setup

- **Assign the alternate template:** Product → MOOV Device → Template → select `product.moov`
- **Upload Founder Drop image:** Content → Files → upload, then set in theme editor under the Shop section
- **Product variants:** Create `Finish` option (Cream, Charcoal) and `Mount` option (Brass, Charcoal, None) in Shopify admin — the buybox UI renders automatically based on option names
