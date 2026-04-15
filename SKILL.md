---
name: ak-elementor-studio
description: >
  Elementor Pro Template Kit Builder. Converts ANY input — image (PNG/JPG/WebP), PDF, HTML+CSS+JS
  files, Figma screenshots, or design mockups — into a valid Elementor Pro Template Kit ZIP file
  ready to import on any WordPress site. Activate this skill whenever the user: uploads a design
  file and wants a full WordPress kit, says "build an elementor kit", "template kit", "kit zip",
  "elementor pro kit", "make me a full wordpress template", "generate elementor pages", or asks
  to convert a design into a multi-page Elementor template. This skill produces Envato-style
  importable .zip kits using ONLY native Elementor Pro widgets — zero raw HTML widgets for
  visual content.
---

# AK Elementor Studio
### by [Abanoub Khalil](https://akstudio.me) — Senior WordPress & Webflow Developer

---

## RULE #1 — THE MOST IMPORTANT RULE

```
HTML WIDGET = JAVASCRIPT <script> TAGS ONLY.

The html widget has exactly one valid use:
  → Embedding a <script> tag for JS behavior (GSAP, AOS, Swiper init, etc.)

That is ALL. Nothing else ever goes in the html widget.
Not headings. Not text. Not badges. Not cards. Not nav links.
Not pricing. Not timelines. Not icons. Not ANY visual content.

Every visual element has a native Elementor widget. Use it.
```

---

## RULE #2 — ABSOLUTE JSON RULES

```
1. NO COMMENTS — /* */ and // both break JSON parsing
2. NO TRAILING COMMAS
3. IDs = UNIQUE 8-CHAR HEX ONLY via secrets.token_hex(4)
4. EVERY ELEMENT MUST HAVE "elements": []
5. isInner RULE:
     Direct children of "content": []    → "isInner": false
     Containers nested inside containers → "isInner": true
     Widgets (inside any container)      → "isInner": false
6. HEX OR rgba() COLORS ONLY — never named colors
     ❌ "background_color": "transparent"
     ✅ "background_color": "rgba(0,0,0,0)"
     ❌ "color": "white"
     ✅ "color": "#FFFFFF"
7. PLACEHOLDER IMAGES: https://placehold.co/WxH/BGHEX/TEXTHEX?text=Label
8. RESPONSIVE: _tablet and _mobile suffixes required on ALL font sizes, paddings, widths
```

---

## TRIGGER CONDITIONS

Activate this skill when the user:

- Uploads PNG/JPG/WebP/PDF and mentions WordPress, Elementor, kit, or template
- Says: "build an elementor kit", "template kit zip", "kit zip", "elementor pro kit"
- Says: "make me a full wordpress template", "generate elementor pages for this design"
- Says: "convert this to a kit", "turn this mockup into a WordPress theme"
- Asks for a multi-page Elementor output (more than one page template)
- Mentions "Envato Elements", "Hello Elementor", "Theme Builder"
- Says: "convert this to elementor", "build this in wordpress", "turn this design into a page"
- Uploads any image/PDF and says "elementor", "wordpress", or "template" — even for a single page

---

## STEP 0 — PRE-FLIGHT INTERVIEW (all in ONE message, never skip)

```
👋 Quick questions before I start building your Elementor Kit:

1. KIT NAME & SLUG
   What should the kit be named? (e.g. "SaaS Pro Kit", slug: "saas-pro-kit")

2. PAGES NEEDED
   Which pages? (e.g. home, about, services, pricing, contact, blog)
   Or just type: "standard 5-page"

3. BRAND COLORS
   Paste your hex codes, OR type "extract from my design" and I'll pull them automatically.

4. FONTS
   Google Font preference? (e.g. "Inter", "Plus Jakarta Sans", "Syne + Inter")
   Or type "auto" to use Inter by default.

5. ELEMENTOR VERSION
   Pro (recommended) or Free?
   Pro = sticky header, Theme Builder header/footer, Form widget, Loop Grid
   Free = header/footer baked into each page, shortcode for forms

6. CUSTOM POST TYPES
   Any CPTs needed? (Blog, Portfolio, Services, Team, Testimonials)
   These add pro Posts/Loop Grid widgets and ACF field hints.

7. THEME
   Dark or Light?

8. INDUSTRY / NICHE
   e.g. SaaS, Agency, Restaurant, Medical, eCommerce, Portfolio, Education
   (Affects default copy tone and section order)
```

