# Story-Report — automatisch extrahiert (Diagnostics Master v2)

*Erzeugt: 20260803_095330 | Quelle: run_full_20260721_145309 | Modus: full | Regeln: `diagnostics_foundation.txt` (ex ante fixiert)*

## Kontext
- Kombinationen: 742 (Modell × Regime × Seed × CP-Methode), Länder: 10, Testmonate: 127 (2015-10…2026-04)
- Validity-gated (Kupiec+Christoffersen, BH-FDR q≥0.1, ≥70% der Länder): 485/742 Kombinationen
- Fehlende Modellfamilien: keine

## Headline-Findings (Score ≥ 0.6)

### [F5] Fehlkalibrierung: xlstm/GF/saocp_cqr — Score 0.97
xlstm/GF/saocp_cqr: Coverage-Validität in 100% der Länder FDR-verworfen; mittlere Coverage 0.936 (Überdeckung um 13.6pp).
*(+228 weitere F5-Instanzen in dieser Score-Klasse, Spanne 0.60–0.97 — vollständig in `findings.parquet`)*

### [F3] RQ2 lstm/G (saocp_cqr): adaptiv vs const — Score 0.95
RQ2 lstm/G (saocp_cqr): adaptiv vs const: A ist im Median über Länder um 21.7% des Referenz-Winklers schlechter (Panel-t[NW] = 8.41; 0% der Länder mit Vorteil A; 60% FDR-signifikant; Median-q = 0.018).
*(+89 weitere F3-Instanzen in dieser Score-Klasse, Spanne 0.60–0.95 — vollständig in `findings.parquet`)*

### [F2] RQ1 lstm (saocp_cqr): G vs L — Score 0.94
RQ1 lstm (saocp_cqr): G vs L: A ist im Median über Länder um 30.4% des Referenz-Winklers schlechter (Panel-t[NW] = 8.33; 0% der Länder mit Vorteil A; 53% FDR-signifikant; Median-q = 0.073).
*(+78 weitere F2-Instanzen in dieser Score-Klasse, Spanne 0.60–0.94 — vollständig in `findings.parquet`)*

### [F10] F10 arma vs arma_monthly (cqr_static) — Score 0.78
F10 arma vs arma_monthly (cqr_static): A ist im Median über Länder um 6.4% des Referenz-Winklers schlechter (Panel-t[NW] = 39.15; 10% der Länder mit Vorteil A; 60% FDR-signifikant; Median-q = 0.000).
*(+2 weitere F10-Instanzen in dieser Score-Klasse, Spanne 0.66–0.78 — vollständig in `findings.parquet`)*

### [F1] Beste CP-Methode für lstm/GF: pid_cqr — Score 0.70
lstm/GF: `pid_cqr` erreicht validity-gated den höchsten Median-Winkler-Skill vs. RW-native (0.057) bei mittlerer Coverage 0.831 (Ziel 0.80).
*(+4 weitere F1-Instanzen in dieser Score-Klasse, Spanne 0.60–0.70 — vollständig in `findings.parquet`)*

### [F8] Pool-Sensitivität: lstm/GI/cqr_rolling — Score 0.68
lstm/GI/cqr_rolling: mittlere Coverage-Differenz rolling vs. expanding = 8.9pp (> 2pp-Schwelle) — Pool-Dünnbesetzung relevant (§F8.1, erwartet für SPCI).
*(+2 weitere F8-Instanzen in dieser Score-Klasse, Spanne 0.66–0.68 — vollständig in `findings.parquet`)*

### [F13] VIX-Komplexität: glänzen komplexe Modelle bei VIX>20? — Score 0.67
Komplex-vs-Klassisch-Vorsprung (Median-Skill): low_vix +0.103 → high_vix +0.025 (Δ -0.077) — widerlegt sie. Bestes Komplexmodell im high_vix-Regime: tirex (Skill 0.019). VIX>20 deckt 28% der Testmonate.

### [F7] Länder-Konsistenz der Rankings — Score 0.64
Mittlere paarweise Kendall-Rangkorrelation der Kombinations-Rankings zwischen Ländern: τ = 0.35 (länderspezifische Muster).

### [F6] Stress-Fragilität: lstm/L/native — Score 0.64
lstm/L/native: Coverage fällt in Stress-Monaten (Vol-Terzil 3) auf 0.808 (gesamt 0.865; Drop -5.7pp).
*(+2 weitere F6-Instanzen in dieser Score-Klasse, Spanne 0.60–0.64 — vollständig in `findings.parquet`)*

## Berichtenswert (0.4 ≤ Score < 0.6)

