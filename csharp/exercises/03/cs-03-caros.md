---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - inheritance
source: Units/U03/S99.ProposedSolutions
---

# Caros — Zoo: Operatoren += / -= & Vererbung

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Klassen entwerfen, um einen Zoo zu verwalten.

- Basisklasse `Animal` mit gemeinsamen Eigenschaften
- Abgeleitete Klassen für bestimmte Arten (`Lion`, `Bird`) mit speziellen Eigenschaften
- Klasse `Zoo`, die alle Tiere enthält
- Tiere über `+=` hinzufügen bzw. über `-=` löschen
- Alle Tiere ausgeben

## Lösung

```csharp
using System.Collections.Generic;

class Animal
{
    public string Name { get; set; }

    public override string ToString() => $"Name: {this.Name}";
}

class Lion : Animal
{
    public bool HasMane { get; set; }

    public override string ToString() => $"{base.ToString()}, hat Mähne: {this.HasMane}";
}

class Bird : Animal
{
    public bool HasFeathers { get; set; }

    public override string ToString() => $"{base.ToString()}, hat Federn: {this.HasFeathers}";
}

class Zoo : List<Animal>
{
    public static Zoo operator -(Zoo zoo, string name)
    {
        var item = zoo.Find(t => t.Name == name);
        if (item is not null)
            _ = zoo.Remove(item);
        return zoo;
    }

    public static Zoo operator +(Zoo zoo, Animal tier)
    {
        zoo.Add(tier);
        return zoo;
    }
}

// --- Verwendung ---

var clemens = new Lion { Name = "Clemens", HasMane = true };
var gwaihir = new Bird { Name = "Gwaihir", HasFeathers = true };

Console.WriteLine(clemens);
Console.WriteLine(gwaihir);

var zoo = new Zoo();
zoo += clemens;                                  // operator + / +=
zoo += gwaihir;
Console.WriteLine($"Tiere im Zoo: {string.Join(", ", zoo)}");

zoo -= "Bert, der nicht verzeichnet ist";        // nicht vorhanden -> keine Änderung
zoo -= "Gwaihir";                                // operator - / -=
Console.WriteLine($"Tiere im Zoo: {string.Join(", ", zoo)}");
```

## Konzepte

- **Vererbung (S04 Inheritance)** — `Lion` und `Bird` erben `Name` von `Animal` und ergänzen eigene Eigenschaften. `ToString` wird mit `override` und `base.ToString()` erweitert.
- **`Zoo : List<Animal>`** — durch Ableiten von `List<Animal>` erbt der Zoo die komplette Listenfunktionalität (`Add`, `Remove`, `Find`, Enumeration).
- **Operatorüberladung mit Compound-Assignment (S02 Operators)** — die binären Operatoren `+` (Tier hinzufügen) und `-` (Tier per Name entfernen) werden überladen; `+=` / `-=` nutzen diese automatisch. Beide geben den modifizierten `Zoo` zurück.
- **Polymorphie** — `string.Join(", ", zoo)` ruft das jeweils überschriebene `ToString` der konkreten Tierart auf.
