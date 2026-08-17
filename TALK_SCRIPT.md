# Vortragsskript — Erste Group, Risk Management

**Deck:** `Erste_Risk_Talk.pptx`, 25 Folien · **Dauer:** ~34 min + Q&A
**Sprache:** Vortrag deutsch, Folien und Fachbegriffe englisch
**Begleitdokument:** `TALK_REFERENCE.md` — dort steht jeder Test, jede Korrektur und jede
Kennzahl im Detail. Dieses Skript verweist darauf mit `→ REF §x`.

---

## Wie du dieses Dokument benutzt

Pro Folie gibt es drei Blöcke:

- **SAGEN** — sprechbarer Text. Nicht auswendig lernen. Einmal laut lesen, dann in eigenen
  Worten. Die *kursiven* Sätze sind die, die wörtlich sitzen sollten, weil sie die Aussage
  tragen.
- **TIEFE** — was hinter der Folie steht, für Rückfragen. Nur folienspezifisches; die
  allgemeine Statistik-Maschinerie steht in `TALK_REFERENCE.md`.
- **FALLEN** — was du nicht sagen solltest, und wo du dich selbst angreifbar machst.

Eine Entscheidung, die du kennen musst: **Auf den Folien ist `tirex2/ZS/cqr_static` (Skill
0.146) als bestes Paket genannt, mit sofortigem Instabilitätshinweis; `decay_ocp_cqr` (0.144)
ist die Variante, die man einsetzen würde.** In `results.tex` §8.3 steht derzeit noch
`decay_ocp_cqr` als Headline. Wenn jemand die Arbeit gelesen hat, sag: „Im Text führe ich
die robuste Variante als Empfehlung, hier zeige ich beide, weil die Unterscheidung der
eigentliche Punkt ist."

---

# Folie 1 — Titel

**SAGEN.**
Guten Tag, mein Name ist Felix Neuwirth. Ich habe an der TU Wien meine Masterarbeit in
Financial and Insurance Mathematics geschrieben, betreut von Professor Gerhold, und ich
stelle Ihnen heute die Ergebnisse vor. Das Thema sind Prognoseintervalle für
Staatsanleihen-Spreads im Euroraum — konkret: wie man ein Band um eine Prognose legt, dem man
trauen kann, und wie man nachweist, dass es hält.

Ich habe rund eine halbe Stunde. Der Aufbau ist: erst die Aufgabenstellung und die
Bewertungsmaße, dann die Ergebnisse, dann die Grenzen. Fragen gerne dazwischen.

**FALLEN.** Nicht mit „meine Arbeit über xLSTM" anfangen. Die Architektur ist ein Mittel,
nicht das Thema — und sie ist am Ende die schwächste. Wenn du sie vorne ins Zentrum stellst,
liest sich Folie 13 wie ein Scheitern statt wie ein Befund.

---

# Folie 2 — Objective and research questions

**SAGEN.**
Die Aufgabe in einem Satz: *Wir prognostizieren die Monatsänderung von zehn
Euro-Staatsanleihen-Spreads einen Monat voraus, aber nicht als Punktschätzung, sondern als
Intervall. Dann prüfen wir statistisch, welche Kombinationen aus Modell und Kalibrierung
überhaupt zulässig sind, und ranken nur die zulässigen nach Intervallqualität.*

Drei Fragen waren vorregistriert, also vor dem ersten Ergebnis festgelegt. RQ1: Bringt ein
globales Modell über alle Länder mehr als ein Modell pro Land, und holt Fine-Tuning die
Differenz auf? RQ2: Bringt eine Intervallbreite, die das Modell selbst über die Zeit variiert,
mehr als eine konstante Breite? RQ3: Bringt die Conformal-Schicht überhaupt etwas gegenüber
dem, was das Modell von sich aus als Quantile ausgibt?

Zwei Fragen sind nachträglich dazugekommen, und ich weise sie als post hoc aus. Q4: Bringen
die 17 makroökonomischen Kovariaten etwas über die Spread-Historie hinaus? Q5: Hält das
Ranking unter Marktstress?

Die n-Spalte ist die Anzahl der Kontraste in der jeweiligen Testfamilie — bei RQ3 sind das
689 Vergleiche. Jede Familie wird separat für Mehrfachtestung korrigiert.

Ein Prinzip vorweg, weil es alles danach bestimmt: *Kalibrierung benutze ich als Zulassungs­
kriterium, nie als Rangkriterium.* Warum, kommt auf Folie 7 und 8.

**TIEFE.**
- „Vorregistriert" heißt: es existiert ein Dokument, `diagnostics_foundation.txt`, in dem der
  Kontrastsatz, die Schwellen und der Findings-Katalog vor der Auswertung festgelegt wurden.
  Der beste Beleg, dass es echt war: von 13 vorregistrierten Findings-Typen ist einer, F12,
  **leer** — er verlangt ein MAE-Verhältnis unter 0.98 bei negativem Skill, und das beste
  MAE-Verhältnis im ganzen Feld ist 0.9965. Die Bedingung ist unerfüllbar. Ein leerer
  vorregistrierter Typ beweist, dass der Katalog nicht nachträglich passend gemacht wurde.
- Die n-Werte: RQ1 266 Kontraste, RQ2 280, RQ3 689, Q4 140 Ländertests. Dazu 66 post-hoc
  Cross-Model-Kontraste und 84 für den nachträglich ergänzten GF-vs-L-Kontrast.
- Warum eigene FDR-Familien: → REF §3.4.

**FALLEN.** Sag nicht „wir haben alles getestet". Sag „drei Fragen waren vorher festgelegt,
zwei sind dazugekommen und als solche deklariert". Der Unterschied ist der ganze Punkt.

---

# Folie 3 — Summary of findings

**SAGEN.**
Ich stelle die Ergebnisse vorweg, damit alles Weitere Beweisführung ist und nicht Spannung.
Elf Zeilen, jede mit der tragenden Zahl und der Folie, auf der sie belegt wird. Ich gehe nur
auf vier ein, den Rest können Sie mitlesen.

Erstens: *Kein Modell schlägt den Random Walk bei der Punktprognose.* Das beste
MAE-Verhältnis im ganzen Feld ist 0.997, also ein Vorsprung von 0.3 Prozent, und der ist
statistisch nicht von null zu unterscheiden.

Drittens: Das beste Intervallpaket ist ein vortrainiertes Foundation Model, das nie auf
diesen Daten trainiert wurde — Winkler-Skill 0.146 gegenüber dem Random Walk.

Achtens: Die 17 makroökonomischen Kovariaten ändern auf diesem Horizont nichts Messbares.

Elftens, und das ist die wichtigste Einschränkung: *Kein einziger paarweiser Modellvergleich
übersteht die Korrektur für Mehrfachtestung.* Von 66 Vergleichen null. Die Rangfolge, die ich
Ihnen zeige, ist deskriptiv. Was stabil ist, ist die grobe Gruppierung, nicht die Reihenfolge
darin.

**TIEFE.**
- Die Kennzahlen der Zeilen, die du nicht ansprichst: Zeile 2 Coverage 0.872 gegen Ziel 0.800
  bei 13.2 % Gate-Rate; Zeile 4 xLSTM Median-Skill −0.047; Zeile 5 Breite = 72.8 % des Scores;
  Zeile 6 G-vs-L +0.017 zugunsten lokal; Zeile 7 modelleigene Adaptivität +0.020 gegen
  CP-Adaptivität −0.054; Zeile 9 Korrelation Skill/Kapitalquotient −0.711 gegen
  Skill/Sharpe −0.147; Zeile 10 Spearman +0.846 im Stressterzil gegen +0.399 bei hohem VIX.
- Wenn jemand nach Zeile 11 sofort nachfragt: → REF §5.4 und §6.

**FALLEN.** Sag bei Zeile 11 nicht „statistisch gesichert ist die Gruppierung". Das ist zu
stark. Richtig ist: „die Rangfolge ist deskriptiv; die grobe Gruppierung bleibt über alle
Robustheitsschnitte stabil". Stabil ist nicht dasselbe wie nachgewiesen.

---

# Folie 4 — Data and target

**SAGEN.**
Kurz zu den Daten. Zielgröße ist die Monatsänderung des Spreads, definiert als
Zehnjahresrendite des Landes minus Deutschland, aus dem ECB-Datensatz. Ich modelliere in
Differenzen, weil der Spread-Level extrem persistent ist — wer den Level prognostiziert,
bekommt ein hervorragendes R-Quadrat, das nur die Persistenz abbildet und nichts über die
Prognosefähigkeit sagt.

*Bewertet wird aber immer im Level-Raum.* Jede Prognose wird zurücktransformiert, indem der
letzte bekannte Level addiert wird. Damit ist die berichtete Genauigkeit nicht durch die
Differenzierung geschönt.

Zehn Länder, monatlich, 2003 bis 2026, 277 Monate, also 2770 Länder-Monate. Ein einziger
fehlender Wert im ganzen Panel — der griechische Spread im Juli 2015, während der
Kapitalverkehrskontrollen — und der liegt außerhalb der Testperiode.

Jedes Modell gibt drei Quantile aus, 0.1, 0.5 und 0.9. Das mittlere ist die Punktprognose,
die äußeren spannen ein zentrales 80-Prozent-Intervall auf, also Alpha gleich 0.20. Getestet
wird auf 127 Monaten von Oktober 2015 bis April 2026.

Zur Wahl von 0.1 und 0.9, weil das eine naheliegende Frage ist: *Die beiden vortrainierten
Modelle können nur Dezile ausgeben.* 0.1 und 0.9 ist damit das breiteste Paar, das jedes
Modell im Vergleich nativ liefert. Hätte ich 1 Prozent und 99 Prozent gewählt, hätten die
Foundation Models extrapolieren müssen und der Vergleich wäre unfair geworden. Das ist eine
Vergleichbarkeitsentscheidung, keine Aussage über Risikoappetit.

**TIEFE.**
- Griechenland hat zwei Besonderheiten: die fehlende Marktnotierung 2015-07 und den
  Strukturbruch 2012 durch das Private Sector Involvement, also den Schuldenschnitt. Beides
  ist dokumentiert; der fehlende Monat wird fortgeschrieben, nie interpoliert, und das Ziel
  wird nie imputiert.
- Für 2015-08 fehlt zusätzlich der Level-Anker, weil ohne s(t−1) die Rücktransformation nicht
  definiert ist. Beide Monate liegen im Burn-in.
- Warum kein 99-Prozent-Intervall: bei 127 Testmonaten und Alpha 1 Prozent erwartet man 1.27
  Verletzungen pro Zelle. Damit hat jeder Kalibrierungstest praktisch keine Power. Bei Alpha
  20 Prozent erwartet man 25.4 Verletzungen — das ist die Grundlage dafür, dass Kupiec und
  Christoffersen überhaupt etwas aussagen können. → REF §2.5
- Standardisierung: z-Standardisierung pro Land und pro Feature, gefittet nur auf dem
  Trainingsfenster des jeweiligen Refits. Kein Zukunftswissen in der Skalierung.

**FALLEN.** Wenn gefragt wird „warum nicht täglich?": monatlich, weil die
Fundamentaldaten-Kovariaten monatlich oder quartalsweise publiziert werden. Bei täglicher
Frequenz wären zwölf der 17 Features über Wochen konstant. Nicht behaupten, täglich wäre
schlechter — es ist eine andere Frage.

---

# Folie 5 — Die 17 Kovariaten und ihre Publikationslags

**SAGEN.**
Was die Modelle sehen. 17 Features pro Land, davon fünf global und identisch über alle
Länder, zwölf lokal, und die lokalen gehen als Differenz zu Deutschland ein — also nicht das
absolute Schuldenniveau Italiens, sondern Italien minus Deutschland.

