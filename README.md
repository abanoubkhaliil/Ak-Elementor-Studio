[![AK Elementor Studio Banner](https://github.com/abanoubkhaliil/Ak-Elementor-Studio/raw/main/banner.svg)](https://akstudio.me)

[![Portfolio](https://img.shields.io/badge/Portfolio-akstudio.me-f78166?style=flat-square&logo=firefox&logoColor=white)](https://akstudio.me)
[![License](https://img.shields.io/badge/License-MIT-56d364?style=flat-square)](https://github.com/abanoubkhaliil/Ak-Elementor-Studio/blob/main/LICENSE)
[![Claude Skill](https://img.shields.io/badge/Claude_Skill-Compatible-79c0ff?style=flat-square&logo=anthropic&logoColor=white)](https://docs.anthropic.com)
[![Elementor](https://img.shields.io/badge/Elementor-Free_%26_Pro-d2a8ff?style=flat-square)](https://elementor.com)
[![WordPress](https://img.shields.io/badge/WordPress-6.x-ffa657?style=flat-square&logo=wordpress&logoColor=white)](https://wordpress.org)

### Convert any design into a production-ready Elementor Template Kit — in minutes, not hours.

---

## What Is This?

**AK Elementor Studio** is a [Claude AI skill](https://docs.anthropic.com) that gives Claude the expert knowledge needed to convert *any* design input into a fully importable **Elementor Pro Template Kit ZIP** — the same Envato-style format used by professional template marketplaces.

It follows the same structured pipeline a senior Elementor developer uses: detect input → analyze layout → extract styles → map widgets → generate responsive JSON → assemble kit ZIP → validate output. But instead of taking hours, it takes minutes.

**What's new in v2:** The skill now produces complete **multi-page Template Kit ZIPs** (not just individual page JSON files), includes a full Python builder for automated generation, enforces stricter JSON rules, and covers 35+ native widgets including the full Pro widget set.

Built and maintained by **Abanoub Khalil**, Senior WordPress & Webflow Developer with 7+ years of experience at [akstudio.me](https://akstudio.me).

---

## Supported Input Types

| Input | What You Provide | Example |
|---|---|---|
| 📸 **Screenshot / Mockup** | Any PNG, JPG, WebP of a webpage or UI | Competitor page, inspiration screenshot |
| 🎨 **Figma Export** | Figma design or component export | Client's Figma file |
| 💻 **HTML / CSS / JS** | Code pasted or uploaded as files | Existing site code, a template you bought |
| 📄 **PDF** | Single or multi-page design document | Style guides, wireframe docs |
| ✏️ **Text Description** | Natural language description of the page | "A dark SaaS hero with 3-column features grid" |
| 🔧 **Existing JSON** | An Elementor `.json` template to modify | Edit or extend templates you already have |

---

## What Gets Generated

Every run produces a complete **Envato-style Template Kit ZIP**:

```
kit-name.zip
├── kit.json                     ← Kit metadata + template list
└── templates/
    ├── global-styles.json       ← Brand colors + typography (import FIRST)
    ├── header.json              ← Sticky header (Pro)
    ├── footer.json              ← Footer template (Pro)
    ├── home.json
    ├── about.json
    ├── services.json
    ├── pricing.json
    └── contact.json
```

Each page template includes:
- Navbar → Hero → Features → Stats → Testimonials → Pricing → CTA → Footer
- Full responsive breakpoints on every element (desktop + tablet + mobile)
- Unique 8-character hex IDs on every element
- Placeholder images from `placehold.co`
- Global brand colors and typography from `global-styles.json`

---

## How It Works

```
Your Input → Detection → Analysis → Widget Mapping → JSON Build → Kit ZIP → Import to WP
```

**Step 1 — Pre-flight Interview:** Claude asks 8 quick questions: kit name, pages, brand colors, fonts, Free vs Pro, CPTs, dark/light theme, and industry.

**Step 2 — Input Analysis:**
- **Images/PDFs** → extracts dominant colors via quantization, detects section boundaries by brightness bands
- **HTML/CSS** → parses all hex values, font-family declarations, structural tags
- **Text** → infers brand palette, section order, and copy tone from industry/niche

**Step 3 — Widget Mapping:** Every visual element is mapped to the correct native Elementor widget. Zero `html` widgets for visual content — the `html` widget is reserved exclusively for `<script>` JS injection.

**Step 4 — JSON Assembly:** Builds section containers, inner containers, and widgets with all required keys: `id`, `elType`, `isInner`, `settings`, `elements`. Enforces all JSON rules (no comments, no trailing commas, hex/rgba colors only).

**Step 5 — Kit ZIP:** Assembles `kit.json`, `global-styles.json`, `header.json`, `footer.json`, and one `.json` per page into the Envato-compatible ZIP structure.

---

## Installation

### Prerequisites

- Claude (claude.ai or Claude Code)
- A WordPress site with Elementor (Free or Pro)

### Install the Skill

**Option A — Clone:**
```bash
git clone https://github.com/abanoubkhaliil/Ak-Elementor-Studio.git
```
Place the folder in your Claude skills directory (e.g. `/mnt/skills/user/`).

**Option B — Manual download:**
1. Click **Code → Download ZIP**
2. Extract and place the folder in your Claude skills directory

Once the `SKILL.md` is in place, Claude automatically detects and uses it whenever you mention Elementor, WordPress, template kit, or upload a design file.

---

## Python Builder (Optional — Computer Use / Claude Code)

The repo includes a production-ready Python CLI builder for automated kit generation:

### Install dependencies
```bash
pip install Pillow PyMuPDF beautifulsoup4 lxml
```

### Generate a kit from any input

```bash
# From an image
python main.py --input design.png --kit-name "SaaS Kit" --pages home,about,pricing,contact --output ./output/

# From a PDF
python main.py --input mockup.pdf --kit-name "Agency Kit" --pages home,services,portfolio,contact --output ./output/

# From HTML + CSS + JS
python main.py --input index.html --css style.css --js main.js --kit-name "Corporate Kit" --output ./output/

# From a text description
python main.py --description "Dark SaaS landing page with hero, features, and pricing" --kit-name "Dark SaaS" --output ./output/

# Elementor Free mode (no Pro widgets)
python main.py --input design.png --kit-name "My Kit" --free --output ./output/
```

### Run tests
```bash
python tests/test_kit_builder.py
# Expected: 11 passed, 0 failed
```

### Builder modules
```
builder/
  analyzer.py        ← Analyzes image/PDF/HTML/description → DesignSpec
  widget_factory.py  ← Factory functions for 35+ native Elementor widgets
  json_builder.py    ← Section + page assemblers, tree validation
  kit_assembler.py   ← Assembles the Envato-compatible ZIP
main.py              ← CLI entry point
```

---

## Usage Examples

Just describe what you want — Claude handles the rest:

```
"Convert this screenshot into an Elementor Template Kit ZIP"

"Build this Figma export as a full WordPress template kit with home, about, pricing, contact"

"Turn this HTML/CSS into an Elementor kit — dark theme, Inter font, 5 pages"

"Create a SaaS landing kit: dark hero with gradient, 3-column features, pricing table, CTA banner"

"Edit this existing template and change the hero background to #1A1A2E"
```

---

## Importing to WordPress

### Method A — Template Kit ZIP *(Recommended, Pro)*

**Prerequisites:**
- Hello Elementor theme active
- Elementor Pro installed and active
- Elementor → Settings → Features → Flexbox Container → **Active**
- WordPress → Settings → Permalinks → Post Name → Save

**Steps:**
1. WordPress → **Tools → Template Kit → Upload Template Kit ZIP File**
2. Click **"Import Global Kit Styles" FIRST** (sets brand colors + fonts site-wide)
3. Import Header, then Footer, then each page template in order
4. Pages → Edit with Elementor → folder icon → **View Installed Kit** → Insert Template

### Method B — Individual JSON Files *(Free + Pro)*

1. WordPress → **Elementor → Templates → Saved Templates**
2. Click the Import icon (top right)
3. Select a `.json` file from inside the ZIP → **Import Now**
4. Open any page in Elementor → folder icon → **My Templates** → Insert

---

## What's in This Repo

```
Ak-Elementor-Studio/
├── SKILL.md                        ← Main skill file (Claude reads this)
├── main.py                         ← Python CLI entry point
├── builder/
│   ├── analyzer.py                 ← Input analyzer (image/PDF/HTML/description)
│   ├── widget_factory.py           ← 35+ widget JSON factories
│   ├── json_builder.py             ← Section builders + page assembler + validator
│   └── kit_assembler.py            ← ZIP kit builder
├── templates/
│   └── section_patterns/
│       ├── hero.json               ← Full-screen 2-col hero pattern
│       ├── navbar.json             ← Sticky Pro navbar pattern
│       ├── features_grid.json      ← 3-col icon-box card grid
│       └── cta_banner.json         ← Gradient CTA section
├── tests/
│   └── test_kit_builder.py         ← 11-test suite (IDs, JSON, guards, ZIP)
├── banner.svg
├── README.md
├── CONTRIBUTING.md
└── LICENSE
```

---

## Widget Coverage

### Free Widgets (21)
`heading` · `text-editor` · `button` · `image` · `icon-box` · `icon-list` · `image-box` · `testimonial` · `counter` · `progress` · `divider` · `spacer` · `accordion` · `tabs` · `social-icons` · `video` · `image-carousel` · `image-gallery` · `google-maps` · `shortcode` · `alert`

### Pro Widgets (15)
`posts` · `portfolio` · `slides` · `price-list` · `price-table` · `flip-box` · `call-to-action` · `countdown` · `share-buttons` · `nav-menu` · `site-logo` · `site-title` · `page-title` · `post-content` · `form`

---

## Non-Negotiable Rules (Built Into the Skill)

| Rule | Detail |
|---|---|
| `html` widget = `<script>` only | Never used for visual content |
| Unique 8-char hex IDs | `secrets.token_hex(4)` — never duplicated |
| No JSON comments | `/* */` and `//` break Elementor's parser |
| No trailing commas | Parser fails silently |
| Hex or `rgba()` colors only | Never named colors (`white`, `transparent`, etc.) |
| `"elements": []` on every node | Required even on widgets |
| `isInner` rules enforced | Top-level = `false`, nested containers = `true` |
| Responsive on everything | `_tablet` + `_mobile` suffixes on fonts, padding, widths |

---

## Elementor Free vs. Pro

The skill generates Free-compatible output by default. When Pro features are used, the response clearly flags them:

```
⚠️ Pro features used: sticky nav, site-logo widget, form widget, header/footer templates
✅ Free alternatives: icon-list for nav, image widget for logo, shortcode for forms
```

---

## Post-Import Checklist

After importing any kit, remember to:
- Replace all `placehold.co` images with real images
- Update nav links to actual page URLs
- Set the site logo in Elementor → Site Settings → Site Identity
- Update the email in the form widget or shortcode
- Replace `© 2025 YourBrand` in the footer
- Assign header/footer in Elementor Pro → Theme Builder

---

## About the Author

**Abanoub Khalil** — Senior WordPress & Webflow Developer

| | |
|---|---|
| 🌐 Portfolio | [akstudio.me](https://akstudio.me) |
| 📍 Location | Cairo, Egypt |
| 💼 Experience | 7+ years WordPress & Webflow |
| 🏢 Current | InVitro Capital — AllCare.ai, AllRx.ai |

---

## License

MIT — free to use, modify, and share. See [LICENSE](https://github.com/abanoubkhaliil/Ak-Elementor-Studio/blob/main/LICENSE) for details.

---

## Contributing

Issues and pull requests are welcome. If you find a widget mapping that's missing, a section pattern to add, or a JSON bug — please open an issue.

---

If this skill saves you time, a ⭐ on the repo is always appreciated.  
[akstudio.me](https://akstudio.me) · Built with 7+ years of Elementor expertise
