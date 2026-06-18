---
course: csharp
type: solution
lecture: 1
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-01
  - methods
source: Units/U01/S99.ProposedSolutions
---

# Astren — Kontrollstrukturen & Methode Power

**Unit:** [[cs-01-strukturierte-programmierung|U01: Basic Elements of Structured Programming]]
**Tasks:** [[Units/U01/Tasks.md|Tasks.md]]

---

## Ziel

Kontrollstrukturen anwenden und eine erste eigene Methode implementieren, um $b^n$ für feste natürliche Zahlen $b$ und $n$ zu berechnen.

- $b^n$ inline mit einer `for`-Schleife berechnen
- Methode `Power(b, n)` implementieren, die das Ergebnis zurückgibt
- Ergebnis formatiert ausgeben

## Lösung

```csharp
using System;

static void Main()
{
    const int b = 2;
    const int n = 8;
    var p = 1;

    // Inline-Berechnung b^n
    for (var i = 0; i < n; i++)
        p *= b;

    Console.WriteLine($"01| {b}^{n}={p}");
    Console.WriteLine($"02| {b}^{n}={Power(b, n)}");
}

static int Power(int b, int n)
{
    var p = 1;
    for (var i = 0; i < n; i++)
        p *= b;
    return p;
}
```

## Konzepte

- **`for`-Schleife** — wiederholtes Multiplizieren von `p` mit `b` realisiert die Potenz `b^n` (S13 Iteration Statements).
- **Methode mit Rückgabewert** — `Power(int b, int n)` kapselt die Berechnung, nimmt Parameter entgegen und liefert das Ergebnis über `return` zurück (S08 Methods).
- **`const`** — `b` und `n` sind Compile-Zeit-Konstanten; ihr Wert steht fest und kann nicht verändert werden.
- **String-Interpolation** — formatierte Ausgabe `$"{b}^{n}={p}"`.
