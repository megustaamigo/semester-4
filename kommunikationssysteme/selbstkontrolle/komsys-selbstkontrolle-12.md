---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 12
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 12

Erklaeren Sie den Begriff Leitungskodierung!::Leitungskodierung beschreibt, wie Bits oder Symbolfolgen als physikalische Signalverlaeufe auf dem Uebertragungsmedium dargestellt werden.
Welche Anforderungen kennen Sie, die ein guter Leitungscode erfuellen sollte?::Er sollte Taktrueckgewinnung ermoeglichen, wenig Gleichanteil haben, effizient sein, robust gegen Stoerungen und moeglichst wenig Bandbreite benoetigen.
Erklaeren Sie den Unterschied zwischen Basisband- und Breitband-Uebertragung!::Basisband uebertraegt das Nutzsignal direkt im Grundband, Breitband moduliert es auf einen oder mehrere Traeger.
Wieso spricht man von Symbolen bei der physikalischen Uebertragung und nicht von Bits?::Weil physikalisch Zustandsaenderungen oder Signalzustandskombinationen uebertragen werden; ein Symbol kann je nach Modulation ein oder mehrere Bits repraesentieren.
Erklaeren Sie den Unterschied zwischen Bit- und Baudrate!::Bitrate ist die Zahl uebertragener Bits pro Sekunde, Baudrate die Zahl uebertragener Symbole pro Sekunde.
Was ist der Unterschied zwischen einem NRZ- und einem RZ-Code?::Bei NRZ bleibt das Signal waehrend des ganzen Bitintervalls auf einem Pegel, bei RZ kehrt es innerhalb des Bitintervalls auf einen Grundpegel zurueck.
Welche Eigenschaften besitzt der Manchester-Code? Wie unterscheiden sich die Standards nach G.E. Thomas und IEEE 802.3? Sie koennen Datenbits in Manchester-Signale darstellen und umgekehrt.::Manchester hat immer eine Pegelaenderung in der Bitmitte und ist dadurch selbsttaktend, braucht aber mehr Bandbreite. Thomas und IEEE 802.3 verwenden entgegengesetzte Zuordnungen der Flankenrichtung zu 0 und 1.
Welche Vorteile hat ein 4B/5B Code gegenueber einem 1B/2B Code?::Er hat deutlich weniger Overhead und garantiert trotzdem genug Signalwechsel fuer Taktrueckgewinnung.
Welche Eigenschaften eines Traegersignals koennen zur Modulation verwendet werden?::Amplitude, Frequenz und Phase.
Welche Moeglichkeiten kennen Sie, um bei einer Breitbanduebertragung die Datenrate zu erhoehen?::Hoehere Symbolrate, mehr Bits pro Symbol durch hoehere Modulationsordnung, mehrere Kanaele/Frequenzen oder mehrere parallele Raumpfade.
Warum ist PSK weniger stoeranfaellig als z.B. ASK?::Weil Amplitudenstoerungen das Signal bei PSK weniger direkt verfalschen; die Information steckt in der Phase statt im Pegel.
Wie unterscheiden sich QPSK und QAM?::QPSK nutzt vier Phasenlagen mit konstanter Amplitude; QAM kombiniert Phase und Amplitude und kann daher mehr Bits pro Symbol tragen.
Welche Storeinfluesse gibt es bei der Datenuebertragung mit Funkwellen?::Rauschen, Daempfung, Mehrwegeausbreitung/Fading, Interferenzen, Abschattung und Stoerquellen anderer Sender.
Worin unterscheidet sich ein kabelgebundenes gemeinsames Medium (z.B. ein Bus bei 10Base2) von einem funkbasierten 'gemeinsamen' Medium (Luftraum bei WLAN)?::Beim Kabel ist das Medium physisch begrenzt und fuer alle Teilnehmer aehnlich sichtbar; im Funk ist die Reichweite ortsabhaengig, asymmetrisch und durch Hidden/Exposed Stations schwieriger einzuschaetzen.
Wie gross sind typische Uebertragungsraten beim WLAN? Welche Frequenzbaender werden benutzt?::Je nach Standard von einigen Mbit/s bis in den Gbit/s-Bereich; genutzt werden vor allem 2,4 GHz, 5 GHz und zunehmend 6 GHz.
Warum ist das CSMA/CD-Verfahren bei WLAN nur schwer anwendbar?::Weil eine Station waehrend des Sendens kaum zuverlaessig gleichzeitig empfangen kann und weil sie moegliche Kollisionen wegen Hidden Stations oft gar nicht mitbekommt.
Erklaeren Sie das 'Hidden Station'-Problem bei WLAN!::Zwei Sender hoeren sich gegenseitig nicht, funken aber beide zu derselben Basisstation; fuer den Access Point kollidieren die Rahmen.
Erklaeren Sie das 'Exposed-Station'-Problem bei WLAN!::Eine Station verzichtet auf Senden, weil sie einen anderen Sender hoert, obwohl ihre eigene Uebertragung einen anderen Empfaenger gar nicht stoeren wuerde.
Erklaeren Sie grob das Vorgehen bei CSMA/CA! Wie werden Kollisionen verhindert?::Eine Station hoert erst das Medium ab, wartet bei Frei-Zustand DIFS plus zufaelligen Backoff und sendet dann; optionale RTS/CTS-Reservierung und ACKs helfen, Kollisionen besonders bei Hidden Stations zu vermeiden.
