---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 1
date: 2026-04-09
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 1

1. Was ist der Unterschied zwischen Client-Server und Peer-to-Peer Netzwerken?::**Client-Server:** Klare Rollentrennung — Server ist langlebig, wartet auf Anfragen und bearbeitet sie (reagierend). Client ist kurzlebig, stellt Anfragen (initiierend). Ein Server kann auch als Client anderer Dienste agieren. **Peer-to-Peer:** Alle Teilnehmer sind gleichberechtigt, jeder hat eine Anwendung + lokale Datensicht. Oft bessere Leistung, uebergreifender Datenbestand. Beispiele: File-Sharing, NoSQL.

2. Wie unterscheiden sich Point-to-Point- und Multi-Access-Netzwerke?::**Point-to-Point:** Direkte Leitung zwischen genau zwei Geraeten, kein anderes Geraet kann diese nutzen. Kann Full-Duplex (gleichzeitig senden/empfangen), Half-Duplex oder Simplex sein. **Multi-Access:** Mehrere Geraete teilen sich einen Uebertragungskanal. Daten muessen mit Zieladresse versehen werden, alle Geraete pruefen ob Nachricht fuer sie bestimmt ist.

3. Welche Konsequenzen entstehen fuer die Datenkommunikation bei Multi-Access-Netzwerken?::Daten muessen mit **Zieladresse** versehen werden; Daten werden in **Protocol Data Units (PDU)** eingeteilt; Alle Geraete pruefen aktiv, ob Nachrichten fuer sie bestimmt sind; Fuer Nachrichten an alle: **Broadcast-Adressen** noetig; Zugriff auf das geteilte Medium muss geregelt werden (MAC).

4. Wie unterscheiden sich statische und dynamische Netzwerke?::**Statisch:** Fest verdrahtete Verbindungen, keine inhaerent im Netz verankerte Vermittlungsfunktion. Vermittlung nur ueber Netzgrenzen durch Store-and-Forward. Das Internet basiert auf diesem Prinzip. **Dynamisch:** Enthalten konfigurierbare Schaltelemente, die Wege dynamisch schalten koennen. Direkte Kommunikation zwischen beliebigen Knoten moeglich. Beispiel: Telefonnetz.

5. Was versteht man unter dem Store-and-Forward-Verfahren, und wo wird es eingesetzt?::Zwischenknoten speichern ankommende Nachrichten vollstaendig und leiten sie dann an den naechsten Knoten weiter (basierend auf Routing-Tabellen). Eingesetzt in statischen Netzen, insbesondere im Internet, wo Router an Netzgrenzen Datenpakete weiterleiten.

6. Welche Metriken kennen Sie, um Topologien von Rechnernetzen zu charakterisieren?::**Durchmesser:** Maximaler Abstand zweier Knoten (Anzahl Hops) — soll moeglichst klein sein. **Bisektionsbreite:** Minimale Anzahl Kanten, um das Netz in zwei Haelften zu teilen — soll moeglichst gross sein (Fehlertoleranz). **Knotengrad:** Anzahl Verbindungen eines Knotens zu Nachbarn — soll moeglichst klein sein (Kosten).

7. Welche raeumliche Ausdehnung besitzen typischerweise LANs?::10 Meter bis wenige Kilometer (Raum, Gebaeude, Campus). Im Besitz einer Organisation. Uebertragungskapazitaet bis 10.000 Mbit/s, Latenz < 10 ms. Wichtigstes Beispiel: (Gigabit-)Ethernet.

8. Was versteht man unter einem Protokoll im Kontext der Datenkommunikation?::Ein Protokoll ist die **Gesamtheit aller Vereinbarungen zwischen Computeranwendungen zum Zweck einer gemeinsamen Kommunikation**. Es regelt: Uebertragungsrichtung, Datenformat/-kodierung, Wegeermittlung und Fehlerbehandlung.

9. Nennen Sie 3-4 Probleme, die fuer eine erfolgreiche Datenkommunikation zu loesen sind!::Physikalische Eigenschaften des Uebertragungsmediums und Darstellung als Signal; Zugriffsregeln bei geteilten Medien (Multi-Access); Adressierung von Endpunkten und Erreichbarkeit entfernter Systeme; Erkennung von Uebertragungsfehlern; Kodierungsregeln und Semantiken.

