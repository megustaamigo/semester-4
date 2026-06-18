---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - tuples
source: Units/U03/S99.ProposedSolutions
---

# Cennorium — Erweiterter Euklid mit Tupel-Rückgabe

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Den erweiterten Euklidischen Algorithmus implementieren.

- Statische Methode für zwei Integer `left` und `right`
- Rückgabe von `gcd(left, right)` sowie Faktoren `a`, `b` mit `a·left + b·right = gcd` über Tupeltypen
- Test mit 1071 und 462

## Lösung

```csharp
using System;

static (int Gcd, int LeftFactor, int RightFactor) EuclideanExtended(int left, int right)
{
    ArgumentOutOfRangeException.ThrowIfNegative(left);
    ArgumentOutOfRangeException.ThrowIfNegative(right);

    if (right == 0)
        return (left, 1, 0);

    var (gcd, first, second) = EuclideanExtended(right, left % right);
    var q = left / right;

    return (gcd, second, first - (q * second));
}

// --- Verwendung ---

int left = 1071, right = 462;
var t = EuclideanExtended(left, right);
// var (Gcd, LeftFactor, RightFactor) = EuclideanExtended(left, right);   // Variante: Dekonstruktion
Console.WriteLine($"{t.LeftFactor} * {left} + {t.RightFactor} * {right} = gcd({left}, {right}) = {t.Gcd}");
```

## Konzepte

- **Tupeltypen (S08 Tuple Types)** — die Methode gibt ein benanntes Werte-Tupel `(int Gcd, int LeftFactor, int RightFactor)` zurück; der Zugriff erfolgt über die Feldnamen (`t.Gcd`, `t.LeftFactor`, …).
- **Dekonstruktion** — `var (gcd, first, second) = EuclideanExtended(...)` zerlegt das zurückgegebene Tupel direkt in einzelne Variablen.
- **Rekursion** — der erweiterte Euklid wird rekursiv formuliert; der Basisfall `right == 0` liefert `(left, 1, 0)`, im Rückschritt werden die Bézout-Faktoren aktualisiert.
- **Argumentprüfung** — `ArgumentOutOfRangeException.ThrowIfNegative` validiert die Eingaben (Fail-Fast).
