---
course: data-science
type: selbstkontrolle
lecture: 8
date: 2026-06-02
tags:
  - data-science
  - selbstkontrolle
  - clustering
  - k-means
  - k-means-plusplus
  - hierarchisches-clustering
  - bisecting-k-means
  - agglomeratives-clustering
  - linkage
  - dendrogramm
  - flashcards/data-science
source: 08_Clustering_Algorithmen.pdf
---

# Selbstkontrolle 08 — Clustering: Algorithmen

**Quelle:** [[data-science/resources/08_Clustering_Algorithmen.pdf|08_Clustering_Algorithmen.pdf]]

## Fragen

Was ist Clusteranalyse / Clustering?::**Clusteranalyse** umfasst Methoden zur Identifizierung von Gruppen (Clustern) **aehnlicher** Datenpunkte. **Clustering** ist die Partition (Einteilung) der Datenpunkte in Gruppen. Verfahren unterscheiden sich in der Definition von Aehnlichkeit, von Gruppen und im algorithmischen Vorgehen.

Warum wollen wir Daten clustern?::Um Aussagen ueber **Gruppen in den Daten** zu machen (z.B. Kundensegmente). Clustering macht verborgene Struktur sichtbar und ist meist der **Startpunkt** einer Analyse, nicht das Endergebnis.

Was ist der Unterschied zwischen partitionierenden und hierarchischen Clusterverfahren?::**Partitionierend** (z.B. $k$-Means) liefert **eine** flache Partition in vorgegebene $k$ Cluster. **Hierarchisch** liefert eine ganze **Hierarchie** (Dendrogramm), aus der man durch Abschneiden bei einem Level jede Clusterzahl ablesen kann. Hierarchisch ist top-down (Bisecting k-Means) oder bottom-up (agglomerativ).

Welche Groesse minimiert k-Means?::Die Summe der **Intra-Cluster-Variationen** $\arg\min_{C_1,\dots,C_k} \sum_{i=1}^{k} \frac{1}{|C_i|} \sum_{x,y \in C_i} \|x-y\|^2$. Aequivalent: die Summe der quadrierten Abstaende der Punkte zu ihrem Clusterzentrum $\sum_i \sum_{x \in C_i}\|x-m_i\|^2$ (Faktor 2).

Was ist die Intra-Cluster-Variation?::$W(C_i) = \frac{1}{|C_i|} \sum_{x,y \in C_i} \|x-y\|^2$ — der gemittelte quadrierte euklidische Abstand ueber **alle Paare** $x,y$ innerhalb des Clusters. Kleine Werte = kompaktes Cluster.

Wie funktioniert der k-Means Algorithmus?::1. Waehle $k$ zufaellige Clusterzentren aus den Daten. 2. Wiederhole bis Stoppkriterium: **(3) Zuordnung** — jeder Punkt zum naechstgelegenen Zentrum $C_i = \{X_j: \|X_j-m_i\|^2 = \min_{i^*}\|X_j-m_{i^*}\|^2\}$; **(4) Update** — Zentrum als Mittelwert $m_i^{(t+1)} = \frac{1}{|C_i|}\sum_{X_j \in C_i} X_j$.

Warum ist k-Means ein iterativer Algorithmus statt einer exakten Loesung?::Das Optimierungsproblem (Minimierung der summierten Intra-Cluster-Variation) ist **(NP-)schwer**. Der iterative Algorithmus liefert eine — aber nicht zwangslaeufig die beste — Loesung.

Wie haengt der k-Means Algorithmus mit der Zielfunktion zusammen?::Es gilt $\frac{1}{|C_i|}\sum_{x,y \in C_i}\|x-y\|^2 = 2\sum_{x \in C_i}\|x-m_i\|^2$ mit $m_i = \frac{1}{|C_i|}\sum_{x \in C_i} x$. **Schritt 3** (Zuordnung zum naechsten Zentrum) minimiert die rechte Seite und damit auch die linke Seite, also die Zielfunktion.

Was sind die wichtigsten Schwachstellen / Nachteile von k-Means?::Abhaengig von der **zufaelligen Initialisierung** (findet nur **lokale Minima**); $k$ muss **vorab** festgelegt werden (verschiedene $k$ → verschiedene Cluster); **keine Ausreissererkennung**. Zudem werden kugelfoermige Cluster und ein sinnvolles euklidisches Distanzmass vorausgesetzt (skalierungsempfindlich).

Welche Empfehlung gibt es gegen die Initialisierungsabhaengigkeit von k-Means?::$k$-Means mit **mehreren unterschiedlichen zufaelligen Initialisierungen** durchfuehren und das Ergebnis mit dem **kleinsten Wert der Zielfunktion** (summierte Intra-Cluster-Variation) waehlen.

Warum sollte man Merkmale vor k-Means skalieren?::$k$-Means nutzt die **euklidische Distanz**, sodass Merkmale mit grossem Wertebereich dominieren. Beispiel: Alter 30 vs. 60 erscheint "naeher" als Einkommen 50 000 vs. 50 031. Skalierung (Normierung/Standardisierung) macht alle Merkmale vergleichbar.

Wie unterscheidet sich k-Means++ von k-Means?::Nur in der **Initialisierung**. $k$-Means waehlt alle Startzentren zufaellig gleichverteilt; $k$-Means++ waehlt sie **sukzessive** mit Wahrscheinlichkeit **proportional zu $D^2(x)$** (Distanz zum naechsten bereits gewaehlten Zentrum). So liegen die Startzentren weit auseinander → bessere, stabilere Loesungen.

