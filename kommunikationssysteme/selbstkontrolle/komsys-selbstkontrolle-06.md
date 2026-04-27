---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 6
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 6

Wie identifiziert TCP ein Segment bzw. ein ACK? In welcher Einheit wird dies spezifiziert?::Ueber Sequence Number und Acknowledgment Number; beide beziehen sich auf Bytestream-Positionen, also Bytes, nicht auf Paketnummern.
Wird bei jedem TCP-Paket ein ACK mitgesendet?::Nein. ACKs koennen verzoegert oder mit Nutzdaten kombiniert werden; reine ACK-Pakete entstehen nur bei Bedarf.
Warum muss der Empfaenger seine aktuelle Fenstergroesse ('advertised window') dem Sender mitteilen? Wie?::Damit der Sender nicht mehr Daten schickt als der Empfaenger puffern kann. Die freie Kapazitaet wird im Window-Feld jedes TCP-Headers mitgeteilt.
Erklaeren Sie auf Empfaengerseite den Unterschied zwischen Empfangspuffer-Groesse und aktueller Fenster-Groesse!::Die Empfangspuffer-Groesse ist die gesamte reservierte Pufferkapazitaet; die aktuelle Fenster-Groesse ist der momentan noch freie Teil davon.
Unter welchen Bedingungen wird in TCP ein ACK versendet?::Bei neu empfangenen Daten, bei Ankunft ausser der Reihe oft als Duplicate ACK, und spaetestens nach einem kurzen Delayed-ACK-Timeout, falls kein Piggyback moeglich ist.
Welche Strategien zur Fehlerbehebung verwendet TCP? (Go-Back-N? Selective Repeat? Selective Reject?)::Klassisches TCP arbeitet mit kumulativen ACKs und verhaelt sich damit eher wie Go-Back-N; mit SACK kommt selektives Wiederholen hinzu. Selective Reject als eigenes Protokollflag ist nicht Standard-TCP.
Wozu dient der Retransmission Timer? Wie wird seine Laenge bestimmt?::Er startet nach dem Senden unbestaetigter Daten und loest bei ausbleibendem ACK eine Wiederholung aus. Seine Dauer wird adaptiv aus geschaetztem RTT und dessen Streuung berechnet.
Was ist ein 'Congestion Collapse'? Wie wird er hervorgerufen?::Ein Zustand, in dem wegen Ueberlastung fast nur noch Retransmissions unterwegs sind und der gute Durchsatz zusammenbricht. Ausgeloest wird er durch zu aggressive Sender bei ueberfuellten Routern und Verlusten.
Welche Einflussgroessen steuern die Bandbreite des Senders bei TCP?::Vor allem Congestion Window, Advertised Window und RTT; effektiv sendbar ist grob min(cwnd, rwnd) pro RTT.
Was versteht man bei TCP unter 'Slow Start'?::Eine Start- bzw. Wiederanlaufphase, in der das Congestion Window zunaechst klein ist und pro RTT ungefaehr exponentiell waechst, bis Verluste oder ein Schwellwert erreicht werden.
Wie funktioniert der Verbindungsaufbau bei TCP? Welcher Begriff wird zur Beschreibung des Verfahrens verwendet?::Per Three-Way Handshake: SYN, SYN-ACK, ACK. Dabei synchronisieren beide Seiten Sequenznummern und Parameter.
Wie funktioniert der Verbindungsabbau bei TCP?::Ueblicherweise mit FIN und ACK in beiden Richtungen; daher entsteht meist ein Four-Way-Teardown mit einer TIME-WAIT-Phase auf einer Seite.
Wozu dient der Keepalive-Timer? Wann ist dieser Timer wichtig?::Er prueft bei langen Leerlaufphasen, ob die Gegenseite oder der Pfad noch lebt. Wichtig ist er bei stillen Verbindungsabbruechen, z.B. hinter NAT oder nach Host-Absturz.
Welche Datenfelder enthaelt der TCP-Header? Wozu werden sie jeweils verwendet?::Ports fuer Prozesse, Sequenz- und ACK-Nummern fuer Ordnung und Bestaetigung, Data Offset fuer Headerlaenge, Flags fuer Steuerung, Window fuer Flusskontrolle, Checksum fuer Fehlererkennung, Urgent Pointer fuer URG-Daten und Optionen z.B. MSS oder Window Scale.
Wie funktioniert die (haeufig eingesetzte) Window-Scale Option in TCP? Wann wird sie benoetigt? Welchen Nachteil sehen Sie hier?::Beim Handshake wird ein Skalierungsfaktor vereinbart, mit dem das 16-Bit-Window-Feld logisch nach links verschoben wird. Benoetigt wird das bei grossen Bandwidth-Delay-Produkten; Nachteil ist mehr Komplexitaet und moegliche Inkompatibilitaet mit alten oder fehlerhaften Implementierungen.
Was versteht man unter dem 'Silly Window'-Syndrom? Wie kann es geloest werden?::Es entstehen viele winzige Segmente bzw. Fensterupdates, was unnoetigen Overhead verursacht. Gegenmittel sind Clark-Loesung auf Empfaengerseite und Nagle-Algorithmus auf Senderseite.
Wozu dienen die PSH und URG-Flaggen im TCP Header?::PSH fordert die zuegige Weitergabe der empfangenen Daten an die Anwendung; URG markiert zusammen mit dem Urgent Pointer dringende Daten innerhalb des Streams.
