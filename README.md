# mark2view-themes

The official community theme repository for Mark2View. A distraction-free Markdown editor.

Themes are browsable and installable directly from within the app via **Settings -> Appearance -> Theme Market**.

## Available Themes

| Theme | Variant | Key technique |
|---|---|---|
| [Nord](themes/nord/nord.css) | Dark | Cool blue palette, all variables defined |
| [Tokyo Night](themes/tokyo-night/tokyo-night.css) | Dark | Deep purple + electric accents |
| [Dracula](themes/dracula/dracula.css) | Dark | Vibrant per-element heading colours |
| [Solarized Dark](themes/solarized-dark/solarized-dark.css) | Dark | Warm amber tones, careful contrast |
| [GitHub Light](themes/github-light/github-light.css) | Light | Clean professional, matches GitHub |
| [Paper](themes/paper/paper.css) | Light | Custom serif font-family, warm parchment |

## Installing a Theme

1. Open **Mark2View**
2. Press `Ctrl+,` to open Settings
3. Navigate to the **Appearance** tab
4. Click **Open Theme Market**
5. Browse, click **Install**, then **Apply**

## Creating & Submitting a Theme

Read the full authoring guide in **[CONTRIBUTING.md](CONTRIBUTING.md)**.

The short version:

1. Fork this repo
2. Create `themes/your-theme-name/your-theme-name.css`
3. Add an entry to `themes/index.json`
4. Test locally in Mark2View
5. Open a Pull Request

## Repository Structure

```
mark2view-themes/
├── themes/
│   ├── index.json          < Marketplace catalog (fetched by the app)
│   ├── nord/
│   │   └── nord.css
│   ├── tokyo-night/
│   │   └── tokyo-night.css
│   └── ...
├── CONTRIBUTING.md         < Theme authoring guide
└── README.md
```

## License

All themes in this repository are released under the [MIT License](LICENSE).
