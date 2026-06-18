---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - delegates
source: Units/U03/S99.ProposedSolutions
---

# Cirac — Wertetabelle über Delegate-Array

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Wertetabelle für reelle Funktionen erzeugen.

- Array von Funktionen f: R→R, initialisiert mit sin, cos, sin², cos²
- Formatierte Wertetabelle für [0, 2π] in π/4-Schritten für alle Funktionen ausgeben

## Lösung

```csharp
using System;

delegate double Trigonometry(double x);

// --- Verwendung ---

var functions = new Trigonometry[]
{
    Math.Sin,
    Math.Cos,
    x => Math.Pow(Math.Sin(x), 2),
    x => Math.Pow(Math.Cos(x), 2)
};

var x = 0.0;
var delta = Math.PI / 4;

Console.WriteLine("   x   |sin(x) |cos(x) |sin²(x)|cos²(x)|");
while (x <= 2 * Math.PI)
{
    Console.Write($"{x,6:f3} |");
    foreach (var f in functions)
    {
        Console.Write($"{f(x),6:f3} |");
    }
    Console.WriteLine();
    x += delta;
}
```

## Konzepte

- **Delegate-Array (S09 Delegates)** — `Trigonometry[]` ist ein Array aus Funktionsreferenzen; über `f(x)` wird in der Schleife jede Funktion aufgerufen.
- **Gemischte Initialisierung** — das Array enthält sowohl Methodenreferenzen (`Math.Sin`, `Math.Cos`) als auch Lambda-Ausdrücke (`x => Math.Pow(Math.Sin(x), 2)`), da beide zur Signatur `double(double)` passen.
- **Formatierte Ausgabe** — `$"{x,6:f3}"` richtet rechtsbündig mit Feldbreite 6 und 3 Nachkommastellen aus; die doppelte Schleife erzeugt zeilenweise die Wertetabelle für [0, 2π].