---

## STEP 1 — INPUT DETECTION

| Input Type | Pipeline Used |
|---|---|
| PNG / JPG / WebP / screenshot / Figma export | Visual Pipeline |
| PDF (single or multi-page) | PDF Pipeline → Visual Pipeline per page |
| HTML + CSS + JS files | Code Pipeline |
| Text description only | Description Pipeline |
| Existing Elementor JSON | Edit Pipeline |
| Mixed inputs | All relevant pipelines, merged |

---

## STEP 2 — ANALYSIS PHASE

### Visual Pipeline (images + PDFs)
1. Extract dominant colors via PIL quantization (top 6 hex values)
2. Detect dark/light theme from luminance of primary color
3. Detect section boundaries by horizontal brightness bands
4. Identify section types: navbar, hero, features, stats, testimonials, pricing, cta, footer
5. Extract fonts from any visible text (estimate if needed)
6. Note animation libraries if script tags are visible (GSAP, AOS, Swiper)

### Code Pipeline (HTML/CSS/JS)
1. Parse all `#RRGGBB` hex values from CSS → brand colors
2. Extract `font-family` declarations → font list
3. Map structural HTML tags (`<header>`, `<section>`, `<footer>`) to section types
4. Detect inline background colors from style attributes
5. Find animation library script srcs → note for html widget injection

### Output — DesignSpec object:
```
pages:            list of page names
brand_colors:     [primary_bg, accent, light_bg, card_bg, dark_bg, highlight]
fonts:            [heading_font, body_font]
sections:         ordered list of detected sections with types and bg colors
is_dark_theme:    boolean
has_animations:   boolean
animation_scripts: list of script src URLs
```

---

## STEP 3 — WIDGET ASSIGNMENT REFERENCE

### Free Widgets
| Element | Widget |
|---|---|
| h1 / h2 / h3 / h4 / h5 / h6 | `heading` |
| Paragraph / body text | `text-editor` |
| Button / CTA | `button` |
| Photo / image | `image` |
| Icon + title + description | `icon-box` |
| Bulleted/icon list | `icon-list` |
| Photo + title + text card | `image-box` |
| Customer quote | `testimonial` |
| Animated number counter | `counter` |
| Progress / skill bar | `progress` |
| Horizontal line | `divider` |
| Vertical blank space | `spacer` |
| FAQ / collapsible | `accordion` |
| Tab navigation | `tabs` |
| Social media icons | `social-icons` |
| YouTube / Vimeo | `video` |
| Image slider | `image-carousel` |
| Image grid | `image-gallery` |
| Map embed | `google-maps` |
| Plugin shortcode | `shortcode` |
| Info / status banner | `alert` |

### Pro Widgets
| Element | Widget |
|---|---|
| Blog / post grid | `posts` |
| Portfolio grid | `portfolio` |
| Full-screen slider | `slides` |
| Price list | `price-list` |
| Pricing card | `price-table` |
| Flip card | `flip-box` |
| CTA with bg image | `call-to-action` |
| Countdown timer | `countdown` |
| Share buttons | `share-buttons` |
| Navigation menu | `nav-menu` |
| Site logo | `site-logo` |
| Site title | `site-title` |
| Page title (Theme Builder) | `page-title` |
| Post content (Theme Builder) | `post-content` |
| Contact / lead form | `form` |

### ⚠️ html widget — ONLY for `<script>` tags
```json
{
  "widgetType": "html",
  "settings": {
    "html": "<script src=\"https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js\"></script>"
  }
}
```

---

## STEP 4 — COMPLEX ELEMENT PATTERNS

### Navigation Bar (Pro)
```
flex container (row, space-between, sticky)
  └── site-logo widget
  └── icon-list widget (layout: inline, icons: none) ← nav links
  └── button widget ← CTA
```

### Navigation Bar (Free)
```
flex container (row, space-between)
  └── image widget ← logo PNG/SVG
  └── icon-list widget (layout: inline) ← nav links
  └── button widget ← CTA
```

