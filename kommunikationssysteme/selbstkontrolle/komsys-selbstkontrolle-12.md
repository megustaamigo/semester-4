---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 12
date: 2026-05-12
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - tcp
  - sliding-window
  - flusskontrolle
  - staukontrolle
  - three-way-handshake
  - flashcards/kommunikationssysteme
source: Kommunikationssysteme_12_TCP.pdf
---

# Selbstkontrolle 12 — TCP: Ein Sliding-Window-Protokoll

**Quelle:** [[kommunikationssysteme/resources/Kommunikationssysteme_12_TCP.pdf|Kommunikationssysteme_12_TCP.pdf]]

## Fragen

Was implementiert TCP als Basisprotokoll fuer die Flusskontrolle?::TCP implementiert ein **Sliding-Window-Protokoll**: Bei einer Fenstergroesse $n$ koennen $n$ **Bytes** verschickt werden, ohne dass ein ACK empfangen werden muss. Sobald der Empfang bestaetigt wurde, verschiebt sich das Fenster und neue Daten koennen gesendet werden.

Wie nummeriert TCP Segmente?::Zyklisch mit 32 Bit. Segmente werden durch ihren **Byte-Offset im Stream** identifiziert. Die Startposition wird beim Verbindungsaufbau zufaellig festgelegt.

Was bedeutet kumulatives ACK in TCP?::ACK $n+1$ sagt aus, dass **alle Daten von der vorigen logischen Position bis zur Position $n$** korrekt empfangen wurden und nun das Segment $n+1$ erwartet wird. Ein einzelnes ACK kann also viele Segmente auf einmal bestaetigen.

Was ist Selective Repeat in TCP?::Der Empfaenger puffert auch ausserhalb der Reihenfolge empfangene Segmente. Da TCP keine echten NAKs hat, werden mehrfach gleiche kumulative ACKs als NAK angesehen → **Triple-Duplicate-ACK** triggert einen **Fast Retransmit** bereits vor dem Timeout.

Was ist das Advertised Window (Receiver Window)?::Jede Bestaetigung enthaelt das Feld **Window**, das den **aktuell freien Platz im Empfangspuffer** angibt. Berechnung: `AdvertisedWindow = Empfangspuffer - ((NextByteExpected - 1) - LastByteRead)`. Das Sliding Window des Senders wird dadurch beschraenkt.

Wie berechnet sich das Effective Window auf Senderseite?::`EffectiveWindow = AdvertisedWindow - (LastByteSent - LastByteAcked)`. Es darf gelten: `LastByteSent - LastByteAcked < AdvertisedWindow`.

Was ist die Huckepack-Technik (Piggybacking)?::Bestaetigungen koennen auf dem Datenpaket der **Gegenrichtung** "mitreiten". Da TCP bidirektional ist, wird so eigenstaendige ACK-Pakete vermieden — eine Datennachricht bestaetigt zugleich frueher empfangene Segmente.

Was sind Delayed Acknowledgments?::Liegen keine Daten in Gegenrichtung an, werden ACKs bis zu **500 ms** verzoegert, in der Hoffnung, dass noch Daten kommen, mit denen man huckepack bestaetigen kann. Bestaetigungen werden ohnehin spaetestens fuer jedes **zweite** Segment versendet.

Was tut der Empfaenger bei Ankunft eines erwarteten Segments?::Wenn vorherige Daten schon bestaetigt sind: Delayed ACK (bis 500 ms warten). Falls vorheriges Segment noch nicht bestaetigt: sofortige kumulative Bestaetigung beider Segmente.

Was tut der Empfaenger, wenn ein Segment hinter der erwarteten Sequenznummer eintrifft?::Sofortiges Verschicken eines **Duplicate ACK (DupACK)**, das die naechste erwartete Sequenznummer angibt. Drei DupACKs in Folge triggern beim Sender den Fast Retransmit.

Wie schaetzt TCP die RTT?::TCP misst die Zeit zwischen Senden eines Segments und Empfang seines ACKs (**SampleRTT**), wobei Neuuebertragungen und Delayed ACKs ausgeblendet werden. Der SampleRTT wird **geglaettet** (Tiefpass), so dass die Schaetzung eine Mittelung ueber die Vergangenheit ist.

