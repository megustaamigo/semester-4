---
course: numerik
type: exercise
lecture: 11
date: 2026-06-22
tags:
  - numerik
  - exercise
  - numerische-integration
  - trapezregel
  - simpson-regel
  - gauss-quadratur
  - romberg
  - bulirsch
  - adaptive-integration
source: NumI-11.pdf
---

# Uebung 11 — Numerische Integration

**Aufgaben-PDF:** [[numerik/resources/NumI-11.pdf|NumI-11.pdf]]
**Loesungen:** [[numerik/exercises/11/num-solution-11|num-solution-11]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Trapez, Simpson und Gauss-Quadratur|Aufgabe 1 — Trapez, Simpson und Gauss-Quadratur]]
- [[#Aufgabe 2 — Romberg- und Bulirsch-Extrapolation|Aufgabe 2 — Romberg- und Bulirsch-Extrapolation]]
- [[#Aufgabe 3 — Adaptive Trapezregel|Aufgabe 3 — Adaptive Trapezregel]]

---

## Aufgabe 1 — Trapez, Simpson und Gauss-Quadratur

Betrachten Sie die Integrationsaufgabe

$$\int_0^1 \frac{1}{1 + x^2}\,dx.$$

**(a)** Berechnen Sie den **exakten** Wert des Integrals.
**(b)** Berechnen Sie eine Naeherung mit Hilfe der **summierten Trapez-Regel** mit 8 Trapezen und bestimmen Sie den absoluten Fehler.
**(c)** Wenden Sie die **summierte Simpson-Regel** mit Streifenzahl 4 an. Wie gross ist der absolute Fehler?
**(d)** Berechnen Sie mit dem **Gauss-Verfahren $G_2(f)$** eine Naeherung und bestimmen Sie den absoluten Fehler. Fuer $G_2(f)$ gilt auf dem Intervall $[-1,1]$

$$x_0 = -\sqrt{\tfrac{3}{5}}, \quad x_1 = 0, \quad x_2 = \sqrt{\tfrac{3}{5}}, \qquad \beta_0 = \tfrac{5}{9}, \quad \beta_1 = \tfrac{8}{9}, \quad \beta_2 = \tfrac{5}{9}.$$

---

## Aufgabe 2 — Romberg- und Bulirsch-Extrapolation

Berechnen Sie Naeherungen der Ordnung $2, 4, 6, 8, 10, 12, 14, 16$ von

$$\int_{-1}^{1} \sin(\pi x^2)\,dx$$

mit Hilfe des Extrapolationsverfahrens nach

**(a)** **Romberg**
**(b)** **Bulirsch**

Vergleichen Sie die Ergebnisse mit denen der Funktion `quad` aus `scipy.integrate`.

---

## Aufgabe 3 — Adaptive Trapezregel

Betrachten Sie das Integral

$$\int_0^4 \frac{3x}{x + 1}\,dx.$$

**(a)** Berechnen Sie den **exakten** Wert des Integrals.

**(b)** Approximieren Sie das Integral mit Hilfe der **adaptiven Trapezregel**. Bei der adaptiven Integration auf Basis der Trapezregel benutzt man den Fehlerschaetzer

$$\tilde F = \frac{|\tilde T - T|}{3}$$

fuer $\tilde T$, wobei

$$T = \frac{b_i - a_i}{2}\big(f(a_i) + f(b_i)\big), \qquad \tilde T = \frac{b_i - a_i}{4}\Big(f(a_i) + 2 f\big(\tfrac{a_i + b_i}{2}\big) + f(b_i)\Big).$$

Starten Sie mit **einem Trapez** und benutzen Sie als Abbruchkriterium $\tilde F < 0.05$. Vergleichen Sie das Ergebnis mit dem exakten Wert des Integrals.
