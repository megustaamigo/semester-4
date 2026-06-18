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

# Bathalia — Fallunterscheidung & Bedingungsoperator

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine mathematische Funktion mit Fallunterscheidung implementieren: $f(n) = \frac{n}{2}$, falls $n \ge 0$ und gerade, sonst $f(n) = 0$.

- Funktion für Integer implementieren
- Modulo-Operator `%` und Bedingungsoperator `?` nutzen
- Mit Beispielzahlen und formatierter Ausgabe testen

## Lösung

```csharp
static int MyFunction(int n) => (n >= 0 && n % 2 == 0) ? n / 2 : 0;

static void PrintFunctionValue(int n) => Console.WriteLine($"f({n}) = {MyFunction(n)}");

static void Main()
{
    PrintFunctionValue(-10);
    PrintFunctionValue(0);
    PrintFunctionValue(2023);
    PrintFunctionValue(2024);
}
```

## Konzepte

- **Bedingungsoperator `?:`** — kompakte Fallunterscheidung als Ausdruck statt `if/else` (S03 Methods / Operatoren).
- **Modulo `%`** — `n % 2 == 0` prüft Geradheit.
- **Expression-bodied Methods (`=>`)** — einzeilige Methoden ohne Block-Body (S03 Methods).
