# KI-Textanalyse & Handlungsempfehlung — Masterarbeit TU Wien

**Arbeit:** *Conformal Prediction Intervals for Sovereign Credit Spread Forecasting: An xLSTM and Foundation-Model Study*
**Stand des Repos:** Commit `a85012e` (21.08.2026) · **Analyse erstellt:** 21.08.2026

---

## 0. Kurzfassung

| | |
|---|---|
| **Analysierter Umfang** | 8 in `main.tex` eingebundene Kapitel, ca. 60.000 Wörter Quelltext |
| **Klar KI-geschrieben (Prio 1)** | ~4.700 Wörter — die Tabellen-Captions und `Notes.`-Blöcke in `results.tex` und `limitations.tex` |
| **Klar KI-geschrieben (Prio 1b)** | ~1.150 Wörter — **Kapitel 5 „Experimental Design", Zeile 1–160** (nachgetragen, siehe §4.1b) |
| **Wahrscheinlich KI-Entwurf (Prio 2)** | ~2.400 Wörter — Fließtext in `conformal.tex` ab Abschnitt „Group (b)" |
| **Bereits einmal überarbeitet (Prio 3)** | ~1.200 Wörter — die in Commit `1450e0e` angefassten Stellen |
| **Erkennbar deine eigene Stimme** | Der gesamte Fließtext in `results.tex`, `limitations.tex`, `evaluation.tex` sowie `conformal.tex` bis Zeile 243 |
| **Empfehlung** | Prio 1 und 2 **umschreiben**, danach KI im Anhang „Übersicht verwendeter Hilfsmittel" deklarieren. Nicht: unverändert lassen und einzeln als Zitat markieren. |

> **Nachtrag 21.08.2026 (zweite Analyserunde).** Die erste Fassung dieses Dokuments hat **Kapitel 5 „Experimental Design" übersehen**. Es trägt die dichteste KI-Signatur der gesamten Arbeit — 7 von 11 Absätzen, und die Zeilen 1–160 wurden seit dem Erst-Import nie überarbeitet. Grund für den Fehlschluss: Kapitel 5 enthält **keine einzige** britische Schreibung, mein Hauptmarker war dort blind; zusätzlich hatte ich die Datei als „behandelt" abgelegt, weil Commit `1450e0e` sie angefasst hatte — der Commit betraf aber nur die Zeilen 161–195. Details in §1.5 und §4.1b.

**Wichtig vorweg:** Es geht ausschließlich um **Formulierungen**. Die Zahlen, Tabellen und Ergebnisse stammen nachweislich aus deinen eigenen Läufen (`results/tables/*.csv`, `run_metadata.json`). Das ist kein Ghostwriting der Forschung, sondern KI-gestützte Textproduktion — rechtlich eine völlig andere Kategorie und deutlich unproblematischer. Ebenfalls geprüft und **sauber**: alle 52 zitierten BibTeX-Keys existieren in `references.bib`, es gibt **keine halluzinierten Quellen**.

---

## 1. Wie ich die Stellen identifiziert habe

Ich habe mich nicht auf „Bauchgefühl" verlassen, sondern auf vier unabhängige Indizien. Entscheidend ist, dass sie alle auf **dieselben** Textblöcke zeigen.

### 1.1 Indiz A — Der Rechtschreib-Fingerabdruck (stärkstes Signal)

Dein eigener Text verwendet durchgängig **amerikanische** Schreibung (`normalized`, `optimization`, `realized`, `behavior`, `%`). Die verdächtigen Blöcke verwenden durchgängig **britische** Schreibung (`normalised`, `favour`, `behaviour`, `per cent`, `modelling`).

Verteilung der britischen Formen über den gesamten Text:

