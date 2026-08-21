# Kontrollliste zu den neu erzeugten Tabellenbeschreibungen

Erzeugt nach `PROMPT_TABELLENBESCHREIBUNGEN.md`. Alle alten `\caption{}`-Texte
und alle alten Notes-Bloecke wurden verworfen und vollstaendig neu geschrieben.

**Umfang:** 37 Tabellen in `sections/results.tex` (18), `sections/appendix.tex` (11),
`sections/limitations.tex` (3), `sections/data.tex` (2),
`sections/experimental_design.tex` (2), `sections/conformal.tex` (1).
`sections/prompts.tex` wurde nicht angefasst — die Datei ist in `main.tex`
nicht eingebunden und enthaelt nur alte Duplikate von `tab:features_overview`
und `tab:publication-lags`.

**Regelanwendung:** Der Prompt sagt „Bei einer Tabelle ohne Panels und ohne
Marker entfaellt B ersatzlos." Das wurde streng angewendet. 17 Tabellen haben
Panels oder Marker und damit einen Notes-Block, 20 Tabellen haben nur eine
Caption. Bei diesen 20 musste alles, was zum Lesen noetig ist, in die 25 Woerter
der Caption passen. Wo das nicht ging, steht die Luecke unten in Abschnitt C.

**Formale Pruefung:** Alle Captions <= 25 Woerter, alle Notes <= 90 Woerter,
hoechstens ein Semikolon pro Notes-Block, keine britische Schreibung, keine
Geviertstriche im Fliesstext der Beschreibung. Alle `\cref`-Ziele existieren.

---

## A. Bewusst weggelassen, weil bereits im Fliesstext

### `tab:res_gate`
- Durchfallquote und Umfang des Gates — *„Only 65.4 percent of all combinations
  (485) survived the validity gate."*
- Uebercoverage der nativen Ausgabe — *„they are very conservative with an
  average coverage of 0.872 instead of 0.8."*
- DQ-Ablehnungsquote — *„The DQ test that additionally tests for 4 lags and
  width rejects 49.3% of all combinations (q<0.10)."*
- Zielcoverage 0.80 — steht in `sec:eval:coverage`: *„We want our prediction
  intervals to cover 80% of the realized data points."* Ersetzt durch `\cref`.

### `tab:res_point`
- Vorsprung von TiRex-2 — *„Only TiRex2 beats the Random Walk with an advantage
  of 0.35%."*
- PT-Trefferquote und Signifikanzanteil — *„The models predict the right
  direction on average in 0.5397% with the Pesaran Timmermann Test being
  significant at 5% in 7.2% of cells only."*
- Definition der Richtungsprognose und das RW-Artefakt — *„A forecast is counted
  as positive if the future value is predicted to be bigger than the current
  one. Since the Random Walk always predicts no change it produces negative
  predictions only."*

### `tab:res_ranking`
- Alle Skill- und Coverage-Werte der Spitzengruppe — *„the highest median winkler
  skill is reached by tirex2/ZS/cqr_static followed by tirex2_cov/ZS/native with
  respective median winkler skills of 0.146/0.141 and mean coverage of
  0.75/0.839, with both being admissible in only 8/10 countries."*
- Platzierung des besten klassischen Pakets — *„The best classical approach
  rw/L/aci_cqr reached 8th place with a median winkler skill of 0.085."*

### `tab:res_tirex2_cp`
- Vergleich `cqr_static` gegen `decay_ocp_cqr` — *„the second best performing
  (winkler skill) CP method decay_ocp_cqr only has a difference, in winkler
  skill, of 0.002 to the leading model but its coverage (0.787) is way closer to
  the target of 0.8 and it is admissible in all 10 countries."*
- Spannweite der Familie — *„Inside this family the median skill reaches from
  -0.044 (pid_tan) up to 0.146 (cqr_static)."*

### `tab:res_percountry`
- Alle Laenderbefunde — *„All models represented in the Table reach the highest
  skill on Ireland. While the top three models reach the lowest skill on
  Finland, the classical method reaches its lowest skill on Italy (-0.049)."*
