---
course: numerik
type: exercise
lecture: 6
date: 2026-05-14
tags:
  - numerik
  - exercise
  - cg-verfahren
  - gerschgorin
  - eigenwertabschaetzung
source: NumI-06.pdf
---

# Uebung 6 — CG-Verfahren und Eigenwertabschaetzungen

**Aufgaben-PDF:** [[numerik/resources/NumI-06.pdf|NumI-06.pdf]]
**Loesungen:** [[numerik/exercises/06/num-solution-06|num-solution-06]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — CG-Verfahren auf der Membran-Matrix|Aufgabe 1 — CG-Verfahren auf der Membran-Matrix]]
- [[#Aufgabe 2 — Gerschgorin-Kreise|Aufgabe 2 — Gerschgorin-Kreise]]
- [[#Aufgabe 3 — Eigenwertabschaetzung fuer symmetrische Matrizen|Aufgabe 3 — Eigenwertabschaetzung fuer symmetrische Matrizen]]

---

## Aufgabe 1 — CG-Verfahren auf der Membran-Matrix

Das einfachste physikalische Modell fuer eine elastische Membran fuehrt auf ein lineares SPD-Gleichungssystem mit Block-Tridiagonal-Matrix

$$A = \begin{pmatrix} B & -I \\ -I & B & -I \\ & \ddots & \ddots & \ddots \\ & & -I & B & -I \\ & & & -I & B \end{pmatrix} \in \mathbb{R}^{m^2 \times m^2}, \quad B = \begin{pmatrix} 4 & -1 \\ -1 & 4 & -1 \\ & \ddots & \ddots & \ddots \\ & & -1 & 4 & -1 \\ & & & -1 & 4 \end{pmatrix} \in \mathbb{R}^{m \times m}.$$

In der Datei `membran_lib.py` finden Sie die Funktion `A, b = system(m)`, die $A$ als CSR-Sparse-Matrix erzeugt.

Loesen Sie das System fuer $m = 50, 100, 200$ (also $n = 2500, 10000, 40000$) mit dem CG-Verfahren **ohne** Vorkonditionierer. Iterieren Sie, bis $\|r_k\|_2 / \|r_0\|_2 \leq 10^{-6}$ ist, und plotten Sie die normierten Residuen halblogarithmisch.

Die deformierte Membran kann mit `plotxk(x)` visualisiert werden.

---

## Aufgabe 2 — Gerschgorin-Kreise

Sei

$$A = \begin{pmatrix} 4 & -2 & 3 \\ 3 & 2 & -2 \\ 2 & -1 & 3 \end{pmatrix}.$$

Schaetzen Sie die Lage der Eigenwerte von $A$ durch Anwendung des Satzes von Gerschgorin auf $A$ und $A^T$ ab. Skizzieren Sie die Gerschgorin-Kreise.

---

## Aufgabe 3 — Eigenwertabschaetzung fuer symmetrische Matrizen

**(a)** Beweisen Sie folgenden Satz:

Sei $A \in \mathbb{R}^{n \times n}$ symmetrisch mit reellen Eigenwerten $\lambda_1, \ldots, \lambda_n$. Fuer beliebige $\lambda \in \mathbb{R}$ und $x \in \mathbb{R}^n \setminus \{0\}$ gilt mit $d = Ax - \lambda x$:

$$\min_{i = 1, \ldots, n} |\lambda - \lambda_i| \leq \frac{\|d\|_2}{\|x\|_2}.$$

**Anleitung:** Benutzen Sie, dass eine Basis aus Eigenvektoren von $A$ existiert.

**(b)** Die Matrix

$$A = \begin{pmatrix} 6 & 4 & 3 \\ 4 & 6 & 3 \\ 3 & 3 & 7 \end{pmatrix}$$

besitzt die Eigenwerte $2$, $4$ und $13$. Mit $\lambda = 12$ und $x = (3, 4, 5)^T$: wenden Sie (a) an, um eine Lageabschaetzung eines Eigenwertes zu erhalten.