### Badge / Status Pill / Tag
```
button widget (size: xs, border_radius: 100px)
  button_background_color: rgba(0,90,255,0.12)
  button_text_color: #005AFF
  link: { url: "#" }
```
OR:
```
alert widget (type: info/success/warning, show_dismiss_button: no)
```

### Pricing Card
```
inner container (column, bg: card color, border-radius: 12px, padding: 40px 32px)
  └── text-editor widget    ← plan label (uppercase, small)
  └── heading widget (h2)   ← "$49" price
  └── text-editor widget    ← "/month" period (muted)
  └── divider widget
  └── icon-list widget      ← features (fas fa-check icons)
  └── button widget         ← CTA (full-width)
```
Featured plan: add `border_border: solid, border_color: accent, border_width: 2px`

### Testimonial Card
```
inner container (column, bg: card color, border-radius: 12px, padding: 28px 24px)
  └── testimonial widget
      image: avatar URL
      name: "Client Name"
      testimonial_job: "Title, Company"
      testimonial_content: "Quote here."
```

### Footer (4-column)
```
section-wrapper (bg: dark)
  └── row container (4 cols, flex-wrap)
      └── col 1 (25%): image (logo) + text-editor (tagline) + social-icons
      └── col 2 (20%): heading (h5) + icon-list (nav links)
      └── col 3 (20%): heading (h5) + icon-list (nav links)
      └── col 4 (25%): heading (h5) + text-editor (contact) + button (CTA)
  └── divider widget
  └── row container: text-editor (copyright) + text-editor (links)
```

### Stats Row
```
row container (4 cols, flex-wrap, space-around)
  └── counter widget × 4 (number, suffix, title, color)
```
### Career Timeline / Work History Entry
```
inner container (column, border-left: 2px solid accent, padding-left: 20px)
  └── inner container (row, gap: 8px)
      └── text-editor widget    ← "OCT 2024 — PRESENT" (small, muted)
      └── button widget (xs, pill, rgba bg) ← "FULL-TIME" badge
  └── heading widget (h3)       ← Job title
  └── text-editor widget        ← "Company — Location"
  └── text-editor widget        ← Description paragraph
  └── inner container (row, flex-wrap, gap: 8px)
      └── button widget (xs, ghost) × N  ← tech stack tags
```

### Contact Info Panel
```
inner container (column, bg: card, border-radius: 12px, padding: 32px)
  └── inner container (row) × N  ← one per info row
      └── text-editor widget    ← label "LOCATION" (small, muted, uppercase)
      └── text-editor widget    ← value "Cairo, Egypt"
  └── inner container (row)     ← clickable row
      └── text-editor widget    ← label "EMAIL"
      └── button widget (link)  ← "hello@example.com" (href: mailto:)
  └── alert widget              ← "✅ Currently available" status
  └── button widget             ← "Book a discovery call" CTA (full-width)
```

### Logo Cloud / Partner Logos
```
image-carousel widget
  slides_to_show: 5 (tablet: 3, mobile: 2)
  autoplay: yes
  navigation: none
  images: [logo1, logo2, logo3, logo4, logo5]
```



### Hero (2-col with image)
```
section-wrapper (bg: dark, min-height: 90vh, flex-direction: row → column on tablet/mobile)
  └── inner col (55%):
      └── alert (badge)
      └── heading (h1, large)
      └── text-editor (subheadline)
      └── row container: button (primary CTA) + button (ghost secondary)
  └── inner col (45%):
      └── image widget (border-radius: 12px)
```

---

## STEP 5 — CSS → ELEMENTOR QUICK REFERENCE

