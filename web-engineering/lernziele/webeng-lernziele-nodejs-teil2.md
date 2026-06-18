---
course: web-engineering
type: lernziele
lecture: 10
date: 2026-05-12
tags:
  - web-engineering
  - lernziele
  - nodejs
  - mustache
  - cookies
  - sessions
  - validierung
  - argon2
  - websockets
  - flashcards/web-engineering
source: 10-nodejs-Teil2.pdf
---

# Lernziele — Node.js Teil 2

**Quelle:** [[web-engineering/resources/10-nodejs-Teil2.pdf|10-nodejs-Teil2.pdf]]
**Vorlesung:** [[web-engineering/lectures/06/webeng-10-nodejs-teil2|10 — Node.js Teil 2]]

## Fragen

Was ist das Ziel einer Template-Engine und wie funktioniert sie?::Saubere **Trennung von Darstellung und Inhalt** (HTML vs. Node-Code). Sie liest eine HTML-Vorlage mit Platzhaltern ein und ersetzt diese durch Attributwerte eines Datenobjekts. Mustache nutzt `{{firstName}}` und escaped HTML automatisch.

Wie wird Mustache mit Express integriert?::`app.engine('html', mustacheExpress())`, `app.set('view engine', 'mustache')`, `app.set('views', path.join(__dirname, 'views'))`. In Route-Handlern dann `res.render('tpl_datei_views.html', inhalt)` — gibt direkt das gerenderte HTML zurueck. Mit ESModulen muss `__dirname` ueber `fileURLToPath(import.meta.url)` selbst erzeugt werden.

Wozu dient das `consolidate`-Package?::Es bietet eine **Zwischenschicht** fuer verschiedene Template-Engines, damit alle dieselbe Express-Schnittstelle `__express(filePath, options, callback)` bereitstellen — `mustache-express` ist nur ein Beispiel. Mit consolidate koennen verschiedene Engines nahtlos genutzt werden.

Warum gehen Zustaende eines Node.js-Skripts nach Ausfuehrung verloren? Wie loest man das?::Variablen sind nur fuer einen einzigen Aufruf gueltig — HTTP ist zustandslos und Sessions sind auf OSI-Schicht 5 nicht vorhanden. Loesung: **Cookies** (clientseitig) und **Sessions** (serverseitig).

Wozu dienen Cookies und wie werden sie gesetzt?::Clientseitige Datenhaltung — der Nutzer muss sich nicht mehrfach anmelden, Tracking ist moeglich. Cookies werden im **HTTP-Header** zurueckgeliefert (`Set-Cookie: name=value`), das Setzen muss vor dem Senden des Inhalts geschehen. Der Client sendet das Cookie beim naechsten Aufruf der Seite (nur fuer den Setter).

Wie werden Cookies in Express gesetzt und gelesen?::Setzen mit `res.cookie('name', 'value', options)` — Optionen: `expires`, `path`, `domain`, `httpOnly`. Lesen mit Middleware **cookie-parser**: `app.use(cookieParser())` und dann `req.cookies.rememberme`. Achtung: Cookie kann **nicht im selben Skriptdurchlauf** geschrieben und gelesen werden.

Was unterscheidet Cookies von Sessions?::**Cookies** speichern alle Daten clientseitig — vom Benutzer manipulierbar. **Sessions** speichern Daten serverseitig, der Client erhaelt nur eine Session-ID (als Cookie). Sessions sind nicht manipulierbar und unterstuetzen komplexe Datenstrukturen (Arrays, Objekte).

Wie werden Sessions in Node.js genutzt?::Middleware `express-session`. Konfiguration mit `secret` (signiert das Cookie, Schutz vor Manipulation), `cookie.secure: true` (HTTPS-Pflicht), `cookie.domain` (alle Subdomains), `resave: false`, `saveUninitialized: false` (Privacy Compliance — verhindert leere Cookies bevor User zugestimmt hat). HttpOnly ist Default. Beenden: `req.session.destroy()`.

Wie unterscheiden sich GET- und POST-Requests in Express?::**GET**: Parameter in der URL nach `?`, in `req.query.id` verfuegbar. Nicht geeignet fuer grosse oder sensible Daten. **POST**: Parameter im HTTP-Body, in `req.body.text` (mit Middleware `express.urlencoded({extended: true})`). Daten nicht in der URL sichtbar.

