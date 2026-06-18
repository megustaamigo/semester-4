---
course: csharp
type: solution
lecture: 1
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-01
  - floating-point
source: Units/U01/S99.ProposedSolutions
---

# Awhya — Mercator-Reihe & Fließkomma-Präzision

**Unit:** [[cs-01-strukturierte-programmierung|U01: Basic Elements of Structured Programming]]
**Tasks:** [[Units/U01/Tasks.md|Tasks.md]]

---

## Ziel

Die Partialsumme der alternierenden harmonischen Reihe (Mercator-Reihe) bis zum 1000. Term auf drei Arten berechnen und die begrenzte Präzision von Fließkommazahlen kennenlernen. Das korrekte Ergebnis der unendlichen Summe ist $\ln(2)$.

- Summe von links nach rechts
- Summe von rechts nach links
- Summe der Teilsummen der positiven und negativen Terme
- Grund für die unterschiedlichen Ergebnisse benennen

## Lösung

```csharp
using System;

static void Main()
{
    // Mercator-Reihe: ln(2) = 1 - 1/2 + 1/3 - 1/4 + ...
    var sum = 0.0;
    var mul = 1.0;
    var limit = 1000;

    // Variante 1: von links nach rechts
    for (var i = 1; i <= limit; i++, mul *= -1.0)
        sum += 1.0 / (mul * i);
    Console.WriteLine($"01| sum = {sum}, diff = {sum - Math.Log(2.0)}");

    // Variante 2: von rechts nach links
    sum = 0.0;
    mul = -1.0;
    for (var i = limit; i >= 1; --i, mul *= -1.0)
        sum += 1.0 / (mul * i);
    Console.WriteLine($"02| sum = {sum}, diff = {sum - Math.Log(2.0)}");

    // Variante 3: Teilsummen der positiven und negativen Terme
    var sumPos = 0.0;
    var sumNeg = 0.0;
    for (var i = 1; i <= limit; i += 2)
        sumPos += 1.0 / i;
    for (var i = 2; i <= limit; i += 2)
        sumNeg -= 1.0 / i;
    sum = sumPos + sumNeg;
    Console.WriteLine($"03| sum = {sum}, diff = {sum - Math.Log(2.0)}");
}
```

## Konzepte

- **Alternierende harmonische Reihe** — `1/1 - 1/2 + 1/3 - …`; das Vorzeichen wird hier über den Faktor `mul` gesteuert, der pro Iteration mit `-1.0` multipliziert wird.
- **Schleifen-Varianten** — vorwärts (`i++`), rückwärts (`--i`) und schrittweise `i += 2` zum getrennten Aufsummieren gerader/ungerader Indizes (S13 Iteration Statements).
- **Fließkomma-Rundung** — die drei Varianten liefern leicht **unterschiedliche** Ergebnisse, weil `double` nur endlich viele Stellen speichert: Bei jeder Addition wird gerundet. Werden viele kleine Beträge zu einem bereits großen Wert addiert, gehen niederwertige Bits verloren. Die **Reihenfolge der Addition** ändert daher das Ergebnis — von rechts nach links (kleine Beträge zuerst) ist meist genauer als von links nach rechts.
- **`decimal` als Alternative** — höhere Präzision (128 Bit, dezimal) reduziert den Rundungsfehler deutlich, ist aber langsamer und ebenfalls nicht exakt für die unendliche Summe.
