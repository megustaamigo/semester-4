---
course: data-science
type: selbstkontrolle
lecture: 2
date: 2026-04-07
tags:
  - data-science
  - selbstkontrolle
  - python
  - flashcards/data-science
source: 02_Python.pdf
---

# Selbstkontrolle 02 — Python

**Quelle:** [[data-science/resources/02_Python.pdf|02_Python.pdf]], Folien 34–35

## Fragen

Wie werden Codeblöcke in Python gekennzeichnet?::Durch Einrückung (Indentation). Es gibt keine geschweiften Klammern `{}` wie in anderen Sprachen.

Welche Vorteile hat Python?::Einfache Syntax, effiziente Entwicklung, interpretiert, Garbage Collection, dynamische Typisierung, Open Source mit riesigem Paket-Ökosystem (~755.000 Pakete).

Wie kann man Python-Code lesbar machen?::Durch Type Hints (Typangaben für Parameter/Rückgabewerte), Docstrings (Dokumentation mit `:param`/`:return`) und sinnvolle Zeilenumbrüche.

Was ist der Unterschied zwischen einer Liste und einem Tupel in Python?::Eine Liste (`[1, 2, 3]`) ist veränderbar (mutable), ein Tupel (`(1, 2, 3)`) ist unveränderbar (immutable).

Welche zwei Arten von Argumenten gibt es für Funktionen?::Arguments (positionale Argumente) und Keyword Arguments (kwargs, benannte Argumente mit Standardwert, z.B. `c: int = 0`).

Was ist der Unterschied zwischen Klassenattributen und Instanzattributen?::Klassenattribute werden auf Klassenebene definiert (z.B. `version = "1.0.0"`) und gelten für alle Instanzen. Instanzattribute werden in `__init__` mit `self.` definiert und sind spezifisch für jede Instanz.

Was ist die Verwendung von ABC (abstract base class) in Python?::ABCs ersetzen Interfaces in Python. Klassen, die von ABC erben, müssen alle mit `@abstractmethod` markierten Methoden implementieren — so wird eine einheitliche Schnittstelle erzwungen.

Was ist ein Python-Modul?::Eine Python-Datei (`.py`), die Definitionen (Klassen, Funktionen, Variablen) und Anweisungen enthält.

Warum verwenden wir Module und Pakete?::Um Code in verschiedene Dateien aufzuteilen, ihn sauber und übersichtlich zu halten und Wiederverwendbarkeit zu ermöglichen.

Wird Python-Code kompiliert oder interpretiert?::Python wird interpretiert, nicht kompiliert.

Sind Type Hints notwendig?::Nein, sie sind optional (Python ist dynamisch typisiert), aber sie verbessern die Lesbarkeit und ermöglichen statische Code-Analyse.

Was sind Dictionaries in Python?::Datenstrukturen zum Speichern von Key-Value-Paaren, ähnlich dem JSON-Format. Zugriff über Keys, Methoden: `keys()`, `values()`, `items()`, `get()`.

Was ist in Python der Unterschied zwischen [1, 2, 3], (1, 2, 3) und {1, 2, 3}?::`[1, 2, 3]` ist eine Liste (mutable, geordnet), `(1, 2, 3)` ist ein Tupel (immutable, geordnet), `{1, 2, 3}` ist ein Set (mutable, ungeordnet, keine Duplikate).

Wofür wird der Lambda-Operator verwendet?::Zum Erstellen anonymer Funktionen (Einzeiler), z.B. `lambda a, b: a + b`. Kann als Wert einer Variablen zugewiesen werden.

Was ist der Unterschied zwischen einer Methode und einer statischen Methode in einer Python-Klasse?::Eine Methode hat `self` als ersten Parameter und kann auf Instanzattribute zugreifen. Eine statische Methode (mit `@staticmethod`) hat keinen `self`-Parameter und kann nicht auf Instanzattribute zugreifen.

Was ist ein Python-Paket?::Ein Ordner, der eine `__init__.py`-Datei enthält und mehrere Module zusammenfasst. Ermöglicht hierarchische Strukturierung von Code.

Wie können Module in Python importiert werden?::Mit `import module_name` (gesamtes Modul importieren).

Wie kann eine einzelne Definition in Python importiert werden?::Mit `from module_name import DefinitionName`. Bad practice: Wildcards wie `from module import *`.
