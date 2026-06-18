---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - interfaces
source: Units/U02/S99.ProposedSolutions
---

# Buetiva — Interface & abstrakte Klasse (Kontakt-Verwaltung)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine Kontakt-Verwaltung mit Interface und abstrakter Basisklasse entwerfen.

- Abstrakte Klasse `BusinessContact` (Name, Liste von Mail-Adressen)
- Interface `IReport` mit `MonthlyReport`
- Ableitungen `Customer` (`decimal Balance`) und `Supplier` (`string BankDetails`)
- Beide implementieren `MonthlyReport`; `MonthlyReport` bleibt in `BusinessContact` abstrakt
- Gemeinsame Liste füllen und `MonthlyReport` polymorph aufrufen

## Lösung

```csharp
interface IReport
{
    void MonthlyReport();
}

abstract class BusinessContact : IReport
{
    public List<string> MailAddresses { get; set; }

    public string Name { get; set; }

    public abstract void MonthlyReport();

    public override string ToString() => $"Name: {this.Name}, E-Mails: {string.Join(",", this.MailAddresses)}";
}

class Customer : BusinessContact
{
    public decimal Balance { get; set; }

    public override void MonthlyReport() => Console.WriteLine($"Kunde: [{this}], Kontostand: {this.Balance:c}");
}

class Supplier : BusinessContact
{
    public string BankDetails { get; set; }

    public override void MonthlyReport() => Console.WriteLine($"Lieferant: [{this}], Bankverbindung: {this.BankDetails}");
}

// --- Verwendung ---
static void Main()
{
    Customer bert = new()
    {
        Name = "Susan M. Johnson",
        MailAddresses = ["SusanMJohnson@rhyta.com", "susan@brentandsahar.com"],
        Balance = 689.78m
    };

    Supplier shop = new()
    {
        Name = "Shop",
        MailAddresses = ["info@shop.com"],
        BankDetails = "DE91100000000123456789"
    };

    var list = new List<BusinessContact> { bert, shop };
    foreach (var kontakt in list)
    {
        kontakt.MonthlyReport();
    }
}
```

## Konzepte

- **Interface `IReport`** — definiert den Vertrag `MonthlyReport()` ohne Implementierung (S08 Interfaces).
- **Abstrakte Klasse** — `BusinessContact` bündelt gemeinsamen Zustand (Name, Mail-Adressen) und lässt `MonthlyReport` abstrakt, sodass jede Ableitung es selbst implementieren muss (S06 Abstract & Sealed).
- **Polymorphie** — die `List<BusinessContact>` hält Kunden und Lieferanten; `kontakt.MonthlyReport()` ruft je nach Laufzeittyp die passende Überschreibung (S05 Inheritance).
- **`decimal` & `:c`** — `decimal` für Geldbeträge, Format `:c` für Währungsausgabe.
- **Collection-Expression `[...]`** — kompakte Listeninitialisierung (`MailAddresses = [...]`).
