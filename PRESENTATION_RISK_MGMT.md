# Leitfaden: Präsentation der Ergebnisse im Risikomanagement (Erste Group)

**Stand:** 2026-08-10 · **Format:** ~34 min Vortrag + Q&A danach · **Folien:** Englisch, Vortrag deutsch
**Deck:** `Erste_Risk_Talk.pptx` — 25 Folien (20 Vortrag + 5 Anhang), 16:9, Sprechernotizen auf allen 25

**Festgelegt:** Headline = `tirex2/ZS/cqr_static` (0.146) als bestes Paket **genannt**, aber im
gleichen Zug als instabiler ausgewiesen (Coverage 0.750, 8/10 Länder, Kreuzungsrate 0.724 %);
`decay_ocp_cqr` (0.144, 0.787, 10/10, 0.028 %) als die Variante, die man einsetzen würde.
Achtung: `results.tex` §8.3 führt derzeit noch `decay_ocp_cqr` als Headline — Folien und Thesis
weichen an dieser Stelle bewusst voneinander ab, bis das entschieden ist.

Alle Zahlen unten sind aus `sections/results.tex`, `sections/limitations.tex`,
`results/tables/*.csv` und `notebooks/diagnostics_master_v2.ipynb` (Diagnostics-Run
`run_full_20260810_110834`). Herkunft je Block angegeben.

---

## 0a. Gestalterische Vorgaben, die im Deck umgesetzt sind

- **Sachlich, nicht narrativ.** Keine kursiven Pointen-Zeilen, keine rhetorischen Aufbauten.
  Jede Folie ist Titel + Tabelle(n) + eine knappe Ergebniszeile in Prosa.
- **Eine Akzentfarbe.** Drei Grautöne für Text, `#58C4DD` als einziger Akzent für Struktur und
  hervorgehobene Zahlen, `#FC6255` ausschließlich für negative oder durchgefallene Werte.
  Nichts sonst.
- **Tabellen statt Prosa.** 54 Tabellen auf 25 Folien; die Haupttabellen sind direkt die aus dem
  Ergebniskapitel (`tab:res_point`, `tab:res_gate`, `tab:res_ranking`, `tab:res_tirex2_cp`,
  `tab:res_cov`, `tab:res_regime`).
- **Jede Zahl wird vorher definiert.** MAE-Ratio und Winkler-Skill stehen mit Formel auf Folie 8,
  bevor sie auf Folie 10 und 12 als Werte auftreten. Auf Folie 10 gibt es zusätzlich eine
  Spaltendefinitions-Tabelle.
- **Pakete immer vollständig benannt** als Modell / Regime / CP-Methode.

---

## 0b. Foliennummern im gebauten Deck

| Folie | Inhalt | min |
|---|---|---|
| 1 | Titel | 0.5 |
| 2 | Objective and research questions (RQ1–RQ3 + Q4/Q5, mit n je FDR-Familie) | 2 |
| 3 | Summary of findings (11 Zeilen, je mit Kennzahl und Folienverweis) | 2 |
| 4 | Data and target | 2 |
| 5 | **Die 17 Kovariaten und ihre Publikationslags** | 2.5 |
| 6 | Twelve model variants under one protocol | 2.5 |
| 7 | Evaluation 1: Validität — Testbatterie, Gate, Begründung der Schwellen | 2.5 |
| 8 | Evaluation 2: Winkler-Formel, Skill, MAE-Ratio — die Leseregeln | 2.5 |
| 9 | The conformal layer — Klassen, Garantien, Disziplin | 1.5 |
| 10 | Point forecasts: no model beats the random walk | 2.5 |
| 11 | Raw model quantiles are too wide and rarely valid | 2.5 |
| 12 | Interval quality: best package per family + alle 14 Methoden für tirex2 | 3 |
| 13 | The namesake architecture is the weakest family (xLSTM) | 2 |
| 14 | What drives the Winkler score (Formel + Zerlegung) | 2 |
| 15 | RQ1 und RQ2 | 2 |
| 16 | **Q4: do the 17 covariates add anything?** | 2.5 |
| 17 | Q5a: Kapitalquotient | 1.5 |
| 18 | Q5b: Verhalten unter Stress + Rangstabilität | 2.5 |
| 19 | Limitations (6 Zeilen: Limitation, Evidenz, Konsequenz) | 2.5 |
| 20 | Conclusions and next steps | 1.5 |
| 21–25 | Anhang A–E | — |

Summe ≈ **44 min**, wenn du jede Folie ausschöpfst. Für 34 min heißt das: Folien 4, 6, 9 und 17
zügig, und auf 3 nur die vier wichtigsten Zeilen ansprechen statt alle elf. Zeit nehmen bei
7, 8, 10, 11, 12, 16 und 19.

**Anhang:** A Kovariaten-Konstruktion und Quellen · B alle 14 CP-Methoden mit Mechanismus und
fixierten Parametern · C die 742/21 200-Arithmetik · D Cross-Model-Kontraste und MCS ·
E Regimeverhalten je CP-Methode.

---

## 1. Blockplan

Netto ~27 min, Rest ist Puffer (die Sitzung fängt immer später an). Spalte "Cut" = Reihenfolge,
in der du kürzt, wenn du in Zeitnot kommst.

| # | Block | min | Cut |
|---|---|---|---|
| 0 | Aufhänger + Antwort vorweg | 2 | — |
| 1 | Setup: Ziel, Modelle, Protokoll | 3 | 3. |
| 2 | Die zwei Zahlen: Coverage & Winkler | 4 | nie |
| 3 | Conformal Prediction in 90 Sekunden | 2 | nie |
| 4 | Negativergebnis 1: Punktprognose | 2 | — |
| 5 | Das Validity-Gate | 2.5 | — |
| 6 | Hauptranking | 3.5 | — |
| 6b | RQ1/RQ2 komprimiert | 1 | 1. |
| 7 | Warum: Winkler-Zerlegung | 1.5 | 4. |
| 8 | Risikomanagement-Nutzen: Kapital & Regime | 3 | — |
| 9 | Was ich NICHT behaupte | 2.5 | nie |
| 10 | Takeaways + nächste Schritte | 1.5 | — |

---

### Block 0 — Aufhänger + Antwort vorweg (2 min)

**Inhalt.** Nicht "meine Arbeit über xLSTM". Sondern die operative Frage:

> Für zehn Euro-Staatsanleihen-Spreads, einen Monat voraus: ihr braucht keine Zahl, ihr
> braucht ein Band, hinter das man Kapital stellen kann. Woher kommt dieses Band, und
> woher weiß man, dass es hält?

