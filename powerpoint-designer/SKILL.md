---
name: powerpoint-designer
description: Erstellt PowerPoint-Präsentationen im Prediger Corporate Design aus einer natürlichsprachigen Beschreibung. Aktivieren wenn der Nutzer eine Präsentation, Folien, eine PPTX-Datei oder ein Slide-Deck erstellen möchte. Nutzt ausschließlich die Layouts aus powerpoint_master.pptx. Keine Animationen.
license: Proprietary
compatibility: Requires Python 3.11+ and python-pptx (pip install python-pptx). powerpoint_master.pptx must be present in the skill directory.
metadata:
  author: prediger
  version: "1.0"
allowed-tools: Bash Read Write
---

# PowerPoint-Agent — Prediger Corporate Design

Erstellt vollständige `.pptx`-Präsentationen aus einer Nutzerbeschreibung. Alle Folien basieren ausschließlich auf den Layouts der Datei `powerpoint_master.pptx`.

## Workflow

### 1. Anfrage klären
Falls die Anfrage unklar ist, frage nach:
- Titel und Thema der Präsentation
- Grobe Gliederung (Kapitel, Kernaussagen)
- Ob Bilder vorhanden sind (Dateipfade)

Bei eindeutiger Anfrage direkt starten.

### 2. JSON-Spezifikation schreiben

Erstelle `<thema>.json`:

```json
{
  "title": "Titel der Präsentation",
  "output": "dateiname.pptx",
  "slides": [
    { "layout": "titelfolie", "title": "Präsentationstitel" },
    { "layout": "agenda", "heading": "Agenda", "items": ["Punkt 1", "Punkt 2"] },
    { "layout": "kapitel_1", "text": "01 Erster Abschnitt" },
    { "layout": "inhalt", "supertitle": "Kategorie", "heading": "Kernaussage", "bullets": ["Stichpunkt 1", "Stichpunkt 2"] },
    { "layout": "titelfolie", "title": "Vielen Dank.\nwww.prediger.de" }
  ]
}
```

### 3. Generator ausführen

```bash
python pptx_generator.py <thema>.json
```

### 4. Ergebnis melden

- Dateiname und Pfad der erzeugten `.pptx`
- Tabellarische Übersicht aller Folien (Nr., Layout, Inhalt)
- Hinweis auf leere Bildplatzhalter

---

## Pflichtregeln (nicht verhandelbar)

1. **Erste Folie**: immer `titelfolie`
2. **Letzte Folie**: `titelfolie` mit Dank/Kontakt
3. **Vor jedem Themenblock**: `kapitel_1` oder `kapitel_2` (abwechselnd)
4. **Keine Animationen** — der Generator fügt keine ein, so muss es bleiben
5. **Nur Vorlagen-Layouts** — keine eigenen Formen, Farben oder Schriften
6. **Texte kurz**: max. 6 Stichpunkte, max. 8 Wörter je Stichpunkt

---

## Verfügbare Layouts

| `layout`-Schlüssel | Wann verwenden | Pflichtfelder |
|---|---|---|
| `titelfolie` | Titelfolie, Abschluss | `title` |
| `titelfolie_bild` | Titelfolie mit Foto | `title`, `image` (opt.) |
| `kapitel_1` | Abschnittstrenner (dunkel) | `text` |
| `kapitel_2` | Abschnittstrenner (gelb) | `text` |
| `agenda` | Inhaltsübersicht | `items[]` |
| `inhalt` | Statement / Kernaussage | `heading` oder `bullets[]` |
| `inhalt_bild` | Text + Bild | `heading`, `image` (opt.) |
| `zwei_spalten` | Vergleich, Pro/Contra | `heading`, `col1_*`, `col2_*` |
| `drei_spalten` | Drei Themen nebeneinander | `heading`, `col1_*`, `col2_*`, `col3_*` |
| `prozess` | Prozessschritte (max. 5) | `heading`, `steps[]` |
| `zahl_gross` | Eine große Kennzahl | `number`, `description` |
| `vier_zahlen` | KPI-Übersicht | `numbers[]` (max. 4) |
| `zwei_bilder` | Zwei Fotos | `heading`, `images[]` |
| `drei_bilder` | Drei Fotos | `heading`, `images[]` |
| `bild_2_texte` | Bild + Text links/rechts | `heading`, `image` (opt.) |
| `galerie_1` | Fotogalerie (6 Bilder) | `heading` |
| `galerie_2` | Fotogalerie (7 Bilder) | `heading` |

Vollständige Feldbeschreibungen → `powerpoint_skill.json` → `injected_knowledge.layouts`

### Layout-Feldreferenz (häufigste)

**`zwei_spalten`**
```json
{ "layout": "zwei_spalten", "heading": "Vergleich",
  "col1_supertitle": "Vorher", "col1_bullets": ["Alt 1", "Alt 2"],
  "col2_supertitle": "Nachher", "col2_bullets": ["Neu 1", "Neu 2"] }
```

**`prozess`** — `steps[]` mit `label`, `title`, `text` (oder `bullets`), max. 5 Schritte:
```json
{ "layout": "prozess", "heading": "Unser Prozess",
  "steps": [
    { "label": "01", "title": "Analyse",  "text": "Bedarfsanalyse" },
    { "label": "02", "title": "Konzept",  "text": "Lichtplanung" },
    { "label": "03", "title": "Angebot",  "text": "Festpreis" },
    { "label": "04", "title": "Montage",  "text": "Fachbetrieb" },
    { "label": "05", "title": "Service",  "text": "10 Jahre Garantie" }
  ] }
```

**`vier_zahlen`** — `numbers[]` mit `value` und `description`, max. 4:
```json
{ "layout": "vier_zahlen",
  "numbers": [
    { "value": "60+",  "description": "Jahre am Markt" },
    { "value": "95 %", "description": "Weiterempfehlung" }
  ] }
```

**`zwei_bilder` / `drei_bilder`** — `images[]` mit `image` (Dateipfad) und `caption`:
```json
{ "layout": "zwei_bilder", "heading": "Referenzen",
  "images": [
    { "image": "fotos/projekt_a.jpg", "caption": "Hotel Berlin" },
    { "image": "fotos/projekt_b.jpg", "caption": "Büro Hamburg" }
  ] }
```

---

## Bilder

- Vorhandene Bilddatei: Pfad im `"image"`-Feld angeben
- Kein Bild vorhanden: Feld weglassen — Platzhalter bleibt leer
- Im Ergebnisbericht angeben, welche Folien noch Bilder benötigen

---

## Corporate Design

- **Font**: Poppins (in Vorlage definiert)
- **Farben**: Anthrazit `#2D3036` · Gelb `#F5DF4D` · Creme `#FDF8E3` · Orange `#FFC000`
- **Format**: 16:9 Widescreen (33,87 × 19,05 cm)
- **Animationen**: keine
