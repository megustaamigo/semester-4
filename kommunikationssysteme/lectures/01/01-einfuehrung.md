---
course: kommunikationssysteme
type: lecture
date: 2026-04-09
tags:
  - kommunikationssysteme
  - lecture
  - grundlagen
  - datenkommunikation
  - netzwerke
source: Kommunikationssysteme_1_Einfuehrung.pdf
---

# 01 — Einfuehrung

**Dozent:** Prof. Dr. Volker Sander (FH Aachen, Campus Juelich, Raum 01G05)
**Folien:** [[kommunikationssysteme/resources/Kommunikationssysteme_1_Einfuehrung.pdf|Kommunikationssysteme_1_Einfuehrung.pdf]]

## Lehrziele

### Vorlesung
- Grundlagen der Datenkommunikation beherrschen
- Funktionsweise der Internetprotokolle beschreiben und spezifische Eigenschaften begruenden koennen
- Gaengige Techniken zum Aufbau lokaler Netze kennen und anwenden koennen

### Praktikum
- Erste Client-Server-Anwendungen entwerfen und implementieren
- Kommunikationsmuster verstehen
- TLS anwenden koennen
- XML verarbeiten und **Multithreading** anwenden koennen
- Topologie von IP-Netzen kennen
- TCP und UDP verstehen

## Struktur der Vorlesung

1. **Grundlagen der Datenkommunikation** (diese Vorlesung)
2. Einfache Client-Server-Anwendungen
3. Internet-Protokolle
4. Netzwerke

## Literatur

- *Computer Networking: A Top-Down Approach* — J.F. Kurose / K.W. Ross, 6th Edition, Pearson
- *Computer Networks* — A.S. Tanenbaum, 5th Edition, Pearson
- *Computernetzwerke und Internets* — Douglas E. Comer, 3. Auflage, Pearson Studium

## Informationsquellen

- **RFCs** (Request for Comments) — technische Spezifikationen und organisatorische Hinweise fuer das Internet, veroeffentlicht durch die **IETF** (Internet Engineering Task Force)

---

## Grundbegriffe der Datenkommunikation

### Definition
Datenkommunikation beschaeftigt sich mit dem **immateriellen Transport digitaler Daten** zwischen Endgeraeten. Behandelt werden die dafuer benoetigten Verfahren und Vereinbarungen.

### Bedeutung
- Ermoeglicht Zugriff auf fremde/entfernte Ressourcen und Dienste
- Erfordert: effiziente Methoden zum Datenaustausch + Absprachen/Regeln (Protokolle)
- Vorteile:
  - Nutzung lokal nicht verfuegbarer Ressourcen
  - Kostensenkung durch gemeinsame Nutzung von Betriebsmitteln

### Kernaufgabe
Information zwischen beteiligten Systemen gemaess den Anforderungen uebertragen. Datenkommunikation ist eine elementare Funktionalitaet in verteilten Umgebungen. Basis sind **Kommunikationsnetze** (Technologien und deren Nutzung).

---

## Rollen der beteiligten Systeme

### Client-Server-Modell
- **Server-Prozess:** Langlebige Anwendung, die kontinuierlich auf Anfragen wartet, diese verarbeitet und beantwortet (reagierender Prozess)
- **Client-Prozess:** Zumeist kurzlebige Anwendung, die Anfragen an den Server stellt und ggf. auf die Antwort wartet (initiierender Prozess)
- Ein Server kann auch andere Dienste nutzen und somit gleichzeitig als Client agieren

### Client-Server-Varianten

#### Server-Verbund
- Dienst wird von einem Verbund von Servern erbracht
- Erst durch den Verbund ergibt sich die Gesamtsicht — ein einzelner Server "weiss" nicht alles
- Client merkt ggf. nichts von dem Verbund

#### Forward Proxy
- Zwischenspeichern/Anonymisieren der Client-Anfragen (Stellvertreter-Prinzip)
- Oftmals Stellvertreter fuer viele Clients

#### Reverse Proxy
- Lastbalancierung von Webseiten
- Bildet den Kontakt-Endpunkt, leitet Anfragen an Worker weiter

### Peer-to-Peer (Gleichrangige Prozesse)
- Alle Teilnehmer sind gleichberechtigt (Anwendung + lokale Datensicht)
- Oft bessere Leistung als Client-Server
- Uebergreifender Datenbestand
- Beispiele: File-Sharing, NoSQL

---

## Kommunikationsarten