Dann sofort die drei Antworten als Vorwegnahme:
1. Punktprognose: für die Monatsänderung gibt es praktisch kein Signal — nicht mal für ein
   auf Millionen Zeitreihen vortrainiertes Foundation Model.
2. Beim Intervall gibt es sehr wohl Unterschiede — und der größte einzelne Hebel ist nicht
   das Modell, sondern die Rekalibrierungsmethode.
3. Statistisch gesichert ist davon die *Ordnung*, nicht die Rangfolge. Warum, kommt am Ende.

**Warum.** Dieses Publikum entscheidet in den ersten zwei Minuten, ob es zuhört. Wenn die
Antwort vorne steht, ist alles Folgende Beweisführung statt Spannungsbogen — und du kannst
später jederzeit gekürzt werden, ohne die Botschaft zu verlieren. Außerdem: Punkt 1 räumt
die falsche Erwartung ab, bevor sie entsteht.

---

### Block 1 — Setup (3 min, 2 Folien)

**Inhalt.**

*Folie A — die Aufgabe.* Ziel = Δ-Spread, 10 Euro-Länder, monatlich, h = 1. Immer in
Level-Raum zurücktransformiert vor der Bewertung. Testfenster 127 Monate, 2015-10 bis 2026-04.
Modelle geben drei Quantile {0.1, 0.5, 0.9} → 80%-Intervall, α = 0.20.
Ein Satz zur Wahl von 0.1/0.9: das ist das breiteste Paar, das *alle* Modelle können
(TiRex gibt nur Dezile aus) — also keine Extrapolation für irgendjemanden.

*Folie B — Modellleiter und Protokoll.* Eine Achse "steigende Kapazität":
Random Walk → ARMA → LightGBM → LSTM → xLSTM → TiRex / TiRex-2 (zero-shot, vortrainiert,
nicht auf diesen Daten trainiert).
Dann das Protokoll in drei Zeilen, weil genau das ihre erste Frage wäre:
Rolling Refit alle 12 Monate auf expandierendem Trainingsfenster; Monate 115–150 sind
Burn-in — HPO-Validierung **und** CP-Kalibrierung; bewertet wird nur 151–277.
24-Monats-Informationsobergrenze für alle trainierten Modelle, damit keins länger
zurückschauen darf. Publication-Lag-Dictionary auf jedes Feature. Zielland-Renditen und
-Spreads sind als Input ausgeschlossen.
Ein Reproduzierbarkeitssatz, der nichts kostet und viel bringt: **identischer `data_sha256`
über alle sieben Modellfamilien** — alle Modelle haben bewiesen dasselbe Panel gesehen.

Und *ein* Satz zum Umfang, nicht mehr: "742 Kombinationen aus 12 Modellvarianten, 5 Regimen,
3 Seeds und 14 Rekalibrierungsmethoden, ausgewertet auf 21 200 Zellen. Ich zeige Ihnen nur
die Teile, die eine Schlussfolgerung tragen."

**Warum.** Look-ahead-Bias ist das Erste, was ein Risikomanager prüft. Wenn du es
selbstbewusst vorwegnimmst, ist es weg — und du musst es nie wieder verteidigen. Der
`data_sha256`-Satz ist billig und signalisiert Sorgfalt auf einer Ebene, die Nicht-Praktiker
nicht erwarten.

**Falle.** Die 24-Monats-Obergrenze gilt *nicht* für TiRex/TiRex-2 (die nutzen die volle
expandierende Level-Historie). Das steht so in `experimental_design.tex` und ist ein
Informationsvorteil für ausgerechnet die Gewinnerfamilie. Nenn es hier von selbst in einem
Halbsatz — sonst kommt es in Q&A als Vorwurf.

---

### Block 2 — Die zwei Zahlen (4 min, 2 Folien) ← hier nicht sparen

**Inhalt.**

*Coverage.* Nominal 80%. Empirisch = Trefferquote. Zwei Tests, je ein Satz:
Kupiec prüft nur die *Rate*, Christoffersen zusätzlich, ob die Verletzungen *clustern*.
Explizit die Brücke schlagen: "Das ist dieselbe Logik wie im VaR-Backtesting, nur auf ein
80%-Intervall statt ein 99%-Quantil."

*Winkler-Score.* Eine Formel, und nur diese:

```
W = (u − l)  +  (2/α)·(l − y)·1{y<l}  +  (2/α)·(y − u)·1{y>u}
     Breite        Strafe zu hoch          Strafe zu tief
```

Drei Sätze dazu: (i) proper scoring rule — das Minimum liegt genau bei den wahren Quantilen,
also kein Ad-hoc-Maß; (ii) es ist ein **Verlust**, kleiner = besser; (iii) daraus der
Skill: `Skill = 1 − W_Modell / W_RW` je Land, dann Median über Länder — **positiv = besser
als der Random Walk**.

Diese Leseregel einmal groß hinschreiben. Ohne sie liest der Raum jede weitere Tabelle
falsch herum, und du merkst es nicht.

**Warum.** Wenn sie diese zwei Zahlen nicht besitzen, funktioniert ab Block 5 keine Folie
mehr. Das ist die einzige Stelle, an der du bewusst langsam bist. Wenn du kürzen musst,
kürze Block 1 und 6b — nicht diesen.

**Nicht zeigen:** Pinball-Loss, CQR-Score, DQ-Test (der kommt in Block 5 als Zahl, nicht
als Methode), die Gate-Formel mit BH-FDR.

---

### Block 3 — Conformal Prediction in 90 Sekunden (2 min, 1 Folie)

**Inhalt.** Die Idee in einem Satz: *Was das Modell behauptet, ist erst einmal unverbindlich.
Wir schauen, wie falsch es in letzter Zeit lag, und ziehen das Intervall so weit auf oder zu,
dass die empirische Trefferquote auf 80% läuft — verteilungsfrei, ohne Annahme über die
Fehlerverteilung.*

Dann die Version für dieses Publikum: **Regelkreis.** Regelgröße = kumulierter
Coverage-Fehler, Stellgröße = Intervallbreite. Die klassische Variante braucht
Exchangeability; die Online-Varianten (ACI = Integralregler, PID = Regler mit
Proportional- und Differenzialanteil) verzichten darauf und garantieren stattdessen
Langfrist-Coverage. Genau deshalb funktionieren sie auf Zeitreihen mit Regimewechseln.

14 Methoden nur als drei Klassen nennen: einmalige statische Rekalibrierung / gefensterte
Rekalibrierung / Online-Regler. Keine Einzelaufzählung.

**Warum.** Das ist die Methodik, auf der dein interessantestes Ergebnis (Block 5, RQ3)
überhaupt erst verständlich wird. Und die Regler-Analogie ist der Punkt, an dem ein
Physiker im Raum aufhört, das für Machine-Learning-Folklore zu halten.

