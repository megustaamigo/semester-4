---
course: data-science
type: selbstkontrolle
lecture: 7
date: 2026-05-26
tags:
  - data-science
  - selbstkontrolle
  - dimensionsreduktion
  - mds
  - multidimensional-scaling
  - isomap
  - pca
  - manifold-learning
  - flashcards/data-science
source: 07_Isomap.pdf
---

# Selbstkontrolle 07 — Dimensionsreduktion: MDS und Isomap

**Quelle:** [[data-science/resources/07_Isomap.pdf|07_Isomap.pdf]]

## Fragen

Wie koennen wir fuer hochdimensionale Daten ($p > n$) effizient die Hauptkomponenten bestimmen?::Statt der grossen Matrix $\mathbf{X}^T\mathbf{X} \in \mathbb{R}^{p \times p}$ nutzt man die **kleine** Matrix $\mathbf{X}\mathbf{X}^T \in \mathbb{R}^{n \times n}$ — beide haben dieselben Eigenwerte. (1) Berechne $\lambda_i, u_i$ mit $\mathbf{X}\mathbf{X}^T u_i = \lambda_i u_i$. (2) Setze $v_i = \mathbf{X}^T u_i$. (3) Normiere $w_i = \frac{\mathbf{X}^T u_i}{\|\mathbf{X}^T u_i\|}$, da Eigenvektoren nicht automatisch Laenge 1 haben.

Was ist Multidimensional Scaling (MDS)?::Eine Familie von Verfahren zur Visualisierung und Dimensionsreduktion. Das "klassische" Verfahren (Torgerson-Scaling / Principal Coordinate Analysis, PCoA) rekonstruiert aus paarweisen Distanzen die Koordinaten der Punkte. Es ist Grundlage von Isomap.

Was ist beim MDS gegeben und gesucht?::**Gegeben**: die Matrix der paarweisen Distanzen $D = (d_{ij})_{i,j=1}^{n}$ zwischen $n$ Objekten. **Gesucht**: eine Datenmatrix $\mathbf{X}$ mit Koordinaten, sodass die Objekte diese paarweisen Distanzen haben. MDS startet mit den Distanzen, PCA mit der Datenmatrix $\mathbf{X}$.

Warum ist die MDS-Loesung nicht eindeutig?::Distanzen sind **invariant unter orthogonalen Transformationen** (Rotation, Spiegelung) und **Translation**. Die rekonstruierte Konfiguration kann also gedreht/gespiegelt/verschoben sein, obwohl alle paarweisen Abstaende stimmen (Beispiel: Spanien-Karte).

Wie kann mit MDS die Dimension reduziert werden?::(1) Sei $\mathbf{X} \in \mathbb{R}^{n \times p}$. (2) Bestimme die paarweisen Distanzen $D$. (3) Mit MDS aus $D$ eine neue Datenmatrix $\widetilde{\mathbf{X}}$ mit $d < p$ Dimensionen bestimmen, sodass die Abstaende moeglichst nah an den urspruenglichen $d_{ij}$ bleiben.

Aus welchen 3 Schritten besteht der MDS-Algorithmus?::(1) **Quadriere** die Eintraege von $D$: $\widetilde{D} = (d_{ij}^2)$. (2) **Gram-Matrix** $B = \mathbf{X}\mathbf{X}^T$ per doppelter Zentrierung: $B = -\frac{1}{2} H \widetilde{D} H$ mit $H = I - \frac{1}{n}\mathbf{1}\mathbf{1}^T$. (3) **Eigenwertzerlegung** $B = V\Lambda V^T$ und $\mathbf{X} = V\Lambda^{1/2} = (\sqrt{\lambda_1} v_1, \dots, \sqrt{\lambda_p} v_p)$.

Wie lautet die Gram-Matrix $B$ in Schritt 2 des MDS?::$B = \mathbf{X}\mathbf{X}^T = -\frac{1}{2} H \widetilde{D} H$ mit der Zentrierungsmatrix $H = I - \frac{1}{n}\mathbf{1}\cdot\mathbf{1}^T$ und der Matrix der quadrierten Distanzen $\widetilde{D}$. Das ist die **doppelte Zentrierung** der quadrierten Distanzmatrix.

Warum laesst sich $B$ in Schritt 3 immer spektral zerlegen?::$B = \mathbf{X}\mathbf{X}^T$ ist reell und **symmetrisch** ($B^T = (\mathbf{X}\mathbf{X}^T)^T = \mathbf{X}\mathbf{X}^T = B$). Reelle symmetrische Matrizen sind orthogonal diagonalisierbar: $B = V\Lambda V^T$ mit orthogonalem $V$ (Spalten = Eigenvektoren) und Diagonalmatrix $\Lambda$ der Eigenwerte.