| Art | Beschreibung |
|-----|-------------|
| **Unicast** | Nachricht an genau einen Empfaenger |
| **Broadcast** | Nachricht an alle Teilnehmer |
| **Multicast** | Nachricht an eine ausgewaehlte Gruppe |
| **Anycast** | Nachricht an den naechstgelegenen aus einer Gruppe |

### Synchrone vs. Asynchrone Kommunikation
- **Synchron:** Client wartet nach dem Versenden der Anfrage **blockierend** auf die Antwort
- **Asynchron:** Nach dem Versenden kann anderes erledigt werden; die Antwort wird spaeter (meist event-getriggert) verarbeitet

---

## Klassifikation der Kommunikationsnetze

### Nach Uebertragungstechnik

#### Point-to-Point (Punkt-zu-Punkt)
- Ein Paar von Geraeten ist durch eine direkte Leitung verbunden
- Kein anderes Geraet kann diese Leitung nutzen
- **Full-Duplex:** Senden und Empfangen gleichzeitig moeglich
- **Half-Duplex:** Nur eines von beiden gleichzeitig
- **Simplex:** Daten koennen nur in eine Richtung fliessen

#### Multi-Access-Netze
- Mehrere Geraete teilen sich einen Uebertragungskanal
- Daten muessen mit einer **Zieladresse** versehen werden
- Daten werden in **Uebertragungseinheiten (Protocol Data Unit, PDU)** eingeteilt
- Alle Geraete am Netz pruefen aktiv, ob die Nachricht fuer sie bestimmt ist
- Fuer Nachrichten an alle: **Broadcast-Adressen**

### Nach Verbindungsart

#### Statisch
- Geraete (Knoten) besitzen ggf. mehrere Netzanschluesse: fest verdrahtete Punkt-zu-Punkt oder Multi-Access-Netze
- **Keine** inhaerent im Netz verankerte Vermittlungsfunktion
- Vermittlung ueber Netzgrenzen durch Weiterleiten moeglich (**Store-and-Forward-Prinzip**)
- Das Internet basiert auf diesem Prinzip

#### Dynamisch
- Verbindungen enthalten **konfigurierbare Schaltelemente**
- Koennen **dynamisch vermitteln** (Weg wird geschaltet)
- Direkte Kommunikation zwischen beliebigen Knoten moeglich
- Basis z.B. fuer CPU-Speicher-Anbindung, Telefonnetz

### Bewertungskriterien fuer statische Netze

| Kriterium | Beschreibung | Ziel |
|-----------|-------------|------|
| **Durchmesser** | Maximaler Abstand zweier Knoten (Anzahl Hops) | Moeglichst klein |
| **Bisektionsbreite** | Min. Anzahl Kanten, um das Netz in zwei Haelften zu teilen | Moeglichst gross (Fehlertoleranz) |
| **Knotengrad** | Anzahl Verbindungen eines Knotens zu Nachbarn | Moeglichst klein (Kosten) |

> **Merke:** Haben alle Knoten den gleichen Grad, spricht man von einem **regulaeren Netz**.

### Statische Verbindungsnetze — Topologien

**Eindimensional:**
- **Kette** (kein Bus!) — lineares Netz
- **Ring** (z.B. Tokenring) — geschlossener Kreis

**Zweidimensional:**
- **Chordaler Ring** — Ring mit zusaetzlichen Querverbindungen
- **Barrel Shifter** — Verbindung aller Knoten im Abstand 2^i

### Nach Ausdehnung

| Distanz | Bereich | Netztyp |
|---------|---------|---------|
| 1 m | — | **PAN** (Personal Area Network) |
| 10 m - 1 km | Raum / Gebaeude / Campus | **LAN** (Local Area Network) |
| 10 km | Stadt | **MAN** (Metropolitan Area Network) |
| 100 - 1000 km | Land / Kontinent | **WAN** (Wide Area Network) |
| 10.000 km | Planet | **Internet** |

---

## LAN (Local Area Network)
- Kommunikationsinfrastruktur fuer begrenzten geographischen Bereich (10m - wenige km)
- Bindet typischerweise auch die Endsysteme direkt an
- **Ueblicherweise im Besitz einer Organisation**
- Uebertragungskapazitaet bis zu 10.000 Mbit/s
- Uebertragungsdauer im unteren Millisekundenbereich (<10 ms)
- Wichtigstes Beispiel: **(Gigabit-)Ethernet**
- Mittels **VPN** koennen LANs an verschiedenen geographischen Regionen kombiniert werden