---

### Block 4 — Negativergebnis 1: Punktprognose (2 min, 1 Folie)

**Inhalt.** MAE-Verhältnis gegen Random Walk, Median über Länder:
`tirex2` 0.997 — das einzige unter 1, also 0.35% Vorsprung. Alle selbst trainierten Modelle
*schlechter*: `lstm` 1.061, `lgbm` 1.065, `xlstm` 1.104.
Richtungstrefferquote im Mittel 0.5397; Pesaran-Timmermann in 7.2% der Zellen auf 5%
signifikant (7.74% der *definierten* Tests) — unter Zufall erwartet man 5%.

Dann der Satz, der diese Folie zur besten methodischen Folie des Vortrags macht:
Der Post-hoc-DM-Test (HLN, Panel-t mit Newey-West) auf `tirex2` gegen `rw` ergibt
**Panel-t = −0.498, kein einziges von 10 Ländern FDR-signifikant**, und nur 5 von 10 Ländern
haben überhaupt ein MAE-Verhältnis unter 1. Die 0.35% sind Rauschen.
**Und die Kontrolle:** dieselbe Prozedur auf `arma` gegen `rw` ergibt Panel-t = +2.447
(ARMA signifikant *schlechter*, FI mit q = 0.0040). Der Test hat also Power — er findet bei
`tirex2` nichts, weil nichts da ist. Das ist eine echte Nicht-Ablehnung, keine
Power-Schwäche.

**Warum.** Drei Dinge auf einer Folie: (a) räumt die falsche Erwartung ab, (b) das
Kontrollexperiment ist der stärkste Kompetenzbeleg im ganzen Vortrag — "ich habe geprüft,
ob mein Nicht-Befund nur ein schwacher Test ist" ist genau das, was einen Analysten von
einem Modellbauer unterscheidet, (c) du legst hier die Munition für Block 9 (Kontamination),
und *kassierst sie dort ein*. Sag das schon an: "Merken Sie sich diesen Nicht-Befund, ich
komme darauf zurück."

**Falle, die du selbst nennen musst.** Der Random Walk hat mit 0.5827 die *höchste*
Richtungstrefferquote im Feld. Das ist ein Artefakt: seine Δ-Prognose ist konstant null,
damit wird der Nenner der PT-Statistik exakt null und der Test ist undefiniert (in 90% der
`rw`-Zellen NaN). Die Zahl ist schlicht der Anteil der Monate ohne Spread-Anstieg. Nenn es
als Artefakt, deute es nicht — wenn jemand es findet und du hast es nicht gesagt, kostet es
dich mehr als es wert ist.

---

### Block 5 — Das Validity-Gate (2.5 min, 1 Folie)

**Inhalt.** Das ist deine erste substanzielle *positive* Aussage.

Nur **485 von 742 Kombinationen (65.4%)** passieren das Gate (Kupiec + Christoffersen, nach
BH-FDR, in ≥70% der Länder nicht verworfen). Das Gate ist kein Formalismus, es sortiert ein
gutes Drittel aus.

Die eigentliche Story ist die Gate-Rate nach Methode:
- `native` (rohe Modellquantile): **13.2%**, mittlere Coverage **0.872** statt 0.80
- adaptive CP-Verfahren (`dtaci`, `pid`, `pid_local`, `sfogd`, `spci`): **100%**

Und die Richtung der Fehlkalibrierung ist der praktisch wichtige Teil:
**Es ist Überdeckung, nicht Unterdeckung.** Die Modelle sind nicht zu optimistisch, sie sind
zu konservativ → Intervalle unnötig breit → teuer. Das ist die Brücke zu Block 8.

Länderschnitt, ein Satz: GR 0.519 und PT 0.616 am schwersten, FR 0.864 und FI 0.844 am
leichtesten — die Peripherie mit sprunghaften Spreads ist am schwersten kalibrierbar.
*(Das ist der erste Kandidat zum Streichen, wenn die Zeit knapp wird.)*

Dann die Ehrlichkeit im selben Atemzug, nicht später:
Der DQ-Test, der zusätzlich auf 4 Lags **und die Intervallbreite** konditioniert, verwirft
**49.3%** aller Zellen. Nimmt man den Breiten-Regressor heraus, fällt die Rate auf 20.8% —
also praktisch auf UC-Niveau. Die Ablehnungen sind fast vollständig ein Breiten-Phänomen,
kein Lag-Phänomen. Von den signifikanten Fällen haben 99.9% ein negatives γ (Median −1.631):
die Breitensteuerung reagiert in die *richtige* Richtung, aber sie **überschießt** in beide
Richtungen — marginal mittelt sich das auf 0.80 heraus, deshalb sieht Kupiec nichts.

Der Satz, mit dem du diese Folie beendest:
> *The conformal layer trades temporal dependence of violations for width-conditional
> dependence.*

**Warum.** Hier ist deine Aussage unangreifbar und praktisch relevant: rohe Modellquantile
sind systematisch zu breit, die CP-Schicht reparariert das Niveau. Und die DQ-Offenlegung
ist das Signal "der verkauft mir nichts". Ein Lehrbuchfall von bedingter Fehlkalibrierung,
die von marginaler Coverage maskiert wird — das ist eine Beobachtung, die in diesem Raum
Respekt erzeugt, weil sie jeder von ihnen aus der eigenen Praxis kennt.

---

### Block 6 — Hauptranking (3.5 min, 1 Folie + 1 kleine Tabelle)

**Inhalt.** Deine Headline:

> Das beste zulässige Paket ist **`tirex2/ZS/decay_ocp_cqr`**: Winkler-Skill **0.144**,
> mittlere Coverage **0.787** (Ziel 0.80), zulässig in **10 von 10 Ländern**.
> Ein Zero-Shot-Foundation-Model, das nie auf diesen Daten trainiert wurde.

Nebensatz, nicht mehr: `cqr_static` hat mit 0.146 den höchsten Rohskill, deckt aber nur
0.750 und ist nur in 8 von 10 Ländern zulässig — nach dem validity-first-Prinzip aus dem
Evaluationskapitel fällt es damit aus der Wertung. *(Du hast noch ein zweites Argument, das
gratis ist: `cqr_static` hat eine Quantilkreuzungsrate von 0.724% im Mittel, mit Extremwerten
bis 77.95%; `decay_ocp_cqr` liegt bei 0.028%.)*

