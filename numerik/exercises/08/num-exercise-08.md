---
course: numerik
type: exercise
lecture: 8
date: 2026-06-08
tags:
  - numerik
  - exercise
  - newton-verfahren
  - sekanten-verfahren
  - nullstellen
  - fixpunktiteration
source: NumI-08.pdf
---

# Uebung 8 — Newton- und Sekanten-Verfahren

**Aufgaben-PDF:** [[numerik/resources/NumI-08.pdf|NumI-08.pdf]]
**Loesungen:** [[numerik/exercises/08/num-solution-08|num-solution-08]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Newton vs. Sekante (Weltbevoelkerung)|Aufgabe 1 — Newton vs. Sekante (Weltbevoelkerung)]]
- [[#Aufgabe 2 — Newton als Kontraktion|Aufgabe 2 — Newton als Kontraktion]]
- [[#Aufgabe 3 — Lineare vs. quadratische Konvergenz|Aufgabe 3 — Lineare vs. quadratische Konvergenz]]
- [[#Aufgabe 4 — Newton fuer ein nichtlineares System|Aufgabe 4 — Newton fuer ein nichtlineares System]]

---

## Aufgabe 1 — Newton vs. Sekante (Weltbevoelkerung)

Vergleicht man das **Newton-Verfahren**

$$x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}, \quad k = 0, 1, \ldots$$

mit dem **Sekanten-Verfahren**

$$x_{k+1} = x_k - \frac{x_k - x_{k-1}}{f(x_k) - f(x_{k-1})} f(x_k), \quad k = 0, 1, \ldots$$

so faellt auf, dass beim Sekanten-Verfahren (ausser im ersten Schritt) pro Schritt nur **eine** Funktionsauswertung $f(x_k)$ anfaellt, wogegen beim Newton-Verfahren **zwei** Auswertungen ($f(x_k)$ und $f'(x_k)$) noetig sind.

Wenden Sie beide Verfahren an, um folgendes Problem zu loesen. Die Funktion

$$b(x) = \frac{a}{1 - c\,e^{-dx}}, \quad a = 9.8606,\quad c = -1.1085 \cdot 10^{25},\quad d = 0.029$$

beschreibt naeherungsweise die Weltbevoelkerung (in Milliarden) im Jahr $x$. Bestimmen Sie den Zeitpunkt $x$, an dem die Weltbevoelkerung die **9-Milliarden-Grenze** uebersteigt.

Startwerte: Newton $x_0 = 1961$; Sekante $x_{-1} = 1961,\ x_0 = 2000$. Vergleichen Sie die Genauigkeit. Welches Verfahren benoetigt weniger Aufwand, um Maschinengenauigkeit zu erreichen?

---

## Aufgabe 2 — Newton als Kontraktion

Untersuchen Sie auf $X = [1, 2]$ die Funktion

$$f(x) = x + \ln(x) - 2$$

mit Hilfe des Newton-Verfahrens auf Nullstellen.

**(a)** Zeigen Sie, dass das Newton-Verfahren auf $X$ eine Kontraktion mit $\alpha = \tfrac{1}{4}$ ist.

**(b)** Geben Sie mit Hilfe der **a-priori-Abschaetzung** eine obere Schranke fuer die Anzahl an Iterationen an, die noetig sind, um bei $x_0 = 1$ den absoluten Fehler sicher unter $10^{-6}$ zu druecken.

**(c)** Fuehren Sie die Newton-Iteration fuer $x_0 = 1$ durch. Benutzen Sie die **a-posteriori-Fehlerabschaetzung** und stoppen Sie, sobald der absolute Fehler sicher kleiner als $10^{-6}$ ist.

---

## Aufgabe 3 — Lineare vs. quadratische Konvergenz

Die Funktion $f(x) = \arctan(x) - x$ hat $x = 0$ als einzige Nullstelle.

**(a)** Zeigen Sie, dass das Newton-Verfahren nur **linear** konvergiert.

**(b)** Wenden Sie die in der Vorlesung besprochenen **Modifikationen** an, um ein quadratisch konvergentes Verfahren zu erhalten. Bestimmen Sie zu $x_0 = 1$ die Iterierten $x_1, \ldots, x_4$ des Standard-Newton-Verfahrens sowie die Iterierten der beiden Modifikationen.

---

## Aufgabe 4 — Newton fuer ein nichtlineares System

Suchen Sie mit Hilfe des Newton-Verfahrens Nullstellen von

$$f : \begin{pmatrix} x \\ y \end{pmatrix} \longmapsto \begin{pmatrix} \sin(x) - y \\ e^{-y} - x \end{pmatrix}.$$
