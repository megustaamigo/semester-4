---
course: numerik
type: solution
date: 2024-07-16
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
source: exams/test-exam-2024.pdf
---

# Loesungen — Probeklausur Numerik I (16.07.2024, Gedaechtnisprotokoll)

**Klausur-PDF:** [[numerik/exams/test-exam-2024.pdf|test-exam-2024.pdf]]

> [!info] Hinweis
> Diese Klausur ist ein **Gedaechtnisprotokoll** (aus dem Gedaechtnis rekonstruiert). Einzelne Angaben (Zahlenwerte, Formulierungen) koennen daher leicht von der Originalklausur abweichen. Alle unten stehenden Zahlenergebnisse wurden mit NumPy/SciPy verifiziert.

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot|Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot]]
- [[#Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren|Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren]]
- [[#Aufgabe 3 — Interpolation und Ausgleichsgerade|Aufgabe 3 — Interpolation und Ausgleichsgerade]]
- [[#Aufgabe 4 — Singulaerwerte, Gerschgorin|Aufgabe 4 — Singulaerwerte, Gerschgorin]]
- [[#Aufgabe 5 — Numerische Integration|Aufgabe 5 — Numerische Integration]]
- [[#Aufgabe 6 — Newton-Verfahren, Kontraktion, Konvergenzordnung|Aufgabe 6 — Newton-Verfahren, Kontraktion, Konvergenzordnung]]

---

## Aufgabe 1 — LU-Zerlegung mit Spalten-Pivot

Gegeben

$$A=\begin{pmatrix}0&2&0&4\\0&-1&2&6\\2&1&-1&0\\-1&-1&1&3\end{pmatrix}.$$

Gesucht: $L$, $U$ und Permutationsvektor $p$ mit $PA=LU$. Bei **Spalten-Pivot-Suche** (partielle Pivotisierung) waehlt man in jeder Spalte das betragsgroesste Element als Pivot und vertauscht die entsprechenden Zeilen.

### Schritt 1 — Spalte 1

Betraege in Spalte 1: $\{0,0,\mathbf{2},1\}$ → groesstes Element in **Zeile 3**. Vertausche Zeile 1 $\leftrightarrow$ Zeile 3.

$$\begin{pmatrix}\mathbf{2}&1&-1&0\\0&-1&2&6\\0&2&0&4\\-1&-1&1&3\end{pmatrix},\qquad p=(3,2,1,4).$$

Multiplikatoren (Pivot $=2$): $l_{21}=0,\ l_{31}=0,\ l_{41}=\dfrac{-1}{2}=-\tfrac12$.

Zeile 4 $\leftarrow$ Zeile 4 $-(-\tfrac12)\cdot$Zeile 1 $=(0,\,-\tfrac12,\,\tfrac12,\,3)$:

$$\begin{pmatrix}2&1&-1&0\\0&-1&2&6\\0&2&0&4\\0&-\tfrac12&\tfrac12&3\end{pmatrix}.$$

### Schritt 2 — Spalte 2

Betraege in Spalte 2 (ab Zeile 2): $\{1,\mathbf{2},\tfrac12\}$ → groesstes in **Zeile 3**. Vertausche Zeile 2 $\leftrightarrow$ Zeile 3 (die bereits berechneten $l$-Eintraege in Spalte 1 sind hier beide $0$, werden aber mitvertauscht).

$$p=(3,1,2,4).$$

$$\begin{pmatrix}2&1&-1&0\\0&\mathbf{2}&0&4\\0&-1&2&6\\0&-\tfrac12&\tfrac12&3\end{pmatrix}.$$

Multiplikatoren (Pivot $=2$): $l_{32}=\dfrac{-1}{2}=-\tfrac12,\ l_{42}=\dfrac{-\tfrac12}{2}=-\tfrac14$.

- Zeile 3 $\leftarrow$ Zeile 3 $-(-\tfrac12)$Zeile 2 $=(0,0,2,8)$
- Zeile 4 $\leftarrow$ Zeile 4 $-(-\tfrac14)$Zeile 2 $=(0,0,\tfrac12,4)$

$$\begin{pmatrix}2&1&-1&0\\0&2&0&4\\0&0&2&8\\0&0&\tfrac12&4\end{pmatrix}.$$

### Schritt 3 — Spalte 3

Betraege in Spalte 3 (ab Zeile 3): $\{\mathbf{2},\tfrac12\}$ → Pivot bleibt in Zeile 3, **keine Vertauschung**.

Multiplikator: $l_{43}=\dfrac{\tfrac12}{2}=\tfrac14$. Zeile 4 $\leftarrow$ Zeile 4 $-\tfrac14$Zeile 3 $=(0,0,0,2)$.

### Ergebnis

$$\boxed{\,L=\begin{pmatrix}1&0&0&0\\0&1&0&0\\0&-\tfrac12&1&0\\-\tfrac12&-\tfrac14&\tfrac14&1\end{pmatrix},\quad
U=\begin{pmatrix}2&1&-1&0\\0&2&0&4\\0&0&2&8\\0&0&0&2\end{pmatrix},\quad p=(3,1,2,4)\,}$$

Der Permutationsvektor $p=(3,1,2,4)$ bedeutet: die Zeilen von $A$ werden in der Reihenfolge $3,1,2,4$ angeordnet, also $PA=LU$ mit

$$PA=\begin{pmatrix}2&1&-1&0\\0&2&0&4\\0&-1&2&6\\-1&-1&1&3\end{pmatrix}.$$

**Probe** ($L\cdot U$, Zeile 4): $-\tfrac12(2,1,-1,0)-\tfrac14(0,2,0,4)+\tfrac14(0,0,2,8)+(0,0,0,2)=(-1,-1,1,3)$ $\checkmark$ — stimmt mit $A$-Zeile 4 ueberein. Numerisch mit `scipy.linalg.lu` bestaetigt: $PA=LU$.

> [!tip] Merke
> Bei der partiellen Pivotisierung wird in Spalte $k$ das betragsgroesste Element **auf oder unterhalb** der Diagonale gesucht. Beim Zeilentausch werden die bereits berechneten Multiplikatoren $l_{ij}$ ($j<k$) **mit** vertauscht. So gilt $|l_{ij}|\le 1$ fuer alle Eintraege von $L$.

---

## Aufgabe 2 — Jacobi- und Gauss-Seidel-Verfahren

$$A=\begin{pmatrix}1&1\\1&2\end{pmatrix},\quad b=\begin{pmatrix}2\\3\end{pmatrix},\quad x^{(0)}=\begin{pmatrix}0\\0\end{pmatrix}.$$

Exakte Loesung: $x_1+x_2=2,\ x_1+2x_2=3 \Rightarrow x^\ast=(1,1)^T$.

Zerlegung $A=D+L+R$ mit $D=\begin{pmatrix}1&0\\0&2\end{pmatrix}$, $L=\begin{pmatrix}0&0\\1&0\end{pmatrix}$, $R=\begin{pmatrix}0&1\\0&0\end{pmatrix}$.

### (a) Iterierte

**Jacobi** — $x^{(k+1)}=D^{-1}\big(b-(L+R)x^{(k)}\big)$, komponentenweise:

$$x_1^{(k+1)}=2-x_2^{(k)},\qquad x_2^{(k+1)}=\tfrac{1}{2}\big(3-x_1^{(k)}\big).$$

| $k$ | $x_1^{(k)}$ | $x_2^{(k)}$ |
|---|---|---|
| 0 | $0$ | $0$ |
| 1 | $2-0=2$ | $\tfrac12(3-0)=\tfrac32$ |
| 2 | $2-\tfrac32=\tfrac12$ | $\tfrac12(3-2)=\tfrac12$ |
| 3 | $2-\tfrac12=\tfrac32$ | $\tfrac12(3-\tfrac12)=\tfrac54$ |

$$x^{(1)}=\begin{pmatrix}2\\ \tfrac32\end{pmatrix},\quad x^{(2)}=\begin{pmatrix}\tfrac12\\ \tfrac12\end{pmatrix},\quad x^{(3)}=\begin{pmatrix}\tfrac32\\ \tfrac54\end{pmatrix}.$$

**Gauss-Seidel** — $x^{(k+1)}=(D+L)^{-1}\big(b-Rx^{(k)}\big)$, sofortige Verwendung neuer Werte:

$$x_1^{(k+1)}=2-x_2^{(k)},\qquad x_2^{(k+1)}=\tfrac12\big(3-x_1^{(k+1)}\big).$$

| $k$ | $x_1^{(k)}$ | $x_2^{(k)}$ |
|---|---|---|
| 0 | $0$ | $0$ |
| 1 | $2-0=2$ | $\tfrac12(3-2)=\tfrac12$ |
| 2 | $2-\tfrac12=\tfrac32$ | $\tfrac12(3-\tfrac32)=\tfrac34$ |
| 3 | $2-\tfrac34=\tfrac54$ | $\tfrac12(3-\tfrac54)=\tfrac78$ |

$$x^{(1)}=\begin{pmatrix}2\\ \tfrac12\end{pmatrix},\quad x^{(2)}=\begin{pmatrix}\tfrac32\\ \tfrac34\end{pmatrix},\quad x^{(3)}=\begin{pmatrix}\tfrac54\\ \tfrac78\end{pmatrix}.$$

Gauss-Seidel naehert sich sichtbar schneller an $x^\ast=(1,1)^T$.

### (b) Iterationsmatrizen und Spektralradien

**Jacobi:** $B_J=-D^{-1}(L+R)$.

$$D^{-1}=\begin{pmatrix}1&0\\0&\tfrac12\end{pmatrix},\quad L+R=\begin{pmatrix}0&1\\1&0\end{pmatrix}\ \Rightarrow\ B_J=-\begin{pmatrix}0&1\\ \tfrac12&0\end{pmatrix}=\begin{pmatrix}0&-1\\ -\tfrac12&0\end{pmatrix}.$$

Eigenwerte: $\det(B_J-\lambda I)=\lambda^2-\tfrac12=0 \Rightarrow \lambda=\pm\tfrac{1}{\sqrt2}$. Also

$$\rho(B_J)=\frac{1}{\sqrt2}\approx 0{,}7071.$$

**Gauss-Seidel:** $B_{GS}=-(D+L)^{-1}R$.

$$D+L=\begin{pmatrix}1&0\\1&2\end{pmatrix},\quad (D+L)^{-1}=\frac{1}{2}\begin{pmatrix}2&0\\-1&1\end{pmatrix}=\begin{pmatrix}1&0\\ -\tfrac12&\tfrac12\end{pmatrix},$$
$$B_{GS}=-\begin{pmatrix}1&0\\ -\tfrac12&\tfrac12\end{pmatrix}\begin{pmatrix}0&1\\0&0\end{pmatrix}=-\begin{pmatrix}0&1\\0&-\tfrac12\end{pmatrix}=\begin{pmatrix}0&-1\\0&\tfrac12\end{pmatrix}.$$

Eigenwerte (obere Dreiecksmatrix): $\lambda_1=0,\ \lambda_2=\tfrac12$. Also

$$\rho(B_{GS})=\frac12=0{,}5.$$

> [!tip] Merke
> Hier gilt $\rho(B_{GS})=\rho(B_J)^2=\big(\tfrac{1}{\sqrt2}\big)^2=\tfrac12$. Das ist typisch fuer $2\times2$-Systeme (und allgemein fuer konsistent geordnete Matrizen): Gauss-Seidel konvergiert etwa **doppelt so schnell** wie Jacobi. Beide Radien sind $<1$, also konvergieren beide Verfahren.

---

## Aufgabe 3 — Interpolation und Ausgleichsgerade

Punkte: $x=(-2,0,1,2)$, $y=(7,-7,7,35)$.

### (a) Newton-Darstellung (dividierte Differenzen)

| $x_i$ | $y_i$ | 1. Ord. | 2. Ord. | 3. Ord. |
|---|---|---|---|---|
| $-2$ | $7$ | | | |
| | | $\frac{-7-7}{0-(-2)}=-7$ | | |
| $0$ | $-7$ | | $\frac{14-(-7)}{1-(-2)}=7$ | |
| | | $\frac{7-(-7)}{1-0}=14$ | | $\frac{7-7}{2-(-2)}=0$ |
| $1$ | $7$ | | $\frac{28-14}{2-0}=7$ | |
| | | $\frac{35-7}{2-1}=28$ | | |
| $2$ | $35$ | | | |

Diagonalkoeffizienten $d=(7,\,-7,\,7,\,0)$. Da die dividierte Differenz 3. Ordnung $=0$ ist, ist das Polynom **hoechstens vom Grad 2**:

$$p(x)=7-7\,(x+2)+7\,(x+2)(x-0)+0\cdot(\dots)$$

Ausmultiplizieren:

$$p(x)=7-7x-14+7x^2+14x=\boxed{7x^2+7x-7}.$$

**Probe:** $p(-2)=28-14-7=7$, $p(0)=-7$, $p(1)=7$, $p(2)=28+14-7=35$ — alle Punkte exakt $\checkmark$ (numerisch bestaetigt: $d=(7,-7,7,0)$).

### (b) Ausgleichsgerade $p(x)=a+bx$ (lineare Least-Squares)

Normalengleichungen $A^TA\,(a,b)^T=A^Ty$ mit den Summen ($n=4$):

$$\sum x_i=1,\quad \sum x_i^2=9,\quad \sum y_i=42,\quad \sum x_iy_i=63.$$

$$\begin{pmatrix}n&\sum x_i\\ \sum x_i&\sum x_i^2\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}=\begin{pmatrix}\sum y_i\\ \sum x_iy_i\end{pmatrix}
\;\Longrightarrow\;\begin{pmatrix}4&1\\1&9\end{pmatrix}\begin{pmatrix}a\\b\end{pmatrix}=\begin{pmatrix}42\\63\end{pmatrix}.$$

Determinante $=4\cdot9-1=35$. Mit der Cramer-Regel:

$$a=\frac{42\cdot9-1\cdot63}{35}=\frac{315}{35}=9,\qquad b=\frac{4\cdot63-1\cdot42}{35}=\frac{210}{35}=6.$$

$$\boxed{p(x)=9+6x.}$$

(Numerisch mit `np.linalg.lstsq` bestaetigt: $a=9,\ b=6$.)

> [!info] Hinweis
> Die Einzelrechnungen: $\sum x_iy_i=(-2)(7)+0+1\cdot7+2\cdot35=-14+7+70=63$; $\sum y_i=7-7+7+35=42$. Bei nur zwei Parametern loest man das $2\times2$-System direkt.

---

## Aufgabe 4 — Singulaerwerte, Gerschgorin

$$A=\begin{pmatrix}1&1&0\\1&0&1\end{pmatrix}.$$

Die Singulaerwerte $\sigma_i$ sind die Wurzeln der (nichtnegativen) Eigenwerte von $A^TA$ **bzw.** $AA^T$. Beide Matrizen haben dieselben **von Null verschiedenen** Eigenwerte.

$$A^TA=\begin{pmatrix}2&1&1\\1&1&0\\1&0&1\end{pmatrix},\qquad AA^T=\begin{pmatrix}2&1\\1&2\end{pmatrix}.$$

### (a) Satz von Gerschgorin

**Auf $A^TA$** ($3\times3$): Gerschgorin-Kreise (auf der reellen Achse, da symmetrisch — Mittelpunkt $=$ Diagonalelement, Radius $=$ Summe der Betraege der uebrigen Zeileneintraege):

- Zeile 1: Mittelpunkt $2$, Radius $|1|+|1|=2$ → $[0,4]$
- Zeile 2: Mittelpunkt $1$, Radius $1$ → $[0,2]$
- Zeile 3: Mittelpunkt $1$, Radius $1$ → $[0,2]$

Vereinigung: alle Eigenwerte von $A^TA$ liegen in $[0,4]$, also $\sigma_i\in[0,2]$.

**Auf $AA^T$** ($2\times2$): beide Zeilen Mittelpunkt $2$, Radius $1$ → $[1,3]$.

Alle **von Null verschiedenen** Eigenwerte (und damit die Quadrate der Singulaerwerte) liegen in $[1,3]$:

$$\lambda\in[1,3]\ \Rightarrow\ \sigma_i=\sqrt{\lambda}\in[1,\sqrt3\,].$$

$$\boxed{\text{Kleinstes Intervall fuer alle }\sigma_i:\ [1,\ \sqrt3\,]\approx[1,\ 1{,}732].}$$

Das Intervall aus $AA^T$ ist **schaerfer** als das aus $A^TA$ ($[0,2]$), weil der Eigenwert $0$ von $A^TA$ kein Singulaerwert ($>0$) ist und $AA^T$ diesen Nulleigenwert gar nicht besitzt.

### (b) Exakte Singulaerwerte

$AA^T=\begin{pmatrix}2&1\\1&2\end{pmatrix}$ hat charakteristisches Polynom $(2-\lambda)^2-1=0 \Rightarrow \lambda=2\pm1$, also $\lambda\in\{3,1\}$.

$$\boxed{\sigma_1=\sqrt3\approx1{,}732,\qquad \sigma_2=1.}$$

(Kontrolle: $A^TA$ hat Eigenwerte $3,1,0$ → dieselben $\sigma$. `np.linalg.svd` liefert $\{1{,}7320508,\ 1\}$ $\checkmark$. Beide Singulaerwerte liegen im Intervall $[1,\sqrt3]$ aus Teil (a) $\checkmark$.)

---

## Aufgabe 5 — Numerische Integration

$$I=\int_{-3}^{3}\frac{4x^2}{x^2+3}\,dx,\qquad g(x)=\frac{4x^2}{x^2+3}.$$

Wichtige Funktionswerte: $g(-3)=g(3)=\dfrac{4\cdot9}{12}=3$, $\ g(0)=0$.

### Exakter Wert (zum Vergleich)

Polynomdivision: $\dfrac{4x^2}{x^2+3}=4-\dfrac{12}{x^2+3}$. Mit $\int\dfrac{dx}{x^2+3}=\dfrac{1}{\sqrt3}\arctan\!\frac{x}{\sqrt3}$:

$$I=\Big[4x-\tfrac{12}{\sqrt3}\arctan\tfrac{x}{\sqrt3}\Big]_{-3}^{3}=24-\frac{12}{\sqrt3}\Big(\tfrac{\pi}{3}-(-\tfrac{\pi}{3})\Big)=24-\frac{8\pi}{\sqrt3}\approx \boxed{9{,}4896}.$$

($\arctan(3/\sqrt3)=\arctan(\sqrt3)=\pi/3$. Numerisch via `scipy.integrate.quad`: $9{,}48960$ $\checkmark$.)

### (a) Einfache Trapez-Regel

$$T=\frac{b-a}{2}\big(g(a)+g(b)\big)=\frac{6}{2}(3+3)=\boxed{18}.$$

### (b) Summierte Trapezregel, 2 Trapeze

Schrittweite $h=\dfrac{6}{2}=3$, Knoten $-3,0,3$:

$$T_2=\frac{h}{2}\big(g(-3)+2g(0)+g(3)\big)=\frac{3}{2}(3+0+3)=\boxed{9}.$$

### (c) Einfache Simpson-Regel

$$S=\frac{b-a}{6}\big(g(a)+4g(m)+g(b)\big)=\frac{6}{6}\big(3+4\cdot0+3\big)=\boxed{6}.$$

### (d) Gauss $G_1$ (2-Punkt)

Transformation von $[-1,1]$ auf $[-3,3]$: $x=\dfrac{a+b}{2}+\dfrac{b-a}{2}t=3t$, $dx=3\,dt$. Knoten $t=\pm\sqrt{\tfrac13}$, Gewichte $1,1$:

$$G_1=3\Big[g\big(3\cdot(-\tfrac{1}{\sqrt3})\big)+g\big(3\cdot\tfrac{1}{\sqrt3}\big)\Big]=3\big[g(-\sqrt3)+g(\sqrt3)\big].$$

Mit $g(\pm\sqrt3)=\dfrac{4\cdot3}{3+3}=\dfrac{12}{6}=2$:

$$G_1=3\,(2+2)=\boxed{12}.$$

### Vergleich

| Verfahren | Naeherung | Fehler zu $I\approx9{,}490$ |
|---|---|---|
| Trapez (einfach) | $18$ | $+8{,}51$ |
| Trapez (2 Trapeze) | $9$ | $-0{,}49$ |
| Simpson (einfach) | $6$ | $-3{,}49$ |
| Gauss $G_1$ | $12$ | $+2{,}51$ |
| **exakt** | $24-\tfrac{8\pi}{\sqrt3}\approx 9{,}490$ | — |

> [!warning] Achtung
> Der Integrand ist auf $[-3,3]$ stark gekruemmt (Minimum bei $0$, symmetrische Anstiege zu den Raendern). Deshalb liegen die einfachen Ein-Intervall-Regeln (Trapez, Simpson, Gauss) hier noch weit vom exakten Wert entfernt; erst die summierte Trapezregel trifft zufaellig gut. Alle Werte numerisch bestaetigt.

---

## Aufgabe 6 — Newton-Verfahren, Kontraktion, Konvergenzordnung

$$f(x)=1-\frac{1}{x^2},\qquad f'(x)=\frac{2}{x^3}.$$

Nullstellen: $1-\tfrac{1}{x^2}=0\Rightarrow x^2=1\Rightarrow x=\pm1$. In $X=[\tfrac23,1]$ liegt die einfache Nullstelle $x^\ast=1$.

### (a) Iterationsfunktion, Ableitungen

$$\Phi(x)=x-\frac{f(x)}{f'(x)}=x-\frac{1-\tfrac{1}{x^2}}{\tfrac{2}{x^3}}=x-\frac{(1-\tfrac{1}{x^2})\,x^3}{2}=x-\frac{x^3-x}{2}.$$

$$\boxed{\Phi(x)=\frac{3x-x^3}{2}.}$$

$$\Phi'(x)=\frac{3-3x^2}{2}=\frac{3}{2}\,(1-x^2),\qquad \Phi''(x)=-3x.$$

Kontrolle: $\Phi(1)=\tfrac{3-1}{2}=1=x^\ast$ (Fixpunkt $\checkmark$), $\Phi'(1)=\tfrac32(1-1)=0$.

### (b) Kontraktion auf $X=[\tfrac23,1]$

**Selbstabbildung:** Auf $X$ ist $\Phi'(x)=\tfrac32(1-x^2)\ge0$, also $\Phi$ monoton wachsend. Daher

$$\Phi(X)=\big[\Phi(\tfrac23),\,\Phi(1)\big]=\Big[\tfrac{3\cdot\frac23-(\frac23)^3}{2},\,1\Big]=\Big[\tfrac{23}{27},\,1\Big]\approx[0{,}852,\,1]\subset\Big[\tfrac23,1\Big].$$

Also bildet $\Phi$ die Menge $X$ in sich ab.

**Kontraktionszahl:** Auf $X$ ist $x^2\in[\tfrac49,1]$, somit $1-x^2\in[0,\tfrac59]$ und

$$|\Phi'(x)|=\frac{3}{2}\,(1-x^2)\le \frac{3}{2}\cdot\frac{5}{9}=\frac{15}{18}=\frac{5}{6}.$$

Das Maximum wird am linken Rand $x=\tfrac23$ angenommen: $|\Phi'(\tfrac23)|=\tfrac56$. Nach dem Mittelwertsatz ist $\Phi$ eine **Kontraktion mit $\alpha=\tfrac56<1$** $\checkmark$ (numerisch: $\max_{X}|\Phi'|=0{,}8333=\tfrac56$).

Nach dem Banachschen Fixpunktsatz konvergiert die Newton-Iteration fuer jeden Startwert $x_0\in X$ gegen $x^\ast=1$.

### (c) Konvergenzordnung fuer $x_0\in X$ (mindestens)

$x^\ast=1$ ist eine **einfache** Nullstelle von $f$ (da $f'(1)=2\neq0$), und $f$ ist dort hinreichend glatt. Fuer einfache Nullstellen konvergiert das Newton-Verfahren **mindestens quadratisch**:

$$\boxed{\text{Ordnung} \ge 2 \ \text{(quadratisch).}}$$

Grund: $\Phi'(x^\ast)=\Phi'(1)=0$, aber $\Phi''(1)=-3\neq0$, also genau Ordnung $2$.

### (d) Newton fuer $\tilde f$ — hoechste Ordnung

$$\tilde f(x)=1-\frac{2}{x^2}+\frac{1}{x^4}=\Big(1-\frac{1}{x^2}\Big)^2=f(x)^2.$$

$\tilde f$ hat in $X$ dieselbe Nullstelle $x^\ast=1$, aber als **doppelte** Nullstelle (Vielfachheit $m=2$): $\tilde f(1)=0$ und $\tilde f'(1)=2f(1)f'(1)=0$.

Fuer Nullstellen der Vielfachheit $m\ge2$ konvergiert das gewoehnliche Newton-Verfahren nur noch **linear** (mit asymptotischer Fehlerkonstante $1-\tfrac1m=\tfrac12$):

$$\boxed{\text{Ordnung hoechstens } 1 \ \text{(linear).}}$$

> [!tip] Merke
> Die Konvergenzordnung von Newton haengt an der **Vielfachheit** der Nullstelle: einfache Nullstelle → quadratisch, $m$-fache Nullstelle → nur linear. Da $\tilde f=f^2$ die Nullstelle von $f$ zur doppelten Nullstelle macht, verliert Newton die quadratische Konvergenz. (Mit dem modifizierten Newton-Verfahren $x_{k+1}=x_k-m\,\tilde f/\tilde f'$ liesse sich die quadratische Ordnung wiederherstellen.)
