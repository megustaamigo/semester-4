---
course: data-science
type: selbstkontrolle
lecture: 4
date: 2026-05-05
tags:
  - data-science
  - selbstkontrolle
  - visualisierung
  - tufte
  - chartjunk
  - korrelation
  - multivariate-eda
  - flashcards/data-science
source: 04_Visualisierungen.pdf
---

# Selbstkontrolle 04 — Visualisierungen und mehrdimensionale EDA

**Quelle:** [[data-science/resources/04_Visualisierungen.pdf|04_Visualisierungen.pdf]]

## Fragen

Was beschreibt das Daten-Druckerschwaerze-Verhaeltnis?::Das Verhaeltnis $q = \frac{\text{Tintenmenge fuer die Darstellung von Daten}}{\text{Tinte der gesamten Abbildung}}$. Nach Tufte sollte es maximiert werden — nur so viel Tinte wie noetig, so wenig wie moeglich.

Warum sollte der Luegenfaktor $\ell = \frac{\text{Groesse eines Effekts in der Abbildung}}{\text{Groesse eines Effekts in den Daten}}$ moeglichst nah bei 1 sein?::Damit die Abbildung das Publikum nicht in die Irre fuehrt. Bei $\ell \neq 1$ werden Effekte ueber- oder untertrieben dargestellt und die Visualisierung wird manipulativ.

Was sind Bad Practices fuer Visualisierungen?::Mittelwerte ohne Standardabweichung; interpolierte Werte ohne tatsaechliche Datenpunkte; manipuliertes Seitenverhaeltnis zur Dramatisierung; fehlender Koordinatenursprung (Standard in matplotlib!); fehlende oder irrefuehrende Achsenbeschriftungen; unnoetige 3D-Effekte; Chartjunk.

Was ist Chartjunk?::Visuelle Elemente in Abbildungen, die unnoetig sind oder das Verstaendnis erschweren (Schmuck, 3D-Schatten, dekorative Hintergruende, redundante Gitter). Begriff von Edward Tufte.

Warum sind manche Farbverlaeufe besser fuer Visualisierungen geeignet als andere?::Gute Farbverlaeufe (Viridis, Magma, Inferno, Plasma) haben ein lineares Leuchtdichteprofil ($df/dx \approx const$). Damit entsprechen gleiche Datenunterschiede gleichen perzeptuellen Farbunterschieden. Jet hat ein nichtlineares Profil und betont vermeintliche Eigenschaften, die nicht existieren. Gute Farbverlaeufe sind auch in Graustufen und fuer Farbfehlsichtige nutzbar (8% Maenner, 0.5% Frauen haben eine Fehlsichtigkeit).

Was ist der Unterschied zwischen univariater und multivariater EDA?::Univariat: ein Merkmal/Feature, Methoden wie Kennzahlen fuer Lage und Streuung, Boxplots. Multivariat: mehr als ein Merkmal, untersucht Abhaengigkeiten zwischen Merkmalen (z.B. Mietpreis vs. Wohnungsgroesse).

Wie koennen wir 2- bzw. 3-dimensionale Daten visualisieren?::2D: Scatterplot (Streudiagramm) mit `plt.scatter(x, y)`. 3D: Bubble Chart (Blasendiagramm) mit `plt.scatter(x, y, s=z)` — die dritte Dimension wird als Punktgroesse kodiert.

Was beschreibt der (Pearson) Korrelationskoeffizient?::$r = \frac{s_{XY}}{s_X s_Y}$ mit $s_{XY} = \sum (x_i - \bar{x})(y_i - \bar{y})$. Er ist normiert ($-1 \le r \le 1$) und misst die Staerke des **linearen** Zusammenhangs. $r = \pm 1$ bei perfekter Gerade, $r = 0$ bei konstanter Gerade. Rule-of-Thumb: schwach $|r| < 0.5$, mittel $0.5 \le |r| < 0.8$, stark $|r| \ge 0.8$.

Was ist die empirische Kovarianz und wie ergibt sich ihr Vorzeichen?::$s_{XY} = \sum_{i=1}^{n} (x_i - \bar{x}_n)(y_i - \bar{y}_n)$. Das Vorzeichen jedes Summanden ergibt sich aus der Lage von $(x_i, y_i)$ gegenueber dem Schwerpunkt $(\bar{x}_n, \bar{y}_n)$: Punkte im ersten/dritten Quadranten (bzgl. Schwerpunkt) liefern positive, Punkte im zweiten/vierten negative Beitraege.
