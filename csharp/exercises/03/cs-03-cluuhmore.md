---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - structs
source: Units/U03/S99.ProposedSolutions
---

# Cluuhmore — readonly struct ComplexNumber

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Einen schreibgeschützten Werttyp für komplexe Zahlen entwerfen.

- `ComplexNumber` als `readonly struct`
- Eigenschaften/Konstruktor für Real- und Imaginärteil (`double`)
- Statische Methode `Conjugate` für die konjugiert komplexe Zahl
- Implizite Konvertierung `double` → `ComplexNumber` (als Realteil)
- Operatoren `+`, `-`, `*`, `/` sowie `==`, `!=` überladen
- Indexer über `int`: 0 → Realteil, sonst Imaginärteil
- `ToString` überschreiben
- Test mit zwei Beispielzahlen

## Lösung

```csharp
using System;
using System.Numerics;

readonly struct ComplexNumber(double real, double imaginary) :
    IEquatable<ComplexNumber>,
    IAdditionOperators<ComplexNumber, ComplexNumber, ComplexNumber>,
    IDivisionOperators<ComplexNumber, ComplexNumber, ComplexNumber>,
    IEqualityOperators<ComplexNumber, ComplexNumber, bool>,
    IMultiplyOperators<ComplexNumber, ComplexNumber, ComplexNumber>,
    ISubtractionOperators<ComplexNumber, ComplexNumber, ComplexNumber>,
    IUnaryNegationOperators<ComplexNumber, ComplexNumber>,
    IUnaryPlusOperators<ComplexNumber, ComplexNumber>
{
    public double this[int index] => (index == 0) ? this.Real : this.Imaginary;

    public double Imaginary { get; } = imaginary;

    public double Real { get; } = real;

    public static ComplexNumber Conjugate(ComplexNumber value) => new(value.Real, -value.Imaginary);

    public static implicit operator ComplexNumber(double value) => new(value, 0);

    public static ComplexNumber operator -(ComplexNumber value) => new(-value.Real, -value.Imaginary);

    public static ComplexNumber operator -(ComplexNumber left, ComplexNumber right) => left + -right;

    public static bool operator !=(ComplexNumber left, ComplexNumber right) => !(left == right);

    public static ComplexNumber operator *(ComplexNumber left, ComplexNumber right) => new((left.Real * +right.Real) - (left.Imaginary * right.Imaginary), (left.Real * right.Imaginary) + (left.Imaginary * right.Real));

    public static ComplexNumber operator /(ComplexNumber left, ComplexNumber right)
    {
        var magnitude = (right.Real * right.Real) + (right.Imaginary * right.Imaginary);
        return magnitude == 0
            ? throw new DivideByZeroException()
            : new(((left.Real * +right.Real) + (left.Imaginary * right.Imaginary)) / magnitude, ((left.Imaginary * right.Real) - (left.Real * right.Imaginary)) / magnitude);
    }

    public static ComplexNumber operator +(ComplexNumber value) => new(+value.Real, +value.Imaginary);

    public static ComplexNumber operator +(ComplexNumber left, ComplexNumber right) => new(left.Real + right.Real, left.Imaginary + right.Imaginary);

    public static bool operator ==(ComplexNumber left, ComplexNumber right) => left.Equals(right);

    public bool Equals(ComplexNumber other) => (this.Real == other.Real) && (this.Imaginary == other.Imaginary);

    public override bool Equals(object obj) => obj is ComplexNumber other && this.Equals(other);

    public override int GetHashCode() => HashCode.Combine(this.Real, this.Imaginary);

    public override string ToString()
    {
        var sign = double.IsNegative(this.Imaginary) ? "-" : "+";
        var absImaginary = double.Abs(this.Imaginary);
        var textImaginary = absImaginary == 1 ? string.Empty : absImaginary.ToString();
        return $"({this.Real} {sign} {textImaginary}i)";
    }
}

// --- Verwendung ---

ComplexNumber x = new(5.0, 3.0);
ComplexNumber y = new(4.0, 2.0);
var d = -1.5;

Console.WriteLine(x);
Console.WriteLine($"conj(x) = {ComplexNumber.Conjugate(x)}");
Console.WriteLine($"-{x} = {-x}");
Console.WriteLine($"{x} + {y} = {x + y}");
Console.WriteLine($"{x} - {y} = {x - y}");
Console.WriteLine($"{x} * {y} = {x * y}");
Console.WriteLine($"{x} / {y} = {x / y}");
Console.WriteLine($"{x} == {y}? {x == y}");
Console.WriteLine($"x[0] = {x[0]}, x[1] = {x[1]}");
Console.WriteLine($"{d} * {x} = {d * x}");   // implizite Konvertierung double -> ComplexNumber
```

## Konzepte

- **`readonly struct` (S01 Value Types)** — alle Felder sind unveränderlich; der Werttyp wird per Wert kopiert. Über den **primären Konstruktor** `(double real, double imaginary)` werden die Get-only-Properties initialisiert.
- **Operatorüberladung (S02 Operators)** — komplexe Arithmetik nach den üblichen Rechenregeln; Subtraktion über `left + -right`, Division über die Multiplikation mit dem Konjugierten geteilt durch das Betragsquadrat.
- **Indexer (S03 Indexers)** — `this[int]` liefert 0 → Realteil, sonst Imaginärteil.
- **Implizite Konvertierung** — `double` → `ComplexNumber` mit Imaginärteil 0, sodass `d * x` funktioniert.
- **Numerische Interfaces (System.Numerics)** — `IAdditionOperators`, `IMultiplyOperators`, `IEqualityOperators` usw. machen den Typ generisch verwendbar; `IEquatable<ComplexNumber>` für wertbasierte Gleichheit.