*Diese Auswahl ist ausdrücklich nicht meine Leistung.* Sie kommt aus einer ECB-Arbeit von
Afonso, Arghyrou und Kontonikas, Working Paper 1781, die Spread-Treiber für genau dieselben
zehn Länder auf derselben Monatsstruktur untersucht. Ich habe vier globale Risikofaktoren
ergänzt, jeder mit eigener Literaturbegründung. Warum ich das betone: wenn ich später zeige,
dass die Kovariaten nichts bringen, ist das ein Ergebnis über die Prognostizierbarkeit — nicht
über meine Featureauswahl.

Jetzt der Teil, der methodisch wichtig ist, die Lags. Ich hatte keinen Zugang zu
Vintage-Daten, also zu den Zahlen, wie sie damals veröffentlicht wurden. Ich arbeite mit
revidierten Daten. *Und eine Revision trägt Information, die es zum Referenzzeitpunkt noch
nicht gab — das ist eine Form von Leakage.*

Die Gegenmaßnahme ist ein Publikations-Lag-Dictionary. Jede Zeitreihe wird um k volle Monate
nach vorne geschoben, wobei k die Publikationsverzögerung in Tagen durch 30 ist,
**aufgerundet**. Der Fiskalblock — Schuldenstand, Fiskalsaldo, Zinsdienst — hat 113 Tage
Verzögerung, wird also um vier Monate geschoben. TARGET2 hat einen Split-Lag: der Zähler ist
in zwei Monaten verfügbar, der GDP-Nenner erst in vier, also verwende ich TARGET2 von t minus
zwei geteilt durch GDP von t minus vier.

Der Preis ist eine Überlagerung von bis zu 29 Tagen. *Das ist bewusst so: mir ist lieber, das
Modell ist zu spät dran, als dass es spickt.*

Eine Restunsauberkeit weise ich aus, statt sie zu verschweigen: die Monatsmittel-Serien, also
Moody's Renditen und der EPU-Index, erscheinen etwa fünf Tage in den Folgemonat hinein. Auf
Monatsfrequenz rundet das auf null Monate Verschiebung, ein Vorlauf von wenigen Tagen bleibt
also. Ich halte das für vernachlässigbar und schreibe es hin.

**TIEFE.** Vollständige Konstruktion je Feature → Anhang A auf Folie 21 und `REF §7`.
- **Warum aufrunden statt abrunden?** Aufrunden garantiert, dass kein Wert im Panel steht,
  bevor er veröffentlicht war. Abrunden hätte im Erwartungswert weniger Verzerrung, aber in
  der Hälfte der Fälle echtes Leakage. Bei einer Anti-Leakage-Maßnahme ist die konservative
  Richtung die einzig vertretbare.
- **Die vier Ergänzungen und ihre Begründung:** `us_aaa_treasury` (Moody's Aaa minus
  US-Treasury) nach Codogno et al., die zeigen, dass US-Kreditrisiko ein eigenständiger
  Spread-Treiber ist; `us_baa_aaa` nach Bernoth et al. als Indikator für
  Investoren-Risikoaversion; `epu_log`, der Economic-Policy-Uncertainty-Index für Europa,
  nach Bernal et al.; und die Interaktion `us_aaa_treasury_x_debt` analog zur
  VIX-mal-Schulden-Interaktion bei Afonso.
- **Warum Abweichung statt Level in den Interaktionen:** `vix_log_x_debt` verwendet die
  Abweichung des log-VIX von seinem expandierenden Mittel, nicht den Level. Sonst wäre der
  Term stark kollinear mit dem reinen Schuldenterm.
- **Vier Substitutionen, die ich erzwungen habe** und die man kennen sollte: realisierte statt
  erwarteter Schulden- und Fiskaldaten (erwartete waren nicht verfügbar); Industrieproduktion
  als monatlicher GDP-Proxy, weil GDP nur quartalsweise vorliegt; nicht saisonbereinigte
  Fiskalreihen mit einem gleitenden Vierquartalsmittel, weil Eurostat für nicht alle zehn
  Länder bereinigte Serien publiziert; und der Verzicht auf Bid-Ask-Spreads und den
  US-High-Yield-OAS, weil beide ohne Lizenz nicht beschaffbar waren.
- **Die Saisonalität ist quantifiziert:** eine Regression des Fiskalblocks auf Quartalsdummies
  ergibt R² = 0.35. Nach dem Vierquartalsmittel liegt es bei etwa null. Die vier Monate Lag
  bleiben erhalten.
- **PC2:** die zweite Hauptkomponente der z-standardisierten Länderdaten, mit 12-Monats-Refit
  auf expandierendem Fenster von mindestens 36 Monaten. Die Ladungen sind so verankert, dass
  Peripherieländer immer positiv laden, damit das Vorzeichen über die Refits stabil bleibt.
  PC2 misst den Core-Periphery-Kontrast, nicht monetäres Risiko.
- **TARGET2 als Kapitalfluchtindikator:** De Grauwe und Ji argumentieren, dass
  Vertrauensverlust zu Bond-Sell-off und dann Liquiditätsabfluss führt. TARGET2-Salden sind
  die buchhalterische Gegenseite dieses Abflusses. Das GDP wird rekonstruiert als
  Schuldenstand in Euro geteilt durch Schuldenstand in Prozent des GDP.

**FALLEN.** Sag nicht „ich habe die Features selbst ausgewählt und optimiert". Das würde den
Kovariaten-Nullbefund auf Folie 16 sofort entwerten — man würde annehmen, die Auswahl war
schlecht. Die Literaturherkunft ist dein Schutz.

---

# Folie 6 — Twelve model variants under one protocol

**SAGEN.**
Zwölf Modellvarianten, aufsteigend nach Kapazität. Random Walk als naive Referenz, ARMA als
linearer sequenzieller Vergleich, LightGBM als nichtlinearer aber nicht-sequenzieller
Vergleich, LSTM als sequenzielles Deep-Learning-Modell, xLSTM als dessen
State-of-the-Art-Erweiterung, und zwei vortrainierte Foundation Models, TiRex und TiRex-2, die
nie auf diesen Daten trainiert wurden.

Dazu drei sogenannte const-width-Zwillinge. Ein Zwilling nimmt exakt dieselbe Punktprognose
und dieselben Kovariaten wie sein Elternmodell, aber mit über die Zeit konstanter
Intervallbreite. *Damit isoliere ich sauber, ob die zeitvariable Breite, die das Modell lernt,
überhaupt etwas beiträgt* — das ist RQ2.

Die Regime rechts: L bedeutet ein Modell pro Land. G bedeutet ein gepooltes Modell über alle
Länder, wobei die Länderidentität als Feature mitläuft — bei den neuronalen Netzen als
gelerntes Embedding, bei LightGBM als kategoriale Variable. GF ist das gepoolte Modell,
anschließend pro Land feingetunt mit einem Zehntel der Lernrate. GI ist das gepoolte Modell
plus ein additiver Niveau-Shift pro Land. ZS ist Zero-Shot, also kein Training auf diesem
Panel.

Das Protokoll: Refit alle zwölf Monate auf expandierendem Trainingsfenster. Monate 1 bis 114
sind das Minimum-Trainingsfenster. Monate 115 bis 150 sind Burn-in — die dienen doppelt, als
Validierungsfenster für die Hyperparameter und als Kalibrierungsfenster für die
Conformal-Schicht. Bewertet werden ausschließlich die Monate 151 bis 277, also 127 Monate, die
kein Modell vorher gesehen hat.

Alle trainierten Modelle haben eine 24-Monats-Obergrenze für den Informationshorizont, damit
keins länger zurückschauen darf als die anderen. *Eine Ausnahme muss ich selbst nennen: TiRex
und TiRex-2 nutzen die volle expandierende Historie. Das ist ein Informationsvorteil für
ausgerechnet die Modellfamilie, die am Ende gewinnt.*

Zur Größenordnung: 742 Kombinationen aus Modell, Regime, Seed und Rekalibrierungsmethode.
*Das ist ausdrücklich kein volles Faktorielles* — zwölf mal fünf mal drei mal vierzehn wären
2520. Die Familien haben unterschiedliche Regime, und nur die neuronalen Modelle laufen mit
drei Seeds. Mal zehn Länder ergibt das 7420 Auswertungszellen.

Und ein Reproduzierbarkeitspunkt, der billig ist und viel sagt: über alle sieben
Modellfamilien ist der SHA-256-Hash des Eingangspanels identisch. Damit ist belegt, dass jedes
Modell dieselben Daten gesehen hat.

**TIEFE.**
- **Warum 12-Monats-Refit und nicht monatlich?** Rechenaufwand, und es ist empirisch geprüft:
  `arma` gegen `arma_monthly` ist der Kontrast F10. Der Median-Effekt über 14 Kontraste ist
  0.65 %, nur 14 % FDR-signifikant. Einschränkung: dieser Check existiert nur für ARMA. Für
  die neuronalen Modelle wäre er prohibitiv teuer. Eine Ausnahme lohnt die Erwähnung: bei
  `cqr_static` ist der Refit-Effekt mit 6.4 % deutlich, Panel-t 39.15 — der statische
  Kalibrierpool reagiert auf die Refit-Frequenz, die adaptiven Verfahren nicht.
- **HPO-Protokoll:** Optuna mit TPE-Sampler, Seed 42, 20 Startup-Trials, MedianPruner, 100
  Trials pro Studie, eine Studie je Regime. Innerhalb eines Trials maximal 100 Epochen mit
  Early Stopping, Patience 15. Die Epochenzahl des Siegertrials wird dann eingefroren und alle
  Refits trainieren genau diese Zahl ohne Early Stopping. Selektionskriterium ist der mittlere
  Pinball-Loss auf der standardisierten Delta-Skala über die Burn-in-Monate.
- **Was tatsächlich gewählt wurde:** xLSTM L mit Lookback 3 und 5 Epochen, G mit Lookback 24
  und 1 Epoche. LSTM L Lookback 3 und 29 Epochen, G Lookback 3 und 10 Epochen. LightGBM L:
  7 Blätter, min. 5 Beobachtungen pro Blatt, Lernrate 0.153, keine Rolling Means. G: 15
  Blätter, Tiefe 3, min. 40 pro Blatt, Lernrate 0.083, Rolling Means an.
- **Seeds:** nur LSTM und xLSTM, Seeds 42/43/44. Die Hyperparameter werden einmal bei Seed 42
  getunt und dann eingefroren, die Seeds unterscheiden sich nur in den Produktionsrefits. Jeder
  Seed ist eine eigene unabhängige Refit-Kette und behält sein Label durch die Conformal- und
  Diagnostikstufe. Vorhersagen werden nie über Seeds gepoolt, bevor die Metriken berechnet sind.
- **Warum es keine GF für LightGBM gibt:** Gradient Boosting hat kein Fine-Tuning-Analogon,
  man kann einen fertigen Booster nicht mit reduzierter Lernrate weitertrainieren, ohne das
  Ensemble zu verändern.
- **Warum die Zero-Shot-Modelle keine const-Zwillinge und kein GI haben:** beide brauchen
  In-Sample-Residuen, und ein Zero-Shot-Modell hat keine. Das ist Absicht, nicht Lücke.
- Modellarchitekturen im Detail → REF §8.

**FALLEN.** Die 24-Monats-Obergrenze niemals als „alle Modelle hatten dieselbe Information"
verkaufen. Sie hatten es *nicht*, und TiRex ist der Begünstigte. Selbst nennen, in einem
Halbsatz, ruhig. Wenn du es verschweigst und jemand liest `experimental_design.tex`, ist der
Vertrauensschaden größer als der Befund wert ist.

---

