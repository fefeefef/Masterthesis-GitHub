# KI-Protokoll (intern, nicht abgeben)

Ausgefüllt am 21.8.2026.

Produkt / Version: **Tabellen 21.8.2026**

Umfang: **37 Tabellen und 1 Abbildung**, gesamte Arbeit (`data.tex` 2,
`experimental_design.tex` 2, `conformal.tex` 1, `results.tex` 18 Tabellen und die
einzige eingebundene Abbildung, `limitations.tex` 3, `appendix.tex` 11).
Caption und Notes-Block jeder Tabelle und der Abbildung vollständig neu erzeugt, die alten Fassungen
verworfen. `sections/prompts.tex` ist ausgenommen, die Datei ist in `main.tex` nicht
eingebunden und enthält nur alte Duplikate.

Bearbeitung: `unverändert` · `redaktionell` (nur Tippfehler, Umbrüche) · `überarbeitet` (von mir neu formuliert)

| Tabelle / Abbildung | Datum | Bearbeitung | Zahlen gegen CSV geprüft |
|---|---|---|---|
| tab:features_overview | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:publication-lags | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:refit_blocks | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:model_selection | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:cp_methods | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:res_gate | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:res_point | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| fig:skill_heatmap | 21.8.2026 | unverändert | ☐ |  <!-- neu, Abbildung -->
| tab:res_ranking | 21.8.2026 | unverändert | ☐ |
| tab:res_tirex2_cp | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:res_percountry | 21.8.2026 | unverändert | ☐ |
| tab:rq3_results | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:rq1_results | 21.8.2026 | unverändert | ☐ |
| tab:res_contrasts | 21.8.2026 | unverändert | ☐ |
| tab:rq2_results | 21.8.2026 | unverändert | ☐ |
| tab:res_rq2_lstmG | 21.8.2026 | unverändert | ☐ |
| tab:res_cov | 21.8.2026 | unverändert | ☐ |
| tab:res_xm | 21.8.2026 | unverändert | ☐ |
| tab:res_mcs | 21.8.2026 | unverändert | ☐ |
| tab:res_regime | 21.8.2026 | unverändert | ☐ |
| tab:res_robustness_pool | 21.8.2026 | unverändert | ☐ |
| tab:res_robustness_ie | 21.8.2026 | unverändert | ☐ |
| tab:res_robustness_seeds | 21.8.2026 | unverändert | ☐ |
| tab:res_robustness_burnin | 21.8.2026 | unverändert | ☐ |
| tab:lim_point | 21.8.2026 | unverändert | ☐ |
| tab:lim_dq | 21.8.2026 | unverändert | ☐ |
| tab:lim_amendments | 21.8.2026 | unverändert | ☐ |
| tab:app_frozen | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_protocol | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_nn_space | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_lgbm_space | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_frozen_nn | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_frozen_lgbm | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_frozen_arma | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_country | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_tail | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_pool | 21.8.2026 | unverändert | ☐ |  <!-- neu -->
| tab:app_fragility | 21.8.2026 | unverändert | ☐ |  <!-- neu -->

Die mit `<!-- neu -->` markierten Zeilen fehlten in der ersten Liste; sie sind
genauso betroffen wie die übrigen.

Prompt geändert? Neue Fassung hier notieren und oben vermerken, für welche Tabellen sie galt.

| Fassung | Datum | Änderung | Galt für |
|---|---|---|---|
| v1 | 21.8.2026 | Erstfassung | alle 37 Tabellen |
| v2 | 21.8.2026 | Ergänzt die Stilregel „Stil aus dem Fließtext wird übernommen"; die Ausgabe verlangt keine Kontrollliste der weggelassenen Angaben mehr | `fig:skill_heatmap` direkt unter v2 erzeugt; alle 37 Tabellen erneut geprüft, davon 10 textlich geändert: `tab:app_fragility`, `tab:cp_methods`, `tab:lim_point`, `tab:publication-lags`, `tab:res_cov`, `tab:res_mcs`, `tab:res_point`, `tab:res_ranking`, `tab:res_rq2_lstmG`, `tab:res_tirex2_cp` |

