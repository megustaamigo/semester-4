---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - lambda
source: Units/U03/S99.ProposedSolutions
---

# Callaekur — Lambda für Intervall-Test

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Lambda-Ausdruck für einen Intervall-Test definieren.

- Lambda-Ausdruck, der testet, ob eine Integer-Zahl im Intervall [a, b] (a, b Integer) liegt

## Lösung

```csharp
using System;

// --- Verwendung ---

var inside = (int x, int a, int b) => a <= x && x <= b;

Console.WriteLine($"2 in [3,4]={inside(2, 3, 4)}");   // False
Console.WriteLine($"3 in [3,4]={inside(3, 3, 4)}");   // True
Console.WriteLine($"5 in [3,4]={inside(5, 3, 4)}");   // False
```

## Konzepte

- **Lambda-Ausdruck (S09 Delegates / Lambda)** — `(int x, int a, int b) => a <= x && x <= b` definiert eine anonyme Funktion direkt als Wert; der Compiler leitet daraus einen `Func<int, int, int, bool>` ab.
- **Intervall-Test** — die zusammengesetzte Bedingung `a <= x && x <= b` prüft die Zugehörigkeit zum abgeschlossenen Intervall [a, b].
