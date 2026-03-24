# Shopify store marketing copy (original)

> **This Shopify store is the canonical destination.** It replaces both [moovtech.ca](https://www.moovtech.ca) (brand/mission) and [moovmornings.com](https://moovmornings.com) (product/pre-order). All links to "About", "How it works", and "Contact" should resolve to pages within this store (e.g. `/pages/about`, `/pages/how-it-works`, `/pages/contact`). Do not link outbound to the legacy sites.

Original, store-ready messaging synthesised from the MOOV mission and product story. **Not** verbatim from either legacy site—written for conversion, clarity, and Shopify surfaces.

Use **`{{ 'store_marketing.KEY' | t }}`** for strings that exist in `locales/en.default.json`. Longer or experimental variants live only in this file.

---

## Voice & rules

- **Tone:** Calm confidence, human, slightly premium. Hopeful without hype.
- **Avoid:** Medical claims, guaranteed outcomes, fake urgency, guilt.
- **Lead with:** Outcome (clarity, momentum, agency) → mechanism (alarm + app + routines) → proof/risk reversal (guarantee, shipping, impact).

---

## SEO & sharing

| Use | Copy |
|-----|------|
| **Home title suffix** | MOOV — mornings you lead, not your phone |
| **Meta description** | MOOV is a morning system: a movement-based alarm, app blocking for early hours, and AI routines that adapt to your life. Pre-order the smart wake experience. |
| **Social / OG teaser** | The first hour belongs to you again. |

*Keys:* `store_marketing.seo_title_suffix`, `store_marketing.seo_description`, `store_marketing.social_teaser`

---

## Homepage hero (primary)

| Role | Copy |
|------|------|
| **Social proof (under eyebrow)** | 2,400+ people have reserved their spot |
| **Eyebrow** | Morning wellness, engineered |
| **Headline** | Lead the first hour of your day |
| **Subhead** | MOOV combines a wall-mounted alarm you physically disarm, a companion app that shields distracting apps at wake, and short routines that adapt to your goals—so you start clear, not reactive. |
| **Primary CTA** | Reserve your MOOV |
| **Secondary CTA** | See how it works |
| **Micro-trust (under CTAs)** | 30-day satisfaction promise · US & Canada shipping |

> **Why these changes:** "Reserve" triggers the Endowment Effect — securing something desirable rather than making a purchase gamble. The social proof number (update when real) activates Mimetic Desire. "See how it works" lowers activation energy vs. "How it works."

*Keys:* `hero_*` in `store_marketing`

---

## Hero variant B (shorter)

- **Headline:** Wake with momentum, not momentum-killing scroll.
- **Subhead:** Stand to silence the alarm. Let the app hold social feeds at bay. Finish with a routine that fits 10 minutes—or 60.

---

## "The problem" band (homepage or about teaser)

*(Keep as-is — strong contrast effect and reframing.)*

**Heading:** Most alarms don't fail you—your morning environment does.

**Body:** Snooze buys minutes but sells focus. The first glance at a phone can hijack the whole morning. MOOV isn't another louder alarm; it's a **system** that restores a sane sequence: **up → protected attention → intentional action.**

---

## "Who this is for" block (homepage or product intro)

*(New — self-selection copy using Similarity/Unity bias. Exclusion increases desire.)*

**MOOV is for you if:**
- You've hit snooze more mornings than you'd like to admit
- You know the first phone-check sets the tone for the whole day
- You've tried "just being more disciplined" and found willpower runs out

**MOOV is not for you if** you're looking for a louder alarm or a basic timer. This is a system.

---

## Three pillars (icon row / feature strip)

| Pillar | Title | Supporting line |
|--------|--------|------------------|
| 1 | **Stand to silence** | Disarm the alarm with a deliberate step—breaking the snooze loop before it starts. |
| 2 | **Quiet the feed** | Block the apps that pull you sideways until you've chosen to engage. |
| 3 | **Routines that learn** | Short, guided flows shaped by AI around your energy, time, and priorities—not generic hustle quotes. |

*Keys:* `pillar_1_title`, `pillar_1_body`, … `pillar_3_*`

---

## "What MOOV stands for" (acronym block—fresh wording)

**Intro:** MOOV is a name and a compass—four ideas we design against every day.

- **Momentum** — Progress you feel in your body before you feel it in your calendar.
- **Output** — Finishing small, honest wins that stack.
- **Optimism** — Mornings as a reset, not a verdict.
- **Vitality** — Energy that comes from alignment, not adrenaline alone.

**Closing line:** One system. Four commitments. Fewer reactive starts.

---

## Collection page intro

**Heading:** Shop MOOV

**Subhead:** Hardware and digital tools for a steadier first hour—built for people who want agency over attention.

**Empty state (if needed):** New drops land here first. Join the list or pre-order to hold your place.

---

## Product page (above the fold support bullets)

*(Rewritten outcome-first — Jobs-to-Be-Done framing. What the customer feels, not what the product does.)*

1. **Up in five minutes, committed for years** — peel-and-stick mount; no tools, no holes, no excuses.
2. **Break the snooze loop for good** — silencing requires one deliberate step. You can't tap it half-asleep.
3. **Your phone waits. You don't scroll first.** — the apps that derail mornings go quiet until you're ready.
4. **A 10-minute win every morning** — or 45 if you have it. Routines built around your actual day, not a guru's ideal one.
5. **Still going at week 12** — designed for sustainability, not the first-week high.

**Subtitle under price (pre-order):**

> No charge until we ship. You'll get a confirmation email with your estimated date, and you can cancel anytime before fulfilment.

> **Why this change:** The original ("We'll confirm timing as production milestones lock") introduced uncertainty at the payment moment — the worst place for doubt. This version answers the three silent objections: *When do I pay? When does it arrive? What if I change my mind?*

---

## Price anchoring (near product price)

*(New — Anchoring Effect. Visitors arrive with no price frame; this sets one before they see the number.)*

> Less than a month of coffee shop mornings. A fraction of what a sleep consultant charges.

Or, if competitor pricing is known:

> Comparable smart alarm hardware runs $150–$400. MOOV includes the app layer.

---

## Cart & checkout adjacent

**Cart trust line (revised):**

> Your order is protected — 30-day satisfaction promise. Cancel before fulfilment, no questions asked.

> **Why this change:** The original ("Secure checkout · Pre-order details at checkout") was sterile at the highest-friction moment. This version leads with risk reversal at the point where Loss Aversion and exit intent peak simultaneously.

**Order note placeholder:** "Gift message or delivery notes (optional)"

**Impact micro-line (footer or cart):** A share of revenue supports mental wellness and young builders—because better mornings should ripple outward.

*Align exact % and legal language with your approved source before publishing.*

---

## About teaser (homepage → full about page)

**Heading:** We're building for the first hour

**Body:** MOOV Technologies exists because mornings quietly steer mental health, focus, and confidence. We pair thoughtful hardware with software that respects attention—then we reinvest in communities that need more of both.

**CTA:** Read our story → (link to `/pages/about`)

---

## Newsletter / footer hook

*(Revised — adds an immediate deliverable to activate Zero-Price Effect and Present Bias.)*

**Heading:** Get the morning system guide — free

**Body:** Drop your email and we'll send our 5-day morning reset framework instantly. You'll also get early pricing and honest ship updates—no daily spam.

**Button:** Send it to me

> **Why this change:** "First to know" + "Notify me" offered no immediate reward. Adding a tangible deliverable (the guide) creates reciprocity, gives a reason to sign up today rather than later, and starts the commitment ladder toward a purchase.

---

## FAQ hooks (questions as H2s—answers in theme blocks)

*(Three psychological objections added — the logistics questions remain.)*

1. **Is MOOV just an alarm clock?**
2. **Do I need the app?**
3. **What if I share a bedroom?**
4. **How does pre-order billing work?**
5. **What's your return policy?**
6. **Is this actually going to change my mornings, or is it just another gadget I stop using?** *(Addresses scepticism head-on — admitting the risk makes the answer more credible)*
7. **I've tried morning routines before and they never stick. Why would this be different?** *(Loss aversion from past failures — the most common objection for habit-change products)*
8. **What does the companion app actually block, and can I override it?** *(Control anxiety — critical for an app-blocking feature)*

*(Draft answers in the theme editor; keep answers short and policy-accurate.)*

---

## One-liners (buttons, badges, banners)

- **Scarcity (only if true):** Early release allocation
- **Risk reversal:** 30 days to feel the difference
- **Community:** Join the MOOVment
- **Category:** Smart morning system
- **Social proof (soft):** 2,400+ mornings reclaimed
- **Proof style:** Built on behavioral science
- **Ownership:** Reserve yours

> **Change:** "Designed with behavioral science in mind" was too academic. "Built on behavioral science" is shorter and more confident. "Social proof (soft)" is new — update the number when real data is available.

---

## Legal-adjacent reminder

Before publishing: reconcile **impact pledge** wording and **pre-order terms** with legal-approved copy (your storefront policy pages and live flagship terms). This document is **marketing draft**, not legal text.

---

## Implementation

1. **Wired in theme:** `store_marketing.*` is used as fallbacks when section fields are left blank (or via toggles) in:
   - `sections/hero.liquid` — social proof, headline stack, CTAs, micro-trust
   - `sections/store-marketing-bands.liquid` — problem, who-for, about teaser (on `templates/index.json`)
   - `sections/product-intro.liquid` — intro line + proof chips + CTA defaults
   - `sections/app-section.liquid` — eyebrow, headline, body, three pillars
   - `sections/moov-product.liquid` — eyebrow, price anchor, pre-order note, feature strip from `product_bullet_1–4` (split on em dash)
   - `sections/moov-collection.liquid` — collection subheading + empty state
   - `sections/cart.liquid` — protected line + trust strip copy
   - `sections/footer.liquid` — newsletter + impact line + `general.socials`
   - `snippets/meta-tags.liquid` — homepage meta description when shop description is empty
2. Keep product-specific facts (ship date, specs, deep-dives) in **section settings** or **metafields** so merchandisers can edit without deploys.
3. Refresh this file when positioning shifts; keep `en.default.json` in sync for any key you reference in Liquid.

### Internal page map (Shopify replaces both legacy sites)

| Content | Legacy URL | Shopify page |
|---------|-----------|--------------|
| Brand story & mission | moovtech.ca | `/pages/about` |
| Product info & pre-order | moovmornings.com | `/products/moov-alarm` (or collection) |
| How it works | moovmornings.com/#how | `/pages/how-it-works` |
| Contact | moovtech.ca/contact | `/pages/contact` |
| FAQ | — | `/pages/faq` or `sections/faq.liquid` |

All footer, hero, and about CTA links should use the Shopify-internal paths above.

---

### Translation keys (`store_marketing.*`)

**Existing keys:**
`seo_title_suffix`, `seo_description`, `social_teaser`, `hero_eyebrow`, `hero_heading`, `hero_subheading`, `hero_cta_primary`, `hero_cta_secondary`, `hero_micro_trust`, `section_problem_heading`, `section_problem_body`, `pillar_1_title`, `pillar_1_body`, `pillar_2_title`, `pillar_2_body`, `pillar_3_title`, `pillar_3_body`, `acronym_intro`, `acronym_momentum`, `acronym_output`, `acronym_optimism`, `acronym_vitality`, `acronym_closing`, `collection_heading`, `collection_subheading`, `collection_empty`, `product_bullet_1`–`product_bullet_5`, `product_preorder_note`, `cart_trust_line`, `impact_micro_line`, `about_teaser_heading`, `about_teaser_body`, `about_teaser_cta`, `newsletter_heading`, `newsletter_body`, `newsletter_cta`, `badge_early_release`, `badge_30_day`, `badge_community`, `badge_category`

**New keys to add:**
`hero_social_proof`, `who_for_heading`, `who_for_1`, `who_for_2`, `who_for_3`, `who_for_not`, `price_anchor`, `cart_trust_line_v2`, `newsletter_guide_heading`, `newsletter_guide_body`, `newsletter_guide_cta`, `badge_social_proof`, `badge_ownership`

Example: `{{ 'store_marketing.hero_social_proof' | t }}`