Vergleichsanker, drei Zeilen:
- bestes selbst trainiertes Modell: `lgbm/G/decay_ocp_cqr` 0.139 (aber nur 7/10 Länder)
- bester klassischer Ansatz: `rw/L/aci_cqr` 0.085 (10/10)
- **Die Spannung, die du explizit benennst:** dasselbe Modell, das bei der Punktprognose
  exakt auf RW-Niveau liegt (Block 4), erzeugt die besten Intervalle. Der Vorsprung liegt
  vollständig in der *Form* der prädiktiven Verteilung, nicht in ihrem Zentrum.

Dann das xLSTM-Ergebnis, ehrlich und ohne Beschönigung:
`xlstm` ist die **schwächste** Familie — Median-Skill über alle Kombinationen **−0.047**,
letzter Platz. Das beste zulässige Paket ist `xlstm/L/native` mit 0.094 — ausgerechnet mit
der *nativen* Quantilausgabe, also ohne Nutzen aus der CP-Schicht. Und es ist fragil: nur
1 von 3 Seeds ist überhaupt zulässig (Seed 44: 0.094; Seed 42: 0.043; Seed 43: −0.008),
die Seed-Spanne 0.102 ist **größer als der Skill selbst**.
Deine Erklärung, als Hypothese gekennzeichnet: 277 Monate × 10 Länder sind zu wenig Daten
für eine Architektur dieser Kapazität.

Und der praktisch wertvollste Satz des ganzen Vortrags:
> Innerhalb derselben Modellfamilie `tirex2` reicht der Skill über die 14
> Rekalibrierungsmethoden von **−0.044** bis **+0.146**. Die Wahl der Kalibrierungsmethode
> ist für das Ergebnis ungefähr so wichtig wie die Wahl des Modells.

**Warum.** Das ist die Antwort auf die Frage aus Block 0. Und die xLSTM-Ehrlichkeit ist
der Grund, warum sie dir den Rest glauben — ein saubergemessenes negatives Kernergebnis ist
wissenschaftlich mehr wert als ein herbeigeredetes positives, und dieses Publikum weiß das.
Der letzte Satz ist der, den sie am nächsten Tag im eigenen Team wiederholen: er sagt ihnen,
wo *sie* mit wenig Aufwand viel gewinnen können.

**Falle.** Projiziere **nicht** `figures/skill_heatmap.png`. 24 Zeilen × 14 Spalten sind auf
Projektionsentfernung unlesbar und kosten dich fünf Minuten Gemurmel. Wenn du ein Bild
willst: reduzier auf ein Balkendiagramm der 6–8 relevanten Zeilen. Die Heatmap gehört in den
Backup.

---

### Block 6b — RQ1 / RQ2 komprimiert (1 min, 1 Folie)

**Inhalt.** Deine beiden anderen vorregistrierten Forschungsfragen, je zwei Sätze. Nur die
Pointen, keine Panels:

- **Pooling über Länder** hilft den neuronalen Modellen nicht (G vs L: `d_norm` +0.017,
  also L besser). Fine-Tuning holt einen Teil zurück (−0.006), bleibt aber hinter dem
  lokalen Modell (+0.016). LightGBM ist die Ausnahme und gewinnt durch Pooling (−0.014).
  Sauberster Einzelbefund: Gewichts-Fine-Tuning schlägt eine reine Niveau-Korrektur klar
  (GF vs GI: −0.054; bei xLSTM −0.060, Panel-t −4.93, und der **einzige** RQ1-Kontrast,
  der über alle drei Seeds vorzeichenstabil ist).
- **Modelleigene adaptive Intervallbreite** bringt nichts: im Median ist die adaptive
  Variante *schlechter* als ihr Konstant-Breiten-Zwilling (+0.020). Dreiteilung: beim LSTM
  schadet sie konsistent (+0.062 in G und GF, in nur 10% der Länder besser, seed-stabil zu
  86%/79%), bei LightGBM hilft sie leicht (−0.018 / −0.009), bei xLSTM ist sie **nicht
  entscheidbar** (Seeds stimmen nur in 14–21% der Kontraste im Vorzeichen überein).
- Der Satz, der beide Blöcke verbindet und zurück auf Block 3 zeigt:
  **Die nützliche Adaptivität kommt aus der CP-Schicht, nicht aus dem Modell.** Die
  Online-Regler schlagen die native Ausgabe um −0.054 (`dtaci`, `pid`) — etwa das Zehnfache
  dessen, was die modelleigene Breitenadaptivität bewegt.

**Warum.** Diese zwei Fragen sind zwei Drittel deiner vorregistrierten Agenda; sie
komplett weglassen wäre eine Lücke, wenn jemand die Arbeit gelesen hat. Aber sie sind für
dieses Publikum die am wenigsten übertragbaren Ergebnisse (5 Regime, Zwillinge,
Seed-Stabilität). Eine Folie, ein Take-away, weiter. Der letzte Satz rechtfertigt
rückblickend, warum du überhaupt 14 Methoden verglichen hast.

**Wichtig für die Formulierung:** *kein einziger* RQ1-Kontrast ist auf Kontrastebene
FDR-signifikant (von 266 vorregistrierten Kontrasten haben 30 ein Median-q < 0.10, das
kleinste ist 0.019, gleichzeitig haben 131 ein |Panel-t| > 1.96). Formuliere alle Befunde
konsequent als **gerichtet**, nicht als signifikant: "consistently favours X", nicht
"significantly better". Der Panel-t fragt "ist der Effekt im Mittel über die Länder da?",
der q-Wert "ist er *innerhalb* einzelner Länder nach Korrektur nachweisbar?" — bei zehn
Ländern und 127 Monaten fällt die zweite Frage negativ aus. Ein Satz dazu, sonst wirkt es
widersprüchlich.

---

### Block 7 — Warum: die Winkler-Zerlegung (1.5 min, 1 Folie)

**Inhalt.** Der Mechanismus hinter dem gesamten Ranking.

Über die zulässigen Kombinationen: mittlere Breite 0.3301, Unterschreitungsstrafe 0.0521,
Überschreitungsstrafe ≈0.0715. **Die Breite macht 72.8% des mittleren Winkler-Scores aus.**
→ Der Score wird von der Schärfe dominiert, nicht von den Verletzungen. Das erklärt, warum
ein leicht unterdeckendes Verfahren wie `cqr_static` überhaupt vorne landen kann — und
rechtfertigt nachträglich, warum du das Gate *vor* das Ranking gestellt hast.

Dann der eigentlich interessante Befund:
Die Verletzungen sind der **Anzahl** nach praktisch symmetrisch (Tail-Asymmetrie ≈0.505,
Median genau 0.5). Die **Strafen** sind es nicht: 0.0715 oben gegen 0.0521 unten, also +37%
Übergewicht nach oben.
> Gleich häufig, aber wenn der Spread aus dem Intervall läuft, dann nach oben und weiter.

