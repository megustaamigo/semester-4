---
course: data-science
type: selbstkontrolle
lecture: 9
date: 2026-06-09
tags:
  - data-science
  - selbstkontrolle
  - cluster-validierung
  - silhouette
  - prediction-strength
  - purity
  - mutual-information
  - interne-validierung
  - externe-validierung
  - flashcards/data-science
source: 09_Cluster_Validierung.pdf
---

# Selbstkontrolle 09 — Clustering: Validierung

**Quelle:** [[data-science/resources/09_Cluster_Validierung.pdf|09_Cluster_Validierung.pdf]]

## Fragen

Wozu benutzen wir Clustervalidierungsverfahren?::Cluster-Verfahren finden **immer** Cluster (auch in zufaelligen Daten). Clustervalidierungsverfahren bewerten die **Qualitaet** eines Clusterings — also welches Clustering sinnvoll ist — und ermoeglichen so die **Wahl von Hyperparametern** wie der Clusteranzahl $k$.

Wie unterscheiden sich interne und externe Clustervalidierung?::**Externe** Validierung nutzt externe Information ueber die wahre Clusterzugehoerigkeit (ground truth), z.B. Purity und Mutual Information. **Interne** Validierung basiert nur auf den Daten, z.B. Silhouette und Prediction Strength. Intern ist in der Praxis am verbreitetsten, da ground truth meist fehlt.

Was sind die drei Arten der Clustervalidierung?::1) Statistische Tests auf Abwesenheit von Clustern ($H_0$: Daten gleichverteilt, unueblich), 2) Externe Validierung (nutzt wahre Zugehoerigkeit), 3) Interne Validierung (nur auf Daten basierend).

Was ist ein Hyperparameter (am Beispiel $k$-Means)?::Ein Parameter, der das Modell beschreibt, aber **nicht geschaetzt** wird. Bei $k$-Means ist die Clusteranzahl $k$ ein Hyperparameter; die Clusterzentren $m^{(1)}, \dots, m^{(k)}$ sind **kein** Hyperparameter (sie werden gelernt).

Was ist die Purity eines Clusterings?::Ein externes Mass fuer die **Reinheit** der Cluster gegen eine ground truth $T_1, \dots, T_k$: $\text{purity} = \frac{1}{n} \sum_{i=1}^{k} \max_{j=1}^{k} |C_i \cap T_j|$. Jedes Cluster $C_i$ wird dem haeufigsten wahren Cluster zugeordnet.

In welchem Wertebereich liegt die Purity und was ist der beste Wert?::$\text{purity} \in (0, 1]$, bester Wert ist $1$ (jedes Cluster enthaelt nur Punkte eines wahren Clusters). Achtung: $1$ ist trivial erreichbar (nur ein wahres Cluster, oder ein Cluster je Punkt).

Was misst Mutual Information bei der externen Validierung?::Die **Aehnlichkeit zwischen den Partitionen** $C$ und $T$: $I(C, T) = \sum_{i,j} p_{ij} \log_2 \frac{p_{ij}}{p_{C_i} p_{T_j}}$ mit $p_{ij} = \frac{|C_i \cap T_j|}{n}$. Je groesser, desto aehnlicher (besser).

Sind grosse oder kleine Werte der Mutual Information zu bevorzugen?::**Grosse** Werte — je groesser $I(C, T)$, desto aehnlicher sind das Clustering $C$ und die ground truth $T$, also desto besser.

Was misst der Silhouettenindex?::Wie gut ein **einzelner Punkt** $x_i$ geclustert wurde: die **Kohaesion** $a(x_i)$ (mittlere Distanz im eigenen Cluster) in Relation zur **Separation** $b(x_i)$ (mittlere Distanz zum naechsten Cluster). $s(x_i) = \frac{b(x_i) - a(x_i)}{\max\{a(x_i), b(x_i)\}}$.

Wie sind Kohaesion $a(x_i)$ und Separation $b(x_i)$ definiert?::**Kohaesion** $a(x_i) = \frac{1}{|C_j|-1} \sum_{x \in C_j, x \neq x_i} d(x_i, x)$ (mittlere Distanz im eigenen Cluster, fuer $|C_j|>1$). **Separation** $b(x_i) = \min_{k \neq j} \frac{1}{|C_k|} \sum_{x \in C_k} d(x_i, x)$ (mittlere Distanz zum naechsten Cluster).

