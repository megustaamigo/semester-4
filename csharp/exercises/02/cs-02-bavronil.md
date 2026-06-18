---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - conditional
source: Units/U02/S99.ProposedSolutions
---

# Bavronil — Schaltjahre (IsLeapYear)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Prüfen, ob ein Jahr ein Schaltjahr ist (gregorianische Regel: teilbar durch 4, aber nicht durch 100, es sei denn durch 400).

- Methode `IsLeapYear(int year)` implementieren, die einen `bool` zurückliefert
- Mit geeigneten Jahreszahlen und formatierter Ausgabe testen

## Lösung

```csharp
static bool IsLeapYear(int year)
{
    if (year % 400 == 0)
        return true;
    else if (year % 100 == 0)
        return false;
    else if (year % 4 == 0)
        return true;
    else
        return false;

    // Kurzform:
    // => (year % 400 == 0) || (year % 4 == 0 && year % 100 != 0);
}

static void LeapYearTest(int year)
{
    if (IsLeapYear(year))
        Console.WriteLine($"Das Jahr {year} ist ein Schaltjahr.");
    else
        Console.WriteLine($"Das Jahr {year} ist KEIN Schaltjahr.");
}

static void Main()
{
    LeapYearTest(1600);
    LeapYearTest(1700);
    LeapYearTest(1900);
    LeapYearTest(2000);
    LeapYearTest(2024);
}
```

## Konzepte

- **Verschachtelte Fallunterscheidung** — die Reihenfolge der `if/else if`-Prüfungen ist entscheidend: `% 400` muss vor `% 100` stehen (S03 Methods / Kontrollfluss).
- **Modulo `%`** — Teilbarkeitsprüfung über den Rest.
- **Boolescher Rückgabewert** — Methode kapselt die Logik wiederverwendbar.
