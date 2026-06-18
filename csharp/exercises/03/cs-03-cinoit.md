---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - delegates
source: Units/U03/S99.ProposedSolutions
---

# Cinoit — Delegate für f: Z×Z→Z

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Funktionswert für Ganzzahlen in zwei Veränderlichen berechnen.

- Delegate definieren, mit dem sich Funktionen f: Z×Z→Z aufrufen lassen
- Funktionen zur Addition und Multiplikation passend zur Delegate-Signatur definieren
- Beide Funktionen über das Delegate mit Testwerten aufrufen

## Lösung

```csharp
using System;

delegate int TwoOperands(int x, int y);

static int Add(int x, int y) => x + y;

static int Multiply(int x, int y) => x * y;

// --- Verwendung ---

// Variante 1: Methodenreferenz
TwoOperands add = Add;
TwoOperands multiply = Multiply;

// Variante 2: Lambda-Ausdruck
var add2 = (int x, int y) => x + y;

// Variante 3: Lokale Funktion
static int add3(int x, int y) => x + y;

int n = 2, m = 3;
Console.WriteLine($"{n} + {m} = {add(2, 3)}");
Console.WriteLine($"{n} * {m} = {multiply(2, 3)}");
Console.WriteLine($"{n} + {m} = {add2(2, 3)}");
Console.WriteLine($"{n} + {m} = {add3(2, 3)}");
```

## Konzepte

- **Delegate (S09 Delegates)** — `delegate int TwoOperands(int x, int y)` deklariert einen Funktionstyp für f: Z×Z→Z. Ein Delegate ist eine typsichere Referenz auf eine Methode.
- **Methodenreferenz** — `TwoOperands add = Add;` weist eine passende Methode zu; der Aufruf `add(2, 3)` ruft sie indirekt auf.
- **Alternative Zuweisungen** — dieselbe Signatur lässt sich auch über einen Lambda-Ausdruck (`(int x, int y) => x + y`) oder eine lokale Funktion realisieren.
