---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 12
date: 2026-07-01
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - bituebertragungsschicht
  - leitungskodierung
  - modulation
  - wlan
  - flashcards/kommunikationssysteme
source: Kommunikationssysteme_19_Bituebertragungsschicht.pdf
---

# Fragen zur Selbstkontrolle — Vorlesung 12

Erklären Sie den Begriff Leitungskodierung!::Leitungskodierung (Schicht 1) ist die physikalische Darstellung von Digitalsignalen auf dem Medium — sie legt fest, wie eine 0 bzw. 1 als physikalisches Signal (z.B. Spannungspegel auf Kupfer) repräsentiert und in an den Kanal angepasste Codewörter (Symbole) überführt wird.

Welche Anforderungen kennen Sie, die ein guter Leitungscode erfüllen sollte?::Hohe Widerstandsfähigkeit gegen Dämpfung, Effizienz (hohe Übertragungsrate durch mehrwertige Codewörter), Taktrückgewinnung beim Empfänger (Synchronisation durch möglichst häufige Pegelwechsel) und Gleichstromfreiheit (positive und negative Signale treten etwa gleich oft auf).

Erklären Sie den Unterschied zwischen Basisband- und Breitband-Übertragung!::Basisband: die digitalen Informationen werden so über die Leitung übertragen wie sie sind (nur ein Signal gleichzeitig), nötig sind nur Kodierungsverfahren für 0/1. Breitband: die Signale werden auf eine Trägerwelle aufmoduliert, durch verschiedene Trägerfrequenzen können mehrere Informationen gleichzeitig übermittelt werden.

Wieso spricht man von Symbolen bei der physikalischen Übertragung und nicht von Bits?::Weil nicht die Datenbits direkt, sondern Symbolbits (Codeelemente) übertragen werden — ein Symbol kann mehrere diskrete Kennzustände annehmen (z.B. ternär/quaternär) und dadurch mehrere Datenbits gleichzeitig kodieren; die Zuordnung Daten→Symbole ist selbst eine Codierung.

Erklären Sie den Unterschied zwischen Bit- und Baudrate!::Die Baudrate (Schrittgeschwindigkeit) ist $v_s = 1/T$ mit T = Schrittdauer eines Symbols; die Bitrate ist $v_u = v_s \cdot \mathrm{ld}\,n$ mit n = Anzahl diskreter Kennzustände des Codeelements. Nur bei binären Codeelementen (n=2) stimmen Bit- und Baudrate überein.

Was ist der Unterschied zwischen einem NRZ- und einem RZ-Code?::NRZ (Non-Return-to-Zero) hält den Pegel über die gesamte Bitdauer konstant und kehrt nie auf Null zurück. RZ (Return-to-Zero) kehrt innerhalb eines Bits (nach T/2) wieder auf den Ruhepegel zurück, wodurch bei 1-Folgen Taktinformation entsteht.

Welche Eigenschaften besitzt der Manchester-Code? Wie unterscheiden sich die Standards nach G.E. Thomas und IEEE 802.3?::Manchester ist gleichstromfrei und selbsttaktend (Pegelwechsel in jeder Bitmitte), nutzt aber nur 50% der Kapazität (1B/2B, Taktrate = 2× Schrittgeschwindigkeit → höhere Bandbreite nötig). IEEE 802.3: 0 = Wechsel low→high (−5V→+5V bzw. −U_H→U_H), 1 = high→low. G.E. Thomas (Tanenbaum) definiert es genau umgekehrt: 0 = negativ→positiv, 1 = positiv→negativ. Umwandeln geht in beide Richtungen: aus Datenbits ergeben sich die zwei um 180° phasenverschobenen Signalelemente S1/S2, und aus der Richtung des Pegelwechsels in der Bitmitte liest man das Datenbit zurück.

Welche Vorteile hat ein 4B/5B Code gegenüber einem 1B/2B Code?::4B/5B kodiert 4 Datenbits in 5 Symbolbits → 80% Effizienz statt nur 50% beim 1B/2B-Code (Manchester). Durch Auswahl der günstigsten 16 von 32 Codewörtern (max. 3 Nullen in Folge) bleibt die Taktrückgewinnung möglich, und die überzähligen Codewörter dienen als Steuerinformation.

Welche Eigenschaften eines Trägersignals können zur Modulation verwendet werden?::Die drei Parameter der Sinuswelle $s(t)=A\cdot\sin(2\pi f t + \varphi)$: Amplitude A (ASK), Frequenz f (FSK) und Phase φ (PSK) — sowie Kombinationen davon (QAM).

Welche Möglichkeiten kennen Sie, um bei einer Breitbandübertragung die Datenrate zu erhöhen?::Höherwertige Modulation, bei der pro Symbol mehrere Bits kodiert werden — z.B. mehr Phasenlagen (QPSK bzw. M-PSK, wobei M eine Zweierpotenz sein muss) oder Kombination von Amplitude und Phase (QAM), sodass n Bit gleichzeitig übertragen werden.