```json
padding/margin:
  "padding": { "unit": "px", "top": "80", "right": "60", "bottom": "80", "left": "60", "isLinked": false }
  "padding_tablet": { "unit": "px", "top": "50", "right": "30", "bottom": "50", "left": "30", "isLinked": false }
  "padding_mobile": { "unit": "px", "top": "40", "right": "20", "bottom": "40", "left": "20", "isLinked": false }

typography:
  "typography_typography": "custom"
  "typography_font_family": "Inter"
  "typography_font_weight": "700"
  "typography_font_size": { "unit": "px", "size": 48 }
  "typography_font_size_tablet": { "unit": "px", "size": 36 }
  "typography_font_size_mobile": { "unit": "px", "size": 28 }
  "typography_line_height": { "unit": "em", "size": 1.3 }
  "typography_letter_spacing": { "unit": "px", "size": -1 }
  "typography_text_transform": "uppercase"

background gradient:
  "background_background": "gradient"
  "background_color": "#005AFF"
  "background_color_b": "#1A1A2E"
  "background_gradient_type": "linear"
  "background_gradient_angle": { "unit": "deg", "size": 135 }

border + radius:
  "border_border": "solid"
  "border_width": { "unit": "px", "top": "1", "right": "1", "bottom": "1", "left": "1", "isLinked": true }
  "border_color": "#E5E7EB"
  "border_radius": { "unit": "px", "top": "12", "right": "12", "bottom": "12", "left": "12", "isLinked": true }

box shadow:
  "box_shadow_box_shadow_type": "yes"
  "box_shadow_box_shadow": { "horizontal": 0, "vertical": 4, "blur": 24, "spread": 0, "color": "rgba(0,0,0,0.08)" }

responsive width:
  "width": { "unit": "%", "size": 33 }
  "width_tablet": { "unit": "%", "size": 50 }
  "width_mobile": { "unit": "%", "size": 100 }

flex gap:
  "flex_gap": { "unit": "px", "column": "24", "row": "24", "isLinked": true }

flex wrap:
  "flex_wrap": "wrap"

sticky (Pro):
  "sticky": "top"
  "sticky_offset": 0
```

---

## STEP 6 — JSON SCHEMA REFERENCE

### Page JSON wrapper
```json
{
  "title": "Home",
  "type": "page",
  "version": "0.4",
  "page_settings": {
    "hide_title": true,
    "page_layout": "elementor_canvas"
  },
  "content": [],
  "settings": {}
}
```

### Section container (top-level)
```json
{
  "id": "a1b2c3d4",
  "elType": "container",
  "isInner": false,
  "settings": {
    "content_width": "full",
    "flex_direction": "column",
    "flex_align_items": "center",
    "flex_justify_content": "flex-start",
    "flex_gap": { "unit": "px", "column": "30", "row": "30", "isLinked": true },
    "padding": { "unit": "px", "top": "80", "right": "60", "bottom": "80", "left": "60", "isLinked": false },
    "padding_tablet": { "unit": "px", "top": "50", "right": "30", "bottom": "50", "left": "30", "isLinked": false },
    "padding_mobile": { "unit": "px", "top": "40", "right": "20", "bottom": "40", "left": "20", "isLinked": false },
    "background_background": "classic",
    "background_color": "#FFFFFF"
  },
  "elements": []
}
```

### Inner container (nested)
```json
{
  "id": "b2c3d4e5",
  "elType": "container",
  "isInner": true,
  "settings": {
    "width": { "unit": "%", "size": 50 },
    "width_tablet": { "unit": "%", "size": 100 },
    "width_mobile": { "unit": "%", "size": 100 },
    "flex_direction": "column",
    "flex_align_items": "flex-start",
    "padding": { "unit": "px", "top": "20", "right": "20", "bottom": "20", "left": "20", "isLinked": true }
  },
  "elements": []
}
```

### Widget
```json
{
  "id": "c3d4e5f6",
  "elType": "widget",
  "widgetType": "heading",
  "isInner": false,
  "settings": {},
  "elements": []
}
```

---

## STEP 7 — KIT ZIP STRUCTURE

```
kit-slug.zip
├── kit.json                        ← Kit metadata + template list
└── templates/
    ├── global-styles.json          ← Global Kit Styles (IMPORT THIS FIRST)
    ├── header.json                 ← Pro only (type: "header")
    ├── footer.json                 ← Pro only (type: "footer")
    ├── home.json
    ├── about.json
    ├── services.json
    ├── pricing.json
    └── contact.json
```

