---
course: numerik
type: exercise
lecture: 1
date: 2026-04-09
tags:
  - numerik
  - exercise
  - python
  - quadratische-gleichung
  - reihenentwicklung
  - numerische-integration
  - wurzelberechnung
source: NumI-01.pdf
---

# Uebung Numerik I — Blatt 1

**Quelle:** [[numerik/resources/NumI-01.pdf|NumI-01.pdf]]
**Dozenten:** M. Grajewski, M. Reissel
**Praktikum:** [[/home/samuel/numerik|Numerik Python Repo (extern)]]

---

## Aufgabe 1 — Quadratische Gleichung (pq-Formel)

Gegeben: $x^2 + 2px - q = 0$ mit $p, q > 0$

**Standardverfahren (pq-Formel):**
$$x_2 = -p + \sqrt{p^2 + q}$$

**Alternatives Verfahren (ueber Vieta):**
- Aus $x_1 \cdot x_2 = -q$ und $x_1 = -p - \sqrt{p^2 + q}$ folgt:
$$x_2 = \frac{-q}{x_1}$$

**Aufgabe:** Beide Algorithmen in Python implementieren (Funktionen mit p, q als Eingabe, x als Ausgabe). Vergleich fuer $p = 10^2 / 10^4 / 10^6 / 10^7 / 10^8$ und $q = 1$. Welches Verfahren ist fuer grosse p genauer?

---

## Aufgabe 2 — Reihenentwicklung ln(1+x)

Fuer den natuerlichen Logarithmus gilt:
$$\ln(1 + x) = \sum_{k=1}^{\infty} (-1)^{k+1} \frac{x^k}{k}$$

**Algorithmus (abgebrochene Reihe bis n):**
- $s = 0$, $x_k = x$
- Fuer $k = 1, \ldots, n$:
  - $s := s + (-1)^{k+1} \frac{x_k}{k}$
  - $x_k := x_k \cdot x$

**Aufgabe:** In Python implementieren. Berechnung fuer $x = 0.9$ und $n = 50$ mit NumPy `float16` und `float64`. Vergleich mit `numpy.log`.

---

## Aufgabe 3 — Numerische Integration (Rechteck-Summen & Trapezregel)

**Linke Rechteck-Summe:**
$$h \sum_{i=0}^{n-1} f(a + ih)$$

**Rechte Rechteck-Summe:**
$$h \sum_{i=1}^{n} f(a + ih)$$

**Trapezregel:**
$$\frac{h}{2} \left( f(a) + 2 \sum_{i=1}^{n-1} f(a + ih) + f(b) \right)$$

wobei $h = \frac{b-a}{n}$.

**Aufgabe:** Als Python-Funktionen mit Parametern $f, a, b, n$ implementieren. Anwenden auf:
$$\int_{1/10}^{10} \frac{1}{x^2} dx \quad \text{und} \quad \int_1^2 \ln(x) dx$$
Vergleich mit exakter Loesung.

---

## Aufgabe 4 — Vektorisierte Trapezregel

Vektoroperationen in Python sind schneller als Schleifen ueber Skalare.

**Aufgabe:** Trapezregel in Vektornotation umschreiben:
- Vektor mit allen Stuetzstellen $a + ih$ erzeugen
- Integranden $f$ auf den Vektor anwenden
- Ergebnisvektor mit `sum` aufsummieren

---

## Aufgabe 5 — Wurzelberechnung

Naeherung von $y = \sqrt{x}$ nur mit den 4 Grundrechenarten.

### (a) Taylorentwicklung

Iterationsvorschrift:
$$a_0 = y_0 = \sqrt{x_0}$$
$$a_k = \left(\frac{3}{2k} - 1\right)\left(\frac{x}{x_0} - 1\right) a_{k-1}$$
$$y_k = y_{k-1} + a_k$$

**Aufgabe:** Implementieren. Naeherungen von $y = \sqrt{2}$ fuer $x_0 = 1$ (also $a_0 = y_0 = 1$) und $x_0 = 4$ (also $a_0 = y_0 = 2$). Abweichung vom exakten Wert bestimmen. Wie viele Schritte bis Abweichung < 0.005?

### (b) Heron-Verfahren

$$y_k = \frac{1}{2}\left(y_{k-1} + \frac{x}{y_{k-1}}\right)$$

**Aufgabe:** Implementieren mit Startwerten $y_0 = 1$ bzw. $y_0 = 2$. Abweichung bestimmen und mit Taylorentwicklung vergleichen.
