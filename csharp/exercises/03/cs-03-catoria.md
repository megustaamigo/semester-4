---
course: csharp
type: solution
lecture: 3
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-03
  - generics
source: Units/U03/S99.ProposedSolutions
---

# Catoria — Generischer Stack mit Events (MyStack<T>)

**Unit:** [[cs-03-advanced|U03: Advanced Elements of C#]]
**Tasks:** [[Units/U03/Tasks.md|Tasks.md]]

---

## Ziel

Einen dynamischen und generischen Stapelspeicher entwerfen.

- Generische Klasse `MyStack<TItem>`, intern auf `LinkedList<TItem>` (dynamisch wachsend)
- Standard-Operationen `Push`, `Pop`, `Peek`
- Methode `Empty`: liefert alle Einträge als Array in LIFO-Reihenfolge und leert den Stapel
- Events beim Hinzufügen/Entnehmen, mit generischen Ereignisargumenten, die das Element enthalten
- Test mit 'Alpha', 'Beta', 'Gamma', 'Delta' und anschließendem `Empty`, Ausgabe über die Events

## Lösung

```csharp
using System;
using System.Collections.Generic;

class MyStack<TItem>
{
    public event EventHandler<ItemEventArgs<TItem>> ItemPopped;

    public event EventHandler<ItemEventArgs<TItem>> ItemPushed;

    readonly LinkedList<TItem> list = new();

    public int Count { get; private set; }

    public TItem[] Empty()
    {
        var array = new TItem[this.Count];
        var i = 0;
        while (this.Count > 0)
        {
            array[i++] = this.Pop();
        }
        return array;
    }

    public TItem Peek() => this.Count == 0 ? throw new InvalidOperationException("Stack is empty") : this.list.Last.Value;

    public TItem Pop()
    {
        if (this.Count == 0)
            throw new InvalidOperationException("Stack is empty");
        var item = this.list.Last.Value;
        this.list.RemoveLast();
        this.Count--;
        this.ItemPopped?.Invoke(this, new ItemEventArgs<TItem> { Item = item });
        return item;
    }

    public void Push(TItem item)
    {
        _ = this.list.AddLast(item);
        this.Count++;
        this.ItemPushed?.Invoke(this, new ItemEventArgs<TItem> { Item = item });
    }
}

class ItemEventArgs<TItem> : EventArgs
{
    public TItem Item { get; init; }
}

// --- Verwendung ---

var stack = new MyStack<string>();
stack.ItemPopped += (sender, e) => Console.WriteLine($"'{e.Item}' wurde vom Stapel entnommen.");
stack.ItemPushed += (sender, e) => Console.WriteLine($"'{e.Item}' wurde dem Stapel hinzugefügt.");

stack.Push("Alpha");
stack.Push("Beta");
stack.Push("Gamma");
stack.Push("Delta");

Console.WriteLine($"Count: {stack.Count}");
Console.WriteLine(string.Join(", ", stack.Empty()));   // LIFO: Delta, Gamma, Beta, Alpha
Console.WriteLine($"Count: {stack.Count}");            // 0
```

## Konzepte

- **Generische Klasse (S05 Generics)** — `MyStack<TItem>` arbeitet typsicher mit beliebigen Elementtypen; auch `ItemEventArgs<TItem>` ist generisch.
- **`LinkedList<TItem>` als interne Datenstruktur** — wächst dynamisch; `AddLast`/`RemoveLast`/`Last` realisieren LIFO ohne festes Kapazitätslimit.
- **Events (S10 Events)** — `ItemPushed` und `ItemPopped` vom Typ `EventHandler<ItemEventArgs<TItem>>` ermöglichen es externen Klassen, sich an- bzw. abzumelden; ausgelöst über `?.Invoke(...)` (null-sicher).
- **Stapel-Operationen** — `Peek` liest die Spitze ohne Entnahme, `Pop` entnimmt, `Push` legt ab; `Empty` leert über wiederholtes `Pop` und liefert das Ergebnis als Array.
- **Fehlerbehandlung** — `Pop`/`Peek` werfen `InvalidOperationException` bei leerem Stapel.
