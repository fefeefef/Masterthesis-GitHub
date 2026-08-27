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
| tab:features_overview | 21.8.2026 | unverändert | ☐ |  
| tab:publication-lags | 21.8.2026 | unverändert | ☐ | 
| tab:refit_blocks | 21.8.2026 | unverändert | ☐ |  
| tab:model_selection | 21.8.2026 | unverändert | ☐ | 
| tab:cp_methods | 21.8.2026 | unverändert | ☐ |  
| tab:res_gate | 21.8.2026 | unverändert | ☐ |  
| tab:res_point | 21.8.2026 | unverändert | ☐ |  
| fig:skill_heatmap | 21.8.2026 | unverändert | ☐ | 
| tab:res_ranking | 21.8.2026 | unverändert | ☐ |
| tab:res_tirex2_cp | 21.8.2026 | unverändert | ☐ |  
| tab:res_percountry | 21.8.2026 | unverändert | ☐ |
| tab:rq3_results | 21.8.2026 | unverändert | ☐ |  
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
| tab:app_frozen | 21.8.2026 | unverändert | ☐ | 
| tab:app_protocol | 21.8.2026 | unverändert | ☐ |  
| tab:app_nn_space | 21.8.2026 | unverändert | ☐ | 
| tab:app_lgbm_space | 21.8.2026 | unverändert | ☐ |  
| tab:app_frozen_nn | 21.8.2026 | unverändert | ☐ | 
| tab:app_frozen_lgbm | 21.8.2026 | unverändert | ☐ | 
| tab:app_frozen_arma | 21.8.2026 | unverändert | ☐ | 
| tab:app_country | 21.8.2026 | unverändert | ☐ |
| tab:app_tail | 21.8.2026 | unverändert | ☐ |  
| tab:app_pool | 21.8.2026 | unverändert | ☐ |  
| tab:app_fragility | 21.8.2026 | unverändert | ☐ |  


Prompt geändert? Neue Fassung hier notieren und oben vermerken, für welche Tabellen sie galt.

| Fassung | Datum | Änderung | Galt für |
|---|---|---|---|
| v1 | 21.8.2026 | Erstfassung | alle 37 Tabellen |

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




# Prompt: Tabellen-Captions und Notes

**Zweck:** Erzeugung der Tabellenbeschreibungen in `sections/results.tex` und `sections/limitations.tex`.
**Für den Anhang „Übersicht verwendeter Hilfsmittel" aufbewahren.**

---

## VOR DER AUSFÜHRUNG

1. **Alte Captions und Notes löschen.** Sonst stimmt die Prämisse des Prompts nicht und die spätere Dokumentation wäre unrichtig. `\caption{}` auf einen Platzhalter reduzieren, `Notes.`-Minipages ganz entfernen.
2. **Protokoll unten ausfüllen** — Produktname, Versionsnummer, Datum. Nachträglich nicht rekonstruierbar.
3. **Tabellenweise ausführen**, nicht die ganze Arbeit auf einmal. Ein Durchlauf pro Tabelle, mit dem zugehörigen Abschnitt als Kontext. Ergebnisse werden sonst inkonsistent und Zahlen unzuverlässig.

---

## PROMPT (gueltige Fassung, 21.8.2026)