Ökonomisch plausibel (Ausweitungen sind sprunghaft, Einengungen graduell) und konsistent mit
der bekannten Rechtsschiefe von Kreditspreads — als Beobachtung formulieren, nicht kausal.
Konsequenz benennen: ein **symmetrischer** CQR-Score kann diese Asymmetrie strukturell nicht
abbilden. Und der Nicht-Befund, der das abrundet: über alle CP-Methoden reicht die
Asymmetrie nur von 0.47 bis 0.56 — die Rekalibrierung korrigiert das *Niveau* der Coverage,
aber nicht ihre Schiefe.

**Warum.** Das ist der einzige Block, in dem du einen *Mechanismus* zeigst statt eines
Rankings. Für ein Publikum, das Kreditrisiko im Bauch hat, ist "der Bruch, der weh tut, ist
der nach oben" die Aussage mit der höchsten intuitiven Trefferquote im ganzen Vortrag. Und
sie führt direkt zu deinem Future-Work-Punkt in Block 10.

---

### Block 8 — Risikomanagement-Nutzen (3 min, 2 Folien)

**Inhalt.**

*Folie A — Kapital.* Kapitalproxy = Intervallbreite relativ zur breitesten *zulässigen*
Breite im selben Land (`width / max width`, aus `diagnostics_master_v2.ipynb` Zelle 23).
Niedriger ist besser.
- `tirex2/ZS/decay_ocp_cqr`: **0.573**
- Median über alle 485 zulässigen Kombinationen: **0.669**, Spanne 0.551 bis 0.878
- Korrelation Skill ↔ Kapitalquotient: **−0.711** → Intervallgüte zahlt auf die
  Kapitalkennzahl ein (negativ ist hier der *erwünschte* Zusammenhang, weil ein niedriger
  Quotient gut ist — dieses Vorzeichen unbedingt erklären, sonst liest man es falsch)
- Korrelation Skill ↔ Sharpe der Handelsregel: **−0.147** → auf Handelsperformance zahlt es
  praktisch nicht ein

> Der Nutzen besserer Intervalle liegt im Risikomanagement, nicht im Alpha.

*Folie B — Regime, und das ist der praktisch entscheidende Teil.*
Zwei Stressachsen, beide ex ante fixiert: realisierte Spread-Volatilität in Terzilen
(rückwärtsgerichtet, `shift(1)`) und VIX ≤ 20 vs > 20.
- Realisierte Volatilität: Coverage **steigt** im Stress (0.7795 / 0.8166 / 0.8412), der
  Skill **fällt** (+0.1114 / +0.0100 / −0.0037).
- VIX: Coverage **fällt** (0.8342 → 0.7588), Skill fällt ebenfalls (+0.0513 → −0.0222).

Der scheinbare Widerspruch ist der interessanteste methodische Punkt des Kapitels und
löst sich so: die beiden Achsen sind fast orthogonal — 36 High-VIX-Monate, 38 Monate im
oberen Vol-Terzil, **nur 12 überlappen**; gleichzeitige Korrelation 0.009, stärkste
verzögerte 0.170 bei zwei Monaten Versatz.
> **VIX > 20 markiert den *Eintritt* in die Belastung, das obere Vol-Terzil ihr *Andauern*.
> Die Online-Regler hinken beim Eintritt hinterher (Coverage 0.759) und überschießen im
> Andauern (0.841).**

Und die Konsequenz, die ein Risikomanager hören muss:
Im oberen Vol-Terzil **bleibt** die Rangfolge (Spearman +0.846 gegen die
Gesamtrangfolge). Bei VIX > 20 **zerfällt** sie (+0.399): dein Spitzenpaket fällt von
Platz 1 auf Platz 8 (Skill +0.0219), bestes Paket wird `lgbm/G/decay_ocp_cqr`
(+0.0797), `arma/L/dtaci_cqr` steigt von Platz 12 auf Platz 6.
> Das Paket mit dem besten Gesamtergebnis ist nicht das, das in den Monaten hält, in denen
> der Stress *einsetzt*.

Regime-Zahlen deines Headline-Pakets `tirex2/ZS/decay_ocp_cqr` (aus dem Diagnostics-Lauf
`run_full_20260810_110834`; die Werte in `tab:res_regime` Panel C sind die der
`cqr_static`-Variante, siehe offener Punkt 1):

| Regime | Skill | Coverage | (Vergleich `cqr_static`) |
|---|---|---|---|
| calm | +0.1569 | 0.7218 | 0.1554 / 0.6535 |
| mid | +0.1129 | 0.7878 | 0.1186 / 0.7642 |
| stressed | +0.1806 | 0.8500 | 0.1820 / 0.8263 |
| VIX ≤ 20 | +0.1758 | 0.8055 | 0.1754 / 0.7648 |
| VIX > 20 | +0.0219 | 0.7389 | 0.0191 / 0.7111 |

Zwei Dinge, die du daraus mitnehmen solltest, weil sie deine Entscheidung stützen:
`decay_ocp_cqr` liegt in **4 von 5 Regimen** näher an der Zielcoverage 0.80 — der
`cqr_static`-Wert von 0.654 in ruhigen Monaten, der in der Arbeit als Caveat steht, ist bei
deinem Paket 0.722. Die eine Ausnahme nennst du von selbst: im oberen Vol-Terzil
**überschießt** `decay_ocp_cqr` stärker (0.850 gegen 0.826). Das ist genau der
Überschieß-Mechanismus aus Block 5 und dem Absatz oben, hier am eigenen Sieger sichtbar —
also kein Widerspruch, sondern ein Konsistenzbeleg deiner Erzählung. So auch formulieren.

Dazu die Komplexitätshypothese in einem Satz: der Vorsprung der komplexen Modelle schrumpft
von +0.1026 (low VIX) auf +0.0251 (high VIX), Δ −0.0775 — Hypothese widerlegt. Und die
ehrliche Zerlegung: das trägt vollständig der Zero-Shot-Arm (+0.1315 → +0.0254). Die auf
diesem Panel *trainierten* komplexen Modelle hatten nie einen nennenswerten Vorsprung
(+0.0143 in ruhigen Märkten) und liegen bei hohem VIX unter den klassischen. Also: was
existierte und dann verschwand, war ein *Pretraining*-Vorteil, keine "Komplexität".

