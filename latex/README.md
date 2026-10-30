# DS612 IDV Group 7 — LaTeX Project (v3)

## How to compile
1. Upload this folder to **Overleaf** → New Project → Upload `.zip`
2. Set the **main document** to `main.tex`
3. Compiler: **pdfLaTeX**
4. Full build order for correct references: pdfLaTeX → BibTeX → pdfLaTeX → pdfLaTeX

---

## File map

```
main.tex                         ← ROOT — compile this
preamble.tex                     ← all \usepackage + custom commands
references.bib                   ← BibTeX entries

sections/
  00_titlepage.tex               ← team names & student IDs
  01_abstract.tex
  02_introduction.tex            ← CPQH table
  03_dataset.tex                 ← dataset overview, features, severity note
  04_fault_analysis.tex          ← 12 data quality faults
  05_data_preparation.tex        ← pipeline, 2×IQR rationale, engineered features
  06_hypotheses.tex              ← H1–H4 statements & abstractions
  07_overview_viz.tex            ← V1 geo map, V2 states, V3 dist, V4 yearly
  08_exploratory_boxplots.tex    ← V_B1 distance, V_B2 duration, V_B3 count  [NEW v3]
  09_h1_weather.tex              ← V5a–V5d + H1 verdict  (v3 numbers)
  10_h2_rush.tex                 ← V6a–V6d + H2 verdict  (v3 plateau)
  11_road_features.tex           ← V7 road features
  12_extended_hypotheses.tex     ← H3, H4, limitations
  13_statistical_validation.tex  ← test results tables (fill from Python)
  14_conclusion.tex              ← final summary table

figures/                         ← drop all exported chart PNGs here
tables/                          ← optional standalone reusable tables
```

---

## What changed in v3

| Change | Where |
|--------|-------|
| 2023 data excluded (partial year) | `03_dataset`, `05_data_preparation`, headers |
| 2×IQR outlier removal on Duration & Distance | `05_data_preparation` |
| New section: Exploratory Box Plots (V_B1, V_B2, V_B3) | `08_exploratory_boxplots` |
| Updated H1 numbers: Rain 24.38%, Cloudy 22.43%, Snow/Ice 21.45% | `09_h1_weather` |
| Updated H4: early morning plateau hours 3–6, Saturday peak ~0.3176 | `10_h2_rush`, `12_extended_hypotheses` |
| H3 weather gap updated to 6.53 pp (was 5.71 pp) | `12_extended_hypotheses` |

---

## Adding a figure

Every chart slot has a grey `\placeholder{}` box with a commented-out
`\figchart{}` line above it. When a chart is ready:

1. Save PNG to `figures/` e.g. `figures/v1_geo_map.png`
2. **Comment out** the `\begin{figure}...\end{figure}` placeholder block
3. **Uncomment** the `\figchart` line:
   ```latex
   \figchart{v1_geo_map}{Geographic distribution of high-severity accidents}{fig:v1}
   ```

---

## Custom commands (all in `preamble.tex`)

| Command | Purpose |
|---------|---------|
| `\todo{msg}` | Red flag for pending work — search `\todo{` to find all |
| `\placeholder[h]{text}` | Grey box for missing figure |
| `\figchart[w]{file}{caption}{label}` | Drop-in figure once exported |
| `\idiomtable{marks}{channels}{action}{scale}` | Standard idiom analysis box |
| `\verdict{HX}{verdict}{detail}` | Hypothesis verdict summary table |
| `\insightbox{text}` | Styled italic blockquote for chart insights |

---

## Pending work — search `\todo{` in Overleaf

| Location | What's needed |
|----------|--------------|
| All section figure slots | Export charts from Python → `figures/` |
| `13_statistical_validation` | Run tests in Python, paste statistics |
| `12_extended_hypotheses` table | Hour 3h severity rate |
| `09_h1_weather` table | `Other` weather group rate |
| `01_abstract` | Final polish once all viz complete |
| `references.bib` | Add any additional citations |