- Gate-Abdeckung — *„The classical package is the only one out of the 4 to be
  admissible on all 10 countries."*

### `tab:rq3_results`
- Effektgroessen der Gewinner und Verlierer — *„The seven winning methods have a
  d_norm ranging from -0.054 to -0.031 with Panel-t between -4 and -2. Three
  methods perform worse than the native: cqr_static (+0.027), saocp (+0.052),
  pid_tan (+0.102)."*
- Gate-Raten — *„Native model output only passes the gate in 13.2% of cases while
  dtaci/pid/pid_local/sfogd/spci pass in 100% of cases."*
- FDR-Anteile — *„After the FDR correction the rate of winning models ranges only
  from 0.37 to 0.50."*

### `tab:rq1_results`
- Alle vier Kontrastwerte — *„the Global (G) vs. the Local (L) regime has a
  positive d_norm indicating a superiority of the L regime."*, *„the fine tuned
  models had an overall, but not statistically significant, better performance
  with a d_norm of -0.006"*, *„the strongest effect in the comparison GF vs. GI
  with the d_norm being -0.0543 and share_a_better=0.90"*, *„The comparison GF
  vs. L was added post hoc. With a d_norm of 0.016."*
- Panel-C-Regimewerte — *„The zero-shot regime reaches the highest gate rate with
  0.833 followed by the local regime with 0.668. Regime GI has the lowest gate
  rate with 0.602."*
