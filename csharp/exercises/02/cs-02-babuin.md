---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - classes
source: Units/U02/S99.ProposedSolutions
---

# Babuin — Klasse Contact

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine Klasse für Kontakte entwerfen.

- Klasse `Contact` mit parameterlosem Konstruktor
- Konstruktor mit Name und Alter
- Eigenschaften für Name und Alter
- `ToString` zur formatierten Ausgabe überschreiben

## Lösung

```csharp
class Contact
{
    public Contact() => this.Name = "N.N.";

    public Contact(string name, int age)
    {
        this.Name = name;
        this.Age = age;
    }

    public int Age { get; set; }

    public string Name { get; set; }

    public override string ToString() => $"Contact: {this.Name}, age: {this.Age}.";
}

// --- Verwendung ---
static void Main()
{
    var first = new Contact
    {
        Name = "Werner",
        Age = 23
    };
    Console.WriteLine(first);

    var second = new Contact("Uschi", 45);
    Console.WriteLine(second);
}
```

## Konzepte

- **Auto-Properties** — `{ get; set; }` erzeugt das Backing Field automatisch (S02 Fields & Properties).
- **Konstruktor-Überladung** — parameterloser Ctor setzt Default `"N.N."`, der zweite initialisiert beide Felder (S04 Constructors).
- **Object-Initializer** — `new Contact { Name = ..., Age = ... }` setzt Properties nach dem parameterlosen Ctor.
- **`ToString()` überschreiben** — `override` liefert eine lesbare Darstellung; wird von `Console.WriteLine` automatisch genutzt (S05 Inheritance / `object`).
