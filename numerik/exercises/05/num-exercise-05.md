---
course: numerik
type: exercise
lecture: 5
date: 2026-05-07
tags:
  - numerik
  - exercise
  - jacobi
  - gauss-seidel
  - nachiteration
source: NumI-05.pdf
---

# Uebung 5 — Iterative Verfahren und Nachiteration

**Aufgaben-PDF:** [[numerik/resources/NumI-05.pdf|NumI-05.pdf]]
**Loesungen:** [[numerik/exercises/05/num-solution-05|num-solution-05]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Jacobi- und Gauss-Seidel-Verfahren|Aufgabe 1 — Jacobi- und Gauss-Seidel-Verfahren]]
- [[#Aufgabe 2 — Nachiteration fuer die LU-Zerlegung|Aufgabe 2 — Nachiteration fuer die LU-Zerlegung]]

---

## Aufgabe 1 — Jacobi- und Gauss-Seidel-Verfahren

Betrachten Sie das lineare Gleichungssystem $Ax = b$ mit

$$A = \begin{pmatrix} 2 & 0 & 1 \\ 0 & 1 & 0 \\ 1 & 0 & 2 \end{pmatrix}, \quad b = \begin{pmatrix} 4 \\ 0 \\ 5 \end{pmatrix}.$$

**(a)** Bestimmen Sie die Iterationsmatrizen $B_J$ und $B_{GS}$ des Jacobi- bzw. Gauss-Seidel-Verfahrens und deren Spektralradien.

**(b)** Berechnen Sie die Iterierten $x_1, \ldots, x_4$ fuer das Jacobi- und das Gauss-Seidel-Verfahren zum Startwert $x_0 = (0, 0, 0)^T$. Benutzen Sie jeweils die komponentenweise Darstellung der Verfahren.

---

## Aufgabe 2 — Nachiteration fuer die LU-Zerlegung

Implementieren Sie die Nachiteration fuer die LU-Zerlegung mit Spalten-Pivot-Suche. Wenden Sie das Verfahren auf folgende Gleichungssysteme $Ax = b$ an.

**(a)** Fuer $n = 50, 70, 100$ sei

$$A = \begin{pmatrix} 1 & 0 & \cdots & 0 & 1 \\ -1 & \ddots & \ddots & \vdots & \vdots \\ -1 & \ddots & 1 & 0 & 1 \\ \vdots & & -1 & 1 & 1 \\ -1 & \cdots & -1 & -1 & 1 \end{pmatrix} \in \mathbb{R}^{n \times n}, \quad b_i = \begin{cases} 3 - i & i = 1, \ldots, n - 1 \\ 2 - n & i = n \end{cases}$$

und die exakte Loesung ist jeweils $x = (1, \ldots, 1)^T$.

**(b)** Fuer $n = 5, 10, 15$ sei

$$A = (a_{ij})_{i,j=1,\ldots,n}, \quad a_{ij} = \begin{cases} 0 & j > i \\ 1 & j = i \\ i + j & j < i \end{cases}, \quad b = (1, 0, \ldots, 0)^T,$$

d.h. die exakte Loesung ist immer ganzzahlig und $x_1 = 1$.