10. Welche Vorteile (und ggf. auch Nachteile) haben Schichten-Architekturen?::**Vorteile:** Modularisierung — einzelne Komponenten austauschbar, sehr flexibel. Jede Schicht hat klare Verantwortlichkeit, Komplexitaet wird handhabbar. **Nachteile:** Durch vorgegebene Struktur wird vieles umstaendlich, dadurch nicht so effizient wie ein monolithisches, auf eine Anwendung optimiertes Programm. Overhead durch Schichten-Kommunikation.

11. Wie heissen die sieben Schichten des ISO/OSI-Modells, und welche grobe Aufgabe haben sie jeweils?::1. **Physical** (Bituebertragung): Transport einzelner Bits ueber physisches Medium; 2. **Data Link** (Sicherung): Fehlerfreie Uebertragung zwischen Rechnern im selben Netz, Zugriffskontrolle; 3. **Network** (Vermittlung): Routing — Datenuebertragung zwischen Rechnern ueber Netzgrenzen hinweg; 4. **Transport**: Kommunikation zwischen Anwendungen/Prozessen, Segmentierung, Fluss-/Staukontrolle; 5. **Session** (Sitzung): Dialogkontrolle und Wiederaufsetzpunkte; 6. **Presentation** (Darstellung): Plattformunabhaengige Datendarstellung; 7. **Application** (Anwendung): Standard-Schnittstellen/Grunddienste fuer Anwendungstypen.

12. Wie uebertraegt jede Schicht die fuer sie relevanten Informationen?::Jede Schicht versieht ihre Nachricht mit Kontrollinformationen (**Header**) — zusammen als **Protocol Data Unit (PDU)** bezeichnet. Die PDU von Schicht n wird an Schicht (n-1) weitergeleitet, wo sie als Inhaltsdaten behandelt wird. Untere Schichten benoetigen ein **Upper-Layer-Protocol-Feld**, damit die passende obere Schicht ausgewaehlt werden kann.

13. Was ist hier das Besondere an der Sicherungsschicht?::Die Sicherungsschicht weicht vom normalen Schema ab: hier findet **Framing** statt — die Daten werden in Rahmen unterteilt mit einer **Pruefsumme am Ende** (nicht nur Header am Anfang). Dies dient der Fehlererkennung/-korrektur und Synchronisation.

14. Welche Unterschiede existieren zwischen dem ISO/OSI-Modell und dem Internet-Referenzmodell?::OSI hat 7 Schichten, TCP/IP hat 4. Die Schichten 5-7 (Session, Presentation, Application) werden im Internet in der **Anwendungsschicht** zusammengefasst — die Anwendung implementiert diese Funktionen selbst. Schichten 1-2 werden zur **Host-to-Network-Schicht** zusammengefasst und durch die Netzwerkkarte realisiert. TCP/IP ist pragmatischer, OSI systematischer.

15. Zu welcher Schicht gehoeren die Protokolle TCP und UDP, und wie unterscheiden sie sich?::Beide gehoeren zur **Transportschicht** (Schicht 4). **TCP:** Zuverlaessig, verbindungsorientiert. Garantiert Reihenfolge und Vollstaendigkeit. Segmentierung, Flusskontrolle, Staukontrolle. **UDP:** Unzuverlaessig ("best effort"), verbindungslos. Schneller, aber keine Garantie. Geeignet wenn Geschwindigkeit wichtiger als Zuverlaessigkeit (Sprache, Video).

16. Welche Schichten implementiert der Benutzer, welches das Betriebssystem und welche sind auf der Netzwerkkarte?::**Anwendung (Benutzer):** Anwendungsschicht — realisiert Kommunikationsregeln, Darstellung, Wiederaufsetzpunkte. **Betriebssystem:** Transport- und Vermittlungsschicht (TCP/UDP und IP). **Netzwerkkarte (Hardware):** Host-to-Network-Schicht (Sicherung + Bituebertragung).

17. Welche Vorteile hat es, die Host-to-Network-Schicht komplett in Hardware zu realisieren?::Die Netzwerkkarte entscheidet bei Multi-Access-Netzen **selbst**, ob Daten fuer den eigenen Rechner bestimmt sind → **Entlastung des Betriebssystems**. Irrelevante Nachrichten werden bereits in Hardware gefiltert, ohne dass CPU-Zyklen verbraucht werden. Bei der hohen Datenrate moderner Netze waere eine reine Software-Loesung zu langsam.
