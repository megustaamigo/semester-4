---
course: numerik
type: solution
date: 2021-07-12
tags:
  - numerik
  - solution
  - probeklausur
  - lu-zerlegung
  - jacobi
  - gauss-seidel
  - interpolation
  - ausgleichsrechnung
  - singulaerwerte
  - gerschgorin
  - integration
  - newton-verfahren
source: exams/test-exam-2021.pdf
---

# Loesungen — Probeklausur Numerik I (12.07.2021)

**Klausur-PDF:** [[numerik/exams/test-exam-2021.pdf|test-exam-2021.pdf]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot-Suche|Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot-Suche]]
- [[#Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren|Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren]]
- [[#Aufgabe 3 — Interpolation und Ausgleichsrechnung|Aufgabe 3 — Interpolation und Ausgleichsrechnung]]
- [[#Aufgabe 4 — Singulaerwerte und Gerschgorin|Aufgabe 4 — Singulaerwerte und Gerschgorin]]
- [[#Aufgabe 5 — Numerische Integration|Aufgabe 5 — Numerische Integration]]
- [[#Aufgabe 6 — Newton-Verfahren und Kontraktion|Aufgabe 6 — Newton-Verfahren und Kontraktion]]

---

## Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot-Suche

Gesucht ist die Zerlegung $PA = LU$ der Matrix

$$A = \begin{pmatrix} 2 & 0 & 4 & 1 \\ 2 & 1 & 2 & 2 \\ 4 & 4 & 0 & 2 \\ 2 & 3 & 0 & 2 \end{pmatrix}.$$

Bei der **Spalten-Pivot-Suche** wird in jedem Schritt $k$ das betragsgroesste Element der aktuellen Spalte $k$ (in den Zeilen $k,\dots,n$) als Pivot gewaehlt und durch Zeilentausch an die Diagonale gebracht.

### Schritt 1 (Spalte 1)

Spalte 1: $(2,2,\mathbf{4},2)^T$. Betragsgroesstes Element $4$ in **Zeile 3** $\Rightarrow$ tausche Zeile 1 $\leftrightarrow$ Zeile 3:

$$\begin{pmatrix} 4 & 4 & 0 & 2 \\ 2 & 1 & 2 & 2 \\ 2 & 0 & 4 & 1 \\ 2 & 3 & 0 & 2 \end{pmatrix}.$$

Multiplikatoren $\ell_{i1} = a_{i1}/4$: $\ \ell_{21}=\tfrac12,\ \ell_{31}=\tfrac12,\ \ell_{41}=\tfrac12$. Elimination:

$$\begin{pmatrix} 4 & 4 & 0 & 2 \\ 0 & -1 & 2 & 1 \\ 0 & -2 & 4 & 0 \\ 0 & 1 & 0 & 1 \end{pmatrix}.$$

### Schritt 2 (Spalte 2)

Spalte 2, Zeilen 2–4: $(-1,\mathbf{-2},1)^T$. Betragsgroesstes Element $-2$ in **Zeile 3** $\Rightarrow$ tausche Zeile 2 $\leftrightarrow$ Zeile 3 (die bereits berechneten $L$-Eintraege der Spalte 1 werden **mitgetauscht**, hier beide $\tfrac12$, also unveraendert):

$$\begin{pmatrix} 4 & 4 & 0 & 2 \\ 0 & -2 & 4 & 0 \\ 0 & -1 & 2 & 1 \\ 0 & 1 & 0 & 1 \end{pmatrix}.$$

Multiplikatoren $\ell_{i2}=a_{i2}/(-2)$: $\ \ell_{32}=\tfrac{-1}{-2}=\tfrac12,\ \ell_{42}=\tfrac{1}{-2}=-\tfrac12$. Elimination:

$$\begin{pmatrix} 4 & 4 & 0 & 2 \\ 0 & -2 & 4 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 2 & 1 \end{pmatrix}.$$

### Schritt 3 (Spalte 3)

Spalte 3, Zeilen 3–4: $(0,\mathbf{2})^T$. Betragsgroesstes Element $2$ in **Zeile 4** $\Rightarrow$ tausche Zeile 3 $\leftrightarrow$ Zeile 4 (die $L$-Eintraege der Spalten 1,2 werden mitgetauscht: Zeile 3 $(\tfrac12,\tfrac12)$ und Zeile 4 $(\tfrac12,-\tfrac12)$ tauschen die Plaetze):

$$U = \begin{pmatrix} 4 & 4 & 0 & 2 \\ 0 & -2 & 4 & 0 \\ 0 & 0 & 2 & 1 \\ 0 & 0 & 0 & 1 \end{pmatrix}.$$

Der Multiplikator $\ell_{43}=0/2=0$; nichts mehr zu tun.

### Ergebnis

$$L = \begin{pmatrix} 1 & 0 & 0 & 0 \\ \tfrac12 & 1 & 0 & 0 \\ \tfrac12 & -\tfrac12 & 1 & 0 \\ \tfrac12 & \tfrac12 & 0 & 1 \end{pmatrix}, \qquad U = \begin{pmatrix} 4 & 4 & 0 & 2 \\ 0 & -2 & 4 & 0 \\ 0 & 0 & 2 & 1 \\ 0 & 0 & 0 & 1 \end{pmatrix}.$$

Der **Permutationsvektor** protokolliert, welche Originalzeile in welcher Position steht. Die Zeilentausche $1\leftrightarrow3$, dann $2\leftrightarrow3$, dann $3\leftrightarrow4$ ergeben (0-basiert)

$$\boxed{p = (2,\,0,\,3,\,1)} \qquad \text{bzw. 1-basiert } (3,\,1,\,4,\,2).$$

Das heisst: $PA$ ordnet die Originalzeilen in der Reihenfolge $3,1,4,2$ an.

> [!example] Probe
> $PA = LU$ wurde numerisch bestaetigt ($PA - LU = 0$). Die permutierte Matrix ist
> $$PA = \begin{pmatrix} 4 & 4 & 0 & 2 \\ 2 & 0 & 4 & 1 \\ 2 & 3 & 0 & 2 \\ 2 & 1 & 2 & 2 \end{pmatrix} = LU.$$

> [!tip] Merke
> Bei Zeilentausch in Schritt $k$ muessen die bereits berechneten $L$-Eintraege der Spalten $1,\dots,k-1$ **mitgetauscht** werden, damit $PA=LU$ gilt. Nur so bleiben Pivotisierung und Faktorisierung konsistent.

---

## Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren

$$A = \begin{pmatrix} 1 & 0 & 0 \\ 1 & 2 & 0 \\ 0 & 1 & 1 \end{pmatrix}, \qquad b = \begin{pmatrix} 1 \\ 3 \\ 2 \end{pmatrix}, \qquad x^{(0)} = \begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}.$$

Die exakte Loesung ist $x = (1,1,1)^T$ (Einsetzen: $x_1=1$, $2x_2=3-1$, $x_3=2-1$).

### (a) Iterierte

Komponentenweise lauten die Gleichungen ($a_{11}=1,\ a_{22}=2,\ a_{33}=1$):

$$x_1 = \tfrac{1}{1}\big(1\big),\quad x_2 = \tfrac{1}{2}\big(3 - x_1\big),\quad x_3 = \tfrac{1}{1}\big(2 - x_2\big).$$

**Jacobi** (alle rechten Seiten mit den **alten** Werten $x^{(k)}$):

| $k$ | $x_1$ | $x_2$ | $x_3$ | Rechnung |
|---|---|---|---|---|
| $0$ | $0$ | $0$ | $0$ | Startwert |
| $1$ | $1$ | $\tfrac{3-0}{2}=\tfrac32$ | $2-0=2$ | $x^{(1)}=(1,\tfrac32,2)^T$ |
| $2$ | $1$ | $\tfrac{3-1}{2}=1$ | $2-\tfrac32=\tfrac12$ | $x^{(2)}=(1,1,\tfrac12)^T$ |
| $3$ | $1$ | $\tfrac{3-1}{2}=1$ | $2-1=1$ | $x^{(3)}=(1,1,1)^T$ |

$$\boxed{x^{(1)}=\begin{pmatrix}1\\ \tfrac32\\ 2\end{pmatrix},\quad x^{(2)}=\begin{pmatrix}1\\ 1\\ \tfrac12\end{pmatrix},\quad x^{(3)}=\begin{pmatrix}1\\ 1\\ 1\end{pmatrix}}$$

**Gauss-Seidel** (bereits berechnete **neue** Werte $x^{(k+1)}$ sofort einsetzen):

| $k$ | $x_1$ | $x_2$ | $x_3$ | Rechnung |
|---|---|---|---|---|
| $0$ | $0$ | $0$ | $0$ | Startwert |
| $1$ | $1$ | $\tfrac{3-1}{2}=1$ | $2-1=1$ | $x^{(1)}=(1,1,1)^T$ |
| $2$ | $1$ | $1$ | $1$ | $x^{(2)}=(1,1,1)^T$ |
| $3$ | $1$ | $1$ | $1$ | $x^{(3)}=(1,1,1)^T$ |

$$\boxed{x^{(1)}=x^{(2)}=x^{(3)}=(1,1,1)^T}$$

Gauss-Seidel trifft die exakte Loesung bereits nach **einem** Schritt.

### (b) Iterationsmatrizen und Spektralradien

Zerlege $A = D + L + R$ mit $D=\operatorname{diag}(1,2,1)$, strikt unterer Teil $L$, strikt oberer Teil $R$. Hier ist $A$ **untere Dreiecksmatrix**, also $R = 0$:

$$D = \begin{pmatrix}1&0&0\\0&2&0\\0&0&1\end{pmatrix},\quad L = \begin{pmatrix}0&0&0\\1&0&0\\0&1&0\end{pmatrix},\quad R = \begin{pmatrix}0&0&0\\0&0&0\\0&0&0\end{pmatrix}.$$

**Jacobi:** $B_J = -D^{-1}(L+R) = -D^{-1}L$:

$$B_J = -\begin{pmatrix}1&0&0\\0&\tfrac12&0\\0&0&1\end{pmatrix}\begin{pmatrix}0&0&0\\1&0&0\\0&1&0\end{pmatrix} = \begin{pmatrix}0&0&0\\ -\tfrac12&0&0\\ 0&-1&0\end{pmatrix}.$$

**Gauss-Seidel:** $B_{GS} = -(D+L)^{-1}R$. Da $R=0$:

$$B_{GS} = \begin{pmatrix}0&0&0\\0&0&0\\0&0&0\end{pmatrix}.$$

**Spektralradien:** Beide Matrizen sind strikt untere Dreiecksmatrizen mit Nulldiagonale, also **nilpotent**; alle Eigenwerte sind $0$:

$$\boxed{\rho(B_J) = 0, \qquad \rho(B_{GS}) = 0.}$$

> [!tip] Merke
> Ist $A$ eine **Dreiecksmatrix**, so ist $R=0$ (untere) bzw. $L=0$ (obere) und die Gauss-Seidel-Iterationsmatrix wird zur Nullmatrix — das Verfahren ist dann ein **direktes** Verfahren (exakt nach einem Schritt). Wegen $\rho(B_J)=\rho(B_{GS})=0<1$ konvergieren beide Verfahren; die Nilpotenz garantiert Konvergenz in endlich vielen ($\le n$) Schritten.

---

## Aufgabe 3 — Interpolation und Ausgleichsrechnung

Punkte $x = (-1,0,1,2)$, $y = (-44,-11,0,55)$.

### (a) Newton-Interpolationspolynom (dividierte Differenzen)

Schema der dividierten Differenzen $d_{nm}$:

| $x_i$ | $y_i$ | 1. Ordnung | 2. Ordnung | 3. Ordnung |
|---|---|---|---|---|
| $-1$ | $-44$ | | | |
| | | $\tfrac{-11-(-44)}{0-(-1)}=33$ | | |
| $0$ | $-11$ | | $\tfrac{11-33}{1-(-1)}=-11$ | |
| | | $\tfrac{0-(-11)}{1-0}=11$ | | $\tfrac{22-(-11)}{2-(-1)}=11$ |
| $1$ | $0$ | | $\tfrac{55-11}{2-0}=22$ | |
| | | $\tfrac{55-0}{2-1}=55$ | | |
| $2$ | $55$ | | | |

Die Diagonalkoeffizienten sind $d = (-44,\ 33,\ -11,\ 11)$, also

$$\boxed{p(x) = -44 + 33(x+1) - 11(x+1)x + 11(x+1)x(x-1).}$$

Ausmultipliziert ergibt sich

$$p(x) = 11x^3 - 11x^2 + 11x - 11 = 11\,(x-1)(x^2+1).$$

> [!example] Probe
> $p(-1)=11(-2)(2)=-44$, $\ p(0)=11(-1)(1)=-11$, $\ p(1)=0$, $\ p(2)=11(1)(5)=55$ — alle Stuetzstellen exakt getroffen (numerisch bestaetigt).

### (b) Ausgleichspolynom $p(x) = a + b x^2$

Ansatz mit den Basisfunktionen $\varphi_1(x)=1$ und $\varphi_2(x)=x^2$. Mit der Designmatrix

$$M = \begin{pmatrix} 1 & x_0^2 \\ 1 & x_1^2 \\ 1 & x_2^2 \\ 1 & x_3^2 \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \\ 1 & 1 \\ 1 & 4 \end{pmatrix}$$

lauten die **Normalengleichungen** $M^T M \begin{pmatrix}a\\ b\end{pmatrix} = M^T y$. Es ist

$$M^TM = \begin{pmatrix} \sum 1 & \sum x_i^2 \\ \sum x_i^2 & \sum x_i^4 \end{pmatrix} = \begin{pmatrix} 4 & 6 \\ 6 & 18 \end{pmatrix},$$
$$M^Ty = \begin{pmatrix} \sum y_i \\ \sum x_i^2 y_i \end{pmatrix} = \begin{pmatrix} -44-11+0+55 \\ (1)(-44)+0+(1)(0)+(4)(55) \end{pmatrix} = \begin{pmatrix} 0 \\ 176 \end{pmatrix}.$$

Das System

$$\begin{pmatrix} 4 & 6 \\ 6 & 18 \end{pmatrix}\begin{pmatrix}a\\ b\end{pmatrix} = \begin{pmatrix} 0 \\ 176 \end{pmatrix}$$

hat die Determinante $\det = 4\cdot18 - 6\cdot6 = 36$. Mit Cramer:

$$a = \frac{\det\begin{pmatrix}0&6\\176&18\end{pmatrix}}{36} = \frac{0-1056}{36} = -\frac{88}{3} \approx -29.333,$$
$$b = \frac{\det\begin{pmatrix}4&0\\6&176\end{pmatrix}}{36} = \frac{704}{36} = \frac{176}{9} \approx 19.556.$$

$$\boxed{p(x) = -\frac{88}{3} + \frac{176}{9}\,x^2.}$$

> [!tip] Merke
> Beim linearen Ausgleich ist $x^2$ nur eine **feste Basisfunktion** — das Modell $a+bx^2$ ist linear in den Parametern $a,b$, daher genuegen die gewoehnlichen Normalengleichungen $M^TM\,c = M^Ty$.

---

## Aufgabe 4 — Singulaerwerte und Gerschgorin

$$A = \begin{pmatrix} 2 & -2 & 1 \\ 2 & 2 & 1 \end{pmatrix}.$$

Die Singulaerwerte $\sigma_i$ sind die Wurzeln der **positiven** Eigenwerte von $A^TA$ (bzw. $A A^T$).

### (a) Satz von Gerschgorin

$$A^TA = \begin{pmatrix} 8 & 0 & 4 \\ 0 & 8 & 0 \\ 4 & 0 & 2 \end{pmatrix}, \qquad A A^T = \begin{pmatrix} 9 & 1 \\ 1 & 9 \end{pmatrix}.$$

**Gerschgorin-Kreise von $A^TA$** (Mittelpunkt = Diagonale, Radius = Zeilenbetragssumme der Nebendiagonale):

$$K_1: |\lambda-8|\le 4 \Rightarrow [4,12], \quad K_2: |\lambda-8|\le 0 \Rightarrow \{8\}, \quad K_3: |\lambda-2|\le 4 \Rightarrow [-2,6].$$

Vereinigung: $\lambda \in [-2,12]$. Da $A^TA$ symmetrisch **positiv semidefinit** ist ($\lambda\ge0$), folgt $\lambda\in[0,12]$, also $\sigma\in[0,\sqrt{12}]$.

**Gerschgorin-Kreise von $A A^T$** (kleinere Matrix, schaerfere Schranke):

$$K_1: |\lambda-9|\le 1 \Rightarrow [8,10], \quad K_2: |\lambda-9|\le 1 \Rightarrow [8,10].$$

Also $\lambda\in[8,10]$. Da $A^TA$ und $A A^T$ **dieselben** von Null verschiedenen Eigenwerte besitzen, liegen alle positiven Eigenwerte im Schnitt beider Schranken, d.h. in $[8,10]$. Damit

$$\boxed{\sigma_i \in [\sqrt{8},\ \sqrt{10}] \approx [2.828,\ 3.162].}$$

Dies ist das kleinstmoegliche Intervall aus den Gerschgorin-Schranken (die $2\times2$-Matrix $A A^T$ liefert die engere Einschliessung).

### (b) Berechnung der Singulaerwerte

Die Eigenwerte von $A A^T = \begin{pmatrix}9&1\\1&9\end{pmatrix}$ ergeben sich aus

$$\det\!\begin{pmatrix}9-\lambda & 1\\ 1 & 9-\lambda\end{pmatrix} = (9-\lambda)^2 - 1 = 0 \;\Rightarrow\; 9-\lambda = \pm1 \;\Rightarrow\; \lambda_1 = 10,\ \lambda_2 = 8.$$

Beide liegen erwartungsgemaess in $[8,10]$. Die Singulaerwerte sind die Wurzeln:

$$\boxed{\sigma_1 = \sqrt{10} \approx 3.162, \qquad \sigma_2 = \sqrt{8} = 2\sqrt{2} \approx 2.828.}$$

(Kontrolle via $A^TA$: Eigenwerte $10, 8, 0$ — der zusaetzliche Eigenwert $0$ liefert keinen positiven Singulaerwert. Numerisch mit `np.linalg.svd` bestaetigt.)

---

## Aufgabe 5 — Numerische Integration

$$I = \int_{-3}^{3} \frac{4x^2}{x^2+3}\,dx.$$

**Exakter Wert** (zum Vergleich): Polynomdivision $\tfrac{4x^2}{x^2+3} = 4 - \tfrac{12}{x^2+3}$, also

$$I = \Big[\,4x - \tfrac{12}{\sqrt3}\arctan\tfrac{x}{\sqrt3}\,\Big]_{-3}^{3} = 24 - \tfrac{24}{\sqrt3}\arctan(\sqrt3) = 24 - \tfrac{24}{\sqrt3}\cdot\tfrac{\pi}{3} = 24 - \frac{8\pi}{\sqrt3} \approx 9.4896.$$

Der Integrand ist gerade; mit $f(x)=\tfrac{4x^2}{x^2+3}$ gilt

$$f(-3)=f(3)=\frac{36}{12}=3,\qquad f(0)=0.$$

### (a) Einfache Trapez-Regel

$$T = \frac{b-a}{2}\big(f(a)+f(b)\big) = \frac{6}{2}(3+3) = \boxed{18}.$$

### (b) Summierte Trapezregel, 2 Trapeze

Schrittweite $h=\tfrac{b-a}{2}=3$, Knoten $-3,0,3$:

$$T_2 = h\Big(\tfrac12 f(-3) + f(0) + \tfrac12 f(3)\Big) = 3\Big(\tfrac32 + 0 + \tfrac32\Big) = \boxed{9}.$$

### (c) Einfache Simpson-Regel

$$S = \frac{b-a}{6}\big(f(a) + 4f(\tfrac{a+b}{2}) + f(b)\big) = \frac{6}{6}\big(3 + 4\cdot0 + 3\big) = \boxed{6}.$$

### (d) Gauss $G_1$ (2-Punkt Gauss-Legendre)

Knoten $\pm\sqrt{\tfrac13}$, Gewichte $1,1$ auf $[-1,1]$. Substitution $x = \tfrac{b-a}{2}t + \tfrac{a+b}{2} = 3t$, $dx = 3\,dt$:

$$G_1 = 3\Big(1\cdot f(3\cdot(-\sqrt{\tfrac13})) + 1\cdot f(3\cdot\sqrt{\tfrac13})\Big).$$

Es ist $3\sqrt{\tfrac13} = \sqrt{9\cdot\tfrac13} = \sqrt{3}$, also die transformierten Knoten $\pm\sqrt3$ mit

$$f(\pm\sqrt3) = \frac{4\cdot3}{3+3} = \frac{12}{6} = 2 \;\Rightarrow\; G_1 = 3(2+2) = \boxed{12}.$$

### Vergleich

| Verfahren | Wert | Fehler $|I-\text{Naeh.}|$ |
|---|---|---|
| Exakt | $24 - \tfrac{8\pi}{\sqrt3} \approx 9.4896$ | — |
| Trapez einfach | $18$ | $8.51$ |
| Trapez summiert (2) | $9$ | $0.49$ |
| Simpson | $6$ | $3.49$ |
| Gauss $G_1$ | $12$ | $2.51$ |

> [!warning] Achtung
> Hier ist die **summierte Trapezregel** am genauesten, obwohl Simpson und Gauss formal hoehere Ordnung haben. Grund: Der Integrand hat bei $x=0$ ein Minimum mit $f(0)=0$, sodass die einfachen Ein-Intervall-Regeln (Trapez, Simpson) und Gauss die Kruemmung schlecht erfassen. Genauigkeitsaussagen ueber die Ordnung gelten **asymptotisch** (fuer feine Zerlegungen), nicht fuer ein einzelnes grobes Intervall. Alle Werte wurden numerisch bestaetigt.

---

## Aufgabe 6 — Newton-Verfahren und Kontraktion

$$f(x) = 1 - \frac{3}{x^2}, \qquad f'(x) = \frac{6}{x^3}.$$

Nullstelle in $X=[\tfrac32,2]$: $f(x)=0 \Leftrightarrow x^2=3 \Leftrightarrow x=\sqrt3\approx1.732 \in X$.

### (a) Newton-Iterationsfunktion $\Phi$

Das Newton-Verfahren lautet $x_{k+1} = \Phi(x_k)$ mit

$$\Phi(x) = x - \frac{f(x)}{f'(x)}.$$

Einsetzen und vereinfachen:

$$\Phi(x) = x - \frac{1 - \tfrac{3}{x^2}}{\tfrac{6}{x^3}} = x - \frac{x^3}{6}\Big(1 - \frac{3}{x^2}\Big) = x - \frac{x^3}{6} + \frac{x}{2}.$$

$$\boxed{\Phi(x) = \frac{3}{2}x - \frac{x^3}{6}.}$$

Ableitungen:

$$\Phi'(x) = \frac{3}{2} - \frac{x^2}{2}, \qquad \Phi''(x) = -x.$$

### (b) Kontraktion auf $X = [\tfrac32, 2]$

**Selbstabbildung** $\Phi(X)\subseteq X$: $\Phi$ hat auf $X$ das Maximum bei $x=\sqrt3$ (dort $\Phi'=0$) mit $\Phi(\sqrt3)=\sqrt3\approx1.732$; an den Raendern $\Phi(\tfrac32)=1.6875$ und $\Phi(2)=\tfrac53\approx1.667$. Also $\Phi(X)\subseteq[1.667,\,1.732]\subseteq[\tfrac32,2]$. $\checkmark$

**Kontraktionszahl:** $\Phi'(x)=\tfrac32-\tfrac{x^2}{2}$ ist auf $X$ monoton fallend (denn $\Phi''(x)=-x<0$). Damit nimmt $|\Phi'|$ seine Extremwerte an den Randpunkten an:

$$\Phi'(\tfrac32) = \frac{3}{2} - \frac{(3/2)^2}{2} = \frac{3}{2} - \frac{9/4}{2} = \frac{3}{2} - \frac{9}{8} = \frac{3}{8} = 0.375,$$
$$\Phi'(2) = \frac{3}{2} - \frac{4}{2} = \frac{3}{2} - 2 = -\frac{1}{2}.$$

Also

$$\max_{x\in X}|\Phi'(x)| = \max\Big\{\tfrac38,\ \tfrac12\Big\} = \frac{1}{2} =: \alpha.$$

Nach dem Mittelwertsatz ist $|\Phi(x)-\Phi(y)| \le \alpha\,|x-y|$ mit $\alpha=\tfrac12<1$, d.h. $\Phi$ ist eine **Kontraktion** auf $X$ mit $\alpha\le\tfrac12$. $\checkmark$ (numerisch bestaetigt: $\max_X|\Phi'|=0.5$).

> [!tip] Merke
> Der Banachsche Fixpunktsatz liefert damit: $\Phi$ besitzt in $X$ **genau einen** Fixpunkt (die Nullstelle $\sqrt3$), und die Newton-Iteration konvergiert fuer jeden Startwert $x_0\in X$ dagegen.

### (c) Konvergenzordnung fuer $f$

Bei $f$ ist die Nullstelle $\sqrt3$ **einfach** ($f'(\sqrt3)=\tfrac{6}{3\sqrt3}\ne0$). Es gilt

$$\Phi'(\sqrt3) = \frac{3}{2} - \frac{3}{2} = 0, \qquad \Phi''(\sqrt3) = -\sqrt3 \ne 0.$$

Da $\Phi'(\sqrt3)=0$, aber $\Phi''(\sqrt3)\ne0$, konvergiert das Newton-Verfahren **mindestens quadratisch** (Ordnung $p=2$).

$$\boxed{\text{Konvergenzordnung } p = 2 \text{ (quadratisch).}}$$

### (d) Ordnung fuer $\tilde f$

$$\tilde f(x) = 1 - \frac{6}{x^2} + \frac{9}{x^4} = \Big(1 - \frac{3}{x^2}\Big)^2 = f(x)^2.$$

$\tilde f$ hat also in $X$ dieselbe Nullstelle $\sqrt3$ wie $f$, aber als **doppelte** (zweifache) Nullstelle. Bei mehrfachen Nullstellen der Vielfachheit $m\ge2$ konvergiert das gewoehnliche Newton-Verfahren nur noch **linear** (mit asymptotischer Rate $1-\tfrac1m=\tfrac12$).

$$\boxed{\text{Newton fuer } \tilde f \text{ konvergiert hoechstens } \textbf{linear} \text{ (Ordnung } p=1).}$$

> [!warning] Achtung
> Eine doppelte Nullstelle zerstoert die quadratische Konvergenz: fuer $\tilde f=f^2$ ist $\tilde\Phi'(\sqrt3) = 1-\tfrac1m = \tfrac12 \ne 0$, also nur lineare Konvergenz. Abhilfe schafft das **modifizierte Newton-Verfahren** $x_{k+1}=x_k - m\,\tfrac{\tilde f}{\tilde f'}$ (mit $m=2$), das die quadratische Ordnung wiederherstellt.
