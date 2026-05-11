---
course: numerik
type: solution
lecture: 4
date: 2026-04-30
tags:
  - numerik
  - solution
  - householder
  - qr-zerlegung
source: numerik/repos/numerik/blatt04/
---

# Loesungen — Blatt 4

**Aufgaben:** [[numerik/exercises/04/num-exercise-04|Uebung 4]]
**PDF:** [[numerik/exercises/04/num-solution-04.pdf|num-solution-04.pdf]]
**Quellcode:** `numerik/repos/numerik/blatt04/`

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$|Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$]]
- [[#Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt|Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt]]
- [[#Aufgabe 3 — Implementierung des Householder-Verfahrens|Aufgabe 3 — Implementierung des Householder-Verfahrens]]

---

## Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$

Mit $w = \begin{pmatrix} -\sin\alpha \\ \cos\alpha \end{pmatrix}$ ist $\|w\|_2 = 1$ und

$$w w^T = \begin{pmatrix} \sin^2\alpha & -\sin\alpha\cos\alpha \\ -\sin\alpha\cos\alpha & \cos^2\alpha \end{pmatrix}.$$

Mit den Doppelwinkelformeln $2\sin^2\alpha = 1 - \cos(2\alpha)$, $2\cos^2\alpha = 1 + \cos(2\alpha)$, $2\sin\alpha\cos\alpha = \sin(2\alpha)$ folgt

$$2 w w^T = \begin{pmatrix} 1 - \cos(2\alpha) & -\sin(2\alpha) \\ -\sin(2\alpha) & 1 + \cos(2\alpha) \end{pmatrix}.$$

Daraus ergibt sich

$$Q = I - 2 w w^T = \begin{pmatrix} \cos(2\alpha) & \sin(2\alpha) \\ \sin(2\alpha) & -\cos(2\alpha) \end{pmatrix} = \begin{pmatrix} c & s \\ s & -c \end{pmatrix}$$

mit $c = \cos(2\alpha)$, $s = \sin(2\alpha)$. Wegen $c^2 + s^2 = \cos^2(2\alpha) + \sin^2(2\alpha) = 1$ ist die Behauptung gezeigt. $\square$

---

## Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt

Sei $a_1 = (a_{11}, a_{21}, \ldots, a_{n1})^T$ die erste Spalte und $d := -\mathrm{sign}(a_{11})\|a_1\|$. Setze $v := a_1 - d \cdot e_1$, also $v_1 = a_{11} - d$ und $v_i = a_{i1}$ fuer $i \geq 2$.

**Erste Norm-Identitaet** ($d^2 = \|a_1\|^2$): klar aus der Definition von $d$.

**Berechnung von $\|v\|_2^2$:**

$$\|v\|_2^2 = (a_{11} - d)^2 + \sum_{i=2}^{n} a_{i1}^2 = (a_{11} - d)^2 + (\|a_1\|^2 - a_{11}^2).$$

Mit $\|a_1\|^2 = d^2$:

$$\|v\|_2^2 = a_{11}^2 - 2 a_{11} d + d^2 + d^2 - a_{11}^2 = 2 d^2 - 2 a_{11} d = -2 d (a_{11} - d) = -2 v_1 d. \quad \square$$

> [!tip] Merke
> Durch die Wahl $d = -\mathrm{sign}(a_{11})\|a_1\|$ haben $a_{11}$ und $d$ unterschiedliche Vorzeichen, sodass $v_1 = a_{11} - d$ ohne Ausloeschung berechnet wird. Zudem ist $-2 v_1 d > 0$, also numerisch unkritisch.

---

## Aufgabe 3 — Implementierung des Householder-Verfahrens

### Implementierung

```python
import numpy as np

def householder_zerlegung(A):
    """Speichert v unter/auf Diagonale, R oberhalb, d separat."""
    A = A.astype(float).copy()
    n, m = A.shape
    d = np.zeros(min(n, m))
    for k in range(min(n, m)):
        a = A[k:, k]
        norm = np.linalg.norm(a)
        if norm == 0.0:
            d[k] = 0.0; continue
        sign = 1.0 if a[0] >= 0 else -1.0
        d[k] = -sign * norm
        v1 = a[0] - d[k]
        A[k, k] = v1
        v_norm2 = -2.0 * v1 * d[k]
        v = A[k:, k]
        for j in range(k + 1, m):
            beta = 2.0 * (v @ A[k:, j]) / v_norm2
            A[k:, j] -= beta * v
    return A, d

def householder_qty(QR, d, y):
    x = y.astype(float).copy()
    for k in range(len(d)):
        v = QR[k:, k]
        v_norm2 = -2.0 * v[0] * d[k]
        if v_norm2 == 0.0: continue
        beta = 2.0 * (v @ x[k:]) / v_norm2
        x[k:] -= beta * v
    return x

def householder_solve(A, b):
    QR, d = householder_zerlegung(A)
    n = len(d)
    qtb = householder_qty(QR, d, b)
    x = qtb[:n].copy()
    for i in range(n - 1, -1, -1):
        x[i] = (x[i] - QR[i, i + 1:n] @ x[i + 1:n]) / d[i]
    return x
```

Aus Aufgabe 2 wird $\|v\|^2 = -2 v_1 d$ direkt verwendet — somit muss kein zusaetzlicher Norm-Berechnungsschritt pro Iteration vorgenommen werden.

### Matrix $A$ und Vektor $b$

```python
def matrix_blatt4(n):
    A = -np.tril(np.ones((n, n)), -1)
    np.fill_diagonal(A, 1.0)
    A[:, -1] = 1.0
    return A

def vektor_blatt4(n):
    return np.array([3 - i for i in range(1, n)] + [2 - n], dtype=float)
```

### Ergebnisse (exakte Loesung $x = (1, \ldots, 1)^T$)

| $n$ | Fehler Householder | Fehler LU mit Spalten-Pivot |
|---|---|---|
| 10  | $1.33 \cdot 10^{-15}$ | $0.00$ |
| 50  | $1.69 \cdot 10^{-14}$ | $0.00$ |
| 100 | $6.95 \cdot 10^{-14}$ | $1.00$ |

### Beobachtung

> [!warning] Achtung
> Bei dieser Matrix waechst der Wachstumsfaktor der LU-Zerlegung **trotz Spalten-Pivot-Suche** exponentiell wie $2^{n-1}$ (Beispiel von Wilkinson). Bei $n = 100$ uebersteigt der Wachstumsfaktor die Maschinengenauigkeit und das LU-Ergebnis ist unbrauchbar.

Das Householder-Verfahren ist rueckwaerts stabil mit Wachstumsfaktor in $\mathcal{O}(\sqrt n)$ und liefert daher auch fuer $n = 100$ Ergebnisse nahe der Maschinengenauigkeit.

> [!tip] Merke
> Householder-QR ist robuster als LU-mit-Pivot fuer Matrizen mit grossem Wachstumspotential. Der Mehraufwand ($\sim 2 n^3$ vs. $\sim \tfrac{2}{3} n^3$ Operationen) lohnt sich, sobald die Kondition oder der LU-Wachstumsfaktor problematisch werden.