Wieso ist Reaktion auf Ueberlast durch Reduktion der Datenrate sinnvoll?::Bei Timeout werden viele bereits gesendete Segmente neu uebertragen (Go-Back-N/Selective Repeat). Wenn viele Verbindungen das gleichzeitig tun, werden Router noch weiter ueberlastet (**Congestion Collapse**) und verwerfen mehr Pakete → noch mehr Timeouts. Drosselung der Senderate bricht diese positive Rueckkopplung.

Was ist der Unterschied zwischen Flusskontrolle und Staukontrolle?::**Flusskontrolle** regelt den Datenfluss zwischen den **Endpunkten** (Empfaengerpuffer-Limit) — Sliding-Window-basiert. **Staukontrolle** befasst sich mit **Ueberlastsituationen in Routern** (Zwischensystemen) und drosselt zusaetzlich anhand des Congestion Window.

Was ist das Congestion Window (cwnd)?::Eine zusaetzliche Beschraenkung des verfuegbaren Fensters auf **Senderseite** — meist in Vielfachen der MSS gefuehrt. Es wird vom Sender selbst verwaltet und durch Slow-Start und Congestion Avoidance veraendert, um Ueberlastsituationen zu vermeiden und Fairness zu erzielen.

Was ist die Slow-Start-Phase?::Anfangsphase: cwnd startet bei **1 MSS** und wird bei jedem erfolgreichen ACK um 1 MSS erhoeht → bei Bestaetigung eines vollen Fensters verdoppelt sich cwnd pro RTT (**exponentielles Wachstum**). Endet, wenn ssthresh oder Receiver Window erreicht ist.

Wann wechselt TCP von Slow-Start zu Congestion Avoidance?::Wenn cwnd den Schwellenwert **ssthresh** erreicht. Ab da Additive Increase: cwnd waechst nur noch um etwa 1 MSS pro RTT (vorsichtige Annaeherung an die Netzkapazitaet).

Was passiert bei einem Timeout in TCP?::ssthresh wird auf cwnd/2 gesetzt, cwnd zurueck auf 1 MSS (Slow Start), und das verlorene Segment wird neu uebertragen (Go-Back-N bzw. Selective Repeat).

Was sind Fast Retransmit und Fast Recovery?::Bei **drei DupACKs** erfolgt sofortige Neuuebertragung des fehlenden Segments **vor dem Timeout** (Fast Retransmit). Da das Netz noch Pakete uebertraegt, wird kein Slow Start ausgeloest, sondern cwnd nur halbiert (**Multiplicative Decrease**) — Fast Recovery.

Was schafft TCP Staukontrolle bzgl. mehrerer Verbindungen?::**Fairness**: Wird eine Leitung von $N$ TCP-Verbindungen genutzt, erhaelt jede etwa $1/N$ der Bandbreite.

Wie laeuft der TCP-Verbindungsaufbau ab (Three-Way-Handshake)?::1) Client → Server: `SYN, SEQ=x`. 2) Server → Client: `SYN, SEQ=y, ACK=x+1` (kombinierte SYN+ACK-Nachricht). 3) Client → Server: `ACK=y+1, SEQ=x+1`. Sequenznummern sind zufaellig, MSS wird ausgehandelt (Minimum beider Seiten).

Wie laeuft der TCP-Verbindungsabbau ab?::4-Way-Handshake: Jede Richtung wird separat per FIN signalisiert und mit ACK bestaetigt. Die Seite, die zuerst FIN sendet, signalisiert nur "ich sende keine Daten mehr", kann aber noch empfangen → die Gegenseite kann noch Daten senden und schliesst erst spaeter mit eigenem FIN.

Warum 4-Way statt 3-Way beim Verbindungsabbau?::Beim Aufbau koennen SYN und ACK in einem Paket kombiniert werden, da beide Seiten **gleichzeitig** aufbauen wollen. Beim Abbau ist das nicht immer moeglich — die Seite, die zuerst FIN sendet, will nur **selbst** abschliessen, die andere Seite kann noch Daten senden.

