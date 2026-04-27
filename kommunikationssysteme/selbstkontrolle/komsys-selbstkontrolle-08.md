---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 8
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 8

Was ist MIME? Warum wird es benoetigt? Nennen Sie wichtige Attribute, die hier gesetzt werden?::MIME erweitert Internet-Nachrichten um Typinformationen und Kodierungen fuer nicht-reinen ASCII-Inhalt. Wichtig sind z.B. Content-Type, Content-Transfer-Encoding, Content-Disposition und ggf. Charset/Boundary.
Erklaeren Sie das Konzept von 'Virtual Hosts' bei HTTP!::Mehrere Websites teilen sich dieselbe IP-Adresse; der Server waehlt anhand des Host-Headers bzw. bei HTTPS ueber SNI die passende Konfiguration.
Wo ist der SSL-Layer im ISO/OSI oder Internet-Schichtenmodell angesiedelt?::Zwischen Anwendung und Transport, also oberhalb von TCP und unterhalb von HTTP, SMTP usw.
Erklaeren Sie die Unterschiede/Vorteile/Nachteile von symmetrischer und asymmetrischer Verschluesselung!::Symmetrisch ist schnell und gut fuer grosse Datenmengen, braucht aber einen sicheren Schluesselaustausch. Asymmetrisch erleichtert Authentisierung und Schluesselverteilung, ist aber deutlich langsamer.
Was ist ein Zertifikat? Wer stellt es aus, und welche Informationen enthaelt es?::Ein Zertifikat bindet einen oeffentlichen Schluessel an eine Identitaet. Es wird von einer Certification Authority signiert und enthaelt u.a. Subject, Public Key, Gueltigkeit, Aussteller und Signatur.
Welche Eigenschaften hat eine Hash-Funktion? Wo wird sie im Kontext der Verschluesselung eingesetzt?::Sie soll deterministisch, schnell, kollisionsarm und praktisch irreversibel sein. Sie wird fuer Integritaet, Signaturen, Zertifikate, Passwortspeicherung und MAC/HMAC genutzt.
Wie kann man bei 2 Kommunikationspartnern ein 'Geheimnis' (z.B. einen Schluessel zur symmetrischen Verschluesselung) erzeugen, ohne dieses Geheimnis ueber das Netzwerk oder einen anderen Kanal auszutauschen?::Mit einem Schluesselaustauschverfahren wie Diffie-Hellman bzw. ECDHE, bei dem beide Seiten aus oeffentlichen Werten denselben Sitzungsschluessel ableiten.
Welche Aufgaben hat das SSL Handshake Protokoll und das SSL Record Protokoll?::Der Handshake handelt Algorithmen und Schluessel aus und authentisiert die Parteien; das Record-Protokoll uebertraegt danach die Nutzdaten vertraulich und integer geschuetzt.
Erklaeren Sie den Handshake::Client und Server handeln Version und Cipher Suite aus, authentisieren typischerweise den Server per Zertifikat, fuehren einen Schluesselaustausch durch und leiten daraus Sitzungsschluessel fuer die Record-Verschluesselung ab.
Warum ist das FTP-Protokoll so besonders? Welche Vorteile hat das Vorgehen?::FTP trennt Steuer- und Datenverbindung. Das erlaubt klare Kommandosteuerung und getrennte Datenkanaele, macht das Protokoll aber komplizierter.
Warum sollte 'einfaches' FTP heute nicht mehr verwendet werden?::Weil Zugangsdaten und Daten unverschluesselt uebertragen werden und leicht mitgelesen oder manipuliert werden koennen.
Warum gibt es in einem typischen Heimnetzwerk Probleme mit dem FTP 'Active' Mode?::Weil der Server fuer die Datenverbindung aktiv zum Client zurueck verbindet, was hinter NAT und Firewalls oft blockiert wird.
Welches Protokoll wird zum Versenden von Emails verwendet? Was passiert konkret beim Versenden, und wie ist das DNS beteiligt?::SMTP. Der sendende Server sucht per DNS meist den MX-Record der Ziel-Domaene und uebergibt die Mail an den zustaendigen Mailserver.
Warum ist einfaches SMTP unsicher?::Ohne TLS und Zusatzmechanismen ist SMTP im Klartext, leicht spoofbar und bietet weder Vertraulichkeit noch starke Authentisierung.
Wozu dient das (veraltete) TELNET-Protokoll? Wozu kann es (z.B. im Praktikum) sinnvoll eingesetzt werden?::Telnet bietet interaktive Terminal-Sitzungen im Klartext. Heute ist es unsicher, aber zum einfachen Testen textbasierter TCP-Dienste kann es noch nuetzlich sein.
Welches Protokoll sollte heute zum Login auf entfernten Rechnern verwendet werden?::SSH.
Wie kann man einen sicheren 'Tunnel' zu/von einem beliebigen (TCP-) Port einrichten?::Mit SSH Port Forwarding, lokal, remote oder dynamisch.
Wozu wird SNMP verwendet? Welches Transportprotokoll verwendet es? Was ist ein SNMP-Agent und eine MIB?::SNMP dient Monitoring und Management von Netzgeraeten und nutzt meist UDP. Ein Agent laeuft auf dem Geraet; die MIB beschreibt die verwalteten Objekte bzw. OIDs.