# Folie 7 — Evaluation, part 1: is the interval valid?

**SAGEN.**
Jetzt die Bewertung, und ich brauche dafür zwei Zahlen. Die erste ist Validität, also: hält
das Intervall?

Empirische Coverage ist einfach der Anteil der Testmonate, in denen der realisierte Spread im
Intervall lag. Nominalziel 0.80. *Der Punkt ist: eine Trefferquote von 0.80 allein reicht
nicht.* Deshalb drei Tests, aufsteigend streng.

Kupiec prüft nur die Rate: ist die Verletzungsquote gleich Alpha? Likelihood-Ratio-Test, Chi²
mit einem Freiheitsgrad. Was er ignoriert: wann die Verletzungen passieren.

Christoffersen prüft zusätzlich, ob die Verletzungen clustern. Ein Intervall, das drei Monate
hintereinander bricht, ist schlechter als eines, das dreimal verstreut bricht — bei
identischer Trefferquote. Der Unabhängigkeitsteil konditioniert auf die vorherige Verletzung,
der Conditional-Coverage-Test kombiniert beides und hat zwei Freiheitsgrade.

Der Dynamic-Quantile-Test von Engle und Manganelli geht weiter: er regressiert die zentrierten
Verletzungsindikatoren auf vier eigene Lags **und auf die Intervallbreite**. Unter der
Nullhypothese ist keine dieser Größen prognostisch. Er ist der strengste, und ich berichte
ihn, benutze ihn aber nicht als Gate — warum, zeige ich auf Folie 11.

Damit zum Zulassungskriterium: *Eine Kombination aus Modell, Regime, Seed und Methode ist
zulässig, wenn Kupiec und Christoffersen-CC beide nicht verworfen werden — nach
Benjamini-Hochberg-Korrektur, also q mindestens 0.10 — und zwar in mindestens 70 Prozent der
zehn Länder.* Nur innerhalb dieser zulässigen Menge wird überhaupt gerankt.

Die vier Designentscheidungen unten habe ich begründet, weil jede angreifbar ist. 70 Prozent
statt aller zehn Länder, weil die zehn Spreads stark korreliert sind und jede Zelle nur 127
Monate hat — ein strengeres Kriterium leert die zulässige Menge. q statt p, weil 7420 Zellen
getestet werden und man ohne Korrektur allein durch Zufall hunderte Verwerfungen bekommt. Und
der wichtigste Punkt: *Kalibrierung gatet, der Winkler-Score rankt. Denn Coverage kann man
trivial maximieren, indem man das Intervall immer breiter macht — als Score wäre sie also
wertlos.*

**TIEFE.** Formeln und Interpretation jedes Tests → REF §2, Korrekturen → REF §3.
Die drei Punkte, die hier am häufigsten nachgefragt werden:

1. **Nicht-Verwerfung ist kein Validitätsbeweis.** Bei 127 Monaten und erwarteten 25
   Verletzungen hat Kupiec moderate Power. Ein Test, der nicht verwirft, sagt „die Daten
   widersprechen der Nullhypothese nicht" — nicht „die Nullhypothese gilt". Das steht so in
   den Limitations und ich sage es von selbst.
2. **Warum 70 Prozent und nicht 80 oder 100.** Es ist eine vorregistrierte Konvention mit
   einem echten Tradeoff: streng heißt, fast alles fällt raus und es gibt nichts zu ranken;
   lax heißt, das Gate ist zahnlos. Bei 70 Prozent fallen 34.6 Prozent der Kombinationen
   durch — das Gate greift also messbar, ohne den Suchraum zu vernichten.
3. **Warum FDR und nicht Bonferroni.** Bonferroni kontrolliert die Wahrscheinlichkeit
   *irgendeines* Fehlers und wäre bei 7420 Tests so konservativ, dass praktisch nichts mehr
   verworfen wird — hier hieße das, jede Kombination gilt als kalibriert, auch die kaputten.
   BH kontrolliert den erwarteten *Anteil* falscher Entdeckungen und ist bei
   Screening-Aufgaben das passende Instrument. → REF §3.2

**FALLEN.** Sag nicht „die zulässigen Kombinationen sind kalibriert". Sag „für die zulässigen
Kombinationen kann ich die Kalibrierungshypothese nicht verwerfen". Bei diesem Publikum ist
der Unterschied hörbar und zählt.

---

# Folie 8 — Evaluation, part 2: how good is the interval?

**SAGEN.**
Die zweite Zahl ist Qualität. Dafür nehme ich den Interval Score, auch Winkler-Score, aus
1972.

Er hat drei Terme. Der erste ist einfach die Breite, obere minus untere Grenze. Der zweite und
dritte sind Strafen: liegt die Realisierung unter dem Intervall, wird der Abstand mit zwei
durch Alpha multipliziert und aufgeschlagen; liegt sie darüber, dasselbe nach oben. *Ein
schmales Intervall kostet also nichts, solange es hält — und wenn es bricht, kostet es
proportional zum Abstand.*

Vier Eigenschaften, die zählen. Er ist eine strictly proper scoring rule: das Minimum liegt
genau bei den wahren Quantilen der datengenerierenden Verteilung. Damit ist das Ranking nicht
ad hoc, sondern hat eine theoretische Rechtfertigung. Er ist ein Verlust, kleiner ist besser.
Er skaliert mit dem Abstand, im Gegensatz zu einer reinen Trefferquote, die einen Fehltreffer
um fünf Basispunkte und einen um fünfzig gleich behandelt. Und er ist über Modelle hinweg
nicht additiv zerlegbar — das heißt, ich kann eine Verbesserung nicht auf eine einzelne
Quelle zurückführen.

Jetzt die Leseregel, und die ist wichtig, weil sonst jede folgende Tabelle falsch gelesen
wird. *Der Skill ist eins minus mittlerer Winkler des Modells geteilt durch mittleren Winkler
des Random Walk mit seinen eigenen Quantilen, gerechnet pro Land, dann Median über die zehn
Länder. Skill größer null heißt besser als der Random Walk.*

Warum pro Land und nicht gepoolt: die Spread-Niveaus unterscheiden sich um den Faktor 9.5
zwischen den Niederlanden und Griechenland. Würde ich rohe Winkler-Werte über Länder mitteln,
würde Griechenland jedes Aggregat dominieren.

Rechts das Punktprognosemaß, weil es auf Folie 10 auftaucht: das MAE-Verhältnis ist der
absolute Fehler der Medianprognose im Level-Raum geteilt durch den des Random Walk, pro Land,
dann Median. Verhältnis kleiner eins heißt besser als der Random Walk. RMSE analog.

**TIEFE.** Vollständig → REF §1.
- **„Strictly proper" präzise:** Eine Scoring Rule ist proper, wenn der erwartete Score unter
  der wahren Verteilung durch Angabe der wahren Verteilung minimiert wird. Strictly proper
  heißt, das Minimum ist eindeutig. Praktische Konsequenz: es gibt keine Strategie, den Score
  durch Falschangabe zu verbessern. Genau das macht ihn manipulationsresistent — ein Modell
  kann nicht durch systematisch zu breite oder zu schmale Intervalle gewinnen.
- **Warum 2/α als Strafgewicht:** damit die Scoring Rule proper ist. Bei α = 0.20 ist der
  Faktor 10. Eine Verletzung um 0.1 Prozentpunkte kostet also 1.0 im Score, während die
  gesamte mittlere Breite bei 0.33 liegt — eine einzige größere Verletzung kann den Monat
  dominieren.
- **Common Sample:** der Skill wird nur auf den Monaten gerechnet, für die beide Verfahren
  eine Prognose haben. Ohne diese Einschränkung würde man Verfahren auf verschiedenen
  Monatsmengen vergleichen, und ein Verfahren, das ausgerechnet die Krisenmonate auslässt,
  sähe künstlich gut aus.
- **Median statt Mittel über Länder:** robuster gegen Griechenland und Irland, die beide
  Ausreißer sind. Preis: der Median ist nicht linear, deshalb ist die Differenz zweier
  Mediane nicht der Median der Differenzen. Genau deshalb weichen auf Folie 16 die
  Δ-Skill-Spalte und die paarweise DM-Spalte im Vorzeichen ab.
- **Pinball-Loss ist etwas anderes:** das ist die *Trainings*-Zielfunktion der
  quantilschätzenden Modelle, nicht das Bewertungsmaß. → REF §1.6

**FALLEN.** Die Leseregel wirklich laut sagen, nicht nur zeigen. Wenn ein Zuhörer „Skill" als
Fehler statt als Verbesserung liest, hält er ab Folie 12 alles für invertiert und meldet sich
nicht.

---

# Folie 9 — The conformal layer

**SAGEN.**
Das Prinzip kennen Sie von mir schon, deshalb nur kurz und dafür präzise, was ich tatsächlich
rechne.

Die Modellquantile werden als unverbindlich behandelt. Aus dem jüngsten Verlauf, wie weit die
Realisierung außerhalb des Intervalls lag, wird eine Korrektur bestimmt, sodass die empirische
Coverage auf 0.80 zuläuft. *Verteilungsfrei — es gibt keine Annahme über die
Fehlerverteilung.* Formal ist es ein Regelkreis: Regelgröße ist der kumulierte
Coverage-Fehler, Stellgröße die Intervallbreite.

Vierzehn Methoden in sieben Klassen. Die Baseline `native` ist gar keine Conformal-Methode,
sondern die rohe Modellausgabe mit Korrektur null — ich brauche sie, um den Effekt der Schicht
zu isolieren. `cqr_static` ist das klassische Split-CQR: die Schwelle wird einmal auf den 36
Burn-in-Scores bestimmt und dann eingefroren. Das ist die einzige Methode mit einer
Finite-Sample-Garantie, allerdings unter Exchangeability. Die gefensterten Verfahren rechnen
monatlich neu. Die ACI-Familie regelt statt der Schwelle das effektive Alpha. SPCI dreht die
Logik um und *prognostiziert* das nächste Score-Quantil. Und die Online-Verfahren führen einen
Zustand fort statt einen Pool zu lesen.

Der wichtigste Satz dieser Folie steht in der unteren Tabelle. Drei Disziplinpunkte:
*Erstens, ein einziger Nonconformity-Score für alle — der CQR-Score, identisch für jedes
Modell und jede Methode. Zweitens, null getunte Parameter: jede Schrittweite, jedes Fenster,
jeder Regler-Gain ist ex ante aus der Originalarbeit übernommen und nichts auf diesen Daten
gefittet. Drittens, die Schicht liegt downstream — die Modell-Notebooks geben nur rohe
Quantile aus und enthalten keinerlei Kalibrierungscode.*

Zusammen heißt das: ein Unterschied in der Intervallqualität ist ein Unterschied im Modell
und nicht in der Kalibrierung.

**TIEFE.** Alle 14 Methoden mechanisch → REF §9, Score und Pool → REF §10.
Die Fragen, die hier kommen:
- **Was ist Exchangeability und warum ist sie hier verletzt?** Austauschbarkeit heißt, die
  gemeinsame Verteilung der Scores ist invariant unter Permutation. Zeitreihen mit
  Volatilitätsclustern erfüllen das nicht: ein Score aus einem Stressmonat ist nicht
  austauschbar mit einem aus einem ruhigen. Deshalb geben die Online-Verfahren die
  Finite-Sample-Garantie auf und tauschen sie gegen Langfrist-Coverage. Das ist der
  eigentliche methodische Kern der ganzen Arbeit.
- **Was ist der CQR-Score genau?** E_t = max(q̂_0.1 − y_t, y_t − q̂_0.9). Positiv heißt, die
  Realisierung lag außerhalb des Rohintervalls; negativ heißt, das Rohintervall war zu breit.
  Er fasst beide Ränder in eine Zahl zusammen. Auf der Level-Skala, nach der monotonen
  Rearrangierung.
