---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - files
source: Units/U02
---

# Boudadillo — Objekte aus Dateien & Tabelle (Bundesliga)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

> [!info] Hinweis
> Für diese Aufgabe gibt es keine offizielle Musterlösung im Repo — die folgende Lösung ist selbst erstellt und nutzt die Konzepte aus U01 (Dateien, S15) und U02 (Klassen).

---

## Ziel

Aus einer Ergebnis-Datei Objekte erzeugen und eine Bundesliga-Tabelle formatiert ausgeben.

- `Bundesliga-0.txt` aus den Snippets einlesen
- Jede Zeile in eine zweite Datei schreiben, aber in umgekehrter Reihenfolge (letzte Zeile zuerst — Zeilen NICHT zeichenweise umdrehen)
- Klasse `Club` mit geeigneten Eigenschaften entwerfen
- Punkte und Tore je Verein berechnen (in eigener Klasse `Table`)
- Vereine und Tabelle formatiert ausgeben: erst Konsole, dann Datei

## Lösung

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

// Verein mit kumuliertem Saison-Zustand.
class Club
{
    public string Name { get; }
    public int Points { get; set; }
    public int GoalsFor { get; set; }
    public int GoalsAgainst { get; set; }

    public int GoalDifference => this.GoalsFor - this.GoalsAgainst;

    public Club(string name) => this.Name = name;

    public override string ToString() =>
        $"{this.Name,-20} {this.Points,3} {this.GoalsFor,3}:{this.GoalsAgainst,-3} ({this.GoalDifference,3})";
}

// Sammelt alle Spielergebnisse und baut daraus die Tabelle.
class Table
{
    readonly Dictionary<string, Club> clubs = new();

    Club GetOrAdd(string name)
    {
        if (!this.clubs.TryGetValue(name, out var club))
        {
            club = new Club(name);
            this.clubs[name] = club;
        }
        return club;
    }

    // Erwartetes Zeilenformat: "HeimVerein HeimTore:GastTore GastVerein"
    // HINWEIS: Das Parsing muss an das echte Format von Bundesliga-0.txt
    // angepasst werden — hier defensiv mit Trennzeichen-Annahme implementiert.
    public void AddResult(string line)
    {
        var parts = line.Split(' ', StringSplitOptions.RemoveEmptyEntries);
        if (parts.Length != 3 || !parts[1].Contains(':'))
            return; // Zeile passt nicht zum Format -> ignorieren

        var goals = parts[1].Split(':');
        if (!int.TryParse(goals[0], out var home) || !int.TryParse(goals[1], out var away))
            return;

        var homeClub = this.GetOrAdd(parts[0]);
        var awayClub = this.GetOrAdd(parts[2]);

        homeClub.GoalsFor += home;
        homeClub.GoalsAgainst += away;
        awayClub.GoalsFor += away;
        awayClub.GoalsAgainst += home;

        if (home > away) homeClub.Points += 3;
        else if (home < away) awayClub.Points += 3;
        else { homeClub.Points += 1; awayClub.Points += 1; }
    }

    // Sortierung: Punkte, dann Tordifferenz, dann erzielte Tore.
    public IEnumerable<Club> Ranking() =>
        this.clubs.Values
            .OrderByDescending(c => c.Points)
            .ThenByDescending(c => c.GoalDifference)
            .ThenByDescending(c => c.GoalsFor);

    public string Format()
    {
        var lines = new List<string> { $"{"#",2} {"Verein",-20} {"Pkt",3} {"Tore",-7} {"Diff"}" };
        var rank = 1;
        foreach (var club in this.Ranking())
            lines.Add($"{rank++,2} {club}");
        return string.Join(Environment.NewLine, lines);
    }
}

static void Main()
{
    const string input = "Bundesliga-0.txt";
    const string reversed = "Bundesliga-reversed.txt";
    const string output = "Tabelle.txt";

    var allLines = File.ReadAllLines(input);

    // Zeilen in umgekehrter Reihenfolge in zweite Datei schreiben.
    File.WriteAllLines(reversed, allLines.Reverse());

    var table = new Table();
    foreach (var line in allLines)
        table.AddResult(line);

    var formatted = table.Format();

    // Erst Konsole, dann Datei.
    Console.WriteLine(formatted);
    File.WriteAllText(output, formatted);
}
```

## Konzepte

- **`File.ReadAllLines` / `WriteAllLines` / `WriteAllText`** — einfache Datei-I/O ohne manuelles Stream-Handling (U01 S15 Dateien).
- **Zeilen umkehren** — `allLines.Reverse()` dreht die Reihenfolge der Zeilen um, nicht deren Inhalt (LINQ).
- **Klasse `Club`** — kapselt den Saison-Zustand; `GoalDifference` als berechnete (read-only) Property (S02 Fields & Properties).
- **Klasse `Table`** — `Dictionary<string, Club>` aggregiert Ergebnisse je Verein; Trennung der Verantwortlichkeiten (Club = Datensatz, Table = Logik/Ausgabe).
- **Defensives Parsing** — `Split` + `int.TryParse`, ungültige Zeilen werden übersprungen, statt eine Exception zu werfen.
- **Formatierte Ausgabe** — Ausrichtungs-Specifier wie `{Name,-20}` für tabellarische Spalten.