- LightGBM ohne GF und ohne Seeds — steht in `sec:exp:regimes` (*„LightGBM uses
  only L and G"*) und `sec:exp:seeds` (*„The remaining models, RW, ARIMA,
  LightGBM, TiRex, and TiRex-2, are deterministic and run with a single seed."*).
  Deshalb kein `n/a`-Hinweis in den Notes.

### `tab:res_contrasts`
- Saemtliche Effektwerte, sie stehen verteilt in `sec:res:rq1`, `sec:res:rq2` und
  `sec:res:rq3`. Die Notes verweisen nur per `\cref` auf diese drei Abschnitte.

### `tab:rq2_results`
- Gesamtbefund — *„In the median case the adaptive variant performs worse than
  the constant width clone (d_norm=+0.020, share_a_better=0.40, Panel-t=+0.91)."*
- LSTM- und LightGBM-Befunde — *„For LSTM the adaptive variant is consistently
  worse ... with a d_norm of 0.062 in both the G and GF regime"*, *„LightGBM sees
  a light improvement via the adaptive variant with the L and G regime having
  respective d_norms of -0.018/-0.009."*
- Seed-Stabilitaet der xLSTM-Kontraste — *„the seeds only agree in 14% for
  xlstm/G and 21% for xlstm/GF."*
- Seed-Fragilitaet des Siegerpakets — *„it being admissible only for one out of
  three seeds."*
- FDR-Bilanz — *„Out of all 280 contrasts, 46 are FDR significant (median_q <
  0.10) with the smallest q being 0.003."*
- Konstruktion des Zwillings — *„The twin exists only for the trainable models
  since the interval width is created from the in-sample residuals."*

### `tab:res_rq2_lstmG`
- Der Einzelbefund — *„In Italy in seed 44 the adaptive quantiles produce a small
  positive impact (-0.038). For Ireland we see a statistically significant win
  for the constant quantiles with q<0.001 and share_a_better=0.83."*

### `tab:res_cov`
- Alle Panel-A-Aggregate — *„The median difference in median skill between all CP
  methods is +0.0036 with a median d_norm of -0.0023, a Panel t of -0.75 and a
  median statistically better share of 10% where cov won over uni."*
- Der `native`-Ausreisser — *„We see the highest Panel t (2.13) for the native
  model output with 70% where cov performed better."*
- Alle Panel-B-Mediane — *„The median over all countries yields for the
  difference in Winkler skill 0.0016, for the difference in width -0.0041, for
  the difference in MAE 0.0002 with W ratio and Width ratio being a little under
  one."*
- Anzahl der Kovariaten und identisches Testfenster — *„whether including our 17
  covariates leads to an advantage ... the same model, on the same 127 test
  months."*

### `tab:res_xm`
- Signifikanzbilanz — *„no contrast reaches significance on the median q, with
  the lowest value (0.185) in the contrast between tirex2 and xlstm_const."*
- Laenderebene — *„92 of the 660 single country tests do fall below 0.10, but no
  pair gets more than 4 of its 10 countries there."*
- Panel-t-Bilanz — *„That is why 29 of the 66 pairs come out over 1.96."*
- Optimismus der Auswahl — *„lstm was picked out of 105 admissible packages and
  xlstm out of 103, while rw had 9 and tirex2 12."*
- **Bewusst behalten:** die Schwelle 1.96 in der Marker-Legende. Sie steht zwar
  im Fliesstext (*„tirex2_cov has the most panel t values over 1.96"*), ohne sie
  ist der Marker $\dagger$ aber nicht lesbar. Regel 5 (Marker-Legende ist
  Pflicht) hat hier Vorrang bekommen.

### `tab:res_mcs`
- Aufbau des Kandidatensatzes — *„we use the MCS on the 26 best admissible
  packages, one for each model family and regime, plus the Random Walk
  reference, which we force into the set."*
- Einschlussbilanz — *„15 out of the 26 packages are in the winning set for all
  10 countries"* und *„Finland and France exclude none of the 26 packages while
  Ireland excludes 8."*
- **Korrektur gegen `mcs_results.csv`:** die alte Notes-Zeile zur Seed-Herkunft
  war unvollstaendig. Sie nannte Seed 43 fuer die Kandidaten 5, 6, 17, 18, 20, 23
  und Seed 44 fuer 8, 11, 19. Tatsaechlich laeuft Kandidat 25
  (`xlstm/G/pid_cqr`) auf Seed 43 und Kandidat 22 (`xlstm_const/G/pid_cqr`) auf
  Seed 44. Beide fehlten. Die neuen Notes nennen alle elf.

### `tab:res_regime`
- Alle Regimewerte — *„Over all models we see in the terziles calm/mid/stressed
  respectively a coverage of 0.7795/0.8166/0.8412 and median winkler skill of
  0.1114/0.01/-0.0037."* und *„Looking at the VIX ... in calm phases (VIX<=20)
  the coverage/skill is 0.8342/0.0513 and in stressed phases (VIX>20) it is
  0.7588/-0.0222."*
- Ex-ante-Festlegung und Behandlung der ersten zwoelf Monate — *„Both methods for
  the regime classification were ex ante fixed. The realized spread volatility
  terziles are calculated from the spread's own lag and per country. The first
  twelve test months are classified as the average terzile."*
- Untercoverage von `cqr_static` in ruhigen Monaten — *„it covers only at 0.654
  in the calm months."*
- Abweichende Skill-Definition gegenueber der Komplexitaetshypothese — *„We use
  two different skills in the regime table vs the pre-registered complexity
  hypothesis."*

### `tab:res_robustness_pool`
- Namen der sechs pool-lesenden Methoden — *„only 6 out of the 14 make use of
  this pool, they are: cqr_rolling, aci_cqr, agaci_cqr, dtaci_cqr, spci_cqr,
  mondrian_cqr."*
- Alle Abweichungswerte — *„an average absolute coverage difference to rolling36
  of 0.040 for expanding (Maximum 0.189), of 0.021 for rolling24 (max 0.102) and
  of 0.016 for rolling48 (max 0.087)."*
- Coverage-Ordnung der Pools — *„rolling24 0.813 < rolling36 0.817 < rolling48
  0.820 < expanding 0.838."*
- Begruendung fuer Panel C — *„cqr_rolling is the only CP method using the pool
  that calculates their quantiles from scratch every month."*
- Kendall-Werte in Panel D — *„tau spanning from 0.0373 in FI vs. GR to 0.650 in
  ES vs. PT and an overall tau of 0.35."*

### `tab:res_robustness_ie`
- Rangstabilitaet — *„The Spearman rank correlation over all 485 admissible
  combinations is 0.939."*
- Mittlere und mediane Aenderung — *„The average Winkler skill over all
  admissible combinations decreases by -0.021 (median -0.018)."*
- Warum Irland traegt — *„Ireland also is with a median winkler skill of 0.231
  over all admissible packages the easiest country with second being Portugal
  (0.191)."*
- Der groesste Verlierer — *„The package that suffers the most ... is
  lgbm/L/aci_cqr which went from a positive winkler score including Ireland
  (0.067) to a negative (-0.027)."*

### `tab:res_robustness_seeds`
- Alle Spannwerte — *„Over all NN combinations the median seed span is 0.043, on
  average 0.051 and at max 0.315 (lstm/GI/saocp_cqr)."*
- Fragilitaetsanteile je Familie — *„lstm_const 23.8%, xlstm 50.6%, xlstm_const
  58.7%, lstm 68.5%."*
- Wirkung des Gates — *„Validity gating does not change this dynamic, here the
  share of fragile combinations increases to 58.6%."*
- Beide Rechenbeispiele stehen ausgeschrieben im Fliesstext.
- Deutung des Seed-Spans — *„the seed span of the skill is a direct measure of
  the randomness of the outcome"* (`sec:lim:seeds`). Deshalb steht in den Notes
  nur noch die Rechenvorschrift, keine Deutung.

### `tab:res_robustness_burnin`
- Alle Werte — *„On average over all 14 methods we see a slight under coverage
  (-1.07%) in the first 36 months. The biggest difference appears in the native
  method (-6.64%) followed by decay_ocp -4.1% and saocp -3.7%."*
- Erklaerung des Musters — *„We assume this effect occurs mainly in methods that
  need a full calibration pool not in those that adjust online."*

### `tab:lim_point`
- Beide Panel-t-Werte und die Trefferbilanz — *„TiRex-2 does not achieve a
  significantly better MAE ratio than the Random Walk (0.35% advantage, Panel-t
  = -0.4979, 0/10 countries significant). A control contrast from arma vs. rw
  showed the power of the tests."*
- Der kleinste q-Wert — *„a smallest q-value of 0.3803"* und *„FI with q 0.0040"*.
- Frankreich-Ausschluss — *„9-country panel, as France reduces to ARMA(0,0) and
  reproduces the RW identically"* (`sec:lim:scope`). **Bewusst behalten**, weil
  die Zeile `FR ... undefined` sonst in der Tabelle unlesbar bleibt und der Satz
  in einem anderen Unterabschnitt steht als die Tabelle.

### `tab:lim_dq`
- Alle Ablehnungsquoten — *„The DQ test has a rejection rate of 49.3%. If we
  exclude the width regressor it drops to 20.8% ... Testing just with the width
  regressor yields a rejection rate of 68.9%."*
- Christoffersen-Wert — *„a rejection rate of 6.1%."*
- Vorzeichenbilanz und Median — *„From the 3949 cells with FDR significant gamma,
  99.92% are negative with a median of -1.631."*
- Klassenvergleich und Familienreihung stehen vollstaendig im Fliesstext.
- **Bewusst behalten:** der Satz *„A negative gamma means wide intervals
  overcover and narrow ones undercover."* Er steht sinngemaess im Fliesstext, ist
  aber nach Regel 4 als Vorzeichenkonvention verpflichtend.

### `tab:lim_amendments`
- Der Inhalt der Zeilen erklaert sich aus der Tabelle selbst; nichts aus dem
  Fliesstext wiederholt.

### `tab:publication-lags`
- Rundungskosten — *„Since we are always snapping to the end of the month, we
  risk over-lagging by 29 to 30 days."*
- Ziel nie verschoben — *„The target itself is never lagged."* (in den Notes
  trotzdem knapp genannt, weil es die Lesart der Spalte *Shift* festlegt).
- Der Fuenf-Tage-Vorlauf — *„Look-ahead bias still remains in the monthly average
  series (Moody's Aaa/Baa yields, EPU): they appear about 5 days into the next
  month. We still employ no time lag since we judge it negligible."*

### `tab:features_overview`
- Aufbau und Herkunft der Features stehen vollstaendig in `sec:data:features`;
  die Caption nennt nur noch Spaltenbedeutung und Bezugsgroesse.

### `tab:refit_blocks`
- Blocklaenge, Trainingsminimum und Testfenster — *„We retrain all models on an
  expanding training set every 12 months ... The first 114 months form the
  minimal training set ... Evaluation takes place on months 151 to 277."*
- Doppelrolle der Burn-in-Monate — *„Months 115 to 150 serve a double role: they
  are the burn-in set on which the conformal methods are calibrated ... and the
  validation set for the hyperparameter optimization."*

### `tab:model_selection`
- Gemeinsame Randbedingungen — *„Every trainable model with an information
  horizon is restricted to a shared maximum of 24 months"* und die
  Skalierungsbeschreibung in `sec:exp:sequences`.

### `tab:cp_methods`
- Dass kein Parameter getunt wurde — *„No parameter in the conformal calibration
  layer was tuned or optimized on data."* Die Caption verweist nur per `\cref`.

### `tab:app_frozen`, `tab:app_protocol`, `tab:app_nn_space`, `tab:app_lgbm_space`, `tab:app_frozen_nn`, `tab:app_frozen_lgbm`, `tab:app_frozen_arma`
- Das gesamte Suchprotokoll steht in `sec:exp:hpo`, u. a. *„The hyperparameters
  are tuned with Optuna (TPE sampler, seed 42, 20 start-up trials ...)"* und
  *„In the G regime the country enters as a native categorical feature."*
- Determinismus von RW und ARMA — *„Both models are deterministic."*
  (`sec:exp:hpo`).
- Frankreich-Kollaps — *„The trend is suppressed, so ARMA(0,0) nests the Random
  Walk."* (in der Caption trotzdem knapp genannt, weil die Spalte FR sonst
  unlesbar ist).

### `tab:app_country`, `tab:app_tail`, `tab:app_pool`, `tab:app_fragility`
- Alle Werte stehen in `sec:lim:sample`, `sec:res:ranking` und
  `sec:res:robustness`, u. a. *„The mean pairwise Kendall-tau between the
  combination rankings of countries is 0.346"*, *„The tail asymmetry spreads
  wider per country (0.431 to 0.642) than per model (0.483 to 0.618)"* und
  *„Greece is the country that rejects the most with a gate rate of 51.9%."*

---

## B. Inhalte aus den alten Captions und Notes, die NIRGENDS im Fliesstext stehen

Diese Angaben sind mit den alten Beschreibungen weggefallen. Nach dem Protokoll
in `PROMPT_TABELLENBESCHREIBUNGEN.md` gehoeren sie in den Fliesstext, nicht
zurueck in die Notes. Bitte einzeln entscheiden.

| Tabelle | Verlorener Inhalt |
|---|---|
| `tab:res_ranking` | Das strengere Post-hoc-Gate ueber alle zehn Laender. Vier von zwoelf Auswahlen aendern sich: `tirex2` auf `decay_ocp_cqr` (0.144), `tirex2_cov` auf `decay_ocp_cqr` (0.139), `lgbm` auf `agaci_cqr` (0.128), `xlstm` auf `cqr_rolling` (0.068). Die Spitzengruppe bleibt geordnet, `xlstm` faellt dann unter den Random Walk. |
| `tab:rq1_results` | Die FDR-Bilanz der Kontrastfamilie: ueber die 266 praeregistrierten Kontraste kleinstes Median-q 0.019, nur 30 unter 0.10, dagegen 131 mit \|t\|>1.96; in der Post-hoc-Familie kleinstes Median-q 0.083 und 6 von 84 unter 0.10. Ebenso der Satz, dass Panel-Statistik und q-Wert verschiedene Fragen beantworten. |
| `tab:res_rq2_lstmG` | Die Aufloesung der Differenz zur Aggregatzeile in `tab:rq2_results` (+0.062 dort ist der Median der 42 Kontrast-Mediane, hier werden 10x42=420 Zellen gepoolt) sowie der Umfang der FDR-Familie (12.490 Laenderzellen). Die neuen Notes nennen die Differenz, aber ohne Zahlen. |
| `tab:res_cov` | Der gesamte Signifikanzblock: 18 von 140 Laendertests unter p<0.10 gegen 14 unter der Nullhypothese erwartete, Aufteilung 10 zu 8; nach BH innerhalb der 140 bleibt keiner (kleinstes q 0.42, Median 0.98); in der grossen Familie blieben 8 uebrig (kleinstes q 0.013, 5 zu 3). Ebenso die Provenienz *„The paired columns are computed post hoc ... and are not part of the shipped run artefacts"* und die Panel-B-Bilanz (Verengung um 2.2 bis 6.9 Prozent in allen zehn Laendern, Coverage-Median von 0.843 auf 0.831). |
| `tab:res_xm` | Dass 52 der 66 Paare mindestens einen signifikanten Laendertest enthalten, dass 15 der 29 Dagger-Paare ueber 2.58 liegen, und dass der Median ueber alle 66 Paare 0.564 betraegt. |
| `tab:res_mcs` | Die Einschlussbilanz auf Zellebene: 238 von 260 Entscheidungen sind Einschluesse (0.915), neun weitere Kandidaten ueberleben in mindestens acht Laendern, und Set-Groesse und terminaler p-Wert laufen nicht parallel (FI behaelt alle 26 beim kleinsten p, NL schliesst zwei beim groessten aus). |
| `tab:res_regime` | Zwei Bloecke. Erstens die Rangkorrelation gegen `tab:res_ranking`: unter realisiertem Stress Spearman +0.846 (p=0.0005, groesste Rangaenderung vier), unter hohem VIX +0.399 (p=0.199, groesste Rangaenderung sieben). Zweitens der ganze Absatz zum Kalenderschnitt aus `config/pipeline.yaml`: 89.5 Prozent der gestressten Laendermonate liegen vor 2022-01, danach sind 51.2 Prozent ruhig gegen 7.7 Prozent gestresst. |
| `tab:res_robustness_pool` | Die Referenzwerte der acht pool-invarianten Methoden (mittlere Coverage 0.847, Median-Skill -0.024, unter allen vier Pools bit-identisch). |
| `tab:lim_amendments` | Der Hinweis, dass jede Zeile zusaetzlich in Absatz 11 des Protokolldokuments unter demselben Datum verzeichnet ist. |
| `tab:lim_point`, `tab:lim_dq` | Die Bearbeitungsdaten (2026-08-03, Nachpruefung 2026-08-17). Ersetzt durch einen `\cref` auf `tab:lim_amendments`, wo sie stehen. |

---

## C. Definitionen, die mir im Fliesstext gefehlt haben

Nach Ausgabepunkt 3 des Prompts: hier habe ich nichts erfunden, sondern die
Luecke stehen gelassen.

1. **Kapitel 7 definiert die Vergleichsstatistik nicht.**
   `\subsection{Formal forecast comparison}` (`sec:eval:comparison`) ist ein
   leerer Stub, ebenso `sec:eval:mcs`, `sec:eval:regime`, `sec:eval:economic`
   und `sec:eval:robustness`. Damit sind `d_norm`, Panel `t`, DM--HLN, Median
   `q`, FDR-sig. und seed-stable **nirgends in Kapitel 7 definiert**; sie tauchen
   erst in `sec:lim:inference` auf. Der Prompt sieht fuer solche Groessen einen
   reinen `\cref` vor. Weil es kein Ziel gibt, musste ich sie in jedem
   betroffenen Notes-Block ausschreiben, was dort 30 bis 40 der 90 Woerter
   verbraucht. Wenn du die Stubs fuellst, koennen sieben Notes-Bloecke deutlich
   kuerzer werden.

2. **`tab:res_gate`, Spalte *Mean coverage*.** Ueber welche Grundgesamtheit
   gemittelt wird (die 530 Zellen der Methode), steht nirgends. Die Tabelle hat
   weder Panels noch Marker, also ist ein Notes-Block nach der Regel des Prompts
   ausgeschlossen, und in 25 Caption-Woerter passt es nicht mehr.

3. **`tab:rq3_results` gegen `tab:res_contrasts`: gleicher Spaltenname, andere
   Groesse.** Gegen `dm_contrasts.csv` geprueft: *Share Sig. (FDR)* in
   `tab:rq3_results` ist der **Median ueber die 53 Kontraste des Anteils der
   Laender mit q<0.10** (`dtaci_cqr`: 0.4333). *FDR-sig.* in
   `tab:res_contrasts` Panel A ist dagegen der **Anteil der Kontraste, deren
   Median-q unter der Schwelle liegt** (`dtaci_cqr`: 0.2830). Daher stehen fuer
   dieselbe Methode 0.43 in der einen und 0.28 in der anderen Tabelle. Das ist
   nirgends erklaert. `tab:rq3_results` hat keine Panels und keine Marker, kann
   die Klaerung also nicht in einem Notes-Block tragen.

4. **`tab:res_cov`, Panel B, Spalte $\Delta$ cov.** Die Lesart „negativ spricht
   fuer die Kovariaten" gilt sauber fuer Skill, Breite, MAE und die beiden
   Ratios (gegen `cells.csv` nachgerechnet). Bei einer reinen
   Coverage-Differenz ist „spricht fuer" keine definierte Richtung. Im
   Fliesstext steht dazu nichts. Ein Satz im Text oder ein Zusatz in der
   Spaltenueberschrift wuerde das aufloesen.

5. **`tab:app_country`, Spalte *Median skill*.** Der Bezug (Median ueber die
   zulaessigen Kombinationen des jeweiligen Landes) passt nicht mehr in die
   25 Woerter und die Tabelle darf keinen Notes-Block haben.

6. **`tab:res_gate`, Spalte *Admissible share*.** Ob der Nenner alle 53
   Kombinationen der Methode oder nur eine Teilmenge ist, geht aus dem
   Fliesstext nicht eindeutig hervor. Die Caption formuliert deshalb neutral
   („the fraction of a method's combinations").

---

## D. Weitere Beobachtungen beim Durchgang

- `\cref{tab:cp_params}` in `sections/conformal.tex` zeigt auf ein Label, das im
  Projekt nicht existiert. Nicht von mir verursacht, faellt beim Setzen als
  `??` auf.
- `fig:skill_heatmap` in `sections/results.tex` traegt noch die deutsche Caption
  *„Heatmap zur Uebersicht der Modell-Skills."* mit TODO. Abbildungen waren
  nicht Teil dieses Auftrags.
- `\cref{sec:res:limitations}` wird in `results.tex` und `limitations.tex`
  verwendet, das Label existiert nicht.
- `sections/hilfsmittel.tex` ist in `main.tex` nicht eingebunden. Die Liste
  *Betroffene Tabellen* dort habe ich auf alle 37 Tabellen erweitert, Datum und
  Bearbeitungsvermerk bleiben Platzhalter.
- In fuenf Captions (`tab:app_nn_space`, `tab:app_frozen_nn`, `tab:app_pool`,
  `tab:model_selection`, `tab:cp_methods`) steht `---` in Anfuehrungszeichen.
  Das ist kein Geviertstrich im Sinne der Stilregel, sondern das Zitat des
  Tabelleneintrags, dessen Bedeutung erklaert wird.
