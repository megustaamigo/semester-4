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

Schrittweise Visualisierung der LU-Zerlegung einer 3×3-Matrix mit Pivotelementen und Multiplikatoren.

- **Open in browser:** [lu_zerlegung_3x3.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/lu_zerlegung_3x3.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/lu_zerlegung_3x3.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Jacobi-Verfahren (2×2) — Vergleich mit Gauß-Seidel

Schrittweise Durchführung des Jacobi-Iterationsverfahrens an einem 2×2-System (A = [[2,2],[1,2]], b = (2,4), Startwert (0,0), Lösung (−2,3)). Zeigt die Oszillation, die Zerlegung A = D + (L+U), die Iterationsmatrix B_J und den Spektralradius ρ(B_J) = 1/√2 ≈ 0.707. Direkter Vergleich mit ρ(B_GS) = 0.5 desselben Systems.

- **Open in browser:** [jacobi_2x2_gleich_gauss_seidel.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>

---

## Gauß-Seidel-Verfahren — komplett

Schrittweise Gauß-Seidel-Iteration am selben 2×2-System wie das Jacobi-Beispiel. Zeigt das sofortige Einsetzen neu berechneter Komponenten (x₁⁽ᵏ⁺¹⁾ geht direkt in die Berechnung von x₂⁽ᵏ⁺¹⁾ ein), die Zerlegung A = D + L + U, die Iterationsmatrix B_GS und ρ(B_GS) = 0.5 — deutlich schneller als Jacobi.

- **Open in browser:** [gauss_seidel_komplett.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/gauss_seidel_komplett.html)

<iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/gauss_seidel_komplett.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>