Was sind k-Median und k-Medoids?::Varianten von $k$-Means mit anderer Zentrumsdefinition: **$k$-Median** nutzt statt des Mittelwerts den **Median** pro Cluster; **$k$-Medoids** erlaubt als Clusterzentren nur **Punkte aus dem Datensatz**.

Was ist hierarchisches Clustering?::Erzeugt eine **Hierarchie von Clustern** — von einem Cluster mit $n$ Elementen bis zu $n$ Clustern mit je einem Element, dargestellt als **Dendrogramm**. Top-down (1 → $n$ Cluster) oder bottom-up (n → 1 Cluster).

Was ist ein Dendrogramm?::Die Darstellung einer Clusterhierarchie als **Baum**: jedes **Blatt** ist ein Cluster mit einem Element, jeder **Knoten** die Vereinigung seiner Kinder. Die **$y$-Achse** zeigt oft die (Un-)Aehnlichkeit der vereinigten Cluster (Hoehe der Vereinigung).

Wie erhalten wir aus einem Dendrogramm ein Clustering?::Durch **horizontales Abschneiden** bei einer bestimmten Hoehe (Level). Jeder durchtrennte Ast = ein Cluster. Tieferer Schnitt → mehr Cluster, hoeherer Schnitt → weniger Cluster.

Was ist der Unterschied zwischen Top-Down und Bottom-Up Clusterverfahren (mit Beispielen)?::**Top-down (divisiv)** startet mit einem Cluster (alle Punkte) und teilt iterativ auf → Beispiel **Bisecting k-Means**. **Bottom-up (agglomerativ)** startet mit einem Cluster je Punkt und verschmilzt iterativ → Beispiel **Single / Complete / Average Linkage**.

Wie funktioniert Bisecting k-Means?::Top-down: Start mit einem Cluster aus allen Punkten; wiederholt wird ein Cluster mit **2-Means** in zwei geteilt, bis $k$ Cluster vorliegen. Geteilt wird das **groesste** Cluster (`largest_cluster`) oder das mit der **groessten Intra-Cluster-Variation** (`biggest_inertia`).

Wie funktioniert agglomeratives Clustering?::Bottom-up: Start mit einem Cluster je Punkt; in jedem Schritt werden die **beiden Cluster mit kleinster Distanz** (gemaess Linkage) verschmolzen, bis ein Cluster bleibt. Ergebnis ist ein Dendrogramm.

Was ist der Unterschied zwischen Single, Complete und Average Linkage?::Definition der Distanz zwischen zwei Clustern $X,Y$: **Single** $D=\min_{x,y} d(x,y)$ (kleinster Paarabstand), **Complete** $D=\max_{x,y} d(x,y)$ (groesster Paarabstand), **Average** $D=\frac{1}{|X||Y|}\sum_{x,y} d(x,y)$ (mittlerer Paarabstand). Average/Complete erzeugen i.d.R. ausgewogenere Cluster als Single (Single neigt zu Chaining/Ketten).

Auf welchen Annahmen basieren die unterschiedlichen Clusterverfahren?::Gemeinsam: aehnliche Punkte liegen nahe zusammen und das Abstandsmass ist geeignet. **$k$-Means**: Cluster sind (tendenziell) **kugelfoermig**, keine Ausreisser. **Hierarchisch**: Cluster sind **hierarchisch** (grosse Cluster aus kleineren zusammengesetzt).

Datensatz: 50 Frauen / 50 Maenner aus 3 Nationalitaeten; 2 Cluster = Geschlechter, 3 Cluster = Nationalitaeten. Welches Verfahren?::**$k$-Means** — denn die Cluster sind **nicht hierarchisch**: die 3 Nationalitaeten-Cluster resultieren nicht aus der Teilung der 2 Geschlechter-Cluster. Ein hierarchisches Verfahren wuerde die 3-Cluster-Loesung durch Aufspalten der 2-Cluster-Loesung erzeugen.

Welches ist das beste Clusterverfahren?::Es gibt **kein** universell bestes Verfahren. Jedes beruht auf anderen Annahmen und passt zu anderen Datenstrukturen (z.B. $k$-Means schlecht fuer verschachtelte Ringe, dichtebasierte Verfahren wie DBSCAN dort gut). Die Wahl haengt von Daten und Ziel ab; die Bewertung von Partitionen ist ein offenes Forschungsproblem.

Welche praktischen Schritte umfasst eine Cluster-Analyse?::1. Passendes **Distanzmass** waehlen. 2. **Vorverarbeitung** (Einfluss der Skalierung pruefen). 3. **Wahl der Cluster** (Einfluss der Clusteranzahl untersuchen). 4. **Interpretation** der Ergebnisse. Die Cluster-Analyse ist meist Startpunkt, nicht Endergebnis.

Warum darf man k-Means nicht mit k-nearest neighbors (kNN) verwechseln?::$k$-Means ist ein **Clusteringverfahren** (unsupervised, teilt Punkte in $k$ Cluster). $k$-nearest neighbors ist ein **Klassifikationsverfahren** (supervised, ordnet einem Punkt die Mehrheitsklasse seiner $k$ naechsten Nachbarn zu). Trotz des "$k$" voellig verschiedene Ziele.