- **Warum symmetrisch aufgeschlagen?** Dieselbe Zahl Q_t wird unten abgezogen und oben
  addiert. Das ist eine Designentscheidung, keine Notwendigkeit — und genau die strukturelle
  Grenze, die auf Folie 14 sichtbar wird.
- **Kalibriert wird pro Land**, nie über Länder gepoolt, auch nicht für die global trainierten
  Modelle. Begründung: ein gepoolter Threshold wäre für Österreich zu breit und für
  Griechenland zu eng.

**FALLEN.** Nicht sagen „Conformal Prediction garantiert 80 Prozent Coverage". Die
Finite-Sample-Garantie gilt nur für `cqr_static` und nur unter Exchangeability, die hier
verletzt ist. Die Online-Verfahren garantieren Langfrist-Coverage, also asymptotisch. Der
Unterschied ist in diesem Raum relevant.

---

# Folie 10 — Point forecasts: no model beats the random walk

**SAGEN.**
Das erste Ergebnis, und es ist ein negatives. Links die Tabelle, rechts oben was die Spalten
sind — die lese ich einmal vor, damit die Zahlen einordbar sind.

Das MAE-Verhältnis ist der mittlere absolute Fehler der Medianprognose, geteilt durch den des
Random Walk, pro Land gerechnet, dann Median über die Länder. Kleiner eins heißt besser als
der Random Walk.

*In der ganzen Tabelle liegt genau ein Modell unter eins: TiRex-2 mit 0.997.* Das ist ein
Vorsprung von 0.3 Prozent. Der Random Walk selbst ist per Konstruktion 1.000. Und darunter
kommt alles andere — ARMA 1.017, LSTM 1.061, LightGBM 1.065, xLSTM 1.104. *Jedes Modell, das
ich selbst trainiert habe, ist schlechter als gar nichts zu tun.*

Jetzt die Frage, die man stellen muss: sind die 0.3 Prozent echt? Rechts unten der Test. Ein
Diebold-Mariano-Test mit Harvey-Leybourne-Newbold-Korrektur, aggregiert als Panel-t über die
zehn Länder, ergibt minus 0.498. Kein einziges von zehn Ländern übersteht die
FDR-Korrektur. Und nur fünf von zehn Ländern haben überhaupt ein Verhältnis unter eins.

*Und jetzt der Punkt, der diesen Nicht-Befund belastbar macht: derselbe Test auf ARMA gegen
Random Walk ergibt Panel-t plus 2.447 — ARMA ist signifikant schlechter, Finnland mit q gleich
0.004. Der Test hat also Power auf dieser Stichprobe. Er findet bei TiRex-2 nichts, weil da
nichts ist.* Das ist eine echte Nicht-Ablehnung und keine Power-Schwäche.

Die Richtungsprognose bestätigt das. Mittlere Trefferquote 0.540 über alle Zellen, und der
Pesaran-Timmermann-Test ist in 7.2 Prozent der Zellen auf fünf Prozent signifikant — bei
reinem Zufall erwartet man fünf Prozent.

Eine Zahl muss ich als Artefakt kennzeichnen, bevor Sie sie finden: der Random Walk hat mit
0.578 die höchste Richtungstrefferquote im Feld. Das ist kein Befund. Seine Delta-Prognose ist
konstant null, damit wird der Nenner der PT-Statistik exakt null und der Test ist in 90 Prozent
seiner Zellen undefiniert. Die Zahl ist schlicht der Anteil der Monate ohne Spread-Anstieg.

Merken Sie sich diesen Nicht-Befund — ich komme bei der Kontaminationsfrage darauf zurück.

**TIEFE.** DM, HLN, Panel-t → REF §5. Pesaran-Timmermann → REF §4.
- **Warum ein Kontrollexperiment nötig war:** Ein Nicht-Befund ist wertlos, wenn man nicht
  zeigt, dass der Test überhaupt etwas finden könnte. Das ARMA-Ergebnis ist die Positivkontrolle.
- **Die vollständige Interpretation der Spalten:** `PT rejections` ist der Anteil der Zellen
  eines Modells, in denen PT auf fünf Prozent verwirft — nicht ein p-Wert. Beim Random Walk
  steht ein Strich, weil der Test dort undefiniert ist; „0 Prozent signifikant" würde
  fälschlich als „keine Evidenz" gelesen.
- **RMSE gegen MAE:** RMSE bestraft große Fehler stärker. Dass beide Verhältnisse fast gleich
  laufen (TiRex-2 0.997 gegen 0.999), heißt, der Unterschied liegt nicht in wenigen
  Ausreißermonaten.
- **`tirex2_cov` liegt bei 1.001**, also minimal über eins und schlechter als die univariate
  Variante. Das ist der erste Hinweis auf Folie 16.
- **Warum die trainierten Modelle *schlechter* sind statt nur gleich gut:** sie schätzen
  Parameter auf 277 Monaten. Jede Schätzung bringt Schätzunsicherheit. Wenn im Signal nichts
  drin ist, ist der Random Walk mit null Parametern die bessere Wahl — das ist der klassische
  Bias-Varianz-Kompromiss in seiner härtesten Form.
- **F12 ist leer**: der vorregistrierte Findings-Typ „Punkt-Intervall-Divergenz" verlangt ein
  MAE-Verhältnis unter 0.98 bei negativem Skill. Bestes Verhältnis 0.9965 — unerfüllbar. Aktiv
  berichten, es belegt die Vorregistrierung.

**FALLEN.**
- Sag nicht „TiRex-2 ist bei der Punktprognose besser". Es ist nicht unterscheidbar von gleich
  gut. Der Unterschied ist auf Folie 19 die Grundlage deines Kontaminations-Gegenarguments.
- Interpretiere die 0.578 des Random Walk nicht. Nenn sie als Artefakt und geh weiter.

---

# Folie 11 — Raw model quantiles are too wide and rarely valid

**SAGEN.**
Jetzt das erste positive Ergebnis, und es ist das praktisch relevanteste der ganzen Arbeit.

Links alle vierzehn Rekalibrierungsmethoden mit drei Spalten: dem Anteil ihrer Kombinationen,
die das Gate passieren, ihrer mittleren Coverage, und der Ablehnrate im DQ-Test.

Von 742 Kombinationen sind 485 zulässig, also 65.4 Prozent. *Das Gate ist also kein
Formalismus, es sortiert ein gutes Drittel aus.*

Die eigentliche Story ist die Verteilung. *Die rohe Modellausgabe, `native`, passiert das Gate
in 13.2 Prozent der Fälle.* Ihre mittlere Coverage ist 0.872 bei einem Ziel von 0.800. Fünf
der vierzehn Methoden — dtaci, pid, pid_local, sfogd und spci — passieren in 100 Prozent.

Und jetzt kommt es auf die Richtung an. *Es ist Überdeckung, nicht Unterdeckung. Die Modelle
sind nicht zu optimistisch, sie sind zu konservativ. Ihre Intervalle sind unnötig breit — und
Breite ist teuer.* Das ist exakt die Größe, die ich auf Folie 17 als Kapitalquotient messe.

Der Länderschnitt in einem Satz: Griechenland mit 0.519 und Portugal mit 0.616 sind am
schwersten zu kalibrieren, Frankreich mit 0.864 und Finnland mit 0.844 am leichtesten. Die
Peripherie mit ihren sprunghaften Spreads ist das harte Problem.

Jetzt der Teil, den ich von selbst offenlege, bevor Sie fragen: warum benutze ich den
DQ-Test nicht als Gate? Weil er 49.3 Prozent aller Zellen verwirft. Wenn ich ihn ins Gate
nehme, bleibt praktisch nichts übrig. Aber die interessante Frage ist, *woran* er scheitert.
Die Zerlegung steht unten rechts: nehme ich den Breiten-Regressor heraus, fällt die
Ablehnrate von 49.3 auf 20.8 Prozent — also praktisch auf das unkonditionale Niveau. Nur der
Breiten-Regressor allein verwirft 68.9 Prozent.

*Die Ablehnungen sind also fast vollständig ein Breiten-Phänomen und kein Clustering-Phänomen.*
Und die Richtung ist eindeutig: von den signifikanten Breitenkoeffizienten sind 99.9 Prozent
negativ, Median minus 1.631. Das heißt, in Monaten mit breitem Intervall wird überdeckt, in
Monaten mit schmalem unterdeckt. *Die Breitensteuerung reagiert in die richtige Richtung, aber
sie überschießt in beide.* Marginal mittelt sich das auf 0.80 heraus — deshalb sieht Kupiec
nichts.

Der Satz, mit dem ich das zusammenfasse: die Conformal-Schicht tauscht zeitliche Abhängigkeit
der Verletzungen gegen breitenbedingte Abhängigkeit.

**TIEFE.** DQ-Test formal → REF §2.4.
- **Wie die Zerlegung gerechnet ist:** dieselbe Wald-Statistik, dieselbe BH-Familie, nur mit
  reduzierter Instrumentenmatrix. Volles Z hat sechs Spalten (Konstante, vier Lags, Breite) →
  Chi²(6). Ohne Breite fünf Spalten → Chi²(5). Nur Breite zwei Spalten → Chi²(2). Der
  Koeffizient allein → Chi²(1), verwirft in 53.2 Prozent. Post hoc gerechnet am 2026-08-03 und
  als Amendment deklariert.
- **Die Gegenprobe von der anderen Seite:** die Christoffersen-Unabhängigkeitsrate liegt bei
  nur 6.1 Prozent. Zeitliches Clustering ist also nachweisbar *nicht* das Problem. Zwei
  unabhängige Wege zum selben Schluss.
- **Zwei saubere Fehlermodi**, die man als Ergebnis verkaufen kann: die statischen Verfahren
  (native, cqr_static, saocp, decay_ocp) haben geclusterte Verletzungen — Lag-Ablehnraten 40
  bis 54 Prozent — bei unauffälliger Breite, 7 bis 20 Prozent. Sie adaptieren nicht. Die
  Online-Tracker beseitigen das Clustering fast vollständig, pid_cqr auf 1.5 Prozent, und
  erzeugen dafür die überschießende Breitenreaktion, 50 bis 97 Prozent.
- **Warum `saocp` mit 0.917 so konservativ ist:** die Referenzimplementierung klemmt die
  Radien bei null. Auf signierten CQR-Scores heißt das, die Methode kann das Rohintervall nur
  verbreitern, nie verengen. Das ist eine Eigenschaft ihrer Domänendefinition und kein Beleg,
  dass sie besser wäre. Wichtig, weil sonst „0.917 Coverage" nach Qualität aussieht.
- **Quantilkreuzung**, falls gefragt: tritt in 0.451 Prozent aller kalibrierten Intervalle
  auf, 0.398 Prozent bei den zulässigen. 81.7 Prozent aller Zellen kreuzen nie. `native` und
  `saocp` haben exakt null — bei native per Konstruktion, bei saocp wegen der nichtnegativen
  Radien. `pid_tan` ist mit 2.554 Prozent Spitzenreiter, `cqr_static` liegt bei 0.724 Prozent
  im Mittel, hat aber Extremfälle bis 77.95 Prozent (xlstm/L/Seed 42/Irland) und 32.3 Prozent
  bei TiRex-2 in Portugal.

**FALLEN.** Die DQ-Zahl nicht verschweigen und nicht kleinreden. Wenn du „49.3 Prozent
Ablehnung" selbst auf die Folie schreibst und erklärst, ist es Souveränität. Wenn ein Zuhörer
es in der Arbeit findet, ist es ein Vorwurf.

---

# Folie 12 — Interval quality: the best package per model family