### [F3] RQ2 xlstm/GF (cqr_static): adaptiv vs const — Score 0.60
RQ2 xlstm/GF (cqr_static): adaptiv vs const: A ist im Median über Länder um 2.9% des Referenz-Winklers besser (Panel-t[NW] = -0.66; 60% der Länder mit Vorteil A; 53% FDR-signifikant; Median-q = 0.083).
*(+150 weitere F3-Instanzen in dieser Score-Klasse, Spanne 0.40–0.60 — vollständig in `findings.parquet`)*

### [F5] Fehlkalibrierung: xlstm/L/mondrian_cqr — Score 0.60
xlstm/L/mondrian_cqr: Coverage-Validität in 60% der Länder FDR-verworfen; mittlere Coverage 0.853 (Überdeckung um 5.3pp).
*(+25 weitere F5-Instanzen in dieser Score-Klasse, Spanne 0.40–0.60 — vollständig in `findings.parquet`)*

### [F2] RQ1 lstm (decay_ocp_cqr): G vs L — Score 0.60
RQ1 lstm (decay_ocp_cqr): G vs L: A ist im Median über Länder um 3.1% des Referenz-Winklers schlechter (Panel-t[NW] = 1.45; 30% der Länder mit Vorteil A; 50% FDR-signifikant; Median-q = 0.165).
*(+147 weitere F2-Instanzen in dieser Score-Klasse, Spanne 0.40–0.60 — vollständig in `findings.parquet`)*

### [F1] Beste CP-Methode für lstm_const/G: agaci_cqr — Score 0.55
lstm_const/G: `agaci_cqr` erreicht validity-gated den höchsten Median-Winkler-Skill vs. RW-native (0.111) bei mittlerer Coverage 0.798 (Ziel 0.80).
*(+15 weitere F1-Instanzen in dieser Score-Klasse, Spanne 0.40–0.55 — vollständig in `findings.parquet`)*

### [F10] F10 arma vs arma_monthly (cqr_rolling) — Score 0.51
F10 arma vs arma_monthly (cqr_rolling): A ist im Median über Länder um 0.2% des Referenz-Winklers schlechter (Panel-t[NW] = 1.17; 50% der Länder mit Vorteil A; 30% FDR-signifikant; Median-q = 0.314).
*(+9 weitere F10-Instanzen in dieser Score-Klasse, Spanne 0.42–0.51 — vollständig in `findings.parquet`)*

### [F11] Burn-in-Doppelnutzung (erste 36 Testmonate) — Score 0.50
Coverage erste 36 Testmonate vs. Rest, Mittel über Kombinationen: größte Differenz bei `native` (-6.6pp); Mittel über alle Methoden -1.1pp (Proposal §4.3: möglicher Anfangsoptimismus, selbstlimitierend).

### [F4] Gesamtpaket: bestes ML vs. bester Klassiker — Score 0.50
Bestes validity-gated ML-Paket: tirex2/ZS/cqr_static (Skill 0.146) vs. bester Klassiker rw/aci_cqr (Skill 0.085). Sekundäre RQ2-Rahmung: Gesamtpaket-Test (Adaptivität + Kovariaten + Punktgüte).

### [F9] Seed-Fragilität: lstm/G/dtaci_cqr — Score 0.44
Seed-Spannweite des Skills (0.043) übersteigt den Effekt — Rangaussage wird nach §F4.4 NICHT getroffen.
*(+2 weitere F9-Instanzen in dieser Score-Klasse, Spanne 0.44–0.44 — vollständig in `findings.parquet`)*


## Anhang (Score < 0.4): 46 Kandidaten — siehe findings.parquet

## Automatische Caveats
- **Unvollständigkeit:** fehlende Familien —; RQ1/RQ2 derzeit nur für die vorhandenen trainierbaren Modelle beantwortbar.
- **Power:** n = 127 Monate/Land; Kalibrierungstests haben moderate Power — Nicht-Ablehnung ≠ Validitätsbeleg (§F3.4).
- **Multiple Vergleiche:** 42170 Test-Zellen insgesamt; FDR kontrolliert die Fehlentdeckungsrate je Familie, nicht einzelne Fehlschlüsse.
- **Querschnittskorrelation:** 10 Länder ≠ 10 unabhängige Evidenzstücke; Panel-t mit Zeit-Clustern berichtet (§F4.2).
- **Seeds:** vorhanden [np.int64(42), np.int64(43), np.int64(44)]; Seed-Disziplin §F4.4 aktiv.
- **Burn-in-Diagnostik:** siehe F11; **GR:** Lücke 2015-07/08 liegt im Burn-in, Testperiode vollständig.