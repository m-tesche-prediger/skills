---
name: prediger-design-system
description: >
  Gives access to the complete Prediger Design System: color tokens (hex values + CSS variables),
  typography scale, 8px spacing grid, SVG icon library (163 icons), and 289 UI component patterns
  with HTML markup and BEM modifier variants. Use for frontend implementation and Figma design work.
---

# Prediger Design System

Du hast Zugriff auf das vollständige Prediger Design System. Die Daten liegen als JSON-Datei neben dieser SKILL.md.

## Wie du die Daten nutzt

Lies die Datei mit dem `Read`-Tool oder über den Bash-Befehl:

```bash
cat prediger_design_system_skill.json | jq '.injected_knowledge.design_tokens.colors.brand'
```

Die Skill-Datei ist unter `prediger_design_system_skill.json` im Skill-Verzeichnis erreichbar (`.claude/skills/prediger-design-system/prediger_design_system_skill.json`).

## Struktur der Daten

```
injected_knowledge
├── design_tokens
│   ├── colors          → 35 Farben mit hex, CSS-Variablenname, Verwendungshinweis
│   │   ├── brand       → --color-brand (#f2d851), --color-petrol (#3b5c68) …
│   │   ├── neutrals    → --color-dark (#262a30), --color-light (#fff) …
│   │   ├── mid_tones   → --color-slate, --color-iron …
│   │   ├── semantic    → --color-error, --color-success, --color-warning …
│   │   └── tints       → rgba-Werte für Overlays
│   ├── typography
│   │   ├── font_families → Mont (copy & display), Fallback: Helvetica/Arial
│   │   ├── font_weights  → light=400, regular=500, bold=700
│   │   └── size_scale    → 12px–80px mit rem, line-height, CSS-Shorthand-Variable
│   ├── spacing         → 8px Baseline-Grid, Vielfache: 8/16/24/32/40/48/64/80px
│   ├── shadows         → light/medium/heavy/popup/inset
│   └── transitions     → default (.05s) / smooth (.25s) / smoother (.45s)
├── layout
│   └── grid            → Foundation 12-Spalten, max. 1200px, Breakpoints s/m/l
├── icons               → 163 SVG-Icons in 6 Gruppen
│   ├── ui              → 113 Icons (arrow, cart, heart, magnifier …)
│   ├── product_attributes → Schutzklassen, LED, Kelvin …
│   ├── payment         → paypal, visa, mastercard …
│   └── social          → facebook, instagram, linkedin …
├── css_class_index     → CSS-Klasse → kss_ref (für schnelle Suche)
└── components
    └── by_type
        ├── component   → Atome: Button, Link, Input, Card …
        ├── module      → Moleküle: Navigation, Filter, Accordion …
        └── organism    → Organismen: Header, Footer, Product Grid …
```

## CSS-Namenskonvention (BEM)

- `c-` = component (Atom)
- `m-` = module (Molekül)
- `o-` = organism (Organismus)
- Modifier: `c-button--secondary` (Doppel-Bindestrich)
- In Markup-Templates: `[modifier class]` = Platzhalter für optionale Modifier-Klasse

## Icon-Verwendung

```html
<svg class="c-icon">
  <use xlink:href="#svg_icon-arrow"></use>
</svg>
```

## Farben per CSS-Variable verwenden

```css
color: var(--color-brand);       /* #f2d851 — Brand Yellow */
color: var(--color-petrol);      /* #3b5c68 — Petrol (sekundäre Markenfarbe) */
color: var(--color-dark);        /* #262a30 — Standard-Textfarbe */
background: var(--color-error);  /* #c80000 */
```

## Typografie per CSS-Variable verwenden

```css
font: var(--font-16-copy);    /* 1rem/1.5 Mont — Body-Text */
font: var(--font-24-display); /* 1.5rem/1.333 Mont — Headline */
font-weight: var(--font-weight-bold); /* 700 */
```

## Figma-Leitfaden

- **Color Styles**: Gruppe Brand/, Neutrals/, Semantic/ — Namen = Token-Name ohne `--color-`
- **Text Styles**: Copy/12, Copy/16 (Body), Display/24, Display/32 — Font: Mont
- **Spacing**: 8px-Raster in Figma, Standardwerte: 8, 16, 24, 32, 40, 48, 64, 80
- **Components**: Modifier → Figma Variants (z. B. Button → Primary/Secondary/Large)
- **Icons**: SVG-Icons als Figma-Komponenten, gruppiert wie in der Bibliothek (UI Icons/, Payment Icons/)
