---
course: numerik
type: exercise
lecture: 13
date: 2026-07-06
tags:
  - numerik
  - exercise
  - optimierung
  - trust-region
  - bfgs
  - quasi-newton
  - armijo
source: NumI-13.pdf
---

# Uebung 13 — Optimierung: Trust-Region & BFGS

**Aufgaben-PDF:** [[numerik/resources/NumI-13.pdf|NumI-13.pdf]]
**Loesungen:** [[numerik/exercises/13/num-solution-13|num-solution-13]]

---

## Inhaltsverzeichnis

- [[#Aufgabe 1 — Trust-Region: restringierte vs. unrestringierte Minimierung|Aufgabe 1 — Trust-Region: restringierte vs. unrestringierte Minimierung]]
- [[#Aufgabe 2 — BFGS-Quasi-Newton mit Armijo-Liniensuche|Aufgabe 2 — BFGS-Quasi-Newton mit Armijo-Liniensuche]]

---

## Aufgabe 1 — Trust-Region: restringierte vs. unrestringierte Minimierung

Bei **Trust-Region-Verfahren** wird in jedem Schritt eine quadratische Approximation der Zielfunktion $f$ ueber den Vertrauensbereich

$$\Omega_k = \{x \mid \|x - x_k\|_2 \le r_k\}$$

minimiert. Wie sich das Ergebnis einer solchen **restringierten** Optimierung vom **nicht restringierten** Fall unterscheidet, soll anhand der folgenden Funktionen untersucht werden. Minimieren Sie

**(a)** $\;f(x_1, x_2) = (x_1 - x_2)^2 + (x_1 + x_2)^2 + x_1$

**(b)** $\;f(x_1, x_2) = (x_1 - x_2)^2 + (x_1 + x_2)^2 + 8x_2$

erst ueber ganz $\mathbb{R}^2$, dann ueber dem **Einheitskreis**

$$\Omega = \{x \mid x \in \mathbb{R}^2,\ \|x\|_2 \le 1\}.$$

---

## Aufgabe 2 — BFGS-Quasi-Newton mit Armijo-Liniensuche

Implementieren Sie das **BFGS-Quasi-Newton-Verfahren** mit **Armijo-Liniensuche** (Startwert $\alpha = 1$) und Parametern $\rho = 0.5$ und $\tau = 0.5$. Berechnen Sie die Suchrichtung ueber

$$p_k = -D_k\, f'(x_k).$$

Dabei ist $D_k = B_k^{-1}$ und $B_k$ die BFGS-Naeherung der Hessematrix $f''(x_k)$. Benutzen Sie $D_0 = I$ und die Update-Formel

$$D_{k+1} = \begin{cases} D_k + \kappa_1 h_k h_k^T - \kappa_2\,(h_k v_k^T + v_k h_k^T) & \text{fuer } h_k^T y_k > 0 \\ D_k & \text{sonst} \end{cases}$$

mit

$$h_k = x_{k+1} - x_k, \qquad y_k = f'(x_{k+1}) - f'(x_k), \qquad v_k = D_k y_k,$$
$$\kappa_2 = \frac{1}{h_k^T y_k}, \qquad \kappa_1 = \kappa_2\,(1 + \kappa_2\, y_k^T v_k).$$

Suchen Sie damit Minima der Funktion

$$f(x_1, x_2) = (\sin(x_1) - x_2)^2 + (e^{-x_2} - x_1)^2.$$

Benutzen Sie die Startwerte $(5, 2)$, $(6, 2)$, $(-1, -1)$, $(-2, -2)$.
