---
course: numerik
type: resource
date: 2026-05-21
tags:
  - numerik
  - exam
  - artifacts
---

# Numerik Exam Artifacts

Interactive HTML artifacts for exam prep. Click the link to open in the system browser, or use the embedded preview below (Reading view).

---

## LU-Zerlegung 3×3

Schrittweise Visualisierung der LU-Zerlegung einer $3\times3$-Matrix per Gauß-Elimination: $A = L \cdot U$ (untere mal obere Dreiecksmatrix). Zeigt die Wahl der Pivotelemente $a_{kk}^{(k)}$ und die Berechnung der Multiplikatoren $m_{ik} = a_{ik}^{(k)} / a_{kk}^{(k)}$.

- **Open in browser:** [lu_zerlegung_3x3.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/lu_zerlegung_3x3.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/lu_zerlegung_3x3.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Jacobi-Verfahren (2×2) — Vergleich mit Gauß-Seidel

Schrittweise Durchführung des Jacobi-Iterationsverfahrens am $2\times2$-System $A = \left(\begin{smallmatrix}2 & 2\\ 1 & 2\end{smallmatrix}\right)$, $b = (2,\,4)^\top$, Startwert $x^{(0)} = (0,\,0)$, exakte Lösung $x^* = (-2,\,3)$. Zeigt die Oszillation der Iterierten, die Zerlegung $A = D + (L+U)$, die Iterationsmatrix $B_J$ und den Spektralradius $\rho(B_J) = \tfrac{1}{\sqrt2} \approx 0{,}707$. Direkter Vergleich mit $\rho(B_{GS}) = 0{,}5$ desselben Systems — Gauß-Seidel konvergiert schneller.

- **Open in browser:** [jacobi_2x2_gleich_gauss_seidel.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Gauß-Seidel-Verfahren — komplett

Schrittweise Gauß-Seidel-Iteration am selben $2\times2$-System wie das Jacobi-Beispiel. Zeigt das sofortige Einsetzen neu berechneter Komponenten ($x_1^{(k+1)}$ geht direkt in die Berechnung von $x_2^{(k+1)}$ ein), die Zerlegung $A = D + L + U$, die Iterationsmatrix $B_{GS}$ und $\rho(B_{GS}) = 0{,}5$ — deutlich schneller als Jacobi.

- **Open in browser:** [gauss_seidel_komplett.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/gauss_seidel_komplett.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/gauss_seidel_komplett.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Jacobi-Rotation — Eigenwerte & Eigenvektoren (2×2)

Bestimmung der Eigenwerte und Eigenvektoren der symmetrischen Matrix $A = \left(\begin{smallmatrix}3 & 4\\ 4 & -3\end{smallmatrix}\right)$ mit einem einzigen Jacobi-Rotationsschritt. Zeigt die Berechnung der Hilfsgröße $\alpha = -\tfrac{3}{4}$, der Rotationsparameter $c = \tfrac{2}{\sqrt5}$ und $s = -\tfrac{1}{\sqrt5}$, die Transformation $A^{(1)} = Q^\top A\,Q = \operatorname{diag}(5,\,-5)$ sowie das Ablesen der Eigenwerte $\pm5$ und Eigenvektoren (Spalten von $Q$) mit Verifikation $A\,v = \lambda\,v$. Bei $2\times2$ diagonalisiert eine Rotation exakt.

- **Open in browser:** [jacobi_rotation_eigenwerte_2x2.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_rotation_eigenwerte_2x2.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_rotation_eigenwerte_2x2.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Newton-Verfahren — Kontraktion & Konvergenzordnung

Analyse des Newton-Verfahrens für $f(x) = 1 - \tfrac{3}{x^2}$ (Nullstelle $x^* = \sqrt3$) entlang der vier Aufgabenteile: **(a)** Herleitung und Vereinfachung der Iterationsfunktion $\Phi(x) = \tfrac32 x - \tfrac16 x^3$ samt $\Phi'(x) = \tfrac{3-x^2}{2}$ und $\Phi''(x) = -x$; **(b)** Nachweis, dass $\Phi$ auf $[\tfrac32, 2]$ eine Kontraktion mit Kontraktionszahl $a = \tfrac12 \le \tfrac12 < 1$ ist (Selbstabbildung + Lipschitz-Schranke); **(c)** quadratische Konvergenz ($p = 2$) wegen $\Phi'(\sqrt3) = 0$, $\Phi''(\sqrt3) \neq 0$, mit numerischer Fehlertabelle; **(d)** $g(x) = 1 - \tfrac{6}{x^2} + \tfrac{9}{x^4} = f(x)^2$ hat eine doppelte Nullstelle, daher höchstens lineare Konvergenz ($p = 1$, Faktor $\tfrac12$).

- **Open in browser:** [newton_verfahren_kontraktion_ordnung.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_verfahren_kontraktion_ordnung.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_verfahren_kontraktion_ordnung.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>
