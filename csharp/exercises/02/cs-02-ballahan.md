---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - inheritance
source: Units/U02/S99.ProposedSolutions
---

# Ballahan — Vererbung: Shape (primäre Konstruktoren)

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Eine Geometrie-Anwendung mit verschiedenen Formen entwerfen.

- Abstrakte Generalisierung `Shape` (keine Instanzen erzeugbar)
- `Move` — nicht überschreibbar; einzige Möglichkeit, die Position zu ändern
- `Reset` — überschreibbare Standardimplementierung
- Ableitungen Kreis, Quadrat, Rechteck mit primären Konstruktoren; Größen ganzzahlig, nur bei Instanziierung setzbar
- Korrekte Berechnung von Umfang und Flächeninhalt
- Einheitliche Standardausgabe (Bezeichnung, Mittelpunkt, Umfang, Flächeninhalt)

## Lösung

```csharp
abstract class Shape
{
    protected Shape(int x, int y) => this.Move(x, y);

    public abstract double Area { get; }

    public int BorderSize { get; set; } = 2;

    public int CenterX { get; private set; }

    public int CenterY { get; private set; }

    public abstract double Perimeter { get; }

    protected abstract string Description { get; }

    public void Move(int x, int y)         // nicht virtuell -> nicht überschreibbar
    {
        this.CenterX = x;
        this.CenterY = y;
    }

    public virtual void Reset() => this.Move(0, 0);

    public override string ToString() => $"{this.Description} | Mittelpunkt ({this.CenterX},{this.CenterY}) | Umfang {this.Perimeter:0.##} | Flächeninhalt {this.Area:0.##}";
}

class Circle(int x, int y, int radius) : Shape(x, y)
{
    public override double Area => Math.PI * this.Radius * this.Radius;

    public override double Perimeter => 2 * Math.PI * this.Radius;

    public int Radius { get; } = radius;

    protected override string Description => "Kreis";
}

class Rectangle(int x, int y, int width, int height) : Shape(x, y)
{
    public override double Area => this.Width * this.Height;

    public int Height { get; } = height;

    public override double Perimeter => 2 * (this.Width + this.Height);

    public int Width { get; } = width;

    protected override string Description => "Rechteck";
}

class Square(int x, int y, int side) : Shape(x, y)
{
    public override double Area => this.Side * this.Side;

    public override double Perimeter => 4 * this.Side;

    public int Side { get; } = side;

    protected override string Description => "Quadrat";
}

// --- Verwendung ---
static void Main()
{
    var rectangle = new Rectangle(5, 2, 3, 4)
    {
        BorderSize = 3
    };
    Console.WriteLine(rectangle);

    rectangle.Move(-2, 0);
    Console.WriteLine(rectangle);

    var square = new Square(1, 1, 5);
    Console.WriteLine(square);

    var circle = new Circle(0, 0, 3);
    Console.WriteLine(circle);
}
```

## Konzepte

- **Abstrakte Klasse `Shape`** — kann nicht instanziiert werden; definiert abstrakte Properties `Area`, `Perimeter`, `Description` (S06 Abstract & Sealed).
- **Primäre Konstruktoren** — `class Circle(int x, int y, int radius) : Shape(x, y)` übernimmt Parameter direkt und reicht `x, y` an die Basis weiter; `Radius { get; } = radius;` initialisiert die read-only Property.
- **`protected` Konstruktor** — `Shape` nur für Ableitungen aufrufbar.
- **Versiegeltes Verhalten via Nicht-`virtual`** — `Move` ist nicht `virtual`, daher in Ableitungen nicht überschreibbar; `Reset` ist `virtual` und damit modifizierbar (S05 Inheritance).
- **`private set`** — `CenterX`/`CenterY` nur intern (über `Move`) änderbar, von außen schreibgeschützt.