```
Du erstellst die Beschreibung fuer EINE Tabelle meiner Masterarbeit.

Ich gebe dir drei Dinge: (1) den vollstaendigen Fliesstext des Abschnitts, in
dem die Tabelle steht, (2) den LaTeX-Code der Tabelle selbst, (3) die
zugrundeliegende CSV aus results/tables/.

AUFGABE
Schreibe genau zwei Dinge, auf Englisch:
  A) einen \caption{}-Text, hoechstens 25 Woerter
  B) einen Notes-Block, hoechstens 90 Woerter
Bei einer Tabelle ohne Panels und ohne Marker entfaellt B ersatzlos.

WAS IN DIE NOTES GEHOERT - und sonst nichts:
  1. Definition jeder Spalte, deren Bedeutung sich nicht aus der Kopfzeile
     ergibt UND die nicht bereits in Kapitel 7 (Evaluation Framework) definiert
     ist. Ist sie dort definiert: nur \cref-Verweis, keine Wiederholung.
  2. Stichprobe und Einheit der Zelle (z. B. "127 monthly observations per
     country, ten countries").
  3. Verwendeter Test samt Korrektur, in einem Satz.
  4. Vorzeichenkonvention - welches Vorzeichen bedeutet welches Ergebnis. Das
     ist verpflichtend, sobald die Tabelle eine Groesse zeigt, bei der das nicht
     offensichtlich ist.
  5. Legende der Marker (z. B. was + bedeutet).
  6. Provenienz, falls sie zum Nachvollziehen noetig ist (etwa welche Seeds
     welcher Zeile zugrunde liegen).

DREI HARTE VERBOTE
  * KEINE Zahl, die bereits im mitgelieferten Fliesstext steht. Pruefe jede Zahl
    gegen den Fliesstext, bevor du sie schreibst. Zahlen, die nur in der Tabelle
    stehen, sind erlaubt, wenn sie zur Definition einer Spalte noetig sind.
  * KEINE Interpretation, Bewertung oder Einordnung. Keine Saetze ueber das, was
    das Ergebnis bedeutet, warum es so ausfaellt oder wie stark es ist. Die
    Beschreibung sagt, wie man die Tabelle LIEST, nicht was sie ZEIGT.
  * KEINE Zahl, die du nicht in der Tabelle oder der CSV verifizieren kannst.
    Erfinde nichts, runde nichts um, rechne nichts nach.

STIL - halte dich messbar an diese Vorgaben:
  * Amerikanische Schreibung: normalized, behavior, favor, optimization.
    NICHT normalised, behaviour, favour, per cent.
  * Hoechstens ein Semikolon pro 90 Woerter.
  * Keine Geviertstriche.
  * Keine Dreierparallelen der Form "A, B, and C" mit gleichgebautem Nebensatz
    an jedem Glied.
  * Keine "not X but Y"- und "rather than"-Konstruktionen.
  * Durchschnittliche Satzlaenge um 18 Woerter, hoechstens 28.
  * Schlichte Aussagesaetze. Wo es passt, mit "We", "This" oder "The" beginnen.
  * Stil aus dem Fliesstext wird uebernommen.

KONVENTIONEN DER ARBEIT
  * Uebernimm die Notation des Fliesstextes unveraendert: d_norm, Winkler skill,
    admissible, package, cell, stream, gate.
  * Referenzen als \cref{}, Modellnamen in \texttt{}.
  * Die Arbeit verwendet mehrere unterschiedliche Vorzeichenkonventionen
    nebeneinander. Pruefe im Fliesstext, welche fuer DIESE Tabelle gilt, und
    uebernimm sie woertlich. Rate nicht.

AUSGABE
  1. Der fertige LaTeX-Code fuer \caption{} und, falls noetig, den Notes-Block.
  2. Falls dir im Fliesstext eine Definition fehlt, die du fuer die Notes
     braeuchtest: sag es, statt sie zu erfinden.
```

---

## NACH DER AUSFUEHRUNG

- Jede Zahl in der Ausgabe gegen die CSV pruefen und die Spalte *Zahlen gegen
  CSV geprueft* abhaken.
- Inhalte, die in den alten Captions standen und im Fliesstext fehlen, gehoeren
  in den Fliesstext, nicht zurueck in die Notes. Die Liste dazu steht in
  `TABELLEN_KONTROLLLISTE.md`, Abschnitt B.
- Fehlende Definitionen aus `TABELLEN_KONTROLLLISTE.md`, Abschnitt C,
  entscheiden.

---

## PROTOKOLL (fuer den Anhang)

**Produkt / Version:** Tabellen 21.8.2026
**Ausfuehrungszeitraum:** 21.8.2026
**Umfang:** 37 Tabellen und 1 Abbildung, gesamte Arbeit. Caption und Notes-Block
wurden jeweils vollstaendig neu erzeugt, die vorherigen Fassungen verworfen.
`sections/prompts.tex` ist ausgenommen, die Datei ist in `main.tex` nicht
eingebunden.

**Bearbeitung:** `unveraendert` - `redaktionell` (nur Tippfehler, Umbrueche) -
`ueberarbeitet` (von mir neu formuliert)

