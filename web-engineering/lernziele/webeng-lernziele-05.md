---
course: web-engineering
type: lernziele
lecture: 5
date: 2026-05-06
tags:
  - web-engineering
  - lernziele
  - nodejs
  - express
  - middleware
  - flashcards/web-engineering
source: 09-nodejs-Teil1.pdf
---

# Lernziele — Vorlesung 5

Was sollten Sie ueber node.js, Packages und package.json wissen?::Node.js fuehrt JavaScript serverseitig aus; externe Packages werden ueber npm verwaltet, und `package.json` beschreibt Metadaten, Einstiegspunkt und Abhaengigkeiten eines Projekts.

Was sollten Sie ueber Module in node.js wissen?::Sie koennen Kernmodule, lokale Module und Third-Party-Module verwenden und eigene Funktionalitaet per `exports`, `module.exports` oder modern per `import` und `export` bereitstellen.

Was sollten Sie ueber express und den nativen HTTP-Server in node.js wissen?::Der native HTTP-Server verarbeitet Requests direkt ueber das `http`-Modul, waehrend Express Routing und typische Webserver-Aufgaben deutlich komfortabler kapselt.

Was sollten Sie ueber das Middleware-Konzept wissen?::Middleware verarbeitet Requests in einer Kette, kann `req` und `res` bearbeiten und gibt mit `next()` weiter oder beendet die Antwort selbst.

Was sollten Sie ueber die Definition von Routen wissen?::Routen kombinieren HTTP-Methode und Pfad; ihre Reihenfolge ist relevant, und Router ermoeglichen eine modulare Gruppierung unter gemeinsamen URL-Prefixen.
