---
course: numerik
type: exercise
lecture: 9
date: 2026-06-08
tags:
  - numerik
  - exercise
  - interpolation
  - lagrange
  - newton-darstellung
  - dividierte-differenzen
  - runge-phaenomen
  - kubischer-spline
source: NumI-09.pdf
---

# Uebung 9 — Polynominterpolation und kubische Splines

**Aufgaben-PDF:** [[numerik/resources/NumI-09.pdf|NumI-09.pdf]]
**Loesungen:** [[numerik/exercises/09/num-solution-09|num-solution-09]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Interpolationspolynom (Lagrange & Newton)|Aufgabe 1 — Interpolationspolynom (Lagrange & Newton)]]
- [[#Aufgabe 2 — Dividierte Differenzen, Horner-Schema, Runge-Phaenomen|Aufgabe 2 — Dividierte Differenzen, Horner-Schema, Runge-Phaenomen]]
- [[#Aufgabe 3 — Natuerlicher kubischer Spline|Aufgabe 3 — Natuerlicher kubischer Spline]]

---

## Aufgabe 1 — Interpolationspolynom (Lagrange & Newton)

Bestimmen Sie das Interpolationspolynom zu den folgenden Wertepaaren

| $i$ | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| $x_i$ | $-1$ | $0$ | $1$ | $3$ |
| $y_i$ | $-2$ | $4$ | $6$ | $22$ |

**(a)** mit Hilfe von **Lagrange-Polynomen**
**(b)** ueber die **Newton-Darstellung**

---

## Aufgabe 2 — Dividierte Differenzen, Horner-Schema, Runge-Phaenomen

Das Interpolationspolynom in Newton-Darstellung lautet

$$p_n(x) = d_{00} + d_{10}(x - x_0) + \ldots + d_{n0}(x - x_0)\cdots(x - x_{n-1}),$$

wobei $d_{k0}$ dividierte Differenzen sind:

$$d_{ii} = y_i \ (i = 0,1,\ldots), \qquad d_{nm} = \frac{d_{n\,m+1} - d_{n-1\,m}}{x_n - x_m}\ (n > m).$$

Zur Auswertung schreibt man $p_n(x)$ analog zum Horner-Schema:

$$q_0 = d_{n0}, \qquad q_k = d_{n-k\,0} + (x - x_{n-k})\,q_{k-1}\ (k = 1,\ldots,n), \qquad p_n(x) = q_n.$$

**(a)** Implementieren Sie je eine Funktion zur Berechnung der **dividierten Differenzen** und zur **Auswertung** nach dem Horner-aehnlichen Schema. Testen Sie anhand des Vorlesungsbeispiels: die 3 Punkte $(0,3), (1,2), (3,6)$ werden interpoliert und das Polynom bei $x = 2$ ausgewertet.

**(b)** Interpolationspolynome hohen Grades neigen zu starken Oszillationen (**Runge-Phaenomen**). Betrachten Sie

$$f(x) = \frac{1}{1 + x^2}, \quad x \in [-5, 5],$$

mit $m$ aequidistanten Stuetzstellen $x_i = -5 + \tfrac{10}{m-1}\,i,\ i = 0,\ldots,m-1$. Berechnen Sie fuer $m = 7, 9, 11$ das Interpolationspolynom und vergleichen Sie den Graphen von $f$ mit dem des Interpolationspolynoms.

**(c)** Berechnen Sie analog fuer die nicht-aequidistanten (**Chebyshev-**)Stuetzstellen

$$\tilde x_i = -5\cos\!\left(\pi\,\frac{2i+1}{2m}\right), \quad \tilde y_i = f(\tilde x_i), \quad i = 0,\ldots,m-1,$$

das Interpolationspolynom und vergleichen Sie es mit dem aequidistanten Fall fuer $m = 7, 9, 11$.

---

## Aufgabe 3 — Natuerlicher kubischer Spline

Bestimmen Sie den **natuerlichen** kubischen Spline zu den Wertepaaren

| $x_i$ | $-1$ | $0$ | $2$ |
|---|---|---|---|
| $y_i$ | $16$ | $8$ | $16$ |

indem Sie

**(a)** die 8 Bedingungsgleichungen fuer $s, s', s''$
**(b)** das Gleichungssystem fuer die Momente $\beta_i$

aufstellen und aufloesen.
