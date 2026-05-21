---
course: web-engineering
type: lecture
lecture: 11
date: 2026-05-18
tags:
  - web-engineering
  - lecture
  - javascript
  - promises
  - async-await
  - event-loop
  - microtask
  - sequelize
  - sql-injection
  - prepared-statements
  - prototypen
  - exceptions
  - babel
source: 11-JavaScript-Teil2.pdf
---

# 11 — JavaScript Teil 2

**Folien:** [[web-engineering/resources/11-JavaScript-Teil2.pdf|11-JavaScript-Teil2.pdf]]
**Lernziele:** [[web-engineering/lernziele/webeng-lernziele-11|Lernziele Vorlesung 11]]

## Inhaltsverzeichnis

- [[#Asynchronitaet als Basis|Asynchronitaet als Basis]]
- [[#Promises|Promises]]
- [[#Promise-Chaining|Promise-Chaining]]
- [[#async / await|async / await]]
- [[#Event Loop: Job- vs. Task-Queue|Event Loop: Job- vs. Task-Queue]]
- [[#Datenbankzugriff mit Sequelize.js|Datenbankzugriff mit Sequelize.js]]
- [[#SQL Injection und Prepared Statements|SQL Injection und Prepared Statements]]
- [[#JavaScript Exceptions|JavaScript Exceptions]]
- [[#Vererbung in JavaScript: Prototypen|Vererbung in JavaScript: Prototypen]]
- [[#Object.create|Object.create]]
- [[#ECMAScript 7 und Babel|ECMAScript 7 und Babel]]
- [[#Best Practices fuer Node.js|Best Practices fuer Node.js]]
- [[#Bezug zu Lernzielen|Bezug zu Lernzielen]]

---

## Asynchronitaet als Basis

```js
// synchron — blockiert
const fs = require('fs');
const data = fs.readFileSync('/file.md');
// blocks here until file is read
```

> [!warning] Achtung
> JavaScript ist **single-threaded**. Skripte mit **blockierender I/O** sind nicht performant! Die meisten Standardfunktionen erwarten daher **Callbacks**, die das Ergebnis verarbeiten (kein `return`-Wert).

```js
// asynchron
const fs = require('fs');
fs.readFile('/file.md', function (err, data) {
  if (err) throw err;
});
```

### Callback Hell

> [!quote] Definition
> "Anything that needs to happen after a callback has fired needs to be invoked from within it."

```js
chooseToppings(function (toppings) {
  placeOrder(toppings, function (order) {
    collectOrder(order, function (pizza) {
      eatPizza(pizza);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```

Schlecht lesbar, Programmablauf unklar → **Promises** als Ausweg.

---

## Promises

> [!quote] Definition (Promise)
> Das **Promise**-Interface repraesentiert einen **Platzhalter** (Proxy) fuer ein zukuenftiges Resultat (vgl. Futures in Java).

### Drei Zustaende

```mermaid
stateDiagram-v2
    [*] --> pending
    pending --> fulfilled : resolve(value)
    pending --> rejected : reject(error)
```

- **Pending** (Startzustand): `result: undefined`
- **Fulfilled**: `result: value`
- **Rejected**: `result: error`

### Erzeugung

```js
new Promise(executor);
new Promise(function (resolve, reject) { ... });

let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done"), 1000);
});
```

> [!info] Hinweis
> Der **Executor wird direkt synchron** ausgefuehrt. `resolve` / `reject` werden je nach Verwendung direkt oder spaeter ueber die Event Queue aufgerufen. Diese Aufrufe legen die **Handler-Ausfuehrung auf die Microtask Queue** — die Handler laufen erst spaeter, nachdem der aktuelle Call-Stack fertig ist.

### Beispiel und Abarbeitungsfolge

```js
const promise1 = new Promise((resolve, reject) => {
  console.log("Los geht es");
  setTimeout(() => resolve('Sieht aus wie mitten drin'), 0);
});
console.log("Zweiter");
promise1.then(value => console.log(value));
console.log("Man meint ich kaeme zum Schluss");
```

**Output**:
```
Los geht es
Zweiter
Man meint ich kaeme zum Schluss
Sieht aus wie mitten drin
```

```mermaid
flowchart TD
    A["0. Skript (sync)<br/>new Promise → setTimeout<br/>p.then(console.log)"]
    A --> B["Ablegen des resolve<br/>auf die Task-Queue"]
    A --> C["Ablegen des console.log<br/>auf die Job-Queue"]
    B --> D["Task-Queue (macrotask)<br/>setTimeout → resolve('Hallo')<br/>(resolve legt Microtasks an)"]
    D --> E["Job-Queue (microtask)<br/>console.log('Hallo')"]
    E --> F[Weiter mit naechstem Task...]
```

### Zugriff auf das Resultat: then / catch

```js
let promise = new Promise(resolve => setTimeout(() => resolve("done!"), 1000));
promise.then(alert); // zeigt "done!" nach 1 Sekunde

let promise2 = new Promise((resolve, reject) =>
  setTimeout(() => reject(new Error("Whoops!")), 1000)
);
promise2.catch(alert); // gleichwertig zu promise.then(null, alert)
```

> [!tip] Merke
> - Ohne `then()` wird der Wert von `resolve` nie verarbeitet.
> - **Chaining**: `then` und `catch` liefern neue Promise-Objekte zurueck.
> - Ein `catch()` reicht fuer alle Rejections in der Kette.

### Vom Callback zu Promise

```js
function readFilePromisified(filePath) {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, 'utf8', (err, data) => {
      if (err) reject(err);
      else resolve(data);
    });
  });
}

readFilePromisified('bla.txt')
  .then(data => console.log('CONTENT:', data))
  .catch(err => console.log('ERROR:', err));
```

---

## Promise-Chaining

> [!tip] Merke
> Sowohl `then` als auch `catch` geben **neue Promise-Objekte zurueck** → Chaining moeglich.

```mermaid
flowchart LR
    P1[Promise] -->|resolve| T1[then onFulfilled]
    P1 -->|reject| C1[catch onRejected]
    T1 -->|return data| P2[Promise]
    P2 --> T2[then onFulfilled]
    C1 -->|throw error| P2
```

### Wichtig: `return` im then-Callback

```js
// FALSCH: data nicht durchgereicht
readFilePromisified("bla.txt")
  .then(data => { console.log('CONTENT:', data) })
  .then(data => { console.log("2nd then", data); /* undefined */ });

// RICHTIG: return data
readFilePromisified("bla.txt")
  .then(data => { console.log('CONTENT:', data); return data; })
  .then(data => { console.log("2nd then", data); /* file contents */ });
```

> [!warning] Achtung
> Ein `return` in einem `then`-Callback fuehrt zu einem `Promise.resolve` auf den Return-Wert. **`console.log` ist kein Promise** — ohne explizites `return data` ist die naechste Stufe `undefined`.

### Callback-Hell vs. Promise

```js
// Vorher
chooseToppings(function (toppings) {
  placeOrder(toppings, function (order) {
    collectOrder(order, function (pizza) {
      eatPizza(pizza);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);

// Nachher
chooseToppings()
  .then(toppings => placeOrder(toppings))
  .then(order => collectOrder(order))
  .then(pizza => eatPizza(pizza))
  .catch(failureCallback);
```

---

## async / await

> [!quote] Definition
> `async` als Schluesselwort **vor `function`** zeigt an, dass es asynchrone Abschnitte gibt. Eine `async`-Funktion **liefert immer ein Promise**:
> - `return x` entspricht `return Promise.resolve(x)`
> - `throw e` entspricht `return Promise.reject(e)`
> 
> **Innerhalb** einer async-Funktion kann `await` genutzt werden, um auf ein Promise zu "warten".

### Syntax

```js
async function read1st() {
  data = await readFilePromisified("bla.txt");
  console.log("1st then ", data);
  return data; // → Promise.resolve(data)
}

async function read2nd() {
  data = await read1st();
  console.log("2nd then ", data);
}

read2nd();
```

### catch via try/catch

```js
async function read1st() {
  try {
    data = await readFilePromisified("bla.txt");
    console.log("1st then ", data);
    return data;
  } catch (e) {
    throw e; // → Promise.reject(e)
  }
}
```

### Wo darf await stehen?

- Nur in `async`-Funktionen, sonst SyntaxError
- Ab **Node 14.8** gibt es **Top-Level await** im **ES-Modul-Modus**
- Auch in IIFE: `(async () => { await ... })()`

> [!tip] Merke
> - `async`-Funktion liefert **immer** ein Promise.
> - `await` ist **nicht blockierend**: es wertet den Promise aus und liefert den Code **danach** als **Microtask** auf die Job-Queue. Der Thread blockiert nicht.
> - `await` ist syntaktischer Zucker fuer Promise-Chaining mit `then()`.

### Achtung: await sequentialisiert!

```js
// SCHLECHT — sequentiell
async function doThings() {
  const thing1 = await asyncThing1();
  console.log(thing1);
  const thing2 = await asyncThing2();
  console.log(thing2);
}
```

```js
// BESSER — parallel mit Promise.all
async function doThings() {
  const p1 = asyncThing1();
  const p2 = asyncThing2();
  const [thing1, thing2] = await Promise.all([p1, p2]);
  console.log(thing1, thing2);
}
```

> [!warning] Achtung
> Async-Funktionen sollten trotzdem **schnell** sein. Aufeinanderfolgende `await`-Statements blockieren den Programmfluss kuenstlich, wenn die Operationen voneinander unabhaengig sind → `Promise.all` parallelisiert.

### await — Ablauf am Beispiel

```js
async function example() {
  console.log('Start');
  const result = await Promise.resolve('Hello');
  console.log(result);
  console.log('End');
}

example();
console.log('Ich komme vor dem Teil ab await');
```

**Output**:
```
Start
Ich komme vor dem Teil ab await
Hello
End
```

```mermaid
flowchart TD
    A[Call-Stack: example calls foo] --> B[Promise.resolve fulfilled]
    B --> C["await → *Microtask*<br/>(continue bar)"]
    C --> D["Call-Stack: console.log<br/>('Volker, ') noch synchron"]
    D --> E["Call-Stack leer →<br/>Job-Queue abarbeiten"]
    E --> F["Microtask:<br/>result = 1; console.log(1)"]
```

---

## Event Loop: Job- vs. Task-Queue

```mermaid
flowchart TB
    CS[Call Stack]
    WA[Web APIs<br/>resolve / timeout]
    JQ[Job Queue<br/>microtask]
    TQ[Task Queue<br/>macrotask]
    CS -->|Schedule| WA
    WA -->|Enqueue| JQ
    WA -->|Enqueue| TQ
    EL[Event Loop] -->|Dequeue| CS
    JQ --> EL
    TQ --> EL
```

> [!tip] Merke
> - `setTimeout`-Callbacks landen auf der **Task Queue (macrotask)**.
> - `Promise.then`-Callbacks landen auf der **Job Queue (microtask)**.
> - Nach jedem Call-Stack-Durchlauf wird die **Job Queue komplett** abgearbeitet, bevor die naechste macrotask gezogen wird.
> - **Promises sind also schneller als Timeouts** — selbst bei `setTimeout(..., 0)`.

### Beispiel

```js
setTimeout(function timeout() { console.log('Timed out'); }, 0);
Promise.resolve(1).then(function resolve() { console.log('Resolved'); });
// Output: Resolved \n Timed out
```

```js
async function foo() {
  setTimeout(() => console.log('Timed out'), 0);
  await Promise.resolve().then(() => console.log('Resolved'));
  console.log('Wird ausgefuehrt');
}

foo();
console.log('Hello');
// Output: Hello \n Resolved \n Wird ausgefuehrt \n Timed out
```

---

## Datenbankzugriff mit Sequelize.js

> [!warning] Achtung
> **Datenhaltung mittels Dateien nicht empfehlenswert** — Datenbanken nutzen.

Mit Node.js nutzbar: (Oracle) MySQL, MariaDB, Oracle Database, SQLite, MS SQL, PostgreSQL, IBM Informix, MongoDB (NoSQL). Im Kurs: **SQLite**.

### Sequelize.js

Third-Party-Node.js-Modul fuer Datenbankzugriffe:
- **ORM** (Object-Relational Mapping) — kein SQL noetig
- **Raw Queries** (auf Basis von Promises)

```js
const Sequelize = require('sequelize');

// Option 1: Parameter
const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: /* 'mysql' | 'mariadb' | 'postgres' | 'mssql' */
});

// Option 2: URI
const sequelize2 = new Sequelize('postgres://user:pass@example.com:5432/dbname');

// SQLite
const sequelize3 = new Sequelize({
  dialect: 'sqlite',
  storage: 'path/to/database.sqlite'
});
```

`new Sequelize(...)` liefert ein **Promise**. Verbindung pruefen:

```js
sequelize.authenticate()
  .then(() => console.log('Connection has been established successfully.'))
  .catch(err => console.error('Unable to connect to the database:', err));
```

### Raw Queries (mit await)

```js
const results = await sequelize.query(
  "SELECT * FROM user WHERE id = ?",
  { replacements: [req.params.id], type: sequelize.QueryTypes.SELECT }
);
console.log(results);

const [results2, metadata] = await sequelize.query("...");
// metadata gibt bei einigen DB-Systemen zusaetzliche Informationen
```

### ORM-Beispiel

```js
const Note = sequelize.define('notes', {
  note: Sequelize.TEXT,
  tag: Sequelize.STRING
});

sequelize.sync({ force: true })
  .then(() => {
    console.log('Database & tables created!');
    return Note.bulkCreate([
      { note: 'after work party', tag: 'fun' },
      { note: 'Scrum Meeting', tag: 'work' }
    ]);
  })
  .then(() => Note.findAll())
  .then(notes => console.log(notes));

app.get('/notes', (req, res) => {
  Note.findAll().then(notes => res.json(notes));
});

app.get('/notes/:id', (req, res) => {
  Note.findAll({ where: { id: req.params.id } }).then(notes => res.json(notes));
});
```

---

## SQL Injection und Prepared Statements

### Was ist SQL Injection?

> [!warning] Achtung — SQL Injection
> Fehlende Validierung/Maskierung von Benutzereingaben fuehrt zu ungewollten Datenbankstatements.

| | Erwarteter Aufruf | Angriff |
|---|---|---|
| Aufruf | `http://example.com/page?id=42` | `http://example.com/page?id=42;DROP+TABLE+user` |
| Erzeugtes SQL | `SELECT titel, text FROM artikel WHERE ID=42;` | `SELECT titel, text FROM artikel WHERE ID=42;DROP TABLE user;` |

Anderes Beispiel:
| | Erwartet | Angriff |
|---|---|---|
| Aufruf | `user=admin&pw=honigkuchen` | `user=admin'+OR+1='1'--&pw=asdf` |
| Erzeugtes SQL | `SELECT id FROM user WHERE name='admin' AND pw='...'` | `SELECT id FROM user WHERE name='admin' OR 1='1' --AND pw='...'` |

→ Login als beliebiger User ohne Passwort.

### Prepared Statements

> [!success] Best Practice
> **Prepared Statements**:
> - Vorbereitete Anweisung wird an die Datenbank gesendet (Platzhalter fuer Variablen)
> - Ausfuehrung des Statements nach Einsetzen der Variablen — DB-System kuemmert sich um Escaping
> - Verhindert SQL Injections und prueft die Gueltigkeit der Parameter
> - Geschwindigkeitsvorteil bei mehrfacher Ausfuehrung

#### Replacements: ?-Notation (unnamed)

```js
sequelize.query("SELECT * FROM user WHERE id = ?", {
  replacements: [req.params.id],
  type: sequelize.QueryTypes.SELECT
}).then(userinfos => {
  if (userinfos.length !== 0) console.log(userinfos[0].user);
  else console.log("no users");
});
```

#### Replacements: :-Notation (named)

```js
sequelize.query("SELECT * FROM user WHERE id = :id", {
  replacements: { id: req.params.id },
  type: sequelize.QueryTypes.SELECT
}).then(...);
```

#### Array-Replacements und %-Operator

```js
sequelize.query("SELECT * FROM user WHERE id IN(:ids)", {
  replacements: { ids: [1, 2, 3, 4] },
  type: sequelize.QueryTypes.SELECT
});

// LIKE
sequelize.query("SELECT * FROM user WHERE name LIKE :search_name", {
  replacements: { search_name: 'an%' } // beginnt mit 'an'
});
```

---

## JavaScript Exceptions

### Erzeugung und Werfen

```js
const rangeException = new RangeError('out of range');
throw rangeException;
```

### Gaengige Typen

| Typ | Bedeutung |
|---|---|
| `Error` | genereller Fehler (Basis-Klasse) |
| `ReferenceError` | nicht valide Referenz |
| `TypeError` | nicht valider Typ |
| `SyntaxError` | falsche Syntax von Quellcode |
| `RangeError` | Parameter ausserhalb valider Grenzen |

Properties der Error-Klasse: `name`, `message`, `stack`.

### Exception Handling

```js
try {
  // code that can throw an exception
} catch (ex) {
  // ex holds the exception info
}
// Programm laeuft hier weiter
```

> [!warning] Achtung
> Try-Bloecke werden oft **nicht** von der JS-Engine optimiert. Keine durchsatzkritischen Operationen in try-Bloecken.

### Mehrere Exception-Typen

> [!tip] Merke
> Jedes try-catch kann **nur einen** catch-Block enthalten. Filterung der Exception-Typen geschieht im Body:

```js
try {
  // ...
} catch (ex) {
  if (ex instanceof TypeError) { /* ... */ }
  else if (ex instanceof ReferenceError) { /* ... */ }
  else if (ex instanceof SyntaxError) { /* ... */ }
}
```

ECMAScript erlaubt **nur diese Syntax** — multiple Catches sind nicht Teil des Standards.

---

## Vererbung in JavaScript: Prototypen

### const kann modifiziert werden

```js
const vs = { name: "Volker Sander", beruf: "Professor" };
vs.name = "V. Sander"; // erlaubt! const verhindert nur Reassignment der Variable
console.log(vs);
```

### Klassenlose Vererbung

> [!quote] Definition
> Vererbung in JavaScript ist **klassenlos**: Objekte erben von Objekten ueber **Prototypen**.

- Jedes Objekt (und damit jede Funktion) besitzt ein privates Attribut `__proto__`, das auf das **Prototyp-Objekt** zeigt
- Das Prototypobjekt hat selbst auch ein `__proto__` — bis hin zu `Object` (dort ist `__proto__ = null`)
- Ein Prototypobjekt kann **Attribute und Methoden** haben
- Vererbung wird ueber die Verlinkung der Prototypen realisiert — die **Prototype Chain**

```mermaid
flowchart LR
    A[obj] -->|__proto__| B[Function.prototype]
    B -->|__proto__| C[Object.prototype]
    C -->|__proto__| D[null]
```

### prototype vs. __proto__

> [!tip] Merke
> - **`[Funktion].prototype`** — magisches Attribut, das jede Funktion besitzt. Wird genutzt, um das interne `__proto__` aller mit dieser Funktion (via `new`) erzeugten Objekte zu setzen.
> - **`[Objekt].__proto__`** — internes magisches Attribut, das jedes Objekt besitzt. Referenziert die `prototype`-Eigenschaft der Konstruktorfunktion.

```js
function f() {}
f.prototype.b = 123;

const obj = new f();
console.log(obj.b);                       // 123
console.log(obj.__proto__ === f.prototype); // true
console.log(f.__proto__ === Function.prototype); // true
console.log(obj.__proto__.b);              // 123
```

### Zugriff auf ein Attribut — Ablauf

`alert(obj.a)`:
1. Besitzt `obj` ein Attribut `a`? Falls ja → nehmen.
2. Sonst: Besitzt `obj.__proto__` ein Attribut `a`?
3. Sonst: Besitzt `obj.__proto__.__proto__` ein Attribut `a`?
4. … bis gefunden oder `null` erreicht (dann `undefined`).

### Beispiel — Suchhierarchie

```js
let f = function () {
  this.a = 1;
  this.b = 2;
};
let o = new f();             // {a: 1, b: 2}
f.prototype.b = 3;
f.prototype.c = 4;
console.log(o.a);            // 1
console.log(o.b);            // 2 — Eigene Eigenschaft hat Vorrang!
console.log(o.__proto__.b);  // 3
console.log(o.c);            // 4
console.log(o.d);            // undefined
f.e = 5;                     // Attribut der Funktion f, nicht des Objekts
console.log(f.e);            // 5
console.log(o.e);            // undefined
```

### Konstruktor mit Methoden am Prototyp

```js
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function () {
  console.log(`Hello, my name is ${this.name}`);
};

const john = new Person('John');
john.greet();                // Hello, my name is John

Person.prototype.age = 30;
const jane = new Person('Jane');
jane.greet();                // Hello, my name is Jane
console.log(jane.age);       // 30
console.log(john.age);       // 30 — geerbt!
```

### Vererbung in Aktion

```js
function Fahrzeug(speed) {
  this.speed = speed;
  this.distance = 0;
}
Fahrzeug.prototype.go = function (times) {
  this.distance += this.speed * times;
};

function Auto(speed, fabrikat) {
  Fahrzeug.call(this, speed); // ruft "Konstruktor" auf
  this.fabrikat = fabrikat;
}
Auto.prototype = new Fahrzeug();        // Prototyp-Chain herstellen
Auto.prototype.hupen = function () {
  console.log('Mooop!');
};

const auto = new Auto(150, 'Opel');
auto.hupen();      // Mooop!
auto.go(1);
console.log(auto.distance); // 150
```

---

## Object.create

> [!quote] Definition
> `Object.create(proto, [, propertiesObject])` erstellt ein Objekt mit `proto` als `__proto__`. `null` als `proto` erzeugt ein Objekt **ohne Prototyp**.

```js
const foo = { eins: 1, zwei: 2, pi: 3.14159 };
let bar = Object.create(foo); // Objekt mit foo als Prototyp
bar.drei = 3;
console.log(bar.eins);   // 1 (vom Prototyp)
console.log(bar.zwei);   // 2 (vom Prototyp)
console.log(bar.drei);   // 3
console.log(bar.pi);     // 3.14159

bar.pi = 4;
console.log(bar.pi);     // 4 — Eigenschaft vom Prototyp ueberdeckt
```

### Chained Prototypes

```js
var obj1 = { a: 1 };
var obj2 = Object.create(obj1); obj2.b = 2;
var obj3 = Object.create(obj2); obj3.c = 3;

obj1; // { a: 1 }
obj2; // { a: 1, b: 2 } (a geerbt)
obj3; // { a: 1, b: 2, c: 3 } (a, b geerbt)
```

### Objekt erstellen — Uebersicht

```js
// 1) Object.create
var obj = Object.create(null);
obj.foo = 1; obj.bar = 2;

// 2) JSON-like Notation
var obj2 = { foo: 1, bar: 2 };

// 3) Konstruktorfunktion
var obj3 = new Object();
obj3.foo = 1; obj3.bar = 2;
```

---

## ECMAScript 7 und Babel

### ECMAScript 7 (ES2016, Juni 2016)

- Math.pow-Kurzschreibweise: `x ** y`
- `Array.prototype.includes`
- **Async / await** als wesentliche Neuerung

Guter Browser-Support (Stand April 2023).

### Babel — JavaScript-Transpiler

```mermaid
flowchart LR
    A["(x) ⇒ x + 1<br/>(ES2015)"] --> T[Babel Transpiler]
    T --> B["var adding = function (x) {<br/>  return x + 1;<br/>}<br/>(altes JS)"]
```

> [!info] Hinweis
> **Babel** ist ein JavaScript-Compiler, der modernes JS in alteres JavaScript transpiliert, damit auch aeltere Browser den Code ausfuehren koennen. **Bei Angular wird standardmaessig der TypeScript Compiler (`tsc`) verwendet** — es geht aber auch Babel.

---

## Best Practices fuer Node.js

> [!success] Best Practice
> Bei der Nutzung von Node.js sollten Sie:
> - **Event Loop nicht blockieren**
> - **Fehler immer sauber behandeln**
> - **Async-Code konsequent und einheitlich schreiben**
> - **Streaming** fuer grosse Datenmengen nutzen
> - **Projekt sauber strukturieren**
> - **Skalierung und Ressourcenverbrauch frueh mitdenken**

---

## Bezug zu Lernzielen

Die kompakten Karteikarten finden sich unter [[web-engineering/lernziele/webeng-lernziele-11|Lernziele Vorlesung 11]]. Im Folgenden ausfuehrliche Antworten zur Pruefungsvorbereitung.

**Was ist Asynchronitaet in JavaScript und warum gibt es sie?**

JS ist **single-threaded** — blockierende I/O wuerde die Event Loop blockieren. Daher arbeiten die Standardfunktionen mit Callbacks oder Promises, die das Ergebnis verarbeiten. Klassisches Beispiel `fs.readFile(path, (err, data) => ...)`. Tiefe Verschachtelung fuehrt zur "**Callback Hell**" — Promises sind der Ausweg.

**Was ist ein Promise und wie funktionieren `then` und `catch`?**

Ein Promise ist ein **Stellvertreter fuer ein zukuenftiges Resultat** mit drei Zustaenden: pending, fulfilled, rejected. Der Executor `(resolve, reject) => {...}` wird direkt synchron ausgefuehrt; `resolve(value)` oder `reject(reason)` legen Handler in die **Microtask Queue**. `then(onFulfilled)` registriert einen Erfolgs-Handler, `catch(onRejected)` einen Fehler-Handler. Beide liefern wieder ein Promise → **Chaining**. Ein einziger `catch()` reicht fuer alle Rejections in der Kette.

**Wie funktioniert async/await?**

- `async`-Funktion liefert **immer** ein Promise. `return x` → `Promise.resolve(x)`, `throw e` → `Promise.reject(e)`.
- `await` darf nur in `async`-Funktionen genutzt werden (Ausnahme: Top-Level await in ES-Modulen ab Node 14.8). Es ist **nicht blockierend** — der Code dahinter wird als Microtask in die Job-Queue gelegt.
- Try/catch ersetzt den `.catch()`-Handler.

**Warum sequentialisiert `await` und wie parallelisiert man?**

Mehrere `await`-Statements in Folge laufen sequentiell. `Promise.all([p1, p2, ...])` startet die Operationen parallel und wartet gemeinsam auf alle:
```js
const [a, b] = await Promise.all([asyncThing1(), asyncThing2()]);
```

**Wieso sind Promises schneller als Timeouts?**

`Promise.then`-Callbacks landen in der **Job Queue (microtask)**, `setTimeout`-Callbacks in der **Task Queue (macrotask)**. Die Job Queue wird **vollstaendig** nach jedem Call-Stack-Durchlauf abgearbeitet — vor der naechsten macrotask. Daher gibt `Promise.resolve().then(...)` **vor** `setTimeout(..., 0)` aus.

**Was leistet Sequelize.js?**

Sequelize ist ein Third-Party-Modul fuer Datenbankzugriffe mit zwei Modi:
- **ORM**: `sequelize.define(...)` + `Note.findAll()`, `Note.bulkCreate(...)` — kein SQL noetig.
- **Raw Queries** auf Basis von Promises (oder await): `sequelize.query(...)` mit `replacements` und `type`.

Unterstuetzte DBs: MySQL, MariaDB, Oracle, SQLite, MS SQL, PostgreSQL, IBM Informix, MongoDB.

**Wie verhindert man SQL Injection?**

Mit **Prepared Statements** — Platzhaltern, die das DBMS escaped:
- `?`-Notation (unnamed): `WHERE id = ?` mit `replacements: [id]`
- `:`-Notation (named): `WHERE id = :id` mit `replacements: { id }`
- Array fuer `IN(:ids)`, `%`-Operator fuer LIKE-Patterns

Ohne Prepared Statements droht z.B. `?id=42;DROP+TABLE+user` oder `user=admin'+OR+1='1'--`.

**Wie funktioniert Exception-Handling in JavaScript?**

Exceptions werden ueber `throw exception` geworfen und mit `try { ... } catch (ex) { ... }` gefangen. Standardtypen: `Error`, `ReferenceError`, `TypeError`, `SyntaxError`, `RangeError`. Properties: `name`, `message`, `stack`. ECMAScript erlaubt **nur einen catch-Block** pro try — Filterung mittels `instanceof` im Body.

**Wie funktioniert Vererbung ueber Prototypen?**

JavaScript ist **klassenlos**. Jede Funktion hat ein `prototype`-Attribut; mit `new Funktion()` erzeugte Objekte erhalten dieses als `__proto__`. Beim Attribut-Zugriff wandert die Engine die Prototype Chain hoch: erst das Objekt selbst, dann `__proto__`, dann `__proto__.__proto__` … bis null. Praktisch:
```js
function Person(name) { this.name = name; }
Person.prototype.greet = function() { ... };
const john = new Person('John'); // __proto__ === Person.prototype
```

**Wozu dient `Object.create`?**

`Object.create(proto)` erzeugt ein Objekt mit `proto` als `__proto__`. Mit `Object.create(null)` ohne Prototyp. Praktische Variante, um Vererbung zu nutzen, ohne `new` mit Konstruktorfunktion zu schreiben.

**Was ist ECMAScript 7 und was ist Babel?**

ES2016 (Juni 2016) brachte den Exponentation-Operator `**`, `Array.prototype.includes` und **async/await**. **Babel** ist ein JavaScript-Compiler/Transpiler, der modernes JS in alteres JavaScript uebersetzt — wichtig fuer alte Browser. Angular nutzt standardmaessig den TypeScript Compiler (`tsc`).

**Welche Best Practices gelten in Node.js?**

Event Loop nicht blockieren, Fehler sauber behandeln, Async-Code konsequent schreiben, Streaming fuer grosse Datenmengen, Projekt sauber strukturieren, Skalierung und Ressourcenverbrauch frueh mitdenken.
