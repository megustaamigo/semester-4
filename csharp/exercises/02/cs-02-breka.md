---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - classes
source: Units/U02/S99.ProposedSolutions
---

# Breka — Klasse Polynomial

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine Klasse für Polynome mit reellen Koeffizienten entwerfen.

- Konstruktor, der eine Koeffizientenmenge entgegennimmt
- Property `Degree` (Grad des Polynoms)
- `Eval` — Auswertung an einer Stelle $x$
- `Multiply` — Multiplikation mit einem Skalar
- `Add` — Addition zweier Polynome
- `IsZero` — Test auf Nullpolynom

## Lösung

```csharp
class Polynomial
{
    readonly List<double> coefficients = [];

    public Polynomial()
    {
        this.coefficients.Add(0);
        this.Degree = -1;
    }

    public Polynomial(params double[] coefficients)
    {
        foreach (var d in coefficients)
        {
            this.coefficients.Add(d);
        }
        this.Degree = coefficients.Length - 1;
    }

    public int Degree { get; init; }

    public Polynomial Add(Polynomial other)
    {
        var max = int.Max(this.coefficients.Count, other.coefficients.Count);
        var result = new double[max];

        for (var i = 0; i < max; i++)
        {
            result[i] = (i < this.coefficients.Count ? this.coefficients[i] : 0.0) + (i < other.coefficients.Count ? other.coefficients[i] : 0.0);
        }

        return new(result);
    }

    public double Eval(double x)
    {
        var d = this.coefficients[this.Degree];      // Horner-Schema

        for (var i = this.Degree - 1; i >= 0; --i)
        {
            d = this.coefficients[i] + (x * d);
        }

        return d;
    }

    public bool IsZero() => this.coefficients.TrueForAll(c => Math.Abs(c) < 1e-15);

    public Polynomial Multiply(double scalar) => new(this.coefficients.ConvertAll(x => scalar * x).ToArray());

    public override string ToString() => $"pol degree: {this.Degree}, [{string.Join("; ", this.coefficients)}]";
}

// --- Verwendung ---
static void Main()
{
    var pol1 = new Polynomial();
    Console.WriteLine(pol1);
    Console.WriteLine($"IsZero? {pol1.IsZero()}");

    var pol2 = new Polynomial(1.2, 3.4);
    Console.WriteLine(pol2);

    // p(x) = 3x^2 + 2x + 1; p(2) = 12 + 4 + 1
    var pol3 = new Polynomial(1.0, 2.0, 3.0);
    Console.WriteLine($"{pol3}, p(2.0) = {pol3.Eval(2.0)}");

    var pol4 = pol3.Multiply(-1);
    Console.WriteLine($"{pol4}, p(2.0) = {pol4.Eval(2.0)}");

    var pol5 = pol2.Add(pol3);
    Console.WriteLine($"{pol5}, p(2.0) = {pol5.Eval(2.0)}");
}
```

## Konzepte

- **`params double[]`-Konstruktor** — beliebig viele Koeffizienten; `coefficients[i]` ist der Koeffizient zu $x^i$ (S04 Constructors).
- **`init`-Property** — `Degree { get; init; }` nur im Konstruktor/Initializer setzbar, danach unveränderlich.
- **Horner-Schema in `Eval`** — wertet $p(x)$ effizient von höchstem zu niedrigstem Grad aus (nur $n$ Multiplikationen).
- **Unveränderliche Operationen** — `Add` und `Multiply` liefern ein neues `Polynomial` statt das vorhandene zu ändern (funktionaler Stil).
- **`List<T>.ConvertAll` / `TrueForAll`** — funktionale Listenoperationen für Skalarmultiplikation bzw. Nullpolynom-Test mit Toleranz `1e-15`.
