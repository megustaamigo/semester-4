---
course: data-science
type: selbstkontrolle
lecture: 1
date: 2026-04-02
tags:
  - data-science
  - selbstkontrolle
  - grundbegriffe
  - flashcards/data-science
source: 01_Grundbegriffe.pdf
---

# Selbstkontrolle — 01 Überblick & Grundbegriffe

## Fragen

Was ist das (abstrakte) Ziel von Data Science?::Extraktion von Wissen aus Daten.

Im Schnitt welcher drei Gebiete befindet sich Data Science?::Mathematik/Statistik, Informatik und Fachwissen (Domänenwissen).

Aus welchen Schritten besteht eine "typische" Data Science Pipeline?::1. Datenbeschaffung/-aufbereitung → 2. Explorative Analyse (EDA) → 3. Feature Engineering → 4. Modellierung & Prädiktion.

Was ist ein typischer Schritt der Datenbeschaffung/-aufbereitung?::Daten aus heterogenen Quellen sammeln (APIs, Datenbanken, Webscraping), bereinigen (Fehler, Duplikate, fehlende Werte entfernen), anonymisieren und in ein geeignetes Format transformieren (ETL-Prozess).

Was ist ein typischer Schritt der explorativen Datenanalyse?::Daten visualisieren, deskriptive Statistiken berechnen, Hypothesen aufstellen und mit statistischen Tests prüfen. Zyklischer Prozess: Beobachten → Hypothesen entwickeln → Testen → erneut beobachten.

Was ist ein typischer Schritt des Feature Engineerings?::Merkmale (Features) identifizieren oder konstruieren, die die Daten bezogen auf die Fragestellung gut beschreiben — z.B. beim MNIST-Datensatz Schwärze und Asymmetrie als Features berechnen. Nutzt Erkenntnisse aus der EDA und Domänenexpertise.

Was ist ein typischer Schritt der Modellierung?::Ein Modell wählen (z.B. lineare Regression, Entscheidungsbaum, neuronales Netz), Parameter aus Trainingsdaten lernen und Vorhersagen treffen.

Wie unterscheiden sich Data Scientist und Data Engineer?::Der Data Engineer kümmert sich um IT-Infrastruktur, Datenspeicherung und -bereitstellung (ETL). Der Data Scientist analysiert, visualisiert, modelliert die Daten und extrahiert daraus Erkenntnisse und Vorhersagen.

Ist die explorative Datenanalyse ein linearer Prozess? Und die Data Science Pipeline?::Die EDA ist ein zyklischer Prozess (Beobachten → Hypothesen → Testen → Beobachten). Auch die gesamte Pipeline ist nicht streng linear — Erkenntnisse aus späteren Schritten können Rücksprünge zu früheren Schritten erfordern.

Was ist der Unterschied zwischen klassischer Software und maschinellem Lernen?::Klassische Software: Programmierer schreibt Regeln, Computer wendet sie auf Daten an → Ergebnis. Maschinelles Lernen: Computer erhält Daten + Ergebnisse und lernt daraus ein Programm (Modell), das dann auf neue Daten angewendet wird (Inferenz).