| Ort | Treffer |
|---|---|
| **Innerhalb von `\caption{}` / `Notes.`-Blöcken** | **20** |
| Im laufenden Fließtext | 4 (alle „realised", im Regime-Kapitel aus den Tabellen übernommen) |

Ein Mensch wechselt nicht systematisch die Orthographie-Konvention, sobald er eine Tabellenunterschrift schreibt. Ein zweiter Textgenerator tut genau das. Dichte-Vergleich:

| Datei / Bereich | Wörter | brit. Formen pro 1.000 W |
|---|---|---|
| `results.tex` — Fließtext | 4.911 | 1,02 |
| `results.tex` — Tabellenblöcke | 4.708 | **3,61** |
| `limitations.tex` — Fließtext | 2.561 | 0,00 |
| `limitations.tex` — Tabellenblöcke | 755 | **3,97** |

Dazu: 25 der 26 Geviertstriche (`---`) in `results.tex` stehen ebenfalls in den Tabellenblöcken.

### 1.2 Indiz B — Bruch im Stil und in der Fehlerdichte

Dein Fließtext hat charakteristische, wiedererkennbare Merkmale — kleine grammatische Unsauberkeiten, deutsche Interferenz („terzile", „terzil"), uneinheitliche Großschreibung („winkler score"), und Bezeichner in Textmathematik (`$d\_norm$`, `$share\_a\_better$`, `$tirex2/ZS/cqr\_static$`). Dazu wiederkehrende eigene Wendungen: *„We wanted to find out whether…"*, *„We conclude that…"*, *„Now we take a look at…"*.

In den Tabellenblöcken: **null** solcher Merkmale, dafür perfekt parallel gebaute Sätze und vorsichtshalber eingebaute Lesehinweise, die niemand schreibt, der die Tabelle gerade selbst gebaut hat:

> „The two quantities carry opposite sign conventions and must not be read against each other."
> „That number hides what happens one level down."
> „…because a difference of medians is not the median of differences."

Zum Vergleich zwei Sätze über **dasselbe** Thema, wenige Zeilen auseinander:

| | |
|---|---|
| **Dein Fließtext** (`results.tex`, §RQ1) | „In the comparison GF vs. G, so finetuned after global training vs. just global training, the fine tuned models had an overall, but not statistically significant, better performance with a $d\_norm$ of -0.006." |
| **Notes-Block** derselben Tabelle | „The three contrasts form a coherent chain: pooling costs $+0.017$, fine-tuning recovers $-0.006$, and $+0.016$ of the deficit against the local model remains (medians do not add exactly)." |

Der TU-Wien-Plagiatsleitfaden nennt genau das als erstes Erkennungsmerkmal: *„Stilwechsel bzw. Stilbrüche"* und *„Verwendung von außergewöhnlichem Vokabular"* (Abschnitt 3.2).

### 1.3 Indiz C — Spuren im Quelltext selbst

`sections/conformal.tex` enthält im Kopf eine Arbeitsanweisung auf Deutsch, die eindeutig für ein KI-System formuliert ist:

```
%  SO ARBEITEST DU MIT DIESER DATEI:
%    * Die Mathematik ist fertig und geprüft — Formeln NICHT umschreiben.
%    * Der Fließtext ist bewusst dünn. Überall wo "% SCHREIBE:" steht,
%      gehört Prosa hin; darunter steht auf Deutsch exakt, was inhaltlich
%      hineingehört und was NICHT behauptet werden darf.
```

Daraus ergibt sich eine **saubere Trennlinie in dieser Datei**:

- **Zeile 34–243:** Zu jedem Absatz existiert ein `% SCHREIBE:`-Kommentar *und* darüber steht Prosa in deiner Stimme → **du hast die Anweisungen selbst ausformuliert.** Diese Stellen sind unkritisch.
- **Ab Zeile 243:** Dort steht dein Marker `%bis hier gemacht`. Danach gibt es **keine `% SCHREIBE:`-Blöcke mehr**, dafür durchgehend polierte Prosa → **hier hat die KI direkt geschrieben, und du bist nicht mehr durchgegangen.**

Der Marker `%bis hier gemacht` ist damit faktisch deine eigene Notiz darüber, wo dein Überarbeitungsstand endet.

### 1.5 Indiz E — Interpunktions- und Satzbauprofil (zweite Runde)

Die Orthographie versagt dort, wo der KI-Text keine `-ise`/`-our`-Wörter enthält. Ein Marker, der das übersteht, ist die **Semikolon- und Satzbaudichte**. Referenzwerte pro 1.000 Wörter:

| | Semikolon | Triade „A, B, and C" | Sätze mit „We/This/So…" |
|---|---|---|---|
| **[KI]** Captions + Notes | **10,78** | 0,58 | 0,29 |
| **[DU]** Fließtext Results + Limitations | **1,68** | 0,26 | 4,53 |

Gemessen über alle Kapitel (nur Fließtext, Tabellen ausgeschlossen):

| Kapitel | Semikolon | Triade | „We/This…" | Absätze mit KI-Score ≥4/8 |
|---|---|---|---|---|
| **experimental_design** | **6,59** | **4,39** | **0,73** | **7 von 11 (64 %)** |
| models | 6,37 | 2,65 | 6,37 | 2 von 14 (14 %) |
| evaluation | 3,45 | 3,45 | 4,31 | 3 von 7 (43 %)¹ |
| data | 3,64 | 1,46 | 3,64 | 3 von 10 (30 %) |
| conformal | 2,80 | 0,93 | 3,42 | 0 von 21² |
| limitations | 2,28 | 0,38 | 3,80 | 2 von 30 (7 %) |
| results | 1,37 | 0,20 | 4,90 | 0 von 39 |

¹ Zwei der drei Treffer sind „where $X$ is …, $Y$ is …, and $Z$ is …"-Erklärungen nach Formeln — strukturbedingter Fehlalarm, kein KI-Indiz.
² `conformal.tex` erscheint hier unauffällig, weil der Wert über die **ganze** Datei gemittelt ist; der kritische Teil ab Zeile 243 bleibt trotzdem auf der Liste (§4.2).

**Kapitel 5 ist das einzige Kapitel, in dem die Mehrheit der Absätze die Signatur trägt.** Besonders aussagekräftig ist die letzte Spalte: Sätze, die mit „We", „This" oder „So" beginnen, sind bei dir mit 4,53 pro 1.000 Wörter ein Grundmuster. In Kapitel 5 liegen sie bei 0,73 — praktisch auf dem Niveau der Notes-Blöcke (0,29). Dieses Merkmal überlebt eine Umformulierung auf Satzebene nicht zufällig.

**Höchster Einzelwert der gesamten Arbeit:** `models.tex`, §6.4 xLSTM, der Absatz ab *„xLSTM is the state-of-the-art extension of LSTM."* — 19,5 Semikolons und 32,5 `which` pro 1.000 Wörter, Ø 25 Wörter pro Satz. Die drei parallelen „…, which leads to …"-Klauseln über die drei LSTM-Schwächen sind ein Lehrbuchmuster.

### 1.4 Indiz D — Die Git-Historie

```
1450e0e  Rewrite polished passages into own voice; fix typos in new zero-tuned text
         Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
9b7a79f  Add Results chapter skeleton and diagnostics artefacts
         Co-Authored-By: Claude <noreply@anthropic.com>
```

Commit `1450e0e` ist ein Geschenk für diese Analyse: Der Diff zeigt exakt, welche Passagen du selbst als „polished" (= KI) erkannt und angefasst hast. Die entfernten Zeilen tragen die typischen Marker — `specialised`, `tokenise`, `maximise`, *„This underscores the narrative of our paper"*, *„closing the ladder of models"*, *„is not exercised in our setup"*.

**Aber:** Die Überarbeitung war überwiegend oberflächliche Umstellung, kein Neuschreiben. Beispiel:

- vorher: „`spci` reverses the usual conformal logic."
- nachher: „`spci` turns the usual conformal logic around."

Satzbau, Reihenfolge und Argumentation blieben identisch. Ob das eine *„substantielle Änderung"* im Sinne der TU-Wien-Erklärung ist, würde ich verneinen (siehe §2.3). Deshalb steht dieser Block als Priorität 3 weiter unten trotzdem auf der Liste.

---

## 2. Prüfung gegen die TU-Wien-Richtlinien

### 2.1 Die vier relevanten Rechtsquellen

| # | Quelle | Was sie regelt |
|---|---|---|
| **R1** | [§ 51 Abs 2 Z 7, 8 und 12 UG 2002](https://www.ris.bka.gv.at/GeltendeFassung.wxe?Abfrage=Bundesnormen&Gesetzesnummer=20002128) | Masterarbeiten sind *„selbst- und eigenständig anzufertigende schriftliche Arbeiten"* |
| **R2** | [Leitfaden zum Umgang mit Plagiaten an der TU Wien](https://www.tuwien.at/fileadmin/Assets/studium/Zentrum_fuer_strategische_Lehrentwicklung/Dokumente/ZLLRM/Lehre_-_Leitfaden_zum_Umgang_mit_Plagiaten.pdf) (Mitteilungsblatt Nr. 09/2014, lfd. Nr. 90) | Definition Plagiat; Grenze zulässiger fremder Hilfe |
| **R3** | [TU Wien — KI im Studium](https://www.tuwien.at/studium/studieren-an-der-tuw/ki-im-studium) | KI-spezifische Kennzeichnungs- und Offenlegungspflichten |
| **R4** | Standard-Formular „Erklärung zur Verfassung der Arbeit" (aktuelle Fassung, in allen TU-Wien-Abschlussarbeiten ab ~2024) | Was du eidesstattlich unterschreibst |

### 2.2 Was die Quellen konkret sagen

**R2, Abschnitt 2 „Wieviel fremde Hilfe ist erlaubt?"** — die Grenzlinie:

> „Fremde Hilfe bei der Anfertigung von schriftlichen Arbeiten hat jedenfalls dort ihre Grenze, wo die Korrektur […] von formalen Kriterien wie bspw. Orthographie überschritten wird und **inhaltliche Korrekturen bzw. Überarbeitungen durch jemanden anderen** als durch die Studierende/den Studierenden vorgenommen werden."

Dieser Leitfaden ist von 2014 und meint Menschen, nicht KI. Er definiert aber den Maßstab für „Eigenständigkeit", auf den R3 und R4 aufsetzen: **formale Hilfe ja, inhaltliche Textproduktion durch Dritte nein.**

**R3 — TU Wien „KI im Studium"**, die vier tragenden Sätze:

> „Generative Systeme müssen als Hilfsmittel dokumentiert werden und auch die Stellen, an denen KI's eingesetzt wurden, sind entsprechend zu markieren."
> „Für wesentliche Beiträge sind die verwendeten generativen KI-Systeme sowie die Prompts und das Datum der Abfrage anzugeben."
> „Wenn generative KI-Texte **ohne substanzielle Änderungen** in eigenen Arbeiten verwendet werden, sollten sie ähnlich wie **Zitate** aus anderen Quellen behandelt werden."
> „Die nicht offengelegte Verwendung von generativer KI in einer studentischen Arbeit **kommt einem Plagiat gleich**."

**R4 — Die Erklärung, die du unterschreiben wirst** (Wortlaut aus aktuellen TU-Wien-Arbeiten in reposiTUm):

> „Ich erkläre weiters, dass ich mich generativer KI-Tools **lediglich als Hilfsmittel** bedient habe und in der vorliegenden Arbeit **mein gestalterischer Einfluss überwiegt**. Im Anhang „Übersicht verwendeter Hilfsmittel" habe ich **alle** generativen KI-Tools gelistet, die verwendet wurden, und angegeben, **wo und wie** sie verwendet wurden. Für Textpassagen, die **ohne substantielle Änderungen** übernommen wurden, habe ich jeweils die von mir formulierten **Eingaben (Prompts)** und die verwendete IT-Anwendung mit ihrem **Produktnamen und Versionsnummer/Datum** angegeben."

### 2.3 Die Ableitung

**Schritt 1 — Ist KI-Text an der TU Wien verboten? Nein.**
R3 und R4 gehen ausdrücklich davon aus, dass KI eingesetzt wird. Verboten ist nur die **nicht offengelegte** Verwendung. Du hast also grundsätzlich zwei zulässige Wege.

**Schritt 2 — Weg A: Behalten und als Zitat markieren.**
Nach R3/R4 zulässig, wenn du (a) im Anhang alle Tools listest, (b) *jede* unverändert übernommene Passage kennzeichnest und (c) dazu Prompt, Produktname und Version/Datum angibst.

Formal korrekt — praktisch aber aus drei Gründen schlecht:

1. **Die „gestalterischer Einfluss überwiegt"-Bedingung wird eng.** Es geht nicht um irgendwelche 4.700 Wörter, sondern fast ausschließlich um **interpretierende** Passagen: die Notes-Blöcke erklären, wie deine eigenen Zahlen zu lesen sind, warum zwei Kennzahlen entgegengesetzte Vorzeichenkonventionen haben, warum ein Median nicht additiv ist. Das ist genau der Teil, der bewertet wird. Wenn du ihn als Fremdleistung markierst, markierst du deinen Interpretationsbeitrag als nicht von dir.
2. **Es lenkt maximale Aufmerksamkeit auf die Stellen.** Ein Gutachter, der in Kapitel 8 zehnmal „(generiert mit …, Prompt: …)" liest, liest den Rest der Arbeit anders.
3. **Verteidigungsrisiko.** Bei der Defensio musst du jede Aussage erklären können. In den Notes stehen nicht-triviale Behauptungen, z. B. dass die 140 Kovariaten-Tests bewusst als eigene FDR-Familie geführt werden, oder dass `saocp`'s konservative Coverage aus der Domänendefinition folgt und kein Befund ist. Wenn du diese Sätze nicht selbst formuliert hast, musst du sie trotzdem verteidigen.

**Schritt 3 — Weg B: Substantiell umschreiben, dann pauschal deklarieren.**
Wenn du die Passagen wirklich neu schreibst — nicht paraphrasierst, sondern aus den Zahlen heraus selbst formulierst — greift die Zitatpflicht aus R3/R4 nicht mehr, weil es keine „ohne substantielle Änderungen übernommenen Textpassagen" mehr gibt. Es bleibt die Pflicht aus R4 Satz 2: Tool im Anhang listen, mit Angabe wo und wie. Das ist eine halbe Seite Anhang statt zehn Markierungen im Fließtext.

Zusätzlich erfüllt Weg B den Maßstab aus R2 sauber: Die KI war dann Hilfsmittel bei Entwurf und Struktur, die inhaltliche Ausformulierung liegt bei dir.

**Schritt 4 — Zur bereits erfolgten Überarbeitung (`1450e0e`).**
Der Diff zeigt Umstellungen auf Satzebene bei identischem Aufbau und identischer Argumentationsführung. Ich würde das nicht als *„substantielle Änderung"* verteidigen wollen — der Text bleibt in Struktur und Gedankenführung der KI-Entwurf. Deshalb: zweiter Durchgang bei den in §4.3 gelisteten Stellen, dieser Mal mit dem Ziel, den Absatz aus den Fakten neu aufzubauen statt Sätze zu drehen.

### 2.4 Handlungsempfehlung

> **Weg B für Priorität 1 und 2, zweiter Durchgang bei Priorität 3, danach vollständige Deklaration im Anhang.**

Konkret:

1. **Priorität 1 umschreiben** (§4.1) — die Captions und Notes-Blöcke in `results.tex`/`limitations.tex`. Das ist der Kern, weil es interpretierender Text ist. Methode: Tabelle und CSV danebenlegen, Caption ohne Blick auf die alte Fassung neu schreiben, in dem Ton, den dein Fließtext ohnehin hat. Kürzer ist hier auch besser — mehrere Notes-Blöcke sind mit 200+ Wörtern für eine Tabellenunterschrift ohnehin überlang.
2. **Priorität 2 umschreiben** (§4.2) — `conformal.tex` ab Zeile 243. Geringeres inhaltliches Risiko (Beschreibung publizierter Verfahren), aber der größte zusammenhängende unbearbeitete Block.
3. **Priorität 3: zweiter Durchgang** (§4.3).
4. **Anhang „Übersicht verwendeter Hilfsmittel" anlegen.** Vorlage in §5.
5. **Aufräumen vor der Abgabe** (§6) — die deutschen KI-Arbeitskommentare im `.tex`-Quelltext und die `Co-Authored-By: Claude`-Commits sind derzeit öffentlich auf GitHub.

**Was du *nicht* tun musst:** Die Tabellen, Zahlen und Auswertungen anfassen. Die sind deine. Ebenso unkritisch: LaTeX-Formatierung, Code-Hilfe, Rechtschreibkorrektur — das fällt unter „formale Kriterien" nach R2 und ist ohnehin zulässig, wird aber der Vollständigkeit halber im Anhang mitgelistet.

---

## 3. Gegenprüfung mit einem kostenlosen Tool

### 3.1 Empfehlung

| Tool | Link | Anmerkung |
|---|---|---|
| **QuillBot AI Detector** | `quillbot.com/ai-content-detector` | Beste Wahl: kostenlos, kein Login, großzügiges Zeichenlimit |
| **Scribbr KI-Detektor** | `scribbr.de/ki-detektor/` | Kostenlos, kann auch Deutsch, gute Absatz-Markierung |
| **GPTZero** | `gptzero.me` | Free-Tier begrenzt; markiert Sätze einzeln |
| **Sapling** | `sapling.ai/ai-content-detector` | Nur ~2.000 Zeichen, aber pro Satz farbcodiert |

### 3.2 Zwingend nötige Vorsichtsmaßnahme

**KI-Detektoren sind bei deinem Textprofil systematisch unzuverlässig.** Die Standardreferenz dazu ist Liang et al., *„GPT detectors are biased against non-native English writers"* (Patterns 4(7), 2023): Über 61 % der von Nicht-Muttersprachlern verfassten TOEFL-Essays wurden fälschlich als KI eingestuft, während Texte von Muttersprachlern nahezu fehlerfrei erkannt wurden. Der Grund ist genau das Merkmal, das dein eigener Text hat: geringere lexikalische Varianz und einfachere Satzstrukturen.

Für dich heißt das konkret: **Die Detektoren werden vermutlich deinen eigenen Fließtext stärker anschlagen lassen als die tatsächlich KI-geschriebenen Notes-Blöcke.** Nimm sie nur als Zweitmeinung.

### 3.3 So testest du sinnvoll — mit Kontrollprobe

Ein Detektor-Wert allein sagt nichts. Ein *Vergleich* sagt viel. Vorgehen:

1. **Kontrollprobe:** Nimm einen Absatz, von dem du sicher weißt, dass er von dir ist — z. B. `results.tex`, §RQ2, beginnend mit *„We wanted to find out whether adaptive quantile outputs…"*. Notiere den Wert. Das ist deine Nulllinie.
2. **Verdachtsprobe:** Nimm den Notes-Block unter `tab:res_cov` (§4.1, Nr. 6). Notiere den Wert.
3. **Vergleiche.** Nur wenn die Verdachtsprobe *deutlich* höher liegt als deine Nulllinie, bestätigt der Detektor meine Analyse. Ein hoher Absolutwert bei beiden bedeutet nur, dass der Detektor deinen Schreibstil generell nicht mag.
4. Wiederhole mit zwei bis drei weiteren Paaren.

**LaTeX vorher entfernen** — `\texttt{}`, `\cref{}` und Formeln verzerren jeden Detektor. Für Konvertierung: `detexify`, `pandoc`, oder einfach von Hand herauslöschen.

---

## 4. Die Liste — welche Texte du überarbeiten sollst

Format je Eintrag: **Kapitel · Datei/Ort · Anfangssatz → Endsatz.** Anfangs- und Endsatz sind so gewählt, dass du sie per Volltextsuche (`Strg+F`) im Editor oder in Overleaf direkt findest.

### 4.1 PRIORITÄT 1 — klar KI-geschrieben, inhaltlich tragend

Alle folgenden Blöcke sind Tabellen-`\caption{}` bzw. `\textit{Notes.}`-Absätze. Sie stehen im PDF unter bzw. über der jeweiligen Tabelle.

---

**① Kapitel 8 „Results" → 8.3 Main ranking: interval skill**
Datei: `sections/results.tex`, Zeile 504 · Caption von `tab:res_ranking` (70 Wörter)
- **Anfang:** „Best admissible package per model family, ranked by median Winkler skill against RW/L/native."
- **Ende:** „The ordering of the leading group is unaffected, but `xlstm` then ranks below the Random Walk."

---

**② Kapitel 8 → 8.3 Main ranking: interval skill**
Datei: `sections/results.tex`, Zeile 565 · Caption von `tab:res_percountry` (47 Wörter)
- **Anfang:** „Per-country skill and coverage of the three leading packages and of the best classical benchmark."
- **Ende:** „† marks a country in which the calibration null is rejected (FDR q<0.10), i.e. a cell that fails the admissibility gate."

---

**③ Kapitel 8 → 8.5 RQ1: pooling across countries and fine-tuning**
Datei: `sections/results.tex`, Zeile 841 · Caption von `tab:rq1_results` — **189 Wörter, der längste Caption der Arbeit**
- **Anfang:** „Results for RQ1 (pooling and fine-tuning). Panels A and B report normalised Diebold--Mariano loss differentials…"
- **Ende:** „† marks the post hoc contrast GF vs. L, which is not part of the pre-registered set and forms its own FDR family."

---

**④ Kapitel 8 → 8.5 RQ1**
Datei: `sections/results.tex`, Zeile 900 · `Notes.`-Block unter `tab:rq1_results`
- **Anfang:** „**Notes.** LightGBM has no `GF` regime, so only the `G`-vs-`L` contrast exists for it; it is deterministic (single seed), so seed stability does not apply."
- **Ende:** „…because the per-country DM output does not carry a seed identifier; the effect columns are per seed."

---

**⑤ Kapitel 8 → 8.6 RQ2: adaptive versus constant interval width**
Datei: `sections/results.tex`, Zeile 1076 · Caption von `tab:rq2_results` (142 Wörter)
- **Anfang:** „Results for RQ2 (adaptive versus constant interval width). Panel A reports normalised Diebold--Mariano loss differentials…"
- **Ende:** „† marks a package whose seed range exceeds its own skill."

---

**⑥ Kapitel 8 → 8.6 RQ2**
Datei: `sections/results.tex`, Zeile 1137 · `Notes.`-Block unter `tab:rq2_results`
- **Anfang:** „**Notes.** LightGBM is deterministic (single seed), so seed stability does not apply and it has no `GF` regime…"
- **Ende:** „Under the pre-registered seed rule no rank statement may be made for it."

---

**⑦ Kapitel 8 → 8.6 RQ2**
Datei: `sections/results.tex`, Zeile 1259 + 1286 · Caption **und** `Notes.`-Block von `tab:res_rq2_lstmG`
- **Caption-Anfang:** „Country-level breakdown of the `lstm`/G contrast between the adaptive-width model and its constant-width twin."
- **Notes-Anfang:** „*Notes.* A is `lstm`/G, B is `lstm_const`/G."
- **Ende:** „…that number is the median of the 42 per-contrast medians taken across countries."

---

**⑧ Kapitel 8 → 8.7 Do covariates matter? The TiRex-2 contrast**
Datei: `sections/results.tex`, Zeile 1432 · `Notes.`-Block unter `tab:res_cov` — **der längste zusammenhängende KI-Block der Arbeit (~430 Wörter, drei Absätze)**
- **Anfang:** „*Notes.* uni is `tirex2`, cov is `tirex2_cov`. Both are the same pre-trained model in the ZS regime under seed 42…"
- **Ende:** „Coverage falls in nine countries, from a median of 0.843 to 0.831, which moves it towards the nominal 0.80 rather than away from it."
- ⚠️ Enthält die methodisch heikelste Aussage der Arbeit: die Begründung, warum die 140 Kovariaten-Tests als eigene FDR-Familie geführt werden statt in der Familie der 12.490 Zellen. Das musst du verteidigen können.

---

**⑨ Kapitel 8 → 8.8 Cross-model comparison and the Model Confidence Set**
Datei: `sections/results.tex`, Zeile 1565 + 1594 · Caption **und** `Notes.`-Block von `tab:res_xm`
- **Caption-Anfang:** „Pairwise cross-model comparison of the twelve best admissible packages, one per model family."
- **Notes-Anfang:** „*Notes.* Each package is the best admissible one of its family, and the full specification of regime, seed and recalibration method is in …"
- **Ende:** „…so the comparison is optimistic in the sense of \cref{sec:exp:selection}."
- ⚠️ Der Querverweis `sec:exp:selection` existiert nirgends in der Arbeit — typischer Fehler in KI-Entwürfen. Beim Umschreiben mit korrigieren.

---

**⑩ Kapitel 8 → 8.8 Cross-model comparison and the MCS**
Datei: `sections/results.tex`, Zeile 1675 · `Notes.`-Block unter `tab:res_mcs`
- **Anfang:** „*Notes.* Hansen's Model Confidence Set at α=0.10, with the T_max statistic and a moving-block bootstrap of 1000 replications at block length six…"
- **Ende:** „Finland retains all 26 at the lowest p of 0.103, while the Netherlands excludes two at the highest p of 0.406."

---

**⑪ Kapitel 8 → 8.9 Regime-conditional behavior**
Datei: `sections/results.tex`, Zeile 1917 + 1981 · Caption **und** `Notes.`-Block von `tab:res_regime`
- **Caption-Anfang:** „Regime-conditional behaviour on the two volatility axes."  ← beachte: `behaviour`, während deine Überschrift „Regime-conditional beha**v**ior" schreibt. Der Bruch steht direkt nebeneinander.
- **Notes-Anfang:** „*Notes.* Both axes use information available before the forecast month."
- **Ende:** „…and its group medians are not comparable with the levels in Panel C."

---

**⑫ Kapitel 8 → 8.10 Robustness**
Datei: `sections/results.tex`, Zeilen 2274 / 2330 / 2360 · drei Captions
- `tab:res_robustness_pool` — **Anfang:** „Calibration-pool robustness. Panel A: classification of the 14 CP methods by whether they read the historical calibration-score pool at all."
- `tab:res_robustness_ie` — **Anfang:** „Sensitivity of the combination ranking to excluding Ireland."
- `tab:res_robustness_seeds` — **Anfang:** „Seed fragility of the neural-network combinations…" **Ende:** „Panel B: two worked examples showing the per-seed Winkler skill."

---

**⑬ Kapitel 9 „Limitations" → 9.1 Pretraining contamination of the zero-shot models**
Datei: `sections/limitations.tex`, Zeile 302 · Caption von `tab:lim_point` (132 Wörter)
- **Anfang:** „Post hoc test of point-forecast accuracy against the Random Walk, added 2026-08-03 (see \cref{tab:lim_amendments})."
- **Ende:** „…so the loss differential is identically zero and the test is undefined; it enters the FDR family with p=1."

---

**⑭ Kapitel 9 → 9.2 The admissibility gate does not test what the methods fail at**
Datei: `sections/limitations.tex`, Zeile 520 · Caption von `tab:lim_dq` (92 Wörter)
- **Anfang:** „Post hoc decomposition of the dynamic-quantile rejections, computed 2026-08-03 and re-verified 2026-08-17."
- **Ende:** „…using the same asymptotic covariance α(1−α)(X'X)⁻¹ that defines the DQ statistic."

---

**⑮ Kapitel 9 → 9.7 Scope: what this study does not claim**
Datei: `sections/limitations.tex`, Zeile 1312 · Caption **und Tabelleninhalt** von `tab:lim_amendments`
- **Anfang:** „Register of all deviations from and additions to the pre-registered protocol (`diagnostics_foundation.txt`)."
- **Ende:** „Every row is also recorded in paragraph 11 of the protocol document under the same date."
- ⚠️ Hier ist auch die Spalte **„Character"** in der Tabelle selbst KI-formuliert („Post hoc decomposition of a pre-registered statistic, reported to explain rather than to establish"). Mit überarbeiten.

---

### 4.1b PRIORITÄT 1b — Kapitel 5 „Experimental Design", Zeile 1–160

**Nachtrag.** Diese Zeilen wurden seit dem Erst-Import **nie überarbeitet** (`git blame`). Dein Rewrite-Commit `1450e0e` hat in dieser Datei ausschließlich die Zeilen 161–195 berührt — also die drei Abschnitte, die unten in §4.3 stehen. Alles davor ist unbearbeiteter KI-Entwurf.

Datei: `sections/experimental_design.tex`

| Abschnitt | Anfangssatz | Endsatz |
|---|---|---|
| **5.1 Forecasting task** | „For each country $i$ and decision month $t$, the models produce three conditional quantiles…" | „…which needs no distributional assumption." |
| **5.2 Training regimes** | „The deep models are trained in three regimes:" | „LightGBM uses only L and G, while the classical and zero-shot models use a single setting." |
| **5.3 Input sequences and scaling** | „The input is the sequence of the last $W$ monthly feature vectors up to the decision month $t$…" | „…the target and the evaluation are never imputed." |
| **5.4 Rolling refit and temporal evaluation** | „We retrain all models on an expanding training set every 12 months." | „…that is 2015-10 to 2026-04, giving 127 test months per country." |
| **5.5 Hyperparameter selection** (Einleitung) | „The hyperparameter optimization of all tuned models uses months 1 to 114 as the training set…" | „…averaged across countries, over the burn-in validation months." |
| **5.5 ¶ LSTM, xLSTM** | „Both share the same protocol and differ only in architecture." | „(Selected: xLSTM L $W{=}3$/5 ep., G $W{=}24$/1 ep.; LSTM L $W{=}3$/29 ep., G $W{=}3$/10 ep.)" |
| **5.5 ¶ LightGBM** | „We use the same Optuna protocol (TPE, 100 trials, the same pruner, the same burn-in selection)…" | „Early stopping here acts on boosting rounds." |
| **5.5 ¶ ARIMA, Random Walk** | „ARIMA has only the L regime and is therefore fitted per country on the differenced target…" | „They share the test window and the conformal layer with the other models, but none of the selection machinery." |
| **5.5 ¶ TiRex, TiRex-2** | „TiRex and TiRex-2 are pretrained time-series foundation models, so they need no training and no hyperparameter tuning." | „TiRex-2 is additionally run in a covariate variant; the variants are described in \cref{sec:models}." |
| **Caption `tab:refit_blocks`** | „Refit block structure. Burn-in blocks initialize the conformal calibration pool and are not evaluated." | — |
| **Caption `tab:model_selection`** | „Model selection across families. All trainable models share the refit-block structure, per-country scaling and the 24-month information cap…" | „…they differ only as listed here." |

**Zusätzlich, gleiche Kategorie:** `sections/models.tex`, §6.4 xLSTM — der Absatz von *„xLSTM \parencite{beck2024xlstm} is the state-of-the-art extension of LSTM."* bis *„…which increases inference and training time."* (höchster Einzelwert der Arbeit) sowie der anschließende Absatz *„A gate scales a flow."* bis *„…is shared by both cell types below."*

---

### 4.2 PRIORITÄT 2 — `conformal.tex` ab dem Marker `%bis hier gemacht`

**Alles ab Zeile 243 bis zum Dateiende.** Zur Orientierung die betroffenen Unterabschnitte mit Anfangssätzen:

| Abschnitt | Anfangssatz | Endsatz |
|---|---|---|
| **6.3.2 Group (b)** (Rest) | „`mondrian` performs category-conditional calibration…" | „…the method goes back to using the unconditioned global pool." |
| **6.3.3 Group (c): the ACI family** | „Adaptive Conformal Inference (ACI) regulates the effective miscoverage level α_t instead of the threshold itself." | „…so adding sampling variance on top would only blur the methodological contrasts." |
| **6.3.4 Group (d): forecasting the score itself** | „`spci` turns the usual conformal logic around." | „…fully determined by the (1−α)-quantile alone, so we omit the β-search." |
| **6.3.5 Group (e): online control** | „The methods in this last category keep no historical pool of scores." | „…it trades adaptivity for long-term stability." |
| **6.4 Zero tuned parameters** | „No parameter in the conformal calibration layer was tuned or optimized on data." | „…reproduces the parameter dictionary from our execution environment verbatim." |
| **6.5 Derived prediction streams** | „We construct two additional prediction streams computationally from the saved model artifacts…" | „This stream only exists for trainable models with a global regime." |
| **6.6 Implementation notes and deliberate adaptations** | „All 14 implementations follow their original papers, but our monthly CQR-score setup needs a few technical conventions…" | „…so calibration and inference stay cleanly separated." |
| **Caption `tab:cp_methods`** | „The 14 conformal prediction methods applied to every stream, grouped by increasing adaptivity." | „…$\bar S$ denotes the burn-in score scale." |

**Nicht betroffen** und unkritisch: alles **vor** Zeile 243 — §6.1 „Score, interface and per-stream calibration" und §6.2 „Calibration pool and information set". Dort steht deine eigene Prosa.

---

### 4.3 PRIORITÄT 3 — bereits einmal überarbeitet, zweiter Durchgang empfohlen

Diese Stellen hast du in Commit `1450e0e` angefasst, aber nur auf Satzebene umgestellt. Struktur und Gedankenführung stammen weiterhin aus dem KI-Entwurf.

| Kapitel | Datei | Anfangssatz | Endsatz |
|---|---|---|---|
| **5 Experimental Design** → 5.6 Seeds and aggregation | `experimental_design.tex` | „Only the neural models, xLSTM and LSTM, use the three seeds (42, 43, 44)." | „…are deterministic and run with a single seed." |
| **5 Experimental Design** → 5.7 Quantile post-processing | `experimental_design.tex` | „The three quantiles are estimated independently, so they can cross." | „This step is the same for every model." |
| **5 Experimental Design** → 5.8 Conformal prediction layer | `experimental_design.tex` | „The benchmarks emit only raw quantiles and contain no calibration code." | „…the calibration-pool variants are described in \cref{sec:conformal}." |
| **7 Evaluation** → Einleitung | `evaluation.tex` | „Our reporting follows a validity-first logic: the interval calibration works as an admissibility gate…" | „…we make it precise in \cref{sec:eval:validity}." |
| **7 Evaluation** → 7.2 Scale-free aggregation | `evaluation.tex` | „The ratios are first calculated per country and then the median is taken across all countries." | „…on the sign of the median Δ forecast." |
| **6 Models** → 6.3 LSTM (Schluss) | `models.tex` | „The LSTM shares the entire training protocol with the xLSTM and differs only in the recurrent cell." | „…the LSTM is our ablation for what the xLSTM adds." |
| **6 Models** → 6.5 Pre-trained foundation models | `models.tex` | „TiRex-2 is used in two variants: first, the univariate variant that only sees the spread's past as context…" | „There might be a potential contamination in the evaluation window." |
| **3 Data** → 3.4 Publication-lag protocol | `data.tex` | Fußnote ‡ zur Tabelle: „*Split lag.* When an input combines two series with different release calendars…" | „…so we do not lag the product a second time." |

---

### 4.4 Nicht in der Liste — und warum

> ⚠️ **Korrektur zur ersten Fassung:** `experimental_design.tex` und `models.tex` §6.4 standen hier ursprünglich als unkritisch bzw. nur teilweise betroffen. Das war falsch — siehe §4.1b.

| Bereich | Einschätzung |
|---|---|
| Fließtext in `results.tex` und `limitations.tex` | **Deine Stimme.** Dicht besetzt mit deinen Merkmalen, mit typischen kleinen Unsauberkeiten. Kein Handlungsbedarf. |
| `conformal.tex` bis Zeile 243 | **Deine Stimme.** Du hast die `% SCHREIBE:`-Vorgaben selbst ausformuliert. |
| `evaluation.tex` — Testbeschreibungen (Kupiec, Christoffersen, DQ, BH-FDR) | Formelnahe Standardbeschreibungen, deine Formulierung. Unkritisch. |
| `models.tex` — LSTM-/xLSTM-Mechanik | Deine Stimme („We go through a full time step t in an sLSTM block to give the reader a better understanding of its working."). Unkritisch. |
| `appendix.tex` | Kurze, rein faktische Captions zu Hyperparametertabellen. Geringes Risiko. |
| Alle Zahlen, Tabellenwerte, Abbildungen | **Deine eigenen Läufe.** Kein Thema. |
| `sections/prompts.tex` | Ist in `main.tex` **nicht** eingebunden und damit derzeit nicht Teil der Arbeit. Wenn du es noch einbindest: vorher prüfen, es ist nicht analysiert. |

---

## 5. Vorlage für den Anhang „Übersicht verwendeter Hilfsmittel"

Nach der Überarbeitung reicht eine Auflistung dieser Art (anpassen an das, was tatsächlich zutrifft — Tool, Version und Zeitraum musst du selbst einsetzen):

```latex
\section*{Übersicht verwendeter Hilfsmittel}
\addcontentsline{toc}{section}{Übersicht verwendeter Hilfsmittel}

Im Rahmen dieser Arbeit wurden generative KI-Systeme als Hilfsmittel
eingesetzt. Die folgende Übersicht dokumentiert, welche Systeme wofür
verwendet wurden.

\begin{tabular}{@{}p{3.2cm}p{4.2cm}p{6.5cm}@{}}
\toprule
\textbf{System / Version} & \textbf{Zeitraum} & \textbf{Art und Ort der Verwendung} \\
\midrule
[Tool, Version] & [MM/JJJJ--MM/JJJJ] &
  Strukturierung und Entwurfsfassungen von Fließtext in den Kapiteln
  [X, Y, Z]; sämtliche Textteile wurden anschließend von mir
  inhaltlich überarbeitet und neu formuliert. \\
\addlinespace
[Tool, Version] & [MM/JJJJ--MM/JJJJ] &
  Unterstützung bei der Implementierung der Auswertungs-Pipeline
  (Python) sowie beim LaTeX-Satz von Tabellen und Abbildungen. \\
\addlinespace
[Tool, Version] & [MM/JJJJ--MM/JJJJ] &
  Rechtschreib- und Grammatikkorrektur. \\
\bottomrule
\end{tabular}

\vspace{2mm}
\noindent
Die inhaltliche Konzeption, die Auswahl und Implementierung der Modelle,
die Durchführung sämtlicher Experimente sowie die Interpretation der
Ergebnisse liegen ausschließlich bei mir. Es wurden keine Textpassagen
ohne substantielle Änderungen übernommen.
```

⚠️ **Den letzten Satz nur schreiben, wenn er nach der Überarbeitung stimmt.** Wenn du Passagen unverändert behältst, gehören für diese Prompt, Produktname und Version/Datum in die Tabelle — so verlangt es die Erklärung wörtlich.

---

## 6. Vor der Abgabe aufräumen

Drei Dinge, die derzeit unabhängig vom Text KI-Einsatz belegen:

1. **Deutsche KI-Arbeitskommentare im `.tex`-Quelltext.** In `conformal.tex` (Kopf, `% SCHREIBE:`-Blöcke, `% PFLICHT:`, `%bis hier gemacht`, der lange `% !! UEBERARBEITEN`-Block ab Zeile ~650) und in `models.tex` (Block am Dateiende). Erscheinen nicht im PDF, sind aber im Quelltext — und dein Repo ist öffentlich auf GitHub.
2. **Commit-Nachrichten.** `1450e0e` „Rewrite polished passages into own voice" mit `Co-Authored-By: Claude Fable 5` und `9b7a79f` mit `Co-Authored-By: Claude`. Nicht rückwirkend änderbar ohne History-Rewrite. Das ist kein Regelverstoß — im Gegenteil, es ist ehrliche Dokumentation — aber du solltest wissen, dass es sichtbar ist, und es passend im Anhang abbilden.
3. **`REVIEW_2026-07-22.md`** im Wurzelverzeichnis (36 KB): KI-Review-Notizen zur Arbeit. Falls du das Repo als Reproduktionsbeleg angibst, vorher entscheiden, ob es dazugehören soll.

---

## 7. Zwei Nebenbefunde, die nichts mit KI zu tun haben

Beim Durcharbeiten aufgefallen, unabhängig vom eigentlichen Auftrag:

0. **Sechs Zitate wurden am 21.08.2026 nachgetragen** (siehe §8).

1. **Fünf Unterabschnitte in `evaluation.tex` sind leer** — nur Überschrift und `\label`, kein Inhalt: *Formal forecast comparison*, *Model Confidence Set*, *Regime-conditional analysis*, *Economic evaluation (supplementary)*, *Robustness protocol*. Passend dazu sind **11 BibTeX-Einträge nie zitiert**, darunter genau die Methodenquellen für diese Abschnitte: `hansen2011mcs`, `dieboldmariano1995`, `harvey1997`, `newey1987`, `kunsch1989mbb`, `giacomini2006`. Diebold-Mariano, HLN-Korrektur, Newey-West und der MCS werden in Kapitel 8 und 9 durchgehend verwendet, aber nirgends eingeführt oder zitiert. Das ist ein Angriffspunkt für den Gutachter.
2. **Das Abstract ist noch ein TODO-Platzhalter** in `main.tex`, und Introduction sowie Related Work sind auskommentiert.

---

*Analysemethodik: Orthographie-Profilierung (britisch/amerikanisch) über 60.000 Wörter, Marker-Dichte-Vergleich Fließtext vs. Tabellenblöcke, `git blame`- und Diff-Forensik über alle 20 Commits, Auswertung der im Quelltext verbliebenen Arbeitskommentare, Vollständigkeitsprüfung aller 52 Zitationsschlüssel.*


---

## 8. Nachgetragene Zitate (21.08.2026)

Eingefügt wurden ausschließlich Verfahren, die du **verwendest, aber nirgends zitierst** — und deren Einträge bereits in deiner `references.bib` lagen, sodass der Inhalt gesichert ist. Je Verfahren eine Nennung, an der ersten substanziellen Stelle.

| Verfahren | Key | Eingefügt in |
|---|---|---|
| Diebold–Mariano-Test | `dieboldmariano1995` | `results.tex` Caption `tab:rq1_results` (erste Nennung) · `limitations.tex` §9.3 Fließtext |
| Harvey–Leybourne–Newbold-Korrektur | `harvey1997` | `results.tex` Caption `tab:rq1_results` |
| Newey–West-HAC-Varianz | `newey1987` | `results.tex` Caption `tab:rq1_results` · `limitations.tex` §9.3 (dort wird die Formel reproduziert) |
| Model Confidence Set | `hansen2011mcs` | `results.tex` §8.8 Fließtext, „Using the MCS…" |
| Moving-Block-Bootstrap | `kunsch1989mbb` | `results.tex` Notes zu `tab:res_mcs` |

Alle Schlüssel lösen auf; die Arbeit hat weiterhin **keine fehlenden Referenzen**.

### Bewusst nicht gesetzt

- **Driscoll–Kraay** (`limitations.tex` §9.3: *„This is a pragmatic one-dimensional version of the Driscoll-Kraay logic."*) — dafür existiert **kein Eintrag** in deiner `.bib`. Du nennst das Verfahren zweimal namentlich; hier gehört eine Quelle hin. Der Standardbeleg ist Driscoll & Kraay (1998), *Review of Economics and Statistics* 80(4), 549–560. Ich habe den Eintrag **nicht** angelegt, weil ich Seitenzahlen und DOI nicht ungeprüft in deine Bibliographie schreiben will — bitte selbst ergänzen.
- `giacomini2006` (Giacomini & White, *Tests of Conditional Predictive Ability*) — läge thematisch nahe bei deiner Diskussion, dass DM-Tests hier zum Vergleich von **Modellen** statt von Forecasts eingesetzt werden. Das wäre aber eine inhaltliche Erweiterung, keine fehlende Quellenangabe. Deine Entscheidung.
- `diebold2015`, `bcbs2019mar`, `mccracken2016fredmd`, `eurostat_industrialproduction`, `tirex` — bleiben unzitiert. Für `tirex` beachte: der Eintrag enthält noch `note = {TODO: fill exact citation}`, wird aber nicht verwendet (`models.tex` zitiert korrekt `auer2025tirex`). Vor der Abgabe entweder füllen oder löschen.

---

## 9. Tabellen-Captions und Notes: kürzen statt umschreiben (21.08.2026)

> **Korrektur zu §0:** Die dort genannten „~4.700 Wörter" für Prio 1 waren zu hoch gegriffen — die Zahl enthielt die Zahlenzeilen der Tabellen. Der tatsächliche **Prosa**-Umfang in Captions und Notes beträgt **2.590 Wörter** (results 2.317, limitations 273). Davon lassen sich nach der Analyse unten rund **1.600 Wörter ersatzlos streichen**. Es bleiben ~1.000 Wörter, die tatsächlich umformuliert werden müssen.

### 9.1 Ist das überhaupt nötig?

Nein. Die Konvention in ökonometrischen Zeitschriften (JBES, IJF, Econometrica — also genau die Journals in deiner Bibliographie) ist:

| Element | Zweck | Übliche Länge |
|---|---|---|
| **Caption** | Sagt, *was* die Tabelle zeigt. Ein Satz, titelartig. | 10–25 Wörter |
| **Notes** | Nur, was zum *Lesen* der Tabelle nötig ist und nicht im Text steht: Definition der Spalten, Stichprobe, Test samt Korrektur, Vorzeichenkonvention, Bedeutung der Marker. | 40–90 Wörter |
| **Fließtext** | Alles Interpretierende: was das Ergebnis bedeutet, warum es so ausfällt, was daraus folgt. | — |

Deine Notes verletzen die dritte Zeile systematisch. Sie interpretieren.

Der Grund dafür ist erkennbar: Journal-Tabellen müssen **standalone lesbar** sein, weil sie zitiert und aus dem Kontext gerissen werden. Ein KI-Modell, das auf solchen Papers trainiert ist, treibt diese Konvention ins Extrem. In einer Masterarbeit wird linear gelesen — der Absatz steht zwanzig Zeilen über der Tabelle. Standalone-Lesbarkeit ist hier kein Wert an sich.

### 9.2 Umfang gemessen

Verhältnis Caption+Notes zum Fließtext des jeweiligen Abschnitts:

| Tabelle | Caption | Notes | Σ | Fließtext | Verhältnis |
|---|---|---|---|---|---|
| `tab:res_cov` | 33 | **394** | **427** | 329 | **1,30×** ⚠️ |
| `tab:rq1_results` | 173 | 151 | 324 | 460 | 0,70× ⚠️ |
| `tab:res_xm` | 46 | 219 | 265 | 387 | 0,68× ⚠️ |
| `tab:lim_point` | 134 | 0 | 134 | 265 | 0,51× ⚠️ |
| `tab:rq2_results` | 134 | 114 | 248 | 555 | 0,45× |
| `tab:res_regime` | 34 | 334 | 368 | 810 | 0,45× |
| `tab:res_mcs` | 31 | 144 | 175 | 387 | 0,45× |
| `tab:res_rq2_lstmG` | 24 | 147 | 171 | 555 | 0,31× |
| alle übrigen | 28–74 | 0 | 28–74 | — | ≤0,10× |

Bei `tab:res_cov` steht **mehr Text unter der Tabelle als im ganzen Abschnitt darüber**. Die Tabellen ohne Notes (`tab:res_ranking`, `tab:res_robustness_*`, `tab:lim_dq`) liegen alle unter 0,10× und sind völlig in Ordnung — das zeigt, dass das Problem nicht die Tabellen sind, sondern die sieben Notes-Blöcke.

### 9.3 Was ersatzlos gelöscht werden kann

Diese Sätze wiederholen Zahlen oder Aussagen, die im selben Abschnitt bereits im Fließtext stehen. **Löschen, nichts ersetzen.**

| Tabelle | Zu löschender Satz (Anfang) | Steht bereits hier |
|---|---|---|
| `tab:rq1_results` | „The three contrasts form a coherent chain: pooling costs $+0.017$, fine-tuning recovers…" | Fließtext §RQ1 nennt alle drei Werte einzeln |
| `tab:res_xm` | „No pair reaches a median $q$ below 0.10; the smallest is 0.185 and the median across all 66 pairs is 0.564." | Fließtext §8.8: „no contrast reaches significance on the median $q$, with the lowest value (0.185)" |
| `tab:res_xm` | „Underneath that aggregate, 92 of the 660 individual country tests do fall below 0.10…" | Fließtext §8.8: „92 of the 660 single country tests do fall below 0.10" |
| `tab:res_mcs` | „Fifteen candidates survive in all ten countries and nine more in at least eight…" | Fließtext §8.8: „15 out of the 26 packages are in the winning set for all 10 countries" |
| `tab:res_mcs` | „Panel A and Panel B together reconstruct all 260 country--candidate decisions. Of those, 238 are inclusions, a rate of 0.915." | `limitations.tex` §9.3: „inclusion rate of $91.54\%$" |
| `tab:res_mcs` | „Set size and terminal $p$ do not move together: Finland retains all 26…" | Fließtext §8.8: „Finland and France exclude none of the 26 packages while Ireland excludes 8" |
| `tab:res_regime` | „The skill advantage of \texttt{tirex2} in calm markets comes with a coverage of 0.654 against a target of 0.80…" | Fließtext §8.9: „it covers only at 0.654 in the calm months" |
| `tab:res_regime` | „Coverage and Winkler in Panel A are means over the 485 admissible combinations…" | Fließtext §8.9 und Caption sagen beides schon |
| `tab:res_cov` | „Both are the same pre-trained model in the ZS regime under seed 42 on the same 127 test months…" | Fließtext §8.7: „the same model, on the same 127 test months" |
| `tab:res_cov` | „$\Delta$ is the difference of the two median skills, where skill is the median across countries of $1-\bar W/\bar W_{\mathrm{ref}}$…" | Skill ist in §7.2 definiert (`eq:skill`), Verweis genügt |
| `tab:res_cov` | „They narrow the interval in all ten countries, by 2.2 to 6.9 per cent…" + „Coverage falls in nine countries, from a median of 0.843 to 0.831…" | Fließtext §8.7, Absatz „The per-country data of the raw model output…" |
| `tab:res_cov` | „Both effects are small against the scale of the study, where the best admissible package leads…by 0.0609…" | Fließtext §8.7: „the effect is almost zero" |
| `tab:res_rq2_lstmG` | „The last row pools the $10\times42=420$ cells, which is why it differs from the $+0.062$…" | **Ausnahme — behalten.** Verhindert einen echten Lesefehler. |

**Ersparnis allein durch Löschen: rund 620 Wörter, ohne dass ein einziger Satz neu geschrieben werden muss.**

### 9.4 Was in den Fließtext gehört statt in die Notes

Diese Aussagen sind **neu und inhaltlich wertvoll** — sie stehen nur an der falschen Stelle. Verschieben, nicht löschen. Beim Verschieben schreibst du sie ohnehin in deiner Stimme neu, womit gleichzeitig das KI-Problem erledigt ist.

| Aus | Aussage | Nach |
|---|---|---|
| `tab:res_cov` Notes | „Across the 140 country tests, 18 reach $p<0.10$ before correction, close to the 14 expected under the null, and they split 10 to 8 by direction." | §8.7 Fließtext — das ist dein stärkstes Argument für „kein Effekt": die Trefferzahl entspricht dem Zufall |
| `tab:res_cov` Notes | „…we keep the families separate because these tests address a different question from the pre-registered contrasts." | §8.7 Fließtext oder Limitations — eine methodische Entscheidung, die du verteidigen musst, gehört nicht in eine Fußnote |
| `tab:res_regime` Notes | „Under realised stress the overall ranking survives, with a Spearman correlation of $+0.846$ ($p=0.0005$)… Under high VIX it does not, at $+0.399$ ($p=0.199$)…" | §8.9 Fließtext — dein Text behauptet dort „the ranking stays the same", ohne die Zahl zu nennen, die es belegt |
| `tab:res_regime` Notes | „The calendar split of \texttt{config/pipeline.yaml} is not reported separately… 89.5 per cent of stressed country-months fall before 2022-01…" | §8.9 Fließtext oder Limitations — die Begründung, warum eine vorregistrierte Achse weggelassen wurde, muss sichtbar sein |
| `tab:rq1_results` Notes | „No contrast is FDR-significant at the contrast level: over the 266 pre-registered contrasts the smallest median $q$ is 0.019…" | Limitations §9.3 (dort steht das Thema bereits) — dann in §8.5 ersatzlos streichen |

### 9.5 Was zwingend bleiben muss

Nicht kürzen. Ohne diese Angaben ist die Tabelle nicht lesbar oder wird falsch gelesen:

- **Vorzeichenkonventionen.** „a *negative* value means A has the lower loss", „a *positive* value means the adaptive variant performs worse", „a negative entry means the covariates help". Deine Tabellen mischen drei verschiedene Konventionen — der Satz „The two quantities carry opposite sign conventions and must not be read against each other" in `tab:rq1_results` ist keine Füllung, sondern verhindert einen echten Fehler.
- **Marker-Legenden.** Was `†` bedeutet, jeweils einmal pro Tabelle.
- **Spaltendefinitionen**, sofern die Spalte nicht anderswo definiert ist: „Median $q$", „seed-stable", „Share $A>B$", „Gate rate".
- **Test plus Korrektur plus Stichprobe** in einer Zeile: „Diebold--Mariano mit HLN-Korrektur, 127 monatliche Winkler-Differenzen pro Land."
- **Seed-Provenienz** in `tab:res_mcs` („Seeds are 42 except for candidates 5, 6, 17…") — sonst ist die Tabelle nicht reproduzierbar.
- **`tab:lim_point`**: die Erklärung, warum Frankreich fehlt (ARMA-Ordnung $(0,0)$ reproduziert den Random Walk, Differenz identisch null). Kürzen auf einen Satz, aber behalten.

### 9.6 Zielwerte

| Tabelle | jetzt | Ziel | Weg |
|---|---|---|---|
| `tab:res_cov` | 427 | **~90** | 6 Sätze löschen, 2 in den Text verschieben, Rest auf Definitionen eindampfen |
| `tab:res_regime` | 368 | **~110** | 2 löschen, 2 verschieben |
| `tab:rq1_results` | 324 | **~120** | Caption von 173 auf ~60 kürzen (Spaltendefinitionen reichen), 2 Sätze löschen |
| `tab:res_xm` | 265 | **~90** | 3 löschen |
| `tab:rq2_results` | 248 | **~110** | Caption kürzen, Notes weitgehend behalten (überwiegend Definitionen) |
| `tab:res_mcs` | 175 | **~70** | 3 löschen |
| `tab:res_rq2_lstmG` | 171 | **~90** | fast alles Definition, nur straffen |
| `tab:lim_point` | 134 | **~60** | Caption straffen |
| **Summe** | **2.112** | **~740** | **−1.370 Wörter** |

### 9.7 Faustregel für den Rest

Bevor ein Satz in einer Caption oder in Notes stehen bleibt, eine Frage: **Brauche ich ihn, um die Zahlen in der Tabelle zu *lesen* — oder sagt er mir, was sie *bedeuten*?** Nur das Erste gehört dorthin. Alles Zweite gehört in den Fließtext oder ist bereits dort.

Zweite Prüfung: **Steht eine Zahl aus den Notes schon im Absatz darüber?** Dann raus. Eine Zahl gehört genau einmal in die Arbeit.