**SAGEN.**
Das Kernergebnis. Links pro Modellfamilie das beste zulässige Paket, vollständig
spezifiziert als Modell, Regime und Rekalibrierungsmethode, mit Median-Winkler-Skill,
mittlerer Coverage und der Zahl der Länder, in denen es zulässig ist.

*An der Spitze steht TiRex-2, Zero-Shot, mit cqr_static: Skill 0.146.* Also 14.6 Prozent
weniger Winkler-Verlust als der Random Walk. Auf Platz zwei die Kovariaten-Variante desselben
Modells mit 0.141, auf Platz drei LightGBM global mit decay_ocp bei 0.139.

*Das erste selbst trainierte Modell ist also LightGBM, und der beste klassische Ansatz — der
Random Walk mit ACI-Kalibrierung — kommt auf 0.085 und ist in allen zehn Ländern zulässig.*

Jetzt die Einschränkung, und die gehört in denselben Atemzug. Unten die Vergleichstabelle.
`cqr_static` hat den höchsten Skill, aber: die Coverage ist 0.750, also fünf Prozentpunkte
unter dem Ziel. Es passiert das Gate nur in acht von zehn Ländern. Und seine Kreuzungsrate ist
0.724 Prozent im Mittel, in Extremfällen bis 77.95 Prozent.

*Die robuste Alternative ist dieselbe Modellfamilie mit decay_ocp: Skill 0.144, also 0.002
weniger, aber Coverage 0.787 — anderthalb Prozentpunkte statt fünf vom Ziel entfernt —
zulässig in allen zehn Ländern, und Kreuzungsrate 0.028 Prozent.* Wenn ich eine Empfehlung
aussprechen müsste, wäre es diese. Für 0.002 Skill kaufe ich Validität überall und ein
Zehntel der Kreuzungsrate.

Rechts derselbe Punkt aus einem anderen Winkel: dieselbe Modellfamilie TiRex-2 mit allen
vierzehn Methoden. *Der Skill reicht von minus 0.044 bis plus 0.146. Die Wahl der
Kalibrierungsmethode ist für das Ergebnis also ungefähr so wichtig wie die Wahl des Modells* —
und sie ist der billigere Hebel von beiden.

Und ein Detail, das die Spannung benennt: dasselbe Modell, das auf Folie 10 bei der
Punktprognose exakt auf Random-Walk-Niveau lag, erzeugt die besten Intervalle. Der Vorsprung
liegt vollständig in der Form der prädiktiven Verteilung, nicht in ihrem Zentrum.

**TIEFE.**
- **Was „bestes zulässiges Paket je Familie" bedeutet und warum es optimistisch ist:** Für
  jede Familie wird über alle ihre zulässigen Kombinationen das Maximum des Median-Skills
  genommen — auf denselben Testdaten, auf denen anschließend verglichen wird. Das ist
  Selektion auf dem Testset. Und sie ist ungleich: `lstm` wurde aus 105 zulässigen Paketen
  gewählt, `rw` aus 9. Der `lstm`-Wert ist damit stärker nach oben verzerrt.
- **Strengeres Gate:** Verlangt man Zulässigkeit in allen zehn Ländern, ändern sich vier der
  zwölf Auswahlen — tirex2 zu decay_ocp (0.144), tirex2_cov zu decay_ocp (0.139), lgbm zu
  agaci (0.128), xlstm zu cqr_rolling (0.068). Die Ordnung der Spitzengruppe bleibt, aber
  xLSTM rutscht dann unter den Random Walk.
- **Warum Zero-Shot gewinnt, als Hypothese:** es gibt keine Parameterschätzunsicherheit, weil
  nichts geschätzt wird. Die prädiktive Verteilung kommt aus einem Pretraining auf Millionen
  Zeitreihen. Dagegen steht der Kontaminationsverdacht → Folie 19.
- **Der Skill je Land** streut stark: Irland 0.231, Portugal 0.191, Griechenland 0.120,
  Spanien 0.095, Belgien 0.051, dann Niederlande −0.002, Österreich −0.015, Frankreich −0.047,
  Finnland −0.052, Italien −0.070. Ausgerechnet in Italien, dem größten Markt, verliert das
  Feld im Median gegen den Random Walk. Das ist erwähnenswert, wenn jemand nach Anwendbarkeit
  fragt.
- **Länderkonsistenz:** die mittlere paarweise Kendall-Rangkorrelation der
  Kombinationsrankings zwischen Ländern ist τ = 0.35, mit einer Spanne von 0.073 (Finnland
  gegen Griechenland) bis 0.650 (Spanien gegen Portugal). Es gibt also nicht eine Geschichte,
  sondern eher zehn — was die zurückhaltende Formulierung rechtfertigt.

**FALLEN.**
- Nenn nie nur das Modell. Immer Modell/Regime/Methode. „TiRex-2 gewinnt" ist unvollständig
  und lädt genau die Nachfrage ein, die du auf Folie 11 schon beantwortet hast.
- Sag nicht „TiRex-2 ist besser als der Random Walk". Sag „hat den höheren Skill". Der
  Unterschied ist Folie 19, Punkt 2.

---

# Folie 13 — The namesake architecture is the weakest family

**SAGEN.**
Diese Folie ist unbequem, und deshalb steht sie drin. Das xLSTM ist die Architektur, die der
Arbeit den Namen gibt, und sie ist die schwächste der zwölf Familien.

Der Median-Skill über alle ihre Kombinationen ist minus 0.047 — Platz zwölf von zwölf. Ihr
bestes zulässiges Paket ist xLSTM lokal mit `native`, also mit Skill 0.094 und ausgerechnet
*ohne* die Conformal-Schicht. Im GI-Regime ist der Skill offen negativ.

Und dann kommt der Teil, den ich selbst aufgedeckt habe. Links unten dasselbe Paket, nach
Seed aufgeschlüsselt. *Seed 42 liefert 0.043 und fällt durchs Gate. Seed 43 liefert minus
0.008 und fällt durchs Gate. Seed 44 liefert 0.094 und passiert. Die Spannweite über die drei
Seeds ist 0.102 — größer als der berichtete Skill selbst.*

Das heißt: der Wert, der in der Ranking-Tabelle steht, ist der beste von drei Seeds, und die
anderen zwei sind nicht einmal zulässig. Das argmax einer Ranking-Tabelle wählt still den
Gewinner-Seed. *Ich hatte vorher festgelegt, dass ich für solche Kombinationen keine
Rangaussage treffe — und das gilt hier.*

Rechts oben, dass das kein Einzelfall ist: über alle neuronalen Kombinationen übersteigt die
Seed-Spannweite in 51.7 Prozent der Fälle den Betrag des Skills. Beschränkt auf die
zulässigen steigt der Anteil auf 58.6 — das Gate filtert es also nicht heraus, es macht es
schlimmer. Und die Streuung zwischen den Familien ist groß: `lstm` 68.5 Prozent, `lstm_const`
nur 23.8.

Zur Erklärung, und ich kennzeichne das ausdrücklich als Hypothese: 277 Monate mal zehn Länder
sind eine kleine Stichprobe für ein Modell dieser Kapazität. Zwei Beobachtungen sprechen für
ein Daten- und gegen ein Implementierungsproblem. Erstens läuft das LSTM mit identischem
Protokoll und identischer Pipeline besser — der Unterschied ist ausschließlich die Zelle.
Zweitens ist das Versagen seed-abhängig und nicht systematisch; ein Codefehler wäre
systematisch.

**TIEFE.**
- **Warum das LSTM die richtige Ablation ist:** LSTM und xLSTM teilen das gesamte
  Trainingsprotokoll, die Datenpipeline, den Quantilkopf, die HPO-Prozedur und die Seeds. Sie
  unterscheiden sich ausschließlich in der rekurrenten Zelle. Ein Skill-Unterschied kann also
  nur von der Zelle kommen.
- **Was die deployte Konfiguration tatsächlich ist:** mLSTM-only, also xLSTM[1:0] in L und
  xLSTM[3:0] in G. *Memory Mixing ist im eingesetzten Modell also nicht vorhanden*, weil das
  nur in sLSTM-Blöcken auftritt. Wenn jemand nach sLSTM fragt: implementiert, aber von der HPO
  nicht gewählt. Das ist ein fairer Kritikpunkt an meinem Setup.
- **Was xLSTM eigentlich bringen sollte:** exponentielles Gating statt Sigmoid, damit das
  Modell schnell umlernen kann. Meine Erwartung war, dass genau das bei Spread-Regimewechseln
  hilft. Die Messung stützt das nicht. → REF §8.5
- **Die Seed-Disziplin ist keine nachträgliche Ausrede:** die Regel „keine Rangaussage, wenn
  die Seed-Spanne den Effekt übersteigt" steht in `diagnostics_foundation.txt` §4.4, also vor
  der Auswertung.
- **Ein fairer Gegenvergleich, den du nennen solltest:** `lgbm/G/decay_ocp` hat mit 7 von 10
  dieselbe schwache Gate-Rate wie das xLSTM-Paket, ist aber deterministisch. Dort gibt es
  keine Seed-Frage, der Befund ist trotz gleicher Gate-Rate belastbarer. Das zeigt, dass du
  die Fälle unterscheidest und nicht pauschal urteilst.

**FALLEN.**
- Nicht entschuldigend sprechen. Kein „leider", kein „hat nicht funktioniert". Der Tonfall ist
  derselbe wie bei den positiven Ergebnissen: das ist eine Messung.
- Nicht sagen „xLSTM funktioniert nicht". Sag „auf diesem Panel, mit dieser Stichprobengröße,
  in diesem Protokoll". Die Architektur ist auf anderen Daten sehr erfolgreich.

---

# Folie 14 — What drives the Winkler score

**SAGEN.**
Warum das Ranking so aussieht, wie es aussieht. Oben noch einmal die Winkler-Formel, damit die
Zerlegung darunter direkt zuordenbar ist — jede Tabellenzeile ist ein Term der Formel.

Die mittlere Breite über die zulässigen Zellen ist 0.3301. Die Unterschreitungsstrafe 0.0521,
die Überschreitungsstrafe 0.0715. *Die Breite macht damit 72.8 Prozent des mittleren Scores
aus.*

Das heißt: das Ranking von Folie 12 ist in erster Ordnung ein Schärfe-Ranking. *Und genau
deshalb muss die Kalibrierung das Ranking gaten und darf nicht darin aufgehen — sonst gewinnt
das schmalste Intervall, unabhängig davon, ob es hält.*

Jetzt der interessantere Teil, unten. Die Tail-Asymmetrie ist der Anteil der unteren
Verletzungen an allen Verletzungen; 0.5 heißt symmetrisch. Über alle Zellen liegt sie bei
0.5049, der Median bei genau 0.5000. *Der Anzahl nach sind die Verletzungen also praktisch
symmetrisch verteilt.*

Die Strafen sind es nicht. 0.0715 oben gegen 0.0521 unten, das sind 37 Prozent Übergewicht
nach oben. *Gleich häufig — aber wenn der Spread aus dem Intervall läuft, dann nach oben und
weiter.*

Das ist konsistent mit der bekannten Rechtsschiefe von Kreditspreads: Ausweitungen sind
sprunghaft, Einengungen graduell. Ich formuliere das als Beobachtung und nicht als kausale
Aussage — das Design testet es nicht.

Die Konsequenz ist strukturell: *der CQR-Score ist per Konstruktion symmetrisch und kann diese
Asymmetrie nicht abbilden.* Über alle vierzehn Methoden bewegt sich die Asymmetrie nur zwischen
0.47 und 0.56. Die Rekalibrierung korrigiert also das Niveau der Coverage, aber nicht ihre
Schiefe. Das ist mein erster Punkt bei den nächsten Schritten.

