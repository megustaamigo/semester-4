---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 7
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 7

Warum hat UDP neben TCP eine Daseinsberechtigung?::Weil viele Anwendungen geringe Latenz, einfachen Nachrichtentransport oder eigene Zuverlaessigkeitsmechanismen brauchen und dafuer den Overhead von TCP nicht wollen.
Welche Felder enthaelt der UDP-Header?::Source Port, Destination Port, Length und Checksum.
Welche Schritte sind notwendig, um ein analoges Signal in ein digitales zu wandeln?::Abtasten, Quantisieren und anschliessend Codieren der Quantisierungswerte in Bits.
Was besagt das Nyquist-Theorem?::Ein bandbegrenztes Signal kann fehlerfrei rekonstruiert werden, wenn mit mindestens der doppelten Maximalfrequenz abgetastet wird.
Nennen Sie unterschiedliche Kategorien von Multimedia-Anwendungen!::Gespeichertes Streaming, Live-Streaming, interaktive Echtzeitkommunikation wie VoIP/Videokonferenz und klassische Download/Playback-Anwendungen.
Warum ist beim Streaming von Live-Daten immer eine Pufferung auf Empfaengerseite notwendig?::Weil Netzverzoegerung und Jitter schwanken und der Puffer diese Schwankungen ausgleicht, damit die Wiedergabe gleichmaessig bleibt.
Welches Anwendungsprotokoll unterstuetzt die Versendung von Echtzeitdaten ueber UDP?::Typischerweise RTP, oft zusammen mit RTCP.
Warum benoetigt man ein solches Protokoll on-Top von UDP?::UDP liefert keine Zeitstempel, Sequenznummern, Payload-Typen oder Feedback; RTP/RTCP ergaenzen genau diese Informationen fuer Echtzeitmedien.
Wozu dient das Domain Name System (DNS) hauptsaechlich?::Zur Uebersetzung von Namen in IP-Adressen und allgemein zur verteilten Verwaltung von Namensdaten.
Was ist eine Zone, was ist eine (Sub-)Domaene?::Eine Domaene ist ein Teilbaum des DNS-Namensraums; eine Zone ist der administrativ von einem Nameserver verwaltete Ausschnitt davon.
Wie wird eine Zone in den DNS-Raum anschaulich integriert?::Als delegierter Teilbaum unterhalb einer uebergeordneten Zone, abgeschnitten an den Delegationspunkten durch NS-Eintraege.
Woher kennt ein Rechner typischerweise die IPs seines zugehoerigen DNS-Servers?::Meist per DHCP, alternativ statisch konfiguriert.
Was versteht man unter einem Zonen-Transfer?::Die Uebertragung der Zonendaten von einem primaeren auf einen sekundaeren DNS-Server, vollstaendig oder inkrementell.
Loest ein DNS-Server eine Anfrage rekursiv oder iterativ auf?::Ein rekursiver Resolver arbeitet fuer den Client rekursiv, fragt andere Server intern aber meist iterativ ab.
Wie arbeitet die Resolver-Bibliothek beim Client, rekursiv oder iterativ?::Typischerweise schickt sie eine rekursive Anfrage an den konfigurierten Resolver.
Woher kennt ein DNS-Server den 'Startpunkt' seiner Suche?::Aus den fest eingebauten bzw. konfigurierten Root-Server-Hinweisen (Root Hints).
Was ist ein MX-Record?::Ein DNS-Record, der den fuer eine Domaene zustaendigen Mailserver samt Prioritaet angibt.
Warum ist das DNS ein kritischer Dienst im Internet, und Ziel von Hacking-Attacken?::Weil fast alle Internetdienste zuerst Namensaufloesung brauchen; Ausfaelle oder Manipulationen koennen daher grosse Teile der Kommunikation stoeren oder umlenken.
