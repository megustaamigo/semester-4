---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 5
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 5

Nennen Sie die 2 wichtigen Transportprotokolle im Internet! Wie unterscheiden sie sich?::TCP und UDP. TCP ist verbindungsorientiert und zuverlaessig mit Fluss- und Staukontrolle; UDP ist verbindungslos, leichtgewichtig und liefert nur Datagramme ohne Garantien.
Was bedeutet eine 'virtuelle Verbindungsorientierung'?::Es gibt keinen festen physikalischen Kanal, aber das Protokoll laesst fuer die Anwendung eine logische Verbindung mit Auf- und Abbau, Reihenfolge und Zustandsverwaltung entstehen.
Was versteht man unter Segmentierung eines Streams?::Ein kontinuierlicher Bytestrom wird in handhabbare Segmente zerlegt, die einzeln nummeriert, uebertragen und bestaetigt werden koennen.
Erklaeren Sie das ARQ-Protokoll. Wo und warum werden hier Timeouts eingesetzt?::ARQ bestaetigt empfangene Daten mit ACKs und sendet bei Verlust oder Fehlern erneut. Timeouts laufen auf Senderseite, damit fehlende ACKs bzw. verlorene Segmente erkannt und retransmittiert werden.
Warum werden Sequenznummern benoetigt?::Damit Sender und Empfaenger Daten eindeutig identifizieren, Reihenfolge rekonstruieren, Verluste erkennen und Duplikate verwerfen koennen.
Wie ist der Nutzungsgrad einer Verbindung definiert? Warum gibt es bei Verbindungen mit hoher RTT hier Probleme?::Er ist der Anteil der Zeit, in der wirklich Nutzdaten uebertragen werden. Bei hoher RTT wartet der Sender bei Stop-and-Wait lange auf ACKs und die Leitung bleibt die meiste Zeit ungenutzt.
Wie versucht man dieses Problem zu umgehen und welches Problem verursacht man hierbei?::Man sendet mehrere Segmente gleichzeitig per Pipelining/Sliding Window. Das verbessert die Auslastung, erfordert aber Puffer, komplexere Fehlerbehandlung und Umgang mit out-of-order Segmenten.
Wie loest das Go-Back-N Verfahren Uebertragungsfehler?::Fehlt ein Segment oder tritt ein Fehler auf, bestaetigt der Empfaenger nur bis vor die Luecke; nach Timeout sendet der Sender das fehlende Segment und alle danach bereits gesendeten Segmente erneut.
