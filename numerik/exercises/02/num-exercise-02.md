---
course: numerik
type: exercise
lecture: 2
date: 2026-04-09
tags:
  - numerik
  - exercise
  - python
  - lu-zerlegung
  - cholesky
  - lineare-gleichungssysteme
source: NumI-02.pdf
---

# Uebung Numerik I — Blatt 2

**Quelle:** [[numerik/resources/NumI-02.pdf|NumI-02.pdf]]
**Praktikum:** `numerik/repos/numerik/blatt02/`
**Loesung:** [[numerik/exercises/02/num-solution-02|Loesungen Blatt 2]]

---


## Inhaltsverzeichnis

- [[#Aufgabe 1 — LU-Zerlegung (von Hand)|Aufgabe 1 — LU-Zerlegung (von Hand)]]
- [[#Aufgabe 2 — LU-Zerlegung in Python|Aufgabe 2 — LU-Zerlegung in Python]]
- [[#Aufgabe 3 — Elastisches Stromkabel (Randwertproblem)|Aufgabe 3 — Elastisches Stromkabel (Randwertproblem)]]

---

## Aufgabe 1 — LU-Zerlegung (von Hand)

Loesen Sie mit Hilfe der LU-Zerlegung das Gleichungssystem $Ax = b$ mit:

$$A = \begin{pmatrix} 0 & 1 & 3 & 1 \\ 1 & 1 & 2 & 0 \\ 4 & 4 & 8 & 2 \\ 2 & 6 & 4 & 8 \end{pmatrix}$$

fuer:
- (a) $b = (5, 1, 8, 18)^T$
- (b) $b = (5, 7, 28, 22)^T$

---

## Aufgabe 2 — LU-Zerlegung in Python

Programmieren Sie die LU-Zerlegung fuer beliebige regulaere Matrizen mit folgenden Funktionen:

```python
LU, p = zerlegung(A)       # PA = LU
c = permutation(p, b)      # c = Pb
y = vorwaerts(LU, c)       # y ist Loesung von Ly = c
x = rueckwaerts(LU, y)     # x ist Loesung von Ux = y
```

- Zerlegung der Matrix A, Permutation eines Vektors b anhand der Permutationsmatrix P (im Vektor p kodiert)
- Vorwaerts- und rueckwaerts Einsetzen (LU ist dabei ein Array, das L und U enthaelt)
- Zur Permutation der Zeilen: jeweils die erste Zeile unterhalb der Diagonalen mit entsprechendem Element ungleich 0

**Test mit Beispiel aus der Vorlesung:**

$$A = \begin{pmatrix} 0 & 0 & 0 & 1 \\ 2 & 1 & 2 & 0 \\ 4 & 4 & 0 & 0 \\ 2 & 3 & 1 & 0 \end{pmatrix}, \quad b = \begin{pmatrix} 3 \\ 5 \\ 4 \\ 5 \end{pmatrix}, \quad b' = \begin{pmatrix} 4 \\ 10 \\ 12 \\ 11 \end{pmatrix}$$

**Anwendung auf Hilbert-aehnliches System:**

$$A = (a_{ij}), \quad a_{ij} = \frac{1}{i+j-1}, \quad b = (b_i), \quad b_i = \frac{1}{i+1}$$

fuer $n = 10, 20, 100$. Exakte Loesung: $x = (0, 1, 0, \ldots, 0)^T$. Numerische Resultate vergleichen.

---

## Aufgabe 3 — Elastisches Stromkabel (Randwertproblem)

Randwertproblem: $-u''(s) = -1$ fuer $s \in (0,1)$, $u(0) = u(1) = 0$

Diskretisierung mit finiten Differenzen fuehrt auf $Ax = b$ mit Tridiagonalmatrix:

$$A = \begin{pmatrix} 2 & -1 & & \\ -1 & 2 & -1 & \\ & \ddots & \ddots & \ddots \\ & & -1 & 2 \end{pmatrix} \in \mathbb{R}^{n \times n}, \quad b = \frac{1}{(n+1)^2} \begin{pmatrix} -1 \\ \vdots \\ -1 \end{pmatrix} \in \mathbb{R}^n$$

### (a) Cholesky-Zerlegung von Hand
Fuer $n = 4$ die Cholesky-Zerlegung von A durchfuehren. Beachten, welche Matrixeintraege sich veraendern.

### (b) Cholesky-Zerlegung in Python
Implementierung, die Symmetrie und Tridiagonalgestalt von A ausnutzt. Die zwei wesentlichen Matrixdiagonalen in Vektoren speichern. Gleichungssystem loesen fuer $n = 100 / 1000 / 10000$.
