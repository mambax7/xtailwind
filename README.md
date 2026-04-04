xTailwind
=========

A Tailwind CSS + DaisyUI theme for [XOOPS 2.5.12](https://xoops.org). Proof of concept sibling to xSwatch5 for site owners who prefer a utility-first CSS framework.

**Status: Proof of concept.** Use for evaluation. For production sites, see [xSwatch5](../xswatch5/).

## Why a Tailwind theme?

Tailwind takes a fundamentally different approach from Bootstrap:

- **Utility-first** — compose designs from small single-purpose classes (`flex`, `px-4`, `bg-primary`) rather than pre-styled components
- **Build step** — a compiler scans your templates and ships only the classes you actually use, producing a smaller final CSS
- **DaisyUI** bridges the gap by adding component classes (`.btn`, `.card`, `.navbar`, `.modal`) on top of Tailwind, plus 34 pre-built themes switchable via a single `data-theme` attribute

xTailwind pairs Tailwind with DaisyUI so you get the Bootswatch-style theme picker experience (34 themes out of the box) with Tailwind's smaller payload and modern tooling.

## Features

- **34 DaisyUI themes** — light, dark, cupcake, cyberpunk, synthwave, dracula, and 28 more, all switchable from the navbar
- **Live theme switcher** with localStorage persistence and FOUC-free restoration
- **Light/dark toggle** that respects OS preference on first visit
- **Alpine.js** (~15 KB) for interactive components — dropdowns, mobile nav, toasts, modals (loaded from the shared `xoops_lib/Frameworks/alpine/` location so multiple Tailwind themes share a single copy)
- **Built-in RTL support** via Tailwind logical properties
- **WCAG AA** baseline from DaisyUI's color system

## Build

xTailwind requires a one-time CSS build step. You don't need to rebuild unless you edit templates or customize the Tailwind config.

### Option A — Tailwind CLI standalone (no Node.js)

Download the Tailwind standalone binary from the [Tailwind CLI releases](https://github.com/tailwindlabs/tailwindcss/releases/latest) for your platform. DaisyUI is **not** available in the standalone binary, so this path gives you Tailwind only (no component classes or pre-built themes).

### Option B — npm (recommended, enables DaisyUI)

```bash
cd themes/xtailwind
npm install
npm run build
```

This compiles `css/input.css` → `css/styles.css` with Tailwind + DaisyUI and all enabled themes.

For development with auto-rebuild on template changes:
```bash
npm run watch
```

## Installation

1. Copy the `xtailwind/` directory to `themes/` in your XOOPS installation
2. Run `npm install && npm run build` (see Build section)
3. In XOOPS admin → System → Preferences → General Settings → select `xtailwind` as the theme

## Theme Switcher

The navbar includes:

**Dark/Light toggle** — switches to a sensible light or dark DaisyUI theme. The opposite mode is picked based on the current theme's classification (light/dark) defined in `theme_autorun.php`.

**Theme dropdown** — lists all 34 DaisyUI themes. Selecting one applies instantly and persists to `localStorage`. Disable themes you don't want by removing them from both `tailwind.config.js` (the `daisyui.themes` array) **and** `theme_autorun.php` (the `$daisyThemes` array), then rebuild.

## How it compares to xSwatch5

| | xSwatch5 | xTailwind |
|---|----------|-----------|
| CSS framework | Bootstrap 5.3.8 | Tailwind 3.4 + DaisyUI 4 |
| Themes | 21 Bootswatch variants | 34 DaisyUI themes |
| CSS approach | Component library | Utility-first + DaisyUI components |
| JavaScript | Bootstrap bundle | Alpine.js |
| Build step | None (drop-in CSS) | Required (Tailwind compiler) |
| Dark mode | `data-bs-theme` attribute | `data-theme` attribute |
| RTL | Bootstrap logical props | Tailwind logical props |
| Form renderer | `XoopsFormRendererBootstrap5` | `XoopsFormRendererBootstrap5` (DaisyUI styles most BS5 classes gracefully) |

## Known Limitations (Proof of Concept)

- **Form renderer mismatch** — XOOPS core only ships a Bootstrap 5 form renderer. Forms render with BS classes; DaisyUI/Tailwind restyles most of them but some edge cases may look off. A proper `XoopsFormRendererTailwind` class would need to be contributed to XOOPS core for a clean solution.
- **Module templates not included** — only core theme templates are provided. Module overrides (newbb, publisher, wggallery, etc.) still need to be rewritten from the xSwatch5 Bootstrap versions.
- **Admin toolbar** — not yet ported from xSwatch5.
- **Cookie consent** — not yet ported.
- **Slider / jumbotron alternatives** — basic hero section included; full slider not ported.

## Customization

- **Custom styles** — edit `css/input.css` and use Tailwind's `@apply` directive or raw CSS in the appropriate `@layer`. Rebuild with `npm run build`.
- **Enable/disable themes** — edit `daisyui.themes` in `tailwind.config.js` and `$daisyThemes` in `theme_autorun.php`. Each enabled theme adds ~2-3 KB to the compiled CSS.
- **Custom color palette** — add a custom theme to `tailwind.config.js` under `daisyui.themes` with your own primary/secondary/accent colors. See [DaisyUI theming docs](https://daisyui.com/docs/themes/).

## Requirements

- XOOPS 2.5.11+ (RTL auto-detection requires 2.5.12+)
- PHP 8.2+
- Node.js 18+ (for the build step, or use the standalone CLI)

## Credits

- Adam Wathan and team — [Tailwind CSS](https://tailwindcss.com/)
- Saadeghi — [DaisyUI](https://daisyui.com/)
- Caleb Porzio — [Alpine.js](https://alpinejs.dev/)
- XOOPS Project — template integration patterns

## License

GPL v3 — see the XOOPS project license. Third-party assets are MIT-licensed.