**TIEFE.**
- **Achtung, zwei Grundgesamtheiten:** Breite und Strafen sind über die *zulässigen* Zellen
  gerechnet, die Tail-Asymmetrie 0.5049 über *alle* Zellen. Auf den zulässigen Zellen liegt
  die Asymmetrie bei 0.5359. Beide Werte stehen auf der Folie. Wenn jemand nachfragt: die
  Aussage „praktisch symmetrisch" trägt über alle Zellen; auf den zulässigen ist sie schwächer.
- **Warum die Breite dominiert:** bei α = 0.20 tritt eine Verletzung nur in etwa einem Fünftel
  der Monate auf, die Breite zählt in jedem Monat. Auch bei Strafgewicht 10 überwiegt der
  ständige Term.
- **Streuung der Asymmetrie:** je Modellfamilie von xLSTM 0.483 bis arma_monthly 0.618. Je
  Land von Finnland 0.431 bis Irland 0.642. *Die Länderheterogenität ist größer als die
  Modellheterogenität* — was zum Kendall-Befund τ = 0.35 passt und ihn von einer zweiten Seite
  stützt.
- **Was eine asymmetrische Score-Familie wäre:** getrennte Kalibrierung der unteren und
  oberen Grenze, also zwei Thresholds statt einem symmetrischen. Das Original-SPCI-Paper
  optimiert eine Asymmetrie mit; ich habe darauf verzichtet, um das Interface über alle
  vierzehn Methoden einheitlich zu halten. Das ist eine bewusste Designentscheidung mit Kosten.

**FALLEN.** Nicht kausal formulieren. „Weil Spreads rechtsschief sind, sind die Strafen
asymmetrisch" ist eine Behauptung, die mein Design nicht stützt. Richtig: „das passt zur
bekannten Verteilungsform".

---

# Folie 15 — RQ1 und RQ2

**SAGEN.**
Meine beiden anderen vorregistrierten Fragen, kompakt. Die Kennzahl in beiden Tabellen ist ein
normalisiertes Diebold-Mariano-Verlustdifferenzial: das mittlere Verlustdifferenzial zwischen
A und B, geteilt durch den Referenz-Winkler, Median über die zehn Länder. *Negativ heißt A ist
besser.*

Links RQ1, Pooling. Global gegen lokal ergibt plus 0.017 — das heißt, das Modell pro Land hat
den geringeren Verlust. *Pooling über Länder hilft den neuronalen Modellen also nicht.*
Fine-Tuning holt 0.006 davon zurück, bleibt aber mit plus 0.016 hinter dem lokalen Modell. Die
Kette ist in sich stimmig.

LightGBM ist die Ausnahme und gewinnt durch Pooling, minus 0.014. Meine Erklärung, als
Hypothese: Bäume können auf die kategoriale Ländervariable splitten, während ein
Embedding bei 277 Monaten pro Land zu wenig Signal bekommt. *Und ich muss dazu sagen, dass die
naheliegende Erklärung — Skalenheterogenität — nicht ausreicht: LightGBM sieht exakt dieselbe
Heterogenität und gewinnt trotzdem.* Die Erklärung muss also architekturspezifisch sein.

Der sauberste Einzelbefund ist GF gegen GI, minus 0.054. Beides sind Wege, dem globalen Modell
zusätzliche Länderspezifik zu geben: Gewichte anpassen oder nur das Niveau verschieben.
Gewichte gewinnen klar. Beim xLSTM sind es minus 0.060 mit dem stärksten Panel-t der ganzen
Familie, minus 4.93 — und es ist der einzige RQ1-Kontrast, der über alle drei Seeds
vorzeichenstabil ist.

Rechts RQ2, die modelleigene Breitenadaptivität gegen den Konstant-Breiten-Zwilling. *Der
Median über alle 280 Kontraste ist plus 0.020, die adaptive Variante ist also schlechter.* Die
Aufschlüsselung ist aber dreigeteilt und das ist der eigentliche Inhalt. Beim LSTM ist adaptiv
klar schlechter: plus 0.062 in G und GF, in nur zehn Prozent der Länder besser, Panel-t über
drei, und seed-stabil zu 86 beziehungsweise 79 Prozent. Bei LightGBM ist adaptiv leicht
besser. Beim xLSTM ist es *nicht entscheidbar* — die drei Seeds stimmen nur in 14 bis 21
Prozent der Kontraste im Vorzeichen überein, also darf ich keine Richtungsaussage treffen.

Und der Satz, der beide Fragen verbindet: *die nützliche Adaptivität kommt aus der
Conformal-Schicht, nicht aus dem Modell.* Die Online-Regler schlagen die native Ausgabe um
minus 0.054 — etwa das Zehnfache dessen, was die modelleigene Breitenadaptivität bewegt.

Eine Einschränkung für alles auf dieser Folie: *kein einziger Kontrast ist auf Kontrastebene
FDR-signifikant.* Von 266 vorregistrierten RQ1-Kontrasten fallen 30 unter q gleich 0.10, das
kleinste q ist 0.019, gleichzeitig haben 131 einen Panel-t über 1.96 im Betrag. Die beiden
Kriterien beantworten verschiedene Fragen, und ich formuliere alle Befunde als gerichtet, nicht
als signifikant.

**TIEFE.** DM/HLN/Panel-t im Detail → REF §5. Die Regime L/G/GF/GI/ZS → REF §8.8.
- **Warum Panel-t und q-Wert auseinanderlaufen:** Der Panel-t bildet zuerst den
  Querschnittsmittelwert des Verlustdifferenzials pro Monat und testet dann diese eine
  Zeitreihe — er fragt „ist der Effekt im Mittel über die Länder da?". Der q-Wert kommt aus
  den zehn separaten Ländertests und fragt „ist er *innerhalb* eines Landes nach Korrektur
  nachweisbar?". Bei zehn korrelierten Ländern und 127 Monaten fällt die zweite Frage negativ
  aus, weil jedes einzelne Land zu wenig Daten hat. Das ist kein Widerspruch, sondern zwei
  Auflösungsstufen.
- **GF gegen L ist post hoc**, mit eigener BH-Familie von 84 Kontrasten, damit die q-Werte der
  vorregistrierten Kontraste unverändert bleiben. d_norm plus 0.016, Median-q 0.527, also
  schwach. Formuliere es als Richtungsbestätigung, nicht als Beleg.
- **Die Prämisse der Heterogenitäts-Hypothese ist belegbar:** Spread-Niveau im Median von
  Niederlande 0.201 bis Griechenland 1.907, Faktor 9.5. Volatilität, also Standardabweichung
  der Änderungen, von 0.042 bis 0.372, Faktor 8.8.
- **Was ein const-width-Zwilling technisch ist:** identische Punktprognose, identische
  Kovariaten, aber die Intervallbreite kommt aus den In-Sample-Residuenquantilen und ist über
  die Zeit konstant. Deshalb existieren Zwillinge nur für trainierbare Modelle.
- **Warum die gelernte Breite nichts beiträgt, als Hypothese:** die Modelle schätzen drei
  Quantile per Pinball-Loss. Die Breite ist die *Differenz* zweier separat geschätzter
  Quantile und damit doppelt verrauscht, während der Median nur von einem Fehler betroffen
  ist. Das Design testet diese Hypothese nicht.
- **Das stärkste Einzelergebnis im Story-Report** (lstm/G/saocp, A um 21.7 Prozent schlechter,
  Panel-t 8.41, Median-q 0.018) beruht auf einer Methode mit 1.9 Prozent Gate-Rate. Solche
  Extremwerte nicht als Beleg verwenden — den Median über die Kontraste zitieren.

**FALLEN.** Sag konsequent „consistently favours" statt „significantly better". Ein einziges
„signifikant" auf dieser Folie ist falsch und wird auffallen.

---

# Folie 16 — Q4: do the 17 covariates add anything?

**SAGEN.**
Das ist mein persönlicher Lieblingsbefund, und methodisch das saubere Experiment der ganzen
Arbeit.

*Ich vergleiche TiRex-2 mit TiRex-2 plus Kovariaten. Dasselbe vortrainierte Modell, dieselben
127 Testmonate, derselbe Seed, dieselbe Rekalibrierung. Der einzige Unterschied sind die 17
Kovariaten.* Es gibt keine Seed-Varianz, keine Architekturänderung, kein Nachtunen. Kein
anderer Vergleich in dieser Arbeit ist so kontrolliert.

Links das Ergebnis nach der Kalibrierung, Methode für Methode. Die Spalten uni und cov sind
die Median-Skills der beiden Varianten, Δ ihre Differenz, und d das paarweise
DM-Differenzial. *Der Median über alle vierzehn Methoden ist plus 0.0036 zugunsten der
univariaten Variante — also praktisch null.* Der paarweise Wert ist minus 0.0023 bei einem
Panel-t von minus 0.75.

Und die Signifikanzrechnung ist aufschlussreich: von 140 Ländertests erreichen 18 ein p unter
0.10 vor Korrektur. *Unter der Nullhypothese erwartet man 14.* Und sie teilen sich zehn zu
acht nach Richtung. Nach Benjamini-Hochberg innerhalb dieser Familie überlebt keiner, das
kleinste q ist 0.42. Das ist genau das Bild, das man sieht, wenn kein Effekt da ist.

Halte ich die Methode fest, ist die univariate Variante in allen drei führenden Methoden
minimal vorne — 0.1461 gegen 0.1402, 0.1420 gegen 0.1409, 0.1442 gegen 0.1385.

Rechts wird es interessanter, nämlich vor der Kalibrierung, der rohe Modelloutput. *Dort tun
die Kovariaten etwas: sie verengen das Intervall in allen zehn Ländern, um 2.2 bis 6.9
Prozent, sie senken den Winkler-Score in sieben von zehn, und die Coverage fällt von 0.843 auf
0.831 — also in Richtung des Nominalwerts 0.80, nicht weg davon.* Die Kovariaten drücken also
in die richtige Richtung. Der Effekt ist nur zu klein, um bei dieser Stichprobengröße das
Rauschen zu überleben.

Und jetzt die Präzisierung, die mir wichtig ist, weil die Aussage sonst sofort überdehnt wird.
*Das heißt nicht, dass makroökonomische Fundamentaldaten für Staatsanleihen-Spreads irrelevant
sind.* Die Literatur, aus der meine Features kommen, erklärt *Niveaus* im Querschnitt. Ich
prognostiziere *Änderungen* über einen Monat. Das sind verschiedene Fragen, und mein Ergebnis
widerspricht dem nicht. Dazu kommt: durch das konservative Lag-Protokoll sieht das Modell
teilweise veraltete Daten — der Fiskalblock ist vier Monate alt.

**TIEFE.**
- **Warum Δ-Skill und paarweises d im Vorzeichen abweichen können:** Δ ist die Differenz
  zweier Mediane, d ist der Median der paarweisen Differenzen. Beides ist nicht dasselbe. Bei
  `native` hat die univariate Variante den höheren Median-Skill und verliert trotzdem den
  paarweisen Vergleich in sieben von zehn Ländern. Steht so in den Tabellennotizen der Arbeit.
- **Die Wahl der FDR-Familie ist eine Entscheidung:** Innerhalb der eigenen Familie von 140
  Tests überlebt keiner (kleinstes q 0.42, Median 0.98). Würde man die 140 in die große
  Familie der 12 490 Länderzellen des Hauptlaufs einbetten, überlebten acht, kleinstes q
  0.013, fünf für und drei gegen die Kovariaten. *Ich halte die Familien getrennt, weil diese
  Tests eine andere Frage beantworten als die vorregistrierten Kontraste* — und ich weise
  beide Rechnungen aus.