**Warum.** Das ist die Folie, die zeigt, dass du über *ihr* Problem nachgedacht hast und
nicht nur über deine Metrik. Der Eintritt-vs-Andauern-Befund ist genuin und nicht offen-
sichtlich — er ist der Kandidat für die Frage, die sie dir danach stellen. Und die
F13-Zerlegung ist wieder ein Kompetenzsignal: du hast deinen eigenen vorregistrierten
Gruppenmedian auseinandergenommen, weil er kein einziges Modell beschreibt.

**Fallen.**
1. Die **Handelsregel gehört nicht in die Präsentation.** Der Signalfilter macht nur ~2%
   der Monate aktiv (`active_share` beim Spitzenpaket 0.019 bzw. 0.009 — das sind ein bis
   zwei aktive Monate von 127). Ein Sharpe auf so wenigen Monaten ist bedeutungslos, und
   *dieses* Publikum rechnet das im Kopf nach. Nimm nur den Kapitalproxy und die beiden
   Korrelationen, und sag von selbst, dass die Handelsregel ein Vergleichsmaß ohne
   Transaktionskosten ist, keine erzielbare Rendite.
2. Zeig **nicht** `figures/stress_drop.png` neben der Regime-Tabelle. Die Figur ist ein
   Mittel über *alle* Kombinationen, die Tabelle ein Median über die *zulässigen* — bei
   `native`, `cqr_static` und `decay_ocp_cqr` kippt dadurch das Vorzeichen. Wenn beides auf
   derselben Folie steht, fällt es auf.
3. `figures/rolling_coverage_top3.png` ist unbrauchbar: die `cqr_static`-Linie liegt
   vollständig hinter `native` und ist unsichtbar, und die Grafik zeigt nur AT.

---

### Block 9 — Was ich NICHT behaupte (2.5 min, 1 Folie) ← die Einstellungs-Folie

**Inhalt.** Vier Punkte, mit Zahlen, ohne Entschuldigungston.

1. **Kontamination des Siegers.** TiRex-2 wurde am 2026-07-01 veröffentlicht, der
   Pretraining-Cutoff ist nicht dokumentiert, mein Testfenster endet 2026-04. Die
   Renditereihen sind öffentlich und in praktisch jedem großen Zeitreihen-Korpus enthalten
   — es braucht keinen exotischen Datensatz, damit Kontamination plausibel ist.
   Drei Gegenargumente, alle drei aus dieser Arbeit: (a) die Punktprognose ist **nicht**
   besser als der Random Walk (hier kassierst du Block 4 ein: Panel-t −0.498, 0/10 Länder
   signifikant, bei nachgewiesener Power des Tests) — ein memorierendes Modell müsste im
   Median glänzen; (b) der Vorsprung liegt vollständig in der Breite, Memorierung würde
   primär das Zentrum treffen; (c) bei hohem VIX verschwindet der Vorsprung (0.1255 →
   −0.0025) — ein memorierendes Modell hätte gerade in den auffälligen Stressmonaten einen
   Vorteil.
   **Und dann der Satz, der die Folie trägt:** keines der drei Argumente ist ein Beweis.
   Ohne dokumentierten Cutoff ist die Frage empirisch nicht entscheidbar. Ein sauberer Test
   bräuchte ein Fenster nach dem Release.
2. **Grenzen der Inferenz.** **Kein einziger** der 66 Cross-Model-Kontraste ist nach FDR
   signifikant (kleinstes Median-q 0.185, Median 0.564). Die Model Confidence Set schließt
   fast nichts aus: 15 von 26 Paketen sind in allen 10 Ländern drin, FI und FR schließen
   keins aus, IE als trennschärfstes Land acht. Mit 127 Monaten und 10 korrelierten Ländern
   hat der Hansen-Test zu wenig Power.
   → **Was überlebt, ist die qualitative Ordnung (Zero-Shot ≥ LightGBM > klassisch > xLSTM),
   nicht die numerische Rangfolge.** Genau so formulieren.
3. **Seed-Fragilität.** In 51.7% der NN-Kombinationen ist die Seed-Spanne des Skills größer
   als der Betrag des Skills selbst; unter den zulässigen steigt der Anteil auf 58.6%.
   Heterogen: `lstm_const` 23.8%, `lstm` 68.5%. Für diese Fälle mache ich keine
   Rangaussagen — das war vorregistriert.
4. **Irland.** Die Rangfolge ist stabil ohne Irland (Spearman 0.939), das *Niveau* nicht:
   mittlerer Skill −0.021, beim Spitzenpaket −0.043 (0.146 → 0.103). Etwa ein Drittel des
   absoluten Vorsprungs stammt aus einem Land — und zwar aus dem "leichtesten" (IE
   Median-Skill 0.231 gegen PT 0.191), das wegen der MNE-Verzerrung im BIP ohnehin unter
   Vorbehalt steht.

**Warum.** Das ist die Folie, die über die Einstellungsfrage entscheidet. Ein Kandidat, der
von sich aus vier Wege aufzählt, auf denen seine eigene Headline falsch sein könnte, *mit
Zahlen*, liest sich senior. Und praktisch: sie nimmt jede kritische Frage vorweg, sodass die
Q&A auf "was würdest du als nächstes machen" umschwenkt statt auf "aber hast du bedacht".

**Vortragshinweis.** Diese Folie **nicht** hetzen und **nicht** entschuldigend sprechen.
Kein "leider", kein "natürlich hat jede Arbeit Grenzen". Sag es im gleichen Tonfall wie die
Ergebnisse. Das ist der Unterschied zwischen Eingeständnis und Souveränität.

---

### Block 10 — Takeaways + nächste Schritte (1.5 min, 1 Folie)

**Inhalt.** Drei Sätze für sie:
1. Für Monatsänderungen von Sovereign Spreads gibt es praktisch kein Punktprognose-Signal.
   Aufwand, der in die Verbesserung des Zentrums geht, ist verloren.
2. Der Wert liegt im Intervall — und der größte einzelne Hebel ist die
   Rekalibrierungsmethode, nicht die Modellarchitektur. Rohe Modellquantile sind
   systematisch zu breit (0.872 statt 0.80) und damit teuer.
3. Validiert wird am *Eintritt* in den Stress, nicht am Durchschnitt. Das Paket mit dem
   besten Gesamtergebnis war bei VIX > 20 nur Achter.

Drei nächste Schritte:
1. Asymmetrische oder zweiseitig getrennte Score-Familie — der symmetrische CQR-Score kann
   die gemessene Strafenasymmetrie (+37% oben) strukturell nicht abbilden.
2. Evaluationsfenster nach dem TiRex-2-Release, um die Kontaminationsfrage zu entscheiden.
3. Bedingte Kalibrierung entlang der Breitenachse — genau dort sagt der DQ-Test, dass alle
   Verfahren brechen (49.3% → 20.8% ohne den Breiten-Regressor).

