---
course: csharp
type: solution
lecture: 1
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-01
  - iteration
source: Units/U01/S99.ProposedSolutions
---

# Athua — Fibonacci (for / do-while / while)

**Unit:** [[cs-01-strukturierte-programmierung|U01: Basic Elements of Structured Programming]]
**Tasks:** [[Units/U01/Tasks.md|Tasks.md]]

---

## Ziel

Die ersten Zahlen der Fibonacci-Folge $f_n$ bis zu einem vorgegebenen Index $n$ **nicht-rekursiv** mit einer Schleife berechnen, wobei $f_0 = 1$, $f_1 = 1$ und $f_n = f_{n-2} + f_{n-1}$ für $n > 1$.

- Schleife als `for` implementieren
- Schleife als `do-while` implementieren
- Schleife als `while` implementieren

## Lösung

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

static void Main()
{
    const int n = 10;
    int i, fn;

    // Variante: for
    var f0 = 1;
    var f1 = 1;
    List<int> fibonacci = [f0, f1];
    for (i = 2; i < n; i++)
    {
        fn = f0 + f1;
        fibonacci.Add(fn);
        f0 = f1;
        f1 = fn;
    }
    Console.WriteLine($"01| fibonacci: [ {string.Join(' ', fibonacci)} ]");

    // Variante: do-while (Rumpf wird mind. einmal ausgeführt)
    f0 = 1; f1 = 1;
    fibonacci = [f0, f1];
    i = 2;
    do
    {
        fn = f0 + f1;
        fibonacci.Add(fn);
        f0 = f1;
        f1 = fn;
        i++;
    } while (i < n);
    Console.WriteLine($"02| fibonacci: [ {string.Join(' ', fibonacci)} ]");

    // Variante: while (Bedingung wird vor dem Rumpf geprüft)
    f0 = 1; f1 = 1;
    fibonacci = [f0, f1];
    i = 2;
    while (i < n)
    {
        fn = f0 + f1;
        fibonacci.Add(fn);
        f0 = f1;
        f1 = fn;
        i++;
    }
    Console.WriteLine($"03| fibonacci: [ {string.Join(' ', fibonacci)} ]");
}
```

### Bonus: LINQ + Lambda

```csharp
// Nutzt Konzepte späterer Units (LINQ, Lambda, Closure über f0/f1)
int f0 = 1, f1 = 1, fn;
var fibonacci2 = Enumerable.Range(0, 10).Select(k =>
{
    if (k <= 1) return 1;
    fn = f0 + f1;
    f0 = f1;
    f1 = fn;
    return fn;
});
Console.WriteLine($"04| fibonacci: [ {string.Join(' ', fibonacci2)} ]");
```

## Konzepte

- **`for`-Schleife** — kompakte Form mit Initialisierung, Bedingung und Inkrement in einer Zeile; ideal bei bekannter Iterationszahl (S13 Iteration Statements).
- **`while`-Schleife** — prüft die Bedingung **vor** dem Rumpf (kopfgesteuert) — der Rumpf läuft ggf. null Mal.
- **`do-while`-Schleife** — prüft die Bedingung **nach** dem Rumpf (fußgesteuert) — der Rumpf läuft mindestens einmal.
- **Iterative Berechnung** — zwei Vorgänger `f0`, `f1` werden bei jedem Schritt weitergeschoben; kein Stack-Overhead wie bei Rekursion.
- **Collection-Expression `[f0, f1]`** — moderne C#-Syntax zum Initialisieren einer `List<int>`.
- **LINQ/Lambda (Bonus)** — `Enumerable.Range(...).Select(...)` mit einer Closure über `f0`/`f1`; Konzept aus späteren Units, hier nur zur Demonstration.