### kit.json schema
```json
{
  "name": "Kit Name",
  "slug": "kit-slug",
  "version": "1.0.0",
  "author": "Your Name",
  "authorURL": "https://yoursite.com",
  "templates": [
    { "name": "Global Styles", "file": "templates/global-styles.json", "type": "kit-styles",  "thumbnail": "" },
    { "name": "Header",        "file": "templates/header.json",         "type": "header",      "thumbnail": "" },
    { "name": "Footer",        "file": "templates/footer.json",         "type": "footer",      "thumbnail": "" },
    { "name": "Home",          "file": "templates/home.json",           "type": "page",        "thumbnail": "" },
    { "name": "About",         "file": "templates/about.json",          "type": "page",        "thumbnail": "" }
  ],
  "requiredPlugins": [
    { "name": "Elementor",     "slug": "elementor",     "source": "wordpress" },
    { "name": "Elementor Pro", "slug": "elementor-pro", "source": "wordpress" }
  ]
}
```

### global-styles.json schema
```json
{
  "title": "Global Kit Styles",
  "type": "kit-styles",
  "version": "0.4",
  "page_settings": {},
  "content": [],
  "settings": {
    "system_colors": [
      { "_id": "primary",   "title": "Primary",   "color": "#005AFF" },
      { "_id": "secondary", "title": "Secondary",  "color": "#1A1A2E" },
      { "_id": "text",      "title": "Text",       "color": "#111827" },
      { "_id": "accent",    "title": "Accent",     "color": "#00C87A" }
    ],
    "custom_colors": [
      { "_id": "c_bg",    "title": "Background", "color": "#FFFFFF" },
      { "_id": "c_card",  "title": "Card",        "color": "#F9FAFB" },
      { "_id": "c_muted", "title": "Muted",       "color": "#6B7280" },
      { "_id": "c_border","title": "Border",      "color": "#E5E7EB" }
    ],
    "system_typography": [
      { "_id": "primary",   "title": "Primary",   "typography_typography": "custom",
        "typography_font_family": "Inter", "typography_font_weight": "700",
        "typography_font_size": {"unit":"px","size":48},
        "typography_font_size_tablet": {"unit":"px","size":36},
        "typography_font_size_mobile": {"unit":"px","size":28},
        "typography_line_height": {"unit":"em","size":1.15},
        "typography_letter_spacing": {"unit":"px","size":-1} },
      { "_id": "secondary", "title": "Secondary", "typography_typography": "custom",
        "typography_font_family": "Inter", "typography_font_weight": "600",
        "typography_font_size": {"unit":"px","size":24},
        "typography_font_size_tablet": {"unit":"px","size":20},
        "typography_font_size_mobile": {"unit":"px","size":18},
        "typography_line_height": {"unit":"em","size":1.3} },
      { "_id": "text",      "title": "Text",      "typography_typography": "custom",
        "typography_font_family": "Inter", "typography_font_weight": "400",
        "typography_font_size": {"unit":"px","size":16},
        "typography_font_size_mobile": {"unit":"px","size":15},
        "typography_line_height": {"unit":"em","size":1.75} },
      { "_id": "accent",    "title": "Accent",    "typography_typography": "custom",
        "typography_font_family": "Inter", "typography_font_weight": "500",
        "typography_font_size": {"unit":"px","size":13},
        "typography_letter_spacing": {"unit":"px","size":1},
        "typography_text_transform": "uppercase" }
    ],
    "custom_typography": [],
    "default_generic_fonts": "Inter, sans-serif"
  }
}
```

---

## STEP 8 — PRO vs FREE MATRIX

```
⚠️ PRO ONLY:
  type: "header" / "footer" in kit        → Requires Elementor Pro Theme Builder
  nav-menu widget                          → Requires Elementor Pro
  site-logo widget                         → Requires Elementor Pro
  site-title widget                        → Requires Elementor Pro
  page-title widget                        → Requires Elementor Pro
  post-content widget                      → Requires Elementor Pro
  form widget                              → Requires Elementor Pro
  posts widget                             → Requires Elementor Pro
  portfolio widget                         → Requires Elementor Pro
  slides widget                            → Requires Elementor Pro
  price-list widget                        → Requires Elementor Pro
  price-table widget                       → Requires Elementor Pro
  flip-box widget                          → Requires Elementor Pro
  call-to-action widget                    → Requires Elementor Pro
  countdown widget                         → Requires Elementor Pro
  share-buttons widget                     → Requires Elementor Pro
  sticky (on containers)                   → Requires Elementor Pro
  custom_css per element                   → Requires Elementor Pro
  Motion Effects / Scroll Effects          → Requires Elementor Pro

✅ FREE ALTERNATIVES:
  header/footer     → Section containers baked into each page file (type: "page")
  nav-menu          → icon-list widget (layout: inline, icon: none per item)
  site-logo         → image widget (link to homepage)
  site-title        → heading widget + link
  forms             → shortcode widget + WPForms/ContactForm7 free plugin
  posts grid        → shortcode widget + custom query plugin
  price-table       → inner container + heading + icon-list + button
  countdown         → shortcode widget + plugin
  slides            → image-carousel widget (limited but free)
  animations        → Entrance Animations (Advanced tab, free)
```

