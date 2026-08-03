# Ergebnisartefakte — Diagnostics-Run `run_full_20260803_095330`

Quelle aller Zahlen, die in `sections/results.tex` als Kommentar hinterlegt sind.
Kopiert aus `Masterthesis/notebooks/results_diagnostics/run_full_20260803_095330/`.

## Herkunft

| Feld | Wert |
|---|---|
| Diagnostics-Run | `run_full_20260803_095330` (03.08.2026, `mode=full`) |
| Conformal-Run | `run_full_20260721_145309` |
| Panel-Hash (`data_sha256`) | `5f27a4124f93…68ec2e` — identisch über **alle sieben** Modellfamilien |
| git commit (Code) | `b1811e67b6a92460cdf94e405d29093a5945680f` |
| Python / numpy / pandas / scipy | 3.13.1 / 2.1.3 / 2.3.3 / 1.17.1 |

Der identische `data_sha256` über alle Modellläufe belegt, dass sämtliche Modelle auf
demselben Panel nach dem TARGET2-Lag-Rebuild (33 d) gerechnet wurden.

## Konfiguration

`alpha = 0.20` · FDR `q = 0.10` · Gate-Schwelle 70 % der Länder · MCS `alpha = 0.10` ·
Moving-Block-Bootstrap Blocklänge 6, 1000 Draws · DQ-Lags 4 · Rolling-Coverage-Fenster 36 ·
Referenz für alle Skills: `rw / L / native` · `rng_seed = 42`

## Umfang

10 Länder · 127 Testmonate (2015-10 bis 2026-04) · 742 Kombinationen
(Modell × Regime × Seed × CP-Methode) · 21 200 Zellen · 1249 vorregistrierte Kontraste ·
66 Post-hoc-Cross-Model-Kontraste · 816 Findings · **485 / 742 Kombinationen** passieren
das Validity-Gate.

## Tabellen

| Datei | Zeilen | Inhalt |
|---|---|---|
| `tables/cells.csv` | 21 200 | Basistabelle: eine Zeile je Zelle × Pool-Modus. Coverage, Winkler, Breite, Penalties, Skill, MAE/RMSE-Ratio, PT-Test |
| `tables/ranking_all.csv` | 742 | Median-Skill, Coverage, Gate-Flag, Seed-Spanne je Kombination — **Haupttabelle für §8.3** |
| `tables/validity_flags.csv` | 742 | Nur Gate-Ergebnis |
| `tables/calibration_tests.csv` | 7 420 | Kupiec / Christoffersen / DQ, p- und q-Werte je Land |
| `tables/dm_contrasts.csv` | 1 249 | Vorregistrierte Kontraste RQ1 / RQ2 / RQ3 / F10, Panel-Ebene |
| `tables/dm_percountry.csv` | 12 490 | Dieselben Kontraste je Land (DM-HLN) |
| `tables/dm_crossmodel.csv` | 66 | Post-hoc-Cross-Model-Vergleiche (eigene FDR-Familie) |
| `tables/dm_crossmodel_percountry.csv` | 660 | dito je Land |
| `tables/mcs_results.csv` | 260 | Model Confidence Set, 26 Kandidaten × 10 Länder |
| `tables/regime_results.csv` | 5 194 | Coverage / Winkler je Regime (calendar, volatility, vix) |
| `tables/stress_drop.csv` | 742 | Coverage im Stress-Terzil gegen Gesamtcoverage |
| `tables/vix_complexity.csv` | 12 | F13-Komplexitätshypothese je Modell |
| `tables/robustness_ex_ie.csv` | 742 | Reaggregation ohne Irland |
| `tables/econ_results.csv` | 742 | Sharpe, Active Share, Kapitalquotient |
| `tables/findings.csv` | 816 | Automatisch extrahierte Findings mit Score |

`story_report.md` ist der automatisch erzeugte Findings-Report desselben Runs.
`run_metadata.json` enthält die vollständige Konfiguration inklusive aller Quell-Run-Pfade
und Hashes.

## Lesekonventionen

- **Skill** `= 1 − W_modell / W_rw` je Land, dann Median über die Länder. `> 0` = besser als
  die Referenz.
- **`median_dbar_norm`** in den Kontrasttabellen: normalisiertes Loss-Differential `A − B`.
  Winkler ist ein Verlust, also bedeutet **negativ = A besser als B**.
- **`share_a_better`**: Anteil der Länder, in denen A besser ist.
- **`t_panel`**: t-Statistik auf dem Querschnittsmittel des Loss-Differentials je Monat,
  Newey-West-HAC.

## Figuren

`../figures/` enthält `skill_heatmap.png`, `vix_complexity.png`, `stress_drop.png`,
`rolling_coverage_top3.png` aus demselben Run.
