# MOOV Theme — Detailed Reference

> Extended context for Claude. Read this when in doubt about architecture, patterns, or pending work.
> For quick orientation, see `CLAUDE.md`. For full implementation specs, see `HANDOFF.md`.

---

## Theme Architecture

### Directory Structure

```
assets/     Static files (CSS, JS, images). Keep only critical.css and page-wide statics here.
blocks/     Reusable nestable components. Must have {% doc %} and {% schema %} tags.
config/     settings_schema.json (global token schema) + settings_data.json (saved values)
layout/     Top-level HTML wrappers (theme.liquid, password.liquid). Must include
            {{ content_for_header }} and {{ content_for_layout }}.
locales/    Translation JSON files. All copy goes here — use {{ 'key' | t }} in Liquid.
sections/   Full-width page components with {% schema %} for theme editor customisation.
snippets/   Reusable fragments rendered via {% render 'name' %}. Must have {% doc %} tag.
templates/  JSON files that define section/block order per page type.
```

### CSS & JavaScript

- Write per-component styles inside `{% stylesheet %}` and JS inside `{% javascript %}` tags
- These tags are only supported in `snippets/`, `blocks/`, and `sections/`
- `assets/critical.css` is for above-the-fold styles loaded on every page

### LiquidDoc (required on snippets + statically rendered blocks)

```liquid
{% doc %}
  Brief description of what this renders.

  @param {image} image - The image object
  @param {string} [url] - Optional link destination

  @example
  {% render 'image', image: product.featured_image %}
{% enddoc %}
```

### Schema Tag Rules

- Every section and block needs a `{% schema %}` tag exposing settings
- Validate section schemas against `schemas/section.json`
- Validate block schemas against `schemas/theme_block.json`
- Validate locale files against `schemas/translations.json`
- Validate `config/settings_schema.json` against `schemas/theme_settings.json`

---

## Product Page Architecture

Template: `templates/product.moov.json`
Section order: `["shop_main", "reviews_grid", "faq_section"]`

### Section order inside `product-moov.liquid`

| # | Class | Surface | Status |
|---|---|---|---|
| 00 | `.shop-crumbs` | dark | Built |
| 01 | `.shop-hero` | dark | Built |
| 02 | `.shop-buy` | dark | Built |
| 03 | `.shop-specs` | cream | Built |
| 04 | `.shop-box` | cream | Built — *remove per client brief* |
| 05 | `.shop-features` | dark | Built |
| 06 | `.shop-pairs` | cream | Built — *remove per client brief* |
| 07 | `.shop-cta` | dark | Built |

---

## Buybox Interactions (section 02)

### Option UI Logic

| Option name contains | Renders as |
|---|---|
| `finish`, `color`, `colour` | Colour swatch circles |
| `mount`, `style` | Pill buttons |
| anything else | `<select>` dropdown |

### Variant Resolution

Variants are embedded as JSON inside the form — no fetch calls:
```html
<script type="application/json" id="shop-variants-{{ section.id }}">
  {{ product.variants | json }}
</script>
```
On swatch/pill/select change → JS finds matching variant → updates `input[name="id"]` → updates price + button label.

### Live Price in Button

- Format: `Add to cart — $129`
- Qty multiplies: `Add to cart — $258` (×2)
- `data-cta-label` on the button holds the base label text

### Image Counter

Overlay on stage image: `01 / 04`. Updates on thumbnail click.

---

## Pending Work: Client Content Integration

### 1. Locale strings — `locales/en.default.json`

Update existing keys under `sections.shop`:

| Key | Value |
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

Add new keys under `sections.shop`:

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
"founder_body": "The first MOOV Founder Drop will be limited to 500 Ivory White devices...",
"founder_limit_badge": "Limited to 500 units"
```

### 2. Remove sections 04 + 06

Remove `.shop-box` and `.shop-pairs` HTML blocks from `product-moov.liquid`.

### 3. Insert How It Works + Founder Drop

See `HANDOFF.md` for the full Liquid markup, CSS, and schema settings to add.

### 4. Spec defaults to update (in schema)

| Setting ID | Default |
|---|---|
| `spec_1_dt` / `spec_1_dd` | `Battery` / `Up to 4 months` |
| `spec_2_dt` / `spec_2_dd` | `Mounting` / `Magnetic wall mount + 3M Command Strip` |
| `spec_3_dt` / `spec_3_dd` | `Charging` / `USB-C` |
| `spec_4_dt` / `spec_4_dd` | `Compatibility` / `iOS & Android` |
| `spec_5_dt` through `spec_9_dt` | `""` (blank — hides row) |

---

## Shopify Liquid Patterns

### Localised string
```liquid
{{ 'sections.shop.my_key' | t }}
```

### Section setting with locale fallback
```liquid
{%- assign val = section.settings.my_setting | default: 'sections.shop.my_key' | t -%}
```

### Conditional block
```liquid
{%- if section.settings.my_setting != blank -%}
  <p>{{ section.settings.my_setting | escape }}</p>
{%- endif -%}
```

### Image with fallback
```liquid
{%- if img != blank -%}
  {{ img | image_url: width: 900 | image_tag: loading: 'lazy', class: 'my-img', alt: 'Alt text' }}
{%- else -%}
  <div class="my-placeholder"></div>
{%- endif -%}
```

### Loop over numbered locale keys
```liquid
{%- for i in (1..3) -%}
  {%- assign _kt = 'hiw_step_' | append: i | append: '_title' -%}
  {%- assign step_title = 'sections.shop.' | append: _kt | t -%}
{%- endfor -%}
```

---

## Theme Editor Navigation

1. Online Store → Themes → Customize
2. Navigate to a product using the `product.moov` template
3. Click **Shop** section in the left panel

### Commonly Updated Settings

- Eyebrow / Heading / Description — hero text
- Tagline — italic line below heading in buybox
- Perk 1–3 — trust icons below buy button
- Reservation note — small italic text below CTA
- Specs 1–9 — label + value pairs
- Founder Drop image — upload via Shopify Files

---

## Shopify Admin One-time Setup

- **Assign template:** Product → MOOV Device → Template → `product.moov`
- **Upload Founder Drop image:** Content → Files → upload → set in theme editor under Shop section
- **Product variants:** Create `Finish` option (Cream, Charcoal) and `Mount` option (Brass, Charcoal, None) — buybox UI renders automatically based on option names
