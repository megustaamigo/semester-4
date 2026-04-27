---
course: numerik
type: exercise
lecture: 3
date: 2026-04-23
tags:
  - numerik
  - exercise
  - kondition
  - prager-oettli
  - lu-zerlegung
  - spalten-pivot
source: NumI-03.pdf
---

# Uebung Numerik I — Blatt 3

**Quelle:** [[numerik/resources/NumI-03.pdf|NumI-03.pdf]]
**Praktikum:** `numerik/repos/numerik/blatt03/`
**Loesung:** [[numerik/exercises/03/num-solution-03|Loesungen Blatt 3]]

---


## Inhaltsverzeichnis

- [[#Aufgabe 1 — Kondition einer Matrix|Aufgabe 1 — Kondition einer Matrix]]
- [[#Aufgabe 2 — Prager und Oettli (Stoerungsanalyse)|Aufgabe 2 — Prager und Oettli (Stoerungsanalyse)]]
- [[#Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche|Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche]]

---

## Aufgabe 1 — Kondition einer Matrix

Fuer $\beta > 1$ betrachten wir die Matrix:

$$A = \begin{pmatrix} 1 & 0 & \cdots & 0 & \beta \\ -\beta & 1 & 0 & \cdots & 0 \\ 0 & \ddots & \ddots & \ddots & \vdots \\ \vdots & \ddots & -\beta & 1 & 0 \\ 0 & \cdots & 0 & -\beta & 0 \end{pmatrix} \in \mathbb{R}^{n \times n}$$

mit $A^{-1} = -\frac{1}{\beta^n} \cdot (\text{obere Dreiecksmatrix mit Potenzen von } \beta)$

**Aufgabe:** Berechnen Sie die Kondition $\kappa_\infty(A)$. Ist die Kondition unabhaengig von der Dimension $n$ beschraenkt?

---

## Aufgabe 2 — Prager und Oettli (Stoerungsanalyse)

### (a)

Fuer das lineare Gleichungssystem $Ax = b$ mit:

$$A = \begin{pmatrix} 2 & -1 \\ 1 & 1 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 2 \end{pmatrix}$$

wissen wir, dass die Koeffizienten von $A$ und $b$ gestoert sind mit:

$$|\Delta A| \leq \frac{1}{10} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq \frac{1}{10} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

Durch ein numerisches Verfahren haben wir als Naeherungsloesung $\tilde{x} = (0.9, 1.1)^T$ erhalten. Testen Sie mit Hilfe des Resultats von Prager und Oettli, ob $\tilde{x}$ als Loesung akzeptiert werden kann.

### (b)

Wiederholen Sie die Betrachtungen von (a) fuer:

$$A = \begin{pmatrix} 1 & -1 \\ -1 & 1.001 \end{pmatrix}, \quad b = \begin{pmatrix} 1 \\ 0 \end{pmatrix}$$

$$|\Delta A| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 & 1 \\ 1 & 1 \end{pmatrix}, \quad |\Delta b| \leq 5 \cdot 10^{-4} \begin{pmatrix} 1 \\ 1 \end{pmatrix}$$

mit $\tilde{x} = (501, 500)^T$.

---

## Aufgabe 3 — LU-Zerlegung mit Spalten-Pivot-Suche

Implementieren Sie die LU-Zerlegung mit Spalten-Pivot-Suche. Testen Sie die Routinen mit einfachen Beispielsystemen.

Loesen Sie dann das lineare Gleichungssystem mit Matrix $A$ aus Aufgabe 1 und rechter Seite:

$$b = (1 + \beta, \underbrace{1 - \beta, \ldots, 1 - \beta}_{n-2 \text{ mal}}, -\beta)^T$$

fuer $\beta = 10$ und $n = 10, 20, 100$ jeweils mit/ohne Spalten-Pivot-Suche. Vergleichen Sie die Resultate mit der exakten Loesung:

$$x = (1, \ldots, 1)^T$$