Warum ist PSK weniger störanfällig als z.B. ASK?::ASK trägt die Information in der Amplitude, die durch Dämpfung/Abschwächung des Signals leicht verfälscht wird. PSK kodiert die Information in der Phasenlage, die gegenüber Amplitudenstörungen robust ist — daher in der drahtlosen Kommunikation oft bevorzugt.

Wie unterscheiden sich QPSK und QAM?::QPSK nutzt 4 Phasenlagen (2 Bit/Symbol) bei konstanter Amplitude — reine Phasenmodulation. QAM kombiniert ASK und QPSK (Amplitude + Phase) und kann so n Bit gleichzeitig übertragen (QPSK ist der Spezialfall n=2); QAM hat bei zunehmendem n eine geringere Bitfehlerrate als vergleichbare reine PSK-Verfahren.

Welche Störeinflüsse gibt es bei der Datenübertragung mit Funkwellen?::Dämpfung/Abschattung (Regen, Vegetation, Gebäude), Reflexion an großen Flächen, Streuung an kleinen Hindernissen, Beugung an Kanten und Brechung; Folge ist Mehrwegausbreitung — dasselbe Signal trifft mit verschiedenen Phasenlagen ein (Interferenz) und wird zeitlich gestreut (Interferenz mit Nachbarsymbolen). Zudem sinkt die Empfangsleistung proportional zu 1/dᵅ.

Worin unterscheidet sich ein kabelgebundenes gemeinsames Medium (z.B. ein Bus bei 10Base2) von einem funkbasierten 'gemeinsamen' Medium (Luftraum bei WLAN)?::Beim Kabelbus empfangen alle Stationen dasselbe Signal, Kollisionen sind überall erkennbar (CD möglich). Beim Funk-Medium reicht das Signal je Station unterschiedlich weit ("Broadcastnetz ohne garantierten Empfang aller Übertragungen"): eine Kollision tritt beim Empfänger auf, muss aber nicht beim Sender bemerkbar sein → Hidden/Exposed Station; gleichzeitiges Senden und Empfangen ist teuer, daher keine Kollisionserkennung.

Wie groß sind typische Übertragungsraten beim WLAN? Welche Frequenzbänder werden benutzt?::Brutto von 2 Mb/s (802.11) über 11 Mb/s (802.11b) und 54 Mb/s (802.11a/g) bis ~600 Mb/s (802.11n, MIMO); die Nutzdatenrate ist nur etwa die Hälfte der Bruttorate. Frequenzbänder: lizenzfreies 2.4 GHz-ISM-Band (2.4–2.4835 GHz) und optional 5 GHz-Band.

Warum ist das CSMA/CD-Verfahren bei WLAN nur schwer anwendbar?::CD (Collision Detection) erfordert gleichzeitiges Senden und Empfangen (Abhören während der Übertragung), was drahtlos sehr schwierig/teuer ist. Zudem tritt eine Kollision beim Empfänger auf, nicht notwendigerweise beim Sender (Hidden Station) — der Sender kann sie also gar nicht erkennen.

Erklären Sie das 'Hidden Station'-Problem bei WLAN!::A sendet an B, C empfängt A nicht. C will ebenfalls an B senden und stellt per Carrier Sense ein freies Medium fest (CS schlägt fehl), sendet los → Kollision bei B, die A nicht bemerkt (CD schlägt fehl). A ist für C "versteckt" (hidden).

Erklären Sie das 'Exposed-Station'-Problem bei WLAN!::B sendet an A, C will an D senden. C stellt per Carrier Sense ein besetztes Medium fest und wartet — obwohl dies unnötig ist, da A außerhalb der Reichweite von C liegt und das Signal von C bei A ohnehin im Rauschen unterginge. C ist unnötig "exposed" und verschenkt eine mögliche parallele Übertragung.

Erklären Sie grob das Vorgehen bei CSMA/CA! Wie werden Kollisionen verhindert?::Vor dem Senden Carrier Sense: ist das Medium mindestens für DIFS frei, wird direkt gesendet. War es belegt, wird nach dem Freiwerden erneut DIFS gewartet und dann eine zufällige Backoff-Zeit (Vielfaches eines Zeitslots) gewürfelt; der Backoff-Zähler wird bei erneuter Belegung angehalten und beim nächsten Versuch fortgesetzt. Ist das Medium nach Ablauf noch frei, wird gesendet. Jeder korrekte Datenrahmen wird nach SIFS (ohne Backoff) mit einem ACK quittiert. Kollisionen werden durch den zufälligen Backoff und optional durch einen RTS/CTS-Handshake vermieden (adressiert das Hidden-Station-Problem).