**Warum.** Mit "was ich als nächstes machen würde" zu enden statt mit "vielen Dank" signalisiert,
dass du das Thema besitzt und nicht eine Aufgabe abgeschlossen hast. Das ist die eigentliche
Engagement-Aussage — stärker als jede Formulierung über Motivation.

---

## 2. Was bewusst NICHT reinkommt

| Weggelassen | Warum |
|---|---|
| Alle 14 CP-Methoden einzeln | Drei Klassen genügen. Namen wie `sfogd`, `saocp`, `mondrian` erzeugen nur Rauschen. |
| CQR-Score, Pinball-Loss, Gate-Formel | Nicht nötig, um ein Ergebnis zu verstehen. In den Backup. |
| Kalibrierungspool-Robustheit | Wichtig für die Arbeit, für den Vortrag zu fein. Ein Satz in Q&A reicht: pool-sensitiv sind nur 6 von 14 Methoden, die anderen 8 sind bit-identisch. |
| Burn-in-Diagnostik | Selbstlimitierend, kein Erkenntniswert für dieses Publikum. Backup. |
| Kalenderschnitt (QE vs Hiking) | Zu 90% kollinear mit der Volatilitätsachse — misst dieselbe Trennung unter falschem Namen. Wird auch in der Arbeit nicht berichtet. |
| Handelsregel / Sharpe | ~2% aktive Monate. Siehe Falle in Block 8. |
| Volle Skill-Heatmap | Auf Projektionsentfernung unlesbar. Backup. |
| Per-Land-Tabellen | Ein Satz im Fließtext (IE am leichtesten, IT am schwersten) statt zehn Zeilen. |

---

## 3. Backup-Folien (haben, nicht zeigen)

Reihenfolge nach Wahrscheinlichkeit, dass du sie brauchst:

1. Regime-Tabelle Panel B: Coverage calm/stressed und Skill je VIX-Regime für alle 14 Methoden
2. Volle Ranking-Tabelle: bestes zulässiges Paket je Modellfamilie (12 Zeilen)
3. Gate-Tabelle: Gate-Rate, mittlere Coverage, DQ-Ablehnrate je Methode (14 Zeilen)
4. Skill-Heatmap (Modell × Methode)
5. Kovariaten-Kontrast TiRex-2 (siehe Q&A-Frage 4)
6. Cross-Model-Matrix + MCS-Tabelle
7. Seed-Fragilität je Familie
8. Ex-Irland-Tabelle
9. Pool-Robustheit (6 vs 8 Methoden)
10. Refit-Blockstruktur / Zeitachse des Protokolls

---

## 4. Vorbereitete Antworten

Acht Fragen, die mit hoher Wahrscheinlichkeit kommen, mit der Zahl, die sie beantwortet.

**1. "Warum ist ein Modell, das nichts über Staatsanleihen weiß, besser als eines, das ihr
trainiert habt?"**
Weil es nichts *schätzen* muss. Es gibt keine Parameterschätzunsicherheit, weil keine
Parameter geschätzt werden; die prädiktive Verteilung kommt aus dem Pretraining auf
Millionen Zeitreihen. Und der Vorsprung sitzt genau dort: in der Verteilungsform, nicht im
Zentrum (Punktprognose auf RW-Niveau). Danach direkt die Kontaminationsfrage von selbst
aufgreifen — nicht warten, bis sie kommt.

**2. "Können wir das einsetzen?"**
Nicht als es steht, und aus einem Grund, der nichts mit dem Modell zu tun hat: der Sieger
hält bei VIX > 20 nicht (Platz 8, Skill 0.019). Was man einsetzen könnte, ist die
CP-Schicht auf ein Modell, das die Bank schon hat — das ist der Befund mit dem besten
Aufwand-Nutzen-Verhältnis: rohe Quantile passieren das Gate in 13.2% der Fälle, mit
adaptiver Rekalibrierung 100%.

**3. "Wieso 80% und nicht 99%?"**
0.1/0.9 ist das breiteste Quantilpaar, das *alle* Modelle nativ können (TiRex gibt nur
Dezile aus) — sonst hätte ich für die Foundation Models extrapolieren müssen und die
Vergleichbarkeit verloren. 80% hat außerdem bei 127 Monaten deutlich mehr Verletzungen und
damit mehr Testpower als ein 99%-Intervall. Für eine Kapitalanwendung müsste man das
wiederholen — die CP-Schicht ist auf jedes α anwendbar, die Testpower fällt aber.

**4. "Und die Makrodaten bringen wirklich nichts?"**
Der ceteris-paribus-Kontrast ist praktisch null: dasselbe Modell, dasselbe Testfenster,
einziger Unterschied sind die 17 Kovariaten. Median-Differenz im Skill +0.0036 (univariat
minus Kovariaten), Panel-t −0.75; über 140 Ländertests erreichen 18 ein p < 0.10 vor
Korrektur, erwartet wären 14, und sie teilen sich 10 zu 8 nach Richtung.
**Aber sofort präzise eingrenzen** — das ist die Stelle, an der die Aussage leicht
überdehnt wird: Das heißt *nicht*, dass Fundamentaldaten für Sovereign Spreads irrelevant
sind. Die Literatur erklärt *Niveaus* im Querschnitt; ich prognostiziere *Änderungen* über
einen Monat. Dazu kommt: durch das konservative Publication-Lag-Dictionary sieht das Modell
teils veraltete Daten. Und interessant ist der Rohbefund vor der CP-Schicht: die Kovariaten
*verengen* das Intervall in allen 10 Ländern um 2.2 bis 6.9% und schieben die Coverage von
0.843 auf 0.831, also **in Richtung** des Nominalwerts — sie helfen also, nur nicht messbar.

**5. "Ihr habt 742 Kombinationen getestet — wie viel davon ist Multiple Testing?"**
Alles läuft durch Benjamini-Hochberg, getrennt je Kontrastfamilie, und der
Kandidatenkatalog (13 Finding-Typen) stand vorher fest. Der beste Beleg dafür, dass er
vorher stand: einer der 13 Typen ist **leer** — F12 verlangt ein MAE-Verhältnis < 0.98 bei
negativem Skill, und das beste MAE-Verhältnis im ganzen Feld ist 0.9965. Die Bedingung
*kann* nicht erfüllt werden. Ein leerer vorregistrierter Finding-Typ ist der Beweis, dass
der Katalog nicht nachträglich passend gemacht wurde.