Warum ist serverseitige Validierung von Nutzereingaben zwingend? Wie wird sie umgesetzt?::**"Never trust the user"** — Clientseitige Pruefungen koennen umgangen werden. Loesung: Modul **express-validator** mit `check('field').isEmail().isLength({max: 50})` als Middleware-Chain. Fehler werden mit `validationResult(req)` ausgewertet und z.B. mit HTTP 422 zurueckgegeben.

Was ist die Bereinigung (Sanitizing) von Eingaben?::Bereinigung der Eingabe-Strings (Whitespace trimmen, HTML escapen, normalizeEmail, toInt) — meist als Chain hinter `check()` aufgerufen: `check('name').trim().notEmpty().withMessage(...)` oder `sanitizeBody('user').escape().trim()`.

Wie sollten Passwoerter sicher gespeichert werden?::**Niemals im Klartext** — Datenbank koennte kompromittiert werden, Nutzer verwenden Passwoerter mehrfach. Stattdessen Hash-Werte mit **argon2**: `await argon2.hash('superPW123')` erzeugt automatisch einen zufaelligen Salt → gleiche Funktion liefert unterschiedliche Werte. Pruefen mit `argon2.verify(hash, data)`.

Welche Felder enthaelt ein argon2-Hash und wozu dienen sie?::`$argon2id$v=19$m=65536,t=3,p=4$<Salt>$<Hash>` — Algorithmus, Version, (Speicher m, Iterationen t, Threads p), Salt (Base64), Hash (Base64). Optionen ueber `{memoryCost: 4096, parallelism: 3, timeCost: 4}`.

Was ist der Unterschied zwischen argon2 und bcryptjs in Bezug auf Sync/Async?::**bcrypt.hash/compare** sind asynchron — sollten mit `await` in einer `async`-Funktion aufgerufen werden, um die Event Loop nicht zu blockieren. Synchrone Alternativen `bcrypt.hashSync/compareSync` existieren, sind aber zu vermeiden.

Was sind die Gefahren schaedlicher Module aus npm?::Jeder kann Module veroeffentlichen → Schadcode moeglich. **Typosquatting**: `socketio` statt `socket.io`. Auch bekannte Module koennen durch Updates verwundbar werden. Schutz: Versionen in `package.json` **fixieren**, regelmaessig `npm-audit` ausfuehren.

Was sind Websockets?::TCP-basiertes Protokoll fuer **bidirektionale** Verbindungen zwischen Browser und Server. **Nur ein** Verbindungsaufbau (per HTTP-Upgrade), die TCP-Verbindung bleibt offen. Im Gegensatz zu HTTP kann der Server **ohne vorherigen Request** Daten senden. Anwendung: Chats, Spiele, Server-Monitoring.

Wie laeuft der Websocket-Handshake ab?::Client sendet HTTP-GET mit `Upgrade: websocket`, `Connection: Upgrade`, `Sec-WebSocket-Key: <key>`, `Sec-WebSocket-Version: 13`. Server haengt eine **GUID** `258EAFA5-E914-47DA-95CA-C5AB0DC85B11` an den Key, bildet **SHA-1** + Base64 und sendet `Sec-WebSocket-Accept: <hash>` mit HTTP 101 Switching Protocols. Danach laufen Daten bidirektional ueber die offene TCP-Verbindung.

Was haben HTTP/2-Upgrade und Websocket-Upgrade gemeinsam?::Beide nutzen das **Upgrade**-Konzept einer HTTP/1.1-Verbindung. Bei HTTP/2 wurde der Mehraufwand durch **Application Layer Protocol Negotiation (ALPN)** im TLS-Handshake ersetzt — kein extra HTTP-Request mehr. Websockets nutzen weiterhin den klassischen 1.1-Upgrade-Mechanismus.

Welche API stellt ein Browser fuer Websockets bereit?::`new WebSocket("ws://localhost:8080/")` (oder `wss://` fuer HTTPS). Events: `socket.onopen`, `socket.onmessage = e => ...`, `socket.send(data)`.

Wie wird ein Websocket-Server in Node.js implementiert?::Modul `ws`: `new WebSocketServer({ noServer: true })`. Express liefert den HTTP-Server (`app.listen(8080)`); auf `server.on('upgrade', ...)` wird `wsServer.handleUpgrade(...)` aufgerufen, das `connection`-Event emittiert. Im Connection-Handler dann `socket.on('message', cb)` und `socket.send(...)`.