---

## STEP 9 — OUTPUT CHECKLIST

Run through before delivering:

- [ ] ZERO html widgets for visual content — html = `<script>` only
- [ ] Nav links → `icon-list` or `button` widgets
- [ ] Badges/tags → `alert` or `button` (xs, pill, rgba bg)
- [ ] No named colors anywhere (`white`, `black`, `transparent`, etc.) → use hex/rgba
- [ ] No JSON comments (`/* */` or `//`)
- [ ] Every element has unique 8-char hex `id`
- [ ] Every element has `"elements": []`
- [ ] Top-level content children: `"isInner": false`
- [ ] All nested containers: `"isInner": true`
- [ ] All widgets: `"isInner": false`
- [ ] Responsive `_tablet` + `_mobile` on font sizes, padding, widths
- [ ] Placeholder images use `https://placehold.co/WxH/BGHEX/TEXTHEX?text=Label`
- [ ] global-styles.json with real brand colors and fonts
- [ ] header.json + footer.json present (Pro only)
- [ ] kit.json present and valid
- [ ] ⚠️ Pro-only features flagged in delivery message

---

## STEP 10 — USING THE PYTHON BUILDER

The Python builder (included in this repo under `builder/`) handles all input parsing
and JSON assembly automatically. Claude should use it when running in a computer-use
or Claude Code context. In chat-only contexts, Claude generates JSON directly using
the schemas above.

### Install dependencies
```bash
pip install Pillow PyMuPDF beautifulsoup4 lxml --break-system-packages
```

### CLI usage

**From an image (PNG/JPG/WebP):**
```bash
python main.py \
  --input design.png \
  --kit-name "SaaS Landing" \
  --kit-slug "saas-landing" \
  --pages home,about,pricing,contact \
  --output ./output/
```

**From a PDF mockup:**
```bash
python main.py \
  --input mockup.pdf \
  --kit-name "Agency Kit" \
  --kit-slug "agency-kit" \
  --pages home,services,portfolio,contact \
  --output ./output/
```

**From HTML + CSS + JS:**
```bash
python main.py \
  --input index.html \
  --css style.css \
  --js main.js \
  --kit-name "Corporate Kit" \
  --kit-slug "corporate-kit" \
  --pages home,about,services,contact \
  --output ./output/
```

**From a text description:**
```bash
python main.py \
  --description "Dark SaaS landing page with hero, features grid, testimonials, and pricing" \
  --kit-name "Dark SaaS Kit" \
  --kit-slug "dark-saas-kit" \
  --pages home,pricing,contact \
  --industry saas \
  --output ./output/
```

**Force light theme:**
```bash
python main.py --input design.png --kit-name "My Kit" --light --output ./output/
```

**Elementor Free (no Pro widgets):**
```bash
python main.py --input design.png --kit-name "My Kit" --free --output ./output/
```

### Builder modules
```
builder/
  __init__.py        ← Package exports
  analyzer.py        ← Input analysis (image/PDF/HTML/description → DesignSpec)
  widget_factory.py  ← All widget JSON factory functions
  json_builder.py    ← Section + page assemblers, validation
  kit_assembler.py   ← ZIP builder
```

### Run tests
```bash
python tests/test_kit_builder.py
# Expected: 11 passed, 0 failed
```

---

## STEP 11 — SECTION PATTERNS REFERENCE

Pre-built JSON patterns are in `templates/section_patterns/`:

