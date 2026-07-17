# Presentación - AI Coding Instructions

## Project Overview

This is an **academic presentation repository** for a political science thesis analyzing Mexico's 2014 political reform and its effects on democratic quality (2015-2023). The project uses **Quarto** to generate interactive reveal.js presentations combining markdown, Python visualizations, and embedded interactive dashboards.

**Key Technologies:**
- **Quarto** (`.qmd` files) - slides compiled to HTML
- **Python 3** - Highcharts-core for data visualizations
- **CSS3** - Custom styling with Poppins & Georgia fonts
- **Reveal.js** - Presentation framework with Quarto plugins

## Architecture & Key Components

### Source Files Structure
- **Main presentations:**
  - `tesis.qmd` - Primary thesis presentation (2014 reform, democracy/partitocracia analysis)
  - `demo.qmd` - Interactive demo with Shiny app embeds
  - `ejemplo.qmd` - Simple example/template
- **Generated outputs:**
  - `tesis.html`, `demo.html`, `demo1.html` - Compiled presentations
  - `tesis_files/`, `demo_files/` - Auto-generated Quarto asset directories

### Interactive Content Strategy
- **Embedded iframes** link to Shiny dashboards (shinyapps.io): map visualizations, age pyramids, voting convergence
- **Highcharts** Python library generates area charts for CO₂ emissions, economic convergence (see `demo.qmd` & `ejemplo.qmd`)
- **Reactable tables** in `preview/tables/` for democracy/partitocracia categorizations
- **Sankey diagrams** in `preview/` for methodology flow visualization

### Styling System
All styling defined in `styles.css` (no build step; reference-location: document in YAML):
- **CSS variables** for brand colors (primary-blue: #1e3a8a, secondary-blue: #3b82f6)
- **Fixed header/footer** with Universidad Modelo branding and logo (`images/modelo.png`)
- **Reveal.js layout classes**: `.layout-horizontal`, `.layout-vertical`, `.panel-tabset`, `.columns`
- **Fragments** for reveal animations: `.fragment .fade-down`, `.fragment .fade-up`, `.fragment .fade-left`

## Development Conventions

### Quarto Patterns (`.qmd` syntax)
1. **YAML frontmatter** required in all `.qmd` files - includes `format: revealjs` and `css: styles.css`
2. **Slide separator**: `##` creates new slide; nesting with `###` for sub-sections
3. **Layout system**: Use divs with Quarto classes:
   ```markdown
   ::: {.layout-horizontal}
   ::: {.layout-vertical}
   :::{.fragment .fade-down}
   ```
4. **Inline HTML**: iframes for Shiny apps use `<iframe src="..." width="190%" height="..."></iframe>` (width >100% for overflow effect)
5. **Panel tabs**: `::: {.panel-tabset}` wraps tabs; `### Tab Name` defines each tab
6. **Text styling**: `[text]{.highlight-red}`, `[text]{.mark}`, `[text]{.underline}`, `[text]{.smallcaps}`

### Python Visualization Pattern
Use `highcharts_core` for interactive charts in code cells:
```python
from highcharts_core.chart import Chart
from highcharts_core.options import HighchartsOptions

options = HighchartsOptions()
options.chart = ChartOptions(type='area')
# ... configure series, title, tooltip, etc.
chart = Chart.from_options(options)
chart.display()
```
Charts support `dashStyle`, `stacking`, and responsive `Responsive` rules for mobile layouts.

### CSS & Custom Classes
- Override with `styles.css`; use CSS variables for consistency
- Common custom classes: `.text-block`, `.graphic-item`, `.graphic-block`, `.caption`
- Fragment animations: `.fade-down`, `.fade-up`, `.fade-left`, `.fade-right`
- Callouts: `.callout-tip`, `.callout-note`, `.callout-important`, `.callout-caution`, `.callout-warning`

## Build & Deployment

### Build Command
```bash
quarto render tesis.qmd --to revealjs
quarto render demo.qmd --to revealjs
quarto render ejemplo.qmd --to revealjs
```
Output: HTML files in root + auto-generated `*_files/` directories

### No CI/CD: Manual workflow
- Render locally; commit `.html` + asset directories
- Shiny dashboards hosted externally (shinyapps.io) - modify iframe `src=` URLs to update links

## Critical Details for AI Agents

### Common Issues & Solutions
1. **Iframe sizing**: Set `width="190%"` to overflow for zoom effect; height must account for header (50px) + footer (40px) + padding
2. **Fragment timing**: Fragments appear sequentially by `.fragment` order in source; nest inside layout divs for group animation
3. **Asset generation**: After rendering, Quarto auto-creates `tesis_files/libs/` with reveal.js plugins - **do not edit manually**; regenerate via `quarto render`
4. **CSS precedence**: `styles.css` loaded after Quarto defaults via `css: styles.css` in YAML

### Data Integration Points
- **Shiny embed pattern**: Use iframe with full URL; verify `src=` is accessible before committing
- **Python chart pattern**: Define all chart config in `HighchartsOptions()` object; avoid hardcoding dimensions
- **Table imports**: HTML tables in `preview/tables/` use local file paths - adjust relative paths if moving files

### Naming Conventions
- Slide decks: `{topic}.qmd` (lowercase, hyphens for compound words)
- Images: `images/{name}.png` (lowercase, descriptive names)
- CSS classes: camelCase (`.layoutHorizontal`), hyphen-separated for state (`.fragment-fade-down`)
- Quarto divs: `{.class-name}` syntax within `:::` fences

## Quick Reference: Common Tasks

| Task | Pattern |
|------|---------|
| Add slide | Add `##` at desired location |
| Add animation | Wrap content in `:::{.fragment .fade-down}` |
| Embed dashboard | `<iframe src="URL" width="190%" height="..."></iframe>` |
| Create tabs | `::: {.panel-tabset}` + `### Tab Name` sections |
| Style text | `[text]{.highlight-red}` or `.mark`, `.underline` |
| Python chart | Use `highcharts_core.Chart.from_options()` |
| Adjust colors | Edit CSS variables in `styles.css` (--primary-blue, etc.) |

## File Dependencies
- Presentations depend on `styles.css` (no external CDN for styling)
- Reveal.js plugins auto-loaded from `*_files/libs/revealjs/plugin/` after render
- Shiny apps (external) must be manually updated in iframe URLs
