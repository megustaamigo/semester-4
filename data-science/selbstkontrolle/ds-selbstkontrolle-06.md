---
course: data-science
type: selbstkontrolle
lecture: 6
date: 2026-05-19
tags:
  - data-science
  - selbstkontrolle
  - dimensionsreduktion
  - pca
  - hauptkomponentenanalyse
  - fluch-der-dimensionalitaet
  - erklaerte-varianz
  - eigenwerte
  - standardisierung
  - flashcards/data-science
source: 06_Dimensionsreduktion.pdf
---

# Selbstkontrolle 06 — Dimensionsreduktion (PCA)

**Quelle:** [[data-science/resources/06_Dimensionsreduktion.pdf|06_Dimensionsreduktion.pdf]]

## Fragen

Warum wollen wir die Dimension von Daten reduzieren?::Zwei Gruende. (1) **Praktisch**: explorative Datenanalyse und Visualisierung sind bei vielen Dimensionen schwierig (ab $d \ge 4$ kaum darstellbar). (2) **Mathematisch** — der **Fluch der Dimensionalitaet**: im Einheitswuerfel waechst der max. Abstand mit $d_n = \sqrt{n}$ und das Volumen bei Seitenlaengen-Verdoppelung exponentiell ($V([0,2]^n) = 2^n$). Daten verstreuen sich, statistische Analysen werden unzuverlaessig.

Was ist der Fluch der Dimensionalitaet?::In hohen Dimensionen wird der Raum riesig und duenn besetzt: im Einheitswuerfel $[0,1]^n$ ist $V_n = 1$, aber der max. Punktabstand $d_n = \sqrt{n} \to \infty$. Verdoppelt man die Seitenlaenge, waechst das Volumen exponentiell ($2^n$). Dadurch werden statistische Analysen in hohen Dimensionen schwierig.

Warum koennen wir die Dimension von Daten reduzieren?::Weil die **intrinsische Dimension** vieler Datensaetze viel kleiner ist als die Zahl der Merkmale. Merkmale sind oft korreliert oder folgen einer niederdimensionalen Struktur (Punkte fast auf einer Geraden, glatte 365-Tage-Temperaturkurven mit wenigen Komponenten rekonstruierbar). Ueberzaehlige Richtungen enthalten nur Rauschen mit kleiner Varianz.

Wie koennen wir die Dimension von Daten reduzieren (drei Ansaetze)?::(1) **Spalten weglassen** (einfach, aber Info-Verlust moeglich). (2) **Feature Engineering**: neue Features manuell konstruieren, z.B. $\varphi(x_1, x_2) = x_1^2 + x_2^2$ mit $\varphi: \mathbb{R}^2 \to \mathbb{R}$. (3) **PCA**: datengetrieben die beste lineare Reduktion aus den Daten lernen.

Was ist die Idee hinter der PCA?::Suche eine neue **Orthonormalbasis** (Hauptkomponenten), bei der jede Komponente die **Varianz der projizierten Daten maximiert** und **orthogonal** zu allen vorherigen ist. Da $X = s_1 v_1 + s_2 v_2 + \dots \approx s_1 v_1$, stecken die meisten Informationen in den ersten Komponenten; Komponenten mit vernachlaessigbarer Varianz werden weggelassen.

Was ist die Definition der 1. Hauptkomponente?::Fuer **zentrierte** Beobachtungen $X_1, \dots, X_n \in \mathbb{R}^p$ ($\mathbb{E}[X_i] = 0$): $w_1 = \arg\max_{\|w\|=1} \sum_{i=1}^n \langle X_i, w\rangle^2$. In Matrixform mit $\mathbf{X} \in \mathbb{R}^{n\times p}$ (Beobachtungen als Zeilen): $w_1 = \arg\max_{\|w\|=1} \|\mathbf{X}w\|^2 = \arg\max_{\|w\|=1} w^T \mathbf{X}^T\mathbf{X}\,w$.

Was ist die Definition der $k$-ten Hauptkomponente?::Mit den ersten $(k-1)$ Hauptkomponenten und der **Residuenmatrix** $\mathbf{X}_k = \mathbf{X} - \sum_{i=1}^{k-1} \mathbf{X} w_i w_i^T$ (Daten minus Projektion auf $w_1, \dots, w_{k-1}$): $w_k = \arg\max_{\|w\|=1} \|\mathbf{X}_k\, w\|^2$. Man sucht im verbleibenden Rest erneut die Richtung groesster Varianz.

In welchem Verhaeltnis stehen die Hauptkomponenten zueinander?::Sie sind paarweise **orthogonal** und auf Laenge 1 normiert (**Orthonormalbasis** des $\mathbb{R}^p$) und nach **abnehmender erklaerter Varianz** sortiert: $w_1$ maximiert die Varianz, $w_k$ ist orthogonal zu $w_1,\dots,w_{k-1}$ und maximiert die Restvarianz.

