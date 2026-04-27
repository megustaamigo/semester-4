---
course: numerik
type: solution
lecture: 2
date: 2026-04-16
tags:
  - numerik
  - solution
  - python
  - lu-zerlegung
  - cholesky
  - lineare-gleichungssysteme
source: numerik/repos/numerik/blatt02/
---

# Loesungen — Blatt 2

**Aufgaben:** [[numerik/resources/NumI-02.pdf|NumI-02.pdf]]
**PDF:** [[numerik/exercises/02/num-solution-02.pdf|num-solution-02.pdf]]
**Quellcode:** `numerik/repos/numerik/blatt02/`

---


## Inhaltsverzeichnis

- [[#Aufgabe 1 — LU-Zerlegung (von Hand)|Aufgabe 1 — LU-Zerlegung (von Hand)]]
- [[#Aufgabe 2 — LU-Zerlegung in Python|Aufgabe 2 — LU-Zerlegung in Python]]
- [[#Aufgabe 3a — Cholesky-Zerlegung von Hand ($n = 4$)|Aufgabe 3a — Cholesky-Zerlegung von Hand ($n = 4$)]]
- [[#Aufgabe 3b — Cholesky-Zerlegung (Tridiagonal, Stromkabel)|Aufgabe 3b — Cholesky-Zerlegung (Tridiagonal, Stromkabel)]]

---

## Aufgabe 1 — LU-Zerlegung (von Hand)

### Zerlegung

$$A = \begin{pmatrix} 0 & 1 & 3 & 1 \\ 1 & 1 & 2 & 0 \\ 4 & 4 & 8 & 2 \\ 2 & 6 & 4 & 8 \end{pmatrix}$$

**Schritt 1:** $a_{11} = 0$ — Zeilentausch noetig. Erste Zeile mit $a_{i1} \neq 0$: Zeile 2.

Tausche Zeile 1 und Zeile 2: $\pi = (2,1,3,4)$

$$A' = \begin{pmatrix} 1 & 1 & 2 & 0 \\ 0 & 1 & 3 & 1 \\ 4 & 4 & 8 & 2 \\ 2 & 6 & 4 & 8 \end{pmatrix}$$

Multiplikatoren: $l_{21} = 0$, $l_{31} = 4$, $l_{41} = 2$

$$A^{(1)} = \begin{pmatrix} 1 & 1 & 2 & 0 \\ 0 & 1 & 3 & 1 \\ 0 & 0 & 0 & 2 \\ 0 & 4 & 0 & 8 \end{pmatrix}$$

**Schritt 2:** Pivotsuche in Spalte 2 (ab Zeile 2): $|1|, |0|, |4|$ — Tausche Zeile 2 und Zeile 4: $\pi = (2,4,3,1)$

Tausche auch L-Eintraege Spalte 1: $l_{21} = 0 \leftrightarrow l_{41} = 2$ — $l_{21} = 2$, $l_{41} = 0$

$$A^{(1)'} = \begin{pmatrix} 1 & 1 & 2 & 0 \\ 0 & 4 & 0 & 8 \\ 0 & 0 & 0 & 2 \\ 0 & 1 & 3 & 1 \end{pmatrix}$$

Multiplikatoren: $l_{32} = 0$, $l_{42} = \frac{1}{4}$

$$A^{(2)} = \begin{pmatrix} 1 & 1 & 2 & 0 \\ 0 & 4 & 0 & 8 \\ 0 & 0 & 0 & 2 \\ 0 & 0 & 3 & -1 \end{pmatrix}$$

**Schritt 3:** Pivotsuche in Spalte 3 (ab Zeile 3): $|0|, |3|$ — Tausche Zeile 3 und Zeile 4: $\pi = (2,4,1,3)$

Tausche L-Eintraege: Spalte 1: $l_{31} = 4 \leftrightarrow l_{41} = 0$, Spalte 2: $l_{32} = 0 \leftrightarrow l_{42} = \frac{1}{4}$

$$U = \begin{pmatrix} 1 & 1 & 2 & 0 \\ 0 & 4 & 0 & 8 \\ 0 & 0 & 3 & -1 \\ 0 & 0 & 0 & 2 \end{pmatrix}$$

**Ergebnis:**

$$\pi = (2,4,1,3), \quad L = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 2 & 1 & 0 & 0 \\ 0 & \frac{1}{4} & 1 & 0 \\ 4 & 0 & 0 & 1 \end{pmatrix}$$

### (a) Loesen fuer $b = (5, 1, 8, 18)^T$

Permutiere: $b_\pi = (b_2, b_4, b_1, b_3) = (1, 18, 5, 8)^T$

**Vorwaerts** $Ly = b_\pi$:
- $y_1 = 1$
- $y_2 = 18 - 2 \cdot 1 = 16$
- $y_3 = 5 - \frac{1}{4} \cdot 16 = 1$
- $y_4 = 8 - 4 \cdot 1 = 4$

**Rueckwaerts** $Ux = y$:
- $x_4 = \frac{4}{2} = 2$
- $x_3 = \frac{1 - (-1) \cdot 2}{3} = 1$
- $x_2 = \frac{16 - 8 \cdot 2}{4} = 0$
- $x_1 = \frac{1 - 1 \cdot 0 - 2 \cdot 1 - 0 \cdot 2}{1} = -1$

**Loesung:** $x = (-1, 0, 1, 2)^T$

### (b) Loesen fuer $b = (5, 7, 28, 22)^T$

Permutiere: $b_\pi = (7, 22, 5, 28)^T$

**Vorwaerts** $Ly = b_\pi$:
- $y_1 = 7$
- $y_2 = 22 - 2 \cdot 7 = 8$
- $y_3 = 5 - \frac{1}{4} \cdot 8 = 3$
- $y_4 = 28 - 4 \cdot 7 = 0$

**Rueckwaerts** $Ux = y$:
- $x_4 = \frac{0}{2} = 0$
- $x_3 = \frac{3 - (-1) \cdot 0}{3} = 1$
- $x_2 = \frac{8 - 8 \cdot 0}{4} = 2$
- $x_1 = \frac{7 - 1 \cdot 2 - 2 \cdot 1}{1} = 3$

**Loesung:** $x = (3, 2, 1, 0)^T$

---

## Aufgabe 2 — LU-Zerlegung in Python

```python
import numpy as np

def zerlegung(A):
    n = len(A)
    LU = A.astype(float).copy()
    p = list(range(n))
    for k in range(n):
        pivot = next(i for i in range(k, n) if LU[i, k] != 0)
        if pivot != k:
            LU[[k, pivot]] = LU[[pivot, k]]
            p[k], p[pivot] = p[pivot], p[k]
        for i in range(k + 1, n):
            LU[i, k] /= LU[k, k]
            LU[i, k+1:] -= LU[i, k] * LU[k, k+1:]
    return LU, p

def permutation(p, b): return b[p].astype(float).copy()

def vorwaerts(LU, c):
    n, y = len(c), c.copy()
    for i in range(1, n):
        y[i] -= LU[i, :i] @ y[:i]
    return y

def rueckwaerts(LU, y):
    n, x = len(y), y.copy()
    for i in range(n-1, -1, -1):
        x[i] = (x[i] - LU[i, i+1:] @ x[i+1:]) / LU[i, i]
    return x

def solve(A, b):
    LU, p = zerlegung(A)
    return rueckwaerts(LU, vorwaerts(LU, permutation(p, b)))
```

**Test Vorlesungsbeispiel:**
- $b = (3,5,4,5)^T \rightarrow x = (0,1,2,3)^T$ (korrekt)
- $b' = (4,10,12,11)^T \rightarrow x = (1,2,3,4)^T$ (korrekt)

**Hilbert-aehnliches System** (exakt: $x = (0,1,0,\ldots,0)^T$):

| n | max\|x - x\_exact\| |
|---|---|
| 10 | 2.46e-07 |
| 20 | 7.20e-01 |
| 100 | 6.42e-01 |

**Ergebnis:** Die Hilbert-Matrix wird fuer grosse n extrem schlecht konditioniert. Bereits ab n=20 weicht die numerische Loesung stark von der exakten ab.

---

## Aufgabe 3a — Cholesky-Zerlegung von Hand ($n = 4$)

Tridiagonalmatrix:

$$A = \begin{pmatrix} 2 & -1 & 0 & 0 \\ -1 & 2 & -1 & 0 \\ 0 & -1 & 2 & -1 \\ 0 & 0 & -1 & 2 \end{pmatrix}$$

$A$ ist symmetrisch und positiv definit (alle Eigenwerte positiv). Zerlegung $A = LL^T$ mit $L$ als untere Bidiagonalmatrix (Tridiagonalstruktur bleibt erhalten!):

$$L = \begin{pmatrix} d_1 & 0 & 0 & 0 \\ e_1 & d_2 & 0 & 0 \\ 0 & e_2 & d_3 & 0 \\ 0 & 0 & e_3 & d_4 \end{pmatrix}$$

**Berechnung Schritt fuer Schritt:**

$d_1 = \sqrt{a_{11}} = \sqrt{2}$

$e_1 = \frac{a_{21}}{d_1} = \frac{-1}{\sqrt{2}} = -\frac{\sqrt{2}}{2}$

$d_2 = \sqrt{a_{22} - e_1^2} = \sqrt{2 - \frac{1}{2}} = \sqrt{\frac{3}{2}} = \frac{\sqrt{6}}{2}$

$e_2 = \frac{a_{32}}{d_2} = \frac{-1}{\frac{\sqrt{6}}{2}} = -\frac{2}{\sqrt{6}} = -\frac{\sqrt{6}}{3}$

$d_3 = \sqrt{a_{33} - e_2^2} = \sqrt{2 - \frac{2}{3}} = \sqrt{\frac{4}{3}} = \frac{2\sqrt{3}}{3}$

$e_3 = \frac{a_{43}}{d_3} = \frac{-1}{\frac{2\sqrt{3}}{3}} = -\frac{3}{2\sqrt{3}} = -\frac{\sqrt{3}}{2}$

$d_4 = \sqrt{a_{44} - e_3^2} = \sqrt{2 - \frac{3}{4}} = \sqrt{\frac{5}{4}} = \frac{\sqrt{5}}{2}$

**Ergebnis:**

$$L = \begin{pmatrix} \sqrt{2} & 0 & 0 & 0 \\ -\frac{\sqrt{2}}{2} & \frac{\sqrt{6}}{2} & 0 & 0 \\ 0 & -\frac{\sqrt{6}}{3} & \frac{2\sqrt{3}}{3} & 0 \\ 0 & 0 & -\frac{\sqrt{3}}{2} & \frac{\sqrt{5}}{2} \end{pmatrix}$$

**Beobachtung:** Nur die Diagonal- und die untere Nebendiagonale von $L$ sind besetzt — die Tridiagonalstruktur bleibt erhalten. Man muss nur $2n - 1$ Werte statt $\frac{n(n+1)}{2}$ berechnen und speichern.

**Muster:** $d_i = \sqrt{\frac{i+1}{i}}$, $e_i = -\frac{1}{d_i} = -\sqrt{\frac{i}{i+1}}$

---

## Aufgabe 3b — Cholesky-Zerlegung (Tridiagonal, Stromkabel)

```python
import numpy as np

def cholesky_tridiag(n):
    d = np.zeros(n)      # Diagonale von L
    e = np.zeros(n - 1)  # untere Nebendiagonale von L
    d[0] = np.sqrt(2.0)
    for i in range(1, n):
        e[i-1] = -1.0 / d[i-1]
        d[i] = np.sqrt(2.0 - e[i-1]**2)
    return d, e

def solve_tridiag(d, e, b):
    n = len(b)
    y = np.zeros(n)
    y[0] = b[0] / d[0]
    for i in range(1, n):
        y[i] = (b[i] - e[i-1] * y[i-1]) / d[i]
    x = np.zeros(n)
    x[-1] = y[-1] / d[-1]
    for i in range(n-2, -1, -1):
        x[i] = (y[i] - e[i] * x[i+1]) / d[i]
    return x

# u(s) = s(s-1)/2 ist die exakte Loesung
```

| n | max\|u\_h - u\_exact\| |
|---|---|
| 100 | 2.50e-15 |
| 1000 | 5.62e-14 |
| 10000 | 1.24e-12 |

**Ergebnis:** Die Cholesky-Zerlegung ist numerisch stabil. Der Fehler liegt im Bereich der Maschinengenauigkeit und waechst nur minimal mit n.