**6. "Wieso ist Coverage im Stress höher, aber die Qualität schlechter?"**
Weil die Regler nach Volatilitätsanstiegen aufweiten und Stressphasen persistent sind —
sie überschießen. Coverage steigt (0.841), der Winkler bezahlt es mit Breite. Und die
Kehrseite: in ruhigen Phasen sind die Intervalle zu schmal (0.7795 < 0.80). Das ist der
saubere Beleg dafür, dass Coverage allein kein Gütemaß ist. Genau deshalb ist Coverage bei
mir ein *Gate* und Winkler das Rangkriterium, nicht beides zusammengemischt.

**7. "Was wäre nötig, damit man dem Ranking statistisch glauben kann?"**
Mehr Zeit oder ein breiterer Querschnitt — beides senkt die Korrelation, die mir die Power
nimmt. Zehn Euro-Länder sind nicht zehn unabhängige Evidenzstücke; deshalb berichte ich den
Panel-t mit Zeit-Clustern *und* die per-Land-q-Werte, die verschiedene Fragen beantworten.
Bei 127 Monaten und dieser Querschnittskorrelation ist die Rangfolge deskriptiv, und ich
sage das so.

**8. "Warum hat xLSTM nicht funktioniert — Implementierungsfehler?"**
Die ehrliche Antwort ist: das kann ich nicht ausschließen, aber die Evidenz zeigt in eine
andere Richtung. Das LSTM mit demselben Protokoll und derselben Pipeline funktioniert
besser, und der Unterschied ist Kapazität: 277 Monate × 10 Länder. Der Beleg dafür, dass es
ein Daten- und kein Code-Problem ist, ist die Seed-Fragilität — in 50.6% der
xLSTM-Kombinationen ist die Seed-Spanne größer als der Effekt. Ein Implementierungsfehler
wäre systematisch, nicht seed-abhängig.

---

## 5. Offene Punkte, die du VOR der Präsentation klären musst

**(1) ERLEDIGT 2026-08-10 — Diagnostics-Rerun gelaufen, Regime-Zahlen vorhanden.**
Zur Klarstellung, weil es leicht falsch klingt: `decay_ocp_cqr` **wird** in allen Regimen
ausgewertet. Alle 742 Kombinationen × 14 Methoden stehen mit Coverage und Winkler in
`results/tables/regime_results.csv`. Die Lücke ist enger: die **`skill`-Spalte ist dort
komplett leer** (0 von 5194 Zeilen) — der Fix in Zelle 21 von
`notebooks/diagnostics_master_v2.ipynb` ist eingebaut, aber nie ausgeführt. Deshalb wurden
die Skills in `tab:res_regime` Panel C standalone nachgerechnet, und zwar nur für die 12
Ranking-Pakete — für `tirex2` also für die `cqr_static`-Variante.

**Der Rerun ist gelaufen** (`run_full_20260810_110834`, 10.08.2026, auf demselben
Conformal-Run). Die `skill`-Spalte ist jetzt im Artefakt: 5194/5194 gefüllt, dazu neu
`skill_iqr` und `n_countries`. **14 der 15 Tabellen sind bit-identisch** zum 03.08.-Lauf
(maximale absolute Differenz 0 über alle numerischen Spalten), nur `regime_results` ist von
9 auf 11 Spalten gewachsen. Alle bisher berichteten Zahlen bleiben also gültig.

Ergebnis für die Präsentation: **die Umstellung ändert deine Aussagen nicht.**
- `decay_ocp_cqr` ist im oberen Vol-Terzil ebenfalls Platz 1, bei VIX > 20 ebenfalls Platz 8
- Spearman gegen die Gesamtrangfolge identisch: +0.846 (stressed) / +0.399 (high VIX)
- bestes High-VIX-Paket bleibt `lgbm/G/decay_ocp_cqr` (+0.0797)

Getauscht sind zwei Zahlen, beide in Block 8 eingearbeitet: Skill bei VIX > 20 ist
**+0.0219** statt +0.0191, Coverage in ruhigen Monaten **0.7218** statt 0.654.

Was in der Arbeit dazu angepasst wurde: §8.3 nennt jetzt `decay_ocp_cqr` als Headline-Paket
mit Begründung, §8.9 führt beide Varianten, §8.11 nennt den ex-IE-Effekt für beide
(0.144 → 0.100), und die Herkunftsangaben zeigen auf den neuen Lauf.

**(2) Einheit der Intervallbreite.** Die mittlere Breite 0.3301 und der Winkler in
Level-Einheiten (z.B. GR 1.2413) lassen sich für dieses Publikum enorm greifbar machen —
"ein 80%-Band von rund 33 Basispunkten für einen Monat voraus" ist eine Zahl, die im Raum
hängen bleibt. Die Einheit ist in `data.tex` nirgends explizit genannt. Verifizier sie
einmal am Rohpanel, bevor du sie sagst.

**(3) ERLEDIGT — drei Zahleninkonsistenzen im Ergebniskapitel korrigiert.** Am neuen Lauf
verifiziert und im Text auf den belegten Wert gesetzt:
- Gate-Rate `native`: **0.1321**, also 13.2 % — der Fließtext sagte 13.3 %, korrigiert
- Überschreitungsstrafe: **0.071476**, also 0.0715 — an einer Stelle stand 0.0714, korrigiert
- Tail-Asymmetrie über alle Zellen: **0.504885**, also 0.5049 — im Text stand 0.5059,
  korrigiert

**Neu aufgefallen und NICHT geändert (deine Entscheidung):** der Absatz zur
Winkler-Zerlegung mischt zwei Grundgesamtheiten. Breite, Unter- und Überschreitungsstrafe
(0.3301 / 0.0521 / 0.0715) sind über die **zulässigen** Zellen gerechnet, die Tail-Asymmetrie
0.5049 über **alle** Zellen. Auf den zulässigen Zellen liegt die Asymmetrie bei **0.5359** —
dann ist die Aussage „basically symmetrical distributed" schwächer. Für den Vortrag ist das
egal, für einen Gutachter ist der Basiswechsel innerhalb eines Absatzes eine Angriffsfläche.
Entweder beide Größen auf die zulässigen Zellen umstellen (dann 0.536 und die Formulierung
anpassen) oder den Basiswechsel in einem Halbsatz benennen.

**(4) Zwei Skill-Definitionen nicht mischen.** Die Regime-Tabelle nutzt alle 14 Methoden
inkl. `native`, nur die 485 zulässigen Kombinationen, Median über Kombinationen. Der
vorregistrierte F13-Test schließt `native` aus, gatet **nicht** und nimmt den Median über
alle Zellen. Deshalb steht für `tirex2` bei hohem VIX einmal +0.0237 und einmal −0.003.
Wenn du beide Zahlen im selben Vortrag verwendest, sag bei jeder, welche Definition gilt —
oder benutze konsequent nur eine.