## Datenquelle je Tabelle

Für das Abhaken der Spalte *Zahlen gegen CSV geprüft*. Tabellen mit `—` enthalten
keine Ergebniszahlen (Konfigurations- und Katalogtabellen).

- `tab:features_overview` (data.tex) — —
- `tab:publication-lags` (data.tex) — —
- `tab:refit_blocks` (experimental_design.tex) — —
- `tab:model_selection` (experimental_design.tex) — —
- `tab:cp_methods` (conformal.tex) — —
- `tab:res_gate` (results.tex) — calibration_tests.csv, validity_flags.csv
- `tab:res_point` (results.tex) — cells.csv
- `fig:skill_heatmap` (results.tex) — ranking_all.csv
- `tab:res_ranking` (results.tex) — ranking_all.csv
- `tab:res_tirex2_cp` (results.tex) — ranking_all.csv
- `tab:res_percountry` (results.tex) — cells.csv
- `tab:rq3_results` (results.tex) — dm_contrasts.csv
- `tab:rq1_results` (results.tex) — dm_contrasts.csv
- `tab:res_contrasts` (results.tex) — dm_contrasts.csv
- `tab:rq2_results` (results.tex) — dm_contrasts.csv
- `tab:res_rq2_lstmG` (results.tex) — dm_percountry.csv, dm_contrasts.csv
- `tab:res_cov` (results.tex) — cells.csv
- `tab:res_xm` (results.tex) — dm_crossmodel.csv, dm_crossmodel_percountry.csv
- `tab:res_mcs` (results.tex) — mcs_results.csv, run_metadata.json
- `tab:res_regime` (results.tex) — regime_results.csv, stress_drop.csv
- `tab:res_robustness_pool` (results.tex) — cells.csv
- `tab:res_robustness_ie` (results.tex) — robustness_ex_ie.csv
- `tab:res_robustness_seeds` (results.tex) — ranking_all.csv
- `tab:res_robustness_burnin` (results.tex) — cells.csv
- `tab:lim_point` (limitations.tex) — cells.csv, dm_percountry.csv
- `tab:lim_dq` (limitations.tex) — calibration_tests.csv
- `tab:lim_amendments` (limitations.tex) — —
- `tab:app_frozen` (appendix.tex) — —
- `tab:app_protocol` (appendix.tex) — —
- `tab:app_nn_space` (appendix.tex) — —
- `tab:app_lgbm_space` (appendix.tex) — —
- `tab:app_frozen_nn` (appendix.tex) — —
- `tab:app_frozen_lgbm` (appendix.tex) — —
- `tab:app_frozen_arma` (appendix.tex) — —
- `tab:app_country` (appendix.tex) — cells.csv, validity_flags.csv
- `tab:app_tail` (appendix.tex) — cells.csv
- `tab:app_pool` (appendix.tex) — cells.csv
- `tab:app_fragility` (appendix.tex) — ranking_all.csv, robustness_ex_ie.csv

## Offene Punkte

- `TABELLEN_KONTROLLLISTE.md`, Abschnitt B: zehn Inhalte aus den alten Captions,
  die im Fließtext fehlen. Entscheiden, ob sie in den Text kommen.
- `TABELLEN_KONTROLLLISTE.md`, Abschnitt C: sechs fehlende Definitionen, vor
  allem die leeren Stubs `sec:eval:comparison`, `sec:eval:mcs`,
  `sec:eval:regime`, `sec:eval:economic` und `sec:eval:robustness`.
- `sections/hilfsmittel.tex` ist in `main.tex` nicht per `\input` eingebunden.
- Der Titel und die Legende in `figures/skill_heatmap.png` sind deutsch
  („Median-Winkler-Skill vs. RW-native (× = Validity-Flag verfehlt)“). Die Caption
  kann das nicht beheben, die Grafik muss dafür neu erzeugt werden.
- `figures/rolling_coverage_top3.png`, `figures/stress_drop.png` und
  `figures/vix_complexity.png` liegen im Ordner, sind aber in keiner `.tex`-Datei
  eingebunden.
