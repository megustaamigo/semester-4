---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - methods
source: Units/U02/S99.ProposedSolutions
---

# Beroson — Durchschnitt von Ganzzahlen

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Den Durchschnittswert aller Zahlen eines Arrays als `double` berechnen.

- Methode `AverageValue(int[] numbers)` implementieren
- Mit den ersten sechs Quadratzahlen testen

## Lösung

```csharp
static double AverageValue(int[] numbers)
{
    if (numbers.Length == 0)
        return 0;

    var sum = 0;

    for (var i = 0; i < numbers.Length; i++)
    {
        sum += numbers[i];
    }

    return (double)sum / numbers.Length;

    // Built-in LINQ: return numbers.Average();
}

static void Main()
{
    var square = new int[] { 1, 4, 9, 16, 25, 36 };
    var average = AverageValue(square);
    Console.WriteLine($"Der Durchschnittswert der ersten {square.Length} Quadrat-Zahlen ist {average:f3}.");
}
```

## Konzepte

- **Arrays als Parameter** — `int[]` wird per Referenz übergeben, hier aber nur gelesen (S03 Methods).
- **Explizite Typumwandlung `(double)`** — verhindert Ganzzahldivision; ohne Cast würde abgerundet.
- **Edge Case** — leeres Array liefert `0` statt Division durch null.
- **Format-Specifier `:f3`** — drei Nachkommastellen in der Ausgabe.