| File | Section | Key Widgets |
|---|---|---|
| `hero.json` | Full-screen hero, 2-col | alert (badge), heading (h1), text-editor, 2× button, image |
| `navbar.json` | Sticky top nav (Pro) | site-logo, icon-list, button |
| `features_grid.json` | 3-col feature cards | heading, 3× icon-box in bordered containers |
| `cta_banner.json` | Full-width gradient CTA | heading, text-editor, 2× button |
| `pricing_table.json` | 3-tier pricing | 3× [heading + icon-list + button] containers |
| `testimonials.json` | 3-col social proof | 3× testimonial in card containers |
| `footer.json` | 4-col footer + bottom bar | image, text-editor, social-icons, 2× icon-list, button, divider |

---

## STEP 12 — DEFAULT SECTION ORDER BY PAGE

| Page | Sections (in order) |
|---|---|
| home | navbar → hero → features → stats → testimonials → pricing → cta → footer |
| about | navbar → hero → features → stats → footer |
| services | navbar → hero → features → footer |
| pricing | navbar → hero → pricing → cta → footer |
| contact | navbar → hero → footer |
| landing | navbar → hero → features → cta → footer |
| team | navbar → hero → features → footer |
| faq | navbar → hero → accordion → cta → footer |
| blog | navbar → hero → posts (Pro) → footer |
| portfolio | navbar → hero → portfolio (Pro) → footer |

---

## STEP 13 — IMPORT INSTRUCTIONS (always include in delivery)

### Method A — Envato Kit ZIP (Pro, recommended)

**Prerequisites:**
- Hello Elementor theme active
- Elementor Pro installed and active
- Elementor → Settings → Features → Flexbox Container → **Active**
- WordPress → Settings → Permalinks → Post Name → Save Changes

**Steps:**
1. WordPress → Tools → Template Kit → **Upload Template Kit ZIP File**
2. Upload the `.zip` file
3. Click **"Import Global Kit Styles" FIRST** (sets brand colors + fonts site-wide)
4. Then click **"Import Template"** on each page in order (Header first, Footer second, then pages)
5. To apply a template: Pages → Add New → Edit with Elementor → folder icon → **View Installed Kit** → Insert Template

### Method B — Individual JSON files (Free + Pro)

1. WordPress → Elementor → Templates → **Saved Templates**
2. Click the import icon (top right)
3. Select a `.json` file from inside the ZIP → **Import Now**
4. Repeat for each template
5. To use: open a page in Elementor → folder icon → **My Templates** → Insert

---

## POST-IMPORT CHECKLIST (include in every delivery)

After importing, the user should:

- [ ] Replace all `placehold.co` images with real images
- [ ] Update nav links to point to actual page URLs
- [ ] Set the correct menu in Appearance → Menus (for nav-menu widget, Pro)
- [ ] Update email address in the form widget or shortcode
- [ ] Replace `© 2025 YourBrand` in the footer
- [ ] Set the site logo in Elementor → Site Settings → Site Identity
- [ ] Activate Google Fonts if using non-system fonts
- [ ] Test on mobile — check tablet + mobile breakpoints
- [ ] If using header/footer templates: assign them in Elementor Pro → Theme Builder

---

## ANALYSIS SUMMARY FORMAT

Always show this before outputting JSON or delivering a kit:

```markdown
## 🔍 Design Analysis — [Kit Name]

**Source:** [image / PDF / HTML / description]
**Style:** [Dark / Light] · [Minimal / Bold / Corporate / Playful]

| Token       | Value        |
|-------------|--------------|
| Primary BG  | #______      |
| Accent      | #______      |
| Heading txt | #______      |
| Body text   | #______      |
| Card BG     | #______      |
| Border      | #______      |
| Heading font| Inter 700    |
| Body font   | Inter 400    |

**Pages:** home, about, services, pricing, contact

**Widget map:**
1. Navbar   → site-logo + icon-list (nav links) + button (CTA)
2. Hero     → alert (badge) + heading (h1) + text-editor + 2× button + image
3. Features → heading + 3× icon-box in card containers
4. Stats    → 4× counter widgets
5. Pricing  → heading + 3× price-table containers
6. CTA      → heading + text-editor + 2× button (gradient bg)
7. Footer   → image + text-editor + social-icons + 2× icon-list + text-editor

**html widgets used:** None.  (or list only <script> injections)

**⚠️ Pro features used:** sticky nav, site-logo, form widget, header/footer templates
**⚠️ After import:** replace placeholder images, update nav links, set real logo
```
