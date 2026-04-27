---
course: web-engineering
type: lecture
lecture: 1
date: 2026-04-09
tags:
  - web-engineering
  - lecture
  - html
  - html5
  - dom
  - zeichensatz
  - utf-8
source: 02-Einfuehrung-HTML.pdf
---

# 02 — Einfuehrung HTML

**Folien:** [[web-engineering/resources/02-Einfuehrung-HTML.pdf|02-Einfuehrung-HTML.pdf]]
**Lernziele:** [[web-engineering/lernziele/webeng-lernziele-01|Lernziele Vorlesung 1]]


## Inhaltsverzeichnis

- [[#Was ist HTML?|Was ist HTML?]]
- [[#HTML Funktionsweise|HTML Funktionsweise]]
- [[#HTML Tags|HTML Tags]]
- [[#HTML Grundlegender Aufbau|HTML Grundlegender Aufbau]]
- [[#DOM-Baum (Document Object Model)|DOM-Baum (Document Object Model)]]
- [[#HTML Historie: HTML4 → XHTML → HTML5|HTML Historie: HTML4 → XHTML → HTML5]]
- [[#Don'ts in HTML (Altlasten)|Don'ts in HTML (Altlasten)]]
- [[#Zeichensatz|Zeichensatz]]
- [[#Der `<head>` Bereich|Der `<head>` Bereich]]
- [[#Bezug zu Lernzielen|Bezug zu Lernzielen]]

---

## Was ist HTML?

- **HTML** ist die "Formatierungssprache" des WWW
- Hypertext: Dokumente, die Verweise auf andere Dokumente enthalten (koennen)
- Universelle plattformunabhaengige Auszeichnungssprache
- Markup = Teile eines Textes werden als besondere strukturelle Elemente gekennzeichnet (Ueberschriften, Tabellen, ...)
- Auszeichnungselemente werden vorgegeben und mit einer feststehenden Semantik verknuepft (im Gegensatz zu XML)
- Seit 1992 standardisiert, **HTML5 seit 2014**

**Wie werden Web-Seiten aufgebaut?**
- **HTML:** Inhalt und Aufbau/Grundstruktur der Seite
- **CSS:** Design/Erscheinungsbild des Dokumentes
- **JavaScript:** Logik, Programmierung beim Verarbeiten des Dokumentes

```mermaid
graph LR
    HTML["HTML<br/><i>Inhalt & Struktur</i>"]
    CSS["CSS<br/><i>Design & Layout</i>"]
    JS["JavaScript<br/><i>Logik & Interaktion</i>"]

    HTML --- CSS
    HTML --- JS
    CSS -.- JS

    style HTML fill:#e67e22,color:#fff
    style CSS fill:#2980b9,color:#fff
    style JS fill:#27ae60,color:#fff
```

---

## HTML Funktionsweise

**Hierarchische Gliederung der Auszeichnungselemente:**
- Auszeichnungselemente haben einen festen Erstreckungsraum
- Sie haben eine Semantik und ggf. einen Inhalt
- Elemente koennen verschachtelt sein
- Verschachtelte Elemente muessen vollstaendig in den uebergeordneten Elementen enthalten sein
- Ergibt eine Repraesentationsmoeglichkeit als Baum (**DOM-Baum**)

## HTML Tags

- Tags werden in spitzen Klammern (`<`, `>`) notiert
- Die meisten Elemente haben ein einleitendes (`<tag>`) und ein abschliessendes (`</tag>`) Tag
- **Empty-Elements** haben keinen Inhalt und **keinen schliessenden Tag** (z.B. `<br>`, `<col>`, `<link>`, `<img>`)
- Elemente koennen verschachtelten Inhalt und weitere Auszeichnungselemente beinhalten
  - **Achtung:** Bei Verschachtelung spielt die Reihenfolge eine Rolle!
- Attribute werden mittels Key-Value-Paaren angegeben (`name="wert"`)
  - **Achtung:** Bei Attributen spielt die Reihenfolge **keine** Rolle

## HTML Grundlegender Aufbau

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Titel der Webseite</title>
</head>
<body>
    <!-- Eigentliches Dokument mit Auszeichnungselementen -->
</body>
</html>
```

- `<!DOCTYPE html>` — empfohlene Anweisung fuer den Browser (HTML5)
- `<meta charset="utf-8">` — Zeichensatz des Dokumentes
- `<html>` — umschliessendes Element, Wurzel des DOM-Baums

## DOM-Baum (Document Object Model)

- Hilfsmittel bei der Verarbeitung von HTML
- Auszeichnungselemente und Elementinhalte werden durch **Knoten** repraesentiert
- Text ist ein eigener DOM-Knoten (`#text`)
- Zugriff auf alle Tags und Attribute durch Navigation im Baum moeglich
- **Reihenfolge** der Elemente ist wichtig, Reihenfolge der Attribute nicht

## HTML Historie: HTML4 → XHTML → HTML5

**XML und HTML:**
- HTML urspruenglich unabhaengig von XML entwickelt
- Beide entstanden aus SGML (Standard Generalized Markup Language)
- XML erlaubt beliebige Tags, HTML hat vorgegebene Tags mit Semantik
- **XHTML:** Vereinigung von HTML und XML, Version 2.0 zu Gunsten von HTML5 eingestellt

**HTML 4 (1997/1999):** Teils unspezifisch, schwierig browseruebergreifend gleiches Layout zu erzeugen

**XHTML (~2000):** XML-kompatibler Standard, scheiterte wegen Striktheit (minimale Fehler → kompletter Ausfall)

**HTML5 (seit 2014):**
- Einheitlicher Parser verhindert Browserunterschiede und erlaubt Flexibilitaet
- Zahlreiche Erweiterungen (video, audio)
- Viele Altlasten entfernt
- Oft als **Sammelbegriff fuer moderne Web-Seiten** genutzt

## Don'ts in HTML (Altlasten)

- **Styling im HTML-Code** vermeiden — kein `<font>`, keine font-Tags → seit HTML4 mittels **CSS** machen!
- **Escaping von Umlauten** — bei richtigem Zeichensatz (UTF-8) **nicht noetig**
- **Vermengung von JavaScript, CSS und HTML** — in eigene Dateien trennen!
- Viele Internetseiten enthalten weiterhin schlechte Codebeispiele → im Zweifel **MDN** konsultieren

## Zeichensatz

- **Immer UTF-8 waehlen!** (97.7% aller Webseiten)
- UTF-8 ist abwaertskompatibel zu ASCII (erste 128 Zeichen identisch)

**UTF-8 nutzen:**
1. Editor auf UTF-8 stellen
2. Im HTML angeben: `<meta charset="utf-8">`
3. Sicherstellen, dass der Server das gleiche Encoding uebermittelt (`Content-Type: text/html;charset=utf-8`)
   - Bei widerspruechlichen Angaben nutzt der Browser das Server-Encoding!

**Zeichen die escaped werden muessen:**

| Zeichen | Bedeutung | HTML-Code |
|---------|-----------|-----------|
| `<` | Oeffnende Klammer | `&lt;` |
| `>` | Schliessende Klammer | `&gt;` |
| `&` | Ampersand | `&amp;` |
| `"` | Doppelte Anfuehrungszeichen | `&quot;` |
| `'` | Einfaches Anfuehrungszeichen | `&#39;` |

## Der `<head>` Bereich

Enthaelt Informationen ueber das Dokument (fuer Browser, Bots):
```html
<head>
    <meta charset="utf-8">
    <title>Titel der Webseite</title>
    <meta name="author" content="Autor der Seite">
    <meta name="description" content="Beschreibung der Webseite">
    <meta name="keywords" content="Schluesselworte,HTML,lernen">
    <link rel="stylesheet" href="style.css" type="text/css">
</head>
```

Hier werden auch wichtige Ressourcen wie **CSS** (Erscheinungsbild) und **JavaScript** (Logik) eingebunden.

---

## Bezug zu [[web-engineering/lernziele/webeng-lernziele-01|Lernzielen]]

**Lernziel 1 — Struktur von HTML5-Dokumenten:**
- Jedes HTML5-Dokument besteht aus `<!DOCTYPE html>`, `<html>`, `<head>` und `<body>`
- Im `<head>`: Metadaten, Titel, Zeichensatz (`<meta charset="utf-8">`), Einbindung von CSS/JS
- Im `<body>`: der eigentliche sichtbare Inhalt mit Auszeichnungselementen
- HTML5 ist seit 2014 Standard und ersetzt HTML4/XHTML

**Lernziel 3 — Einbinden von Script- und CSS-Elementen:**
- CSS wird per `<link rel="stylesheet" href="style.css">` im `<head>` eingebunden
- JavaScript wird per `<script src="script.js"></script>` eingebunden
- Grafiken per `<img src="katze.jpg" alt="Katze" width="100" height="100">`

**Lernziel 4 — Problematik physischer Auszeichnungselemente:**
- Physische Auszeichnungen wie `<font>`, `<b>` fuer Styling sind veraltet (deprecated seit HTML 4.01, obsolete in HTML5)
- Stattdessen semantische Elemente (`<strong>`) verwenden und Styling ueber CSS regeln

**Lernziel 5 — Do's und Dont's:**
- **Zeichensatz:** Immer UTF-8 verwenden, immer `<meta charset="utf-8">` im `<head>` angeben
- **Escaping:** Nur `<`, `>`, `&`, `"`, `'` muessen escaped werden — bei UTF-8 sind Umlaute direkt moeglich
- **Trennung:** HTML, CSS und JavaScript gehoeren in separate Dateien
- Kein Styling direkt in HTML, keine `<font>`-Tags, kein `bgcolor`
