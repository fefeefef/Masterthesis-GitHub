# Prompt: Tabellen-Captions und Notes

**Zweck:** Erzeugung der Tabellenbeschreibungen in `sections/results.tex` und `sections/limitations.tex`.
**Für den Anhang „Übersicht verwendeter Hilfsmittel" aufbewahren.**

---

## VOR DER AUSFÜHRUNG

1. **Alte Captions und Notes löschen.** Sonst stimmt die Prämisse des Prompts nicht und die spätere Dokumentation wäre unrichtig. `\caption{}` auf einen Platzhalter reduzieren, `Notes.`-Minipages ganz entfernen.
2. **Protokoll unten ausfüllen** — Produktname, Versionsnummer, Datum. Nachträglich nicht rekonstruierbar.
3. **Tabellenweise ausführen**, nicht die ganze Arbeit auf einmal. Ein Durchlauf pro Tabelle, mit dem zugehörigen Abschnitt als Kontext. Ergebnisse werden sonst inkonsistent und Zahlen unzuverlässig.

---

## PROMPT v1 (Erstfassung, 21.8.2026 -- ueberholt)

```
Du erstellst die Beschreibung für EINE Tabelle meiner Masterarbeit
(TU Wien, MSc Financial & Actuarial Mathematics; Arbeitssprache Englisch).

Ich gebe dir drei Dinge:
  (1) den vollständigen Fließtext des Abschnitts, in dem die Tabelle steht,
  (2) den LaTeX-Code der Tabelle selbst,
  (3) die zugrundeliegende CSV aus results/tables/.

AUFGABE
Schreibe genau zwei Dinge, auf Englisch:
  A) einen \caption{}-Text, höchstens 25 Wörter
  B) einen Notes-Block, höchstens 90 Wörter
Bei einer Tabelle ohne Panels und ohne Marker entfällt B ersatzlos.

WAS IN DIE NOTES GEHÖRT — und sonst nichts:
  1. Definition jeder Spalte, deren Bedeutung sich nicht aus der
     Kopfzeile ergibt UND die nicht bereits in Kapitel 7 (Evaluation
     Framework) definiert ist. Ist sie dort definiert: nur \cref-Verweis,
     keine Wiederholung.
  2. Stichprobe und Einheit der Zelle (z. B. "127 monthly observations
     per country, ten countries").
  3. Verwendeter Test samt Korrektur, in einem Satz.
  4. Vorzeichenkonvention — welches Vorzeichen bedeutet welches Ergebnis.
     Das ist verpflichtend, sobald die Tabelle eine Größe zeigt, bei der
     das nicht offensichtlich ist.
  5. Legende der Marker (z. B. was † bedeutet).
  6. Provenienz, falls sie zum Nachvollziehen nötig ist (etwa welche
     Seeds welcher Zeile zugrunde liegen).

DREI HARTE VERBOTE
  * KEINE Zahl, die bereits im mitgelieferten Fließtext steht. Prüfe
    jede Zahl gegen den Fließtext, bevor du sie schreibst. Zahlen, die
    nur in der Tabelle stehen, sind erlaubt, wenn sie zur Definition
    einer Spalte nötig sind.
  * KEINE Interpretation, Bewertung oder Einordnung. Keine Sätze über
    das, was das Ergebnis bedeutet, warum es so ausfällt oder wie stark
    es ist. Die Beschreibung sagt, wie man die Tabelle LIEST, nicht was
    sie ZEIGT.
  * KEINE Zahl, die du nicht in der Tabelle oder der CSV verifizieren
    kannst. Erfinde nichts, runde nichts um, rechne nichts nach.

STIL — halte dich messbar an diese Vorgaben:
  * Amerikanische Schreibung: normalized, behavior, favor, optimization.
    NICHT normalised, behaviour, favour, per cent.
  * Höchstens ein Semikolon pro 90 Wörter.
  * Keine Geviertstriche (---).
  * Keine Dreierparallelen der Form "A, B, and C" mit gleichgebautem
    Nebensatz an jedem Glied.
  * Keine "not X but Y"- und "rather than"-Konstruktionen.
  * Durchschnittliche Satzlänge um 18 Wörter, höchstens 28.
  * Schlichte Aussagesätze. Wo es passt, mit "We", "This" oder "The"
    beginnen.

KONVENTIONEN DER ARBEIT
  * Übernimm die Notation des Fließtextes unverändert: d_norm, Winkler
    skill, admissible, package, cell, stream, gate.
  * Referenzen als \cref{}, Modellnamen in \texttt{}.
  * Die Arbeit verwendet mehrere unterschiedliche Vorzeichenkonventionen
    nebeneinander. Prüfe im Fließtext, welche für DIESE Tabelle gilt,
    und übernimm sie wörtlich. Rate nicht.

AUSGABE
  1. Der fertige LaTeX-Code für \caption{} und, falls nötig, den
     Notes-Block.
  2. Darunter eine kurze Liste: welche Angaben du bewusst WEGGELASSEN
     hast, weil sie schon im Fließtext stehen — mit Zitat der jeweiligen
     Fließtextstelle. Diese Liste ist meine Kontrolle. Ohne sie ist die
     Antwort unvollständig.
  3. Falls dir im Fließtext eine Definition fehlt, die du für die Notes
     bräuchtest: sag es, statt sie zu erfinden.
```

---

## PROMPT v2 (gueltige Fassung, 21.8.2026)

Unterschied zu v1: die Stilregel *"Stil aus dem Fliesstext wird uebernommen"*
kommt dazu, und die Ausgabe verlangt keine Kontrollliste der weggelassenen
Angaben mehr. Inhaltlich sind die Regeln identisch.

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
