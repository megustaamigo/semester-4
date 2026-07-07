---
course: numerik
type: solution
lecture: 12
date: 2026-06-29
tags:
  - numerik
  - solution
  - optimierung
  - stationaere-punkte
  - hessematrix
  - positiv-definit
source: Handrechnung
---

# Loesungen — Blatt 12

**Aufgaben:** [[numerik/exercises/12/num-exercise-12|Uebung 12]]
**PDF:** [[numerik/exercises/12/num-solution-12.pdf|num-solution-12.pdf]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Stationaere Punkte und Klassifikation|Aufgabe 1 — Stationaere Punkte und Klassifikation]]
- [[#Aufgabe 2 — Quadratische Funktion|Aufgabe 2 — Quadratische Funktion]]

---

## Aufgabe 1 — Stationaere Punkte und Klassifikation

Gegeben ist

$$f(x_1, x_2) = 1 - \frac{1}{1 + x_1^2} + \frac{1}{1 + x_2^2} + \frac{x_2^2}{4}.$$

> [!quote] Definition
> Ein Punkt $x^\ast$ heisst **stationaer**, falls $\nabla f(x^\ast) = 0$. Die Art des stationaeren Punktes ergibt sich aus der Definitheit der **Hesse-Matrix** $H_f(x^\ast)$:
> - $H_f$ positiv definit $\Rightarrow$ **lokales Minimum**,
> - $H_f$ negativ definit $\Rightarrow$ **lokales Maximum**,
> - $H_f$ indefinit (Eigenwerte mit verschiedenen Vorzeichen) $\Rightarrow$ **Sattelpunkt**.

Die Funktion ist **separabel**: sie zerfaellt in einen Anteil in $x_1$ und einen in $x_2$. Dadurch verschwinden die gemischten zweiten Ableitungen und die Hesse-Matrix ist diagonal.

### Schritt 1: Gradient

$$\frac{\partial f}{\partial x_1} = -\frac{\partial}{\partial x_1}(1 + x_1^2)^{-1} = -\big(-(1+x_1^2)^{-2}\cdot 2x_1\big) = \frac{2 x_1}{(1 + x_1^2)^2},$$

$$\frac{\partial f}{\partial x_2} = -(1+x_2^2)^{-2}\cdot 2x_2 + \frac{2 x_2}{4} = -\frac{2 x_2}{(1 + x_2^2)^2} + \frac{x_2}{2}.$$

Also

$$\nabla f(x_1, x_2) = \begin{pmatrix} \dfrac{2 x_1}{(1 + x_1^2)^2} \\[2ex] -\dfrac{2 x_2}{(1 + x_2^2)^2} + \dfrac{x_2}{2} \end{pmatrix}.$$

### Schritt 2: Stationaere Punkte ($\nabla f = 0$)

**Erste Komponente:**

$$\frac{2 x_1}{(1 + x_1^2)^2} = 0 \;\Longleftrightarrow\; x_1 = 0,$$

da der Nenner nie verschwindet.

**Zweite Komponente:**

$$-\frac{2 x_2}{(1 + x_2^2)^2} + \frac{x_2}{2} = 0 \;\Longleftrightarrow\; x_2\left(\frac{1}{2} - \frac{2}{(1 + x_2^2)^2}\right) = 0.$$

Damit ist $x_2 = 0$ **oder**

$$\frac{2}{(1 + x_2^2)^2} = \frac{1}{2} \;\Longleftrightarrow\; (1 + x_2^2)^2 = 4 \;\Longleftrightarrow\; 1 + x_2^2 = 2 \;\Longleftrightarrow\; x_2^2 = 1 \;\Longleftrightarrow\; x_2 = \pm 1.$$

(Die negative Wurzel $1 + x_2^2 = -2$ entfaellt, da $1 + x_2^2 > 0$.)

> [!tip] Merke
> Es gibt **drei** stationaere Punkte:
> $$P_0 = (0,\,0), \qquad P_1 = (0,\,1), \qquad P_2 = (0,\,-1).$$

### Schritt 3: Hesse-Matrix

Die gemischte Ableitung verschwindet ($f_{x_1 x_2} = 0$). Fuer die reinen zweiten Ableitungen berechnet man mit der Quotientenregel

$$\frac{\partial}{\partial x}\left[\frac{2 x}{(1 + x^2)^2}\right] = \frac{2(1 + x^2)^2 - 2x\cdot 2(1+x^2)\cdot 2x}{(1 + x^2)^4} = \frac{2(1 + x^2) - 8 x^2}{(1 + x^2)^3} = \frac{2 - 6 x^2}{(1 + x^2)^3}.$$

Damit

$$f_{x_1 x_1} = \frac{2 - 6 x_1^2}{(1 + x_1^2)^3}, \qquad f_{x_2 x_2} = -\frac{2 - 6 x_2^2}{(1 + x_2^2)^3} + \frac{1}{2} = \frac{6 x_2^2 - 2}{(1 + x_2^2)^3} + \frac{1}{2},$$

und

$$H_f(x_1, x_2) = \begin{pmatrix} \dfrac{2 - 6 x_1^2}{(1 + x_1^2)^3} & 0 \\[2ex] 0 & \dfrac{6 x_2^2 - 2}{(1 + x_2^2)^3} + \dfrac{1}{2} \end{pmatrix}.$$

Da $H_f$ **diagonal** ist, sind die Diagonaleintraege bereits die Eigenwerte.

### Schritt 4: Klassifikation

**Punkt $P_0 = (0,0)$:**

$$f_{x_1 x_1}(0,0) = \frac{2}{1} = 2, \qquad f_{x_2 x_2}(0,0) = \frac{-2}{1} + \frac{1}{2} = -\frac{3}{2}.$$

$$H_f(0,0) = \begin{pmatrix} 2 & 0 \\ 0 & -\tfrac{3}{2}\end{pmatrix}, \qquad \text{Eigenwerte } \lambda_1 = 2 > 0,\ \lambda_2 = -\tfrac{3}{2} < 0.$$

Verschiedene Vorzeichen $\Rightarrow$ $H_f$ **indefinit** $\Rightarrow$ **Sattelpunkt**. Funktionswert $f(0,0) = 1$.

**Punkt $P_1 = (0,1)$:** hier $x_1 = 0$, $x_2^2 = 1$, also $(1 + x_2^2)^3 = 2^3 = 8$:

$$f_{x_1 x_1} = 2, \qquad f_{x_2 x_2}(0,1) = \frac{6 - 2}{8} + \frac{1}{2} = \frac{4}{8} + \frac{1}{2} = 1.$$

$$H_f(0,1) = \begin{pmatrix} 2 & 0 \\ 0 & 1\end{pmatrix}, \qquad \lambda_1 = 2 > 0,\ \lambda_2 = 1 > 0.$$

Beide Eigenwerte positiv $\Rightarrow$ $H_f$ **positiv definit** $\Rightarrow$ **lokales Minimum**. Funktionswert $f(0,1) = \tfrac{3}{4}$.

**Punkt $P_2 = (0,-1)$:** $f$ haengt von $x_2$ nur ueber $x_2^2$ ab, ist also **gerade** in $x_2$. Deshalb ist $P_2$ voellig symmetrisch zu $P_1$:

$$H_f(0,-1) = \begin{pmatrix} 2 & 0 \\ 0 & 1\end{pmatrix}, \qquad \lambda_1 = 2 > 0,\ \lambda_2 = 1 > 0 \;\Rightarrow\; \textbf{lokales Minimum}, \quad f(0,-1) = \tfrac{3}{4}.$$

### Zusammenfassung

| Stationaerer Punkt | $H_f$ Eigenwerte | Definitheit | Typ | $f$-Wert |
|---|---|---|---|---|
| $(0,\,0)$ | $2,\ -\tfrac{3}{2}$ | indefinit | **Sattelpunkt** | $1$ |
| $(0,\,1)$ | $2,\ 1$ | positiv definit | **lokales Minimum** | $\tfrac{3}{4}$ |
| $(0,\,-1)$ | $2,\ 1$ | positiv definit | **lokales Minimum** | $\tfrac{3}{4}$ |

*(Gradient, Eigenwerte und Funktionswerte wurden mit numpy numerisch bestaetigt.)*

> [!warning] Achtung
> Ein globales Minimum existiert **nicht**: fuer $x_2 \to \pm\infty$ waechst $\frac{x_2^2}{4}$ unbeschraenkt, fuer $x_1 \to \pm\infty$ strebt der Term $-\frac{1}{1+x_1^2} \to 0$. Die beiden gefundenen Minima sind daher nur **lokal**.

---

## Aufgabe 2 — Quadratische Funktion

Gegeben $q(x) = \tfrac{1}{2} x^T A x + b^T x + c$ mit $A \in \mathbb{R}^{n\times n}$, $b \in \mathbb{R}^n$, $c \in \mathbb{R}$.

### (a) Gradient $q'(x)$

Die quadratische Form $x^T A x = \sum_{i,j} a_{ij} x_i x_j$ hat die partielle Ableitung

$$\frac{\partial}{\partial x_k}(x^T A x) = \sum_j a_{kj} x_j + \sum_i a_{ik} x_i = (A x)_k + (A^T x)_k = \big((A + A^T)x\big)_k.$$

Zusammen mit $\nabla(b^T x) = b$ und $\nabla c = 0$ folgt

$$\boxed{\,q'(x) = \tfrac{1}{2}(A + A^T)\,x + b\,.}$$

Ist $A$ symmetrisch ($A = A^T$), so vereinfacht sich dies zu $q'(x) = A x + b$.

### (b) Hesse-Matrix $q''(x)$

Ableiten von $q'(x) = \tfrac{1}{2}(A + A^T)x + b$ nach $x$ liefert die (konstante) Hesse-Matrix

$$\boxed{\,q''(x) = \tfrac{1}{2}(A + A^T)\,.}$$

Fuer symmetrisches $A$ ist $q''(x) = A$. Die Hesse-Matrix ist stets symmetrisch (Satz von Schwarz) und hier ortsunabhaengig.

### (c) $A$ darf o.B.d.A. symmetrisch angenommen werden

Setze $B := \tfrac{1}{2}(A + A^T)$. Dann ist $B$ **symmetrisch**, denn

$$B^T = \tfrac{1}{2}(A + A^T)^T = \tfrac{1}{2}(A^T + A) = B.$$

Zu zeigen ist $x^T A x = x^T B x$ fuer alle $x$. Der Ausdruck $x^T A x \in \mathbb{R}$ ist ein **Skalar** ($1\times 1$), also gleich seinem Transponierten:

$$x^T A x = (x^T A x)^T = x^T A^T x.$$

Damit

$$x^T B x = \tfrac{1}{2}\,x^T (A + A^T) x = \tfrac{1}{2}\big(x^T A x + x^T A^T x\big) = \tfrac{1}{2}\big(x^T A x + x^T A x\big) = x^T A x.$$

Da $b^T x + c$ unveraendert bleibt, gilt

$$q(x) = \tfrac{1}{2} x^T A x + b^T x + c = \tfrac{1}{2} x^T B x + b^T x + c \qquad \text{fuer alle } x.$$

Die Funktion $q$ aendert sich also nicht, wenn man $A$ durch das symmetrische $B$ ersetzt. $\quad\blacksquare$

> [!tip] Merke
> Nur der **symmetrische Anteil** $\tfrac12(A+A^T)$ einer Matrix traegt zur quadratischen Form $x^T A x$ bei; der schiefsymmetrische Anteil $\tfrac12(A-A^T)$ liefert stets $x^T(A-A^T)x = 0$. Deshalb darf man in $q$ ohne Einschraenkung $A = A^T$ annehmen.

Im Folgenden sei daher $A$ symmetrisch, sodass $q'(x) = A x + b$ und $q''(x) = A$.

### (d) $A$ positiv definit: eindeutiges lokales Minimum

Sei $A$ symmetrisch und **positiv definit**, d.h. $h^T A h > 0$ fuer alle $h \neq 0$.

**Existenz und Eindeutigkeit der Nullstelle von $q'$.**
Eine positiv definite Matrix ist **regulaer** (invertierbar): aus $A h = 0$ folgte $h^T A h = 0$, also nach der Definitheit $h = 0$; der Kern ist trivial. Somit hat

$$q'(x) = A x + b = 0 \;\Longleftrightarrow\; A x = -b$$

**genau eine** Loesung

$$\boxed{\,\bar x = -A^{-1} b\,.}$$

**$\bar x$ ist ein (striktes, sogar globales) lokales Minimum.**
Fuer beliebiges $h \in \mathbb{R}^n$ entwickelt man $q$ um $\bar x$. Mit $A = A^T$ gilt $\bar x^T A h = h^T A \bar x$, daher

$$
\begin{aligned}
q(\bar x + h) &= \tfrac{1}{2}(\bar x + h)^T A (\bar x + h) + b^T(\bar x + h) + c \\
&= \tfrac{1}{2}\bar x^T A \bar x + h^T A \bar x + \tfrac{1}{2} h^T A h + b^T \bar x + b^T h + c \\
&= \underbrace{\Big(\tfrac{1}{2}\bar x^T A \bar x + b^T \bar x + c\Big)}_{= \,q(\bar x)} + h^T \underbrace{(A \bar x + b)}_{= \,q'(\bar x) \,=\, 0} + \tfrac{1}{2} h^T A h \\
&= q(\bar x) + \tfrac{1}{2}\, h^T A h.
\end{aligned}
$$

Da $A$ positiv definit ist, gilt $\tfrac{1}{2} h^T A h > 0$ fuer alle $h \neq 0$, also

$$q(\bar x + h) > q(\bar x) \qquad \text{fuer alle } h \neq 0.$$

Damit ist $\bar x$ ein **striktes globales Minimum** von $q$ — und insbesondere ein lokales Minimum. $\quad\blacksquare$

> [!tip] Merke
> Fuer eine quadratische Funktion mit **positiv definitem** $A$ gilt die exakte Zerlegung
> $$q(\bar x + h) = q(\bar x) + \tfrac{1}{2}\,h^T A h,$$
> ohne Restglied. Die notwendige Bedingung $q'(\bar x) = 0$ ist hier zugleich **hinreichend**, und das Minimum $\bar x = -A^{-1} b$ ist eindeutig und global. Dies ist die theoretische Grundlage z.B. des CG-Verfahrens, das $\min q$ mit spd-Matrix loest.
