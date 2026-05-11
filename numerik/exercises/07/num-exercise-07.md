---
course: numerik
type: exercise
lecture: 7
date: 2026-05-21
tags:
  - numerik
  - exercise
  - jacobi-eigenwert
  - givens-rotation
  - hessenberg-form
source: NumI-07.pdf
---

# Uebung 7 — Jacobi-Eigenwertverfahren und Givens-Rotationen

**Aufgaben-PDF:** [[numerik/resources/NumI-07.pdf|NumI-07.pdf]]
**Loesungen:** [[numerik/exercises/07/num-solution-07|num-solution-07]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Jacobi-Verfahren fuer Eigenwerte und Eigenvektoren|Aufgabe 1 — Jacobi-Verfahren fuer Eigenwerte und Eigenvektoren]]
- [[#Aufgabe 2 — Givens-Rotation auf Hessenberg-Form|Aufgabe 2 — Givens-Rotation auf Hessenberg-Form]]

---

## Aufgabe 1 — Jacobi-Verfahren fuer Eigenwerte und Eigenvektoren

Implementieren Sie das Jacobi-Verfahren zur gleichzeitigen Bestimmung aller Eigenwerte und Eigenvektoren einer symmetrischen Matrix. Benutzen Sie zur Berechnung der Matrizen $Q_k$ die Vorzeichenfunktion

$$\mathrm{sign}(t) = \begin{cases} 1 & t \geq 0 \\ -1 & t < 0 \end{cases}$$

(also $\mathrm{sign}(0) = 1$ und nicht wie in der NumPy-Implementierung $\mathrm{sign}(0) = 0$).

Testen Sie zunaechst an einfachen Beispielen. Berechnen Sie dann die Eigenwerte und Eigenvektoren der Block-Tridiagonal-Matrix

$$A = \begin{pmatrix} B & -I \\ -I & B & -I \\ & \ddots & \ddots & \ddots \\ & & -I & B & -I \\ & & & -I & B \end{pmatrix} \in \mathbb{R}^{m^2 \times m^2}, \quad B = \begin{pmatrix} 4 & -1 \\ -1 & 4 & -1 \\ & \ddots & \ddots & \ddots \\ & & -1 & 4 & -1 \\ & & & -1 & 4 \end{pmatrix}$$

fuer $m = 10$. Iterieren Sie, bis die Quadratsumme der Nebendiagonalelemente $< 10^{-3}$ ist. Vergleichen Sie mit den exakten Eigenwerten.

Stellen Sie die Eigenschwingungsformen zu den 4 kleinsten Eigenwerten graphisch dar. Die Datei `membran_lib.py` liefert `Ablock(m)`, `ew_exakt(m)`, `plotev(v)`, `animev(v)`.

---

## Aufgabe 2 — Givens-Rotation auf Hessenberg-Form

Transformieren Sie die Matrix

$$\begin{pmatrix} 10 & 15 & 20 \\ 15 & -50 & 25 \\ 20 & 25 & -75 \end{pmatrix}$$

mit Hilfe von Givens-Rotationen **von Hand** auf Hessenberg-Form.
