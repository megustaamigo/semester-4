---
course: numerik
type: exercise
lecture: 12
date: 2026-06-29
tags:
  - numerik
  - exercise
  - optimierung
  - stationaere-punkte
  - hessematrix
  - positiv-definit
source: NumI-12.pdf
---

# Uebung 12 — Optimierung: Stationaere Punkte & quadratische Funktionen

**Aufgaben-PDF:** [[numerik/resources/NumI-12.pdf|NumI-12.pdf]]
**Loesungen:** [[numerik/exercises/12/num-solution-12|num-solution-12]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Stationaere Punkte und Klassifikation|Aufgabe 1 — Stationaere Punkte und Klassifikation]]
- [[#Aufgabe 2 — Quadratische Funktion|Aufgabe 2 — Quadratische Funktion]]

---

## Aufgabe 1 — Stationaere Punkte und Klassifikation

Bestimmen Sie alle **stationaeren Punkte** der Funktion

$$f(x_1, x_2) = 1 - \frac{1}{1 + x_1^2} + \frac{1}{1 + x_2^2} + \frac{x_2^2}{4}.$$

Handelt es sich um lokale Minima, Maxima oder um Sattelpunkte?

---

## Aufgabe 2 — Quadratische Funktion

Gegeben sei die quadratische Funktion $q : \mathbb{R}^n \to \mathbb{R}$ durch

$$q(x) = \tfrac{1}{2}\, x^T A x + b^T x + c$$

mit $A \in \mathbb{R}^{n \times n}$, $b \in \mathbb{R}^n$, $c \in \mathbb{R}$.

**(a)** Berechnen Sie $q'(x)$.

**(b)** Berechnen Sie $q''(x)$.

**(c)** Zeigen Sie: Man kann immer davon ausgehen, dass $A$ symmetrisch ist, weil man in $q$ die Matrix $A$ durch eine symmetrische Matrix $B$ ersetzen kann, ohne $q$ selbst zu veraendern.

**(d)** Sei jetzt $A$ **positiv definit**. Beweisen Sie, dass genau eine Nullstelle $\bar x$ von $q'$ existiert und dass $\bar x$ ein lokales Minimum von $q$ ist.
