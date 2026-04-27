---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 2
date: 2026-04-09
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 2

1. Was abstrahiert aus Programmiersicht ein Socket?::Ein Socket abstrahiert den Zugang zum Transportprotokoll (TCP/UDP), der vom Betriebssystem bereitgestellt wird. Man uebergibt Adressinformationen und erhaelt einen logischen Kommunikationskanal mit Input-/Output-Streams.

2. Wer kommuniziert letztendlich miteinander?::Letztendlich kommunizieren **Anwendungsprozesse** (Client und Server) miteinander. Dazu muessen: Socket erstellt, Adressinformationen (IP + Port) angegeben, und die Daten als Bytestream ueber die Streams des Sockets gesendet/empfangen werden.

3. Wieso werden 3 Arten von Adressen benoetigt?::**MAC-Adresse** (Schicht 2): Identifiziert das Geraet im lokalen Netz. **IP-Adresse** (Schicht 3): Identifiziert den Rechner im Internet (netzuebergreifend). **Port-Nummer** (Schicht 4): Identifiziert die Anwendung auf dem Rechner. Jede Schicht benoetigt ihre eigene Adressierung.

4. Warum unterschiedliche Konstruktorparameter bei Client/Server-Sockets?::Der **Client-Socket** benoetigt Ziel-IP und Ziel-Port (2 Parameter), da er die Verbindung initiiert. Der **ServerSocket** benoetigt nur den eigenen Port (1 Parameter), da er passiv wartet — die IP-Adresse ist die eigene/Standardadresse.

5. Warum zwei verschiedene Socket-Klassen auf Server-Seite?::`ServerSocket` dient zum Warten auf Verbindungsanfragen (passive open auf einem Port). `accept()` gibt eine `Socket`-Instanz zurueck, die den konkreten Kommunikationskanal zu einem einzelnen Client repraesentiert. So kann der Server auf dem gleichen Port weitere Clients annehmen.

6. Was bewirkt accept()?::`accept()` **blockiert** den aufrufenden Thread, bis ein Client eine Verbindungsanfrage stellt. Nach Aufbau der logischen Verbindung liefert es eine **neue Socket-Instanz** zurueck, ueber die mit dem Client kommuniziert werden kann.

7. Was ist bei blockierenden Leseoperationen zu beachten?::Blockierende Operationen verhindern, dass der Server andere Clients bedienen kann. Loesung: **Threads** verwenden — nach jedem `accept()` wird ein neuer Thread gestartet, der die Client-Kommunikation uebernimmt, waehrend der Hauptthread weiter auf neue Verbindungen wartet.

8. Warum nicht einfach Port 500?::Ports 1-1023 sind **Well-Known-Ports** und erfordern **Administrator-Privilegien**. Ohne Root-/Admin-Rechte kann man keinen Server auf diesen Ports starten. Eigene Anwendungen sollten Ports > 1023 verwenden.

9. Was ist die Idee von localhost?::localhost (127.0.0.1 / ::1) ist eine **Loopback-Adresse** — eine virtuelle Netzwerkschnittstelle ohne physische Netzwerkkarte. Programme und Dienste koennen darueber **innerhalb des lokalen Rechners** miteinander kommunizieren, ohne das physische Netzwerk zu nutzen.

10. Little-Endian vs. Big-Endian?::**Little-Endian** (Intel): Niedrigstwertiges Byte an niedrigster Speicheradresse. **Big-Endian** (SPARC): Hoechstwertiges Byte zuerst. Problem bei Datenkommunikation: Integer-Werte werden verdreht (aus 5 wird 83886080), Zeichenketten hingegen nicht. Loesung: Einigung auf eine Transfersyntax (z.B. Network Byte Order = Big-Endian) oder textbasierte Uebertragung.

11. Abstrakte Syntax und Transfersyntax?::**Abstrakte Syntax:** Beschreibung der Datentypen unabhaengig von Plattform/Sprache (z.B. ASN.1-Definition, XML-Schema, Java-Klasse). Definiert WAS uebertragen wird. **Transfersyntax:** Die konkrete Kodierung fuer die Uebertragung (z.B. BER, XML-Dokument, Byte-Stream). Definiert WIE es uebertragen wird.

12. Was versteht man unter (De-)Serialisierung?::**Serialisierung (Marshalling):** Umwandlung eines Objekts/Objekt-Graphen in ein serielles Format (Byte-Stream, Text) inkl. Typinformationen. Ziel: Uebertragung oder Speicherung. **Deserialisierung (Un-Marshalling):** Wiederherstellung des Objekts aus dem seriellen Format ohne Vorwissen ueber die konkreten Typen.

13. 3 Technologien fuer die Darstellungsschicht?::**XML** (textbasiert, mit Schema-Validierung); **JSON** (textbasiert, einfache Struktur, eingebaute Typisierung); **Java-Objektserialisierung** (binaer, ueber ObjectInputStream/ObjectOutputStream); Weitere: ASN.1/BER, XDR, CORBA CDR.

14. Aufbau und syntaktische Elemente eines XML-Dokuments?::XML-Deklaration: `<?xml version="1.0" encoding="UTF-8"?>`; Elemente mit Inhalt: `<tag>inhalt</tag>` oder leer: `<tag/>`; Attribute: `<tag attribut="wert">`; Entity-Referenzen: `&lt;`, `&amp;` etc.; Kommentare: `<!-- ... -->`; Processing Instructions: `<?name daten?>`; Baumstruktur (DOM) mit genau einem Wurzelelement.

15. Wohlgeformtheit vs. Validitaet?::**Wohlgeformt:** Syntaktisch korrekt — genau ein Wurzelelement, alle Tags geschlossen, Verschachtelung balanciert, Attribute in Anfuehrungszeichen. **Valide:** Wohlgeformt UND konform zu einer vorgegebenen Dokumentenstruktur (DTD oder XML Schema). Validitaet ist strenger.

16. Wozu dient ein XML-Schema?::Ein XML-Schema beschreibt die **erlaubte Struktur** einer Klasse von XML-Dokumenten: erlaubte Elementbezeichner, deren Reihenfolgen, Inhalte, Attribute und Wertebereiche. Es dient der Validierung und ermoeglicht automatische Code-Generierung (z.B. JAXB).

17. 3 Kompositoren und deren Ergaenzungen?::**`sequence`** — Elemente in fester Reihenfolge; **`all`** — Elemente in beliebiger Reihenfolge, hoechstens einmal; **`choice`** — genau ein Element aus der Liste. **Ergaenzungen:** `minOccurs` (Mindestanzahl), `maxOccurs` (Maximalanzahl, `unbounded` fuer ∞), `default` (Standardwert). Sequenzen und Choices koennen beliebig geschachtelt werden.
