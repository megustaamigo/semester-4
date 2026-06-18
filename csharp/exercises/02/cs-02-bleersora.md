---
course: csharp
type: solution
lecture: 2
date: 2026-06-08
tags:
  - csharp
  - solution
  - unit-02
  - strings
source: Units/U02/S99.ProposedSolutions
---

# Bleersora — Palindrome prüfen

**Unit:** [[cs-02-objektorientierte-programmierung|U02: Basic Elements of Object-Oriented Programming]]
**Tasks:** [[Units/U02/Tasks.md|Tasks.md]]

---

## Ziel

Prüfen, ob ein Text ein Palindrom ist (vorwärts wie rückwärts gleich), z.B. "Na, Freibierfan!".

- Text einlesen und in eigener Methode prüfen
- Groß-/Kleinschreibung, Leerzeichen und Satzzeichen ignorieren

## Lösung

```csharp
static bool IsPalindrom(string check)
{
    var s = check.ToLower();
    int i = 0, j = s.Length - 1;
    while (i < j)
    {
        // Nicht-Buchstaben von beiden Seiten überspringen
        while (i < j && (s[i] < 'a' || s[i] > 'z'))
            i++;
        while (i < j && (s[j] < 'a' || s[j] > 'z'))
            j--;
        if (j <= i || s[i] != s[j])
            return false;
        i++;
        j--;
    }

    return true;
}

static void Main()
{
    string s = "Na,Freibierfan!";
    Console.WriteLine($"{s}? {IsPalindrom(s)}");

    s = "Anna";
    Console.WriteLine($"{s}? {IsPalindrom(s)}");

    s = "abcba";
    Console.WriteLine($"{s}? {IsPalindrom(s)}");

    s = "abcb";
    Console.WriteLine($"{s}? {IsPalindrom(s)}");
}
```

## Konzepte

- **Zwei-Zeiger-Verfahren** — `i` läuft von vorn, `j` von hinten zur Mitte; vergleicht spiegelbildliche Zeichen.
- **Normalisierung** — `ToLower()` ignoriert Groß-/Kleinschreibung; die inneren Schleifen überspringen alles außer `a`–`z` (Leerzeichen, Satzzeichen).
- **Frühzeitiger Abbruch** — bei erstem ungleichen Paar `return false`.
- **Indexzugriff `s[i]`** — Zeichenweiser Zugriff auf den `string`.