In welchem Wertebereich liegt der Silhouettenindex und was ist der beste Wert?::$s(x_i) \in [-1, 1]$. Bester Wert ist $1$ (Punkt sehr aehnlich zum eigenen Cluster, $a(x_i) \ll b(x_i)$).

Wann ist der Silhouettenindex negativ und was bedeutet das?::Wenn $a(x_i) > b(x_i)$, d.h. der Punkt liegt im Mittel **naeher an einem anderen Cluster** als an seinem eigenen. Bedeutung: der Punkt ist vermutlich **falsch zugeordnet**.

Wie veraendert sich $s(x_i)$, wenn $x_i \in C_1$ stattdessen $C_2$ zugeordnet wird?::Bei zwei Clustern kippt das Vorzeichen: $s(x_i) \to -s(x_i)$, weil Kohaesion und Separation die Rollen tauschen.

Was ist der Unterschied zwischen "average silhouette width" und "silhouette score"?::**Average silhouette width** $\bar{s}_j = \frac{1}{|C_j|} \sum_{x \in C_j} s(x)$ bewertet **einzelne Cluster**. **Silhouette Score** $\bar{s} = \frac{1}{n} \sum_{i=1}^n s(x_i)$ bewertet **alle Cluster** zusammen.

Was ist ein Silhouettenplot?::Visuelle Darstellung der Silhouettenindizes des gesamten Datensatzes, nach Cluster gruppiert und je Cluster in absteigender Groesse sortiert. Erlaubt eine visuelle Einschaetzung der Clusterqualitaet.

Welche implizite Annahme liegt dem Silhouettenindex zugrunde?::Dass Cluster **(tendenziell) kugelfoermig** (konvex/kompakt) sind. Bei nicht-konvexen Strukturen (z.B. ineinander liegende Ringe) zeigt der Silhouette Score die richtige Clusteranzahl nicht an.

Wie waehlt man mit dem Silhouette Score die Clusteranzahl?::Den Silhouette Score fuer verschiedene $k$ berechnen und das $k$ mit dem **maximalen** Score waehlen.

Was ist die Idee hinter der Prediction Strength?::Die Clusterqualitaet ist hoch, wenn die Clusterzugehoerigkeit auf **anderen Realisierungen** der Daten zuverlaessig **vorhergesagt** werden kann — analog zu Hyperparameter-Tuning / Model Selection im supervised learning.

Wie wird die Prediction Strength bestimmt?::1) Split in $\mathcal{D}_{\text{train}}$ und $\mathcal{D}_{\text{val}}$. 2) Gleiches Clusterverfahren auf beide. 3) Validierungspunkte gemaess Trainingsmodell zuordnen. 4) Je Validierungs-Cluster den Anteil $p$ der Paare bestimmen, die auch im selben Trainings-Cluster waeren. 5) $\text{ps}(k) = \min_\ell p$ (Minimum ueber alle Cluster).

Wie lautet die formale Definition der Prediction Strength?::Mit Ko-Mitgliedschaftsmatrix $M_{ij} = 1$ falls $\exists k: x_i, x_j \in C_k$ (selbes Trainingscluster), sonst $0$: $\text{ps}(k) = \min_{\ell} \frac{1}{|A_\ell|(|A_\ell|-1)} \sum_{x_i, x_j \in A_\ell, x_i \neq x_j} M_{ij}$, wobei $A_\ell$ die Validierungs-Cluster sind.

Warum gilt fuer die Prediction Strength $\text{ps}(1) = 1$?::Bei $k = 1$ gibt es nur **ein** Cluster, also gehoeren trivialerweise **alle** Punktpaare zum selben Cluster ($M_{ij} = 1$ fuer alle Paare). Damit ist $p = 1$ und das Minimum $\text{ps}(1) = 1$.

Wie erhalten wir mit der Prediction Strength die optimale Clusteranzahl?::Man waehlt den **groessten** Wert von $k$, fuer den die Prediction Strength (noch) maximal/hoch ist (da $\text{ps}(1)=1$ trivial ist, das groesste nicht-triviale $k$). Haeufig 80/20-Split, alternativ Cross Validation.

Wie haengt die Cluster-Zuordnung eines neuen Punktes vom Clusterverfahren ab?::Sie kodiert, was ein Cluster ist: $k$-Means → naechstes Clusterzentrum (Cluster sind kugelfoermig / Voronoi-Polygone); Single Linkage → minimale Distanz zum Cluster; Average Linkage → mittlere Distanz zum Cluster.