- **Der Cross-Model-Kontrast** tirex2/cqr_static gegen tirex2_cov/native ergibt d_norm minus
  0.000061, share 0.50, Median-p 0.4497, Panel-t 0.728. Das ist genau, was man unter der
  Nullhypothese erwartet. Achtung: dieser Kontrast vergleicht *verschiedene* CP-Methoden, für
  die ceteris-paribus-Aussage sind die methodenweisen Werte die richtigen.
- **Wie die Kovariaten technisch eingehen:** als *past covariates*. TiRex-2 hat auch einen
  bidirektionalen Pfad für zukunftsbekannte Kovariaten, den ich nicht nutze, weil ich keine
  habe.
- **Coverage-Differenz je Land** liegt zwischen minus 0.0394 und 0.0000, weil die Intervalle
  schmaler werden; die Breitendifferenz ist durchweg negativ, minus 0.0029 bis minus 0.0555.

**FALLEN.**
- Sag niemals „Makrodaten sind irrelevant für Spreads". Das ist die Überdehnung, und in einem
  Raum voller Risikomanager, die täglich Fundamentaldaten ansehen, wäre es auch unglaubwürdig.
- Sag nicht „die Kovariaten schaden". Sie helfen minimal vor der Kalibrierung; die Differenz
  ist nur nicht von null zu unterscheiden.

---

# Folie 17 — Q5a: economic reading of interval quality

**SAGEN.**
Ein kurzer Block zur ökonomischen Lesart, und ich sage vorab: *das war nie ein Rangkriterium
und ist es auch jetzt nicht.* Coverage und Winkler entscheiden, das hier ist Ergänzung.

Der Kapitalproxy ist die Intervallbreite geteilt durch die breiteste *zulässige* Breite im
selben Land, Median über die Länder. Niedriger ist besser: weniger Breite für dieselbe
nominale Absicherung. *Das ist eine dokumentierte Vereinfachung der FRTB-Logik, keine
Umsetzung der Regeltexte* — ich setze Breite proportional zur Kapitalunterlegung, und das ist
eine Annahme.

Das Spitzenpaket liegt bei 0.551, die robuste Variante bei 0.573, der Median über alle 485
zulässigen Kombinationen bei 0.669, das schlechteste bei 0.878. Der Unterschied zwischen
bestem und Median ist also rund zwölf Prozentpunkte Breite bei gleicher nominaler Coverage.

Rechts die Frage, worauf Intervallqualität eigentlich einzahlt. *Die Korrelation zwischen
Median-Skill und Kapitalquotient ist minus 0.711.* Weil ein niedriger Quotient gut ist, ist
das negative Vorzeichen der erwünschte Zusammenhang: bessere Intervalle tragen weniger Breite.
*Die Korrelation zwischen Skill und dem Sharpe der Handelsregel ist minus 0.147, also
praktisch null.*

Der Schluss daraus: die Gewinne aus besseren Intervallen sind in einer Kapitalkennzahl
realisierbar, nicht in Handelsperformance. Und angesichts von Folie 10, wo kein Modell die
Punktprognose verbessert, ist das das erwartete Ergebnis und nicht ein überraschendes.

Zu den Grenzen, und die sind hart: *die Handelsregel ist in etwa zwei Prozent der Monate
aktiv.* Das sind ein bis zwei Trades von 127. Sie hat keine Transaktionskosten, keine
Bid-Ask-Spannen, keine Kapazitätsgrenzen. Der Median-Sharpe von 0.159 über die zulässigen
Kombinationen ist auf so wenigen aktiven Monaten statistisch bedeutungslos, und ich würde
darauf kein Gewicht legen.

**TIEFE.**
- **Wie die Handelsregel definiert ist:** aktiv, wenn der letzte realisierte Level außerhalb
  des kalibrierten Intervalls liegt, also wenn null nicht im Delta-Intervall liegt. Die
  Position ist Vorzeichen der Prognoserichtung geteilt durch die Intervallbreite,
  annualisierter Sharpe über die Monats-P&L. Das ist eine stilisierte Mean-Reversion-Regel.
- **Warum der Kapitalproxy pro Land normiert ist:** die absoluten Breiten sind zwischen
  Österreich und Griechenland nicht vergleichbar. Der Bezug auf die breiteste zulässige Breite
  desselben Landes macht die Zahl skalenfrei.
- **Was FRTB tatsächlich verlangen würde:** ein Expected-Shortfall-Maß auf verschiedenen
  Liquiditätshorizonten mit Stresskalibrierung, plus P&L-Attribution-Tests. Mein Proxy erfasst
  davon nur die Breitendimension. Sag das offen, wenn jemand aus dem Marktrisiko nachfragt.
- Die Definition steht in `diagnostics_master_v2.ipynb`, Zelle 23; das war ex ante als
  ergänzender Block vorgesehen (§F7 des Foundation-Dokuments).

**FALLEN.** Nicht in eine Diskussion über die Handelsregel geraten. Winke sie aktiv ab, bevor
jemand nachrechnet. Wenn du sie verteidigst, verlierst du — sie ist nicht verteidigungsfähig
und war nie dafür gedacht.

---

# Folie 18 — Q5b: behaviour under stress

**SAGEN.**
Die letzte inhaltliche Folie, und für Sie wahrscheinlich die relevanteste. Zwei Stressachsen,
beide vor der Auswertung festgelegt, beide ohne Look-ahead.

Die erste ist realisierte Spread-Volatilität: eine rollierende Zwölfmonats-Standardabweichung
der realisierten Änderungen, um einen Monat verzögert, pro Land in Terzile geschnitten. Die
zweite ist der VIX mit einer ex ante fixierten Schwelle von 20.

Links das Aggregat über die 485 zulässigen Kombinationen. Auf der Volatilitätsachse *steigt*
die Coverage von 0.7795 über 0.8166 auf 0.8412, während der Skill von plus 0.1114 auf minus
0.0037 *fällt*. Auf der VIX-Achse *fällt* die Coverage von 0.8342 auf 0.7588, und der Skill
fällt ebenfalls.

Die Coverage bewegt sich auf den beiden Achsen also in entgegengesetzte Richtungen. *Das ist
kein Widerspruch, und die Auflösung ist der methodisch interessanteste Punkt des Kapitels.*
Rechts oben die Zahlen: es gibt 36 High-VIX-Monate und 38 Monate im oberen Volatilitätsterzil,
aber *nur zwölf werden von beiden Definitionen als Stress klassifiziert*. Die gleichzeitige
Korrelation ist 0.009, die stärkste verzögerte 0.170 bei zwei Monaten Versatz.

Die Terzile sind rückwärtsgerichtet und länderspezifisch, der VIX ist gleichzeitig und global.
*Die beiden Achsen markieren also verschiedene Phasen und nicht verschiedene Intensitäten: der
VIX markiert den Eintritt in die Belastung, das obere Terzil ihr Andauern. Die Online-Regler
hinken beim Eintritt hinterher — Coverage 0.759 — und überschießen, sobald der Stress andauert
— 0.841.*

Unten, was das für die Praxis heißt. Im oberen Volatilitätsterzil *bleibt* die Rangfolge
erhalten: Spearman plus 0.846 gegen die Gesamtrangfolge, größte Rangänderung vier Plätze. Bei
VIX über 20 *zerfällt* sie: Spearman plus 0.399, nicht signifikant, größte Rangänderung sieben
Plätze. Das Spitzenpaket fällt von Platz eins auf Platz acht. Bestes Paket im High-VIX-Regime
wird LightGBM global mit decay_ocp, und ARMA steigt von Platz zwölf auf Platz sechs.

*Das Paket mit dem besten Gesamtergebnis ist also nicht das, das in den Monaten hält, in denen
der Stress einsetzt.* Und die klassischen Modelle kommen genau dann zurück, wenn es darauf
ankommt.

Dazu noch die vorregistrierte Komplexitätshypothese: der Vorsprung der komplexen Modelle
schrumpft von plus 0.1026 auf plus 0.0251, ist also widerlegt. Aber die Zerlegung ist
ehrlicher: das trägt vollständig der Zero-Shot-Arm, plus 0.1315 auf plus 0.0254. Die auf
diesem Panel *trainierten* komplexen Modelle hatten mit plus 0.0143 nie einen nennenswerten
Vorsprung und liegen bei hohem VIX unter den klassischen. *Was existierte und dann verschwand,
war ein Pretraining-Vorteil und keine Komplexität.*

**TIEFE.**
- **Warum die Coverage im Andauern steigt:** die Regler weiten nach Volatilitätsanstiegen aus,
  und weil Stressphasen persistent sind, überschießen sie. Coverage steigt, der Winkler
  bezahlt es mit Breite. Die Kehrseite: in ruhigen Phasen sind die Intervalle zu schmal,
  0.7795 unter 0.80. Das ist ein echter, benennbarer Adaptivitäts-Lag.
- **Coverage und Skill laufen auf der Volatilitätsachse gegenläufig.** Das ist der sauberste
  Beleg im ganzen Kapitel dafür, dass Coverage allein kein Gütemaß ist — und die Rechtfertigung
  für das Gate-statt-Score-Prinzip von Folie 7.
- **Die Zelle bei hohem VIX ist klein:** 36 von 127 Monaten, also 28.3 Prozent. Die Schätzer
  dort sind entsprechend verrauscht. Selbst nennen.
- **Die Länderstreuung ist in den guten Regimen am größten:** Median-IQR über Länder 0.350 in
  calm gegen 0.122 in stressed. In ruhigen Zeiten sind länderspezifische Faktoren die
  Haupttreiber.
- **Die ersten zwölf Testmonate** haben kein Volatilitätsfenster und gehen ins neutrale
  mittlere Terzil. Konvention, per Amendment 2026-07-10 dokumentiert.
- **Zwei Skill-Definitionen nicht verwechseln:** die Regime-Tabelle nutzt alle 14 Methoden
  inklusive `native`, nur die 485 zulässigen Kombinationen, Median über Kombinationen. Der
  vorregistrierte Komplexitätstest schließt `native` aus, gatet *nicht* und nimmt den Median
  über alle Zellen. Deshalb steht für TiRex-2 bei hohem VIX einmal plus 0.0237 und einmal
  minus 0.003. Wenn du beide Zahlen nennst, sag welche Definition gilt.
- **Der Kalenderschnitt** (QE bis 2021-12 gegen Hiking ab 2022-01) wird nicht berichtet, und
  das ist begründet: er ist ein einzelnes hartes Datum, bildet keine EZB-Entscheidungen ab und
  ist zu 90 Prozent kollinear mit der Volatilitätsachse — 89.5 Prozent der gestressten
  Länder-Monate liegen vor 2022-01.
- **Die F13-Gruppen:** komplex = xlstm, lstm, lgbm, tirex, tirex2, tirex2_cov; klassisch = rw,
  arma, arma_monthly. *Die drei const-Zwillinge stehen in keiner Gruppe* und gehen nicht in die
  Kennzahl ein. Wichtig, sonst liest man das `is_complex=False`-Flag als „klassisch".
- **Warum der Gruppenmedian problematisch ist:** die komplexe Gruppe ist bimodal. Ihre sechs
  Werte streuen im low-VIX-Regime von minus 0.0271 (xlstm) bis plus 0.1255 (tirex2), mit einer
  Lücke von 0.055 zwischen lgbm (+0.0640) und tirex (+0.1191). Der Gruppenmedian plus 0.0916
  fällt genau in diese Lücke — er beschreibt kein Modell, sondern die Lage der Gruppengrenze.

**FALLEN.** Nenn den Absturz von Platz eins auf acht nicht als Argument gegen TiRex-2 an sich.
Es ist ein Argument dafür, an der richtigen Stelle zu validieren. Das ist die konstruktive
Lesart und die, die dir zugerechnet wird.

