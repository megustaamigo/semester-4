---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 9
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 9

In welche zwei Sublayer kann der Data-Link Layer (Schicht 2 in ISO/OSI) unterteilt werden? Was sind grob die Aufgaben dieser zwei Sublayer?::In LLC und MAC. LLC stellt den Dienst zur Netzschicht bereit; MAC regelt Medienzugriff, Rahmenformat und lokale Adressierung.
Welche Kabeltypen kennen Sie?::Z.B. Twisted Pair, Koaxialkabel und Glasfaser; bei Glasfaser insbesondere Singlemode und Multimode.
Wie ist ein Glasfaserkabel prinzipiell aufgebaut? Wo findet die 'Uebertragung' statt?::Es besteht aus Kern, Mantel, Schutzschichten und Ummantelung. Die Lichtuebertragung findet im Kern statt, gehalten durch Totalreflexion an der Grenzflaeche zum Mantel.
Erklaeren Sie im Zusammenhang mit Glasfaserkabeln die Begriffe 'Moden' und 'Dispersion'!::Moden sind verschiedene Ausbreitungswege des Lichts in der Faser. Dispersion beschreibt die zeitliche Aufweitung eines Pulses, z.B. modal oder chromatisch.
Wie unterscheiden sich Monomode- und Multimode Glasfaserkabel? Welche Eigenschaften resultieren aus dem unterschiedlichen Aufbau?::Monomode hat einen sehr kleinen Kern und nur eine Mode, dadurch geringe Dispersion und grosse Reichweite. Multimode hat einen groesseren Kern, mehrere Moden und daher kuerzere Reichweite, ist aber einfacher anzukoppeln.
Was ist der Unterschied zwischen Stufenindex und Gradientenindex?::Beim Stufenindex aendert sich der Brechungsindex sprunghaft zwischen Kern und Mantel; beim Gradientenindex sinkt er im Kern allmaehlich, was Laufzeitunterschiede der Moden reduziert.
Vergleichen Sie Ring-, Bus- und Stern-Topologie bzgl. ihrer Vor- und Nachteile!::Bus ist einfach, aber stoeranfaellig und schwer skalierbar. Ring bietet geordneten Zugriff, ist aber empfindlich gegen Unterbrechungen. Stern ist gut erweiterbar und leicht zu administrieren, braucht aber ein zentrales Geraet.
Wie (und auf welcher Schicht) arbeitet ein Repeater, Switch, Hub, Router, Bridge?::Repeater und Hub arbeiten auf Schicht 1, sie regenerieren bzw. verteilen Signale. Bridge und Switch arbeiten auf Schicht 2 und leiten anhand von MAC-Adressen weiter. Router arbeitet auf Schicht 3 und entscheidet nach IP-Routen.
Warum gibt es in der Schicht 2 neben dem Header auch einen Trailer? Welche Felder enthaelt der Ethernet-Header?::Der Trailer traegt typischerweise die FCS/CRC zur Fehlererkennung am Rahmenende. Der Ethernet-Header enthaelt Ziel-MAC, Quell-MAC, optional VLAN-Tag und EtherType/Laengenfeld.
