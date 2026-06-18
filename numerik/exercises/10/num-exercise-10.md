---
course: numerik
type: exercise
lecture: 10
date: 2026-06-15
tags:
  - numerik
  - exercise
  - ausgleichsrechnung
  - lineare-ausgleichsrechnung
  - normalgleichungen
  - inverse-probleme
  - pseudoinverse
  - tsvd
  - regularisierung
source: NumI-10.pdf
---

# Uebung 10 — Ausgleichsrechnung und regularisierte inverse Probleme

**Aufgaben-PDF:** [[numerik/resources/NumI-10.pdf|NumI-10.pdf]]
**Loesungen:** [[numerik/exercises/10/num-solution-10|num-solution-10]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Ausgleichspolynom p(x) = a + bx^2|Aufgabe 1 — Ausgleichspolynom p(x) = a + bx^2]]
- [[#Aufgabe 2 — Entschaerfung eines Signals (Pseudoinverse & TSVD)|Aufgabe 2 — Entschaerfung eines Signals (Pseudoinverse & TSVD)]]

---

## Aufgabe 1 — Ausgleichspolynom p(x) = a + bx^2

Berechnen Sie fuer die Punkte $(x_i, y_i),\ i = 0,\ldots,3$,

| $i$ | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| $x_i$ | $-2$ | $0$ | $1$ | $2$ |
| $y_i$ | $4$ | $0$ | $1$ | $-4$ |

das **Ausgleichspolynom** der Form $p(x) = a + b x^2$.

---

## Aufgabe 2 — Entschaerfung eines Signals (Pseudoinverse & TSVD)

Beim Fotografieren entstehen "unscharfe" Bilder: scharfe Konturen werden "aufgeweicht". Uebertragen auf "eindimensionale" Fotos (Signale als Vektoren $x$) laesst sich die Unschaerfe als Multiplikation mit der Matrix

$$A = \big(a_{ij}\big)_{i,j=0,\ldots,n}, \qquad a_{ij} = \frac{c}{n}\,e^{-\left(\frac{i-j}{\sqrt{2}\,n\gamma}\right)^2}, \qquad c = \frac{1}{\gamma\sqrt{2\pi}}$$

modellieren. Betrachten Sie fuer $n = 100$, $\gamma = 0.05$ das Original-Signal $x$ mit

$$x_i = \begin{cases} 1 & 45 \le i \le 55 \\ \tfrac12 & 60 \le i \le 65 \\ 0 & \text{sonst} \end{cases}$$

und das zugehoerige unscharfe Bild-Signal $b = Ax$. Stoeren Sie $b$ mit einem mit $\delta = 10^{-6}$ skalierten standardnormalverteilten Zufallsvektor $\triangle b$ (Funktion `randn` aus `numpy.random`) und versuchen Sie, aus $b + \triangle b$ eine Naeherung fuer das Original-Signal $x$ zu bestimmen. Benutzen Sie dazu

**(a)** die **Pseudoinverse** `pinv` aus `numpy.linalg` und berechnen Sie $A^+(b + \triangle b)$.

**(b)** **TSVD** mit Parameter $\alpha = 10^k$, $k = 0, -1, \ldots, -8$, wobei Sie die SVD mit der Funktion `svd` aus `numpy.linalg` berechnen lassen koennen.
