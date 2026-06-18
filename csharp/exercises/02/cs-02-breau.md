---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - parameters
source: Units/U02/S99.ProposedSolutions
---

# Breau — Modifizierer ref, out & params

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Die Parameter-Modifizierer `ref`, `out` und `params` einsetzen.

- `Swap` — tauscht zwei initialisierte `int`-Parameter
- `RandomFill` — füllt ein übergebenes Array mit Zufallswerten (direkt im Argument)
- `DynamicMax` — beliebig viele `int`-Parameter, gibt das Maximum zurück

## Lösung

```csharp
static void Swap(ref int n1, ref int n2)
{
    var n = n1;                 // oder: (n1, n2) = (n2, n1);
    n1 = n2;
    n2 = n;
}

static void RandomFill(int[] array)
{
    var rnd = new Random();
    for (var i = 0; i < array.Length; i++)
        array[i] = rnd.Next();
}

static int DynamicMax(params int[] array)
{
    var result = 0;

    for (var i = 0; i < array.Length; i++)
    {
        if (result < array[i])
            result = array[i];
    }

    return result;

    // Built-in LINQ: return array.Max();
}

static void Main()
{
    int n1 = 5, n2 = 7;
    Console.WriteLine($"n1 = {n1}, n2 = {n2}");
    Swap(ref n1, ref n2);
    Console.WriteLine($"n1 = {n1}, n2 = {n2}");

    int[] array = [1, 2, 3];
    Console.WriteLine($"array: [{string.Join(", ", array)}]");
    RandomFill(array);
    Console.WriteLine($"array: [{string.Join(", ", array)}]");

    Console.WriteLine($"max(array) = {DynamicMax(array)}");
}
```

## Konzepte

- **`ref`** — Parameter wird per Verweis übergeben; Änderungen wirken auf die Variable des Aufrufers (S03 Methods). Variable muss vorher initialisiert sein.
- **Referenztypen (Arrays)** — `RandomFill` braucht kein `ref`: das Array selbst ist ein Referenztyp, Elementänderungen sind außen sichtbar.
- **`params`** — variable Argumentanzahl; `DynamicMax(1, 2, 3)` oder ein Array sind beide möglich.
- **`out`** — würde einen nur-Ausgabe-Parameter erlauben (hier nicht genutzt, aber Teil derselben Modifizierer-Familie).
