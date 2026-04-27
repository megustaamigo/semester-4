---
course: numerik
type: solution
lecture: 3
date: 2026-04-23
tags:
  - numerik
  - solution
  - python
  - lu-zerlegung
  - spalten-pivot
  - kondition
  - prager-oettli
source: numerik/repos/numerik/blatt03/
---

# Loesungen — Blatt 3

**Aufgaben:** [[numerik/exercises/03/num-exercise-03|Uebung 3]]
**PDF:** [[numerik/exercises/03/num-solution-03.pdf|num-solution-03.pdf]]
**Quellcode:** `numerik/repos/numerik/blatt03/`

---


## Inhaltsverzeichnis

- [[#Aufgabe 1 — Kondition $\kappa_\infty(A)$|Aufgabe 1 — Kondition $\kappa_\infty(A)$]]
- [[#Aufgabe 2 — Prager und Oettli|Aufgabe 2 — Prager und Oettli]]
- [[#Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche|Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche]]

---

## Aufgabe 1 — Kondition $\kappa_\infty(A)$

### $\|A\|_\infty$ berechnen

$$A = \begin{pmatrix} 1 & 0 & \cdots & 0 & \beta \\ -\beta & 1 & 0 & \cdots & 0 \\ 0 & \ddots & \ddots & \ddots & \vdots \\ \vdots & \ddots & -\beta & 1 & 0 \\ 0 & \cdots & 0 & -\beta & 0 \end{pmatrix}$$

Zeilensummen der Betraege:
- Zeile 1: $|1| + |\beta| = 1 + \beta$
- Zeilen 2 bis $n-1$: $|-\beta| + |1| = 1 + \beta$
- Zeile $n$: $|-\beta| + |0| = \beta$

$$\|A\|_\infty = 1 + \beta$$

### $\|A^{-1}\|_\infty$ berechnen

Aus der gegebenen Inversen $A^{-1} = -\frac{1}{\beta^n} \cdot M$ mit der oberen Dreiecksmatrix $M$:

Die letzte Zeile von $A^{-1}$ hat die groessten Betraege. Ihre Zeilensumme ist:

$$\frac{1}{\beta^n}(\beta^{n-1} + \beta^{n-2} + \cdots + \beta + 1) = \frac{1}{\beta^n} \cdot \frac{\beta^n - 1}{\beta - 1}$$

Fuer die erste Zeile: Zeilensumme = $\frac{1}{\beta^n}(\beta^{n-1} + \beta^{n-2} + \cdots + \beta) = \frac{1}{\beta^n} \cdot \frac{\beta^n - \beta}{\beta - 1}$

Die letzte Zeile dominiert, also:

$$\|A^{-1}\|_\infty = \frac{\beta^n - 1}{\beta^n(\beta - 1)}$$

### Kondition

$$\kappa_\infty(A) = (1 + \beta) \cdot \frac{\beta^n - 1}{\beta^n(\beta - 1)}$$

Fuer $n \to \infty$: $\frac{\beta^n - 1}{\beta^n} \to 1$, also:

$$\kappa_\infty(A) \to \frac{1 + \beta}{\beta - 1}$$

**Die Kondition ist unabhaengig von $n$ beschraenkt** durch $\frac{1+\beta}{\beta-1}$.

Fuer $\beta = 10$: $\kappa_\infty \approx \frac{11}{9} \approx 1.22$ — die Matrix ist stets gut konditioniert, obwohl die LU-Zerlegung ohne Pivotsuche versagt!

---

## Aufgabe 2 — Prager und Oettli

### Satz

$\tilde{x}$ ist akzeptabel, falls komponentenweise gilt:

$$|r| \leq |\Delta A| \cdot |\tilde{x}| + |\Delta b|$$

wobei $r = b - A\tilde{x}$ das Residuum ist.

### (a)

$$A = \begin{pmatrix} 2 & -1 \\ 1 & 1 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 2 \end{pmatrix}, \quad \tilde{x} = \begin{pmatrix} 0.9 \\ 1.1 \end{pmatrix}$$

$$|\Delta A| \leq \frac{1}{10} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq \frac{1}{10} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

**Residuum:**

$$r = b - A\tilde{x} = \begin{pmatrix} 1 \\ 2 \end{pmatrix} - \begin{pmatrix} 2 \cdot 0.9 + (-1) \cdot 1.1 \\ 1 \cdot 0.9 + 1 \cdot 1.1 \end{pmatrix} = \begin{pmatrix} 1 \\ 2 \end{pmatrix} - \begin{pmatrix} 0.7 \\ 2.0 \end{pmatrix} = \begin{pmatrix} 0.3 \\ 0 \end{pmatrix}$$

**Rechte Seite:**

$$|\Delta A| \cdot |\tilde{x}| + |\Delta b| = \frac{1}{10} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix} \begin{pmatrix} 0.9 \\ 1.1 \end{pmatrix} + \frac{1}{10} \begin{pmatrix} 1 \\ 1 \end{pmatrix} = \frac{1}{10} \begin{pmatrix} 2.0 \\ 2.0 \end{pmatrix} + \begin{pmatrix} 0.1 \\ 0.1 \end{pmatrix} = \begin{pmatrix} 0.3 \\ 0.3 \end{pmatrix}$$

**Vergleich:**
- $|r_1| = 0.3 \leq 0.3$ -- erfuellt
- $|r_2| = 0 \leq 0.3$ -- erfuellt

$\tilde{x} = (0.9, 1.1)^T$ **kann als Loesung akzeptiert werden**.

### (b)

$$A = \begin{pmatrix} 1 & -1 \\ -1 & 1.001 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 0 \end{pmatrix}, \quad \tilde{x} = \begin{pmatrix} 501 \\ 500 \end{pmatrix}$$

$$|\Delta A| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

**Residuum:**

$$r = \begin{pmatrix} 1 \\ 0 \end{pmatrix} - \begin{pmatrix} 501 - 500 \\ -501 + 500.5 \end{pmatrix} = \begin{pmatrix} 1 \\ 0 \end{pmatrix} - \begin{pmatrix} 1 \\ -0.5 \end{pmatrix} = \begin{pmatrix} 0 \\ 0.5 \end{pmatrix}$$

**Rechte Seite:**

$$|\Delta A| \cdot |\tilde{x}| + |\Delta b| = 5 \cdot 10^{-4} \begin{pmatrix} 1001 \\ 1001 \end{pmatrix} + 5 \cdot 10^{-4} \begin{pmatrix} 1 \\ 1 \end{pmatrix} = \begin{pmatrix} 0.5010 \\ 0.5010 \end{pmatrix}$$

**Vergleich:**
- $|r_1| = 0 \leq 0.501$ -- erfuellt
- $|r_2| = 0.5 \leq 0.501$ -- knapp erfuellt

$\tilde{x} = (501, 500)^T$ **besteht den Test knapp**. Aber: Die exakte Loesung ist $x = (1001, 1000)^T$, d.h. $\tilde{x}$ weicht um fast 50% ab! Dies liegt an der schlechten Kondition: $\kappa_\infty(A) \approx 4004$. Der Prager-Oettli-Test bestaetigt nur Konsistenz mit den Datenunsicherheiten, nicht Naehe zur wahren Loesung.

---

## Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche

### Implementierung

```python
import numpy as np

def zerlegung_ohne_pivot(A):
    n = len(A)
    LU = A.astype(float).copy()
    for k in range(n):
        for i in range(k + 1, n):
            LU[i, k] /= LU[k, k]
            LU[i, k+1:] -= LU[i, k] * LU[k, k+1:]
    return LU, list(range(n))

def zerlegung_spalten_pivot(A):
    n = len(A)
    LU = A.astype(float).copy()
    p = list(range(n))
    for k in range(n):
        pivot = k + np.argmax(np.abs(LU[k:, k]))
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

def solve(A, b, mit_pivot=True):
    LU, p = (zerlegung_spalten_pivot if mit_pivot else zerlegung_ohne_pivot)(A)
    return rueckwaerts(LU, vorwaerts(LU, permutation(p, b)))
```

### Matrix A und Vektor b aus Aufgabe 1

```python
def matrix_A(n, beta):
    A = np.zeros((n, n))
    for i in range(n):
        A[i, i] = 1.0 if i < n-1 else 0.0
        if i > 0: A[i, i-1] = -beta
        if i == 0: A[0, n-1] = beta
    return A

def vektor_b(n, beta):
    b = np.full(n, 1.0 - beta)
    b[0] = 1.0 + beta
    b[-1] = -beta
    return b
```

### Ergebnisse ($\beta = 10$, exakte Loesung $x = (1,\ldots,1)^T$)

| n | Fehler ohne Pivot | Fehler mit Pivot |
|---|---|---|
| 10 | 0.00e+00 | 2.22e-16 |
| 20 | 1.00e+00 | 3.33e-16 |
| 100 | 1.52e+82 | 3.33e-16 |

**Ergebnis:** Ohne Spalten-Pivot-Suche waechst der Fehler katastrophal mit n, da die Eintraege in L exponentiell anwachsen (Faktor $\beta$ pro Eliminationsschritt). Mit Spalten-Pivot-Suche bleibt der Fehler im Bereich der Maschinengenauigkeit, da $|l_{ij}| \leq 1$ garantiert wird.
