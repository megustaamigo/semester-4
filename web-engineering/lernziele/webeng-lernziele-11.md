---
course: web-engineering
type: lernziele
lecture: 11
date: 2026-05-18
tags:
  - web-engineering
  - lernziele
  - javascript
  - promises
  - async-await
  - event-loop
  - sequelize
  - sql-injection
  - prototypen
  - exceptions
  - flashcards/web-engineering
source: 11-JavaScript-Teil2.pdf
---

# Lernziele — Vorlesung 11 (JavaScript Teil 2)

**Quelle:** [[web-engineering/resources/11-JavaScript-Teil2.pdf|11-JavaScript-Teil2.pdf]]

## Fragen

Warum sind die meisten JS-Standardfunktionen asynchron mit Callbacks?::JavaScript ist **single-threaded** — blockierende I/O wuerde die Event Loop blockieren. Daher kein `return`-Wert, sondern ein **Callback**, der das Ergebnis verarbeitet (`fs.readFile('/file.md', (err, data) => ...)`).

Was ist die "Callback Hell"?::Tiefe Verschachtelung von Callbacks, weil alles, was nach einem Callback passieren soll, **innerhalb** dieses Callbacks aufgerufen werden muss. Schlecht lesbar, Programmablauf unklar.

Was ist ein Promise und welche drei Zustaende hat es?::Ein **Stellvertreter / Proxy** fuer ein zukuenftiges Resultat (vgl. Future in Java). Zustaende: **Pending** (Startzustand), **Fulfilled** (Erfolg), **Rejected** (Misserfolg). Uebergange werden durch `resolve(value)` bzw. `reject(reason)` ausgeloest.

Wie erzeugt man ein Promise und wann wird der Executor ausgefuehrt?::`new Promise((resolve, reject) => { ... })`. Der Executor wird **direkt synchron ausgefuehrt** — die asynchronen Operationen darin rufen resolve/reject spaeter ueber die Event Queue auf. Diese Aufrufe legen die Handler in die **Microtask Queue**, die nach Abarbeitung des Call-Stacks laeuft.

Was machen `then` und `catch`?::`then(onFulfilled)` registriert einen Handler fuer das Resultat. `catch(onRejected)` registriert einen Handler fuer Fehler — entspricht `then(null, onRejected)`. Beide liefern **neue Promise-Objekte** zurueck → **Chaining**. Ein `catch()` reicht fuer alle Rejections in der Kette.

Wann liefert `Promise.resolve(value)` zurueck?::Es gibt **selbst ein Promise** im Fulfilled-Zustand zurueck. Daher ist Chaining moeglich: `fsProm(...).then(...).then(...).catch(...)`.

Wann werden Promises bei Chaining korrekt weitergereicht?::Wenn das `then`-Callback ein **Promise zurueckgibt** (entweder `return promise` oder eine async-Funktion). Ohne `return` ist die naechste `then`-Stufe `undefined`, weil `console.log` selbst kein Promise ist.

Was bedeutet `async` vor einer Funktion?::Die Funktion liefert **immer ein Promise**. `return x` entspricht `return Promise.resolve(x)`, `throw e` entspricht `return Promise.reject(e)`. Innerhalb der Funktion ist `await` erlaubt.

Was tut `await` technisch?::Wertet den Promise aus und liefert den Code dahinter **als Microtask auf der Job-Queue** aus — der Thread blockiert nicht. `await` ist syntaktischer Zucker fuer ein `.then()`-Chaining. Alles hinter `await` kommt in den then-Block.

Wo darf `await` verwendet werden?::Nur in `async`-Funktionen, sonst SyntaxError. Ab Node 14.8 ist **Top-Level await** im **ES-Modul-Modus** moeglich. Auch in IIFE: `(async () => { ... })()`.

Wie bildet man das catch-Verhalten in einer async-Funktion ab?::Mit klassischem `try/catch`. Ein `throw` in der async-Funktion wird zu `Promise.reject(e)`.

Warum sollten asynchrone Funktionen mit `await` parallelisiert werden?::Mehrere `await` in Folge laufen sequentiell — `Promise.all([p1, p2])` laesst beide Operationen parallel laufen, dann wird auf alle gemeinsam gewartet. `const [a, b] = await Promise.all([asyncThing1(), asyncThing2()])`.

Warum sind Promises schneller als Timeouts?::`setTimeout`-Callbacks landen auf der **Task Queue (macrotask)**, `Promise.then`-Callbacks auf der **Job Queue (microtask)**. Die Job-Queue wird **direkt nach dem Call-Stack** abgearbeitet — vor der naechsten macrotask. Also `Promise.resolve().then(...)` vor `setTimeout(..., 0)`.

In welcher Reihenfolge werden Konsolen-Ausgaben bei async/await ausgefuehrt? Beispiel `console.log('a'); const p = new Promise(...); console.log('c');`::Synchron: `a`, `b` (Executor synchron), `c`. Dann setTimeout-Callback: `D`, `E`. Asynchroner Teil hinter `await` kommt erst nach `Hello`, `End` (synchroner Block) auf die Job-Queue.

Was ist Sequelize.js und welche zwei Nutzungen unterstuetzt es?::Third-Party-Modul fuer **Datenbankzugriffe**. Bietet **ORM** (Datenbankzugriff ohne SQL-Queries) und **Raw Queries** (auf Basis von Promises). Verbindung: `new Sequelize('db', 'user', 'pw', {host, dialect})` oder mit URI; Dialekte: mysql, mariadb, postgres, mssql, sqlite.