## MAN (Metropolitan Area Network)
- Ueberbrueckt groessere Distanzen als ein LAN (Staedtebereich)
- Oftmals Zusammenschaltung mehrerer LANs, ohne direkten Anschluss von Endgeraeten
- Wichtiger Unterschied: **Wegerechte erforderlich**
- Trend: optische Ethernet-Techniken (Metro-Ethernet)

## WAN (Wide Area Network)
- Punkt-zu-Punkt Verbindungen zwischen **Points-of-Presence**
- Beispiel: deutsches Forschungsnetz **X-WIN** (8 Core-Router, 58 Aggregationsrouter, 66 Kernnetzknoten, 1088 Teilnehmer, 10.250 km Glasfaserkabel, bis zu 800 Gbit/s)
- Zentraler Kontenpunkt Frankfurt — Anschluss an das europaeische Wissenschaftsnetz **GEANT**

---

## Fragen zur [[kommunikationssysteme/selbstkontrolle/selbstkontrolle-01|Selbstkontrolle]]

**1. Was ist der Unterschied zwischen Client-Server und Peer-to-Peer Netzwerken?**
- **Client-Server:** Klare Rollentrennung — Server ist langlebig, wartet auf Anfragen und bearbeitet sie (reagierend). Client ist kurzlebig, stellt Anfragen (initiierend). Ein Server kann auch als Client anderer Dienste agieren.
- **Peer-to-Peer:** Alle Teilnehmer sind gleichberechtigt, jeder hat eine Anwendung + lokale Datensicht. Oft bessere Leistung, uebergreifender Datenbestand. Beispiele: File-Sharing, NoSQL.

**2. Wie unterscheiden sich Point-to-Point- und Multi-Access-Netzwerke?**
- **Point-to-Point:** Direkte Leitung zwischen genau zwei Geraeten, kein anderes Geraet kann diese nutzen. Kann Full-Duplex (gleichzeitig senden/empfangen), Half-Duplex oder Simplex sein.
- **Multi-Access:** Mehrere Geraete teilen sich einen Uebertragungskanal. Daten muessen mit Zieladresse versehen werden, alle Geraete pruefen ob Nachricht fuer sie bestimmt ist.

**3. Welche Konsequenzen entstehen fuer die Datenkommunikation bei Multi-Access-Netzwerken?**
- Daten muessen mit **Zieladresse** versehen werden
- Daten werden in **Protocol Data Units (PDU)** eingeteilt
- Alle Geraete pruefen aktiv, ob Nachrichten fuer sie bestimmt sind
- Fuer Nachrichten an alle: **Broadcast-Adressen** noetig
- Zugriff auf das geteilte Medium muss geregelt werden (MAC)

**4. Wie unterscheiden sich statische und dynamische Netzwerke?**
- **Statisch:** Fest verdrahtete Verbindungen, keine inhaerent im Netz verankerte Vermittlungsfunktion. Vermittlung nur ueber Netzgrenzen durch Store-and-Forward. Das Internet basiert auf diesem Prinzip.
- **Dynamisch:** Enthalten konfigurierbare Schaltelemente, die Wege dynamisch schalten koennen. Direkte Kommunikation zwischen beliebigen Knoten moeglich. Beispiel: Telefonnetz.

**5. Was versteht man unter dem Store-and-Forward-Verfahren, und wo wird es eingesetzt?**
- Zwischenknoten speichern ankommende Nachrichten vollstaendig und leiten sie dann an den naechsten Knoten weiter (basierend auf Routing-Tabellen). Eingesetzt in statischen Netzen, insbesondere im Internet, wo Router an Netzgrenzen Datenpakete weiterleiten.

**6. Welche Metriken kennen Sie, um Topologien von Rechnernetzen zu charakterisieren?**
- **Durchmesser:** Maximaler Abstand zweier Knoten (Anzahl Hops) — soll moeglichst klein sein
- **Bisektionsbreite:** Minimale Anzahl Kanten, um das Netz in zwei Haelften zu teilen — soll moeglichst gross sein (Fehlertoleranz)
- **Knotengrad:** Anzahl Verbindungen eines Knotens zu Nachbarn — soll moeglichst klein sein (Kosten)

**7. Welche raeumliche Ausdehnung besitzen typischerweise LANs?**
- 10 Meter bis wenige Kilometer (Raum, Gebaeude, Campus). Im Besitz einer Organisation. Uebertragungskapazitaet bis 10.000 Mbit/s, Latenz < 10 ms. Wichtigstes Beispiel: (Gigabit-)Ethernet.
