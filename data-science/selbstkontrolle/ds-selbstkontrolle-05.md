---
course: data-science
type: selbstkontrolle
lecture: 5
date: 2026-05-12
tags:
  - data-science
  - selbstkontrolle
  - multivariate-eda
  - pcp
  - spearman
  - pearson
  - mutual-information
  - entropie
  - kausalitaet
  - flashcards/data-science
source: 05_Multivariate_EDA.pdf
---

# Selbstkontrolle 05 — Mehrdimensionale EDA

**Quelle:** [[data-science/resources/05_Multivariate_EDA.pdf|05_Multivariate_EDA.pdf]]

## Fragen

Was ist ein PC-Plot (Parallel Coordinate Plot)?::Eine Visualisierungstechnik fuer multivariate Daten, bei der die $d$ Koordinaten jedes Datenpunkts auf $d$ **parallelen** Achsen aufgetragen werden. Jeder Datenpunkt entspricht einer **Kurve / Polylinie**, die die Achsen schneidet.

Wann ist ein PC-Plot hilfreich?::Bei der Exploration **hoeherdimensionaler Raeume** ($d \ge 4$), wo Scatter- und Bubble-Plots nicht mehr ausreichen. Mehrdimensionale Strukturen wie Cluster, Korrelationen, parallele Buendel und Ausreisser werden sichtbar. Beispiele: Iris-Datensatz mit 4 Merkmalen.

Was ist der Spearman Korrelationskoeffizient?::Der Pearson Korrelationskoeffizient angewandt auf die **Raenge** der Werte. Mit $rg(x_i) = k$ falls $x_i = x_{(k)}$ (Ordnungsstatistik): $\rho_S = \frac{\sum (rg(x_i) - \frac{n+1}{2})(rg(y_i) - \frac{n+1}{2})}{\sqrt{\sum (rg(x_i) - \frac{n+1}{2})^2 \sum (rg(y_i) - \frac{n+1}{2})^2}}$. Spearman misst die Staerke **monotoner** Zusammenhaenge.

Wie gehen wir mit identischen Werten bei der Berechnung von Raengen um?::Bei identischen Werten (Ties/Bindungen) ist die Rangvergabe nicht eindeutig. Stattdessen bildet man den **Durchschnittsrang** aller gleichen Werte. Beispiel: Werte 1.09, 2.17, 2.17, 2.17, 3.02, 4.5 → Raenge 1, 3, 3, 3, 5, 6 (denn $(2+3+4)/3 = 3$).

Was ist der Unterschied zwischen Pearson und Spearman Korrelationskoeffizient?::Pearson misst **lineare** Zusammenhaenge und ist fuer diskrete & stetige Merkmale geeignet. Spearman misst **monotone** Zusammenhaenge und ist zusaetzlich fuer **ordinale** Merkmale geeignet (Schwierigkeitsgrade, Noten). Spearman ist invariant unter streng monotonen Transformationen, Pearson nur unter linearen.

Ist der Pearson Korrelationskoeffizient invariant bzgl. linearer Transformationen?::Ja im Betrag. Fuer $\tilde X = a_X X + b_X$ und $\tilde Y = a_Y Y + b_Y$ gilt $r_{\tilde X \tilde Y} = \frac{a_X a_Y}{|a_X| \cdot |a_Y|} r_{XY}$, also $r_{\tilde X \tilde Y} = \pm r_{XY}$ je nach Vorzeichen von $a_X, a_Y$. Insbesondere $|r_{\tilde X \tilde Y}| = |r_{XY}|$.

Ist der Pearson Korrelationskoeffizient invariant bzgl. monotoner Transformationen?::**Nein** — nur Spearman ist invariant unter streng monotonen Transformationen. Pearson reagiert auf Nichtlinearitaeten der Transformation.

Ist der Pearson Korrelationskoeffizient invariant bzgl. Vertauschen von $X$ und $Y$?::Ja: $r_{XY} = r_{YX}$ und ebenso $\rho_S(X, Y) = \rho_S(Y, X)$. Korrelation ist symmetrisch.

Wie koennen wir nicht-monotone Zusammenhaenge messen?::Mit der **Mutual Information** $I(X; Y) = H(X) - H(X|Y)$ aus der Informationstheorie. Sie misst, wie viel Information $Y$ ueber $X$ liefert, und erkennt auch nichtlineare und nicht-monotone Zusammenhaenge.

Was ist Entropie?::Die Entropie $H(X) = -\sum_k p_k \log_2 p_k$ einer diskreten Zufallsvariablen $X$ ist der **mittlere Informationsgehalt** der Werte von $X$ (Einheit: bit). Sie wird maximal bei Gleichverteilung und minimal bei Konzentration auf einen Wert.

Was ist bedingte Entropie?::$H(X|Y) = \sum_j p_Y(y_j) H(X | Y = y_j)$ — der mittlere Informationsgehalt von $X$ unter der Bedingung, dass $Y$ bekannt ist. Interpretation: Wie viel Unsicherheit ueber $X$ bleibt, wenn ich $Y$ kenne.

Was ist Mutual Information?::$I(X; Y) = H(X) - H(X|Y)$. Sie gibt die **Abnahme des mittleren Informationsgehalts** von $X$ durch Kenntnis von $Y$ an, d.h. wie viel Information $Y$ ueber $X$ liefert. Symmetrisch: $I(X; Y) = I(Y; X)$. Nicht-negativ: $I(X; Y) \ge 0$. Auch als *Information Gain* bezeichnet.

Wie koennen wir Mutual Information, Entropie und bedingte Entropie grafisch darstellen?::Als Venn-Diagramm: zwei sich ueberlappende Kreise $H(X)$ und $H(Y)$. Der Schnitt ist $I(X;Y)$. Die nicht-ueberlappenden Teile sind $H(X|Y)$ bzw. $H(Y|X)$. Es gilt $H(X) = I(X;Y) + H(X|Y)$.

Folgt aus Korrelation eine Kausalitaet?::**Nein**. Hohe Werte von Zusammenhangsmassen koennen auf kausale Zusammenhaenge **hinweisen**, beweisen sie aber nicht. Moegliche Ursachen ohne Kausalitaet: Hidden Confounder (Drittvariable), indirekte Beziehung, zufaellige Korrelation (Spurious Correlation), verdeckte Korrelation durch Cluster.

Folgt aus Kausalitaet eine Korrelation?::Nicht notwendigerweise im linearen Sinn. Ein kausaler Zusammenhang $X \to Y$ kann nichtlinear/nicht-monoton sein und so $r = 0$ bzw. $\rho_S = 0$ ergeben (z.B. quadratische Abhaengigkeit). Mutual Information kann auch hier den Zusammenhang aufdecken.

Was ist eine Scheinkorrelation?::Eine Korrelation, die ohne kausalen Zusammenhang vorliegt. Typische Ursachen: Hidden Confounder (z.B. Alter beeinflusst sowohl Wortschatz als auch Koerpergroesse), indirekte Beziehung ($X \to Z \to Y$), zufaellige Korrelation, Cluster-Effekte.

Was ist eine verdeckte Korrelation?::Eine Korrelation, die durch Clusterstruktur in der Stichprobe maskiert wird. Beispiel: Dosierung vs. Therapieerfolg ist innerhalb der Cluster "leicht erkrankt" und "schwer erkrankt" positiv, ueber die gesamte Stichprobe aber negativ.