Welche Datenbanken unterstuetzt Sequelize?::MySQL, MariaDB, Oracle Database, SQLite, MS SQL, PostgreSQL, IBM Informix, MongoDB (NoSQL).

Was ist SQL Injection und wie laesst sie sich vermeiden?::Fehlende Validierung/Maskierung von Benutzereingaben fuehrt zu ungewollten DB-Statements (z.B. `?id=42;DROP+TABLE+user` oder `user=admin'+OR+1='1'--`). Vermeidung mit **Prepared Statements** — vorbereitete Anweisungen mit Platzhaltern, das DBMS uebernimmt das Escaping.

Wie werden Prepared Statements in Sequelize geschrieben?::Mit **`?`-Notation** (unnamed) oder **`:`-Notation** (named). Beispiele: `sequelize.query("SELECT * FROM user WHERE id = ?", { replacements: [req.params.id], type: QueryTypes.SELECT })` oder `WHERE id = :id, { replacements: { id: req.params.id } }`. Array-Replacements `IN(:ids)` mit `ids: [1,2,3,4]`. Auch `%`-Operator: `{ search_name: 'an%' }`.

Wie nutzt man Sequelize als ORM?::`const Note = sequelize.define('notes', { note: Sequelize.TEXT, tag: Sequelize.STRING })`, dann `Note.bulkCreate([...])`, `Note.findAll()`, `Note.findAll({ where: { id: req.params.id } })`. Liefert Promises zurueck (oder nutze `await`).

Welche Exception-Typen kennt JavaScript?::`Error` (Basis), `ReferenceError`, `TypeError`, `SyntaxError`, `RangeError`. Properties: `name`, `message`, `stack`. Erzeugung mit `new RangeError(message)`, Werfen mit `throw exception`.

Wie kann man verschiedene Exception-Typen handhaben?::Nur **ein** `catch`-Block pro `try` — Filterung mit `instanceof` im Body: `if (ex instanceof TypeError) {...} else if (ex instanceof ReferenceError) {...}`. Multiple catches sind kein Standard. Try-Bloecke werden oft **nicht von der JS-Engine optimiert** — keine durchsatzkritischen Operationen darin.

Wie funktioniert Vererbung in JavaScript? Was ist ein Prototyp?::**Klassenlose Vererbung** ueber **Prototypen**. Jedes Objekt hat ein privates `__proto__`-Attribut, das auf einen anderen Prototyp zeigt (Mutter aller Prototypen ist `Object` mit `__proto__ = null`). Vererbung wird ueber die Verlinkung der Prototypobjekte realisiert (**Prototype Chain**). Beim Zugriff auf ein Attribut wird die Chain hochgewandert, bis das Attribut gefunden ist oder undefined.

Was unterscheidet `prototype` von `__proto__`?::`Funktion.prototype` ist das Prototyp-Objekt, das bei `new Funktion(...)` als `__proto__` der erzeugten Objekte gesetzt wird. Es wird fuer **alle** Objekte gemeinsam genutzt und enthaelt geteilte Methoden und Attribute. `obj.__proto__ === Function.prototype` ist `true` fuer Objekte, die mit `new Function()` erzeugt wurden.

Wie nutzt man Prototypen fuer Vererbung?::Konstruktorfunktion: `function Fahrzeug(speed) { this.speed = speed; }`. Methoden am Prototyp: `Fahrzeug.prototype.go = function(times) { ... }`. Erben: `function Auto(speed, fabrikat) { Fahrzeug.call(this, speed); this.fabrikat = fabrikat; }` und dann `Auto.prototype = new Fahrzeug()` plus eigene Methoden anhaengen.

Was tut `Object.create(proto)`?::Erstellt ein Objekt mit `proto` als `__proto__`. `Object.create(null)` erzeugt ein Objekt ohne Prototyp. Beispiel: `let bar = Object.create(foo)` — bar erbt alle Eigenschaften von foo.

Welche Moeglichkeiten gibt es, ein Objekt zu erstellen?::1) `Object.create(null)` mit gezieltem Prototyp. 2) **JSON-like**: `{ foo: 1, bar: 2 }`. 3) Konstruktorfunktion: `new Object()`.

Was ist ECMAScript 7 (ES2016)?::Veroeffentlicht Juni 2016. Wenig Neues: **Exponentiation-Operator** `x ** y`, **Array.prototype.includes**, **async/await** als wesentliche Neuerung. Guter Browser-Support seit Stand 2023.

Was ist Babel?::Ein **JavaScript-Compiler / Transpiler**, der modernes JS (z.B. ES2015+) in alteres JavaScript transpiliert, damit auch aeltere Browser den Code ausfuehren koennen. Bei Angular wird standardmaessig der **TypeScript Compiler (tsc)** verwendet — Babel ist eine Alternative.

Welche Best Practices gelten bei der Nutzung von Node.js?::1) Event Loop nicht blockieren. 2) Fehler immer sauber behandeln. 3) Async-Code konsequent und einheitlich schreiben. 4) Streaming fuer grosse Datenmengen. 5) Projekt sauber strukturieren. 6) Skalierung und Ressourcenverbrauch frueh mitdenken.
