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

Schrittweise Visualisierung der LU-Zerlegung einer $3\times3$-Matrix per Gauß-Elimination: $A = L \cdot U$ (untere mal obere Dreiecksmatrix). Zeigt die Wahl der Pivotelemente $a_{kk}^{(k)}$ und die Berechnung der Multiplikatoren $m_{ik} = a_{ik}^{(k)} / a_{kk}^{(k)}$ — und als Abschluss die Anwendung: Lösen von $A\,x = b$ per Vorwärts-/Rückwärtssubstitution ($L\,y=b$, dann $U\,x=y$) am Beispiel $b = (4,\,10,\,24)^\top$.

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

Analyse des Newton-Verfahrens für $f(x) = 1 - \tfrac{3}{x^2}$ (Nullstelle $x^* = \sqrt3$) entlang der vier Aufgabenteile: **(a)** Herleitung und Vereinfachung der Iterationsfunktion $\Phi(x) = \tfrac32 x - \tfrac16 x^3$ samt $\Phi'(x) = \tfrac{3-x^2}{2}$ und $\Phi''(x) = -x$; **(b)** Nachweis, dass $\Phi$ auf $[\tfrac32, 2]$ eine Kontraktion mit Kontraktionszahl $a = \tfrac12 < 1$ ist (Selbstabbildung + Lipschitz-Schranke); **(c)** quadratische Konvergenz ($p = 2$) wegen $\Phi'(\sqrt3) = 0$, $\Phi''(\sqrt3) \neq 0$, mit numerischer Fehlertabelle; **(d)** $g(x) = 1 - \tfrac{6}{x^2} + \tfrac{9}{x^4} = f(x)^2$ hat eine doppelte Nullstelle, daher höchstens lineare Konvergenz ($p = 1$, Faktor $\tfrac12$).

- **Open in browser:** [newton_verfahren_kontraktion_ordnung.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_verfahren_kontraktion_ordnung.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_verfahren_kontraktion_ordnung.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Newton-Interpolation — parametrische Kurve (dividierte Differenzen)

Parametrische Interpolation der vier Ebenen-Punkte $z_0=(0,0)$, $z_1=(1,1)$, $z_2=(1,0)$, $z_3=(0,1)$ über $t_i = 0,1,2,3$ mit Newtonschen dividierten Differenzen. Zeigt beide Differenzenschemata (für $x$- und $y$-Koordinaten) als farbcodierte Dreiecke, das Aufstellen und Vereinfachen der Newton-Polynome $p_x(t) = \tfrac32 t - \tfrac12 t^2$ (Grad fällt auf 2, da 3. Differenz $=0$) und $p_y(t) = \tfrac23 t^3 - 3t^2 + \tfrac{10}{3}t$, sowie die resultierende Kurve als SVG-Plot mit Stützpunkten. Inklusive Begründung, warum parametrisch interpoliert werden muss (Punkte bilden keine Funktion $y(x)$).

- **Open in browser:** [newton_interpolation_parametrische_kurve.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_interpolation_parametrische_kurve.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/newton_interpolation_parametrische_kurve.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Romberg-Verfahren — Trapezregel + Richardson-Extrapolation

Numerische Berechnung von $\int_{-2}^{2} \frac{135x^2}{2+|x|}\,dx$ mit dem Romberg-Verfahren. Zeigt die drei Trapezsummen der Halbierungsfolge $T_1=540$ ($h=4$), $T_2=270$ ($h=2$), $T_4=225$ ($h=1$) — jeweils als SVG-Trapezflächen über dem Funktionsgraphen — und die Richardson-Extrapolation $T_{i,k}=\frac{4^k T_{i,k-1}-T_{i-1,k-1}}{4^k-1}$ mit der ersten Spalte als Simpson-Regel ($180, 210$) und dem Endwert $T_{2,2}=212$. Inklusive vollständigem Tableau und Vergleich mit dem exakten Wert $208{,}60$: weil $|x|$ bei $x=0$ einen Knick erzeugt, gewinnt die höchste Extrapolationsspalte nicht die volle Ordnung.

- **Open in browser:** [romberg_trapez_extrapolation.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/romberg_trapez_extrapolation.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/romberg_trapez_extrapolation.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Bulirsch-Verfahren — Trapezregel + Neville-Extrapolation

Dasselbe Integral $\int_{-2}^{2} \frac{135x^2}{2+|x|}\,dx$, aber mit der Bulirsch-Schrittweitenfolge $n=1,2,3$ ($h_1=4$, $h_2=2$, $h_3=\frac43$) statt der Halbierungsfolge. Zeigt die drei Trapezsummen $T_1=540$, $T_2=270$, $T_3=240$ (mit den inneren Knoten $\pm\frac23$ bei $h_3$) und die allgemeine Neville-Extrapolation $T_{i,k}=T_{i,k-1}+\frac{T_{i,k-1}-T_{i-1,k-1}}{(h_{i-k}/h_i)^2-1}$, deren Nenner — anders als bei Romberg — nicht konstant $4^k-1$ ist, sondern aus den tatsächlichen Schrittweitenverhältnissen ($3$, $\frac54$, $8$) berechnet wird. Endwert $T_{3,2}=220{,}5$; mit direktem Vergleich exakt $208{,}60$ / Romberg $212$ / Bulirsch $220{,}5$ und derselben Knick-Diskussion.

- **Open in browser:** [bulirsch_trapez_extrapolation.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/bulirsch_trapez_extrapolation.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/bulirsch_trapez_extrapolation.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>
