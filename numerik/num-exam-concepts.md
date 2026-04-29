---
course: numerik
type: resource
date: 2026-04-16
tags:
  - numerik
  - exam
  - lineare-algebra
  - zerlegung
---

# Exam Concepts — Numerik

## Inhaltsverzeichnis

1. [[#1. LU-Zerlegung (Gauß-Elimination)|LU-Zerlegung (Gauß-Elimination)]]
2. [[#2. LU-Zerlegung mit Pivotsuche (Spalten-Pivotisierung)|LU-Zerlegung mit Pivotsuche]]
3. [[#3. Nachiteration (Iterative Refinement)|Nachiteration]]
4. [[#4. Cholesky-Zerlegung|Cholesky-Zerlegung]]
5. [[#5. Kondition einer Matrix|Kondition einer Matrix]]
6. [[#6. Prager und Oettli (Stoerungsanalyse)|Prager und Oettli]]
7. [[#7. Jacobi-Verfahren|Jacobi-Verfahren]]
8. [[#8. Gauss-Seidel-Verfahren|Gauss-Seidel-Verfahren]]
9. [[#Zusammenfassung|Zusammenfassung]]

---

## 1. LU-Zerlegung (Gauß-Elimination)

### Idee

Jede reguläre Matrix $A \in \mathbb{R}^{n \times n}$ lässt sich (unter bestimmten Voraussetzungen) in ein Produkt aus einer **unteren Dreiecksmatrix** $L$ (lower) und einer **oberen Dreiecksmatrix** $U$ (upper) zerlegen:

$$A = L \cdot U$$

wobei:
- $L$ ist eine untere Dreiecksmatrix mit **Einsen auf der Diagonale** ($l_{ii} = 1$)
- $U$ ist eine obere Dreiecksmatrix

### Warum?

Statt $Ax = b$ direkt zu lösen, zerlegen wir in zwei **einfach lösbare** Dreieckssysteme:

1. **Vorwärtssubstitution:** $Ly = b$ → löse nach $y$
2. **Rückwärtssubstitution:** $Ux = y$ → löse nach $x$

Das ist besonders effizient, wenn man dasselbe $A$ mit verschiedenen rechten Seiten $b$ lösen will — die Zerlegung muss nur **einmal** berechnet werden.

### Algorithmus

Die LU-Zerlegung basiert auf der **Gauß-Elimination**. Man eliminiert spaltenweise die Einträge unterhalb der Diagonale:

Für jede Spalte $k = 1, \dots, n-1$:
- Für jede Zeile $i = k+1, \dots, n$:
  - Berechne den Multiplikator: $l_{ik} = \dfrac{a_{ik}^{(k)}}{a_{kk}^{(k)}}$
  - Aktualisiere die Zeile: $a_{ij}^{(k+1)} = a_{ij}^{(k)} - l_{ik} \cdot a_{kj}^{(k)}$ für $j = k, \dots, n$

Nach Abschluss steht $U$ in der transformierten Matrix, und die Multiplikatoren $l_{ik}$ bilden $L$.

### Voraussetzung

Alle **Hauptabschnittsminoren** von $A$ müssen ungleich Null sein, d.h. $\det(A_k) \neq 0$ für $k = 1, \dots, n$, wobei $A_k$ die führende $k \times k$-Untermatrix ist. Insbesondere darf kein Pivotelement $a_{kk}^{(k)} = 0$ auftreten.

### Aufwand

Die LU-Zerlegung benötigt $\dfrac{2}{3}n^3 + \mathcal{O}(n^2)$ Operationen (Multiplikationen und Additionen).

### Beispiel

Gegeben:

$$A = \begin{pmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{pmatrix}$$

**Schritt 1:** Elimination in Spalte 1

Multiplikatoren:
- $l_{21} = \dfrac{4}{2} = 2$
- $l_{31} = \dfrac{8}{2} = 4$

$$A^{(1)} = \begin{pmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 3 & 5 \end{pmatrix}$$

**Schritt 2:** Elimination in Spalte 2

Multiplikator:
- $l_{32} = \dfrac{3}{1} = 3$

$$U = \begin{pmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{pmatrix}$$

**Ergebnis:**

$$L = \begin{pmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 4 & 3 & 1 \end{pmatrix}, \quad U = \begin{pmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{pmatrix}$$

**Probe:** $L \cdot U = \begin{pmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{pmatrix} = A$ ✔

### Lösen eines Gleichungssystems

Sei $b = \begin{pmatrix} 4 \\ 10 \\ 24 \end{pmatrix}$.

**Vorwärtssubstitution** $Ly = b$:

$$\begin{pmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 4 & 3 & 1 \end{pmatrix} \begin{pmatrix} y_1 \\ y_2 \\ y_3 \end{pmatrix} = \begin{pmatrix} 4 \\ 10 \\ 24 \end{pmatrix}$$

- $y_1 = 4$
- $y_2 = 10 - 2 \cdot 4 = 2$
- $y_3 = 24 - 4 \cdot 4 - 3 \cdot 2 = -2$

**Rückwärtssubstitution** $Ux = y$:

$$\begin{pmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} 4 \\ 2 \\ -2 \end{pmatrix}$$

- $x_3 = \dfrac{-2}{2} = -1$
- $x_2 = 2 - 1 \cdot (-1) = 3$
- $x_1 = \dfrac{4 - 1 \cdot 3 - 1 \cdot (-1)}{2} = 1$

**Lösung:** $x = \begin{pmatrix} 1 \\ 3 \\ -1 \end{pmatrix}$

### Beispiel 2 (Zeilentausch nötig — Permutationsvektor)

Gegeben:

$$A = \begin{pmatrix} 0 & 1 & 2 \\ 1 & 2 & 3 \\ 2 & 1 & 1 \end{pmatrix}$$

Permutationsvektor zu Beginn: $\pi = (1, 2, 3)$ (Identität — keine Vertauschungen)

**Schritt 1:** Elimination in Spalte 1

Pivotelement $a_{11} = 0$ → **Division durch Null!** Zeilentausch nötig.

Suche größtes Element in Spalte 1: $|a_{11}| = 0$, $|a_{21}| = 1$, $|a_{31}| = 2$ → Tausche Zeile 1 ↔ Zeile 3

$$A' = \begin{pmatrix} 2 & 1 & 1 \\ 1 & 2 & 3 \\ 0 & 1 & 2 \end{pmatrix}, \quad \pi = (3, 2, 1)$$

Multiplikatoren:
- $l_{21} = \dfrac{1}{2}$
- $l_{31} = \dfrac{0}{2} = 0$

$$A^{(1)} = \begin{pmatrix} 2 & 1 & 1 \\ 0 & \frac{3}{2} & \frac{5}{2} \\ 0 & 1 & 2 \end{pmatrix}$$

**Schritt 2:** Elimination in Spalte 2

Pivotelement $a_{22}^{(1)} = \frac{3}{2}$, $a_{32}^{(1)} = 1$ → $\frac{3}{2} > 1$, kein Tausch nötig.

Multiplikator:
- $l_{32} = \dfrac{1}{\frac{3}{2}} = \dfrac{2}{3}$

$$U = \begin{pmatrix} 2 & 1 & 1 \\ 0 & \frac{3}{2} & \frac{5}{2} \\ 0 & 0 & \frac{1}{3} \end{pmatrix}$$

**Ergebnis:**

$$\pi = (3, 2, 1), \quad L = \begin{pmatrix} 1 & 0 & 0 \\ \frac{1}{2} & 1 & 0 \\ 0 & \frac{2}{3} & 1 \end{pmatrix}, \quad U = \begin{pmatrix} 2 & 1 & 1 \\ 0 & \frac{3}{2} & \frac{5}{2} \\ 0 & 0 & \frac{1}{3} \end{pmatrix}$$

**Lösen mit Permutationsvektor:**

Sei $b = \begin{pmatrix} 3 \\ 6 \\ 4 \end{pmatrix}$. Permutiere $b$ gemäß $\pi = (3, 2, 1)$:

$$b_\pi = \begin{pmatrix} b_3 \\ b_2 \\ b_1 \end{pmatrix} = \begin{pmatrix} 4 \\ 6 \\ 3 \end{pmatrix}$$

**Vorwärtssubstitution** $Ly = b_\pi$:
- $y_1 = 4$
- $y_2 = 6 - \frac{1}{2} \cdot 4 = 4$
- $y_3 = 3 - 0 \cdot 4 - \frac{2}{3} \cdot 4 = \frac{1}{3}$

**Rückwärtssubstitution** $Ux = y$:
- $x_3 = \dfrac{\frac{1}{3}}{\frac{1}{3}} = 1$
- $x_2 = \dfrac{4 - \frac{5}{2} \cdot 1}{\frac{3}{2}} = 1$
- $x_1 = \dfrac{4 - 1 \cdot 1 - 1 \cdot 1}{2} = 1$

**Lösung:** $x = \begin{pmatrix} 1 \\ 1 \\ 1 \end{pmatrix}$

---

## 2. LU-Zerlegung mit Pivotsuche (Spalten-Pivotisierung)

### Problem

Die einfache LU-Zerlegung versagt, wenn ein Pivotelement $a_{kk}^{(k)} = 0$ ist. Selbst wenn es nur **sehr klein** ist, führt dies zu numerischer Instabilität (große Rundungsfehler durch Division durch eine fast-Null).

### Idee

Vor jedem Eliminationsschritt sucht man in der aktuellen Spalte $k$ das **betragsmäßig größte** Element unterhalb (und einschließlich) der Diagonale und tauscht die entsprechende Zeile nach oben. Dies nennt man **Spalten-Pivotisierung** (partial pivoting).

Man erhält die Zerlegung:

$$P \cdot A = L \cdot U$$

wobei $P$ eine **Permutationsmatrix** ist, die die Zeilenvertauschungen beschreibt.

### Algorithmus

Für jede Spalte $k = 1, \dots, n-1$:

1. **Pivotsuche:** Finde $p$ mit $|a_{pk}^{(k)}| = \max_{i=k,\dots,n} |a_{ik}^{(k)}|$
2. **Zeilentausch:** Vertausche Zeile $k$ mit Zeile $p$ (in $A$ **und** in den bisherigen Einträgen von $L$)
3. **Elimination:** Wie bei der normalen LU-Zerlegung

In der Praxis speichert man die Permutation als Vektor $\pi = (\pi_1, \dots, \pi_n)$ statt als volle Matrix.

### Vorteile

- **Immer durchführbar** für reguläre Matrizen (kein Nullpivot möglich)
- **Numerisch stabil:** Alle Multiplikatoren erfüllen $|l_{ik}| \leq 1$, was die Fehlerfortpflanzung begrenzt

### Beispiel

Gegeben:

$$A = \begin{pmatrix} 0 & 2 & 1 \\ 3 & 1 & 1 \\ 1 & 4 & 2 \end{pmatrix}, \quad b = \begin{pmatrix} 5 \\ 7 \\ 11 \end{pmatrix}$$

**Schritt 1:** Pivotsuche in Spalte 1

Beträge: $|a_{11}| = 0$, $|a_{21}| = 3$, $|a_{31}| = 1$ → Maximum bei Zeile 2

Tausche Zeile 1 ↔ Zeile 2:

$$A' = \begin{pmatrix} 3 & 1 & 1 \\ 0 & 2 & 1 \\ 1 & 4 & 2 \end{pmatrix}, \quad \pi = (2, 1, 3)$$

Elimination in Spalte 1 — Ziel: alle Einträge **unterhalb** von $a_{11} = 3$ auf Null bringen.

**Zeile 2:** $a_{21} = 0$ → ist schon Null, nichts zu tun. Multiplikator: $l_{21} = \dfrac{a_{21}}{a_{11}} = \dfrac{0}{3} = 0$

**Zeile 3:** $a_{31} = 1 \neq 0$ → muss eliminiert werden.

Multiplikator: $l_{31} = \dfrac{a_{31}}{a_{11}} = \dfrac{1}{3}$

Neue Zeile 3 = Zeile 3 $-$ $l_{31}$ $\cdot$ Zeile 1:

$$\begin{pmatrix} 1 & 4 & 2 \end{pmatrix} - \frac{1}{3} \cdot \begin{pmatrix} 3 & 1 & 1 \end{pmatrix} = \begin{pmatrix} 1 - 1 & 4 - \frac{1}{3} & 2 - \frac{1}{3} \end{pmatrix} = \begin{pmatrix} 0 & \frac{11}{3} & \frac{5}{3} \end{pmatrix}$$

Ergebnis nach Elimination in Spalte 1:

$$A^{(1)} = \begin{pmatrix} 3 & 1 & 1 \\ 0 & 2 & 1 \\ 0 & \frac{11}{3} & \frac{5}{3} \end{pmatrix}$$

**Schritt 2:** Pivotsuche in Spalte 2

Beträge: $|a_{22}^{(1)}| = 2$, $|a_{32}^{(1)}| = \frac{11}{3} \approx 3.67$ → Maximum bei Zeile 3

Tausche Zeile 2 ↔ Zeile 3. Der Tausch betrifft **sowohl** die Matrixzeilen **als auch** die bereits berechneten $L$-Einträge aus Spalte 1:

**Matrixzeilen tauschen:**

$$A^{(1)} = \begin{pmatrix} 3 & 1 & 1 \\ 0 & \underset{\uparrow}{2} & \underset{\uparrow}{1} \\ 0 & \underset{\downarrow}{\frac{11}{3}} & \underset{\downarrow}{\frac{5}{3}} \end{pmatrix} \xrightarrow{\text{Zeile 2 ↔ 3}} A^{(1)'} = \begin{pmatrix} 3 & 1 & 1 \\ 0 & \frac{11}{3} & \frac{5}{3} \\ 0 & 2 & 1 \end{pmatrix}$$

**$L$-Einträge (Spalte 1) tauschen:** Die Multiplikatoren gehören zu den jeweiligen Zeilen — wenn die Zeilen tauschen, tauschen auch ihre Multiplikatoren:

$$l_{21} = 0, \; l_{31} = \tfrac{1}{3} \xrightarrow{\text{Zeile 2 ↔ 3}} l_{21} = \tfrac{1}{3}, \; l_{31} = 0$$

**Permutationsvektor aktualisieren:** $\pi = (2, 1, 3) \xrightarrow{\text{Pos. 2 ↔ 3}} \pi = (2, 3, 1)$

Elimination in Spalte 2:
- $l_{32} = \dfrac{2}{\frac{11}{3}} = \dfrac{6}{11}$

$$U = \begin{pmatrix} 3 & 1 & 1 \\ 0 & \frac{11}{3} & \frac{5}{3} \\ 0 & 0 & \frac{1}{11} \end{pmatrix}$$

**Ergebnis:**

$$P = \begin{pmatrix} 0 & 1 & 0 \\ 0 & 0 & 1 \\ 1 & 0 & 0 \end{pmatrix}, \quad L = \begin{pmatrix} 1 & 0 & 0 \\ \frac{1}{3} & 1 & 0 \\ 0 & \frac{6}{11} & 1 \end{pmatrix}, \quad U = \begin{pmatrix} 3 & 1 & 1 \\ 0 & \frac{11}{3} & \frac{5}{3} \\ 0 & 0 & \frac{1}{11} \end{pmatrix}$$

Zum Lösen: Permutiere $b$ gemäß $P$, dann Vorwärts-/Rückwärtssubstitution wie gewohnt.

$Pb = \begin{pmatrix} 7 \\ 11 \\ 5 \end{pmatrix}$

---

## 3. Nachiteration (Iterative Refinement)

### Idee

Die Nachiteration verbessert eine bereits berechnete (ungenaue) Loesung $\tilde{x}$ eines linearen Gleichungssystems $Ax = b$ schrittweise. Sie nutzt die **bereits vorhandene LU-Zerlegung** aus und ist daher sehr guenstig — pro Nachiterationsschritt fallen nur $\mathcal{O}(n^2)$ Operationen an (statt $\frac{2}{3}n^3$ fuer eine neue Zerlegung).

> [!tip] Merke
> Die Nachiteration ist besonders sinnvoll, wenn die LU-Zerlegung bereits berechnet wurde und die Loesung durch Rundungsfehler ungenau ist. Man "recycelt" die Zerlegung, um die Loesung iterativ zu verbessern.

### Algorithmus

Gegeben: $A$, $b$, LU-Zerlegung $PA = LU$, Naeherungsloesung $\tilde{x}$

**Fuer** $k = 0, 1, 2, \dots$ (bis Konvergenz):

1. **Residuum berechnen:** $r^{(k)} = b - A\tilde{x}^{(k)}$
2. **Korrektursystem loesen:** $A \cdot d^{(k)} = r^{(k)}$ (mittels der vorhandenen LU-Zerlegung: Vorwaerts-/Rueckwaertssubstitution)
3. **Loesung aktualisieren:** $\tilde{x}^{(k+1)} = \tilde{x}^{(k)} + d^{(k)}$

> [!warning] Achtung
> Das Residuum $r^{(k)} = b - A\tilde{x}^{(k)}$ sollte in **hoeherer Genauigkeit** (z.B. doppelte Praezision) berechnet werden, wenn die Zerlegung in einfacher Praezision erfolgte. Sonst kann die Nachiteration wirkungslos sein, da das Residuum selbst zu ungenau ist.

### Warum funktioniert das?

Der Fehler der aktuellen Naeherung ist $e^{(k)} = x - \tilde{x}^{(k)}$. Es gilt:

$$A \cdot e^{(k)} = A(x - \tilde{x}^{(k)}) = b - A\tilde{x}^{(k)} = r^{(k)}$$

Also loest der Fehler $e^{(k)}$ genau das System $Ad = r^{(k)}$. Die berechnete Korrektur $d^{(k)}$ ist eine Naeherung an $e^{(k)}$, und $\tilde{x}^{(k+1)} = \tilde{x}^{(k)} + d^{(k)}$ ist damit naeher an der exakten Loesung.

### Aufwand pro Iterationsschritt

- Residuum $r = b - A\tilde{x}$: eine Matrix-Vektor-Multiplikation → $\mathcal{O}(n^2)$
- Korrektursystem $Ad = r$ loesen (Vorwaerts-/Rueckwaertssubstitution mit vorhandener LU): $\mathcal{O}(n^2)$
- Update $\tilde{x} + d$: $\mathcal{O}(n)$

**Gesamt:** $\mathcal{O}(n^2)$ pro Schritt — deutlich guenstiger als die initiale Zerlegung mit $\mathcal{O}(n^3)$.

### Beispiel

Gegeben:

$$A = \begin{pmatrix} 4 & 1 \\ 1 & 3 \end{pmatrix}, \quad b = \begin{pmatrix} 9 \\ 7 \end{pmatrix}$$

**Exakte Loesung:** $x = (2, 1)^T$ (Probe: $4 \cdot 2 + 1 \cdot 1 = 9$, $1 \cdot 2 + 3 \cdot 1 = 5$... falsch, nehmen wir $b = (9, 5)^T$, dann stimmt $x = (2, 1)^T$.)

Korrigiert: $b = (9, 5)^T$, exakte Loesung $x = (2, 1)^T$.

Angenommen, durch Rundungsfehler in der LU-Zerlegung erhaelt man $\tilde{x}^{(0)} = (1.98, 1.01)^T$.

**Schritt 1: Residuum berechnen**

$$r^{(0)} = b - A\tilde{x}^{(0)} = \begin{pmatrix} 9 \\ 5 \end{pmatrix} - \begin{pmatrix} 4 & 1 \\ 1 & 3 \end{pmatrix} \begin{pmatrix} 1.98 \\ 1.01 \end{pmatrix} = \begin{pmatrix} 9 \\ 5 \end{pmatrix} - \begin{pmatrix} 8.93 \\ 5.01 \end{pmatrix} = \begin{pmatrix} 0.07 \\ -0.01 \end{pmatrix}$$

**Schritt 2: Korrektursystem loesen**

$$A \cdot d^{(0)} = r^{(0)} = \begin{pmatrix} 0.07 \\ -0.01 \end{pmatrix}$$

$$\begin{pmatrix} 4 & 1 \\ 1 & 3 \end{pmatrix} \begin{pmatrix} d_1 \\ d_2 \end{pmatrix} = \begin{pmatrix} 0.07 \\ -0.01 \end{pmatrix}$$

Mit Cramer oder Substitution: $\det(A) = 11$

- $d_1 = \frac{0.07 \cdot 3 - 1 \cdot (-0.01)}{11} = \frac{0.22}{11} = 0.02$
- $d_2 = \frac{4 \cdot (-0.01) - 0.07 \cdot 1}{11} = \frac{-0.11}{11} = -0.01$

**Schritt 3: Loesung aktualisieren**

$$\tilde{x}^{(1)} = \tilde{x}^{(0)} + d^{(0)} = \begin{pmatrix} 1.98 \\ 1.01 \end{pmatrix} + \begin{pmatrix} 0.02 \\ -0.01 \end{pmatrix} = \begin{pmatrix} 2.0 \\ 1.0 \end{pmatrix}$$

Nach **einem** Nachiterationsschritt hat man bereits die exakte Loesung erreicht.

### Konvergenz

Die Nachiteration konvergiert, solange die Matrix $A$ nicht zu schlecht konditioniert ist. Genauer: Wenn die LU-Zerlegung in Praezision $\varepsilon$ berechnet wird und das Residuum in hoeherer Praezision, dann konvergiert die Nachiteration fuer $\kappa(A) \cdot \varepsilon < 1$.

Bei **schlecht konditionierten** Matrizen ($\kappa(A) \cdot \varepsilon \geq 1$) kann die Nachiteration stagnieren oder divergieren.

---

## 4. Cholesky-Zerlegung

### Idee

Für **symmetrische, positiv definite** (s.p.d.) Matrizen gibt es eine effizientere Zerlegung:

$$A = L \cdot L^T$$

wobei $L$ eine untere Dreiecksmatrix mit **positiven Diagonaleinträgen** ist. Dies ist ein Spezialfall der LU-Zerlegung, bei dem $U = L^T$ gilt.

### Voraussetzungen

$A$ muss:
1. **Symmetrisch** sein: $A = A^T$
2. **Positiv definit** sein: $x^T A x > 0$ für alle $x \neq 0$

Äquivalent: Alle Eigenwerte von $A$ sind positiv.

### Algorithmus

Die Einträge von $L$ berechnen sich direkt:

**Diagonaleinträge** ($i = j$):

$$l_{ii} = \sqrt{a_{ii} - \sum_{k=1}^{i-1} l_{ik}^2}$$

**Einträge unterhalb der Diagonale** ($i > j$):

$$l_{ij} = \frac{1}{l_{jj}} \left( a_{ij} - \sum_{k=1}^{j-1} l_{ik} \cdot l_{jk} \right)$$

### Vorteile gegenüber LU

- **Halber Aufwand:** $\dfrac{1}{3}n^3 + \mathcal{O}(n^2)$ statt $\dfrac{2}{3}n^3$ (Symmetrie wird ausgenutzt)
- **Keine Pivotsuche nötig:** Positiv definite Matrizen sind automatisch stabil
- **Halber Speicher:** Nur $L$ muss gespeichert werden ($U = L^T$)
- **Eingebauter Definitheitstest:** Wenn unter der Wurzel ein negativer Wert auftritt, ist $A$ nicht positiv definit

### Beispiel 1

Gegeben:

$$A = \begin{pmatrix} 4 & 2 & 2 \\ 2 & 5 & 1 \\ 2 & 1 & 6 \end{pmatrix}$$

$A$ ist symmetrisch. Positiv definit? Prüfen wir die Hauptabschnittsminoren:
- $\det(A_1) = 4 > 0$
- $\det(A_2) = 4 \cdot 5 - 2 \cdot 2 = 16 > 0$
- $\det(A_3) = \det(A) = 4(30-1) - 2(12-2) + 2(2-10) = 116 - 20 - 16 = 80 > 0$

Alle positiv → $A$ ist s.p.d. ✔

**Berechnung von $L$:**

$l_{11} = \sqrt{a_{11}} = \sqrt{4} = 2$

$l_{21} = \dfrac{a_{21}}{l_{11}} = \dfrac{2}{2} = 1$

$l_{31} = \dfrac{a_{31}}{l_{11}} = \dfrac{2}{2} = 1$

$l_{22} = \sqrt{a_{22} - l_{21}^2} = \sqrt{5 - 1} = 2$

$l_{32} = \dfrac{a_{32} - l_{31} \cdot l_{21}}{l_{22}} = \dfrac{1 - 1 \cdot 1}{2} = 0$

$l_{33} = \sqrt{a_{33} - l_{31}^2 - l_{32}^2} = \sqrt{6 - 1 - 0} = \sqrt{5}$

**Ergebnis:**

$$L = \begin{pmatrix} 2 & 0 & 0 \\ 1 & 2 & 0 \\ 1 & 0 & \sqrt{5} \end{pmatrix}$$

**Probe:** $L \cdot L^T = \begin{pmatrix} 4 & 2 & 2 \\ 2 & 5 & 1 \\ 2 & 1 & 6 \end{pmatrix} = A$ ✔

### Beispiel 2 (Nicht positiv definit)

$$B = \begin{pmatrix} 1 & 2 \\ 2 & 1 \end{pmatrix}$$

$B$ ist symmetrisch, aber:

$l_{11} = \sqrt{1} = 1$, $l_{21} = \dfrac{2}{1} = 2$

$l_{22} = \sqrt{1 - 4} = \sqrt{-3}$ → **nicht definiert!**

Die Zerlegung scheitert — $B$ ist nicht positiv definit (Eigenwerte: $3$ und $-1$).

### Ausführliches Beispiel: Spalten-Pivot-Suche Schritt für Schritt

Gegeben:

$$A = \begin{pmatrix} 1 & 0 & 0 & 10 \\ -10 & 1 & 0 & 0 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & -10 & 0 \end{pmatrix}, \quad b = \begin{pmatrix} 11 \\ -9 \\ -9 \\ -10 \end{pmatrix}$$

Dies ist die Matrix aus Blatt 3, Aufgabe 1 mit $\beta = 10$, $n = 4$. Exakte Lösung: $x = (1,1,1,1)^T$.

**Permutationsvektor:** $\pi = (1,2,3,4)$

---

**Schritt 1: Elimination in Spalte 1**

Spalten-Pivotsuche: Beträge in Spalte 1 ab Zeile 1: $|1|, |-10|, |0|, |0|$
Maximum: $|-10| = 10$ in Zeile 2 → Tausche Zeile 1 ↔ Zeile 2

$$A' = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 1 & 0 & 0 & 10 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & -10 & 0 \end{pmatrix}, \quad \pi = (2,1,3,4)$$

Multiplikatoren:
- $l_{21} = \dfrac{1}{-10} = -\dfrac{1}{10}$ (Beachte: $|l_{21}| = 0.1 \leq 1$ — das ist der Vorteil der Pivotsuche!)
- $l_{31} = \dfrac{0}{-10} = 0$
- $l_{41} = \dfrac{0}{-10} = 0$

Elimination: Zeile 2 = Zeile 2 $-$ $(-\frac{1}{10})$ $\cdot$ Zeile 1:

$$\begin{pmatrix} 1 & 0 & 0 & 10 \end{pmatrix} - (-\tfrac{1}{10}) \cdot \begin{pmatrix} -10 & 1 & 0 & 0 \end{pmatrix} = \begin{pmatrix} 0 & \tfrac{1}{10} & 0 & 10 \end{pmatrix}$$

$$A^{(1)} = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 0 & \frac{1}{10} & 0 & 10 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & -10 & 0 \end{pmatrix}$$

---

**Schritt 2: Elimination in Spalte 2 (ab Zeile 2)**

Spalten-Pivotsuche: Beträge in Spalte 2 ab Zeile 2: $|\frac{1}{10}|, |-10|, |0|$
Maximum: $|-10| = 10$ in Zeile 3 → Tausche Zeile 2 ↔ Zeile 3

Tausche Matrixzeilen **und** L-Einträge aus Spalte 1:
- $l_{21} = -\frac{1}{10}$ und $l_{31} = 0$ tauschen → $l_{21} = 0$, $l_{31} = -\frac{1}{10}$

$$A^{(1)'} = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 0 & -10 & 1 & 0 \\ 0 & \frac{1}{10} & 0 & 10 \\ 0 & 0 & -10 & 0 \end{pmatrix}, \quad \pi = (2,3,1,4)$$

Multiplikatoren:
- $l_{32} = \dfrac{\frac{1}{10}}{-10} = -\dfrac{1}{100}$
- $l_{42} = \dfrac{0}{-10} = 0$

Elimination: Zeile 3 = Zeile 3 $-$ $(-\frac{1}{100})$ $\cdot$ Zeile 2:

$$\begin{pmatrix} 0 & \tfrac{1}{10} & 0 & 10 \end{pmatrix} - (-\tfrac{1}{100}) \cdot \begin{pmatrix} 0 & -10 & 1 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & \tfrac{1}{100} & 10 \end{pmatrix}$$

$$A^{(2)} = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & \frac{1}{100} & 10 \\ 0 & 0 & -10 & 0 \end{pmatrix}$$

---

**Schritt 3: Elimination in Spalte 3 (ab Zeile 3)**

Spalten-Pivotsuche: Beträge in Spalte 3 ab Zeile 3: $|\frac{1}{100}|, |-10|$
Maximum: $|-10| = 10$ in Zeile 4 → Tausche Zeile 3 ↔ Zeile 4

Tausche Matrixzeilen **und** L-Einträge aus Spalten 1 und 2:
- Spalte 1: $l_{31} = -\frac{1}{10}$, $l_{41} = 0$ tauschen → $l_{31} = 0$, $l_{41} = -\frac{1}{10}$
- Spalte 2: $l_{32} = -\frac{1}{100}$, $l_{42} = 0$ tauschen → $l_{32} = 0$, $l_{42} = -\frac{1}{100}$

$$A^{(2)'} = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & -10 & 0 \\ 0 & 0 & \frac{1}{100} & 10 \end{pmatrix}, \quad \pi = (2,3,4,1)$$

Multiplikator:
- $l_{43} = \dfrac{\frac{1}{100}}{-10} = -\dfrac{1}{1000}$

Elimination: Zeile 4 = Zeile 4 $-$ $(-\frac{1}{1000})$ $\cdot$ Zeile 3:

$$\begin{pmatrix} 0 & 0 & \tfrac{1}{100} & 10 \end{pmatrix} - (-\tfrac{1}{1000}) \cdot \begin{pmatrix} 0 & 0 & -10 & 0 \end{pmatrix} = \begin{pmatrix} 0 & 0 & 0 & 10 \end{pmatrix}$$

---

**Endergebnis:**

$$\pi = (2,3,4,1)$$

$$L = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ -\frac{1}{10} & -\frac{1}{100} & -\frac{1}{1000} & 1 \end{pmatrix}, \quad U = \begin{pmatrix} -10 & 1 & 0 & 0 \\ 0 & -10 & 1 & 0 \\ 0 & 0 & -10 & 0 \\ 0 & 0 & 0 & 10 \end{pmatrix}$$

Beachte: **Alle** $|l_{ij}| \leq 1$ — das ist die zentrale Eigenschaft der Spalten-Pivotsuche!

**Ohne Pivotsuche** würden die Multiplikatoren $l_{ij}$ den Faktor $\beta = 10$ pro Schritt akkumulieren ($l_{ij}$ bis zu $\beta^{n-1}$), was zu katastrophalem Genauigkeitsverlust führt.

---

## 5. Kondition einer Matrix

### Definition

Die **Konditionszahl** einer Matrix $A$ bezüglich einer Norm $\|\cdot\|$ ist:

$$\kappa(A) = \|A\| \cdot \|A^{-1}\|$$

Sie misst, wie empfindlich die Lösung eines Gleichungssystems $Ax = b$ auf Störungen in $A$ oder $b$ reagiert.

### Interpretation

- $\kappa(A) \approx 1$: **gut konditioniert** — kleine Störungen in den Daten führen zu kleinen Fehlern in der Lösung
- $\kappa(A) \gg 1$: **schlecht konditioniert** — kleine Störungen können die Lösung stark verfälschen
- Faustregel: Bei $\kappa(A) \approx 10^k$ verliert man etwa $k$ Dezimalstellen Genauigkeit

### Wichtige Normen

**Zeilensummennorm** (Maximumnorm, $\infty$-Norm):

$$\|A\|_\infty = \max_{i=1,\dots,n} \sum_{j=1}^{n} |a_{ij}|$$

Für jede Zeile werden die Beträge der Einträge aufsummiert — das Maximum dieser Zeilensummen ist die Norm.

**Spaltensummennorm** ($1$-Norm):

$$\|A\|_1 = \max_{j=1,\dots,n} \sum_{i=1}^{n} |a_{ij}|$$

### Fehlerabschätzung

Für ein gestörtes System $(A + \Delta A)(x + \Delta x) = b + \Delta b$ gilt:

$$\frac{\|\Delta x\|}{\|x\|} \leq \kappa(A) \cdot \left( \frac{\|\Delta A\|}{\|A\|} + \frac{\|\Delta b\|}{\|b\|} \right)$$

Der **relative Fehler** in der Lösung ist höchstens $\kappa(A)$-mal so groß wie der relative Fehler in den Eingabedaten.

### Beispiel 1: Gut konditionierte Matrix

$$A = \begin{pmatrix} 2 & -1 \\ 1 & 1 \end{pmatrix}$$

**Schritt 1: $\|A\|_\infty$ berechnen**

Zeilensummen der Beträge:
- Zeile 1: $|2| + |-1| = 3$
- Zeile 2: $|1| + |1| = 2$

$$\|A\|_\infty = \max(3, 2) = 3$$

**Schritt 2: $A^{-1}$ berechnen**

$$A^{-1} = \frac{1}{\det(A)} \begin{pmatrix} 1 & 1 \\ -1 & 2 \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 1 & 1 \\ -1 & 2 \end{pmatrix}$$

**Schritt 3: $\|A^{-1}\|_\infty$ berechnen**

Zeilensummen der Beträge:
- Zeile 1: $\frac{1}{3}(|1| + |1|) = \frac{2}{3}$
- Zeile 2: $\frac{1}{3}(|-1| + |2|) = 1$

$$\|A^{-1}\|_\infty = \max\left(\frac{2}{3}, 1\right) = 1$$

**Schritt 4: Kondition**

$$\kappa_\infty(A) = \|A\|_\infty \cdot \|A^{-1}\|_\infty = 3 \cdot 1 = 3$$

Gut konditioniert — kleine Störungen werden höchstens um Faktor 3 verstärkt.

### Beispiel 2: Schlecht konditionierte Matrix

$$A = \begin{pmatrix} 1 & -1 \\ -1 & 1.001 \end{pmatrix}$$

**$\|A\|_\infty$:** Zeilensummen: $|1| + |-1| = 2$, $|-1| + |1.001| = 2.001$ → $\|A\|_\infty = 2.001$

**$A^{-1}$:** $\det(A) = 1 \cdot 1.001 - (-1)(-1) = 0.001$

$$A^{-1} = \frac{1}{0.001} \begin{pmatrix} 1.001 & 1 \\ 1 & 1 \end{pmatrix} = \begin{pmatrix} 1001 & 1000 \\ 1000 & 1000 \end{pmatrix}$$

**$\|A^{-1}\|_\infty$:** Zeilensummen: $1001 + 1000 = 2001$, $1000 + 1000 = 2000$ → $\|A^{-1}\|_\infty = 2001$

$$\kappa_\infty(A) = 2.001 \cdot 2001 \approx 4004$$

Schlecht konditioniert! Eine relative Störung von $5 \cdot 10^{-4}$ kann zu einem relativen Fehler von bis zu $4004 \cdot 5 \cdot 10^{-4} \approx 2$ führen — die Lösung kann **komplett falsch** sein.

### Beispiel 3: Matrix aus Blatt 3, Aufgabe 1

Für $\beta > 1$:

$$A = \begin{pmatrix} 1 & 0 & \cdots & 0 & \beta \\ -\beta & 1 & 0 & \cdots & 0 \\ 0 & \ddots & \ddots & \ddots & \vdots \\ \vdots & \ddots & -\beta & 1 & 0 \\ 0 & \cdots & 0 & -\beta & 0 \end{pmatrix}$$

**$\|A\|_\infty$:** Die maximale Zeilensumme ist $1 + \beta$ (jede Zeile hat höchstens Betrag $1 + \beta$).

**$\|A^{-1}\|_\infty$:** Aus der gegebenen Inverse sieht man, dass die letzte Zeile die größte Zeilensumme hat:

$$\|A^{-1}\|_\infty = \frac{1}{\beta^n}(\beta^{n-1} + \beta^{n-2} + \cdots + \beta + 1) = \frac{1}{\beta^n} \cdot \frac{\beta^n - 1}{\beta - 1} \approx \frac{1}{\beta - 1}$$

$$\kappa_\infty(A) = (1 + \beta) \cdot \frac{\beta^n - 1}{\beta^n(\beta - 1)}$$

Für große $n$: $\kappa_\infty(A) \to \dfrac{1+\beta}{\beta-1}$, was **unabhängig von $n$** beschränkt ist.

Für $\beta = 10$: $\kappa_\infty \approx \frac{11}{9} \approx 1.22$ — die Matrix ist stets gut konditioniert, obwohl die LU-Zerlegung ohne Pivotsuche versagt!

> **Merke:** Eine gut konditionierte Matrix kann trotzdem bei **falscher Algorithmuswahl** (ohne Pivotsuche) zu katastrophalen Ergebnissen führen. Kondition und algorithmische Stabilität sind verschiedene Dinge!

---

## 6. Prager und Oettli (Störungsanalyse)

### Problem

Man hat ein lineares Gleichungssystem $Ax = b$ mit **unsicheren Koeffizienten**:
- Die Matrix $A$ ist nur bis auf $|\Delta A|$ genau bekannt
- Der Vektor $b$ ist nur bis auf $|\Delta b|$ genau bekannt

Man hat eine Näherungslösung $\tilde{x}$ berechnet. **Frage:** Kann $\tilde{x}$ als Lösung eines „in der Nähe liegenden" Systems akzeptiert werden?

### Satz von Prager und Oettli

Die Näherungslösung $\tilde{x}$ ist genau dann Lösung eines Systems $(A + \delta A)\tilde{x} = b + \delta b$ mit $|\delta A| \leq |\Delta A|$ und $|\delta b| \leq |\Delta b|$, wenn **komponentenweise** gilt:

$$|r| \leq |\Delta A| \cdot |\tilde{x}| + |\Delta b|$$

wobei $r = b - A\tilde{x}$ das **Residuum** ist.

Die Ungleichung muss für **jede Komponente** gelten:

$$|r_i| \leq \sum_{j=1}^{n} |\Delta a_{ij}| \cdot |\tilde{x}_j| + |\Delta b_i| \quad \text{für alle } i = 1, \dots, n$$

### Interpretation

- Das Residuum $r = b - A\tilde{x}$ misst, „wie weit $\tilde{x}$ von einer exakten Lösung entfernt ist"
- Die rechte Seite $|\Delta A||\tilde{x}| + |\Delta b|$ ist der maximal erlaubte Fehler aufgrund der Unsicherheiten
- Wenn $|r| \leq |\Delta A||\tilde{x}| + |\Delta b|$ komponentenweise, dann **existiert** ein zulässiges gestörtes System, das $\tilde{x}$ als exakte Lösung hat

### Beispiel 1: Lösung akzeptabel (Blatt 3, Aufgabe 2a)

$$A = \begin{pmatrix} 2 & -1 \\ 1 & 1 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 2 \end{pmatrix}, \quad \tilde{x} = \begin{pmatrix} 0.9 \\ 1.1 \end{pmatrix}$$

$$|\Delta A| \leq \frac{1}{10} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq \frac{1}{10} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

**Schritt 1: Residuum berechnen**

$$r = b - A\tilde{x} = \begin{pmatrix} 1 \\ 2 \end{pmatrix} - \begin{pmatrix} 2 & -1 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 0.9 \\ 1.1 \end{pmatrix} = \begin{pmatrix} 1 \\ 2 \end{pmatrix} - \begin{pmatrix} 0.7 \\ 2.0 \end{pmatrix} = \begin{pmatrix} 0.3 \\ 0 \end{pmatrix}$$

$$|r| = \begin{pmatrix} 0.3 \\ 0 \end{pmatrix}$$

**Schritt 2: Rechte Seite berechnen**

$$|\Delta A| \cdot |\tilde{x}| + |\Delta b| = \frac{1}{10} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 0.9 \\ 1.1 \end{pmatrix} + \frac{1}{10} \begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{10} \begin{pmatrix} 2.0 \\ 2.0 \end{pmatrix} + \begin{pmatrix} 0.1 \\ 0.1 \end{pmatrix} = \begin{pmatrix} 0.3 \\ 0.3 \end{pmatrix}$$

**Schritt 3: Komponentenweiser Vergleich**

$$|r_1| = 0.3 \leq 0.3 \quad \checkmark$$
$$|r_2| = 0 \leq 0.3 \quad \checkmark$$

Beide Komponenten erfüllt → $\tilde{x} = (0.9, 1.1)^T$ **kann als Lösung akzeptiert werden**.

### Beispiel 2: Lösung problematisch (Blatt 3, Aufgabe 2b)

$$A = \begin{pmatrix} 1 & -1 \\ -1 & 1.001 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \quad \tilde{x} = \begin{pmatrix} 501 \\ 500 \end{pmatrix}$$

$$|\Delta A| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

**Schritt 1: Residuum**

$$r = b - A\tilde{x} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} - \begin{pmatrix} 1 & -1 \\ -1 & 1.001 \end{pmatrix} \begin{pmatrix} 501 \\ 500 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} - \begin{pmatrix} 1 \\ -0.5 \end{pmatrix} = \begin{pmatrix} 0 \\ 0.5 \end{pmatrix}$$

$$|r| = \begin{pmatrix} 0 \\ 0.5 \end{pmatrix}$$

**Schritt 2: Rechte Seite**

$$|\Delta A| \cdot |\tilde{x}| + |\Delta b| = 5 \cdot 10^{-4} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 501 \\ 500 \end{pmatrix} + 5 \cdot 10^{-4} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

$$= 5 \cdot 10^{-4} \begin{pmatrix} 1001 \\ 1001 \end{pmatrix} + \begin{pmatrix} 0.0005 \\ 0.0005 \end{pmatrix} = \begin{pmatrix} 0.5010 \\ 0.5010 \end{pmatrix}$$

**Schritt 3: Komponentenweiser Vergleich**

$$|r_1| = 0 \leq 0.501 \quad \checkmark$$
$$|r_2| = 0.5 \leq 0.501 \quad \checkmark$$

Knapp erfüllt — die Lösung liegt gerade noch im Toleranzbereich. **Aber:** Die exakte Lösung ist $x = (1001, 1000)^T$, und $\tilde{x} = (501, 500)^T$ weicht um fast 50% davon ab!

> **Merke:** Prager-Oettli bestätigt nur, dass $\tilde{x}$ eine Lösung eines **zulässig gestörten** Systems ist — nicht, dass es nahe an der Lösung des ungestörten Systems liegt. Bei schlecht konditionierten Matrizen ($\kappa_\infty \approx 4004$ hier) können kleine Datenstörungen die Lösung trotzdem komplett verschieben. Das Bestehen des Prager-Oettli-Tests sagt nichts über die Nähe zur ungestörten Lösung!

---

## 7. Jacobi-Verfahren

### Idee

Das Jacobi-Verfahren ist ein **iteratives Verfahren** zur Loesung linearer Gleichungssysteme $Ax = b$. Statt einer exakten Loesung berechnet man eine Folge von Naeherungen $x^{(0)}, x^{(1)}, x^{(2)}, \dots$, die (bei Konvergenz) gegen die exakte Loesung konvergiert.

Die Grundidee: Man loest jede Gleichung nach der jeweiligen Unbekannten auf und setzt auf der rechten Seite die **alten** Werte ein.

### Herleitung

Man zerlegt die Matrix $A$ in:

$$A = D + L + U$$

wobei:
- $D$ = Diagonalmatrix (Diagonaleintraege von $A$)
- $L$ = strikte untere Dreiecksmatrix (Eintraege unterhalb der Diagonale)
- $U$ = strikte obere Dreiecksmatrix (Eintraege oberhalb der Diagonale)

**Achtung:** $L$ und $U$ hier sind **nicht** die Matrizen der LU-Zerlegung! Hier sind es einfach die Teile von $A$.

Aus $Ax = b$ wird $(D + L + U)x = b$, also:

$$Dx = b - (L + U)x$$

$$x = D^{-1}(b - (L + U)x)$$

Da $D$ eine Diagonalmatrix ist, ist $D^{-1}$ trivial: $d_{ii}^{-1} = \frac{1}{a_{ii}}$.

### Iterationsvorschrift

$$x^{(k+1)} = D^{-1}\left(b - (L + U)x^{(k)}\right)$$

Komponentenweise:

$$x_i^{(k+1)} = \frac{1}{a_{ii}} \left( b_i - \sum_{\substack{j=1 \\ j \neq i}}^{n} a_{ij} \cdot x_j^{(k)} \right) \quad \text{fuer } i = 1, \dots, n$$

> [!tip] Merke
> Beim Jacobi-Verfahren werden **alle** Komponenten von $x^{(k+1)}$ mit den **alten** Werten $x^{(k)}$ berechnet. Die neuen Werte werden erst im naechsten Schritt verwendet.

### Iterationsmatrix

$$M_J = -D^{-1}(L + U) = I - D^{-1}A$$

Die Iteration lautet dann: $x^{(k+1)} = M_J x^{(k)} + D^{-1}b$

### Konvergenz und Spektralradius

Das Jacobi-Verfahren konvergiert genau dann, wenn der **Spektralradius** der Iterationsmatrix kleiner als 1 ist:

$$\rho(M_J) < 1$$

Der Spektralradius $\rho(M)$ ist der **groesste Betrag** aller Eigenwerte von $M$:

$$\rho(M) = \max_{i} |\lambda_i(M)|$$

**Bedeutung fuer die Konvergenz:**
- $\rho(M_J) < 1$ → Iteration konvergiert fuer **jeden** Startwert $x^{(0)}$
- $\rho(M_J) \geq 1$ → Iteration divergiert (im Allgemeinen)
- Je **kleiner** $\rho(M_J)$, desto **schneller** die Konvergenz — der Fehler wird pro Schritt mit dem Faktor $\rho(M_J)$ multipliziert:

$$\|e^{(k)}\| \approx \rho(M_J)^k \cdot \|e^{(0)}\|$$

> [!quote] Definition
> Der **Spektralradius** $\rho(M)$ einer Matrix $M$ ist der groesste Betrag ihrer Eigenwerte. Er bestimmt das asymptotische Konvergenzverhalten iterativer Verfahren: $\rho < 1$ bedeutet Konvergenz, $\rho \geq 1$ bedeutet Divergenz.

**Hinreichendes Kriterium:** Ist $A$ **strikt diagonaldominant**, d.h.

$$|a_{ii}| > \sum_{\substack{j=1 \\ j \neq i}}^{n} |a_{ij}| \quad \text{fuer alle } i$$

dann ist automatisch $\rho(M_J) < 1$ und Jacobi konvergiert.

### Spektralradius berechnen — Beispiel

Fuer die Matrix aus dem Hauptbeispiel:

$$A = \begin{pmatrix} 4 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 4 \end{pmatrix}$$

Die Iterationsmatrix ist $M_J = -D^{-1}(L + U)$:

$$D = \begin{pmatrix} 4 & 0 & 0 \\ 0 & 4 & 0 \\ 0 & 0 & 4 \end{pmatrix}, \quad L + U = \begin{pmatrix} 0 & -1 & 0 \\ -1 & 0 & -1 \\ 0 & -1 & 0 \end{pmatrix}$$

$$M_J = -\frac{1}{4} \begin{pmatrix} 0 & -1 & 0 \\ -1 & 0 & -1 \\ 0 & -1 & 0 \end{pmatrix} = \begin{pmatrix} 0 & \frac{1}{4} & 0 \\ \frac{1}{4} & 0 & \frac{1}{4} \\ 0 & \frac{1}{4} & 0 \end{pmatrix}$$

**Eigenwerte von $M_J$** (ueber $\det(M_J - \lambda I) = 0$):

$$-\lambda \left(\lambda^2 - \frac{1}{8}\right) - \frac{1}{16}(-\lambda) = 0$$

$$-\lambda^3 + \frac{\lambda}{8} + \frac{\lambda}{16} = 0 \implies \lambda\left(-\lambda^2 + \frac{3}{16}\right) = 0$$

$$\lambda_1 = 0, \quad \lambda_{2,3} = \pm\sqrt{\frac{3}{16}} = \pm\frac{\sqrt{3}}{4} \approx \pm 0.433$$

$$\rho(M_J) = \frac{\sqrt{3}}{4} \approx 0.433 < 1 \quad \implies \text{Konvergenz}$$

Der Fehler schrumpft also pro Iteration um den Faktor $\approx 0.433$ — nach 5 Iterationen ist der Fehler auf ca. $0.433^5 \approx 1.5\%$ des Anfangsfehlers geschrumpft.

### Beispiel

Gegeben:

$$\begin{pmatrix} 4 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 4 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} 5 \\ 10 \\ 5 \end{pmatrix}$$

**Prüfung strikte Diagonaldominanz:**
- Zeile 1: $|4| = 4 > |-1| = 1$ (korrekt)
- Zeile 2: $|4| = 4 > |-1| + |-1| = 2$ (korrekt)
- Zeile 3: $|4| = 4 > |-1| = 1$ (korrekt)

**Iterationsvorschrift aufstellen:**

$$x_1^{(k+1)} = \frac{1}{4}\left(5 + x_2^{(k)}\right)$$

$$x_2^{(k+1)} = \frac{1}{4}\left(10 + x_1^{(k)} + x_3^{(k)}\right)$$

$$x_3^{(k+1)} = \frac{1}{4}\left(5 + x_2^{(k)}\right)$$

**Startwert:** $x^{(0)} = (0, 0, 0)^T$

**Iteration 1:**

- $x_1^{(1)} = \frac{1}{4}(5 + 0) = 1.25$
- $x_2^{(1)} = \frac{1}{4}(10 + 0 + 0) = 2.5$
- $x_3^{(1)} = \frac{1}{4}(5 + 0) = 1.25$

**Iteration 2:**

- $x_1^{(2)} = \frac{1}{4}(5 + 2.5) = 1.875$
- $x_2^{(2)} = \frac{1}{4}(10 + 1.25 + 1.25) = 3.125$
- $x_3^{(2)} = \frac{1}{4}(5 + 2.5) = 1.875$

**Iteration 3:**

- $x_1^{(3)} = \frac{1}{4}(5 + 3.125) = 2.03125$
- $x_2^{(3)} = \frac{1}{4}(10 + 1.875 + 1.875) = 3.4375$
- $x_3^{(3)} = \frac{1}{4}(5 + 3.125) = 2.03125$

**Exakte Loesung:** $x = (2,\; 3.5,\; 2)^T$ — die Naeherung konvergiert sichtbar dagegen.

### Abbruchkriterium

Man iteriert, bis die Aenderung klein genug ist: $\|x^{(k+1)} - x^{(k)}\| < \varepsilon$, oder bis das Residuum klein ist: $\|b - Ax^{(k)}\| < \varepsilon$.

---

## 8. Gauss-Seidel-Verfahren

### Idee

Das Gauss-Seidel-Verfahren ist eine **Verbesserung des Jacobi-Verfahrens**. Der zentrale Unterschied: Sobald eine neue Komponente $x_i^{(k+1)}$ berechnet ist, wird sie **sofort** in den folgenden Berechnungen desselben Iterationsschritts verwendet.

### Herleitung

Wie bei Jacobi zerlegt man $A = D + L + U$. Aus $Ax = b$ wird:

$$(D + L)x = b - Ux$$

$$x = (D + L)^{-1}(b - Ux)$$

### Iterationsvorschrift

$$x^{(k+1)} = (D + L)^{-1}\left(b - Ux^{(k)}\right)$$

Komponentenweise:

$$x_i^{(k+1)} = \frac{1}{a_{ii}} \left( b_i - \sum_{j=1}^{i-1} a_{ij} \cdot x_j^{(k+1)} - \sum_{j=i+1}^{n} a_{ij} \cdot x_j^{(k)} \right)$$

> [!tip] Merke
> Der entscheidende Unterschied zu Jacobi: In der **ersten Summe** ($j < i$) stehen bereits die **neuen** Werte $x_j^{(k+1)}$, in der **zweiten Summe** ($j > i$) noch die **alten** Werte $x_j^{(k)}$.

### Iterationsmatrix

$$M_{GS} = -(D + L)^{-1} U$$

### Konvergenz und Spektralradius

Wie bei Jacobi konvergiert Gauss-Seidel genau dann, wenn $\rho(M_{GS}) < 1$.

**Hinreichende Kriterien:**
1. $A$ ist **strikt diagonaldominant** → Gauss-Seidel konvergiert
2. $A$ ist **symmetrisch und positiv definit** → Gauss-Seidel konvergiert

**Spektralradius im Vergleich:** Fuer dieselbe Matrix gilt oft $\rho(M_{GS}) \approx \rho(M_J)^2$. Das bedeutet: Gauss-Seidel konvergiert ungefaehr **doppelt so schnell** wie Jacobi (gemessen in Iterationen).

Fuer das Beispiel oben: $\rho(M_J) \approx 0.433$, also erwartet man $\rho(M_{GS}) \approx 0.433^2 \approx 0.1875$. Tatsaechlich ist $\rho(M_{GS}) = \frac{3}{16} = 0.1875$ — der Fehler schrumpft pro Schritt auf unter 19%, waehrend er bei Jacobi nur auf 43% schrumpft.

> [!info] Hinweis
> Gauss-Seidel konvergiert in der Praxis oft **schneller** als Jacobi, da die neuen Werte sofort genutzt werden. Es gibt aber Faelle, in denen Jacobi konvergiert und Gauss-Seidel nicht (und umgekehrt) — der Spektralradius muss fuer jedes Verfahren separat geprueft werden.

### Beispiel

Dasselbe System wie beim Jacobi-Verfahren:

$$\begin{pmatrix} 4 & -1 & 0 \\ -1 & 4 & -1 \\ 0 & -1 & 4 \end{pmatrix} \begin{pmatrix} x_1 \\ x_2 \\ x_3 \end{pmatrix} = \begin{pmatrix} 5 \\ 10 \\ 5 \end{pmatrix}$$

**Iterationsvorschrift** (beachte: neue Werte werden sofort genutzt):

- $x_1^{(k+1)} = \frac{1}{4}\left(5 + x_2^{(k)}\right)$
- $x_2^{(k+1)} = \frac{1}{4}\left(10 + x_1^{(\mathbf{k+1})} + x_3^{(k)}\right)$
- $x_3^{(k+1)} = \frac{1}{4}\left(5 + x_2^{(\mathbf{k+1})}\right)$

**Startwert:** $x^{(0)} = (0, 0, 0)^T$

**Iteration 1:**

- $x_1^{(1)} = \frac{1}{4}(5 + 0) = 1.25$
- $x_2^{(1)} = \frac{1}{4}(10 + \mathbf{1.25} + 0) = 2.8125$
- $x_3^{(1)} = \frac{1}{4}(5 + \mathbf{2.8125}) = 1.953125$

**Iteration 2:**

- $x_1^{(2)} = \frac{1}{4}(5 + 2.8125) = 1.953125$
- $x_2^{(2)} = \frac{1}{4}(10 + \mathbf{1.953125} + 1.953125) = 3.47656$
- $x_3^{(2)} = \frac{1}{4}(5 + \mathbf{3.47656}) = 2.11914$

**Iteration 3:**

- $x_1^{(3)} = \frac{1}{4}(5 + 3.47656) = 2.11914$
- $x_2^{(3)} = \frac{1}{4}(10 + \mathbf{2.11914} + 2.11914) = 3.55957$
- $x_3^{(3)} = \frac{1}{4}(5 + \mathbf{3.55957}) = 2.13989$

**Exakte Loesung:** $x = (2,\; 3.5,\; 2)^T$ — Gauss-Seidel ist nach 3 Iterationen bereits naeher dran als Jacobi.

### Vergleich Jacobi vs. Gauss-Seidel

- **Verwendung neuer Werte:** Jacobi erst im naechsten Schritt, Gauss-Seidel sofort
- **Parallelisierbar:** Jacobi ja (alle Komponenten unabhaengig), Gauss-Seidel nein (sequentielle Abhaengigkeit)
- **Konvergenzgeschwindigkeit:** Gauss-Seidel oft ca. doppelt so schnell
- **Konvergenz bei strikt diag.-dom.:** Beide garantiert
- **Konvergenz bei s.p.d.:** Nur Gauss-Seidel garantiert
- **Speicher:** Jacobi braucht $x^{(k)}$ und $x^{(k+1)}$, Gauss-Seidel kann in-place aktualisieren

---

## Zusammenfassung

| Eigenschaft | LU (Gauß) | LU mit Pivot | Cholesky |
|---|---|---|---|
| Zerlegung | $A = LU$ | $PA = LU$ | $A = LL^T$ |
| Voraussetzung | Hauptabschnittsminoren $\neq 0$ | $A$ regulär | $A$ symmetrisch & positiv definit |
| Pivotsuche | Nein | Ja (Spalte) | Nicht nötig |
| Aufwand | $\frac{2}{3}n^3$ | $\frac{2}{3}n^3$ | $\frac{1}{3}n^3$ |
| Stabilität | Kann instabil sein | Numerisch stabil | Stabil |

### Iterative Verfahren

- **Jacobi:** Alle Komponenten mit alten Werten berechnen, konvergiert bei strikt diag.-dom. Matrizen, parallelisierbar
- **Gauss-Seidel:** Neue Werte sofort nutzen, konvergiert bei strikt diag.-dom. und s.p.d. Matrizen, oft ca. doppelt so schnell wie Jacobi

### Fehleranalyse-Werkzeuge

| Werkzeug | Fragestellung | Formel |
|---|---|---|
| **Konditionszahl** | Wie empfindlich ist das System? | $\kappa(A) = \|A\| \cdot \|A^{-1}\|$ |
| **Prager-Oettli** | Ist $\tilde{x}$ eine zulässige Lösung? | $\|b - A\tilde{x}\| \leq \|\Delta A\| \|\tilde{x}\| + \|\Delta b\|$ (komponentenweise) |

> **Wichtig:** Prager-Oettli prüft nur, ob die Näherung mit den Datenunsicherheiten konsistent ist. Bei schlecht konditionierten Systemen kann eine Näherung den Test bestehen und trotzdem weit von der wahren Lösung entfernt sein.
