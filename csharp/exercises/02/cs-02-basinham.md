---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - strings
source: Units/U02/S99.ProposedSolutions
---

# Basinham — Muster in Text finden (FindAll)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Alle Vorkommen eines Musters in einem Text finden und deren Positionen zurückgeben.

- Methode `FindAll` implementieren, die eine Liste der Fundpositionen liefert
- Hier ohne Überlappung: nach einem Treffer wird hinter das Muster gesprungen

## Lösung

```csharp
static List<int> FindAll(string s, string pattern)
{
    var positions = new List<int>();
    var length = pattern.Length;

    var i = 0;
    while (i + length <= s.Length)
    {
        if (s.Substring(i, length) == pattern)
        {
            positions.Add(i);
            i += length;        // keine Überlappung
        }
        else
        {
            i++;
        }
    }

    return positions;
}

static void Main()
{
    var s = "123ab123ababcab";
    var res1 = FindAll(s, "123");
    Console.WriteLine($"find '123': [{string.Join(", ", res1)}]");

    var res2 = FindAll(s, "ab");
    Console.WriteLine($"find 'ab': [{string.Join(", ", res2)}]");
}
```

## Konzepte

- **`List<int>`** — generische, dynamisch wachsende Liste für die Positionen (S03 Methods / Collections).
- **`string.Substring(i, length)`** — schneidet das Vergleichsfenster aus dem Text.
- **Schleifeninvariante** — `i + length <= s.Length` stellt sicher, dass `Substring` nicht über das Stringende liest.
- **Überlappung** — durch `i += length` werden Treffer nicht überlappend gezählt (Designentscheidung laut Aufgabe).
