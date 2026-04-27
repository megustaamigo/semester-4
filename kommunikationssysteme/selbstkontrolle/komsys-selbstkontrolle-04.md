---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 4
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 4

Erklaeren Sie den Unterschied zwischen Routing und Forwarding. Ordnen Sie den beiden Begriffen die Begriffe Control Plane und Data Plane zu.::Routing berechnet bzw. lernt Wege und fuellt die Tabellen; das gehoert zur Control Plane. Forwarding leitet jedes einzelne Paket anhand der Tabelle weiter; das gehoert zur Data Plane.
Welche 2 Arten von Routingverfahren kennen Sie, und welche Unterschiede existieren zwischen den Verfahren?::Link-State und Distance-Vector. Link-State verteilt Topologieinformationen und jeder Router rechnet lokal den kuerzesten Pfad; Distance-Vector tauscht nur Distanzschaetzungen mit Nachbarn aus und rechnet iterativ.
Wieso gibt es 2 Verfahren?::Weil es einen Zielkonflikt zwischen Informationsaufwand, Konvergenzgeschwindigkeit, Skalierbarkeit und Administrationsgrenzen gibt; kein Verfahren ist fuer alle Netze gleich gut.
Beschreiben Sie ein Distance-Vector-Verfahren mit eigenen Worten. Wo wird die Belmann-Ford-Gleichung verwendet.::Jeder Router speichert fuer jedes Ziel die beste bekannte Distanz und tauscht diese periodisch mit Nachbarn aus. Mit Bellman-Ford wird fuer jedes Ziel das Minimum aus Linkkosten zum Nachbarn plus dessen gemeldeter Distanz berechnet.
Was sind ''private' IP-Netze und -Adressen?::Nicht global geroutete interne RFC-1918-Netze wie 10/8, 172.16/12 und 192.168/16.
Wie werden solche Adressen im Zusammenhang mit NAT eingesetzt?::Interne Hosts nutzen private Adressen; am Uebergangsrouter werden sie beim Verlassen des Netzes auf eine oeffentliche Adresse und oft auch auf andere Ports umgesetzt.
Was passiert beim Verschicken eines IP-Paketes ueber einen NAT-Router?::Der Router ersetzt typischerweise Quell-IP und ggf. Quellport, legt ein Mapping an und passt Pruefsummen an, bevor das Paket ins Internet geht.
Was passiert anschliessend beim Empfangen eines Antwortpaketes (von einem Server ausserhalb des privaten Netzes)?::Der NAT-Router sucht das passende Mapping zur oeffentlichen Zieladresse und zum Zielport, setzt beides auf die interne Adresse zurueck und liefert das Paket an den richtigen Host.
Warum verletzt das NAT-Verfahren die Schichtenarchitektur?::Weil NAT nicht nur Netzadressen betrachtet, sondern oft auch TCP/UDP-Ports und teils sogar Anwendungsdaten auswerten oder aendern muss.
Welche Probleme koennen bei der Adressumsetzung auftreten?::Eingehende Verbindungen funktionieren ohne Mapping nicht, Protokolle mit eingebetteten IPs/Ports machen Probleme, Port-Kollisionen sind moeglich und Ende-zu-Ende-Transparenz geht verloren.
Welche unterschiedlichen Aufgaben und Eigenschaften hat eine IP- gegenueber einer MAC-Adresse?::IP ist eine logische, hierarchische und routbare Adresse fuer Ende-zu-Ende-Wege; MAC ist eine lokale Link-Adresse fuer die Zustellung im unmittelbar angeschlossenen Netz.
Wozu wird das ARP-Protokoll eingesetzt? Diskutieren Sie die Zuordnung zur Schicht.::ARP loest lokal eine IP-Adresse in eine MAC-Adresse auf. Es wird meist zwischen Schicht 2 und 3 eingeordnet, oft als "Schicht 2.5".
Wie funktioniert das DHCP-Protokoll?::Typisch per DORA: Discover, Offer, Request, Acknowledge. Ein Client ohne Adresse sendet Broadcasts und erhaelt IP-Adresse, Maske, Gateway und DNS-Server vom DHCP-Server.
Erklaeren Sie die Funktion der Felder "Time to live" und "Fragment offset" im IPv4 Header!::TTL begrenzt die maximale Hop-Zahl und verhindert Endlosschleifen. Fragment Offset markiert die Position eines Fragments im Originaldatagramm in 8-Byte-Einheiten.
Welche IPv4-Header-Elemente werden immer in jedem Router geaendert?::Immer TTL und dadurch auch die Header Checksum; nur bei Fragmentierung kommen weitere Aenderungen hinzu.
Warum werden IP-Pakete auf Ihrem Weg ggf. fragmentiert? Welche Rolle spielt dabei die MTU?::Wenn ein Datagramm groesser ist als die MTU des naechsten Links, muss es zerlegt werden oder wird bei gesetztem DF-Bit verworfen. Die MTU gibt die maximal uebertragbare Rahmen-Nutzlast der darunterliegenden Schicht vor.
Sie koennen die Fragmentierung anwenden und beachten dabei, dass Vielfache von 8 hier wichtig sind.::Alle Fragmente ausser dem letzten muessen eine Nutzdatenlaenge haben, die ein Vielfaches von 8 Bytes ist, weil der Offset in 8-Byte-Schritten codiert wird.
Welche Aufgaben hat das ICMP-Protokoll?::ICMP uebermittelt Fehlermeldungen und Diagnoseinformationen, z.B. Destination Unreachable, Time Exceeded, Echo Request/Reply und Redirect.
Wie lang (in Bytes) ist eine IPv6-Adresse?::16 Bytes bzw. 128 Bit.
Inwiefern ist IPv6 im Vergleich zu IPv4 einfacher?::Es hat einen festen 40-Byte-Header, keine Router-Fragmentierung, keine Header-Pruefsumme im Routerpfad und eine klarere Erweiterung ueber Extension Headers.
Warum ist das FlowLabel-Feld bei IPv6 so interessant?::Es kann Pakete eines Flows kennzeichnen, damit Router sie konsistent oder QoS-freundlich behandeln koennen, ohne tiefer in Transportdaten schauen zu muessen.
Was ist IP-Anycast?::Mehrere Systeme teilen sich dieselbe IP-Adresse; das Routing liefert Pakete automatisch zum topologisch naechsten bzw. guenstigsten Anycast-Knoten.
