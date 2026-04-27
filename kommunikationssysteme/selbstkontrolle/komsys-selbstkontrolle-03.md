---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 3
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 3

Was ist ein sog. Autonomes System (AS) im Internet?::Ein AS ist ein Netz oder Verbund von Netzen unter gemeinsamer Administration mit einheitlicher Routing-Politik; nach aussen wird es durch eine AS-Nummer identifiziert.
Wodurch sind Tier 1-3 ISPs charakterisiert?::Tier 1 hat globale Reichweite und braucht keinen Upstream, Tier 2 kauft Transit und peert teilweise, Tier 3 ist lokaler Access-Provider nahe am Endkunden.
Wieso kann durch den kooperativen Verbund autonomer Systeme es zu Paketverlusten gerade auch zwischen AS kommen?::Zwischen AS gibt es nur Best-Effort-Weiterleitung; Uebergabelinks und Routerpuffer koennen ueberlastet sein, und Peering-/Transit-Politiken garantieren keine verlustfreie Zustellung.
Wodurch zeichnet sich ein Backbone-Netz aus?::Durch sehr hohe Kapazitaet, wenige aber leistungsstarke Kernrouter, grosse Reichweite, Redundanz und die Aufgabe, viele Netze bzw. AS miteinander zu verbinden.
Beschreiben Sie mit eigenen Worten, was ein Router nach dem Empfang eines Paketes mit der IP-Zieladresse X zu tun hat! Welche Informationen werden dabei benoetigt?::Er prueft den Header, dekrementiert TTL, berechnet bei Bedarf die Header-Pruefsumme neu, sucht per Longest Prefix Match den besten Forwarding-Eintrag, bestimmt Next Hop und Ausgangsinterface, loest dort die L2-Adresse auf und leitet weiter oder verwirft mit ICMP. Benoetigt werden Ziel-IP, Forwarding-Tabelle, Praefixe/Masken, Interface-Infos und ggf. ARP-Cache.
Wie kann im (alten) klassenbasierten System von IP-Adressen erkannt werden, um was fuer eine Klasse es sich im konkreten Fall handelt?::An den fuehrenden Bits des ersten Oktetts: 0 = Klasse A, 10 = B, 110 = C, 1110 = D, 1111 = E.
Wie viele Knoten (Hosts) koennen maximal an ein Class-B Netz angeschlossen werden?::2^16 - 2 = 65534 Hosts, weil Netzadresse und Broadcast-Adresse reserviert sind.
Welche speziellen Werte existieren fuer den Host-Anteil in einer IP-Adresse, und was bedeuten sie?::Alle Host-Bits 0 bedeuten Netzadresse, alle Host-Bits 1 bedeuten gerichteten Broadcast fuer dieses Netz.
Was sind sog. private IP Adressen und Netzwerke?::RFC-1918-Adressbereiche fuer interne Netze, die im Internet nicht global geroutet werden: 10.0.0.0/8, 172.16.0.0/12 und 192.168.0.0/16.
Was ist eine Subnetzmaske (haeufig auch einfach nur Netzmaske genannt) und wie ist sie aufgebaut?::Eine Bitmaske mit zusammenhaengenden 1-Bits fuer Netz-/Subnetzanteil und anschliessenden 0-Bits fuer den Hostanteil, z.B. 255.255.255.0 = /24.
Warum sind fuer viele Organisationen Subnetze ein Muss?::Sie strukturieren grosse Netze, begrenzen Broadcast-Domaenen, verbessern Sicherheit und Administration und nutzen Adressen effizienter.
Sie koennen zu einem gegebenen Netz und einer Mindestanzahl an erforderlichen Subnetzen die Subnetzmaske und den fuer jedes Subnetz moegliche Adressraum zur Vergabe von Endpunktadressen bestimmen.::Man leiht dem Hostteil so viele Bits, bis 2^s mindestens die benoetigte Zahl von Subnetzen liefert; neuer Prefix = alter Prefix + s, nutzbare Hosts je Subnetz = 2^(32-neuer Prefix) - 2, die Subnetze entstehen in Blockgroessen von 2^(32-neuer Prefix).
Wieso duerfen Sie die Rechneradressen Netzteil-00..00 und Netzteil-11..11 nicht verwenden?::Weil sie fuer Netzadresse bzw. Broadcast-Adresse reserviert sind und deshalb keine einzelnen Endgeraete bezeichnen.
Sie kennen die Idee von Classless Interdomain Routing und erkennen, dass hierdurch die Untervermietung von Adressbereichen moeglich ist.::CIDR arbeitet mit frei waehhlbaren Prefixlaengen statt Klassen; dadurch kann ein grosser Block flexibel in kleinere Praefixe an Kunden weitergegeben werden.
Sie sind sich darueber bewusst, dass CIDR zur Kompaktierung der Routing-Tabellen fuehrt und koennen entsprechendes auf durch Zusammenfassen von Routing-Tabellen anwenden.::Benachbarte Netze mit gemeinsamem Prefix koennen zu einem groesseren Praefix aggregiert werden; dadurch sinkt die Zahl der Routing-Eintraege.
Sie wissen um das Resultat fuer die Suche nach Routing-Eintraegen und beherrschen die Longest-(Prefix-)Match-Prozedur zum bestimmen des erwaehlten Routing-Eintrages.::Wenn mehrere Routen passen, gewinnt der Eintrag mit dem laengsten passenden Prefix, also die spezifischste Route.
Sie verstehen die Massnahmen, die zum Einsatz privater IP-Adressen notwendig sind und koennen erklaeren, wieso die Bereitstellung eines Dienstes mit einer privaten Adresse beim Zugangsrouter, der eine Network Address (Port) Translation durchfuehrt eine manuelle Konfiguration erfordert (oder Protokolle, die dies ermoeglichen).::Private Adressen brauchen NAT/NAPT am Randrouter; eingehende Verbindungen funktionieren nur mit statischem Port-Forwarding oder NAT-Traversal, weil der Router ohne bestehendes Mapping nicht weiss, an welchen internen Host er das Paket liefern soll.
