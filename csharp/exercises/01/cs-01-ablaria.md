---
course: csharp
type: solution
lecture: 1
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-01
  - loops
source: Units/U01/S99.ProposedSolutions
---

# Ablaria — Primzahlen bis n

**Unit:** [[cs-01-strukturierte-programmierung|U01: Basic Elements of Structured Programming]]
**Tasks:** [[Units/U01/Tasks.md|Tasks.md]]

---

## Ziel

Alle Primzahlen bis zu einer festen natürlichen Zahl $n$ berechnen und ausgeben.

- Nur ungerade Kandidaten prüfen (2 ist die einzige gerade Primzahl)
- Kandidat ist prim, wenn er durch keine bereits gefundene Primzahl ≤ $\sqrt{i}$ teilbar ist

## Lösung

```csharp
using System;
using System.Collections.Generic;

static void Main()
{
    const int n = 50;
    var prims = new List<int> { 2 };

    // nur ungerade Kandidaten ab 3
    for (var i = 3; i <= n; i += 2)
    {
        var max = (int)Math.Floor(Math.Sqrt(i)) + 1;
        var found = false;

        foreach (var prim in prims)
        {
            if (i % prim == 0)      // teilbar -> keine Primzahl
            {
                found = true;
                break;
            }

            if (prim >= max)        // über sqrt(i): kein Teiler mehr möglich
                break;
        }

        if (!found)
            prims.Add(i);
    }

    Console.WriteLine($"prims: [ {string.Join(' ', prims)} ]");
}
```

## Konzepte

- **Probedivision** — ein Kandidat `i` ist prim, wenn `i % prim != 0` für alle relevanten Primteiler gilt.
- **$\sqrt{i}$-Schranke** — es genügt, Teiler bis $\sqrt{i}$ zu prüfen; größere Teiler hätten einen kleineren Komplementärteiler, der schon getestet wurde. Das `max`-Abbruchkriterium spart Iterationen.
- **Geschachtelte Schleifen + `break`** — `for` über die Kandidaten, `foreach` über bereits gefundene Primzahlen; `break` verlässt die innere Schleife frühzeitig (S13 Iteration / Jump Statements).
- **Inkrement `i += 2`** — überspringt alle geraden Zahlen, da diese außer 2 nie prim sind.
