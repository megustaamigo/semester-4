---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - events
source: Units/U03/S99.ProposedSolutions
---

# Chernuluhm — Newton-Verfahren mit Event (√2)

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Newtonverfahren zur Berechnung der Wurzel aus 2 anwenden.

- Klasse zur Durchführung des Newtonverfahrens für f: R→R
- Schreibgeschützte Eigenschaften für f und f' (über primären Konstruktor)
- Eigenschaften für max. Iterationen (Standard 25) und ε (Standard 1e-12)
- Methode `Execute(x0)`: führt das Verfahren aus, Abbruch bei MaxIterations oder |f(xₙ)| < ε, gibt xₙ zurück
- Ereignis `IterationStarted` mit Argumenten für n, xₙ und f(xₙ), zu Beginn jeder Iteration ausgelöst
- Test mit f(x) = 1 − a/x², a = 2, x₀ = 2 → Näherung für √2

## Lösung

```csharp
using System;

class Newton(Func<double, double> function, Func<double, double> derivativeFunction)
{
    public event EventHandler<NewtonEventArgs> IterationStarted;

    public Func<double, double> Function { get; } = function;

    public Func<double, double> DerivativeFunction { get; } = derivativeFunction;

    public int MaxIterations { get; set; } = 25;

    public double Epsilon { get; set; } = 1e-12;

    public double Execute(double startValue)
    {
        var n = 0;
        var value = startValue;
        var functionValue = this.Function(value);

        while (n < this.MaxIterations && Math.Abs(functionValue) >= this.Epsilon)
        {
            this.IterationStarted?.Invoke(this, new NewtonEventArgs { Iteration = n, Value = value, FunctionValue = functionValue });
            n++;
            value -= functionValue / this.DerivativeFunction(value);
            functionValue = this.Function(value);
        }

        return value;
    }
}

class NewtonEventArgs : EventArgs
{
    public double FunctionValue { get; init; }

    public int Iteration { get; init; }

    public double Value { get; init; }
}

// --- Verwendung ---

var a = 2.0;
// f(x) = 1 - a/x², f'(x) = 2a/x³ ; Nullstelle bei x² = a, also x = √2
var newton = new Newton(x => 1.0 - (a / (x * x)), x => 2 * a / (x * x * x));
newton.IterationStarted += (_, e) =>
    Console.WriteLine($"x_{e.Iteration} = {e.Value}, f(x_{e.Iteration}) = {e.FunctionValue}");

var result = newton.Execute(2);
Console.WriteLine($"Result: sqrt(2) ≅ {result}");
```

## Konzepte

- **Primärer Konstruktor (S04 / Primary Constructors)** — `Newton(Func<double,double> function, Func<double,double> derivativeFunction)` übergibt f und f' direkt; sie werden als get-only Properties gespeichert.
- **Funktionen als Werte (S09 Delegates)** — `Func<double, double>` für die Funktion und ihre Ableitung; die Newton-Iteration lautet `xₙ₊₁ = xₙ − f(xₙ)/f'(xₙ)`.
- **Events (S10 Events)** — `IterationStarted` vom Typ `EventHandler<NewtonEventArgs>` wird zu Beginn jeder Iteration null-sicher mit `?.Invoke(...)` ausgelöst; `NewtonEventArgs` transportiert n, xₙ und f(xₙ). Der Test abonniert es per Lambda und gibt jede Iteration aus.
- **Abbruchkriterien** — Schleife endet bei Erreichen von `MaxIterations` oder sobald `|f(xₙ)| < ε`; Rückgabe ist das letzte Folgenglied (≈ √2).
