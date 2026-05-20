# Prediger Design System — Claude Agent Skill

A [Multica](https://multica.ai) / [Anthropic Agent Skills](https://github.com/anthropics/agent-skills) compatible skill that gives Claude Code agents full access to the Prediger Design System.

## What's included

| Category | Contents |
|---|---|
| **Colors** | 35 tokens with hex values, CSS variable names, and usage notes |
| **Typography** | 24 type scales (12–80 px), font weights, line-heights |
| **Spacing** | 8 px baseline grid with standard multiples |
| **Shadows & Transitions** | 5 shadow levels, 3 transition speeds |
| **Icons** | 163 SVG sprite icons across 6 categories |
| **Layout** | Foundation 12-column grid, breakpoints, max-width 1200 px |
| **UI Components** | 289 patterns (components, modules, organisms) with HTML markup, BEM modifiers, and SCSS source paths |

## Import into Multica

1. **Multica UI → Skills → + New → GitHub**
2. Paste this repository URL
3. Multica fetches `SKILL.md` and `prediger_design_system_skill.json` automatically
4. **Agents → [your agent] → Skills** — enable *Prediger Design System*

The skill activates on the agent's next task. Claude Code finds the files under `.claude/skills/prediger-design-system/`.

## Suggested agent instructions

Add this to your agent's *Instructions* field in Multica:

```
You are a frontend design agent for Prediger.

The Prediger Design System is available at:
.claude/skills/prediger-design-system/prediger_design_system_skill.json

Read it with the Read tool or Bash (jq) when you need design tokens, components, or icons.
Always use the tokens, CSS classes, and markup templates from the design system.
Never invent class names or color values — look them up first.
```

## Usage examples for the agent

**Look up a color**
```bash
jq '.injected_knowledge.design_tokens.colors.brand' prediger_design_system_skill.json
```

**Find a component by CSS class**
```bash
jq '.injected_knowledge.css_class_index["c-button"]' prediger_design_system_skill.json
# → "controls-button"

jq '.injected_knowledge.components.by_type.component.items[] | select(.kss_ref == "controls-button")' \
  prediger_design_system_skill.json
```

**List all UI icons**
```bash
jq '.injected_knowledge.icons.ui[]' prediger_design_system_skill.json
```

## Figma quick-reference

| Figma concept | Source field |
|---|---|
| Color styles | `design_tokens.colors.*` — group as `Brand/`, `Neutrals/`, `Semantic/` |
| Text styles | `design_tokens.typography.size_scale` — name as `Copy/16`, `Display/32` |
| Component variants | `components.by_type.*.items[].modifiers` |
| Icon library | `icons.*` — one Figma component per icon name |
| Spacing | 8 px base grid — multiples: 8, 16, 24, 32, 40, 48, 64, 80 |

## CSS conventions

```
c-  component  →  atomic, self-contained  (c-button, c-arrowlink)
m-  module     →  composition of components  (m-accordion, m-anchors)
o-  organism   →  full page region  (o-footer, o-autosuggest)

Modifier:  c-button--secondary   (double-dash)
Template:  [modifier class]      (placeholder in markup strings)
```

## Keeping the skill up to date

The JSON is generated from the live styleguide. To refresh it:

```bash
# 1. Scrape the pattern library
python styleguide_scraper.py

# 2. Rebuild the design skill
python generate_design_skill.py

# 3. Copy the new JSON into this directory
cp prediger_design_system_skill.json prediger-design-system/

# 4. Commit and push — Multica re-imports on the next GitHub sync
git add prediger-design-system/prediger_design_system_skill.json
git commit -m "chore: refresh design system skill"
git push
```

Credentials for the styleguide are read from `.env` (`STYLEGUIDE_USERNAME`, `STYLEGUIDE_PASSWORD`).

## File structure

```
prediger-design-system/
├── SKILL.md                          # Skill entry point (Multica reads this)
├── prediger_design_system_skill.json # Design system data (~750 KB)
└── README.md                         # This file
```
