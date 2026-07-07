---
course: numerik
type: solution
date: 2012-07-04
tags:
  - numerik
  - solution
  - probeklausur
  - tikhonov
  - singulaerwerte
  - gauss-seidel
  - newton-verfahren
  - sekantenverfahren
  - interpolation
  - lagrange
  - ausgleichsrechnung
  - integration
source: exams/test-exam-2012.pdf
---

# Loesungen — Probeklausur Numerik I (04.07.2012, SS 2012)

**Klausur-PDF:** [[numerik/exams/test-exam-2012.pdf|test-exam-2012.pdf]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Tikhonov und Singulaerwerte|Aufgabe 1 — Tikhonov und Singulaerwerte]]
- [[#Aufgabe 2 — Gauss-Seidel-Verfahren|Aufgabe 2 — Gauss-Seidel-Verfahren]]
- [[#Aufgabe 3 — Newton- und Sekantenverfahren|Aufgabe 3 — Newton- und Sekantenverfahren]]
- [[#Aufgabe 4 — Interpolation und Approximation|Aufgabe 4 — Interpolation und Approximation]]
- [[#Aufgabe 5 — Simpson, Trapez und Gauss-Integration|Aufgabe 5 — Simpson, Trapez und Gauss-Integration]]

---

## Aufgabe 1 — Tikhonov und Singulaerwerte

Gegeben ist die Matrix

$$A = \begin{pmatrix} 0 & 1 & 1 \\ 1 & 0 & 1 \end{pmatrix}.$$

### (a) Tikhonov-Regularisierung fuer $\alpha = 1$, $b = (4,4)^T$

Die Tikhonov-regularisierte Loesung $x_\alpha$ loest das (stets eindeutig loesbare) System

$$(A^T A + \alpha I)\,x_\alpha = A^T b.$$

Zuerst die Bausteine berechnen. Es ist

$$A^T A = \begin{pmatrix} 0 & 1 \\ 1 & 0 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 0 & 1 & 1 \\ 1 & 0 & 1 \end{pmatrix} = \begin{pmatrix} 1 & 0 & 1 \\ 0 & 1 & 1 \\ 1 & 1 & 2 \end{pmatrix},$$

$$A^T b = \begin{pmatrix} 0 & 1 \\ 1 & 0 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 4 \\ 4 \end{pmatrix} = \begin{pmatrix} 4 \\ 4 \\ 8 \end{pmatrix}.$$

Mit $\alpha = 1$ folgt das Gleichungssystem

$$\underbrace{\begin{pmatrix} 2 & 0 & 1 \\ 0 & 2 & 1 \\ 1 & 1 & 3 \end{pmatrix}}_{A^TA + I}\, x_\alpha = \begin{pmatrix} 4 \\ 4 \\ 8 \end{pmatrix}.$$

Aus den ersten beiden Zeilen: $2x_1 + x_3 = 4$ und $2x_2 + x_3 = 4$, also $x_1 = x_2 = \tfrac{4 - x_3}{2}$. Einsetzen in die dritte Zeile $x_1 + x_2 + 3x_3 = 8$:

$$2\cdot\frac{4 - x_3}{2} + 3x_3 = 8 \;\Longrightarrow\; 4 - x_3 + 3x_3 = 8 \;\Longrightarrow\; 2x_3 = 4 \;\Longrightarrow\; x_3 = 2.$$

Damit $x_1 = x_2 = \tfrac{4-2}{2} = 1$. Also

$$\boxed{x_\alpha = \begin{pmatrix} 1 \\ 1 \\ 2 \end{pmatrix}.}$$

**Probe:** $(A^TA+I)x_\alpha = (2+2,\ 2+2,\ 1+1+6)^T = (4,4,8)^T$ $\checkmark$ (numerisch bestaetigt).

### (b) Singulaerwerte von $A$

Die Singulaerwerte $\sigma_i > 0$ sind die Wurzeln der positiven Eigenwerte von $A A^T$ (bzw. $A^T A$). Da $A$ eine $2\times 3$-Matrix mit vollem Zeilenrang $2$ ist, gibt es genau **zwei** positive Singulaerwerte. Am einfachsten ueber die kleinere Matrix $A A^T \in \mathbb{R}^{2\times 2}$:

$$A A^T = \begin{pmatrix} 0 & 1 & 1 \\ 1 & 0 & 1 \end{pmatrix} \begin{pmatrix} 0 & 1 \\ 1 & 0 \\ 1 & 1 \end{pmatrix} = \begin{pmatrix} 2 & 1 \\ 1 & 2 \end{pmatrix}.$$

Charakteristisches Polynom:

$$\det(AA^T - \lambda I) = (2-\lambda)^2 - 1 = \lambda^2 - 4\lambda + 3 = (\lambda - 3)(\lambda - 1) = 0.$$

Die Eigenwerte sind $\lambda_1 = 3$, $\lambda_2 = 1$. Damit

$$\boxed{\sigma_1 = \sqrt{3} \approx 1{,}7320508, \qquad \sigma_2 = \sqrt{1} = 1.}$$

(Numerisch bestaetigt via `np.linalg.svd`: Singulaerwerte $\{1{,}7320508,\ 1\}$.)

> [!tip] Merke
> Fuer eine $m\times n$-Matrix hat man $\min(m,n)$ Singulaerwerte. Man rechnet ueber die **kleinere** der beiden Gram-Matrizen $A A^T$ oder $A^T A$ — sie haben dieselben positiven Eigenwerte, und $\sigma_i = \sqrt{\lambda_i}$.

---

## Aufgabe 2 — Gauss-Seidel-Verfahren

Gegeben $A x = b$ mit

$$A = \begin{pmatrix} 4 & 4 \\ -1 & 2 \end{pmatrix}, \qquad b = \begin{pmatrix} 0 \\ 3 \end{pmatrix}.$$

### (a) Iterierte $x_1, \dots, x_4$ (Startwert $x_0 = (0,0)^T$)

Beim Gauss-Seidel-Verfahren werden bereits berechnete neue Komponenten sofort weiterverwendet. Die komponentenweisen Updates lauten

$$x_1^{(k+1)} = \frac{1}{4}\Big(0 - 4\,x_2^{(k)}\Big) = -x_2^{(k)}, \qquad x_2^{(k+1)} = \frac{1}{2}\Big(3 + x_1^{(k+1)}\Big).$$

Iteration:

| $k$ | $x_1^{(k)}$ | $x_2^{(k)}$ |
|---|---|---|
| $0$ | $0$ | $0$ |
| $1$ | $-x_2^{(0)} = 0$ | $\tfrac{1}{2}(3+0) = 1{,}5$ |
| $2$ | $-1{,}5$ | $\tfrac{1}{2}(3-1{,}5) = 0{,}75$ |
| $3$ | $-0{,}75$ | $\tfrac{1}{2}(3-0{,}75) = 1{,}125$ |
| $4$ | $-1{,}125$ | $\tfrac{1}{2}(3-1{,}125) = 0{,}9375$ |

$$\boxed{x_1 = \begin{pmatrix} 0 \\ 1{,}5 \end{pmatrix},\ x_2 = \begin{pmatrix} -1{,}5 \\ 0{,}75 \end{pmatrix},\ x_3 = \begin{pmatrix} -0{,}75 \\ 1{,}125 \end{pmatrix},\ x_4 = \begin{pmatrix} -1{,}125 \\ 0{,}9375 \end{pmatrix}.}$$

Die exakte Loesung ist $x = (-1,\ 1)^T$; die Iterierten naehern sich ihr (numerisch bestaetigt).

### (b) Iterationsmatrix, Norm, Spektralradius, Konvergenz

Zerlege $A = D + L + R$ mit

$$D + L = \begin{pmatrix} 4 & 0 \\ -1 & 2 \end{pmatrix}, \qquad R = \begin{pmatrix} 0 & 4 \\ 0 & 0 \end{pmatrix}.$$

Die Gauss-Seidel-Iterationsmatrix ist $B_{GS} = -(D+L)^{-1} R$. Zunaechst

$$(D+L)^{-1} = \frac{1}{\det}\begin{pmatrix} 2 & 0 \\ 1 & 4 \end{pmatrix} = \frac{1}{8}\begin{pmatrix} 2 & 0 \\ 1 & 4 \end{pmatrix}, \qquad \det(D+L) = 8.$$

Damit

$$B_{GS} = -\frac{1}{8}\begin{pmatrix} 2 & 0 \\ 1 & 4 \end{pmatrix}\begin{pmatrix} 0 & 4 \\ 0 & 0 \end{pmatrix} = -\frac{1}{8}\begin{pmatrix} 0 & 8 \\ 0 & 4 \end{pmatrix} = \begin{pmatrix} 0 & -1 \\ 0 & -\tfrac{1}{2} \end{pmatrix}.$$

$$\boxed{B_{GS} = \begin{pmatrix} 0 & -1 \\ 0 & -\tfrac{1}{2} \end{pmatrix}.}$$

**Zeilensummennorm:** $\|B_{GS}\|_\infty = \max(\,|0|+|-1|,\ |0|+|-\tfrac12|\,) = \max(1,\ \tfrac12) = 1.$

**Spektralradius:** Da $B_{GS}$ obere Dreiecksgestalt hat, sind die Eigenwerte die Diagonaleintraege $\lambda_1 = 0$, $\lambda_2 = -\tfrac12$. Also

$$\rho(B_{GS}) = \max(|0|,\ |{-}\tfrac12|) = \tfrac{1}{2}.$$

> [!tip] Merke
> Das Verfahren **konvergiert**, denn $\rho(B_{GS}) = \tfrac{1}{2} < 1$. Der Spektralradius ist das entscheidende Konvergenzkriterium — nicht die Norm. Hier ist $\|B_{GS}\|_\infty = 1$ nicht $< 1$, liefert also **kein** hinreichendes Kriterium; erst $\rho < 1$ sichert die Konvergenz. (Der Faktor $\approx \tfrac12$ pro Schritt ist auch in Teil (a) sichtbar: der Fehler halbiert sich je Iteration.)

---

## Aufgabe 3 — Newton- und Sekantenverfahren

$$f(x) = \ln(x) - \cos(x), \qquad f'(x) = \frac{1}{x} + \sin(x).$$

Iteriert wird, bis sich die ersten **6 fuehrenden Dezimalziffern** nicht mehr aendern.

### (a) Newton-Verfahren, $x_0 = 1$

$$x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)} = x_k - \frac{\ln(x_k) - \cos(x_k)}{\tfrac{1}{x_k} + \sin(x_k)}.$$

Startwert: $f(1) = \ln 1 - \cos 1 = -0{,}540302$, $\ f'(1) = 1 + \sin 1 = 1{,}841471$.

| $k$ | $x_k$ | $f(x_k)$ |
|---|---|---|
| $0$ | $1{,}0000000$ | $-5{,}40\cdot 10^{-1}$ |
| $1$ | $1{,}2934080$ | $-1{,}66\cdot 10^{-2}$ |
| $2$ | $1{,}3029555$ | $-1{,}48\cdot 10^{-5}$ |
| $3$ | $1{,}3029640$ | $-1{,}18\cdot 10^{-11}$ |
| $4$ | $1{,}3029640$ | $\approx 0$ |

Ab $x_3$ sind die ersten 6 Dezimalziffern ($302964$) stabil ($x_3 = x_4 = 1{,}302964$). Newton konvergiert **quadratisch** (Fehlerexponent verdoppelt sich je Schritt).

$$\boxed{x^\ast \approx 1{,}302964.}$$

### (b) Sekantenverfahren, $x_{-2} = 1$, $x_{-1} = 2$

$$x_{k+1} = x_k - f(x_k)\,\frac{x_k - x_{k-1}}{f(x_k) - f(x_{k-1})}.$$

Mit $f(1) = -0{,}540302$ und $f(2) = \ln 2 - \cos 2 = 0{,}693147 + 0{,}416147 = 1{,}109294$:

| $k$ | $x_k$ | $f(x_k)$ |
|---|---|---|
| $-2$ | $1{,}0000000$ | $-5{,}40\cdot 10^{-1}$ |
| $-1$ | $2{,}0000000$ | $\phantom{-}1{,}11\cdot 10^{0}$ |
| $0$ | $1{,}3275361$ | $\phantom{-}4{,}25\cdot 10^{-2}$ |
| $1$ | $1{,}3007743$ | $-3{,}79\cdot 10^{-3}$ |
| $2$ | $1{,}3029691$ | $\phantom{-}8{,}76\cdot 10^{-6}$ |
| $3$ | $1{,}3029640$ | $\phantom{-}1{,}80\cdot 10^{-9}$ |
| $4$ | $1{,}3029640$ | $\approx 0$ |

Ab $x_3$ sind die ersten 6 Dezimalziffern ($302964$) stabil ($x_3 = x_4 = 1{,}302964$).

$$\boxed{x^\ast \approx 1{,}302964.}$$

> [!tip] Merke
> Beide Verfahren liefern **dieselbe** Nullstelle $x^\ast \approx 1{,}302964$. Newton braucht die Ableitung, konvergiert dafuer quadratisch; das Sekantenverfahren approximiert die Tangente durch eine Sekante (keine Ableitung noetig) und konvergiert mit Ordnung $\approx 1{,}618$ etwas langsamer. Beide sind hier praktisch gleich schnell (Genauigkeit nach ~3–4 Schritten). Alle Werte numerisch bestaetigt.

---

## Aufgabe 4 — Interpolation und Approximation

Punkte $x = (-1, 0, 1)$, $y = (4, 5, 10)$.

### (a) Interpolationspolynom mit Lagrange-Polynomen

$$p(x) = \sum_{i=0}^{2} y_i\, L_i(x), \qquad L_i(x) = \prod_{j\neq i} \frac{x - x_j}{x_i - x_j}.$$

Die drei Basispolynome:

$$L_0(x) = \frac{(x-0)(x-1)}{(-1-0)(-1-1)} = \frac{x(x-1)}{2},$$
$$L_1(x) = \frac{(x+1)(x-1)}{(0+1)(0-1)} = \frac{(x+1)(x-1)}{-1} = -(x^2 - 1) = 1 - x^2,$$
$$L_2(x) = \frac{(x+1)(x-0)}{(1+1)(1-0)} = \frac{x(x+1)}{2}.$$

Damit

$$p(x) = 4\cdot\frac{x(x-1)}{2} + 5\,(1 - x^2) + 10\cdot\frac{x(x+1)}{2}.$$

Ausmultiplizieren:

$$= 2(x^2 - x) + 5 - 5x^2 + 5(x^2 + x) = 2x^2 - 2x + 5 - 5x^2 + 5x^2 + 5x = 2x^2 + 3x + 5.$$

$$\boxed{p(x) = 2x^2 + 3x + 5.}$$

**Probe:** $p(-1) = 2-3+5 = 4$, $\ p(0) = 5$, $\ p(1) = 2+3+5 = 10$ $\checkmark$ (numerisch bestaetigt).

### (b) Lineares Ausgleichspolynom $p(x) = a + bx$ (Normalengleichungen)

Ansatzmatrix und Normalengleichungen $A^T A\,\binom{a}{b} = A^T y$ mit

$$A = \begin{pmatrix} 1 & -1 \\ 1 & 0 \\ 1 & 1 \end{pmatrix}, \qquad y = \begin{pmatrix} 4 \\ 5 \\ 10 \end{pmatrix}.$$

Es gilt (da $\sum x_i = 0$ ist das System entkoppelt):

$$A^T A = \begin{pmatrix} \sum 1 & \sum x_i \\ \sum x_i & \sum x_i^2 \end{pmatrix} = \begin{pmatrix} 3 & 0 \\ 0 & 2 \end{pmatrix}, \qquad A^T y = \begin{pmatrix} \sum y_i \\ \sum x_i y_i \end{pmatrix} = \begin{pmatrix} 19 \\ 6 \end{pmatrix},$$

wobei $\sum y_i = 4+5+10 = 19$ und $\sum x_i y_i = (-1)\cdot 4 + 0\cdot 5 + 1\cdot 10 = 6$. Also

$$3a = 19 \Rightarrow a = \frac{19}{3} \approx 6{,}3333, \qquad 2b = 6 \Rightarrow b = 3.$$

$$\boxed{p(x) = \frac{19}{3} + 3x \approx 6{,}3333 + 3x.}$$

> [!tip] Merke
> Weil die Stuetzstellen symmetrisch um $0$ liegen ($\sum x_i = 0$), wird $A^T A$ diagonal und die Normalengleichungen entkoppeln in zwei skalare Gleichungen. Die Ausgleichsgerade geht durch den Schwerpunkt $(\bar x, \bar y) = (0,\ \tfrac{19}{3})$. (Numerisch bestaetigt.)

---

## Aufgabe 5 — Simpson, Trapez und Gauss-Integration

$$I = \int_1^4 \frac{1}{x}\,dx.$$

### (a) Exakter Wert

$$I = \big[\ln x\big]_1^4 = \ln 4 - \ln 1 = \ln 4 \approx 1{,}3862944.$$

### (b) Einfache Simpson-Regel

Mit $a = 1$, $b = 4$, Mittelpunkt $m = \tfrac{a+b}{2} = 2{,}5$ und $f(x) = \tfrac1x$:

$$S = \frac{b-a}{6}\Big(f(a) + 4 f(m) + f(b)\Big) = \frac{3}{6}\Big(1 + 4\cdot\tfrac{1}{2{,}5} + \tfrac14\Big) = \frac{1}{2}\Big(1 + 1{,}6 + 0{,}25\Big) = \frac{2{,}85}{2} = 1{,}425.$$

$$\boxed{S = 1{,}425.}$$

**Relativer Fehler:** $\displaystyle \frac{|S - I|}{I} = \frac{|1{,}425 - 1{,}3862944|}{1{,}3862944} \approx 0{,}02792 = 2{,}79\,\%.$

### (c) Summierte Trapezregel mit 2 Trapezen

Zwei Trapeze auf $[1,4]$ mit Schrittweite $h = \tfrac{4-1}{2} = 1{,}5$, Knoten $1;\ 2{,}5;\ 4$:

$$T = h\Big(\tfrac12 f(1) + f(2{,}5) + \tfrac12 f(4)\Big) = 1{,}5\Big(\tfrac12\cdot 1 + 0{,}4 + \tfrac12\cdot\tfrac14\Big) = 1{,}5\,(0{,}5 + 0{,}4 + 0{,}125) = 1{,}5\cdot 1{,}025 = 1{,}5375.$$

$$\boxed{T = 1{,}5375.}$$

**Relativer Fehler:** $\displaystyle \frac{|T - I|}{I} = \frac{|1{,}5375 - 1{,}3862944|}{1{,}3862944} \approx 0{,}10907 = 10{,}91\,\%.$

### (d) Summierte Gauss-Regel $G_0$ auf 3 Teilintervallen

$G_0$ ist die 1-Punkt-Gauss-Formel: auf dem Standardintervall $[-1,1]$ Knoten $0$ mit Gewicht $2$. Transformiert auf ein Teilintervall $[c,d]$ der Laenge $d-c$ ergibt das die **Mittelpunktregel** $(d-c)\, f\!\big(\tfrac{c+d}{2}\big)$.

Drei gleich grosse Teilintervalle: $[1,2]$, $[2,3]$, $[3,4]$ (jeweils Laenge $1$) mit Mittelpunkten $1{,}5;\ 2{,}5;\ 3{,}5$:

$$G = 1\cdot f(1{,}5) + 1\cdot f(2{,}5) + 1\cdot f(3{,}5) = \frac{1}{1{,}5} + \frac{1}{2{,}5} + \frac{1}{3{,}5} = \frac{2}{3} + \frac{2}{5} + \frac{2}{7}.$$

Gemeinsamer Nenner $105$: $\ \tfrac{70}{105} + \tfrac{42}{105} + \tfrac{30}{105} = \tfrac{142}{105} \approx 1{,}3523810.$

$$\boxed{G = \frac{142}{105} \approx 1{,}3523810.}$$

**Relativer Fehler:** $\displaystyle \frac{|G - I|}{I} = \frac{|1{,}3523810 - 1{,}3862944|}{1{,}3862944} \approx 0{,}02446 = 2{,}45\,\%.$

### Vergleich der relativen Fehler

| Verfahren | Naeherung | relativer Fehler |
|---|---|---|
| (b) Simpson (einfach) | $1{,}425$ | $2{,}79\,\%$ |
| (c) Trapez (2 Trapeze) | $1{,}5375$ | $10{,}91\,\%$ |
| (d) Gauss $G_0$ (3 Teilintervalle) | $1{,}3523810$ | $2{,}45\,\%$ |

> [!tip] Merke
> Die **summierte Gauss-Regel** $G_0$ mit 3 Knoten ist hier am genauesten ($2{,}45\,\%$), gefolgt von Simpson ($2{,}79\,\%$); die Trapezregel ist mit Abstand am schlechtesten ($10{,}91\,\%$). Gauss-Formeln erreichen bei gleicher Knotenzahl den hoechsten Exaktheitsgrad — $G_0$ (Mittelpunktregel) integriert lineare Polynome exakt, die Trapezregel ebenfalls, aber Gauss verteilt die Auswertungen guenstiger. Alle Werte numerisch bestaetigt.