Was ist die erklaerte Varianz und wofuer nutzen wir sie?::Die erklaerte Varianz von $w_k$ ist $\max_{\|w\|=1} \|\mathbf{X}w\|^2 = \lambda_k$ (zugehoeriger Eigenwert von $\mathbf{X}^T\mathbf{X}$); sie gibt an, wie viel der Gesamtvarianz die Komponente erfasst. Genutzt wird sie, um die Anzahl $d$ der behaltenen Komponenten zu waehlen — so dass die **kumulierte erklaerte Varianz $\approx 100\%$** ist.

Wie koennen wir Hauptkomponenten und erklaerte Varianz effizient berechnen?::Ueber das **Eigenwertproblem** von $C = \mathbf{X}^T\mathbf{X}$ (symmetrisch). Die **Eigenvektoren** $v_1, \dots, v_p$ (sortiert nach Eigenwert) sind die Hauptkomponenten, die **Eigenwerte** $\lambda_1 \ge \dots \ge \lambda_p$ die erklaerten Varianzen. Hintergrund: Spektralzerlegung $C = Q\Lambda Q^T$, und $\max_{\|w\|=1} w^T C w = \lambda_1$, angenommen von $v_1$. Eigenwerte/-vektoren sind effizient berechenbar.

Was misst das Skalarprodukt $\langle X_i, w\rangle$ in der PCA-Definition?::Den Winkel bzw. die **Projektionslaenge** zwischen Beobachtung $X_i$ und Richtung $w$ — je groesser, desto aehnlicher zeigt $X_i$ in Richtung $w$. Die Summe der Quadrate $\sum_i \langle X_i, w\rangle^2$ ist bei zentrierten Daten die **Varianz der Projektionen** auf $w$; deren Maximierung liefert die Richtung groesster Varianz.

Wie projiziert man eine Beobachtung auf die 1. Hauptkomponente?::$\text{proj}(X) = \langle X, w_1\rangle\, w_1$. Wegen $X^T w_1 = \langle X, w_1\rangle$ ist $\mathbf{X} w_1 w_1^T$ die Projektion **aller** Beobachtungen auf $w_1$. Der Skalar $\langle X, w_1\rangle$ ist die neue Koordinate entlang $w_1$.

Wie koennen wir mit der PCA die Dimension reduzieren?::Durch Projektion auf die ersten $d$ Hauptkomponenten: $\mathbb{R}^p \ni X \to (\langle X, w_1\rangle, \dots, \langle X, w_d\rangle)^T \in \mathbb{R}^d$. Die $d$ Skalarprodukte sind die neuen Koordinaten. Wegen $X \approx \sum_{i=1}^d \langle X, w_i\rangle w_i$ geht nur wenig Information verloren.

Wie koennen wir mit der PCA neue Daten generieren?::Ueber die **Rueckrichtung**: aus $d$ Koeffizienten $\alpha_1, \dots, \alpha_d$ rekonstruiert man $\mathbb{R}^d \ni (\alpha_1, \dots, \alpha_d)^T \to \sum_{i=1}^d \alpha_i w_i \in \mathbb{R}^p$. Mit plausibel gewaehlten $\alpha_i$ entstehen neue, realistisch aussehende Datenpunkte (z.B. Temperaturkurven).

Warum muessen die Beobachtungen vor der PCA zentriert sein?::Die PCA misst Varianz ueber $\sum_i \langle X_i, w\rangle^2$, was nur bei $\mathbb{E}[X_i] = 0$ der Varianz der Projektionen entspricht. Falls die Daten nicht zentriert sind, zuerst zentrieren: $\tilde X_i = X_i - \bar X_n$.

Warum und wie standardisiert man Daten fuer die PCA?::Die Hauptkomponenten **haengen von der Skalierung** ab — ein Merkmal mit grosser Wertespanne dominiert sonst die 1. Hauptkomponente. **Standardisierung** je Merkmal/Spalte: $\tilde X_i = \frac{X_i - \bar X_n}{\hat\sigma}$ ergibt Mittelwert 0 und Varianz 1. **Achtung**: Standardisierung kann verzerren und unwichtigen (verrauschten) Merkmalen grosses Gewicht geben.

Welchen Mittelwert und welche Varianz hat eine Stichprobe nach Standardisierung?::Mittelwert **0** und empirische Varianz **1** (z-Transformation $\tilde X_i = (X_i - \bar X_n)/\hat\sigma$).

Wie waehlt man die Anzahl $d$ der behaltenen Hauptkomponenten?::So, dass die **kumulierte erklaerte Varianz $\approx 100\%$** ist. Praktisch liest man das am Knick (Elbow/Scree) im Plot der kumulierten erklaerten Varianz ab. Bei den Temperaturkurven erklaert bereits die 1. Komponente ~85 %, nach 2-3 Komponenten ist man nahe 100 %.
