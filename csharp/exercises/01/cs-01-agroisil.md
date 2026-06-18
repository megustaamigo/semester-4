---
course: csharp
type: solution
lecture: 1
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-01
  - arrays
source: Units/U01/S99.ProposedSolutions
---

# Agroisil — Arrays & dynamische Listen

**Unit:** [[cs-01-strukturierte-programmierung|U01: Basic Elements of Structured Programming]]
**Tasks:** [[Units/U01/Tasks.md|Tasks.md]]

---

## Ziel

Mit Arrays und dynamischen Listen arbeiten und verschiedene Ausgabe- und Filtertechniken üben.

- Array mit 10 Zufallszahlen (1–100) erzeugen und mit `for` ausgeben
- Dynamische `List<int>` mit 10 Zufallszahlen erzeugen und mit `foreach` ausgeben
- Liste erneut via `string.Join()` und String-Interpolation ausgeben
- Zweites Array erzeugen, das nur die geraden Zahlen des ersten enthält

## Lösung

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

static void Main()
{
    const int n = 10;
    var rnd = new Random();

    // Array mit 10 Zufallszahlen 1..100 (obere Grenze exklusiv -> 101)
    var array = new int[n];
    for (var i = 0; i < n; i++)
        array[i] = rnd.Next(1, 101);

    Console.Write("01| array: [");
    for (var i = 0; i < n; i++)
        Console.Write($" {array[i]}");
    Console.WriteLine(" ]");

    // Dynamische Liste
    var list = new List<int>();
    for (var i = 0; i < n; i++)
        list.Add(rnd.Next(1, 101));

    Console.Write("02| list : [");
    foreach (var x in list)
        Console.Write($" {x}");
    Console.WriteLine(" ]");

    // Ausgabe via string.Join() + String-Interpolation
    Console.WriteLine($"03| list : [ {string.Join(' ', list)} ]");

    // Anzahl gerader Zahlen zählen
    var count = 0;
    foreach (var x in array)
        if (x % 2 == 0)
            ++count;
    Console.WriteLine($"04| Anzahl: {count}");

    // Variante 1: manuell in passend dimensioniertes Array
    var even1 = new int[count];
    var j = 0;
    foreach (var x in array)
        if (x % 2 == 0)
            even1[j++] = x;
    Console.WriteLine($"05| even1: [ {string.Join(' ', even1)} ]");

    // Variante 2: LINQ Query-Syntax (später eingeführt)
    var even2 = from x in array where x % 2 == 0 select x;
    Console.WriteLine($"06| even2: [ {string.Join(' ', even2)} ]");

    // Variante 3: LINQ Method-Syntax
    var even3 = array.Where(x => x % 2 == 0);
    Console.WriteLine($"07| even3: [ {string.Join(' ', even3)} ]");
}
```

## Konzepte

- **Array** — feste Länge `new int[n]`, indexbasierter Zugriff per `for`-Schleife (S09 Arrays).
- **`List<T>`** — dynamische Größe, Elemente per `Add()`; iteriert bequem mit `foreach` (S10 Collections).
- **`string.Join()`** — fügt Elemente mit Trennzeichen zu einem String zusammen, eingebettet via String-Interpolation `$"..."`.
- **Gerade-Filter** — `x % 2 == 0`; manuelles Vorab-Zählen, um das Ziel-Array exakt zu dimensionieren.
- **LINQ** — Query- (`from … where … select`) und Method-Syntax (`Where(...)`) als kompakte Alternative zum manuellen Filtern (Konzept späterer Units).
