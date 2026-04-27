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
3. [[#3. Cholesky-Zerlegung|Cholesky-Zerlegung]]
4. [[#4. Kondition einer Matrix|Kondition einer Matrix]]
5. [[#5. Prager und Oettli (Stoerungsanalyse)|Prager und Oettli]]
6. [[#Zusammenfassung|Zusammenfassung]]

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

## 3. Cholesky-Zerlegung

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

## 4. Kondition einer Matrix

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

## 5. Prager und Oettli (Störungsanalyse)

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

## Zusammenfassung

| Eigenschaft | LU (Gauß) | LU mit Pivot | Cholesky |
|---|---|---|---|
| Zerlegung | $A = LU$ | $PA = LU$ | $A = LL^T$ |
| Voraussetzung | Hauptabschnittsminoren $\neq 0$ | $A$ regulär | $A$ symmetrisch & positiv definit |
| Pivotsuche | Nein | Ja (Spalte) | Nicht nötig |
| Aufwand | $\frac{2}{3}n^3$ | $\frac{2}{3}n^3$ | $\frac{1}{3}n^3$ |
| Stabilität | Kann instabil sein | Numerisch stabil | Stabil |

### Fehleranalyse-Werkzeuge

| Werkzeug | Fragestellung | Formel |
|---|---|---|
| **Konditionszahl** | Wie empfindlich ist das System? | $\kappa(A) = \|A\| \cdot \|A^{-1}\|$ |
| **Prager-Oettli** | Ist $\tilde{x}$ eine zulässige Lösung? | $\|b - A\tilde{x}\| \leq \|\Delta A\| \|\tilde{x}\| + \|\Delta b\|$ (komponentenweise) |

> **Wichtig:** Prager-Oettli prüft nur, ob die Näherung mit den Datenunsicherheiten konsistent ist. Bei schlecht konditionierten Systemen kann eine Näherung den Test bestehen und trotzdem weit von der wahren Lösung entfernt sein.
