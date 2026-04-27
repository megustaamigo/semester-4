---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 11
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 11

Was ist ein Hamming-Abstand bei einem Code C mit den Woertern c1 bis cn?::Die Anzahl der Bitpositionen, in denen sich zwei Codewoerter unterscheiden; fuer einen Code betrachtet man oft den minimalen Abstand aller Wortpaare.
Was versteht man unter "Forward Error Correction"?::Fehlerkorrektur durch Redundanz im Codewort, sodass der Empfaenger bestimmte Fehler selbst erkennen und korrigieren kann, ohne Neuuebertragung.
Wie gross muss der Hamming-Abstand mindestens sein, um e-Bitfehler zu erkennen bzw. zu beheben?::Zum Erkennen von e Fehlern braucht man d_min >= e + 1, zum Korrigieren von e Fehlern d_min >= 2e + 1.
Nennen Sie einen Code, der mit einer minimalen Anzahl von Pruefbits Einzelbitfehler korrigieren kann!::Ein Hamming-Code.
Geben Sie einen Code an, der 2-Bit Fehler beheben kann.::Ein Code mit Mindestabstand 5, z.B. ein BCH-Code oder allgemein ein geeigneter 2-Fehler-korrigierender Blockcode.
Was versteht man unter Modulo-2 Arithmetik? Wie werden Addition und Subtraktion durchgefuehrt?::Rechnen ueber den Bits 0 und 1 ohne Uebertrag; Addition und Subtraktion entsprechen beide dem XOR.
Was leistet bzw. wie funktioniert das CRC-Verfahren? Wozu dient das Generator-Polynom? Wo wird das Verfahren im Kontext von Ethernet eingesetzt?::CRC erkennt Burstfehler, indem Daten als Polynom ueber GF(2) interpretiert und durch ein Generatorpolynom dividiert werden; der Rest wird angehaengt. In Ethernet steckt die CRC als FCS im Trailer.
Wieso passt das CRC-Verfahren gut zum Framing der Schicht 2?::Weil ein kompletter Rahmen am Ende ohnehin abgeschlossen ist und die FCS daher bequem als Trailer angehaengt und ueber den ganzen Rahmen geprueft werden kann.
Wie muss ich mir Flusskontrolle und Sliding Window auf der Schicht 2 vorstellen?::Auch auf Schicht 2 kann ein Sender nur eine begrenzte Zahl unbestaetigter Rahmen im Flug haben; ACKs oder Credits verschieben das Fenster und verhindern das Ueberlaufen des Empfaengers.
