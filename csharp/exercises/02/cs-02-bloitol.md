---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - properties
source: Units/U02/S99.ProposedSolutions
---

# Bloitol — Klasse Student (Properties, Konstruktoren)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine Klasse für Studierende entwerfen.

- Schreibgeschützte Properties `FirstName`, `LastName`, `Matriculation`
- `StartOfStudy` (`DateTime`) mit Getter/Setter, nur 01.10. zulässig
- Matrikelnummer bei Instanziierung berechnet (`<Vorname[0]><Nachname[0]><4-stellig zufällig>`)
- Geeignete Konstruktoren
- `GetSemester(DateTime date)` — Semester zum Datum, 0 vor Studienbeginn
- `ToString` überschreiben

## Lösung

```csharp
class Student
{
    public Student(string firstName, string lastName)
    {
        this.FirstName = firstName ?? throw new ArgumentNullException(nameof(firstName));
        this.LastName = lastName ?? throw new ArgumentNullException(nameof(lastName));
        this.Matriculation = string.Concat(this.FirstName[0], this.LastName[0], new Random().Next(1000, 9999).ToString());
    }

    public string FirstName { get; }

    public string LastName { get; }

    public string Matriculation { get; }

    public DateTime StartOfStudy
    {
        get => field;
        set
        {
            if (value.Day != 1 || value.Month != 10)
                throw new ArgumentException("Der Studienstart ist ungültig.");
            field = value;
        }
    }

    public int GetSemester(DateTime date)
    {
        if (date < this.StartOfStudy || date >= this.StartOfStudy.AddYears(3))
            return 0;

        if (date.Month <= 3)
            return (2 * (date.Year - this.StartOfStudy.Year - 1)) + 1;

        if (date.Month >= 10)
            return (2 * (date.Year - this.StartOfStudy.Year)) + 1;

        return 2 * (date.Year - this.StartOfStudy.Year);
    }

    public override string ToString() => $"{this.FirstName} {this.LastName}, {this.Matriculation}, {this.GetSemester(DateTime.Today)}. Semester";
}

// --- Verwendung ---
static void Main()
{
    var student = new Student("John", "Doe")
    {
        StartOfStudy = new DateTime(2025, 10, 1)
    };
    Console.WriteLine(student);
}
```

## Konzepte

- **Read-only Properties** — nur `{ get; }` ohne Setter; werden im Ctor gesetzt und sind danach unveränderlich (S02 Fields & Properties).
- **`field`-Keyword** — im Setter von `StartOfStudy` greift `field` direkt auf das compilergenerierte Backing Field zu, ohne ein eigenes Feld deklarieren zu müssen; ermöglicht Validierung (nur 01.10.) ohne explizites Hilfsfeld.
- **Validierung im Setter** — wirft `ArgumentException` bei ungültigem Datum.
- **Konstruktor mit Null-Check** — `?? throw new ArgumentNullException(...)` erzwingt nicht-leere Namen (S04 Constructors).
- **`ToString()` überschreiben** — formatierte Ausgabe inkl. berechnetem Semester (S05 Inheritance).