Welche Felder hat ein TCP-Segment-Header?::Source Port, Destination Port, Sequence Number, Acknowledgement Number, Data Offset, Reserved, Code (URG/ACK/PSH/RST/SYN/FIN), Window, Checksum, Urgent Pointer, Options, Data.

Was bedeuten die Code-Bits URG, ACK, PSH, RST, SYN, FIN?::URG: Urgent Pointer ist gueltig. ACK: Acknowledgement Field ist gueltig. PSH: Empfaenger soll Daten sofort ausliefern (Push). RST: Verbindung zuruecksetzen. SYN: Synchronize Sequence Numbers (Verbindungsaufbau). FIN: Sender hat Ende seines Byte-Streams erreicht.

Welche typischen MSS-Werte gibt es?::1460 Byte (Ethernet) oder 536 Byte. Die MSS wird so gewaehlt, dass **IP-Fragmentierung vermieden** wird. Die MSS haengt vom Betriebssystem und der TCP-Konfiguration ab.
<!--SR:!2026-05-22,1,230-->

Was ist das Push-Flag (PSH)?::Damit der Empfaenger seinen Puffer nicht unnoetig fuellt: Ein gesetztes PSH-Flag bedeutet, dass die Daten **direkt beim naechsten read** ausgeliefert werden sollen, ohne Pufferung. Eine Terminal-Emulation setzt PSH bei CRLF. In der Praxis ist ein **Flush auf den Output-Stream** ein guter Ansatz.

Was ist der Nagle-Algorithmus?::Limitiert die Anzahl "kleiner" Segmente. Solange ein unbestaetigtes kleines TCP-Segment unterwegs ist, werden keine weiteren kleinen Segmente gesendet — Daten werden im Sendepuffer gesammelt, bis entweder ein ACK eintrifft **oder** ein volles MSS-Segment fuellbar ist. Deaktivierbar mit der Socket-Option **TCP_NODELAY**. Achtung: Nagle und Delayed-Ack behindern sich gegenseitig.

Was ist der Urgent-Pointer (URG)?::Wenn URG-Flag gesetzt ist, interpretiert der Empfaenger den Urgent-Pointer als Offset im Segment, ab dem die wichtigen Daten beginnen. Diese werden **vor** etwaig gepufferten Daten ausgeliefert. Anwendung: interaktive Anwendungen koennen Signale (z.B. Ctrl-C) "ueberholen". API: `socket.sendUrgentData(int data)`.

Was ist das Bandwidth-Delay-Produkt-Problem?::Das TCP-Window-Feld ist nur **16 Bit** breit → maximal 64 kB Uebertragungsfenster. Auf Long-Fat-Pipes (hohe Bandbreite, hohe RTT) reicht das nicht aus. Das Bandwidth-Delay-Produkt gibt die noetige Fenstergroesse bei gegebener Bandbreite und Round-Trip-Zeit an.

Was ist die Window-Scale-Option?::TCP-Option, die einen logarithmischen Skalierungsfaktor (0–14) fuer das 16-Bit-Window-Feld festlegt. Wird nur im SYN-Segment beim Verbindungsaufbau ausgetauscht (in **beiden Richtungen separat**). Mit Wert 14 ergibt sich die maximale Fenstergroesse $65535 \cdot 2^{14} \approx 1{,}07$ GB. TCP-intern wird das Window dann als 30-Bit-Zahl verwaltet.

Welche TCP-Varianten zur Staukontrolle kennen Sie?::Linux unterstuetzt u.a. BIC, **CUBIC** (Default), Westwood, HTCP, HSTCP, Hybla, Vegas, Scalable, LP, Veno, Yeah, Illinois. Die aktive Variante kann ueber `/boot/config-$(uname -r)` ueberprueft werden.

Was ist Self-Clocking bei TCP?::Die Senderate wird durch das Ankommen von ACKs getaktet — der Sender injiziert neue Daten genau dann, wenn er ein ACK erhaelt. Dadurch passt sich TCP automatisch an die Engstelle des Pfads an (sichtbar an der Sequence-Number-Kurve, die sich der Bandbreite anschmiegt).