---

# Folie 19 — Limitations

**SAGEN.**
Sechs Einschränkungen, und ich behandle sie als Reichweitenbegrenzungen und nicht als
Kleingedrucktes. Sie stehen mit Evidenz und Konsequenz da.

Erstens, und das ist die schwerwiegendste, weil sie ausgerechnet die Spitzenfamilie betrifft:
*TiRex-2 wurde am 1. Juli 2026 veröffentlicht, der Pretraining-Cutoff ist nicht dokumentiert,
und mein Testfenster endet im April 2026.* Es ist also möglich, dass das Modell meine
Testdaten im Pretraining gesehen hat. Und bei Staatsanleihen-Renditen ist das besonders heikel,
weil die Reihen öffentlich, langjährig und in praktisch jedem großen Zeitreihen-Korpus
enthalten sind.

Drei Argumente sprechen dagegen. *Erstens ist die Punktprognose nicht besser als der Random
Walk — Panel-t minus 0.498, und ich habe gezeigt, dass der Test Power hat. Ein Modell, das die
Zielreihe memoriert hätte, müsste im Median glänzen; es tut es nicht.* Zweitens liegt der
Vorsprung vollständig in der Breite der Verteilung, während Memorierung primär das Zentrum
treffen würde. Drittens verschwindet der Vorsprung bei hohem VIX, von 0.1255 auf minus
0.0025 — ein memorierendes Modell hätte gerade in den auffälligen Stressmonaten einen Vorteil,
nicht dort einen Einbruch.

*Keines der drei ist ein Beweis. Ohne dokumentierten Cutoff ist die Frage empirisch nicht
entscheidbar.* Ein sauberer Test bräuchte ein Evaluationsfenster nach dem Release.

Zweitens die Grenzen der Inferenz. *Kein einziger der 66 Cross-Model-Kontraste ist
FDR-signifikant, das kleinste Median-q ist 0.185.* Die Model Confidence Set behält 15 von 26
Paketen in allen zehn Ländern, Finnland und Frankreich schließen keins aus. Die Konsequenz:
die Rangfolge ist deskriptiv. Stabil über alle Robustheitsschnitte ist nur die grobe
Gruppierung.

Drittens, und das nenne ich von selbst, obwohl niemand danach fragen würde: die Selektion ist
optimistisch. Jedes Paket wurde als bestes seiner Familie auf denselben Testdaten gewählt, auf
denen es dann verglichen wird. Und ungleich: `lstm` aus 105 zulässigen Paketen, `rw` aus neun.

Viertens die Seed-Fragilität, die Sie auf Folie 13 gesehen haben. Fünftens Irland: die
Rangfolge ist ohne Irland stabil, Spearman 0.939, das Niveau nicht — im Schnitt 0.021 und beim
Spitzenpaket 0.043 weniger. Etwa ein Drittel des Vorsprungs kommt aus dem leichtesten Land des
Panels. Sechstens: bedingte Kalibrierung ist breitflächig verletzt, wie auf Folie 11 gezeigt.
Zulässigkeit belegt marginale, nicht bedingte Validität.

**TIEFE.**
- **Warum Irland ein Sonderfall ist:** die irische Volkswirtschaftsstatistik ist durch
  multinationale Konzerne verzerrt — daher die Modified-GNI-Kennzahl des irischen
  Statistikamts. Debt/GDP ist dort kein sauber vergleichbarer Nenner. Irland hat mit
  Median-Skill 0.231 den höchsten Wert des Panels, nächster ist Portugal mit 0.191.
- **Wer am stärksten unter dem Irland-Ausschluss leidet:** vier der sechs größten
  Verschlechterungen sind `lgbm/L`-Pakete — aci minus 0.094, dtaci minus 0.084, agaci minus
  0.077. *LightGBM im lokalen Regime lebt fast vollständig von Irland.*
- **Warum die MCS nicht trennscharf ist**, und dass das kein Implementierungsfehler ist: bei
  127 Monaten und zehn korrelierten Ländern hat der Hansen-Test zu wenig Power, und die
  Kuratierung verschärft das — jeder Kandidat ist schon der beste aus 9 bis 30 Optionen, also
  liegen sie eng zusammen. Mit allen 742 Kombinationen wäre die MCS vollständig
  informationslos gewesen. → REF §6
- **Burn-in-Doppelnutzung:** dieselben 36 Monate dienen als HPO-Validierung und als
  CP-Initialisierung. Die Diagnostik zeigt in den ersten 36 Testmonaten *Unter*deckung, im
  Mittel minus 1.07 Prozentpunkte, am stärksten bei `native` mit minus 6.64. Also
  Anfangspessimismus, nicht künstlicher Optimismus — und selbstlimitierend.
- **Pool-Robustheit:** die Wahl des Kalibrierungspools betrifft nur sechs der vierzehn
  Methoden; für die anderen acht sind die Fenstervarianten bit-identisch, verifiziert bei
  maximaler Coverage-Differenz exakt null. Für die betroffenen sechs ist die Wahl aber *nicht*
  vernachlässigbar: mittlere absolute Coverage-Differenz zu expanding 4.0 Prozentpunkte,
  maximal 18.9.
- **Giacomini-White ist nicht implementiert**, und das ist eine dokumentierte
  Scope-Entscheidung: bei h = 1, festem Refit-Protokoll und identischen Informationsmengen
  fällt der GW-Vorteil mit diesem Design weitgehend zusammen.
- **Alle Post-hoc-Rechnungen sind an einer Stelle deklariert:** DQ-Zerlegung, Punktprognose-DM
  gegen RW, Länder-Kendall-τ, GF-vs-L-Kontrast, und die paarweisen Kovariatenspalten. Jeweils
  mit Datum und der Aussage, ob sie ein Finding erzeugen.

**FALLEN.**
- Diese Folie nicht hetzen. Sie ist der Grund, warum man dir den Rest glaubt.
- Kein „leider", kein „natürlich hat jede Arbeit Grenzen". Gleicher Tonfall wie bei den
  Ergebnissen.
- Sag bei Punkt 2 nicht „aber der Trend ist klar". Sag „die Rangfolge ist deskriptiv".

---

# Folie 20 — Conclusions and next steps

**SAGEN.**
Sechs Schlussfolgerungen, jede mit der tragenden Zahl.

*Erstens: Monatsänderungen von Euro-Staatsanleihen-Spreads tragen kein ausbeutbares
Punktprognosesignal. Aufwand, der in den bedingten Erwartungswert geht, zahlt sich nicht aus.*

Zweitens: rohe Modellquantile sind ohne Rekalibrierung nicht als Risikointervalle brauchbar.
Sie sind in 13.2 Prozent der Fälle zulässig und systematisch zu breit, Coverage 0.872 gegen
Ziel 0.800.

*Drittens, und das ist für Sie der praktisch wertvollste Punkt: die Rekalibrierungsmethode ist
so folgenreich wie die Modellwahl — und der billigere Hebel von beiden.* Innerhalb eines
einzigen Modells spannt der Skill von minus 0.044 bis plus 0.146.

Viertens: das führende Paket ist ein Zero-Shot-Foundation-Model, aber sein Vorsprung auf den
klassischen Vergleich ist statistisch nicht separierbar.

Fünftens: die 17 Kovariaten ändern auf diesem Horizont nichts Messbares, und die auf diesem
Panel trainierten Modelle hatten nie einen dauerhaften Vorteil.

*Sechstens: validiert werden sollte am Eintritt in den Stress, nicht am Durchschnitt. Das
beste Gesamtpaket war bei VIX über 20 nur Achter.*

Drei nächste Schritte, jeder an einem Befund von heute aufgehängt. Eine asymmetrische
Score-Familie, weil der symmetrische CQR-Score die gemessene 37-Prozent-Strafenasymmetrie
strukturell nicht abbilden kann. Ein Evaluationsfenster nach dem TiRex-2-Release, weil das der
einzige saubere Weg ist, die Kontaminationsfrage zu entscheiden. Und bedingte Kalibrierung
entlang der Breitenachse, weil die DQ-Zerlegung genau dort lokalisiert, wo alle vierzehn
Methoden brechen.

Ich habe Backup zu allen Kovariaten mit Konstruktion und Quelle, zu allen vierzehn
CP-Methoden, zur Arithmetik der 742 Kombinationen, zu den Cross-Model-Kontrasten und der MCS,
und zum Regimeverhalten je Methode. Vielen Dank.

**FALLEN.** Nicht mit „vielen Dank für die Aufmerksamkeit" als letztem Inhalt enden. Die
nächsten Schritte sind der letzte Inhalt; das Dankeschön ist ein Halbsatz danach. Mit „was ich
als Nächstes machen würde" zu enden signalisiert, dass du das Thema besitzt.

---

# Anhangsfolien 21–25

Nicht vortragen, auf Nachfrage ziehen. Was jeweils drauf ist und wofür:

| Folie | Inhalt | Zieh sie, wenn gefragt wird |
|---|---|---|
| 21 | Kovariaten-Konstruktion und Quellen, alle 17 mit Formel | „Wie genau ist Feature X gebaut?" · „Woher kommt die Auswahl?" |
| 22 | Alle 14 CP-Methoden mit Mechanismus, fixierten Parametern, Gate-Rate | „Warum vierzehn?" · „Was macht Methode X?" · „Was habt ihr getunt?" |
| 23 | Die 742/21 200-Arithmetik | „Wie kommen Sie auf 742?" — ein Mathe-Publikum rechnet 12·5·3·14 = 2520 |
| 24 | Cross-Model-Kontraste und MCS | „Wie sicher ist die Rangfolge?" · „Was ist eine MCS?" |
| 25 | Regimeverhalten je CP-Methode | „Welche Methode hält im Stress?" |

Der wahrscheinlichste Griff ist Folie 22 oder 23.

---

# Zeitplan zum Mitschreiben

| Folie | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| min | 0.5 | 2 | 2 | 2 | 2.5 | 2.5 | 2.5 | 2.5 | 1.5 | 2.5 |
| **kumuliert** | 0.5 | 2.5 | 4.5 | 6.5 | 9 | 11.5 | 14 | 16.5 | 18 | 20.5 |

| Folie | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|---|---|---|---|---|---|---|---|---|---|---|
| min | 2.5 | 3 | 2 | 2 | 2 | 2.5 | 1.5 | 2.5 | 2.5 | 1.5 |
| **kumuliert** | 23 | 26 | 28 | 30 | 32 | 34.5 | 36 | 38.5 | 41 | 42.5 |

**Das sind 42.5 min bei vollem Ausschöpfen — zu viel für 34.** Wo du kürzt, in dieser
Reihenfolge:

1. Folie 3 nur vier statt elf Zeilen ansprechen: −1 min
2. Folie 4 und 6 straffen, Protokolldetails weglassen: −2 min
3. Folie 15 nur die drei Pointen, keine Einzelzahlen: −1 min
4. Folie 9 auf 45 Sekunden, sie kennen CP: −0.75 min
5. Folie 17 auf eine Minute, Kapitalquotient und die zwei Korrelationen: −0.5 min
6. Folie 14 die Streuungszahlen weglassen: −0.5 min

Damit landest du bei ~36.5 min. Wenn es hart auf 30 muss, fällt zusätzlich Folie 15 ganz weg
(RQ1/RQ2 in zwei Sätzen auf Folie 12 mitnehmen) und Folie 17 ganz — dann bist du bei 31 und
hast die Kernbotschaft vollständig.

**Nie kürzen:** Folie 7, 8, 11, 19. Die zwei Bewertungsmaße und die Ehrlichkeitsfolie sind
das, was den Vortrag trägt.
