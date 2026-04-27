---
course: data-science
type: selbstkontrolle
lecture: 3
date: 2026-04-24
tags:
  - data-science
  - selbstkontrolle
  - eda
  - visualisierung
  - flashcards/data-science
source: 03_Univariate_EDA.pdf
---

# Selbstkontrolle 03 — Eindimensionale EDA und Visualisierungen

**Quelle:** [[data-science/resources/03_Univariate_EDA.pdf|03_Univariate_EDA.pdf]]

## Fragen

Was sind die Ziele der explorativen Datenanalyse?::Identifikation von Problemen im Datensatz, Einschaetzung zu Annahmen in Daten, Auswahl passender statistischer Methoden und Hypothesenbildung ueber Daten.

Was sind "typische Werkzeuge" der EDA?::Deskriptive Statistik, Visualisierung und Dokumentation des Erkenntniswegs (fuer Reproduzierbarkeit, Kommunikation und Steuerung der Analyse).

Wie unterscheiden sich deskriptive und Inferenzstatistik?::Deskriptive Statistik beschreibt die vorliegende Stichprobe (z.B. Mittelwert, Varianz). Inferenzstatistik schliesst von der Stichprobe auf die unbekannte Population (schliessende Statistik via Wahrscheinlichkeitstheorie).

Was sind Beispiele fuer typische Lagemasse? Und Streumasse?::Lagemasse: Mittelwert, Median, Modus, Quantile. Streumasse: Varianz, Standardabweichung, Interquartilsabstand (IQR). Lagemasse verschieben sich bei Translation der Daten um $c$, Streumasse bleiben invariant.

Wie bestimmen wir Quantile?::Ueber die Ordnungsstatistik: Falls $n \cdot p \in \mathbb{N}$, ist $q_p = \frac{1}{2}(x_{(n \cdot p)} + x_{(n \cdot p + 1)})$. Falls $n \cdot p \notin \mathbb{N}$, ist $q_p = x_{(\lfloor n \cdot p + 1 \rfloor)}$.

Was sind Median, unteres und oberes Quartil?::Median ist das 50%-Quantil ($q_{0.5}$), unteres Quartil das 25%-Quantil ($q_{0.25}$), oberes Quartil das 75%-Quantil ($q_{0.75}$). Sie teilen die sortierte Stichprobe in vier gleich grosse Teile.

Wie ist ein Boxplot aufgebaut?::Box von $q_{0.25}$ bis $q_{0.75}$ (enthaelt 50% der Daten), Median-Linie in der Box, Whisker bis maximal $1.5 \cdot IQR$ ueber die Quartile hinaus (bis zum letzten Datenpunkt innerhalb dieser Grenze), Ausreisser werden als einzelne Punkte jenseits der Whisker dargestellt.

Was sind typische "Fehler" beim Visualisieren von Daten?::Manipulative Achsenskalierung, unnoetige 3D-Darstellungen, Tortendiagramme mit vielen Kategorien, logarithmische Skalen bei Groessenvergleichen, ungleiche Symbolgroessen, abgeschnittene Achsen und das Ueberlagern unzusammenhaengender Daten (spurious correlations).

Was sind Best Practices der Visualisierung von Daten?::Tuftes Regeln: Daten-Druckerschwaerze-Verhaeltnis maximieren, Luegenfaktor minimieren, Chartjunk vermeiden, angemessene Skalen nutzen und Achsen beschriften. Horizontale Balkendiagramme statt Tortendiagramme, 2D statt 3D, gleich grosse Symbole.