| Datei | Tabelle / Abbildung | Datum | Prompt-Fassung | Bearbeitung | Zahlen gegen CSV geprueft | Datenquelle |
|---|---|---|---|---|---|---|
| `data.tex` | `tab:features_overview` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `data.tex` | `tab:publication-lags` | 21.8.2026 | v1+v2 | unverändert | ☐ | — |
| `experimental_design.tex` | `tab:refit_blocks` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `experimental_design.tex` | `tab:model_selection` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `conformal.tex` | `tab:cp_methods` | 21.8.2026 | v1+v2 | unverändert | ☐ | — |
| `results.tex` | `tab:res_gate` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | calibration_tests.csv, validity_flags.csv |
| `results.tex` | `tab:res_point` | 21.8.2026 | v1+v2 | unverändert | ☐ | cells.csv |
| `results.tex` | `fig:skill_heatmap` | 21.8.2026 | v2 | unverändert | ☐ | ranking_all.csv |
| `results.tex` | `tab:res_ranking` | 21.8.2026 | v1+v2 | unverändert | ☐ | ranking_all.csv |
| `results.tex` | `tab:res_tirex2_cp` | 21.8.2026 | v1+v2 | unverändert | ☐ | ranking_all.csv |
| `results.tex` | `tab:res_percountry` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv |
| `results.tex` | `tab:rq3_results` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | dm_contrasts.csv |
| `results.tex` | `tab:rq1_results` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | dm_contrasts.csv |
| `results.tex` | `tab:res_contrasts` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | dm_contrasts.csv |
| `results.tex` | `tab:rq2_results` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | dm_contrasts.csv |
| `results.tex` | `tab:res_rq2_lstmG` | 21.8.2026 | v1+v2 | unverändert | ☐ | dm_percountry.csv, dm_contrasts.csv |
| `results.tex` | `tab:res_cov` | 21.8.2026 | v1+v2 | unverändert | ☐ | cells.csv |
| `results.tex` | `tab:res_xm` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | dm_crossmodel.csv, dm_crossmodel_percountry.csv |
| `results.tex` | `tab:res_mcs` | 21.8.2026 | v1+v2 | unverändert | ☐ | mcs_results.csv, run_metadata.json |
| `results.tex` | `tab:res_regime` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | regime_results.csv, stress_drop.csv |
| `results.tex` | `tab:res_robustness_pool` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv |
| `results.tex` | `tab:res_robustness_ie` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | robustness_ex_ie.csv |
| `results.tex` | `tab:res_robustness_seeds` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | ranking_all.csv |
| `results.tex` | `tab:res_robustness_burnin` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv |
| `limitations.tex` | `tab:lim_point` | 21.8.2026 | v1+v2 | unverändert | ☐ | cells.csv, dm_percountry.csv |
| `limitations.tex` | `tab:lim_dq` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | calibration_tests.csv |
| `limitations.tex` | `tab:lim_amendments` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_frozen` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_protocol` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_nn_space` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_lgbm_space` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_frozen_nn` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_frozen_lgbm` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_frozen_arma` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | — |
| `appendix.tex` | `tab:app_country` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv, validity_flags.csv |
| `appendix.tex` | `tab:app_tail` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv |
| `appendix.tex` | `tab:app_pool` | 21.8.2026 | v1 (v2 unverändert) | unverändert | ☐ | cells.csv |
| `appendix.tex` | `tab:app_fragility` | 21.8.2026 | v1+v2 | unverändert | ☐ | ranking_all.csv, robustness_ex_ie.csv |

**Prompt-Fassungen**

| Fassung | Datum | Aenderung | Galt fuer |
|---|---|---|---|
| v1 | 21.8.2026 | Erstfassung | alle 37 Tabellen |
| v2 | 21.8.2026 | Ergaenzt die Stilregel "Stil aus dem Fliesstext wird uebernommen"; die Ausgabe verlangt keine Kontrollliste mehr | alle 37 Tabellen erneut geprueft, davon 10 textlich geaendert, und `fig:skill_heatmap` direkt unter v2 erzeugt (`tab:publication-lags`, `tab:cp_methods`, `tab:res_point`, `tab:res_ranking`, `tab:res_tirex2_cp`, `tab:res_rq2_lstmG`, `tab:res_cov`, `tab:res_mcs`, `tab:lim_point`, `tab:app_fragility`) |

**Formale Pruefung der Endfassung (automatisiert):** alle Captions <= 25 Woerter
(Mittel 21,4), alle Notes <= 90 Woerter, hoechstens ein Semikolon je Notes-Block,
kein Satz ueber 28 Woerter (Mittel 14,9), keine britische Schreibung, keine
"rather than"- oder "not X but Y"-Konstruktion, alle `\cref`-Ziele aufloesbar.

**Hinweis zur Erklaerung:** Ist die Nachbearbeitung "unveraendert" oder
"redaktionell", gelten die Texte als *ohne substantielle Aenderungen
uebernommen* - dann muessen dieser Prompt, Produktname und Version/Datum im
Anhang angegeben werden. Bei inhaltlicher Ueberarbeitung genuegt die Nennung des
Tools mit Angabe, wo und wie es verwendet wurde.
