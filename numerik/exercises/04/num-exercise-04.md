---
course: numerik
type: exercise
lecture: 4
date: 2026-04-30
tags:
  - numerik
  - exercise
  - householder
  - qr-zerlegung
source: NumI-04.pdf
---

# Uebung 4 — Householder-Verfahren

**Aufgaben-PDF:** [[numerik/resources/NumI-04.pdf|NumI-04.pdf]]
**Loesungen:** [[numerik/exercises/04/num-solution-04|num-solution-04]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$|Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$]]
- [[#Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt|Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt]]
- [[#Aufgabe 3 — Implementierung des Householder-Verfahrens|Aufgabe 3 — Implementierung des Householder-Verfahrens]]

---

## Aufgabe 1 — Householder-Spiegelung in $\mathbb{R}^2$

Zeigen Sie, dass in $\mathbb{R}^2$ jede Householder-Spiegelung

$$Q = I - 2 w w^T, \quad \|w\|_2 = 1$$

die Form

$$Q = \begin{pmatrix} c & s \\ s & -c \end{pmatrix}, \quad c^2 + s^2 = 1$$

besitzt.

**Hinweis:** Benutzen Sie, dass jeder Vektor $w \in \mathbb{R}^2$ mit $\|w\|_2 = 1$ sich als

$$w = \begin{pmatrix} -\sin(\alpha) \\ \cos(\alpha) \end{pmatrix}, \quad \alpha \in \mathbb{R}$$

schreiben laesst.

---

## Aufgabe 2 — Vereinfachungen fuer den Householder-Schritt

Zeigen Sie, dass fuer den ersten Schritt (und damit analog fuer alle weiteren) der Householder-Zerlegung gilt:

$$d = -\mathrm{sign}(a_{11})\, \|a_1\|, \quad v_1 = a_{11} - d, \quad \|v\|_2^2 = -2 v_1 d.$$

Dabei ist $d$ das separat zu speichernde neue Diagonalelement und $v_1$ die erste Komponente von $v$. Damit kann $\|v\|_2$ aus $d$ und $v_1$ berechnet werden, was die Implementierung des Schritts $x = Q^T y$ vereinfacht.

---

## Aufgabe 3 — Implementierung des Householder-Verfahrens

Implementieren Sie das Householder-Verfahren unter Verwendung des Ergebnisses aus Aufgabe 2. Testen Sie das Verfahren zunaechst anhand einfacher Beispielsysteme.

Wenden Sie es dann auf $Ax = b$ mit

$$A = \begin{pmatrix} 1 & 0 & \cdots & 0 & 1 \\ -1 & \ddots & \ddots & \vdots & \vdots \\ -1 & \ddots & 1 & 0 & 1 \\ \vdots & & -1 & 1 & 1 \\ -1 & \cdots & -1 & -1 & 1 \end{pmatrix} \in \mathbb{R}^{n \times n}, \quad b = \begin{pmatrix} 2 \\ 1 \\ 0 \\ \vdots \\ 4 - n \\ 2 - n \end{pmatrix} \in \mathbb{R}^n$$

d.h.

$$b_i = \begin{cases} 3 - i & i = 1, \ldots, n - 1 \\ 2 - n & i = n \end{cases}$$

fuer $n = 10, 50, 100$ an. Die exakte Loesung ist jeweils $x = (1, \ldots, 1)^T$.

Vergleichen Sie das Ergebnis mit dem der LU-Zerlegung mit Spalten-Pivot.
