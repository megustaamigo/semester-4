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

# Caugne — Funktion als Argument (Func<double,double>)

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Funktionen als Argument einer anderen Funktion einsetzen.

- Statische Methode `Eval` implementieren, die mit einer beliebigen Funktion f: R→R und einem Argument x aufgerufen werden kann und f(x) zurückliefert

## Lösung

```csharp
using System;

static double Eval(Func<double, double> f, double x) => f(x);

// --- Verwendung ---

static double f(double x) => x * x;
var x = 3.0;
Console.WriteLine($"f({x}) = {Eval(f, x)}");   // f(3) = 9
```

## Konzepte

- **`Func<double, double>` (S09 Delegates)** — der eingebaute generische Delegate-Typ `Func<TArg, TResult>` repräsentiert eine Funktion f: R→R; `Eval` nimmt sie als Parameter (Funktion höherer Ordnung) und ruft sie mit `f(x)` auf.
- **Funktion als Argument** — die zu evaluierende Funktion wird als lokale Funktion `f` übergeben; ebenso möglich wären eine Methodenreferenz oder ein Lambda-Ausdruck.
