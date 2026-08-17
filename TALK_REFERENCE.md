# Tiefenreferenz zum Vortrag — jeder Test, jede Korrektur, jede Zahl

Begleitdokument zu `TALK_SCRIPT.md`. Hier steht die Maschinerie, die im Skript nur referenziert
wird. Gedacht zum Durcharbeiten vor dem Vortrag, nicht zum Vorlesen.

**Inhalt**

1. [Die Bewertungsmaße](#1-die-bewertungsmaße)
2. [Die Kalibrierungstests](#2-die-kalibrierungstests)
3. [Mehrfachtestung und Korrekturen](#3-mehrfachtestung-und-korrekturen)
4. [Der Richtungstest (Pesaran–Timmermann)](#4-der-richtungstest-pesarantimmermann)
5. [Formaler Prognosevergleich (DM, HLN, Panel-t)](#5-formaler-prognosevergleich-dm-hln-panel-t)
6. [Model Confidence Set](#6-model-confidence-set)
7. [Daten, Kovariaten, Lags](#7-daten-kovariaten-lags)
8. [Die Modelle](#8-die-modelle)
9. [Die vierzehn CP-Methoden](#9-die-vierzehn-cp-methoden)
10. [CQR-Score, Pool, Skalierung](#10-cqr-score-pool-skalierung)
11. [Regimedefinitionen](#11-regimedefinitionen)
12. [Der ökonomische Block](#12-der-ökonomische-block)
13. [Glossar: was jede Spalte bedeutet](#13-glossar-was-jede-spalte-bedeutet)
14. [Q&A-Drill: 30 harte Fragen](#14-qa-drill-30-harte-fragen)

---

## 1. Die Bewertungsmaße

### 1.1 Empirische Coverage

```
cov^ = (1/T) · Σ_t 1{ Y_t ∈ C(X_t) }
```

`Y_t` ist der realisierte Spread-**Level** in Monat t, `C(X_t)` das prognostizierte Intervall
aus der zu t verfügbaren Information. Zielwert 1 − α = 0.80.

Was sie **nicht** sagt: nichts über die Breite (ein Intervall von minus unendlich bis plus
unendlich hat Coverage 1), nichts über den Zeitpunkt der Verletzungen, nichts über die
Verteilung innerhalb des Intervalls.

### 1.2 Winkler-Score (Interval Score)

```
W_α(l, u; y) = (u − l)  +  (2/α)·(l − y)·1{y < l}  +  (2/α)·(y − u)·1{y > u}
                 Breite      Strafe für Unterschreitung      Strafe für Überschreitung
```

Bei α = 0.20 ist der Strafgewichtsfaktor 2/α = **10**.

- **Proper scoring rule.** Der erwartete Score unter der wahren Verteilung wird durch Angabe
  der wahren Quantile minimiert. *Strictly* proper heißt: das Minimum ist eindeutig, es gibt
  keine andere Angabe mit demselben erwarteten Score. Praktische Konsequenz: **manipulations­
  resistent** — man kann den Score nicht durch systematisch zu breite oder zu schmale
  Intervalle verbessern.
- **Konkrete Größenordnung:** die mittlere Breite ist 0.3301. Eine Verletzung um 0.1
  Prozentpunkte kostet 10 · 0.1 = 1.0, also das Dreifache der mittleren Breite. Eine einzige
  größere Verletzung kann den Monatsbeitrag dominieren.
- **Zerlegung** (über die zulässigen Zellen): Breite 0.3301 (72.8 % des Scores),
  Unterschreitungsstrafe 0.0521 (11.5 %), Überschreitungsstrafe 0.0715 (15.7 %).
- **Nicht additiv zerlegbar über Modelle.** Man kann eine Verbesserung nicht auf eine einzelne
  Quelle zurückrechnen. Deshalb ist der Skill diagnostisch stumm — er sagt „besser", nicht
  „besser weil".

### 1.3 Winkler-Skill

```
Skill_m(c) = 1 − W̄_m(c) / W̄_rw/L/native(c)          pro Land c
Skill_m    = median über die 10 Länder
```

- Referenz ist **immer** `rw/L/native` desselben Landes und derselben Monate.
- **Common sample:** nur Monate, für die beide Verfahren eine Prognose haben. Ohne das würde
  ein Verfahren, das ausgerechnet Krisenmonate auslässt, künstlich gut aussehen.
- **Warum pro Land, dann Median:** Spread-Niveaus streuen um Faktor 9.5 (NL 0.201, GR 1.907),
  Volatilität um Faktor 8.8 (0.042 vs 0.372). Gepoolte rohe Winkler-Werte würde Griechenland
  dominieren. Der Median statt Mittel ist robust gegen GR und IE.
- **Preis des Medians:** er ist nicht linear. Die Differenz zweier Mediane ist nicht der Median
  der Differenzen. Deshalb können auf Folie 16 die Δ-Skill-Spalte und die paarweise DM-Spalte
  im Vorzeichen abweichen — beides ist korrekt, es sind verschiedene Größen.
- **Skill > 0** = besser als Random Walk. **Skill = 0** = identisch (die Referenz selbst).

### 1.4 MAE- und RMSE-Verhältnis

```
MAE-Ratio_m(c) = [ (1/T)·Σ|ŝ_m,t(c) − s_t(c)| ] / [ (1/T)·Σ|ŝ_rw,t(c) − s_t(c)| ]
```

Im **Level-Raum**, mit der Medianprognose (dem 0.5-Quantil) als Punktschätzer, pro Land, dann
Median über Länder. Verhältnis < 1 = besser als der Random Walk. RMSE analog auf quadrierten
Fehlern; er bestraft große Fehler stärker.

Dass MAE- und RMSE-Verhältnis für TiRex-2 fast gleich laufen (0.997 vs 0.999) heißt: der
Unterschied sitzt nicht in wenigen Ausreißermonaten.

### 1.5 Tail-Asymmetrie

```
tail_asym = (Anzahl unterer Verletzungen) / (Anzahl aller Verletzungen)
```

0.5 = symmetrisch. Werte: über alle Zellen Mittel 0.5049, Median exakt 0.5000; über die
**zulässigen** Zellen 0.5359. Je CP-Methode nur 0.47–0.56, je Land 0.431 (FI) bis 0.642 (IE),
je Modellfamilie 0.483 (xlstm) bis 0.618 (arma_monthly).

**Länderheterogenität > Modellheterogenität.** Passt zum Kendall-Befund (§13).

### 1.6 Pinball-Loss — nicht verwechseln

```
L(q̂, y) = (1/|T|) · Σ_τ max( τ·(y − q̂_τ), (τ−1)·(y − q̂_τ) )
```

Das ist die **Trainings**-Zielfunktion der quantilschätzenden Modelle (LightGBM, LSTM, xLSTM),
mit τ ∈ {0.1, 0.5, 0.9}. Es ist **nicht** das Bewertungsmaß. Random Walk und ARMA werden
überhaupt nicht auf eine Punktfehler-Loss gefittet — ihre Quantile kommen aus den empirischen
In-Sample-Residuen.

Falls gefragt „warum trainiert ihr auf Pinball und bewertet auf Winkler?": Pinball ist die
natürliche Loss für Quantilregression und für jedes τ separat proper; der Winkler-Score ist die
proper Loss für das *Intervall* als Ganzes. Beide sind konsistent in dem Sinne, dass die wahren
Quantile beide minimieren.

### 1.7 Warum α = 0.20 und nicht 0.01

Bei 127 Testmonaten:

| α | erwartete Verletzungen je Zelle | Testpower |
|---|---|---|
| 0.20 | 25.4 | brauchbar |
| 0.05 | 6.35 | schwach |
| 0.01 | 1.27 | praktisch null |

Ein Kalibrierungstest auf 1.27 erwarteten Verletzungen kann fast nichts erkennen. Der zweite
Grund ist die Vergleichbarkeit: TiRex und TiRex-2 geben nur Dezile aus, 0.1/0.9 ist das
breiteste Paar ohne Extrapolation für irgendein Modell.

---

## 2. Die Kalibrierungstests

Notation für diesen Abschnitt: `I_t = 1{Y_t ∉ C(X_t)}` ist der Verletzungsindikator,
`T` = 127 Monate, `x = Σ I_t` = Anzahl Verletzungen, `π̂ = x/T` = empirische Verletzungsrate,
`α = 0.20` = Zielverletzungsrate.

### 2.1 Kupiec — Unconditional Coverage (UC)

**Frage:** Ist die Verletzungs*rate* richtig?

**Nullhypothese:** π = α, also die Verletzungsrate entspricht dem Nominalwert.

```
LR_uc = −2·ln[ α^x·(1−α)^(T−x) / ( π̂^x·(1−π̂)^(T−x) ) ]   →   χ²(1)
```

Das ist ein Likelihood-Ratio-Test: Zähler = Likelihood unter der Nullhypothese (Rate ist
exakt α), Nenner = Likelihood unter der besten Anpassung (Rate ist π̂). Weicht π̂ von α ab,
wird der Bruch klein, der negative Logarithmus groß, die Statistik groß.

**1 Freiheitsgrad**, weil genau ein Parameter (die Rate) frei geschätzt wird.

**Was er ignoriert:** *wann* die Verletzungen auftreten. 25 Verletzungen am Stück und 25
verstreute Verletzungen ergeben identische Kupiec-Statistiken.

**Rohe Ablehnraten in dieser Arbeit:** p < 0.05 in 23.4 % aller 7 420 Zellen → nach BH-FDR
q < 0.10 in 20.6 %.

### 2.2 Christoffersen — Independence (IND)

**Frage:** Clustern die Verletzungen?

**Nullhypothese:** `I_t` ist seriell unabhängig, also π_01 = π_11.

Konstruktion über die Übergangszählungen `n_ij = #{t : I_{t−1} = i, I_t = j}` mit
i,j ∈ {0,1}. Also: n_00 = kein Bruch nach keinem Bruch, n_01 = Bruch nach keinem Bruch,
n_10 = kein Bruch nach Bruch, n_11 = Bruch nach Bruch. Dazu
`π̂_ij = n_ij / (n_i0 + n_i1)` als bedingte Übergangswahrscheinlichkeiten.

```
LR_ind = −2·ln[ (1−π̂)^(n00+n10) · π̂^(n01+n11)
              / ( (1−π̂_01)^n00 · π̂_01^n01 · (1−π̂_11)^n10 · π̂_11^n11 ) ]   →   χ²(1)
```

Zähler: eine gemeinsame Rate π̂ für beide Vorzustände. Nenner: zwei separate Raten. Unter der
Nullhypothese stimmen sie überein, also trägt eine Verletzung keine Information über die
nächste.

**Rohe Ablehnraten:** p < 0.05 in 16.2 % → q < 0.10 in nur **6.1 %**. *Zeitliches Clustering
ist in dieser Arbeit nachweislich nicht das Problem* — das ist die Gegenprobe zum DQ-Befund.

### 2.3 Christoffersen — Conditional Coverage (CC)

```
LR_cc = LR_uc + LR_ind   →   χ²(2)
```

Die Freiheitsgrade addieren sich, weil zwei Nullhypothesen gleichzeitig getestet werden:
richtige Rate **und** Unabhängigkeit. Das ist der zweite Test im Gate.

**Rohe Ablehnraten:** p < 0.05 in 31.0 % → q < 0.10 in 26.3 %.

### 2.4 Dynamic Quantile (Engle & Manganelli)

**Frage:** Sind die Verletzungen aus ihrer eigenen Vergangenheit **oder aus der
Intervallbreite** prognostizierbar?

Zentrierte Treffer: `Hit_t = I_t − α`. Unter korrekter Kalibrierung ist E[Hit_t] = 0 und
Hit_t ist unkorreliert mit allem, was zu t bekannt ist.

Instrumentenmatrix Z, deren t-te Zeile ist:
```
z_t = ( 1, Hit_{t−1}, Hit_{t−2}, Hit_{t−3}, Hit_{t−4}, û_t − l̂_t )
```
Also: Konstante, vier eigene Lags, **und die aktuelle Intervallbreite**.

```
DQ = [ Hit' · Z · (Z'Z)^(−1) · Z' · Hit ] / [ α·(1−α) ]   →   χ²(q),  q = 6
```

Das ist eine Wald-Statistik auf die Regression von Hit auf Z: unter der Nullhypothese sind
alle sechs Koeffizienten null. Die Normierung α(1−α) ist die Varianz von Hit_t unter der
Nullhypothese.

**Warum der Breitenregressor der entscheidende ist** — die Zerlegung dieser Arbeit
(post hoc, 2026-08-03, gleiche Wald-Statistik, gleiche BH-Familie):

| Spezifikation | Spalten | df | Ablehnrate bei q < 0.10 |
|---|---|---|---|
| Voll: Konstante + 4 Lags + Breite | 6 | 6 | **49.3 %** |
| Ohne Breite: Konstante + 4 Lags | 5 | 5 | **20.8 %** |
| Nur Breite: Konstante + Breite | 2 | 2 | **68.9 %** |
| Breitenkoeffizient γ allein | — | 1 | 53.2 % |

Die 20.8 % ohne Breite liegen praktisch auf dem UC-Niveau von 20.6 %. **Die DQ-Ablehnungen
sind fast vollständig ein Breiten- und kein Lag-Phänomen.**

**Richtung des Effekts:** Von den 3 949 Zellen mit FDR-signifikantem γ haben **99.9 % ein
negatives γ**, Median γ = −1.631.

γ < 0 heißt: in Monaten mit **breitem** Intervall liegt `I_t − α` unter null, also
Überdeckung; in Monaten mit **schmalem** Intervall darüber, also Unterdeckung. Die
Breitensteuerung reagiert damit in die *richtige* Richtung, aber sie **überschießt in beide**:
sie macht zu weit auf, wenn sie aufmacht, und zu weit zu, wenn sie zumacht. Marginal mittelt
sich das auf 0.80 heraus — deshalb sieht Kupiec nichts. **Das ist der Lehrbuchfall von
bedingter Fehlkalibrierung, die von marginaler Coverage maskiert wird.**

**Zwei saubere Fehlermodi:**

| Klasse | Lag-Ablehnung | Breiten-Ablehnung | Diagnose |
|---|---|---|---|
| statisch (native, cqr_static, saocp, decay_ocp) | 40–54 % | 7–20 % | adaptiert nicht |
| online (pid, spci, sfogd, pid_local, aci, agaci, dtaci) | pid_cqr: 1.5 % | 50–97 % | überschießende Breitenreaktion |

Formulierung: *„the conformal layer trades temporal dependence of violations for
width-conditional dependence."*

### 2.5 Power — warum Nicht-Verwerfung kein Beweis ist

Alle vier Tests sind asymptotisch. Bei T = 127 und 25 erwarteten Verletzungen ist die Power
moderat. Ein Test, der nicht verwirft, sagt: *die Daten widersprechen der Nullhypothese
nicht* — nicht: *die Nullhypothese gilt*. Das steht explizit in den Limitations und du sagst
es von selbst. Das Gate ist deshalb ein **Ausschluss**kriterium, kein Gütesiegel.

---

## 3. Mehrfachtestung und Korrekturen

### 3.1 Das Problem in Zahlen

Jede Zelle (Modell × Regime × Seed × Methode × Land) liefert einen p-Wert pro Test. Das sind
7 420 Zellen × 4 Tests = **29 680 p-Werte** allein für die Kalibrierung; über alle Tests der
Arbeit 42 170 Testzellen. Bei einem Signifikanzniveau von 5 % erwartet man allein durch Zufall
rund 1 484 Verwerfungen — ohne jeden echten Effekt.

### 3.2 Bonferroni vs. Benjamini–Hochberg

| | kontrolliert | Formel | Wirkung hier |
|---|---|---|---|
| **Bonferroni** | FWER: P(mind. ein Fehler ≤ α) | p_adj = m·p | bei m = 7 420 so konservativ, dass praktisch nichts verworfen wird → **jede** Kombination würde als kalibriert gelten, auch die kaputten |
| **Benjamini–Hochberg** | FDR: E[Anteil falscher Entdeckungen] | siehe §3.3 | verwirft substanziell, kontrolliert aber den erwarteten Fehlanteil |

Bei einer Screening-Aufgabe — „welche der 742 Kombinationen sind brauchbar?" — ist FDR das
richtige Instrument. FWER wäre angemessen, wenn eine einzige falsche Entdeckung katastrophal
wäre. Hier ist der Zweck, eine Menge zu filtern.

### 3.3 Die BH-Formel

Für sortierte p-Werte `p_(1) ≤ … ≤ p_(m)`:

```
q_(i) = min_{j ≥ i}  min( (m/j)·p_(j),  1 )
```

Zu lesen als: nimm für jeden Rang j den skalierten Wert (m/j)·p_(j), kappe bei 1, und nimm dann
das Minimum über alle Ränge ab i. Das äußere Minimum erzwingt Monotonie — sonst könnte ein
q-Wert kleiner sein als der eines besser gerankten Tests.

**Interpretation des q-Werts:** Verwirft man alle Tests mit q ≤ 0.10, dann sind im Erwartungs­
wert höchstens 10 % der Verwerfungen falsch positiv.

**Was FDR nicht kontrolliert:** die Fehlerwahrscheinlichkeit *einer einzelnen* Aussage. Ein
q-Wert von 0.09 heißt nicht „diese Zelle ist mit 91 % Wahrscheinlichkeit fehlkalibriert". Er
ist eine Eigenschaft der Entscheidungsregel auf der ganzen Familie.

### 3.4 Die Familien und warum sie getrennt sind

| Familie | Inhalt | n Kontraste |
|---|---|---|
| Kalibrierung | UC, IND, CC, DQ je Zelle | 7 420 Zellen |
| RQ1 | G vs L, GF vs G, GF vs GI | 266 |
| RQ2 | adaptiv vs const-Zwilling | 280 |
| RQ3 | jede Methode vs native | 689 |
| F10 | arma vs arma_monthly | 14 |
| XM (post hoc) | Cross-Model, beste Pakete paarweise | 66 |
| GF vs L (post hoc) | eigene Familie, damit RQ1-q-Werte unberührt bleiben | 84 |
| Q4 Kovariaten (post hoc) | tirex2 vs tirex2_cov je Land und Methode | 140 |

**Warum getrennt:** FDR kontrolliert den Fehlanteil *innerhalb* einer Familie. Wirft man alles
in einen Topf, wird die Korrektur durch die Größe der anderen Fragen mitbestimmt — ein
zusätzlicher Robustheitstest würde die Signifikanz der Hauptfrage verschlechtern, was
inhaltlich unsinnig ist. Umgekehrt: die Aufteilung ist eine Entscheidung, die man
vorregistrieren muss, sonst ist sie manipulierbar („ich mache so lange kleine Familien, bis
etwas signifikant wird").

**Wo diese Entscheidung sichtbar wird:** bei Q4 überlebt in der eigenen Familie von 140 Tests
**keiner** (kleinstes q 0.42, Median 0.98). Bettet man dieselben 140 in die große Familie der
12 490 Länderzellen ein, überleben **acht** (kleinstes q 0.013, fünf für und drei gegen die
Kovariaten). Beide Rechnungen sind in der Arbeit ausgewiesen. Die Trennung wird begründet mit:
diese Tests beantworten eine andere Frage als die vorregistrierten Kontraste.

### 3.5 Das Zulässigkeitsgate

```
(1/|C|) · Σ_{c ∈ C} 1{ q_UC(c) ≥ 0.10  ∧  q_CC(c) ≥ 0.10 }   ≥   0.70
```

C sind die zehn Länder. Also: in mindestens sieben von zehn Ländern darf **weder** der Kupiec-
**noch** der Christoffersen-CC-Test nach BH-Korrektur verwerfen.

**Drei Designentscheidungen mit Tradeoff:**

1. **70 % statt 100 %.** Streng heißt: fast alles fällt raus, es gibt nichts zu ranken. Lax
   heißt: das Gate ist zahnlos. Bei 70 % fallen 34.6 % durch — messbar wirksam, ohne den
   Suchraum zu vernichten. Vorregistriert.
2. **q ≥ 0.10 statt p ≥ 0.05.** Ohne Korrektur bei 7 420 Zellen unbrauchbar (§3.1).
3. **Nur UC und CC, nicht DQ.** DQ verwirft 49.3 % — im Gate würde er die zulässige Menge
   leeren. Er wird berichtet, nicht gegatet. Das ist die wichtigste methodische Limitation
   (§2.4).

**Warum Kalibrierung gatet und nicht rankt:** Coverage ist trivial maximierbar durch
Verbreiterung. Als Score wäre sie also wertlos. Das ist die operative Umsetzung von
„maximise sharpness subject to calibration" (Gneiting & Raftery).

---

## 4. Der Richtungstest (Pesaran–Timmermann)

**Frage:** Prognostiziert das Modell das *Vorzeichen* der Änderung besser als Zufall?

```
PT = ( P̂_S − P̂_* ) / sqrt( V̂(P̂_S) − V̂(P̂_*) )   →   N(0,1)
```

- `P̂_S` = beobachtete Trefferquote der Richtungsprognose
- `P̂_*` = erwartete Trefferquote unter Unabhängigkeit von Prognose und Realisation, also
  `P̂_y·P̂_x + (1−P̂_y)·(1−P̂_x)` mit P̂_y = Anteil positiver Realisationen, P̂_x = Anteil
  positiver Prognosen
- die Varianzen sind konsistente Schätzer der jeweiligen Größen

**Ergebnisse:** mittlere Trefferquote 0.5397 über alle Zellen; der Test verwirft in **7.2 %**
der Zellen auf 5 % (bzw. 7.74 % der *definierten* Tests). Unter reinem Zufall erwartet man
5 %. Also praktisch keine ausbeutbare Richtungsinformation.

**Der degenerierte Fall — wichtig, weil er auf Folie 10 sichtbar ist:** Der Random Walk
prognostiziert konstant Δ = 0. Damit ist P̂_x konstant (0 oder 1), `V̂(P̂_*)` wird gleich
`V̂(P̂_S)`, der Nenner wird **exakt null** und die Statistik ist undefiniert. In 90 % der
`rw`-Zellen ist `pt_stat` NaN.

Konsequenz: die Trefferquote 0.5827 des Random Walk ist **kein Befund**, sondern schlicht der
Anteil der Monate ohne Spread-Anstieg (die Vorzeichenkonvention zählt Δ = 0 als „nicht
gestiegen"). Und „0 % signifikante PT-Tests" heißt „Test undefiniert", nicht „keine Evidenz".
Als Artefakt benennen, nicht deuten.

---

## 5. Formaler Prognosevergleich (DM, HLN, Panel-t)

### 5.1 Diebold–Mariano

**Idee:** Vergleiche nicht die Modelle, sondern deren *Verluste*. Verlustdifferenzial

```
d_t = W_{A,t} − W_{B,t}
```

**Nullhypothese:** E[d_t] = 0, also gleiche erwartete Prognosegüte.

```
DM = d̄ / sqrt( LRV̂(d) / n )
```

`d̄` ist der Mittelwert, `LRV̂` die geschätzte Long-Run-Varianz (die Autokorrelation der
Differenziale muss berücksichtigt werden). Asymptotisch N(0,1).

**Wichtiger Caveat (Diebold 2015):** DM ist ein Test der *Prognosen*, nicht der *Modelle*. Bei
genesteten Modellen mit geschätzten Parametern ist er mit Vorsicht zu verwenden. Hier
unkritisch, weil ich Prognose-Outputs vergleiche und keine Populationsmodelle — aber die
Arbeit nennt es ausdrücklich.

### 5.2 Harvey–Leybourne–Newbold-Korrektur

Der DM-Test ist in kleinen Stichproben überdimensioniert, verwirft also zu oft. HLN (1997)
korrigieren mit dem Faktor

```
DM* = DM · sqrt( [ T + 1 − 2h + h(h−1)/T ] / T )
```

Bei h = 1 vereinfacht sich das auf `sqrt((T−1)/T)`. HLN empfehlen zusätzlich, gegen die
**t-Verteilung mit T−1 Freiheitsgraden** zu vergleichen statt gegen die Standardnormale.
Bei T = 127 also **df = 126** — das ist die Zahl, die in allen Tabellennotizen der Arbeit
steht.

### 5.3 Panel-t mit Newey–West

**Das Problem:** Zehn Euro-Länder sind nicht zehn unabhängige Evidenzstücke. Ihre Spreads sind
stark querschnittskorreliert. Zehn separate Ländertests als zehn Bestätigungen zu zählen wäre
irreführend.

**Die Lösung:** Erst über den Querschnitt mitteln, dann testen.

```
d̄_t = (1/10) · Σ_c d_{c,t}                 pro Monat t
t_panel = d̄ / SE_NW(d̄)                     Newey–West-HAC, 3 Lags
```

Damit ist die Querschnittskorrelation im Mittelwert absorbiert, und die HAC-Standardfehler
fangen die verbleibende zeitliche Autokorrelation. Die Statistik ist asymptotisch
standardnormal und hat **keine** Freiheitsgrade.

Newey–West mit Bartlett-Kernel und 3 Lags: die Varianz wird als gewichtete Summe der
Autokovarianzen bis Lag 3 geschätzt, mit linear fallenden Gewichten.

### 5.4 Warum Panel-t und q-Wert auseinanderlaufen — die zentrale Erklärung

Diese Diskrepanz taucht in der Arbeit überall auf, und du musst sie erklären können:

| | Frage | Aggregation | Bei 10 Ländern × 127 Monaten |
|---|---|---|---|
| **Panel-t** | Ist der Effekt *im Mittel über die Länder* vorhanden? | erst Querschnittsmittel, dann ein Test | hat Power, weil 127 Monate ein Signal tragen können |
| **Median-q** | Ist er *innerhalb einzelner Länder* nach Korrektur nachweisbar? | zehn separate Tests, dann BH, dann Median | fällt negativ aus, weil jedes einzelne Land zu wenig Daten hat |

**Konkret bei RQ1:** von 266 vorregistrierten Kontrasten haben **30** ein Median-q < 0.10
(kleinstes 0.019), gleichzeitig haben **131** einen |Panel-t| > 1.96. Das ist kein Widerspruch,
sondern zwei Auflösungsstufen derselben Frage.

**Formulierungskonsequenz:** alle Befunde als **gerichtet** formulieren — „consistently
favours X" — und nie als „significantly better".

### 5.5 Die Kennzahlen je Kontrast

| Größe | Definition | Lesart |
|---|---|---|
| `d_norm` | `mean(d) / mean(W_ref)`, Median über Länder | **negativ = A besser** (Winkler ist ein Verlust) |
| `share_a_better` | Anteil der Länder, in denen A den geringeren Verlust hat | 0.5 = kein Muster |
| `t_panel` | §5.3 | \|t\| > 1.96 ≈ 5 %-Niveau, ohne Korrektur für Mehrfachvergleiche |
| `median_q` | Median der BH-korrigierten p-Werte der Ländertests | < 0.10 = innerhalb der Länder nachweisbar |
| `share_sig_fdr` | Anteil der Länderzellen mit q < 0.10 | |
| `seed_stable` | Anteil der Kontraste, deren **Vorzeichen** über Seeds 42/43/44 übereinstimmt | vorregistrierte Bedingung für eine Rangaussage |

**Warum `d_norm` normalisiert wird:** rohe Winkler-Differenzen sind nicht über Länder
vergleichbar (Skalenproblem, §1.3). Die Division durch den Referenz-Winkler macht sie
skalenfrei und interpretierbar als „Prozent des Referenzverlusts".

### 5.6 Giacomini–White — bewusst nicht implementiert

GW testet *bedingte* Prognosefähigkeit und berücksichtigt Schätzunsicherheit. Bei h = 1,
festem Refit-Protokoll und identischen Informationsmengen fällt der GW-Vorteil mit diesem
Design weitgehend zusammen. Dokumentierte Scope-Entscheidung, begründet in
`diagnostics_foundation.txt` §4.1 — kein Versehen.

---

## 6. Model Confidence Set

**Frage:** Welche Modelle sind auf Konfidenzniveau 1 − α *nicht* statistisch vom besten
unterscheidbar?

**Idee (Hansen, Lunde, Nason 2011):** Iterativ. Teste die Nullhypothese „alle Modelle in der
aktuellen Menge sind gleich gut" (Equal Predictive Ability). Wird sie verworfen, entferne das
schlechteste Modell und wiederhole. Wird sie nicht verworfen, ist die verbleibende Menge die
MCS.

**Statistik:** `T_max = max_i t_i`, wobei `t_i` das standardisierte mittlere
Verlustdifferenzial von Modell i gegen den Durchschnitt der aktuellen Menge ist.

**Verteilung:** nicht analytisch bekannt, deshalb **Moving-Block-Bootstrap** (Künsch 1989):
Ziehe Blöcke aufeinanderfolgender Beobachtungen, damit die Autokorrelationsstruktur der
Verlustdifferenziale erhalten bleibt. Konfiguration hier: **Blocklänge 6, 1 000 Draws,
α_MCS = 0.10, RNG-Seed 42**. Loss = Winkler, pro Land gerechnet.

**Terminal p-value:** der p-Wert des Schritts, an dem die Prozedur stoppt. *Innerhalb eines
Landes teilen alle überlebenden Pakete denselben terminalen p-Wert*, deshalb wird er einmal je
Land berichtet, nicht je Paket.

**Ergebnisse:** 26 Kandidaten × 10 Länder = 260 Entscheidungen, davon **238 Inklusionen**
(Rate 0.915). 15 Kandidaten überleben in allen zehn Ländern, neun weitere in mindestens acht.
Nur `rw/L/native` (p 0.087) und `xlstm/GI/native` (p 0.090) fallen in der Hälfte heraus.

| Land | Setgröße | Terminal p | Ausgeschlossen |
|---|---|---|---|
| FI | 26 | 0.103 | 0 |
| FR | 26 | 0.195 | 0 |
| AT | 25 | 0.366 | 1 |
| IT | 25 | 0.297 | 1 |
| ES, GR, NL, PT | 24 | 0.121–0.406 | 2 je |
| BE | 22 | 0.326 | 4 |
| IE | 18 | 0.118 | 8 |

Setgröße und terminaler p-Wert laufen **nicht** parallel: Finnland behält alle 26 beim
*niedrigsten* p (0.103), die Niederlande schließen zwei aus beim *höchsten* (0.406). Das ist
kein Fehler — der terminale p-Wert bezieht sich auf den letzten, nicht verworfenen Test.

**Warum der Kandidatensatz kuratiert ist** (26 statt 742): bei 742 hochkorrelierten Kandidaten
ist die MCS überhaupt nicht trennscharf, praktisch alles bleibt drin. Kuratiert wurde: bestes
*zulässiges* Paket je Modell × Regime, plus die RW-Referenz erzwungen, damit der Test etwas
über sie sagen kann. **Der Tradeoff ist ehrlich zu nennen:** Kuratierung ist eine
Vorentscheidung und muss vorregistriert sein, sonst ist sie Selektionsverzerrung. Und sie
verschärft das Power-Problem: jeder Kandidat ist schon der beste aus 9 bis 30 Optionen, also
liegen sie eng zusammen.

**Warum die MCS hier nicht trennt, und dass das kein Implementierungsfehler ist:** 127 Monate,
zehn korrelierte Länder. Der Hansen-Test hat zu wenig Power. Das ist eine Aussage über die
Datenlage, nicht über den Code.

**Seeds der Kandidaten:** 42, außer Kandidaten 5, 6, 17, 18, 20, 23 (Seed 43) und 8, 11, 19
(Seed 44).

---

## 7. Daten, Kovariaten, Lags

### 7.1 Panel

10 Länder (AT, BE, ES, FI, FR, GR, IE, IT, NL, PT), monatlich, 2003-04 bis 2026-04, 277
Monate, balanciert 2 770 Länder-Monate. Ein fehlender Wert: griechischer Spread 2015-07
(Kapitalverkehrskontrollen). Für 2015-08 fehlt zusätzlich der Level-Anker s(t−1) für die
Rücktransformation. Beide Monate liegen im Burn-in; das Testfenster ist für alle zehn Länder
vollständig.

Spread = ECB-10y-Rendite des Landes minus Deutschland (Datensatz IRS, Serienschlüssel
`M.{cc}.L.L40.CI.0000.EUR.N.Z`).

Speicherung im Wide-Format: Index = Monatsende, Spalten = MultiIndex (Land, Variable). Der
Spread-Level ist **unverschoben** gespeichert, es gibt kein vorverschobenes Target — das Label
Δs(t+1) wird modellseitig konstruiert.

Zweite Besonderheit Griechenlands: das Private Sector Involvement 2012 (Schuldenschnitt)
erzeugt einen Strukturbruch in der Spread-Reihe.

### 7.2 Die 17 Features

5 global (identisch über Länder), 12 lokal (als Differenz zu Deutschland).

| # | Feature | Scope | Lag (d) | Shift (m) | Herkunft |
|---|---|---|---|---|---|
| 1 | `spread_lag` = s(i, t−1) | lokal | 0 | 0 | Afonso 2015 |
| 2 | `vix_log` = log(VIX) | global | 0 | 0 | Afonso 2015 |
| 3 | `us_baa_aaa` = Moody's Baa − Aaa | global | 0 | 0 | Bernoth 2004 |
| 4 | `us_aaa_treasury` = Aaa − 10y UST | global | 0 | 0 | Codogno 2003 |
| 5 | `epu_log` = log(EPU Europa) | global | 0 | 0 | Baker 2016 |
| 6 | `debt_gdp` rel. DE | lokal | 113 | 4 | Afonso 2015 |
| 7 | `debt_gdp_sq` | lokal | 113 | 4 | Afonso 2015 |
| 8 | `vix_log_x_debt` | lokal | split | — | Afonso 2015 |
| 9 | `us_aaa_treasury_x_debt` | lokal | split | — | Codogno 2003 |
| 10 | `fiscal_balance` = 4Q-Mittel(B9) rel. DE | lokal | 113 | 4 | Afonso 2015 |
| 11 | `debt_service_rev` = 4Q(D41PAY)/4Q(TR) rel. DE | lokal | 113 | 4 | Bernoth 2004 |
| 12 | `ip_growth_yoy` rel. DE | lokal | 45 | 2 | Afonso 2015 |
| 13 | `reer_log` rel. DE | lokal | 30 | 1 | Afonso 2015 |
| 14 | `liquidity_share` = debt_i / Σ debt_j (11 incl. DE) | lokal | 113 | 4 | Bernoth 2004 |
| 15 | `target2_gdp` = (TARGET2/GDP)·100 | lokal | 33 | 2 | De Grauwe 2013 |
| 16 | `pc2_core_periph` | global | 0 | 0 | Afonso 2015 |
| 17 | `rating_num` rel. DE, Skala 1–17 | lokal | 0 | 0 | Afonso 2012 |

**Die vier Ergänzungen über Afonso hinaus** und ihre Begründung: `us_aaa_treasury` (Codogno:
US-Kreditrisiko ist eigenständiger Treiber), `us_baa_aaa` (Bernoth: Investoren-Risikoaversion),
`epu_log` (Bernal: Policy-Uncertainty), `us_aaa_treasury_x_debt` (Interaktion analog zu
Afonsos VIX-mal-Schulden).

**Warum Abweichung statt Level in den Interaktionen:** `vix_log_x_debt` nutzt die Abweichung
des log-VIX von seinem expandierenden Mittel. Der Level wäre stark kollinear mit dem reinen
Schuldenterm.

**PC2:** zweite Hauptkomponente der z-standardisierten Länderdaten, 12-Monats-Refit auf
expandierendem Fenster, mindestens 36 Monate. Ladungen so verankert, dass Peripherieländer
immer positiv laden — sonst kippt das Vorzeichen zwischen Refits. Fehlender GR-Wert wird mit 0
imputiert. **PC2 misst den Core-Periphery-Kontrast, nicht monetäres Risiko.**

**TARGET2:** De Grauwe & Ji zeigen, dass Vertrauensverlust zu Bond-Sell-off und dann
Liquiditätsabfluss führt; TARGET2-Salden sind dessen buchhalterische Gegenseite (Sinn &
Wollmershäuser). GDP wird rekonstruiert als `gov_debt_mio_eur / (gov_debt_pct_gdp/100)`.

### 7.3 Das Lag-Protokoll

Kein Zugang zu Vintage-Daten → revidierte Daten. **Eine Revision trägt Information, die zum
Referenzzeitpunkt nicht existierte — das ist Leakage.**

Gegenmaßnahme: jeder Datenpunkt der Referenzperiode t wird um
```
k = ⌈ lag_in_Tagen / 30 ⌉      volle Monate
```
nach vorne geschoben, auf Monatsende-Index. **Das Target wird nie verschoben.**

**Warum aufrunden:** garantiert, dass kein Wert im Panel steht, bevor er veröffentlicht war.
Abrunden hätte im Erwartungswert weniger Verzerrung, aber in der Hälfte der Fälle echtes
Leakage. Bei einer Anti-Leakage-Maßnahme ist die konservative Richtung die einzig vertretbare.
Preis: bis zu ~29 Tage Überlagerung.

**Split-Lag:** kombiniert ein Input zwei Serien mit verschiedenen Kalendern, wird jede auf
ihrem eigenen Kalender gelagt, *dann* kombiniert. `target2_gdp` = TARGET2(t−2)/GDP(t−4) — der
Zähler bewegt sich in einer Krise am schnellsten und bleibt deshalb auf dem kürzesten
vertretbaren Lag. Die Interaktionsterme: in `global(t) × debt(t−4)` trägt der Schuldenfaktor
seinen 4-Monats-Lag schon, bevor er den zeitgleichen globalen Faktor multipliziert — das
Produkt wird nicht ein zweites Mal gelagt.

**Ausgewiesene Restunsauberkeit:** die Monatsmittel-Serien (Moody's Aaa/Baa, EPU) erscheinen
etwa 5 Tage in den Folgemonat. Auf Monatsfrequenz rundet das auf 0 Monate, ein Vorlauf von
wenigen Tagen bleibt. Als vernachlässigbar eingeschätzt und dokumentiert statt verschwiegen.

**Saisonalität, quantifiziert:** Regression des Fiskalblocks auf Quartalsdummies ergibt
R² = 0.35. Nach dem gleitenden Vierquartalsmittel ≈ 0. Der 4-Monats-Lag bleibt erhalten. Nötig,
weil Eurostat (`gov_10q_ggnfa`) für nicht alle Länder saisonbereinigte Serien liefert; die
Industrieproduktion liegt dagegen als SCA-Serie vor. Eurostat-Quartalsdaten werden bis zum
nächsten Datenpunkt forward-gefüllt.

**Vier erzwungene Substitutionen:** realisierte statt erwarteter Schulden-/Fiskaldaten;
Industrieproduktion als monatlicher GDP-Proxy; NSA-Fiskalreihen mit 4Q-Mittel; Verzicht auf
Bid-Ask-Spreads und US-High-Yield-OAS (ohne Lizenz nur drei Jahre rückwärts verfügbar).

### 7.4 Skalierung

z-Standardisierung pro Land, pro Feature und pro Target, gefittet **nur** auf dem
Trainingsfenster des jeweiligen Refits. Kein Zukunftswissen in der Skalierung. Der einzige
fehlende Input (GR 2015-07) wird um einen Monat fortgeschrieben; Target und Evaluation werden
nie imputiert.

---

## 8. Die Modelle

### 8.1 Random Walk

```
ŝ(t+1) = s(t)    ⟺    Δŝ(t+1) = 0
```

Nenner aller Skill-Verhältnisse. Quantile aus den **empirischen** In-Sample-Residuen, nicht
gaußisch — die Residualquantile werden auf die Punktprognose addiert bzw. subtrahiert.
Deterministisch, ein Seed, nur Regime L.

### 8.2 ARMA(p,q)

```
Δs_t = Σ_{i=1..p} φ_i·Δs_{t−i} + Σ_{j=1..q} θ_j·ε_{t−j} + ε_t,     ε_t ~ WN(0, σ²)
```

ARIMA(p,1,q) auf Levels ist identisch zu ARMA(p,q) auf differenzierten Spreads. Trend
unterdrückt (`trend="n"`), damit **ARMA(0,0) den Random Walk nestet** — das ist ein sauberer
Nesting-Test.

Schätzung: Maximum Likelihood exakt über Kalman-Filter (`statsmodels`). Ordnung (p,q) per AIC
auf dem Gitter {0,1,2,3}² (16 Kandidaten) auf dem Burn-in (bis Monat 114), dann pro Land
eingefroren. **Nicht** auf MAE oder RMSE gefittet. Quantile wie beim Random Walk aus
empirischen Residuen. `arma_monthly` ist dasselbe Modell mit monatlichem Refit — die
Refit-Frequenz-Kontrolle (F10).

### 8.3 LightGBM

Additives Ensemble gradient-geboosteter Entscheidungsbäume:

```
F_M(x) = Σ_{m=1..M} η·f_m(x),      f_m ≈ −∇_F L( y, F_{m−1}(x) )
```

Jeder Baum fittet den negativen Gradienten der Loss — hier der Pinball-Loss. Spezifika von
LightGBM: histogrammbasierte Split-Suche und leaf-wise Wachstum (statt level-wise).

**Ein Booster pro Quantil**, Pinball-Objective bei τ ∈ {0.1, 0.5, 0.9}. Tabellarische Struktur
statt Sequenz: geschachtelte Lag-Mengen aus {1,2,3,6,12,24}, optional Rolling Means über
{3,6,12}, unter der 24-Monats-Obergrenze. In G geht das Land als **native kategoriale
Variable** ein. Early Stopping nach 50 Boosting-Runden.

Eingefrorene HP — L: `num_leaves`=7, `max_depth`=−1, `min_data_in_leaf`=5, lr=0.153, keine
Rolling Means. G: `num_leaves`=15, `max_depth`=3, `min_data_in_leaf`=40, lr=0.083, Rolling
Means an.

Kein GF-Regime: Gradient Boosting hat kein Fine-Tuning-Analogon.

### 8.4 LSTM

```
c_t = f_t·c_{t−1} + i_t·z_t                     Zellzustand (Gedächtnis)
h_t = o_t ⊙ ψ(c_t)                              Hidden State
z_t = φ(w_z' x_t + r_z h_{t−1} + b_z)           Zell-Input (Kandidat)
i_t = σ(w_i' x_t + r_i h_{t−1} + b_i)           Input-Gate
f_t = σ(w_f' x_t + r_f h_{t−1} + b_f)           Forget-Gate
o_t = σ(w_o' x_t + r_o h_{t−1} + b_o)           Output-Gate
```

φ und ψ sind tanh, σ ist die Sigmoidfunktion. Der Zellzustand ist das über die Zeit
persistierende Gedächtnis: das Forget-Gate entscheidet, was behalten wird, Input-Gate plus
Kandidat, was neu geschrieben wird, Output-Gate, was gelesen wird. Der Hidden State fließt im
nächsten Schritt in jedes Gate zurück.

**Quantilkopf:** Hidden State des *letzten* Zeitschritts → LayerNorm → Dropout → Linear auf
drei Quantile. In G wird an jedem Zeitschritt ein Länder-Embedding angehängt.

Eingefroren — L: 1 Layer, hidden 64, Lookback 3. G: 2 Layer, hidden 32, Lookback 3.

**Warum das LSTM die richtige Ablation ist:** LSTM und xLSTM teilen das komplette Protokoll,
die Pipeline, den Quantilkopf, die HPO-Prozedur und die Seeds. Sie unterscheiden sich
**ausschließlich in der rekurrenten Zelle**. Ein Skill-Unterschied kann also nur von der Zelle
kommen.

### 8.5 xLSTM

Adressiert drei Schwächen des LSTM:

| Schwäche | Konsequenz | xLSTM-Lösung |
|---|---|---|
| skalares Gedächtnis | begrenzte Kapazität | mLSTM: Matrixgedächtnis |
| Sigmoid-Gates saturieren | kann nicht schnell überschreiben | exponentielles Gating |
| h_t hängt von h_{t−1} ab | nicht parallelisierbar | mLSTM: Gates nur von x_t |

**Exponentielles Gating.** Ein Gate skaliert einen Fluss. Sigmoid begrenzt auf [0,1], erlaubt
also nur partielles Lernen/Vergessen. Der Fix: `i_t = exp(·)`, ungedeckelt — ein starkes neues
Signal kann das Gedächtnis dominieren. Problem: numerischer Overflow. Lösung: Stabilisator­
zustand

```
m_t  = max( log f_t + m_{t−1},  log i_t )
i'_t = exp( log i_t − m_t )
f'_t = exp( log f_t + m_{t−1} − m_t )
```

Das Paper beweist, dass Output und Gradient nach der Substitution **exakt gleich** bleiben
(App. A.2) — es ist rein numerisch, keine Modelländerung. Weil das Input-Gate nicht mehr auf
[0,1] beschränkt ist, kann der Zellzustand unbeschränkt wachsen und wird durch einen
Normalisiererzustand reskaliert.

**sLSTM** (skalares Gedächtnis, rekurrent, nicht parallelisierbar, mit Memory Mixing):
```
z_t = φ(w_z' x_t + r_z h_{t−1} + b_z)
i_t = exp(w_i' x_t + r_i h_{t−1} + b_i)
f_t = σ(·) oder exp(·)
o_t = σ(w_o' x_t + r_o h_{t−1} + b_o)
c_t = f_t·c_{t−1} + i_t·z_t          n_t = f_t·n_{t−1} + i_t
h_t = o_t ⊙ (c_t / n_t)
```
Memory Mixing: sLSTM-Zellen im selben Head teilen Information über die rekurrenten R-Matrizen.

**mLSTM** (Matrixgedächtnis, vollständig parallelisierbar, **kein** Memory Mixing):
```
q_t = W_q x_t + b_q          k_t = (1/√d)·W_k x_t + b_k          v_t = W_v x_t + b_v
i_t = exp(w_i' x_t + b_i)    f_t = σ(·) oder exp(·)              o_t = σ(W_o x_t + b_o)
C_t = f_t·C_{t−1} + i_t·v_t k_t'                                 n_t = f_t·n_{t−1} + i_t·k_t
h_t = o_t ⊙ ( C_t q_t / max(|n_t' q_t|, 1) )
```
Der neue Inhalt ist das **äußere Produkt** v_t k_t'. Die Gates hängen nur von x_t ab, nicht
von h_{t−1} — daher parallelisierbar. Die Speichermatrix hat feste d×d-Größe und wächst
**nicht** mit der Sequenzlänge, im Gegensatz zum Key-Value-Cache von Attention: Speicherbedarf
konstant, Rechenaufwand linear in der Sequenzlänge.

**Blockstruktur:** jede Zelle in einem Residualblock; sLSTM mit Post-Up-Projection, mLSTM mit
Pre-Up-Projection. Optional kausale Konvolution vor der Gate-/QKV-Berechnung. Residualstack mit
Pre-LayerNorm-Backbone. Notation xLSTM[a:b] = a mLSTM-Blöcke, b sLSTM-Blöcke.

**Was hier tatsächlich deployt ist:** **mLSTM-only** — L: xLSTM[1:0], 1 Block, emb 32,
Lookback 3. G: xLSTM[3:0], 3 Blöcke, emb 64, Lookback 24. *Memory Mixing ist im eingesetzten
Modell also nicht vorhanden.* Wenn jemand nach sLSTM fragt: implementiert, aber von der HPO
nicht gewählt — ein fairer Kritikpunkt.

Architektur: linear Input-Projection → Input-Dropout → xLSTM-Blöcke → Quantilkopf (LayerNorm,
Dropout, Linear auf drei Quantile) auf dem Hidden State des letzten Zeitschritts. In G ein
8-dimensionales Länder-Embedding an jedem Zeitschritt.

**Die Erwartung war**, dass exponentielles Gating bei Spread-Regimewechseln hilft, weil dort
schnelle Anpassung nötig ist. Die Messung stützt das nicht (Folie 13).

### 8.6 TiRex und TiRex-2

Vortrainierte Zeitreihen-Foundation-Models von NX-AI. Beide tokenisieren die Reihe in
**nicht-überlappende Patches**, verarbeiten die Tokensequenz mit einem **xLSTM-Backbone** und
emittieren Quantile über einen Residual-Output-Head.

*Damit endet der Vergleich mit derselben Architektur, mit der er begann* — das ist ein
erzählerisch schöner Punkt: die Architektur, die selbst trainiert versagt, gewinnt
vortrainiert.

**Zero-Shot-Nutzung:** kein Retraining, kein Fine-Tuning, keine HPO. Genau wie von NX-AI auf
Hugging Face released (`NX-AI/TiRex`, `NX-AI/TiRex-2`). Input ist die rohe **Level**-Reihe
(s_1, …, s_{t−1}) als Kontext, monatlich expandierend, one-step-ahead. Aus dem nativen
Dezil-Output werden {0.1, 0.5, 0.9} dynamisch selektiert und dann in Differenzen umgerechnet:
`q_Δ = q_level − s_{t−1}`.

**Zwei Varianten:** `tirex2` univariat (sieht nur die Spread-Historie) und `tirex2_cov` mit den
17 Kovariaten als **past covariates**. TiRex-2 hat auch einen bidirektionalen Pfad für
zukunftsbekannte Kovariaten — nicht genutzt, weil keine vorliegen. `tirex` arbeitet
ausschließlich mit dem Kontext der prognostizierten Reihe.

**Designkonsequenz:** keine In-Sample-Residuen → keine const-width-Zwillinge und kein
GI-Regime. Absicht, nicht Lücke.

**Der Informationsvorteil:** TiRex und TiRex-2 nutzen die **volle expandierende Level-Historie**
und sind damit von der 24-Monats-Obergrenze ausgenommen, die für alle trainierten Modelle gilt.
Selbst nennen.

**Kontaminations-Caveat:** Release 2026-07-01, Pretraining-Cutoff undokumentiert, Testfenster
bis 2026-04.

### 8.7 Die const-width-Zwillinge

Ein Zwilling nimmt **dieselbe Punktprognose** und **dieselben Kovariaten** wie sein
Elternmodell, aber die Intervallbreite kommt aus den In-Sample-Residuenquantilen und ist über
die Zeit **konstant**. Damit ist die Breitenadaptivität sauber isoliert: identischer Median,
identische Inputs, einziger Unterschied ist, ob die Breite über die Zeit variiert.

Existieren nur für die trainierbaren Modelle (lgbm, lstm, xlstm). Für RW, ARMA und die
Zero-Shot-Modelle nicht.

### 8.8 Die Regime

| Code | Bedeutung | Details |
|---|---|---|
| **L** | ein Modell pro Land | bis 277 Monatsbeobachtungen pro Land |
| **G** | ein gepooltes Modell | bis 2 770 Länder-Monate; Länderidentität als gelerntes Embedding (NN) bzw. native kategoriale Variable (LightGBM) |
| **GF** | G, danach pro Land voll feingetunt | ein Zehntel der globalen Lernrate; Epochenzahl einmal auf dem Burn-in aus {5,10,20,40} gewählt |
| **GI** | G plus additiver Intercept-Shift | rollierender Median der letzten ≤36 realisierten OOS-Residuen, Parallelshift auf alle drei Quantile, **Breite unverändert** |
| **ZS** | Zero-Shot | kein Training auf diesem Panel |

**Wichtig für RQ1:** Das Länder-Embedding steckt **bereits in G**. GI ist also *nicht* das
Embedding-Regime, sondern G plus Niveaukorrektur. Damit fragen die Kontraste:

- **GF vs G:** Bringt Gewichts-Fine-Tuning etwas *über* das Embedding hinaus? → ja, wenig (−0.006)
- **GF vs GI:** Von den zwei Wegen, zusätzliche Länderspezifik aufzusetzen — Gewichte anpassen
  oder Niveau verschieben — welcher ist besser? → Gewichte, klar (−0.054)

GI ist eine Guardrail, kein ernsthafter Kandidat: es ist auf beiden Achsen (Gate-Rate 0.602,
Skill −0.037) letzter, was konsistent damit ist.

**Regime-Ebene:** Gate-Rate ZS 0.833 > L 0.668 > G 0.643 > GF 0.631 > GI 0.602. Median-Skill
(zulässige) ZS +0.110 > L +0.031 > G +0.014 > GF +0.010 > GI −0.037. **Beide Achsen ordnen die
Regime identisch:** je mehr künstliche Länderspezifik auf das globale Modell aufgesetzt wird,
desto schlechter werden Kalibrierbarkeit *und* Skill.

---

## 9. Die vierzehn CP-Methoden

Alle reduzieren sich auf **einen additiven Threshold Q_t**, symmetrisch auf beide Ränder
angewandt:
```
C_t = [ q̂_0.1(t) − Q_t ,  q̂_0.9(t) + Q_t ]
```
Die Methoden unterscheiden sich **ausschließlich** darin, wie sie Q_t aus der Score-Historie
bilden. Genau das macht sie vergleichbar. **Ausnahme:** AgACI aggregiert die *Bounds*, nicht
die Thresholds (folgt dem Paper).

| Methode | Klasse | Mechanismus | Fixe Parameter | Gate | Cov. |
|---|---|---|---|---|---|
| `native` | Baseline | keine Rekalibrierung, Q_t = 0. Isoliert den CP-Effekt. | — | 0.132 | 0.872 |
| `cqr_static` | statisch | Split-CQR: Threshold einmal auf den 36 Burn-in-Scores, dann eingefroren. **Einzige Finite-Sample-Garantie** (unter Exchangeability). | — | 0.057 | 0.859 |
| `cqr_rolling` | gefenstert | Split-CQR jeden Monat neu auf dem rollierenden Pool. Spezialfall von weighted CP mit binären Gewichten. | Fenster 36 | 0.906 | 0.823 |
| `mondrian_cqr` | gefenstert | Kategorie-bedingte Kalibrierung auf **Terzilen der modelleigenen Rohbreite** (als Volatilitätsproxy). Fällt auf den globalen Pool zurück, wenn ein Terzil < 5 Scores oder der Pool < 15 hat. | 3 Kategorien | 0.604 | 0.860 |
| `aci_cqr` | ACI | Regelt das **effektive α_t** statt des Thresholds: `α_{t+1} = α_t + γ(α − err_t)`. Nach Verletzung sinkt α_t → Intervall weitet; nach Treffer steigt α_t → Intervall verengt. | γ = 0.005 | 0.962 | 0.815 |
| `agaci_cqr` | ACI | Online-Aggregation über 11 ACI-Experten mit unterschiedlichem γ, via BOA-Algorithmus mit adaptiven Lernraten. Aggregiert Bounds, untere und obere getrennt. Löst die γ-Sensitivität algorithmisch. | K = 11 | 0.981 | 0.811 |
| `dtaci_cqr` | ACI | Exponentielle Gewichte über dieselben Experten, Pinball-Loss direkt auf den **Thresholds**. Gewichtsregularisierung verhindert, dass ein Experte permanent dominiert → bleibt nach Regimewechseln adaptiv. | σ = 1/8, η = 2.72 | 1.000 | 0.822 |
| `spci_cqr` | Score-Prognose | Dreht die Logik um: **prognostiziert** das bedingte (1−α)-Quantil des *nächsten* Scores aus den letzten 12 Scores, mit einem Quantile Random Forest (Leaf-Weights, 100 Bäume, min. Blattgröße 2). Nutzt damit gezielt die zeitliche Abhängigkeit, die Exchangeability ausschließt. Fallback auf empirisches Poolquantil bei < 22 Scores. | w = 12 | 1.000 | 0.769 |
| `pid_cqr` | Online-Regler | PI-Regler auf dem Coverage-Fehler. P-Term = Quantile-Tracking-Schritt; I-Term = saturierender Tangens-Integrator gegen anhaltende Fehldeckung. **Unser `pid` ist der PI-Regler des Papers, ohne D-Term.** | η = 0.1·S̄, C_sat = 5 | 1.000 | 0.813 |
| `pid_local_cqr` | Online-Regler | Wie pid, aber der globale Integrator wird durch ein rollierendes 12-Monats-Fenster ersetzt — alte Fehler werden schneller vergessen, robuster bei Regimewechsel. | Fenster 12 | 1.000 | 0.814 |
| `pid_tan_cqr` | Online-Regler | Fügt den D-Term über einen autoregressiven Scorecaster hinzu. | wie pid + AR | 0.283 | 0.824 |
| `sfogd_cqr` | Online-OCO | Scale-free Online Gradient Descent auf dem Pinball-Subgradienten. | Basisrate S̄ | 1.000 | 0.806 |
| `saocp_cqr` | Online-OCO | Strongly Adaptive OCP: minimiert Regret über **jedes zusammenhängende Zeitintervall**, nicht nur global. Erzeugt je Update-Schritt einen neuen SF-OGD-Experten mit geometrischer Lebensdauer g·2^ν₂(t); Meta-Aggregation per CBCE-Coin-Betting über einen Sleeping-Experts-Prior. | g = 8 | 0.019 | 0.917 |
| `decay_ocp_cqr` | Online-OCO | Online CP mit abnehmender Schrittweite. | η₀, ε aus Paper | 0.208 | 0.873 |

**Pool-sensitiv sind genau sechs:** `cqr_rolling`, `aci`, `agaci`, `dtaci`, `spci`, `mondrian`.
Die anderen acht sind per Konstruktion invariant gegen die Pooldefinition — empirisch
verifiziert bei maximaler Coverage-Differenz **exakt 0** über 530 Zellen je Methode
(bit-identisch, nicht nur „klein").

**Zwei Implementierungsdetails, die Ergebnisse erklären:**

1. **`saocp` kann nur verbreitern.** Sowohl das Paper als auch der Referenzcode
   (`salesforce/online_conformal`) erzwingen nichtnegative Radien, geklemmt bei null. Auf
   signierten CQR-Scores heißt das: die Methode kann das Rohintervall nie verengen. **Die
   konservative Coverage von 0.917 ist damit eine Eigenschaft ihrer Domänendefinition und kein
   Beleg, dass die Methode besser wäre.** Erklärt auch ihre Kreuzungsrate von exakt 0 %.
   Zusatzdetail: Experten werden pro Update-Schritt und nicht pro Kalendermonat erzeugt, damit
   Griechenlands Lückenmonate keine Phantom-Experten anlegen.
2. **`cqr_rolling` ist die einzige der sechs Pool-Methoden, die ihr Quantil jeden Monat
   komplett neu aus dem aktuellen Pool berechnet**, statt einen Zustand online fortzuschreiben.
   Deshalb sind alle drei extremen Pool-Sensitivitäten (F8) `cqr_rolling`-Fälle:
   lstm/GI 8.9 pp, arma_monthly/L 8.5 pp, lstm/GF 8.3 pp.

**Null getunte Parameter.** Jede Schrittweite, jedes Fenster, jeder Gain ist ex ante aus der
Originalarbeit übernommen. Nichts ist auf diesen Daten gefittet. Das ist das stärkste Argument
gegen den Vorwurf, die CP-Schicht sei überfittet.

---

## 10. CQR-Score, Pool, Skalierung

### 10.1 Der Score

```
E_t = max{ q̂_0.1(t) − y_t ,  y_t − q̂_0.9(t) }
```

Auf der **Level**-Skala, **nach** der monotonen Rearrangierung der Rohquantile.

- `E_t > 0`: die Realisierung lag außerhalb des Rohintervalls
- `E_t < 0`: das Rohintervall war zu breit
- Der Score fasst beide Ränder in **eine** Zahl zusammen. Das ist für SPCI und SAOCP relevant.

**Warum Level und nicht Delta:** die Evaluation lebt im Level-Raum. Kalibriert man auf einer
anderen Skala als der bewerteten, kalibriert man die falsche Größe.

**Warum symmetrisch aufgeschlagen:** dieselbe Zahl Q_t unten abgezogen, oben addiert. Das ist
eine **Designentscheidung, keine Notwendigkeit** — und genau die strukturelle Grenze, die die
Tail-Asymmetrie auf Folie 14 sichtbar macht.

### 10.2 Monotone Rearrangierung

Die drei Quantile werden unabhängig geschätzt und können kreuzen. Gekreuzte Quantile würden der
CP-Schicht undefinierte Intervallgrenzen übergeben. Deshalb werden die drei Werte pro Prognose
sortiert — eine monotone Rearrangierung (Chernozhukov et al. 2010), die
q̂_0.1 ≤ q̂_0.5 ≤ q̂_0.9 garantiert. Wie oft die Rohquantile vor dem Sortieren gekreuzt haben,
wird als Diagnostik pro Modell geloggt. Für jedes Modell identisch.

### 10.3 Kalibrierungseinheit

Ein „Strom" = (Modell, Regime, Seed, Land). Kalibriert wird **pro Strom**, Scores werden
**nie** über Länder gepoolt — auch nicht für die global trainierten Modelle. Begründung: ein
gepoolter Threshold wäre für die Kernländer zu breit und für die Peripherie zu eng, weil sich
Niveaus und Volatilitäten um Größenordnungen unterscheiden.

### 10.4 Das Quantil auf endlichem Pool

Statt des empirischen Quantils die Ordnungsstatistik-Konvention (Vovk et al. 2005):

```
Q(S, a) = S_(r),      r = min{ max(1, ⌈(n+1)(1−a)⌉),  n }
```

Die Klemmung auf [1, n] verhindert unendliche oder leere Intervalle: bei sehr kleinen Pools
oder extremem α_t gäbe die Formel sonst einen Rang > n, formal ein unendliches Intervall —
stattdessen wird das Poolmaximum genommen. **Das ist eine Finite-Pool-Approximation und mild
antikonservativ.** Genau so benennen. Relevant besonders für ACI: während einer langen
Verletzungsserie kann α_t unter 1/(n+1) fallen, und genau dann greift die Klemmung.

### 10.5 Der Skalenfaktor S̄

```
S̄ = max over burn-in months |E_t|          pro Strom
```

Alle freien Schrittweiten der Online-Verfahren sind Vielfache von S̄ — die Skalenadaption des
PID-Papers. Ein Gain in Basispunkten wäre für Österreich und Griechenland nicht dasselbe.

**Entscheidend fürs Leakage-Argument:** S̄ wird **nur** aus Burn-in-Monaten gebildet, also aus
Daten vor dem ersten Testmonat, und danach nie mehr verändert.

### 10.6 Pool und Reihenfolge im Loop

```
P_t = { E_s : s < t, E_s realisiert }_{letzte w}          Primärfall w = 36
```

**Die Reihenfolge ist das Leakage-Argument:** in jedem Monat wird **erst emittiert** (Q_t hängt
nur von Scores aus Monaten < t ab), **dann** mit dem realisierten Score E_t aktualisiert. Für
alle 14 Implementierungen im Review-Pass 2026-07-18 verifiziert.

**Burn-in:** Monate 115–150 (36 Monate) füllen den Pool bzw. wärmen die Online-Tracker auf und
werden **nie** evaluiert. Online-Verfahren starten bei Q = 0 und laufen durch den Burn-in mit
Updates ab Monat 115. Damit betritt **jede** Methode Monat 151 mit demselben Informationsset,
egal ob sie einen Pool liest oder einen Zustand fortschreibt. Emission ausschließlich auf den
Testmonaten 151–277.

**Warum genau 36:** das ist die Länge des Burn-in, sodass der Pool am Teststart exakt voll
besetzt ist.

**Pool-Varianten:** expanding, rolling 24, rolling 48 (24 und 48 ex ante fixiert). Das
48er-Fenster hält am Teststart erst 36 Scores und ist zwölf Monate später voll besetzt. Die
Burn-in-Länge selbst wird nicht mitvariiert.

| Pool | n Zellen | Mittlere Coverage | Median-Winkler-Skill | mittl. \|Δcov\| vs rolling(36) | max |
|---|---|---|---|---|---|
| rolling(36) primär | 3 180 | 0.817 | +0.007 | — | — |
| expanding | 3 180 | 0.838 | +0.005 | 4.0 pp | 18.9 pp |
| rolling(24) | 3 180 | 0.813 | +0.000 | 2.1 pp | 10.2 pp |
| rolling(48) | 3 180 | 0.820 | +0.014 | 1.6 pp | 8.7 pp |

*Nur die sechs pool-lesenden Methoden.* Die acht invarianten haben mittlere Coverage 0.847 und
Median-Skill −0.024, identisch über alle vier Pools.

**Lesart:** die mittlere Coverage steigt mit längerem Pool (längere Pools erben alte, breitere
Quantile), der Skill folgt dieser Reihenfolge **nicht**. Ein längerer Pool kauft also mehr
Coverage, nicht besseren Skill.

**Lückenmonate:** Griechenland 2015-07/08 liefern keinen Score. Online-Methoden überspringen
das Update, Pool-Methoden sehen die Monate nie. Nie imputiert. Beide liegen im Burn-in.

---

## 11. Regimedefinitionen

### 11.1 Realisierte Volatilität (Terzile)

Rollierende 12-Monats-Standardabweichung der **realisierten** Δ-Spreads, um einen Monat
verzögert (`shift(1)`, also nur Vergangenheitsinformation), **pro Land** in Terzile
geschnitten. Die ersten 12 Testmonate haben kein Fenster und gehen ins neutrale mittlere
Terzil (Konvention, Amendment 2026-07-10). Weil pro Land geschnitten wird, unterscheiden sich
die Monatszahlen leicht zwischen Ländern.

### 11.2 VIX-Schwelle

VIX > 20, global, ex ante fixiert. Deckt 36 der 127 Testmonate ab (28.3 %). `vix_log` liegt roh
als log(VIX) im Panel und ist über alle Länder identisch; die Regime-Zuordnung ist eine reine
Ex-post-Stratifizierung und **kein Modellinput**. VIX > 25 wird nur nachrichtlich mitgeführt.

Zusätzliche Konvention: eine Land-×-Regime-Zelle muss mindestens 6 Monate haben, damit ihr
Skill in die Aggregation eingeht — dünn besetzte Zellen erzeugen instabile Mittelwerte.

### 11.3 Die Orthogonalität — der methodische Kernpunkt

| Größe | Wert |
|---|---|
| High-VIX-Monate | 36 |
| Monate mit ≥ Hälfte der Länder im oberen Terzil | 38 |
| **Überlappung** | **12** |
| corr(VIX-Niveau, Anteil gestresster Länder), gleichzeitig | 0.009 |
| verzögert 1 / 2 / 3 Monate | 0.112 / **0.170** / 0.145 |

Die Terzile sind rückwärtsgerichtet und länderspezifisch, der VIX gleichzeitig und global. Sie
markieren damit **verschiedene Phasen, nicht verschiedene Intensitäten**: VIX > 20 den
*Eintritt* der Belastung, das obere Terzil ihr *Andauern*. Die stärkste Korrelation bei zwei
Monaten Versatz stützt genau das.

**Konsequenz:** die Online-Regler hinken beim Eintritt hinterher (Coverage 0.759) und
überschießen im Andauern (0.841).

### 11.4 Der weggelassene Kalenderschnitt

QE bis 2021-12 gegen Hiking ab 2022-01 ist in `pipeline.yaml` vorregistriert, wird aber
**nicht berichtet** — und das braucht eine Begründung, weil stillschweigendes Weglassen genau
der Fehler wäre, gegen den die Arbeit anschreibt:

1. Ein einzelnes hart kodiertes Datum, das keine EZB-Entscheidungen abbildet (erste Erhöhung
   erst 07/2022, Halten ab 10/2023, Senkungen ab 06/2024).
2. Zu 90 % kollinear mit der Volatilitätsachse: **89.5 %** aller gestressten Länder-Monate
   liegen vor 2022-01; nach 2022-01 sind 51.2 % der Länder-Monate im ruhigen und nur 7.7 % im
   gestressten Terzil.

Er misst also dieselbe Trennung unter falschem Namen.

### 11.5 Die Komplexitätshypothese (F13)

**Vorregistrierte Gruppen:** komplex = xlstm, lstm, lgbm, tirex, tirex2, tirex2_cov;
klassisch = rw, arma, arma_monthly. **Die drei const-Zwillinge stehen in KEINER Gruppe** und
gehen nicht in die Kennzahl ein — wichtig, sonst liest man das `is_complex=False`-Flag als
„klassisch".

**Vorregistriertes Ergebnis:** Vorsprung komplex minus klassisch schrumpft von +0.1026
(low VIX) auf +0.0251 (high VIX), Δ = −0.0775. Hypothese widerlegt.

**Warum der Gruppenmedian problematisch ist:** die komplexe Gruppe ist bimodal. Ihre sechs
Werte streuen im low-VIX-Regime von −0.0271 (xlstm) bis +0.1255 (tirex2), mit einer **Lücke von
0.055** zwischen lgbm (+0.0640) und tirex (+0.1191). Der Gruppenmedian +0.0916 fällt genau in
diese Lücke — er beschreibt kein Modell, sondern die Lage der Gruppengrenze.

**Zerlegung** (post hoc, reine Umgruppierung derselben zwölf Werte, kein neuer Test):

| Kontrast | low VIX | high VIX | Δ |
|---|---|---|---|
| nur Zero-Shot vs klassisch | +0.1315 | +0.0254 | −0.1061 |
| nur trainiert vs klassisch | +0.0143 | −0.0211 | −0.0354 |

**Das gesamte F13-Ergebnis trägt der Zero-Shot-Arm.** Die trainierten komplexen Modelle hatten
nie einen nennenswerten Vorsprung (+0.0143 in ruhigen Märkten) und liegen bei hohem VIX unter
den klassischen. Formulierung: *„Komplexität" im Sinne von auf diesem Panel trainierten tiefen
Modellen hat in KEINEM Regime geholfen. Was existierte und dann verschwand, war ein
Pretraining-Vorteil.* Die F13-Gruppe wirft beide Behauptungen zusammen.

**Auf der Volatilitätsachse kehrt sich das Bild um:** `tirex2` ist das einzige Modell, dessen
Median-Skill im oberen Terzil *steigt* (calm 0.1357 → stressed 0.1502), während die anderen
einbrechen (lgbm_const −0.1719, xlstm_const −0.1621, rw −0.1563, arma_monthly −0.1470).

### 11.6 Coverage-Reaktion im Stress — zwei Fallen

Die Spalte heißt in den Artefakten `stress_drop`, ist aber `cov_stressed − cov_all`, also
**positiv bei STEIGENDER Coverage**. Im Text nie „drop" schreiben. Und es ist eine
**Coverage**-Größe, kein Skill — nicht mit den Skill-Spalten vermengen.

Mittelwert +0.0283, Median +0.0290. Je Familie von xlstm +0.0158 und xlstm_const +0.0212 bis
tirex2_cov +0.0571 und tirex2 +0.0637.

Kombinationen, deren Coverage im Stress **fällt**: lstm/L/decay_ocp −0.0386,
xlstm/GF/cqr_rolling −0.0365, xlstm/GF/aci −0.0360, xlstm/GI/native −0.0307.

**Die Aufweitung der Zero-Shot-Modelle ist kein Defekt:** über die 485 zulässigen Kombinationen
korreliert die Coverage-Reaktion positiv mit dem Skill im gestressten Terzil (Spearman +0.459,
p < 0.0001). Aber: die Korrelation ist in *allen* Regimen positiv (+0.317 bis +0.402), also
kein stressspezifischer Mechanismus — stärker aufweitende Kombinationen sind generell die
besseren. So einschränken.

**Die echte Fragilität sitzt bei xLSTM und LSTM:** sie weiten am wenigsten aus (xlstm +0.0045
beim besten Paket) und stehen im gestressten Terzil auch beim Skill hinten (xlstm +0.0164).
Coverage und Skill zeigen dort in dieselbe Richtung.

### 11.7 Zwei Skill-Definitionen — nicht mischen

| | Methoden | Gate | Aggregation |
|---|---|---|---|
| **Regime-Tabelle** (Folie 18, Anhang E) | alle 14 inkl. `native` | nur die 485 zulässigen | Median über Kombis des Länder-Medians |
| **Vorregistriertes F13** | `native` ausgeschlossen | **kein** Gate | Median über alle Zellen |

Deshalb steht für `tirex2` bei hohem VIX einmal **+0.0237** (Familienmedian der Regime-Tabelle)
und einmal **−0.003** (F13). Beide Definitionen dort benennen, wo sie vorkommen.

---

## 12. Der ökonomische Block

**Grundsatz, der immer mitgesagt werden muss:** ergänzend, **nie** Rangkriterium. Coverage und
Winkler entscheiden.

### 12.1 Kapitalproxy

```
cap_ratio = width / max(width über alle zulässigen Zellen desselben Landes)
```
Median über die Länder. Niedriger ist besser. Nur über zulässige Zellen gerechnet.

| Paket | cap_ratio |
|---|---|
| tirex2/ZS/cqr_static | 0.551 |
| tirex2/ZS/decay_ocp_cqr | 0.573 |
| lgbm/G/decay_ocp_cqr | 0.598 |
| tirex/ZS/native | 0.619 |
| rw/L/aci_cqr | 0.635 |
| Median über die 485 zulässigen | 0.669 |
| schlechteste zulässige | 0.878 |

**Was das ist und was nicht:** eine dokumentierte Vereinfachung der FRTB-Logik — Breite
proportional zur Kapitalunterlegung. **Keine** Umsetzung der Regeltexte. FRTB verlangt
tatsächlich ein Expected-Shortfall-Maß auf verschiedenen Liquiditätshorizonten mit
Stresskalibrierung plus P&L-Attribution-Tests; der Proxy erfasst davon nur die
Breitendimension.

### 12.2 Handelsregel

Aktiv, wenn der letzte realisierte Level **außerhalb** des kalibrierten Intervalls liegt (also
null nicht im Δ-Intervall). Position = Vorzeichen(Prognoserichtung) / Intervallbreite,
annualisierter Sharpe über die Monats-P&L. Stilisierte Mean-Reversion-Regel.

**Warum sie nicht belastbar ist:** `active_share` liegt im Median bei etwa **2 %** — bei den
Spitzenpaketen 0.019 bzw. 0.009, also ein bis zwei aktive Monate von 127. Median-Sharpe über
die zulässigen Kombinationen 0.159, rund zwei Drittel positiv, Maximum 0.621
(lstm/L/pid_cqr bei active_share 0.020). Keine Transaktionskosten, keine Bid-Ask-Spannen, keine
Finanzierungskosten, keine Kapazitätsgrenzen. **Ausschließlich komparativ zu lesen, nie als
erzielbare Rendite.**

### 12.3 Der eigentliche Befund

```
corr( Median-Skill, Kapitalquotient ) = −0.711
corr( Median-Skill, Median-Sharpe   ) = −0.147
```

Ein **niedriger** Kapitalquotient ist gut, deshalb ist die negative Korrelation der
**erwünschte** Zusammenhang. Das Vorzeichen muss man dazusagen, sonst liest man es
falschherum.

**Aussage:** Intervallgüte zahlt auf die Kapitalkennzahl ein, aber kaum auf die
Handelsperformance. Der Nutzen besserer Intervalle liegt im Risikomanagement, nicht im Alpha.
Angesichts der Punktprognoseergebnisse ist das das erwartete und nicht ein überraschendes
Resultat. Genau das rechtfertigt, dass der Ökonomieblock überhaupt existiert.

---

## 13. Glossar: was jede Spalte bedeutet

| Spalte | Bedeutung | Lesart |
|---|---|---|
| `coverage` | empirischer Anteil der Monate mit Realisierung im Intervall | Ziel 0.80 |
| `admissible share` / Gate-Rate | Anteil der Kombinationen einer Methode, die das Gate passieren | 1.000 = alle |
| `DQ rejection` | Anteil der Zellen, in denen der DQ-Test bei q < 0.10 verwirft | hoch = bedingt fehlkalibriert |
| `skill` | Median-Winkler-Skill vs. `rw/L/native` | **> 0 = besser als RW** |
| `median_skill` | dasselbe, Spaltenname in den Artefakten | |
| `skill_iqr` | Interquartilsabstand des Skills über die Länder | Maß der Länderstreuung |
| `n_countries` | Zahl der Länder, die in den Regime-Skill eingehen | Zellen < 6 Monate fallen raus |
| `MAE ratio` | mittlerer absoluter Fehler / den des RW | **< 1 = besser als RW** |
| `RMSE ratio` | analog auf quadrierten Fehlern | |
| `directional accuracy` | Anteil richtig prognostizierter Vorzeichen | 0.5 = Zufall |
| `PT rejections` | Anteil der Zellen mit PT-Ablehnung auf 5 % | Zufall gibt 0.05 |
| `d_norm` / `d` | normalisiertes DM-Verlustdifferenzial A−B, Median über Länder | **negativ = A besser** |
| `share_a_better` / `Share A` | Anteil Länder, in denen A besser ist | 0.5 = kein Muster |
| `t_panel` / `Panel t` | Querschnittsmittel je Monat, dann Newey-West-t, 3 Lags | \|t\| > 1.96 ≈ 5 % |
| `median_q` / `Med. q` | Median der BH-korrigierten p-Werte der Ländertests | < 0.10 = signifikant |
| `share_sig_fdr` | Anteil Länderzellen mit q < 0.10 | |
| `seed_stable` / `Seed-st.` | Anteil Kontraste mit gleichem Vorzeichen über Seeds 42/43/44 | vorregistrierte Bedingung für Rangaussagen |
| `seed_span` | max − min des Median-Skills über die drei Seeds | > \|skill\| = fragil |
| `width` | mittlere Intervallbreite | in Spread-Einheiten |
| `under_pen` / `over_pen` | mittlere Unter-/Überschreitungsstrafe | mit Faktor 2/α = 10 |
| `tail_asym` | Anteil unterer an allen Verletzungen | 0.5 = symmetrisch |
| `crossed_cal_rate` | Anteil Prognosen mit gekreuzten kalibrierten Grenzen | |
| `cov_first36` / `cov_rest` | Coverage in den ersten 36 Testmonaten vs. Rest | Burn-in-Diagnostik |
| `stress_drop` | `cov_stressed − cov_all` — **trotz Namens positiv bei steigender Coverage** | |
| `mcs_in_set` | 1, wenn das Paket in der MCS des Landes ist | |
| `mcs_pval` | terminaler p-Wert, je Land geteilt von allen Überlebenden | |
| `median_cap_ratio` | Breite / breiteste zulässige Breite desselben Landes | **niedriger = besser** |
| `active_share` | Anteil Monate, in denen die Handelsregel aktiv ist | hier ~2 % → Sharpe bedeutungslos |
| `median_sharpe` | annualisierter Sharpe der Handelsregel, Median über Länder | nur komparativ |
| `validity_flag` | passiert das Gate (≥ 70 % der Länder) | |
| `valid_country_share` | Anteil der Länder, in denen die Zelle valide ist | |
| `pool_mode` | rolling / expanding / rolling24 / rolling48 | **immer darauf konditionieren**, sonst Doppelzählung |

---

## 14. Q&A-Drill: 30 harte Fragen

### Zur Aufgabenstellung

**1. Warum prognostizieren Sie Änderungen und nicht Niveaus?**
Weil der Spread-Level extrem persistent ist. Ein Levelmodell bekommt ein glänzendes R², das
nur die Persistenz abbildet. Die Differenzierung entfernt sie. Bewertet wird trotzdem im
Level-Raum, über die Rücktransformation ŝ(t) = s(t−1) + Δŝ(t) — die berichtete Genauigkeit ist
also nicht durch die Differenzierung geschönt.

**2. Warum monatlich? Spreads bewegen sich täglich.**
Weil zwölf der 17 Kovariaten monatlich oder quartalsweise publiziert werden. Bei täglicher
Frequenz wären sie über Wochen konstant. Das ist eine andere Frage, nicht eine schlechtere —
für eine tägliche Studie müsste man den Feature-Satz anders bauen.

**3. Warum 80 % und nicht 99 %, wie im Marktrisiko üblich?**
Zwei Gründe. Erstens Vergleichbarkeit: die vortrainierten Modelle geben nur Dezile aus, 0.1/0.9
ist das breiteste Paar ohne Extrapolation. Zweitens Testpower: bei α = 0.01 und 127 Monaten
erwartet man 1.27 Verletzungen pro Zelle, damit hat kein Kalibrierungstest Aussagekraft. Bei
α = 0.20 sind es 25.4. Die CP-Schicht ist auf jedes α anwendbar, die Power fällt aber.

### Zu den Ergebnissen

**4. Warum ist ein Modell, das nichts über Staatsanleihen weiß, besser als eines, das Sie
selbst trainiert haben?**
Weil es nichts schätzen muss. Es gibt keine Parameterschätzunsicherheit, weil keine Parameter
geschätzt werden; die prädiktive Verteilung kommt aus einem Pretraining auf Millionen
Zeitreihen. Und der Vorsprung sitzt genau dort, wo man es erwarten würde: in der
Verteilungsform, nicht im Zentrum — die Punktprognose liegt exakt auf Random-Walk-Niveau. Dann
sofort selbst die Kontaminationsfrage aufwerfen.

**5. Wie sicher ist es, dass TiRex-2 nicht einfach Ihre Testdaten gesehen hat?**
Nicht sicher. Release 2026-07-01, Cutoff undokumentiert, Testfenster bis 2026-04. Drei
Argumente sprechen dagegen: die Punktprognose ist nicht besser (Panel-t −0.498, bei
nachgewiesener Power); der Vorsprung liegt in der Breite, während Memorierung das Zentrum
treffen würde; und der Vorsprung verschwindet bei hohem VIX (0.1255 → −0.0025), während ein
memorierendes Modell gerade dort glänzen müsste. Keines ist ein Beweis. Der saubere Test wäre
ein Fenster nach dem Release.

**6. Können wir das einsetzen?**
Nicht wie es steht, und aus einem Grund, der nichts mit dem Modell zu tun hat: das Spitzenpaket
hält bei VIX über 20 nicht — Platz 8, Skill 0.019. Was man einsetzen könnte, ist die
CP-Schicht auf ein Modell, das die Bank schon hat. Das ist der Befund mit dem besten
Aufwand-Nutzen-Verhältnis: rohe Quantile passieren das Gate in 13.2 % der Fälle, mit adaptiver
Rekalibrierung in 100 %.

**7. Die 17 Kovariaten bringen wirklich nichts?**
Der ceteris-paribus-Kontrast ist praktisch null: median Δ-Skill +0.0036, Panel-t −0.75, und von
140 Ländertests erreichen 18 ein p < 0.10 bei 14 erwarteten, aufgeteilt zehn zu acht nach
Richtung. Nach BH überlebt keiner. **Aber:** das heißt nicht, dass Fundamentaldaten irrelevant
sind. Die Literatur erklärt Niveaus im Querschnitt, ich prognostiziere Änderungen über einen
Monat. Und vor der Kalibrierung tun die Kovariaten etwas — sie verengen das Intervall in allen
zehn Ländern um 2.2 bis 6.9 % und schieben die Coverage von 0.843 auf 0.831, also in Richtung
des Nominalwerts.

**8. Warum hat xLSTM nicht funktioniert? Implementierungsfehler?**
Kann ich nicht ausschließen, aber die Evidenz zeigt anders. Das LSTM läuft mit identischem
Protokoll und identischer Pipeline besser, und die beiden unterscheiden sich ausschließlich in
der rekurrenten Zelle. Ein Codefehler wäre systematisch; das Versagen ist seed-abhängig — in
50.6 % der xLSTM-Kombinationen übersteigt die Seed-Spanne den Effekt. Meine Hypothese ist
Kapazität gegen Stichprobengröße: 277 Monate mal zehn Länder.

**9. Sie haben 742 Kombinationen getestet — wie viel davon ist Data Mining?**
Alles läuft durch Benjamini-Hochberg, getrennt je Kontrastfamilie, und der Katalog der 13
Findings-Typen stand vorher fest. Der beste Beleg dafür: einer der 13 ist **leer**. F12
verlangt ein MAE-Verhältnis unter 0.98 bei negativem Skill, und das beste im Feld ist 0.9965 —
unerfüllbar. Ein leerer vorregistrierter Typ beweist, dass der Katalog nicht nachträglich
passend gemacht wurde.

**10. Wieso steigt die Coverage im Stress, aber die Qualität sinkt?**
Weil die Regler nach Volatilitätsanstiegen aufweiten und Stressphasen persistent sind — sie
überschießen. Coverage steigt auf 0.841, der Winkler bezahlt es mit Breite. Und die Kehrseite:
in ruhigen Phasen sind die Intervalle mit 0.7795 zu schmal. Coverage und Skill laufen
gegenläufig, und genau deshalb ist Coverage bei mir ein Gate und der Winkler das Rangkriterium.

**11. Was wäre nötig, damit man dem Ranking statistisch glauben kann?**
Mehr Zeit oder ein breiterer, weniger korrelierter Querschnitt. Zehn Euro-Länder sind nicht
zehn unabhängige Evidenzstücke; deshalb berichte ich den Panel-t mit Zeit-Clustern *und* die
per-Land-q-Werte, die verschiedene Fragen beantworten. Bei 127 Monaten und dieser
Querschnittskorrelation ist die Rangfolge deskriptiv, und ich sage das so.

### Zur Methodik

**12. Warum ist der Winkler-Score das richtige Maß und nicht einfach Coverage plus Breite?**
Weil eine willkürliche Gewichtung von Coverage und Breite kein proper scoring rule wäre — man
könnte sie durch Falschangabe schlagen. Der Winkler-Score ist strictly proper: sein Minimum
liegt eindeutig bei den wahren Quantilen. Man kann ihn nicht durch systematisch zu breite oder
zu schmale Intervalle verbessern.

**13. Was genau bedeutet „proper scoring rule"?**
Der erwartete Score unter der wahren Verteilung wird minimiert, indem man die wahre Verteilung
angibt. Strictly proper heißt, das Minimum ist eindeutig. Praktisch: es gibt keine Strategie,
den Score durch Abweichen von der eigenen ehrlichen Überzeugung zu verbessern.

**14. Warum Benjamini-Hochberg und nicht Bonferroni?**
Bonferroni kontrolliert die Wahrscheinlichkeit *irgendeines* Fehlers und wäre bei 7 420 Zellen
so konservativ, dass praktisch nichts verworfen wird — hier hieße das, jede Kombination gilt
als kalibriert, auch die kaputten. BH kontrolliert den erwarteten *Anteil* falscher
Entdeckungen und ist bei einer Screening-Aufgabe das passende Instrument.

**15. Was kontrolliert die FDR-Korrektur nicht?**
Die Fehlerwahrscheinlichkeit einer einzelnen Aussage. Ein q-Wert von 0.09 heißt nicht „diese
Zelle ist mit 91 % Wahrscheinlichkeit fehlkalibriert". Er ist eine Eigenschaft der
Entscheidungsregel auf der ganzen Familie. Und FDR kontrolliert die Rate je Familie, nicht
einzelne Fehlschlüsse über Familien hinweg.

**16. Warum getrennte FDR-Familien? Ist das nicht bequem?**
Es ist eine Entscheidung mit einem echten Manipulationsrisiko, und deshalb vorregistriert. Die
Begründung: FDR kontrolliert den Fehlanteil innerhalb einer Familie. Wirft man alles zusammen,
verschlechtert ein zusätzlicher Robustheitstest die Signifikanz der Hauptfrage, was inhaltlich
unsinnig ist. Bei den Kovariaten weise ich beide Rechnungen aus: in der eigenen Familie
überlebt keiner der 140 Tests, eingebettet in die große Familie überleben acht.

**17. Warum ist die Nicht-Verwerfung der Kalibrierungstests kein Beleg für Kalibrierung?**
Weil ein Test nur sagt, ob die Daten der Nullhypothese widersprechen. Bei 127 Monaten und 25
erwarteten Verletzungen ist die Power moderat. Das Gate ist deshalb ein Ausschlusskriterium,
kein Gütesiegel. Ich formuliere durchgängig „ich kann die Kalibrierungshypothese nicht
verwerfen", nicht „die Kombination ist kalibriert".

**18. Der DQ-Test verwirft die Hälfte Ihrer Zellen. Ist Ihr Framework dann nicht kaputt?**
Nein, aber es ist eine echte Einschränkung, und ich lokalisiere sie. Nimmt man den
Breitenregressor heraus, fällt die Ablehnrate von 49.3 auf 20.8 %, also auf UC-Niveau. Es ist
ein Breiten- und kein Clustering-Phänomen — was die Christoffersen-Unabhängigkeitsrate von
6.1 % von der anderen Seite bestätigt. Und die Richtung ist erklärbar: 99.9 % der
signifikanten Breitenkoeffizienten sind negativ, die Breitensteuerung überschießt in beide
Richtungen und marginal mittelt sich das heraus. Zulässigkeit belegt marginale, nicht bedingte
Validität — so steht es in den Limitations.

**19. Warum benutzen Sie den DQ-Test dann nicht im Gate?**
Weil er 49.3 % verwirft und die zulässige Menge damit leeren würde. Das ist eine ehrliche
Kosten-Nutzen-Entscheidung, keine Bequemlichkeit: mit DQ im Gate hätte ich nichts zu ranken.
Ich berichte ihn stattdessen und zerlege ihn.

**20. Was ist Exchangeability und warum ist sie bei Ihnen verletzt?**
Austauschbarkeit heißt, die gemeinsame Verteilung der Nonconformity-Scores ist invariant unter
Permutation. Zeitreihen mit Volatilitätsclustern erfüllen das nicht: ein Score aus einem
Stressmonat ist nicht austauschbar mit einem aus einem ruhigen. Deshalb hat nur `cqr_static`
eine Finite-Sample-Garantie, und die gilt unter einer Annahme, die hier verletzt ist. Die
Online-Verfahren geben die Garantie auf und tauschen sie gegen Langfrist-Coverage — das ist der
methodische Kern der Arbeit.

**21. Garantiert Conformal Prediction nicht 80 % Coverage?**
Nur unter Exchangeability, und nur für die statische Variante. Die Online-Verfahren garantieren
Langfrist-Coverage, also asymptotisch, ohne Aussage über ein endliches Fenster. Und meine
Quantilkonvention auf endlichem Pool ist mild antikonservativ, weil der Rang bei n geklemmt
wird. Das weise ich aus.

**22. Wie stellen Sie sicher, dass die CP-Schicht keine Zukunftsinformation nutzt?**
Drei Dinge. Erstens die Reihenfolge im Loop: in jedem Monat wird erst emittiert — Q_t hängt nur
von Scores aus Monaten strikt vor t ab — und dann mit dem realisierten Score aktualisiert; für
alle 14 Implementierungen verifiziert. Zweitens wird der Skalenfaktor S̄ nur aus Burn-in-Monaten
gebildet und danach eingefroren. Drittens werden die Monate 115–150 nie evaluiert.

**23. Wieso ist die MCS so wenig trennscharf? Haben Sie sie richtig implementiert?**
Ja, und die fehlende Trennschärfe ist eine Aussage über die Datenlage. Bei 127 Monaten und
zehn korrelierten Ländern hat der Hansen-Test zu wenig Power. Dazu kommt, dass die Kuratierung
es verschärft: jeder der 26 Kandidaten ist schon der beste aus 9 bis 30 Optionen, also liegen
sie eng zusammen. Mit allen 742 wäre die MCS vollständig informationslos gewesen.

**24. Ihre Selektion des besten Pakets je Familie — ist die nicht zirkulär?**
Sie ist optimistisch, und das sage ich von selbst. Jedes Paket wird als bestes seiner Familie
auf denselben Testdaten gewählt, auf denen es dann verglichen wird. Und ungleich: `lstm` aus
105 zulässigen Paketen, `rw` aus neun — der lstm-Wert ist stärker nach oben verzerrt. Die
Cross-Model-Zahlen sind deshalb Obergrenzen der wahren Trennung.

**25. Warum ist der Panel-t signifikant, der q-Wert aber nicht?**
Weil sie verschiedene Fragen beantworten. Der Panel-t bildet zuerst den Querschnittsmittelwert
je Monat und testet dann diese eine Zeitreihe — er fragt, ob der Effekt im Mittel über die
Länder da ist. Der q-Wert kommt aus zehn separaten Ländertests und fragt, ob er innerhalb eines
einzelnen Landes nach Korrektur nachweisbar ist. Bei zehn korrelierten Ländern und 127 Monaten
fällt die zweite Frage negativ aus. Zwei Auflösungsstufen, kein Widerspruch.

**26. Warum die Harvey-Leybourne-Newbold-Korrektur?**
Weil der Diebold-Mariano-Test in kleinen Stichproben überdimensioniert ist, also zu oft
verwirft. HLN korrigieren mit einem Faktor, der bei h = 1 auf √((T−1)/T) reduziert, und
empfehlen die t-Verteilung mit T−1 = 126 Freiheitsgraden statt der Standardnormalen.

**27. Warum Newey-West mit drei Lags und nicht mehr?**
Die Verlustdifferenziale sind bei h = 1 nur schwach autokorreliert. Drei Lags mit
Bartlett-Kernel ist eine konservative, konventionelle Wahl und wurde ex ante fixiert, nicht
datengetrieben optimiert.

### Zu den Daten

**28. Sie benutzen revidierte Daten. Ist das nicht Leakage?**
Es wäre eines, und deshalb gibt es das Lag-Dictionary. Jede Reihe wird um aufgerundete
⌈Lag/30⌉ volle Monate verschoben, sodass kein Wert im Panel steht, bevor er publiziert war.
Der Fiskalblock kostet vier Monate. Der Preis ist bis zu 29 Tage Überlagerung — das Modell
sieht also teils veraltete Zahlen, was übrigens einer der Gründe sein kann, warum die
Kovariaten nichts beitragen. Eine Restunsauberkeit bleibt und ich weise sie aus: die
Monatsmittel-Serien erscheinen fünf Tage in den Folgemonat und runden auf null Monate.

**29. Warum keine Vintage-Daten?**
Weil sie mir nicht zugänglich waren. Das ist eine echte Einschränkung, keine Designwahl. Mit
Vintage-Daten wäre das Lag-Dictionary unnötig und die Kovariaten hätten aktuellere Information.

**30. Irland trägt ein Drittel Ihres Vorsprungs. Ist Ihr Ergebnis dann nicht ein
Irland-Artefakt?**
Die Rangfolge nicht — Spearman zwischen dem Ranking mit und ohne Irland ist 0.939. Das Niveau
schon: im Schnitt fällt der Skill um 0.021, beim Spitzenpaket um 0.043. Und Irland ist mit
Median-Skill 0.231 das leichteste Land des Panels und steht wegen der
Multinational-Verzerrung im BIP ohnehin unter Vorbehalt. Am stärksten leidet LightGBM im
lokalen Regime — vier der sechs größten Verschlechterungen sind lgbm/L-Pakete. Also: die
Ordnung ist robust, die absoluten Niveaus sind Irland-abhängig, und ich berichte beides.

---

## 15. Ergebnistabellen kompakt

Alles, was auf den Folien steht, an einer Stelle — damit du für eine Zahlenfrage nicht zwischen
Dokumenten springen musst. Quelle durchgängig Diagnostics-Run `run_full_20260810_110834`,
Conformal-Run `run_full_20260721_145309`.

### 15.1 Punktprognose (Folie 10)

Median über die zehn Länder. Ratio < 1 = besser als der Random Walk.

| Modell | MAE-Ratio | RMSE-Ratio | Richtungstreffer | PT-Ablehnungen |
|---|---|---|---|---|
| `tirex2` | **0.997** | 0.999 | 0.555 | 0.10 |
| `rw` | 1.000 | 1.000 | 0.578 ⚠ | undefiniert |
| `tirex2_cov` | 1.001 | 1.000 | 0.534 | 0.20 |
| `arma_monthly` | 1.014 | 1.017 | 0.539 | 0.20 |
| `arma` | 1.017 | 1.017 | 0.536 | 0.20 |
| `tirex` | 1.020 | 1.017 | 0.539 | 0.10 |
| `lgbm_const` | 1.057 | 1.041 | 0.536 | 0.05 |
| `lstm` | 1.061 | 1.055 | 0.552 | 0.08 |
| `lgbm` | 1.065 | 1.044 | 0.535 | 0.03 |
| `lstm_const` | 1.067 | 1.057 | 0.555 | 0.08 |
| `xlstm_const` | 1.093 | 1.074 | 0.530 | 0.06 |
| `xlstm` | 1.104 | 1.080 | 0.522 | 0.06 |

Gesamt: mittlere Richtungstrefferquote **0.5397**, PT-Ablehnungen **7.2 %** aller Zellen
(7.74 % der *definierten* Tests). ⚠ = Artefakt, siehe §4.

**Der Test auf den Vorsprung:** DM/HLN `tirex2` vs `rw` → Panel-t **−0.498**, 0 von 10 Ländern
mit q < 0.10, 5 von 10 Ländern mit MAE-Ratio < 1.
**Positivkontrolle:** `arma` vs `rw` → Panel-t **+2.447**, kleinstes q 0.0040 (Finnland).

### 15.2 Gate und Coverage je CP-Methode (Folie 11)

| Methode | Zulässiger Anteil | Mittlere Coverage | DQ-Ablehnung |
|---|---|---|---|
| `dtaci_cqr` | 1.000 | 0.822 | 0.300 |
| `pid_cqr` | 1.000 | 0.813 | 0.304 |
| `pid_local_cqr` | 1.000 | 0.814 | 0.458 |
| `sfogd_cqr` | 1.000 | 0.806 | 0.632 |
| `spci_cqr` | 1.000 | 0.769 | 0.666 |
| `agaci_cqr` | 0.981 | 0.811 | 0.345 |
| `aci_cqr` | 0.962 | 0.815 | 0.377 |
| `cqr_rolling` | 0.906 | 0.823 | 0.368 |
| `mondrian_cqr` | 0.604 | 0.860 | 0.291 |
| `pid_tan_cqr` | 0.283 | 0.824 | 0.938 |
| `decay_ocp_cqr` | 0.208 | 0.873 | 0.525 |
| `native` | **0.132** | **0.872** | 0.562 |
| `cqr_static` | 0.057 | 0.859 | 0.617 |
| `saocp_cqr` | 0.019 | 0.917 | 0.523 |

Gesamt **485 von 742 zulässig (65.4 %)**. Gate-Rate je Land: FR 0.864, FI 0.844, IT 0.810,
NL 0.771, AT 0.747, BE 0.718, ES 0.660, IE 0.623, PT 0.616, GR 0.519.
Gate-Rate je Modellfamilie: tirex2 und tirex2_cov 0.857, tirex 0.786, lgbm/arma/arma_monthly
0.714, lgbm_const 0.679, lstm_const 0.659, rw und xlstm_const 0.643, lstm 0.625, xlstm 0.613.

### 15.3 Hauptranking (Folie 12)

| Modell | Regime / Methode | Skill | Coverage | Zulässige Länder |
|---|---|---|---|---|
| `tirex2` | ZS / `cqr_static` | **0.146** | 0.750 | 8/10 |
| `tirex2_cov` | ZS / `native` | 0.141 | 0.839 | 8/10 |
| `lgbm` | G / `decay_ocp_cqr` | 0.139 | 0.824 | 7/10 |
| `tirex` | ZS / `native` | 0.133 | 0.813 | 10/10 |
| `lstm_const` | G / `agaci_cqr` | 0.111 | 0.798 | 10/10 |
| `lgbm_const` | G / `cqr_rolling` | 0.099 | 0.829 | 10/10 |
| `xlstm` | L / `native` | 0.094 | 0.817 | 7/10 |
| `rw` | L / `aci_cqr` | 0.085 | 0.812 | 10/10 |
| `xlstm_const` | L / `spci_cqr` | 0.072 | 0.756 | 10/10 |
| `lstm` | L / `pid_cqr` | 0.062 | 0.810 | 10/10 |
| `arma_monthly` | L / `dtaci_cqr` | 0.061 | 0.809 | 10/10 |
| `arma` | L / `dtaci_cqr` | 0.058 | 0.813 | 10/10 |

**Median-Skill über ALLE Kombis je Familie** (auch invalide): tirex2 0.118, tirex2_cov 0.108,
tirex 0.088, lgbm 0.063, lstm_const 0.037, lgbm_const 0.032, rw 0.025, lstm −0.005,
arma −0.014, xlstm_const −0.020, arma_monthly −0.023, **xlstm −0.047**.

**Skill je Land** (zulässige Kombis, Median): IE 0.231, PT 0.191, GR 0.120, ES 0.095,
BE 0.051, NL −0.002, AT −0.015, FR −0.047, FI −0.052, **IT −0.070**.

### 15.4 `tirex2`/ZS über alle 14 Methoden (Folie 12 rechts)

| Methode | Skill | Coverage | Zulässig |
|---|---|---|---|
| `cqr_static` | 0.146 | 0.750 | 8/10 |
| `decay_ocp_cqr` | 0.144 | 0.787 | 10/10 |
| `native` | 0.142 | 0.855 | 7/10 |
| `cqr_rolling` | 0.132 | 0.794 | 10/10 |
| `agaci_cqr` | 0.126 | 0.797 | 10/10 |
| `aci_cqr` | 0.121 | 0.799 | 10/10 |
| `dtaci_cqr` | 0.120 | 0.806 | 10/10 |
| `pid_cqr` | 0.116 | 0.800 | 10/10 |
| `spci_cqr` | 0.080 | 0.762 | 10/10 |
| `pid_local_cqr` | 0.046 | 0.802 | 10/10 |
| `mondrian_cqr` | 0.035 | 0.856 | 9/10 |
| `sfogd_cqr` | 0.017 | 0.801 | 8/10 |
| `saocp_cqr` | 0.012 | 0.894 | 3/10 |
| `pid_tan_cqr` | **−0.044** | 0.810 | 6/10 |

**Spanne −0.044 bis +0.146 innerhalb einer Modellfamilie.**

### 15.5 xLSTM im Detail (Folie 13)

| Seed | Skill | Zulässige Länder | Gate |
|---|---|---|---|
| 42 | 0.043 | 6/10 | fällt durch |
| 43 | −0.008 | 6/10 | fällt durch |
| 44 | **0.094** | 7/10 | passiert |
| Spanne | **0.102** | | > eigener Skill |

Bestes zulässiges xlstm-Paket je Regime: L/native 0.0939, GF/cqr_rolling 0.0098,
G/pid_cqr −0.0070, GI/native −0.0423.

**Seed-Fragilität je Familie** (Anteil mit Spanne > \|Skill\|):

| Familie | n | Median-Spanne | Mittel | alle | nur zulässige |
|---|---|---|---|---|---|
| `lstm` | 168 | 0.046 | 0.070 | 0.685 | 0.733 (n=105) |
| `xlstm_const` | 126 | 0.045 | 0.046 | 0.587 | 0.778 (n=81) |
| `xlstm` | 168 | 0.051 | 0.061 | 0.506 | 0.641 (n=103) |
| `lstm_const` | 126 | 0.015 | 0.020 | 0.238 | 0.145 (n=83) |
| alle NN | 588 | 0.043 | 0.051 | **0.517** | **0.586** (n=372) |

Maximale Seed-Spanne überhaupt: 0.315 (`lstm/GI/saocp_cqr`). Weiteres Beispiel:
`lstm/G/dtaci_cqr` mit 0.0417 / 0.0416 / −0.0014 über die Seeds, Spanne 0.0431 — das Vorzeichen
kippt bei Seed 44.

### 15.6 RQ3 — CP-Schicht gegen `native`

Median über die 53 Kontraste je Methode. **Negativ = Methode besser als native.**

| Methode | d_norm | share_a | Panel-t | share_sig_FDR |
|---|---|---|---|---|
| `dtaci_cqr` | −0.054 | 0.7 | −3.72 | 0.43 |
| `pid_cqr` | −0.054 | 0.8 | −3.97 | 0.50 |
| `aci_cqr` | −0.047 | 0.6 | −3.46 | 0.40 |
| `cqr_rolling` | −0.045 | 0.6 | −3.21 | 0.37 |
| `agaci_cqr` | −0.043 | 0.6 | −3.55 | 0.47 |
| `spci_cqr` | −0.038 | 0.6 | −2.01 | 0.40 |
| `pid_local_cqr` | −0.031 | 0.7 | −2.95 | 0.37 |
| `sfogd_cqr` | −0.008 | 0.5 | −1.61 | 0.30 |
| `decay_ocp_cqr` | +0.002 | 0.5 | −0.36 | 0.47 |
| `mondrian_cqr` | +0.023 | 0.5 | −0.66 | 0.47 |
| `cqr_static` | +0.027 | 0.3 | +8.74 | 0.67 |
| `saocp_cqr` | +0.052 | 0.0 | +4.03 | 0.43 |
| `pid_tan_cqr` | +0.102 | 0.3 | +1.95 | 0.77 |
| **Gesamtmedian** | **−0.0034** | 0.50 | −1.27 | — |

**Wichtig:** über alle Methoden gemittelt ist der Effekt fast null. Erst die Aufspaltung nach
Methode zeigt das Bild — sieben Gewinner, drei klare Verlierer. Den Gesamtmedian nicht
verschweigen.

### 15.7 RQ1 — Pooling und Fine-Tuning

| Kontrast A vs B | n | d_norm | share A | Panel-t | Median q | FDR-sig |
|---|---|---|---|---|---|---|
| `G` vs `L` | 98 | +0.017 | 0.40 | +0.84 | 0.413 | 0.23 |
| `GF` vs `G` | 84 | −0.006 | 0.60 | −1.33 | 0.403 | 0.20 |
| `GF` vs `L` † | 84 | +0.016 | 0.40 | +0.86 | 0.527 | 0.13 |
| `GF` vs `GI` | 84 | −0.054 | 0.90 | −3.33 | 0.245 | 0.35 |
| `lgbm`: G vs L | 14 | −0.014 | 0.65 | −0.48 | 0.546 | n/a |
| `lstm`: G vs L | 42 | +0.019 | 0.40 | +0.95 | 0.510 | seed-st. 0.29 |
| `lstm`: GF vs G | 42 | +0.001 | 0.40 | +0.02 | 0.414 | seed-st. 0.36 |
| `lstm`: GF vs GI | 42 | −0.041 | 0.80 | −2.67 | 0.403 | seed-st. 0.14 |
| `xlstm`: G vs L | 42 | +0.040 | 0.40 | +1.25 | 0.355 | seed-st. 0.57 |
| `xlstm`: GF vs G | 42 | −0.021 | 0.80 | −2.79 | 0.380 | seed-st. 0.86 |
| `xlstm`: GF vs GI | 42 | **−0.060** | 0.90 | **−4.93** | 0.145 | **seed-st. 1.00** |

† post hoc, eigene FDR-Familie. **Die Kette:** Pooling kostet +0.017, Fine-Tuning holt −0.006
zurück, +0.016 Rückstand auf L bleibt. Mediane addieren sich nicht exakt (0.017 − 0.006 = 0.011
gegen gemessene 0.016) — so schreiben.

`xlstm: GF vs GI` ist der **einzige** RQ1-Kontrast mit vollständiger Seed-Vorzeichenstabilität
und gleichzeitig größtem Effekt, stärkstem Panel-t und kleinstem q. Der belastbarste
RQ1-Befund. Die LSTM-Version desselben Kontrasts ist mit 0.14 fast durchgehend instabil — der
Befund trägt über xLSTM, nicht über LSTM.

### 15.8 RQ2 — adaptive gegen konstante Breite

A = adaptiv, B = const-Zwilling. **d_norm > 0 heißt adaptiv SCHLECHTER.**

| Modell / Regime | n | d_norm | share A | Panel-t | Median q | seed-stabil |
|---|---|---|---|---|---|---|
| `lgbm` / L | 14 | −0.018 | 0.65 | −1.60 | 0.536 | n/a |
| `lgbm` / G | 14 | −0.009 | 0.60 | −0.61 | 0.571 | n/a |
| `xlstm` / L | 42 | +0.005 | 0.50 | −0.34 | 0.498 | 0.21 |
| `xlstm` / GF | 42 | +0.006 | 0.50 | +0.45 | 0.339 | 0.21 |
| `lstm` / L | 42 | +0.009 | 0.40 | −0.21 | 0.479 | 0.29 |
| `xlstm` / G | 42 | +0.014 | 0.50 | +0.23 | 0.341 | 0.14 |
| `lstm` / G | 42 | +0.062 | 0.10 | +3.23 | 0.128 | 0.86 |
| `lstm` / GF | 42 | +0.062 | 0.10 | +3.07 | **0.095** | 0.79 |
| **alle** | 280 | **+0.020** | 0.40 | +0.91 | 0.389 | |

**Drei Fälle, nicht ein Befund:** LSTM klar schlechter (seed-stabil 0.86/0.79 — der Befund, der
RQ2 trägt), LightGBM leicht besser, xLSTM **nicht entscheidbar** (seed-stabil 0.14–0.21, nach
Vorregistrierung darf keine Richtungsaussage getroffen werden).

**Median-Skill über alle Kombis, adaptiv vs const:** lstm −0.005 vs +0.037,
xlstm −0.047 vs −0.020, lgbm +0.063 vs +0.032.

**Signifikanz auf drei Ebenen nicht verwechseln:** Einzelkontraste (n=280): 46 mit
median-q < 0.10, kleinstes 0.003. Gruppen (die acht Zeilen): **genau eine** unter der
Schwelle (`lstm/GF` mit 0.095), nächste `lstm/G` mit 0.128, dann Lücke bis 0.339. Alle 280
gepoolt: Median 0.389. Nicht sagen „kein Kontrast ist signifikant" — das ist auf zwei der drei
Ebenen falsch.

### 15.9 Q4 — Kovariaten (Folie 16)

**Panel A, nach Methode.** Δ = Median-Skill uni minus cov (positiv = uni besser);
d̃ = paarweises DM (negativ = uni besser).

| Methode | uni | cov | Δ | d̃_norm | Share uni | Panel-t |
|---|---|---|---|---|---|---|
| `cqr_static` | 0.1461 | 0.1402 | +0.0059 | −0.0024 | 0.6 | +0.63 |
| `decay_ocp_cqr` | 0.1442 | 0.1385 | +0.0058 | −0.0003 | 0.5 | −0.05 |
| `native` | 0.1420 | 0.1409 | +0.0011 | +0.0047 | 0.3 | +2.13 |
| `cqr_rolling` | 0.1316 | 0.1218 | +0.0098 | −0.0026 | 0.8 | −1.17 |
| `agaci_cqr` | 0.1261 | 0.1193 | +0.0068 | −0.0022 | 0.7 | −0.81 |
| `aci_cqr` | 0.1208 | 0.1132 | +0.0077 | −0.0033 | 0.8 | −1.08 |
| `dtaci_cqr` | 0.1195 | 0.1166 | +0.0029 | −0.0003 | 0.6 | −0.69 |
| `pid_cqr` | 0.1160 | 0.1035 | +0.0125 | −0.0060 | 0.6 | −0.21 |
| `spci_cqr` | 0.0796 | 0.0865 | −0.0069 | −0.0001 | 0.5 | −0.86 |
| `pid_local_cqr` | 0.0460 | 0.0516 | −0.0056 | −0.0029 | 0.6 | −0.69 |
| `mondrian_cqr` | 0.0354 | 0.0363 | −0.0009 | −0.0019 | 0.7 | −0.88 |
| `sfogd_cqr` | 0.0172 | 0.0128 | +0.0044 | −0.0055 | 0.8 | −1.04 |
| `saocp_cqr` | 0.0120 | 0.0155 | −0.0035 | +0.0035 | 0.4 | +0.39 |
| `pid_tan_cqr` | −0.0442 | −0.0469 | +0.0027 | −0.0113 | 0.8 | −1.27 |
| **Median** | 0.1178 | 0.1084 | **+0.0036** | **−0.0023** | 0.6 | **−0.75** |

**Panel B, roher Modelloutput vor der CP-Schicht.** Negativ = Kovariaten helfen.

| Land | Winkler uni | Δ Skill | Δ Breite | Δ MAE | Δ Coverage | W-Ratio | Breiten-Ratio |
|---|---|---|---|---|---|---|---|
| AT | 0.1563 | +0.0016 | −0.0041 | +0.0002 | −0.0079 | 1.002 | 0.962 |
| BE | 0.1769 | +0.0006 | −0.0057 | −0.0000 | −0.0157 | 1.001 | 0.960 |
| ES | 0.3129 | +0.0069 | −0.0096 | +0.0013 | −0.0394 | 1.009 | 0.960 |
| FI | 0.1766 | −0.0068 | −0.0053 | −0.0004 | −0.0079 | 0.993 | 0.954 |
| FR | 0.1934 | −0.0028 | −0.0029 | −0.0000 | −0.0079 | 0.997 | 0.978 |
| GR | 1.2413 | −0.0183 | −0.0555 | −0.0007 | −0.0157 | 0.974 | 0.947 |
| IE | 0.2354 | −0.0147 | −0.0133 | +0.0008 | −0.0315 | 0.974 | 0.931 |
| IT | 0.6598 | −0.0067 | −0.0243 | +0.0005 | −0.0157 | 0.993 | 0.942 |
| NL | 0.1415 | −0.0008 | −0.0040 | −0.0001 | 0.0000 | 0.999 | 0.954 |
| PT | 0.5350 | −0.0182 | −0.0258 | +0.0028 | −0.0236 | 0.975 | 0.939 |
| **Median** | 0.2144 | −0.0047 | **−0.0076** | +0.0001 | −0.0157 | 0.995 | **0.954** |

Kovariaten **verengen in allen zehn Ländern** (2.2 bis 6.9 %), senken den Winkler in sieben,
und Coverage fällt von Median 0.843 auf 0.831 — in Richtung 0.80.

**Signifikanz:** 140 Ländertests, 18 mit p < 0.10 vor Korrektur (14 erwartet), Aufteilung
10 zu 8 nach Richtung. BH innerhalb dieser Familie: **keiner überlebt**, kleinstes q 0.42,
Median 0.98. Eingebettet in die Familie der 12 490 Länderzellen: 8 überleben, kleinstes q
0.013, 5 für und 3 gegen die Kovariaten.

**Cross-Model-Kontrast** `tirex2/ZS/cqr_static` vs `tirex2_cov/ZS/native`: d_norm −0.000061,
share 0.50, Median-p 0.4497, Panel-t 0.728, Median-q 0.693. Achtung: vergleicht
*verschiedene* CP-Methoden — für die ceteris-paribus-Aussage die methodenweisen Werte nehmen.

### 15.10 Regime (Folie 18)

| Regime | Länder-Monate | Anteil | Coverage | Mittlerer Winkler | Median-Skill |
|---|---|---|---|---|---|
| Calm | 381 | 0.300 | 0.7795 | 0.2990 | +0.1114 |
| Mid | 509 | 0.401 | 0.8166 | 0.4870 | +0.0100 |
| Stressed | 380 | 0.299 | 0.8412 | 0.5644 | −0.0037 |
| VIX ≤ 20 | 910 | 0.717 | 0.8342 | 0.4333 | +0.0513 |
| VIX > 20 | 360 | 0.283 | 0.7588 | 0.5055 | −0.0222 |

**Rangstabilität der zwölf besten Pakete gegen die Gesamtrangfolge:**

| Regime | Spearman | p | Kendall | größte Rangänderung |
|---|---|---|---|---|
| VIX ≤ 20 | +0.867 | 0.0003 | +0.727 | 3 |
| Stressed | +0.846 | 0.0005 | +0.697 | 4 |
| Calm | +0.490 | 0.1063 | +0.303 | 6 |
| VIX > 20 | +0.399 | 0.1993 | +0.273 | 7 |

**Die zwölf Pakete je Regime** (Skill; Paketnamen nach `tab:res_ranking`):

| Paket | calm | stressed | VIX ≤ 20 | VIX > 20 | Δcov im Stress |
|---|---|---|---|---|---|
| `tirex2` | +0.1554 | +0.1820 | +0.1754 | +0.0191 | +0.0767 |
| `tirex2_cov` | +0.1675 | +0.1702 | +0.1724 | +0.0587 | +0.0219 |
| `lgbm` | +0.1876 | +0.1161 | +0.1609 | **+0.0797** | +0.0159 |
| `tirex` | +0.1552 | +0.1201 | +0.1975 | +0.0545 | +0.0524 |
| `lstm_const` | +0.1894 | +0.0787 | +0.1443 | +0.0135 | +0.0234 |
| `lgbm_const` | +0.1628 | +0.0325 | +0.0943 | +0.0626 | +0.0261 |
| `xlstm` | +0.1602 | +0.0164 | +0.1199 | +0.0680 | +0.0045 |
| `rw` | +0.1658 | −0.0062 | +0.0995 | −0.0038 | +0.0329 |
| `xlstm_const` | +0.1751 | +0.0203 | +0.0733 | −0.0178 | +0.0388 |
| `lstm` | +0.1293 | +0.0663 | +0.1248 | −0.0423 | +0.0345 |
| `arma_monthly` | +0.1456 | −0.0050 | +0.0621 | +0.0484 | +0.0406 |
| `arma` | +0.1397 | −0.0040 | +0.0485 | +0.0540 | +0.0427 |

**Für die `decay_ocp`-Variante von `tirex2`** (nachgerechnet, weil Panel C oben die
`cqr_static`-Variante zeigt): calm +0.1569 / cov 0.7218 · mid +0.1129 / 0.7878 ·
stressed +0.1806 / 0.8500 · VIX ≤ 20 +0.1758 / 0.8055 · VIX > 20 +0.0219 / 0.7389. Ebenfalls
Platz 1 im Stressterzil und Platz 8 bei hohem VIX; Spearman identisch.

### 15.11 Cross-Model und MCS (Anhang D)

**Größte Effekte, alle nicht signifikant** (d_norm negativ = Zeile besser):

| Paar | d_norm | share Zeile | Median q |
|---|---|---|---|
| `tirex2` vs `xlstm_const` | −0.116 | 0.8 | **0.185** |
| `tirex2_cov` vs `xlstm_const` | −0.111 | 0.8 | 0.196 |
| `tirex` vs `xlstm_const` | −0.101 | 0.8 | 0.263 |
| `tirex2` vs `arma` | −0.074 | 0.9 | 0.255 |
| `tirex2` vs `arma_monthly` | −0.067 | 0.9 | 0.298 |
| `tirex2` vs `rw` | −0.040 | 0.9 | 0.568 |
| `tirex2` vs `tirex2_cov` | −0.000061 | 0.5 | 0.693 |

66 Kontraste, **kein einziger** mit Median-q < 0.10; kleinstes 0.185, Median über alle Paare
0.564. Darunter fallen aber **92 von 660** Einzelländertests unter 0.10, und 52 der 66 Paare
enthalten mindestens einen. **29 von 66** Paaren haben \|Panel-t\| > 1.96, 15 davon > 2.58 —
der Panel-t aggregiert erst über die korrelierten Länder und hat mehr Power, trägt aber keine
Korrektur für die 66 Vergleiche. `tirex2_cov` hat mit 9/11 die meisten Panel-t über 1.96,
`tirex2` folgt mit 8/11.

**MCS:** 26 Kandidaten × 10 Länder = 260 Entscheidungen, 238 Inklusionen (0.915). 15 Kandidaten
in allen zehn Ländern, neun weitere in mindestens acht. Nur `rw/L/native` (p 0.087) und
`xlstm/GI/native` (p 0.090) fallen in der Hälfte heraus. Setgrößen je Land siehe §6.

### 15.12 Robustheit

**Ex-Irland** (485 zulässige Kombinationen): Spearman zwischen den Rankings mit und ohne Irland
**0.939**. Mittlere Skill-Änderung **−0.021**, Median −0.018.

| Kombination | mit IE | ohne IE | Δ |
|---|---|---|---|
| `tirex2/ZS/cqr_static` | 0.146 | 0.103 | −0.043 |
| `tirex2/ZS/decay_ocp_cqr` | 0.144 | 0.100 | −0.045 |
| `tirex2/ZS/native` | 0.142 | 0.098 | −0.044 |
| `tirex2_cov/ZS/native` | 0.141 | 0.097 | −0.044 |
| `lgbm/L/aci_cqr` | 0.067 | **−0.027** | **−0.094** |
| `lgbm/L/dtaci_cqr` | 0.075 | −0.009 | −0.084 |
| `lgbm/L/agaci_cqr` | 0.085 | 0.009 | −0.077 |
| `arma_monthly/L/cqr_rolling` | 0.055 | −0.015 | −0.070 |
| `arma/L/pid_tan_cqr` | −0.056 | −0.126 | −0.070 |
| `lgbm/L/cqr_rolling` | 0.077 | 0.010 | −0.067 |

**Vier der sechs größten Verschlechterungen sind `lgbm/L`** — LightGBM im lokalen Regime lebt
fast vollständig von Irland.

**Burn-in** (mittlere Coverage-Differenz erste 36 Testmonate minus Rest, in Prozentpunkten):
native **−6.64**, decay_ocp −4.05, saocp −3.71, cqr_static −3.36, pid_tan −1.00,
mondrian −0.38, agaci −0.34, pid_local −0.25, sfogd +0.11, pid +0.39, cqr_rolling +0.65,
dtaci +0.90, spci +1.21, aci +1.46. **Gesamtmittel −1.07.**
Also **Unter**deckung am Anfang, kein künstlicher Optimismus — und der Effekt trifft genau die
Methoden, die einen gefüllten Pool brauchen, nicht die online nachregelnden.

**Länderkonsistenz** (Kendall τ_b zwischen den Skill-Rankings zweier Länder über alle 742
Kombinationen, 45 Paare): Mittel **0.346**, Median 0.344, Minimum FI–GR **0.073**, Maximum
ES–PT **0.650**.

**Nicht verwechseln:** Kendall τ misst Länderkonsistenz (Achse: Länder), Spearman ρ misst
Rangstabilität gegen einen Schnitt (Achse: Kombinationen). Beides sind Rangkorrelationen auf
verschiedenen Achsen.
