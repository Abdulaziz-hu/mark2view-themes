# Contributing to mark2view-themes

Welcome to the **Mark2View Theme Repository**. The centralized marketplace for community-built themes. This guide covers everything: how to author a theme from scratch, submit it for review, report issues, and get your theme listed in the in-app marketplace.

## Table of Contents

1. [Quick Start](#1-quick-start)
2. [How Themes Work](#2-how-themes-work)
3. [Theme File Structure](#3-theme-file-structure)
4. [CSS Variable Reference](#4-css-variable-reference)
5. [ProseMirror Content Overrides](#5-prosemirror-content-overrides)
6. [The index.json Catalog Entry](#6-the-indexjson-catalog-entry)
7. [Preview Colors](#7-preview-colors)
8. [Theme Checklist](#8-theme-checklist)
9. [Submitting a Pull Request](#9-submitting-a-pull-request)
10. [Review Process](#10-review-process)
11. [Reporting Issues](#11-reporting-issues)
12. [Guidelines & Rules](#12-guidelines--rules)
13. [Theme Examples](#13-theme-examples)

## 1. Quick Start

```bash
# 1. Fork this repository on GitHub, then clone your fork
git clone https://github.com/Abdulaziz-hu/mark2view-themes.git
cd mark2view-themes

# 2. Create your theme folder
mkdir themes/my-theme-name

# 3. Create the two required files
touch themes/my-theme-name/my-theme-name.css
# (you will also add an entry to themes/index.json)

# 4. Build and test locally (see Section 2)

# 5. Open a Pull Request against main
```

## 2. How Themes Work

Mark2View themes are **pure CSS files** that override a set of CSS custom properties (variables). The application uses these variables everywhere. Such as backgrounds, text, borders, accents, editor syntax colours. So a single well-crafted CSS file can completely transform the look of the app.

When a user installs a theme from the marketplace:

1. The app fetches your `*.css` file from this repository via the GitHub raw CDN.
2. The CSS is validated and sanitised (no `<script>` tags, no `javascript:` URLs, no `@import`).
3. It is saved to the user's local `userData/themes/` directory.
4. A `<style>` tag is injected into `<head>` with your CSS, overriding the default variables.

Themes do **not** run any JavaScript. They cannot access the filesystem or make network requests. They are sandboxed to visual customisation only.

## 3. Theme File Structure

Every theme lives in its own folder inside `themes/`:

```
themes/
└── my-theme-name/
    └── my-theme-name.css      < required: the theme stylesheet
```

The folder name and CSS filename **must match** and must be:
- All lowercase
- Hyphens only (no underscores, no spaces)
- Unique across the repository

You also need to add one entry to `themes/index.json`.

## 4. CSS Variable Reference

Your theme overrides CSS variables inside a `:root` block (which applies to both dark and light system preferences). Here is the full variable reference with descriptions:

```css
/*
 * My Theme for Mark2View
 * Author: Your Name
 * Version: 1.0.0
 * Variant: dark | light
 */

:root,
[data-theme="dark"],
[data-theme="light"] {

  /* ──────────────────────────────────────────────────────────────
   * BACKGROUNDS
   * ────────────────────────────────────────────────────────────── */

  --bg-app:       #1a1a2e;   /* Main application background                  */
  --bg-sidebar:   #16213e;   /* Sidebar / file explorer background            */
  --bg-editor:    #1a1a2e;   /* Editor canvas background                      */
  --bg-titlebar:  #0f3460;   /* Top title bar background                      */
  --bg-panel:     #1f2b47;   /* Panels, code blocks, blockquotes              */
  --bg-input:     #1f2b47;   /* Input fields, selects, tag backgrounds        */
  --bg-hover:     #253356;   /* Hover state backgrounds                       */
  --bg-active:    #2b3d66;   /* Active / selected item backgrounds            */
  --bg-modal:     #1f2b47;   /* Modal / dialog backgrounds                    */
  --bg-overlay:   rgba(0, 0, 0, 0.7);   /* Modal backdrop scrim           */
  --bg-toolbar:   rgba(22, 33, 62, 0.96); /* Floating toolbar background   */
  --bg-scrollbar: #1f2b47;   /* Scrollbar track                               */
  --bg-scrollbar-thumb: #2b3d66; /* Scrollbar thumb                          */

  /* ──────────────────────────────────────────────────────────────
   * BORDERS
   * ────────────────────────────────────────────────────────────── */

  --border:        #2b3d66;  /* Standard border colour                        */
  --border-subtle: #1f2b47;  /* Very subtle dividers                          */
  --border-focus:  #e94560;  /* Focused input / active highlight border       */

  /* ──────────────────────────────────────────────────────────────
   * TEXT
   * ────────────────────────────────────────────────────────────── */

  --text-primary:   #eaeaea; /* Main body text                                */
  --text-secondary: #a8b2d8; /* Secondary / de-emphasised text                */
  --text-muted:     #495670; /* Muted hints, placeholders, metadata           */
  --text-accent:    #e94560; /* Accent-coloured text (links active, highlights)*/
  --text-link:      #53d8fb; /* Hyperlink colour                              */
  --text-heading:   #ffffff; /* Heading text colour                           */

  /* ──────────────────────────────────────────────────────────────
   * ACCENT - The brand colour used for interactive elements
   * ────────────────────────────────────────────────────────────── */

  --accent:       #e94560;   /* Primary accent (buttons, active states)       */
  --accent-hover: #f25570;   /* Accent on hover                               */
  --accent-dim:   rgba(233, 69, 96, 0.14); /* Low-opacity accent fill         */

  /* ──────────────────────────────────────────────────────────────
   * STATUS COLOURS
   * ────────────────────────────────────────────────────────────── */

  --danger:       #ff5555;   /* Error states, delete buttons                  */
  --danger-hover: #ff6e6e;
  --warning:      #ffb86c;   /* Warning messages                              */

  /* ──────────────────────────────────────────────────────────────
   * SHADOWS - Match intensity to your background darkness
   * ────────────────────────────────────────────────────────────── */

  --shadow-sm:      0 1px 3px rgba(0,0,0,0.4);
  --shadow-md:      0 4px 16px rgba(0,0,0,0.5);
  --shadow-lg:      0 8px 32px rgba(0,0,0,0.6);
  --shadow-toolbar: 0 4px 20px rgba(0,0,0,0.55);

  /* ──────────────────────────────────────────────────────────────
   * EDITOR SYNTAX - ProseMirror inline element colours
   * ────────────────────────────────────────────────────────────── */

  --syntax-heading: #53d8fb;  /* Heading text colour in the editor            */
  --syntax-bold:    #eaeaea;  /* Bold text colour                             */
  --syntax-italic:  #a8b2d8;  /* Italic text colour                           */
  --syntax-code:    #a6e22e;  /* Inline code text colour                      */
  --syntax-link:    #53d8fb;  /* Link text colour                             */
  --syntax-quote:   #6272a4;  /* Blockquote text colour                       */
  --syntax-hr:      #2b3d66;  /* Horizontal rule colour                       */

  /* ──────────────────────────────────────────────────────────────
   * TYPOGRAPHY - Optional overrides
   * Leave commented out to use the user's current font settings
   * ────────────────────────────────────────────────────────────── */

  /* --font-sans:  'Inter', sans-serif;       */
  /* --font-serif: 'Merriweather', serif;     */
  /* --font-mono:  'JetBrains Mono', monospace; */
}
```

### Tips

- **You do not need to define every variable.** Only override what your theme changes. Undefined variables fall back to the built-in defaults.
- For **light themes**, set lighter backgrounds and darker text. Invert shadow opacities to smaller values (e.g. `rgba(0,0,0,0.08)`).
- Keep `--bg-overlay` semi-transparent so modal backdrops look natural.
- `--accent-dim` should be a very low-opacity version of `--accent` — used for active state backgrounds.

## 5. ProseMirror Content Overrides

For fine-grained control over the writing canvas, you can add element-specific overrides **after** the `:root` block:

```css
/* ── Editor content overrides ───────────────────────────────── */

.ProseMirror h1 { color: #53d8fb; }
.ProseMirror h2 { color: #e94560; }
.ProseMirror h3 { color: #a6e22e; }

.ProseMirror strong { color: #ffffff; }
.ProseMirror em     { color: #a8b2d8; font-style: italic; }

.ProseMirror code {
  background: rgba(166, 226, 46, 0.1);
  color: #a6e22e;
  border-color: rgba(166, 226, 46, 0.2);
}

.ProseMirror a { color: #53d8fb; }

.ProseMirror blockquote {
  border-left-color: #2b3d66;
  color: #6272a4;
}

.ProseMirror pre {
  background: #0f1626;
  border-color: #2b3d66;
}

/* Tables */
.ProseMirror th { background: #1f2b47; color: #eaeaea; }
.ProseMirror td, .ProseMirror th { border-color: #2b3d66; }

/* Horizontal rule */
.ProseMirror hr { border-top-color: #2b3d66; }
```

Use the browser DevTools (in dev mode, the app opens DevTools automatically) to inspect elements and fine-tune styles.

## 6. The index.json Catalog Entry

Add your theme to `themes/index.json`. This is what the in-app marketplace reads:

```json
{
  "id": "my-theme-name",
  "name": "My Theme Name",
  "description": "A short, compelling description. One or two sentences max.",
  "author": "your-github-username",
  "version": "1.0.0",
  "variant": "dark",
  "tags": ["dark", "minimal", "blue"],
  "cssUrl": "https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/mark2view-themes/main/themes/my-theme-name/my-theme-name.css",
  "preview": {
    "background": "#1a1a2e",
    "heading": "#53d8fb",
    "text": "#eaeaea",
    "accent": "#e94560",
    "code": "#a6e22e"
  }
}
```

### Field reference

| Field | Required | Description |
|---|---|---|
| `id` | ✓ | Unique slug. Must match your folder/filename. Lowercase, hyphens only. |
| `name` | ✓ | Display name shown in the marketplace. |
| `description` | ✓ | Short description. Max 120 characters. |
| `author` | ✓ | Your GitHub username. |
| `version` | ✓ | Semantic version string (`MAJOR.MINOR.PATCH`). |
| `variant` | ✓ | `"dark"` or `"light"`. Used for filtering. |
| `tags` | — | Array of lowercase strings. Helps search and discovery. |
| `cssUrl` | ✓ | Full raw GitHub URL to your CSS file. Replace `YOUR_GITHUB_USERNAME` with the repo owner. |
| `preview` | ✓ | Five hex colours used to render the card swatch in the marketplace. |

## 7. Preview Colors

The `preview` object drives the card swatch shown in the marketplace before a user installs your theme. Choose colours that best represent your palette:

```json
"preview": {
  "background": "#1a1a2e",   < Card background fill
  "heading":    "#53d8fb",   < Thick heading line
  "text":       "#eaeaea",   < Two body text lines
  "accent":     "#e94560",   < Accent line (thin, below body)
  "code":       "#a6e22e"    < Short code line (dashed appearance)
}
```

All values must be valid hex colour strings (`#rrggbb` or `#rgb`).

## 8. Theme Checklist

Before opening your PR, verify each item:

- [ ] Folder name matches CSS filename: `themes/my-theme/my-theme.css`
- [ ] CSS uses `:root, [data-theme="dark"], [data-theme="light"]` as the selector
- [ ] No `<script>` tags anywhere in the CSS file
- [ ] No `javascript:` URL references
- [ ] No `@import` rules (fonts must be referenced via CSS `font-family` only with web-safe fallbacks, or the user's font stack)
- [ ] No `expression()` or other IE-era CSS injection vectors
- [ ] `index.json` entry added with all required fields
- [ ] `cssUrl` uses the correct raw GitHub URL pattern
- [ ] `variant` is correctly set to `"dark"` or `"light"`
- [ ] `version` starts at `"1.0.0"`
- [ ] `preview` colours accurately represent the theme's appearance
- [ ] Description is under 120 characters
- [ ] Theme has been visually tested in the app (see Testing section below)

## 9. Submitting a Pull Request

### Step 1 (Fork & clone)

```bash
git clone https://github.com/Abdulaziz-hu/mark2view-themes.git
cd mark2view-themes
git checkout -b theme/my-theme-name
```

### Step 2 (Create your files)

```
themes/
└── my-theme-name/
    └── my-theme-name.css
```

Edit `themes/index.json` and add your entry at the end of the array (before the closing `]`).

### Step 3 (Test locally)

In your Mark2View installation, open the `userData` directory:

- **Windows:** `%APPDATA%\Mark2View\`
- **Linux:** `~/.config/Mark2View/`
- **macOS:** `~/Library/Application Support/Mark2View/`

Create a folder called `themes/` and place your CSS file there. Then manually add an entry to `installed-themes.json` in the same directory:

```json
[
  {
    "id": "my-theme-name",
    "name": "My Theme",
    "cssPath": "/path/to/userData/themes/my-theme-name.css",
    "meta": { "variant": "dark", "author": "you" },
    "installedAt": "2025-01-01T00:00:00.000Z"
  }
]
```

Restart Mark2View. Go to **Settings -> Appearance** and look for your theme in the installed list, or open the Theme Market and switch to the **Installed** tab.

### Step 4 (Open the PR)

Push your branch and open a Pull Request against `main`. Use this title format (based on your theme):

```
[New Theme] My Theme Name - dark/light
OR
[Update Theme] My Theme Name - dark/light
OR
[Fix Theme] My Theme Name - dark/light
```

In the PR description, include:
- A short description of the theme's aesthetic
- At least one screenshot of the app with your theme applied

## 10. Review Process

All submitted themes go through a lightweight review:

1. **Manual review**, a maintainer will:
  - The CSS file contains no disallowed content (scripts, JS URLs, `@import`)
  - The `index.json` entry is valid JSON and has all required fields
  - The `cssUrl` correctly points to the new file
  - File naming conventions are followed
  - Install and visually inspect the theme in the app
  - Check contrast ratios for basic readability (WCAG AA for text on background)
  - Verify the theme is meaningfully distinct from existing entries
  - Leave feedback or approve

2. **Merge**, once approved, the PR is merged to `main`. The theme immediately appears in the marketplace for all users on their next marketplace refresh.

**Timeline:** We aim to review PRs within 7 days. Popular or well-presented themes get priority.


## 11. Reporting Issues

### Bug in an existing theme

Open an issue with the title: `[Theme Bug] Theme Name - Short Description.`

Include:
- The theme name and ID
- What looks wrong (screenshot preferred)
- Your OS and Mark2View version

### Requesting a theme

Open an issue with: `[Theme Request] Theme Name`

Describe the aesthetic you're looking for. The community may pick it up.

### Security issue in a theme

If you find that a theme contains malicious CSS (e.g., a `javascript:` URL that slipped through), **do not open a public issue**. Email the maintainers directly or open a private security advisory on GitHub.

### App bug (not theme-related)

For bugs in Mark2View itself, open an issue in the [mark2view main repository](https://github.com/Abdulaziz-hu/mark2view), not here.

## 12. Guidelines & Rules

### ✓ Allowed

- Any colour palette, aesthetic, or style
- Overriding font families (using system fonts or Google Fonts via CSS `font-family` with fallbacks)
- Targeting Mark2View-specific classes like `.ProseMirror`, `.editor-canvas`, `.titlebar`
- Subtle animations on non-text elements (e.g., border transitions)

### ✗ Not allowed

- Any JavaScript. including event handlers in CSS like `expression()`
- `@import` rules of any kind
- `javascript:` pseudo-URLs
- External resource loading other than fonts declared via `font-family`
- Themes that are near-identical copies of existing themes
- Offensive, hateful, or inappropriate theme names or descriptions
- Themes that deliberately break the UI (hiding critical buttons, zero-opacity text, etc.)

Violations result in immediate PR rejection and, if already merged, removal.

## 13. Theme Examples

Here are some themes (we made) already in the marketplace to learn from:

| Theme | Variant | Key technique |
|---|---|---|
| [Nord](themes/nord/nord.css) | Dark | Cool blue palette, all variables defined |
| [Tokyo Night](themes/tokyo-night/tokyo-night.css) | Dark | Deep purple + electric accents |
| [Dracula](themes/dracula/dracula.css) | Dark | Vibrant per-element heading colours |
| [Solarized Dark](themes/solarized-dark/solarized-dark.css) | Dark | Warm amber tones, careful contrast |
| [GitHub Light](themes/github-light/github-light.css) | Light | Clean professional, matches GitHub |
| [Paper](themes/paper/paper.css) | Light | Custom serif font-family, warm parchment |

## Thank you

Every theme makes Mark2View better for every writer who uses it. We appreciate the time you put into crafting something beautiful.

If you have questions not answered here, open a discussion in the **Discussions** tab on GitHub.

Happy theming!