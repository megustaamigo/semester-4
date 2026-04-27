---
course: kommunikationssysteme
type: selbstkontrolle
lecture: 10
date: 2026-04-27
tags:
  - kommunikationssysteme
  - selbstkontrolle
  - flashcards/kommunikationssysteme
---

# Fragen zur Selbstkontrolle — Vorlesung 10

Welche Eigenschaften haette ein ideales Mehrfachzugriffsprotokoll?::Es wuerde bei nur einem Sender die volle Kanalrate liefern, bei n aktiven Sendern fair etwa R/n, ohne Kollisionen, mit wenig Overhead und dezentral.
Nennen Sie 3 grundsaetzliche Strategien, wie der mehrfache Zugriff auf ein gemeinsames Medium geloest werden kann!::Kanalkonfektionierung/Kanalaufteilung, kontrollierter Zugriff und zufallsbasierter Zugriff.
Erklaeren Sie kurz die Funktionsweise von TDMA, FDMA und CDMA!::TDMA teilt Zeit in Slots, FDMA teilt Frequenzbaender, CDMA laesst alle gleichzeitig senden und trennt ueber verschiedene Spreizcodes.
Erklaeren Sie die Funktionsweise von CSMA/CD! Was bedeutet diese Abkuerzung?::Carrier Sense Multiple Access with Collision Detection: Vor dem Senden wird das Medium abgehoert; waehrend des Sendens wird auf Kollisionen geprueft, bei Kollision abgebrochen und spaeter erneut versucht.
Erklaeren Sie, warum bei CSMA/CD ein Sender eine Mindestzeit senden muss (und parallel per "CD" das Medium abhoeren muss)! Wie laesst sich diese Mindestzeit berechnen und ggf. in eine Mindest-Rahmengroesse umrechnen?::Eine Kollision muss noch waehrend des eigenen Sendens erkannt werden koennen; daher muss die Sendezeit mindestens der Round-Trip-Propagationszeit des Netzes entsprechen. Mindest-Rahmenlaenge = Bitrate mal dieser Mindestzeit.
Wie funktioniert und wozu dient der "Binary Exponential Backoff"-Algorithmus?::Nach jeder Kollision wartet die Station eine zufaellige Zahl von Slotzeiten aus einem mit der Kollisionszahl wachsenden Bereich; so sinkt die Wahrscheinlichkeit erneuter Kollisionen.
Warum wurde bei Gigabit-Ethernet die minimale Rahmenlaenge von 64 auf 520 Bytes erhoeht?::Weil bei deutlich hoeherer Bitrate die 64 Byte zu schnell gesendet waeren, um Kollisionen auf klassischen CSMA/CD-Strecken noch rechtzeitig zu erkennen.
Warum wurde bei Fast-Ethernet die Segmentlaenge von 2500m (10MBit Ethernet) auf 200-250m verkleinert?::Damit die Signallaufzeit trotz hoeherer Bitrate klein genug bleibt, um Kollisionen innerhalb der vorgeschriebenen Sendezeit noch zu erkennen.
Wie ist die Rahmen-Struktur eines Ethernet-Paketes aufgebaut? Welche Daten (-Felder) werden im Header und Trailer auf Schicht 2 uebertragen?::Auf der Leitung: Preamble, SFD, Ziel-MAC, Quell-MAC, optional VLAN, EtherType/Laenge, Nutzdaten mit Padding und im Trailer die FCS/CRC.
Erklaeren Sie die grundsaetzliche Vorgehensweise des Medienzugriffes beim Token-Ring Netz!::Ein spezielles Token kreist im Ring; nur wer das freie Token besitzt, darf senden. Danach gibt der Sender wieder ein freies Token in den Ring.