Wie waehlen wir die Dimension $p$ der Datenmatrix bei MDS?::Ueber den Anteil der erklaerten Distanzen **PDME** (Proportion of Distance Matrix Explained): $\mathrm{PDME}(j) = \frac{\lambda_j}{\sum_{i=1}^{n} |\lambda_i|}$. Man waehlt $p$ so, dass die kumulierte erklaerte Distanz gross ist (analog zur erklaerten Varianz bei PCA).

Wie haengen PCA und MDS zusammen?::Beide rechnen ueber Eigenwerte/-vektoren. **Wenn** $\mathbf{X}$ zentriert ist **und** $D$ aus euklidischen Distanzen besteht, **dann** sind MDS-Koordinaten = PCA-Koordinaten. Grund: $\mathbf{X}^T\mathbf{X}$ (PCA) und $\mathbf{X}\mathbf{X}^T = B$ (MDS) haben dieselben Eigenwerte.

Warum braucht man MDS, wenn es PCA-aequivalent ist?::Wegen der **anderen Ausgangslage**: bei MDS sind die **Distanzen bekannt** und die **Koordinaten unbekannt**. Wenn nur paarweise Distanzen (keine Koordinaten) vorliegen, kann PCA nicht angewendet werden, MDS aber schon.

Was sind die Grenzen der PCA?::PCA entdeckt nur **lineare** Strukturen. Liegen die Daten auf einer **nichtlinearen Mannigfaltigkeit** (z.B. Swiss Roll), hilft PCA nicht: euklidisch nahe Punkte koennen entlang der Mannigfaltigkeit weit entfernt sein, und die lineare Projektion zerstoert die Struktur.

Was ist Isomap?::Isomap (*isometric feature mapping*) ist eine Erweiterung von MDS fuer nichtlineare Mannigfaltigkeiten. Statt euklidischer Distanzen nutzt es **geodaetische Distanzen** (Abstaende entlang der Mannigfaltigkeit), approximiert ueber kuerzeste Pfade in einem Nachbarschaftsgraphen.

Was ist eine geodaetische Distanz?::Die Distanz **entlang der Mannigfaltigkeit** statt quer durch den Raum. Bei Isomap wird sie als Laenge des **kuerzesten Pfades** zwischen zwei Punkten im Nachbarschaftsgraphen approximiert.

Wie funktioniert der Isomap-Algorithmus?::Gegeben $\mathbf{X} \in \mathbb{R}^{n \times p}$, gesucht $\widetilde{\mathbf{X}} \in \mathbb{R}^{n \times d}$ ($d < p$): (1) **Nachbarschaftsgraph** erzeugen (nahe Punkte mit Kante verbinden). (2) **Paarweise Distanzen** auf dem Graph = Laenge des kuerzesten Pfades (geodaetisch). (3) **MDS** auf die Distanzmatrix $D$ anwenden.

Was ist der Unterschied zwischen Dimensionsreduktion mit MDS und Isomap?::Beide nutzen denselben Kern (klassisches MDS). Der Unterschied ist die **Distanzdefinition**: MDS nutzt euklidische Distanzen, Isomap **geodaetische** Distanzen (kuerzeste Pfade im Nachbarschaftsgraphen). Dadurch kann Isomap nichtlineare Mannigfaltigkeiten korrekt entrollen.

Welche Parameter muss man bei Isomap waehlen?::Die **Anzahl der Nachbarn** (`n_neighbors`) fuer den Nachbarschaftsgraphen (zu klein → Graph zerfaellt, zu gross → Short-Circuit-Abkuerzungen) und die **Zieldimension** `n_components` ($= d$).

Wuerden Sie fuer Daten in einem linearen Raum PCA oder Isomap waehlen?::**PCA** (bzw. aequivalent klassisches MDS). Sie ist einfacher, schneller und braucht keine Nachbarschaftsparameter. Isomap waere unnoetig komplex und fehleranfaellig, ohne Mehrwert.

Wuerden Sie fuer Daten in einem nicht-linearen Raum PCA oder Isomap waehlen?::**Isomap**. PCA entdeckt nur lineare Strukturen und projiziert die Mannigfaltigkeit fehlerhaft. Isomap folgt ueber geodaetische Distanzen der Mannigfaltigkeit und entrollt sie korrekt.
