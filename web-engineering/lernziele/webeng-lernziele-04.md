---
course: web-engineering
type: lernziele
lecture: 4
date: 2026-04-29
tags:
  - web-engineering
  - lernziele
  - javascript
  - closures
  - json
  - event-loop
  - flashcards/web-engineering
source: 08-JS-Basic.pdf
---

# Lernziele — Vorlesung 4

Was sollten Sie ueber die Einbindung von JavaScript in eine Web-Umgebung wissen?::JavaScript wird bevorzugt in externe Dateien ausgelagert und per `script` mit `defer` eingebunden; DOM-nahe Initialisierung erfolgt typischerweise ueber `DOMContentLoaded`.

Was sollten Sie ueber den Gueltigkeitsbereich von Variablen und Konstanten in JavaScript wissen?::`var` hat Function Scope, `let` und `const` haben Block Scope; Funktionen erzeugen neue Scopes und `const` fixiert nur die Bindung, nicht automatisch Objektinhalte.

Was sollten Sie ueber Arrays, Strings und Template Literals in JavaScript wissen?::Strings lassen sich mit `+` oder Template Literals verarbeiten, Arrays sind dynamisch und indexbasiert, waehrend benannte Schluessel in Objekte gehoeren.

Was sollten Sie ueber Closures und die Arrow-Notation wissen?::Closures behalten Zugriff auf ihren lexikalischen Scope, und Arrow Functions sind kompakte Funktionen, die kein eigenes `this` binden.

Was sollten Sie ueber JSON und Objekte in JavaScript wissen?::Objekte werden ueber Punkt- oder Klammernotation verarbeitet; JSON bildet strukturierte Daten mit Objekten, Arrays, Strings, Numbers, Booleans und `null` ab.

Was sollten Sie ueber die this-Zeiger-Problematik in JavaScript wissen?::`this` haengt von der Aufrufart ab; typische Gegenmittel sind Wrapper-Funktionen, `bind()`, `call()`, `apply()` oder passende Arrow Functions.

Was sollten Sie ueber Hoisting wissen?::Deklarationen werden vorgezogen, Initialisierungen nicht; bei `let` und `const` gilt zusaetzlich die Temporal Dead Zone.

Was sollten Sie ueber Klassen und Vererbung in JavaScript wissen?::`class` ist in JavaScript syntaktischer Zucker ueber dem prototype-basierten Modell; Vererbung erfolgt modern ueber `extends`.

Was sollten Sie ueber die Event-Loop wissen?::Die Event Loop fuehrt wartende Callbacks erst aus, wenn der Call Stack frei ist; so entsteht asynchrones, nicht blockierendes Verhalten trotz single-threaded JavaScript.
