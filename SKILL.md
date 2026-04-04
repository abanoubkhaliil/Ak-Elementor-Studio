---
name: ak-elementor-studio
description: >
  AK Elementor Studio — built by Abanoub Khalil (akstudio.me). Converts ANY input — HTML/CSS/JS code, screenshots, images (PNG, JPG, WebP), PDFs, Figma exports, design mockups, wireframes, or any visual file — into fully structured Elementor page JSON templates importable into WordPress. Activate this skill whenever the user: uploads a design image or screenshot and wants it built in WordPress, shares HTML/CSS/JS to convert to Elementor, pastes code or attaches a file and mentions Elementor, WordPress, or page builder, says anything like "convert this to Elementor", "build this in WordPress", "turn this design into a page", "make this Elementor template", "recreate this layout in Elementor", "import this into WordPress", or uploads a PDF/image of a webpage, landing page, mockup, or UI design. This skill handles ALL input types: code, images, PDFs, mixed inputs, and even verbal descriptions of layouts.
---

# AK Elementor Studio
### by [Abanoub Khalil](https://akstudio.me) — Senior WordPress & Webflow Developer

> **AK Elementor Studio** is a professional Claude skill that converts any design input into production-ready WordPress Elementor templates — built and maintained by Abanoub Khalil, Senior WordPress & Webflow Developer with 7+ years of experience at [akstudio.me](https://akstudio.me).

Converts **any input format** into production-ready Elementor template JSON files (`.json`) importable directly into WordPress, plus optional direct publishing via the WordPress MCP.

---

## STEP 0 — Input Type Detection (Always Do This First)

Before anything else, identify what the user provided and route to the correct pipeline:

| Input Type | Detection Signals | Pipeline |
|---|---|---|
| **HTML/CSS/JS code** | Code blocks, `.html`/`.css`/`.js` files, pasted markup | → [Code Pipeline](#code-pipeline) |
| **Image file** | `.png`, `.jpg`, `.jpeg`, `.webp`, `.gif`, `.svg` attached | → [Visual Pipeline](#visual-pipeline) |
| **PDF file** | `.pdf` attached | → [PDF Pipeline](#pdf-pipeline) |
| **Figma / Design export** | Figma link, `.fig`, design system screenshot | → [Visual Pipeline](#visual-pipeline) |
| **Screenshot / mockup** | Any image of a webpage, UI, wireframe, or app screen | → [Visual Pipeline](#visual-pipeline) |
| **Mixed** | Code + image together | → Run both pipelines, merge results |
| **Description only** | User describes the page in words | → [Description Pipeline](#description-pipeline) |
| **Elementor JSON (edit)** | Existing `.json` Elementor file | → [Edit Pipeline](#edit-pipeline) |

---

## Code Pipeline

For HTML, CSS, JavaScript inputs.

### Step 1 — Parse Structure
- Identify top-level sections (hero, navbar, features, about, CTA, footer, etc.)
- Each major section → one Elementor **container**
- Note column layouts (2-col, 3-col, etc.) → inner containers with `width` percentages

### Step 2 — Extract Styles
- Pull all `background`, `color`, `font-*`, `padding`, `margin`, `border`, `box-shadow` values
- Map every CSS property to Elementor settings using `references/style-map.md`
- CSS Grid / complex layouts → use `html` widget with scoped `<style>`

### Step 3 — Map Elements to Widgets
Use the widget map table below. Read `references/widget-map.md` for full JSON settings per widget.

### Step 4 — Handle JavaScript
See [JavaScript Strategy](#javascript-strategy) section.

### Step 5 — Build & Output JSON
Assemble full document, validate IDs are unique, output `.json` file.

---

## Visual Pipeline

For PNG, JPG, WebP, GIF, SVG, Figma exports, screenshots, mockups, wireframes.

### Phase 1 — Visual Scan (Read image top-to-bottom)

Scan the design in this order and document findings:

```
1. NAVBAR / HEADER
   - Logo position (left/center/right)
   - Navigation links (list them)
   - CTA button text & color
   - Background color / transparency

2. HERO SECTION
   - Full-width or boxed?
   - Background: solid color / image / gradient / video?
   - Headline text (estimate font size: huge=64px+, large=48px, medium=36px)
   - Subheadline / tagline text
   - CTA button(s): text, colors
   - Hero image position (left/right/center/background)
   - Min-height: full-screen (100vh) or fixed height?

3. CONTENT SECTIONS (repeat for each)
   - Section purpose (features, about, stats, services, portfolio, etc.)
   - Layout: single column / 2-col / 3-col / 4-col / asymmetric?
   - Column widths (estimate: 50/50, 60/40, 33/33/33, etc.)
   - Background color / image
   - Widget types present in each column

4. FOOTER
   - Columns count
   - Content: logo, links, social icons, copyright, newsletter

5. GLOBAL STYLES
   - Primary color (dominant brand color)
   - Secondary / accent color
   - Text color (body, headings)
   - Background color (default sections)
   - Font style (serif / sans-serif / monospace — approximate)
   - Rounded vs sharp corners (estimate border-radius)
   - Card/box shadows visible?
```

### Phase 2 — Color Extraction

Extract exact hex colors by visual estimation:

```
For each color observed:
- Pure white → #FFFFFF
- Off-white / light gray → #F8F9FA or #F5F5F5
- Dark navy → #1A1A2E or #0D1B2A
- Dark gray text → #333333 or #222222
- Medium gray → #666666 or #777777
- Light gray → #BBBBBB or #CCCCCC
- Pure black → #000000
- Common brand blues → #007BFF, #0066CC, #2D6FDB
- Common greens → #28A745, #4CAF50, #2ECC71
- Common reds → #DC3545, #E53935, #FF4757
- Common oranges → #FD7E14, #FF6348, #F39C12
- Gradients: identify start + end colors + angle

ALWAYS provide a hex value — never output "blue" or "dark".
```

### Phase 3 — Typography Estimation

Estimate font sizes from visual proportion:

```
Pixel size estimation guide:
- Massive hero title (dominates the screen)  → 72–96px
- Large hero title                            → 56–64px
- Section headline                            → 36–48px
- Card title / sub-heading                   → 24–30px
- Body text (comfortable reading size)        → 15–18px
- Small caption / label                       → 12–14px
- Navigation links                            → 14–16px
- Button text                                 → 14–16px

Font weight from visual weight:
- Hair-thin lines           → "100" or "200"
- Thin                      → "300"
- Normal/regular body text  → "400"
- Medium (slightly bold)    → "500" or "600"
- Bold headings             → "700"
- Extra bold / heavy        → "800" or "900"

Font family approximation:
- Geometric, modern sans    → "Montserrat", "Poppins", or "Nunito"
- Clean, neutral sans       → "Inter", "Roboto", or "Open Sans"
- Humanist sans             → "Lato" or "Source Sans Pro"
- Elegant serif             → "Playfair Display" or "Merriweather"
- Slab serif                → "Roboto Slab"
- Monospace/code            → "Roboto Mono" or "Fira Code"
- Display / decorative      → "Oswald" or "Raleway"
Always choose from Google Fonts.
```

### Phase 4 — Layout Grid Mapping

Map visual column layouts to Elementor inner containers:

```
LAYOUT PATTERNS → ELEMENTOR STRUCTURE:

Full-width single column:
→ 1 container (flex-direction: column, content_width: full)

Two equal columns (50/50):
→ parent container (flex-direction: row) +
  2 inner containers (width: 50% each)

Two unequal columns (60/40 or 40/60):
→ parent container (flex-direction: row) +
  inner container A (width: 60%) +
  inner container B (width: 40%)

Three equal columns (33/33/33):
→ parent container (flex-direction: row, flex-wrap: wrap) +
  3 inner containers (width: 33.33% each)

Four equal columns:
→ parent container (flex-direction: row, flex-wrap: wrap) +
  4 inner containers (width: 25% each)

Feature cards (3+ cards in a row):
→ parent container (flex-direction: row, flex-wrap: wrap, flex-justify-content: space-between) +
  N inner containers (width: 30% or 31%)

Alternating image+text (left image, right text):
→ parent container (flex-direction: row) +
  inner container LEFT (width: 50%) + image widget
  inner container RIGHT (width: 50%) + heading + text + button

Hero with centered text only:
→ single container (flex-direction: column, flex-align-items: center, min_height: 100vh)
  + heading + text-editor + button

Hero with text-left, image-right:
→ parent container (flex-direction: row, min_height: 100vh) +
  inner LEFT (width: 50%) + heading + text + button +
  inner RIGHT (width: 50%) + image widget
```

### Phase 5 — Element-to-Widget Recognition

Match visual elements to Elementor widgets:

| Visual Element Seen | Elementor Widget | `widgetType` |
|---|---|---|
| Large headline text | Heading | `heading` |
| Paragraph / body text | Text Editor | `text-editor` |
| Photo / illustration | Image | `image` |
| Click/CTA button | Button | `button` |
| YouTube/Vimeo frame | Video | `video` |
| Horizontal line | Divider | `divider` |
| Empty gap / spacing | Spacer | `spacer` |
| Icon + title + description card | Icon Box | `icon-box` |
| List with icon bullets | Icon List | `icon-list` |
| Photo + title + description | Image Box | `image-box` |
| Animated counter (500+, 10K, etc.) | Counter | `counter` |
| Quote with avatar + name | Testimonial | `testimonial` |
| Expandable FAQ rows | Accordion | `accordion` |
| Tab navigation UI | Tabs | `tabs` |
| Social media icon row | Social Icons | `social-icons` |
| Map embed | Google Maps | `google-maps` |
| Logo strip / partner logos | Image Carousel | `image-carousel` |
| Photo gallery grid | Image Gallery | `image-gallery` |
| Progress/skill bar | Progress Bar | `progress` |
| Pricing table | HTML widget (custom) | `html` |
| Complex interactive element | HTML widget | `html` |
| Star rating row | HTML widget | `html` |

### Phase 6 — Spacing Estimation

Estimate padding/margin values from visual proportions:

```
Section padding (top/bottom):
- Very tight sections     → 20–30px
- Normal sections         → 60–80px
- Hero / large sections   → 100–120px
- Extra spacious          → 150–200px

Section padding (left/right):
- Full-width sections     → 0px (background bleeds)
- Contained content       → 20–60px sides

Card/widget internal padding:
- Tight card              → 15–20px
- Normal card             → 25–30px
- Spacious card           → 40–60px

Column gap / space between cards:
- Tight                   → 10–15px
- Normal                  → 20–30px
- Spacious                → 40–60px
```

### Phase 7 — Generate JSON

After completing the visual scan:
1. Build the full Elementor JSON document section by section
2. Apply extracted colors, fonts, spacing to every element
3. Use placeholder image URLs (`https://placehold.co/800x600/EEEEEE/999999?text=Image`) for any images
4. Add a `<!-- Visual Analysis Note -->` comment section before the JSON explaining what was detected
5. Output the `.json` file
6. Optionally, flag elements that need user input (e.g., "Replace placeholder image with your actual photo")

---

## PDF Pipeline

For `.pdf` files attached by the user.

### Step 1 — Determine PDF type
- **Single-page PDF**: treat as a design image → run Visual Pipeline on each rendered page
- **Multi-page PDF** (wireframe doc, style guide, design spec): analyze page by page
- **PDF with selectable text**: extract text content first, then analyze layout visually

### Step 2 — Page-by-page analysis
For each page in the PDF:
1. Run the Visual Pipeline (Phase 1–6)
2. Create one Elementor template per logical page/section
3. If PDF is a style guide: extract colors, fonts, spacing rules and apply globally

### Step 3 — Multi-page output
- Single-page PDF → one `.json` template file
- Multi-page PDF → one `.json` per meaningful page, clearly named
- Always note: "Page X of Y converted" in the summary

### Step 4 — Text extraction priority
When PDF has selectable/copyable text, always use the **exact text** (don't approximate). Use visual analysis only for layout, colors, and spacing.

---

## Description Pipeline

When the user describes the page in words (no code or image).

### Process
1. Ask one clarifying question if the description is very vague: "What type of page? (landing page / portfolio / service page / blog / other)"
2. Map described elements to widgets and containers
3. Use professional defaults for colors (#1A1A2E dark, #FF6B6B accent, #FFFFFF white) unless user specifies
4. Build the JSON from the description
5. Note in the output: "Colors and fonts are defaults — customize in Elementor as needed"

---

## Edit Pipeline

When the user uploads an existing Elementor `.json` file to modify.

### Process
1. Parse the incoming JSON
2. Locate the target element by its `id`, `widgetType`, or text content
3. Apply the requested changes to settings
4. Re-output the complete valid JSON
5. Summarize what changed: "Updated heading text in id `3a4f1b2c`, changed button color in id `9d7e6f10`"

---

## JSON Document Structure

Every Elementor template file:

```json
{
  "title": "Page Title",
  "type": "page",
  "version": "0.4",
  "page_settings": {
    "margin": { "unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true },
    "padding": { "unit": "px", "top": "0", "right": "0", "bottom": "0", "left": "0", "isLinked": true }
  },
  "content": []
}
```

**`type` values:** `page` | `section` | `header` | `footer` | `single` | `archive`

---

## Element Hierarchy

**Modern (Elementor 3.6+) — ALWAYS USE THIS:**
```
Container (isInner: false)
  └── Container (isInner: true)  ← for columns
        └── Widget
```

**Legacy (avoid unless user requires it):**
```
Section → Column → Widget
```

---

## ID Generation Rule

Every element: **unique 8-character hex ID**, never reused.
```
Good: "3a4f1b2c", "9d7e6f10", "a1b2c3d4", "f8e1d2c3"
Bad:  "id1", "widget1", reusing any existing ID
```

---

## Widget Quick Reference

Full JSON examples in `references/widget-map.md`.

| Visual / HTML Input | `widgetType` |
|---|---|
| `<h1>`–`<h6>` / large headline | `heading` |
| `<p>` / body text / rich text | `text-editor` |
| `<img>` / photo | `image` |
| CTA / `<button>` / `<a class=btn>` | `button` |
| YouTube/Vimeo / `<video>` | `video` |
| `<hr>` / separator line | `divider` |
| Gap / whitespace | `spacer` |
| Icon + title + desc card | `icon-box` |
| Bulleted icon list | `icon-list` |
| Photo + title + desc | `image-box` |
| Animated number stat | `counter` |
| Quote / review | `testimonial` |
| FAQ / collapsible | `accordion` |
| Tab nav UI | `tabs` |
| Social media row | `social-icons` |
| Map / location | `google-maps` |
| Plugin shortcode | `shortcode` |
| Alert / notice bar | `alert` |
| Photo grid | `image-gallery` |
| Slider / carousel | `image-carousel` |
| Skill/progress bar | `progress` |
| Custom/complex HTML | `html` |

---

## CSS → Elementor Quick Reference

Full mapping in `references/style-map.md`.

```json
// Spacing
"padding": { "unit": "px", "top": "80", "right": "40", "bottom": "80", "left": "40", "isLinked": false }
"_padding": { ... }   // widget-level (prefix with _)
"_margin":  { ... }   // widget-level

// Typography
"typography_typography": "custom",
"typography_font_family": "Poppins",
"typography_font_size": { "unit": "px", "size": 48 },
"typography_font_size_tablet": { "unit": "px", "size": 32 },
"typography_font_size_mobile": { "unit": "px", "size": 24 },
"typography_font_weight": "700",
"typography_line_height": { "unit": "em", "size": 1.4 },
"typography_letter_spacing": { "unit": "px", "size": -0.5 }

// Background — solid
"background_background": "classic",
"background_color": "#1A1A2E"

// Background — image
"background_background": "classic",
"background_image": { "url": "https://example.com/img.jpg", "id": "" },
"background_size": "cover",
"background_position": "center center",
"background_repeat": "no-repeat"

// Background — gradient
"background_background": "gradient",
"background_gradient_type": "linear",
"background_color": "#FF6B6B",
"background_color_b": "#4ECDC4",
"background_gradient_angle": { "unit": "deg", "size": 135 }

// Border
"border_border": "solid",
"border_width": { "unit": "px", "top": "1", "right": "1", "bottom": "1", "left": "1", "isLinked": true },
"border_color": "#DDDDDD",
"border_radius": { "unit": "px", "top": "8", "right": "8", "bottom": "8", "left": "8", "isLinked": true }

// Box shadow
"box_shadow_box_shadow_type": "yes",
"box_shadow_box_shadow": { "horizontal": 0, "vertical": 4, "blur": 20, "spread": 0, "color": "rgba(0,0,0,0.1)" }
```

---

## JavaScript Strategy

| JS Feature | Elementor Solution |
|---|---|
| CSS transitions / animations | Elementor Entrance Animations (Advanced tab → Motion Effects) |
| Scroll-triggered effects | Elementor Scrolling Effects (Pro) |
| Popup / modal | Elementor Pro Popup Builder (note requirement) |
| Slider / carousel | `image-carousel` widget |
| Counter animation | `counter` widget |
| Typed text effect | `html` widget with typed.js CDN |
| Custom JS | `html` widget with `<script>` tag |
| jQuery plugins | `html` widget or `shortcode` widget |
| Parallax | Container → Motion Effects → Parallax (Pro) |
| Sticky nav | `custom_css` + `position: sticky` |

```json
// JS embedded in html widget
{
  "id": "js1a2b3c",
  "elType": "widget",
  "widgetType": "html",
  "settings": {
    "html": "<div id='typed-target'></div>\n<script src='https://cdn.jsdelivr.net/npm/typed.js@2.0.12'></script>\n<script>new Typed('#typed-target', { strings: ['Developer', 'Designer'], typeSpeed: 60 });</script>"
  },
  "elements": []
}
```

---

## Responsive Settings

Append `_tablet` or `_mobile` to any setting key:

```json
"typography_font_size": { "unit": "px", "size": 48 },
"typography_font_size_tablet": { "unit": "px", "size": 36 },
"typography_font_size_mobile": { "unit": "px", "size": 24 },
"padding": { "unit": "px", "top": "80", "right": "60", "bottom": "80", "left": "60", "isLinked": false },
"padding_tablet": { "unit": "px", "top": "50", "right": "30", "bottom": "50", "left": "30", "isLinked": false },
"padding_mobile": { "unit": "px", "top": "40", "right": "20", "bottom": "40", "left": "20", "isLinked": false },
"width": { "unit": "%", "size": 33 },
"width_tablet": { "unit": "%", "size": 50 },
"width_mobile": { "unit": "%", "size": 100 }
```

---

## Custom CSS

```json
"custom_css": "selector { clip-path: polygon(0 0, 100% 0, 100% 88%, 0 100%); }\nselector:hover { transform: translateY(-5px); }"
```
`selector` = Elementor's placeholder. Requires Elementor Pro. For free Elementor, use `html` widget with `<style>`.

---

## Direct WordPress Publishing (WordPress MCP)

If the WordPress MCP connector is available, offer to publish directly:

### Check if WordPress MCP is connected:
- Look for WordPress MCP in available tools
- If connected: ask user "Should I also publish this directly to your WordPress site?"

### Publish flow via WordPress MCP:
1. Create a new page/post via the MCP
2. Set `_elementor_edit_mode` = `builder`
3. Set `_elementor_data` = the JSON content (stringified)
4. Set page status to `draft` (never publish without user confirmation)
5. Report back: "Draft page created at [URL]. Open in WordPress to review."

### WP-CLI alternative:
```bash
wp post create --post_type=page --post_status=draft --post_title="Page Title"
wp post meta update <post_id> _elementor_data '<json_here>'
wp post meta update <post_id> _elementor_edit_mode builder
wp post meta update <post_id> _wp_page_template elementor_canvas
```

---

## Output Checklist

Before delivering the final JSON, verify:

- [ ] Every element has a **unique 8-char hex `id`**
- [ ] Every element has `"elements": []` (never omit this)
- [ ] `isInner: true` set on all nested/child containers
- [ ] All text content populated (no "Lorem ipsum" unless user asked)
- [ ] Colors are hex values (no color names like "blue")
- [ ] Placeholder images use `https://placehold.co/` URLs
- [ ] Responsive `_tablet` / `_mobile` settings on font sizes and padding
- [ ] JSON is valid (no trailing commas, balanced braces)
- [ ] File saved as `[page-name]-elementor.json`
- [ ] Import instructions included in reply

---

## Import Instructions (Always Include)

> **Method 1 — Template Library:**
> 1. WordPress Dashboard → **Elementor → Templates → Saved Templates**
> 2. Click the **Import** icon (cloud/upload button)
> 3. Select the `.json` file → Import
> 4. Click **Insert** to add to any page
>
> **Method 2 — Page Editor:**
> 1. Edit any page with Elementor
> 2. Click the **folder icon** (bottom center panel)
> 3. Go to **My Templates** → Import → select `.json`
>
> **Method 3 — WP-CLI (advanced):**
> ```bash
> wp post create --post_type=elementor_library --post_status=publish --post_title="My Template"
> wp post meta update <id> _elementor_data "$(cat template.json)"
> wp post meta update <id> _elementor_edit_mode builder
> ```

---

## Common Pitfalls — Never Do These

| ❌ Wrong | ✅ Correct |
|---|---|
| Omit `"elements": []` on any element | Always include `"elements": []` |
| Reuse element IDs | Each ID must be unique 8-char hex |
| Use color names (`"red"`, `"blue"`) | Always use hex (`"#FF0000"`) |
| Forget `isInner: true` on child containers | Set `isInner: true` for nested containers |
| Use CSS Grid in container settings | CSS Grid → `html` widget with `<style>` |
| Put absolute font size only (no responsive) | Always add `_tablet` and `_mobile` variants |
| Embed private/local image paths | Use absolute URLs or `https://placehold.co/` |
| Use Elementor Pro features without noting it | Flag Pro features: ⚠️ Requires Elementor Pro |

---

## Elementor Free vs Pro Feature Flags

When generating JSON with Pro features, add this comment in your response (not in JSON):

```
⚠️ Pro Features Used:
- Custom CSS (custom_css setting) → Requires Elementor Pro
- Popup Builder → Requires Elementor Pro
- Scrolling Effects → Requires Elementor Pro
- Theme Builder (header/footer type) → Requires Elementor Pro
- Form Widget → Requires Elementor Pro
Free alternative: Use the html widget with inline <style> instead of custom_css.
```

---

## Reference Files

- `references/widget-map.md` — Full JSON settings for every Elementor widget (with visual triggers)
- `references/style-map.md` — CSS property → Elementor JSON settings lookup table
- `references/visual-analysis.md` — Deep guide for analyzing images/PDFs/screenshots into Elementor
- `references/section-patterns.md` — Pre-built JSON patterns for common page sections

---

## About This Skill

**AK Elementor Studio** is crafted by **Abanoub Khalil**, Senior WordPress & Webflow Developer based in Cairo, Egypt.

- 🌐 Portfolio: [akstudio.me](https://akstudio.me)
- 💼 7+ years building WordPress & Webflow sites
- 🏢 Currently at InVitro Capital — managing AllCare.ai, AllRx.ai & more
- 🔧 13 live freelance projects across Egypt and the MENA region

*Built with expertise in Elementor, WordPress architecture, SEO optimization, and modern web development workflows.*
