---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - operators
source: Units/U03/S99.ProposedSolutions
---

# Chillmond — Klasse Fraction (Operatorüberladung, Indexer, IComparable)

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Eine Klasse für Brüche entwerfen, die wie ein eingebauter Zahlentyp benutzbar ist.

- Klasse `Fraction` mit schreibgeschütztem `Numerator` und `Denominator` (`int`)
- Konstruktor kürzt den Bruch und macht den Nenner stets positiv
- Implizite Konvertierung `int` → `Fraction`
- Operatoren `+`, `-`, `*`, `/` überladen
- Indexer über `int`: 0 → Zähler, sonst Nenner
- `ToString` überschreiben
- `IComparable<Fraction>` implementieren (`CompareTo` → -1 / 0 / 1)
- Vergleichsoperatoren `==`, `!=`, `<`, `<=`, `>`, `>=` überladen
- Test mit zwei Beispielzahlen

## Lösung

```csharp
using System;
using System.Globalization;
using System.Numerics;

class Fraction :
    IEquatable<Fraction>,
    IComparable<Fraction>,
    IAdditiveIdentity<Fraction, Fraction>,
    IAdditionOperators<Fraction, Fraction, Fraction>,
    IDivisionOperators<Fraction, Fraction, Fraction>,
    IMultiplicativeIdentity<Fraction, Fraction>,
    IMultiplyOperators<Fraction, Fraction, Fraction>,
    ISubtractionOperators<Fraction, Fraction, Fraction>,
    IUnaryNegationOperators<Fraction, Fraction>,
    IUnaryPlusOperators<Fraction, Fraction>,
    IComparisonOperators<Fraction, Fraction, bool>
{
    public Fraction(int numerator, int denominator)
    {
        if (denominator == 0)
            throw new DivideByZeroException("Der Nenner darf nicht 0 sein.");

        var gcd = GreatestCommonDivisor(Math.Abs(numerator), Math.Abs(denominator));
        this.Numerator = numerator / gcd;
        this.Denominator = denominator / gcd;

        if (denominator < 0)
        {
            this.Numerator = -this.Numerator;
            this.Denominator = -this.Denominator;
        }
    }

    public Fraction(int numerator) : this(numerator, 1) { }

    public static Fraction AdditiveIdentity => new(0);

    public static Fraction MultiplicativeIdentity => new(1);

    public int Denominator { get; }

    public int Numerator { get; }

    // Indexer laut Aufgabenstellung: 0 -> Zähler, sonst Nenner
    public int this[int index] => index == 0 ? this.Numerator : this.Denominator;

    public static Fraction Abs(Fraction f) => new(Math.Abs(f.Numerator), f.Denominator);

    public static implicit operator Fraction(int n) => new(n);

    public static Fraction operator -(Fraction f) => new(-f.Numerator, f.Denominator);

    public static Fraction operator -(Fraction f, Fraction g) => f + -g;

    public static bool operator !=(Fraction left, Fraction right) => !(left == right);

    public static Fraction operator *(Fraction f, Fraction g) => new(f.Numerator * g.Numerator, f.Denominator * g.Denominator);

    public static Fraction operator /(Fraction f, Fraction g) => f * ~g;

    public static Fraction operator ~(Fraction f) => new(f.Denominator, f.Numerator);

    public static Fraction operator +(Fraction f, Fraction g) => new((f.Numerator * g.Denominator) + (f.Denominator * g.Numerator), f.Denominator * g.Denominator);

    public static Fraction operator +(Fraction value) => new(+value.Numerator, value.Denominator);

    public static bool operator <(Fraction left, Fraction right) => left.CompareTo(right) < 0;

    public static bool operator <=(Fraction left, Fraction right) => left.CompareTo(right) <= 0;

    public static bool operator ==(Fraction left, Fraction right) => left is null || right is null ? Equals(left, right) : left.Equals(right);

    public static bool operator >(Fraction left, Fraction right) => left.CompareTo(right) > 0;

    public static bool operator >=(Fraction left, Fraction right) => left.CompareTo(right) >= 0;

    public int CompareTo(Fraction other) => other is null ? 1 : (this.Numerator * other.Denominator).CompareTo(this.Denominator * other.Numerator);

    public bool Equals(Fraction other)
    {
        if (other is null)
            return false;
        if (ReferenceEquals(this, other))
            return true;
        if (this.GetType() != other.GetType())
            return false;
        return (this.Numerator == other.Numerator) && (this.Denominator == other.Denominator);
    }

    public override bool Equals(object obj) => this.Equals(obj as Fraction);

    public override int GetHashCode() => HashCode.Combine(this.Numerator, this.Denominator);

    public override string ToString() => this.Denominator == 1 ? this.Numerator.ToString(CultureInfo.CurrentCulture) : $"{this.Numerator}/{this.Denominator}";

    static int GreatestCommonDivisor(int n, int m)
    {
        while (m != 0)
            (n, m) = (m, n % m);
        return n;
    }
}

// --- Verwendung ---

var f1 = new Fraction(1, 2);
var f2 = new Fraction(2, 3);

Console.WriteLine($"{f1} + {f2} = {f1 + f2}");
Console.WriteLine($"{f1} - {f2} = {f1 - f2}");
Console.WriteLine($"{f1} * {f2} = {f1 * f2}");
Console.WriteLine($"{f1} / {f2} = {f1 / f2}");
Console.WriteLine($"-({f2}) = {-f2}");
Console.WriteLine($"Abs({f1}) = {Fraction.Abs(f1)}");
Console.WriteLine($"{f1} < {f2}? {f1 < f2}");
Console.WriteLine($"{f1} == {f2}? {f1 == f2}");

var f3 = new Fraction(1, 3);
Console.WriteLine($"6 * {f3} = {6 * f3}");   // implizite Konvertierung int -> Fraction

// Negativer Nenner wird normalisiert
var f4 = new Fraction(7, -21);               // -> -1/3
Console.WriteLine($"f4 = {f4}");

var list = new List<Fraction> { new(1, 2), new(1), new(1, 3), new(2, 3) };
list.Sort();                                 // nutzt IComparable<Fraction>
foreach (var item in list)
    Console.WriteLine(item);
```

## Konzepte

- **Operatorüberladung (S02 Operators)** — `+ - * /` sowie unär `-`, `+` und `~` (Kehrwert) und die Vergleichsoperatoren werden als `static` Methoden definiert; Division ist als `f * ~g` (Multiplikation mit dem Kehrwert) ausgedrückt.
- **Indexer (S03 Indexers)** — `this[int index]` liefert für 0 den Zähler, sonst den Nenner.
- **Implizite Konvertierung** — `implicit operator Fraction(int n)` erlaubt Ausdrücke wie `6 * f3`.
- **`IComparable<Fraction>`** — `CompareTo` vergleicht über Kreuzmultiplikation; ermöglicht `List.Sort()`. `==`/`!=` stützen sich auf `IEquatable<Fraction>.Equals`.
- **Numerische Interfaces (System.Numerics)** — `IAdditionOperators`, `IMultiplyOperators`, `IComparisonOperators`, `IAdditiveIdentity`, `IMultiplicativeIdentity` usw. machen `Fraction` zu einem generisch nutzbaren Zahlentyp.
- **Kürzen / Normalisierung** — der Konstruktor teilt durch den `ggT` und erzwingt einen positiven Nenner.
