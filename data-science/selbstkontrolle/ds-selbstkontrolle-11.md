---
course: data-science
type: selbstkontrolle
lecture: 11
date: 2026-06-23
tags:
  - data-science
  - selbstkontrolle
  - feature-engineering
  - modellierung
  - supervised-learning
  - klassifikation
  - regression
  - confusion-matrix
  - precision-recall
  - roc-auc
  - knn
  - flashcards/data-science
source: 11_Feature_Eng_Modellierung.pdf
---

# Selbstkontrolle 11 — Feature Engineering & Datengetriebene Modelle

**Quelle:** [[data-science/resources/11_Feature_Eng_Modellierung.pdf|11_Feature_Eng_Modellierung.pdf]]

## Fragen

Was ist das Ziel des Feature Engineerings?::Daten mithilfe von **Domaenenwissen** zu transformieren (und neue Merkmale zu erzeugen), um eine **datengetriebene Modellierung** zu ermoeglichen. Die **Repraesentation** der Daten ist entscheidend — Feature Engineering "vermittelt" zwischen Daten und Modellen.

Woher wissen wir, wie Daten transformiert werden sollten?::Drei Wege: (1) klassische **EDA-Methoden** (deskriptive Statistik, Dimensionsreduktion, Clustering); (2) **Domaenenexperten befragen** (Empfehlung, falls moeglich); (3) **selbst zum Domaenenexperten** werden (Spezialisierung).

Was ist der Unterschied zwischen datengetriebener und theoriebasierter Modellierung?::**Datengetrieben**: mathematische Beschreibung mithilfe von **Daten** (z.B. ML-Modelle, Bild-/Spracherkennung). **Theoriebasiert**: mathematische Beschreibung mithilfe von **Grundprinzipien** (z.B. physikalische Modelle, Wettervorhersage). Dazwischen: theory-guided Data Science Models.

Wann ist maschinelles Lernen hilfreich?::Wenn (1) **Muster** in den Daten existieren, (2) diese sich **nicht (effizient) mathematisch beschreiben** lassen und (3) **Daten** vorliegen. Nicht hilfreich bei Zufall (Roulette) oder exakt beschreibbaren Problemen (Rechteckflaeche).

Wie kann Supervised Learning mathematisch beschrieben werden?::Input $x \in X$ (Features), Output $y \in Y$ (Label), unbekannte Zielfunktion $f: X \to Y$ mit $f(x) = y$. Gegeben: Stichprobe $(x_1, y_1), \dots, (x_n, y_n)$. Ziel: Modell $g$ finden mit $g \approx f$.

Was ist der Unterschied zwischen Klassifikation und Regression?::Bei **Klassifikation** ist $Y$ **diskret** → Vorhersage einer Klasse/Kategorie (Klassifikator). Bei **Regression** ist $Y$ **stetig** → Vorhersage einer numerischen Groesse (Regressor). Beispiele: Nettomiete/Aktienkurs → Regression; Ziffernerkennung → Klassifikation.

Wofuer sind Baseline Models hilfreich?::Einfache, schnell erzeugte Modelle zur **Einschaetzung** eines Klassifikators ("Wie gut ist mein Modell gegenueber einem einfachen Baseline?"). Sie decken Probleme wie Class Imbalance auf (z.B. "immer negativ"-Modell mit 99% Accuracy, das keinen Kranken erkennt).

Was ist bei einem binaeren Klassifikator schlimmer: False Positives oder False Negatives?::**Es kommt auf die Anwendung an.** Smartphone-Entsperren: **FP** schlimmer (Angreifer akzeptiert). Terroristenerkennung: **FN** schlimmer (Terrorist als Zivilist eingestuft).

Was bedeuten TP, TN, FP, FN?::$TP$ = korrekt vorhergesagte Positive, $TN$ = korrekt vorhergesagte Negative, $FP$ = faelschlich als positiv vorhergesagt, $FN$ = faelschlich als negativ vorhergesagt. $P$ = alle echten Positiven, $N$ = alle echten Negativen.

Was ist die Accuracy eines Klassifikators?::$Acc = \frac{TP + TN}{TP + TN + FP + FN}$ — Anteil der **richtig** klassifizierten Datenpunkte. Verallgemeinert: $Acc = \frac{tr(cm)}{sum(cm)}$. Problematisch bei **Class Imbalance** (ungleich grossen Klassen).

Wofuer brauchen wir Precision und Recall?::Weil **Accuracy bei ungleich grossen Klassen** (class imbalance) irrefuehrend ist. Precision und Recall bewerten die Positiv-Klasse differenzierter und bestrafen FP bzw. FN gezielt.

Wie sind Precision und Recall definiert?::$Precision = \frac{TP}{TP + FP}$ (Anteil Richtig-Positiver an allen "positiv" Klassifizierten, bestraft **FP**). $Recall = \frac{TP}{TP + FN} = \frac{TP}{P}$ (Anteil Richtig-Positiver an allen Positiven, bestraft **FN**; auch Sensitivitaet / TPR).

Was ist der F1-Score?::Das **harmonische Mittel** aus Precision und Recall: $F_1 = 2 \frac{Precision \cdot Recall}{Precision + Recall} \in [0, 1]$. Je groesser, desto besser der Klassifikator.

Was ist Specificity?::$Specificity = \frac{TN}{TN + FP} = \frac{TN}{N}$ — Anteil der korrekt erkannten Negativen (auch Spezifitaet / TNR – True Negative Rate).

Was ist der Precision-Recall-Tradeoff?::Ueber einen **freien Parameter** (z.B. Schwellwert $\theta$) kann zwischen $TPR = \frac{TP}{P}$ und $FPR = \frac{FP}{N}$ abgewogen werden. Schwellwert nach links → besserer **Recall**; nach rechts → bessere **Precision**.

Wie erhalten wir eine ROC-Kurve?::Durch Variation des **freien Parameters** (Schwellwert $\theta$) und Auftragen von $TPR$ gegen $FPR$. Die Kurve beschreibt, wie gut die Klassenverteilungen **trennbar** sind; die Diagonale entspricht einer Zufallsvorhersage.

Wie erhalten wir aus einer ROC-Kurve den "besten" Klassifikator?::Ueber den **Youden Index** $J = TPR - FPR$: der Punkt mit **groesster Distanz zur Diagonalen** maximiert $J$. Optimaler Schwellwert: $\tilde\theta = \arg\max_\theta J(\theta)$. Je nach Anwendung kann ein anderer Schwellwert sinnvoller sein.

Welche Groesse fasst die ROC-Kurve in einer Zahl zusammen?::Die **AUC** (Area Under Curve) — die Flaeche unter der ROC-Kurve. $AUC \in [0.5, 1]$: $0.5$ = Zufallsvorhersage, $1$ = perfekte Vorhersage. Erlaubt Vergleich zweier Klassifikatoren, verliert aber Information.

Was ist eine Konfusionsmatrix?::Eine Tabelle wahre Klasse gegen vorhergesagte Klasse (binaer: TP, FN, FP, TN). Sie zeigt das **gesamte Bild** der Klassifikationsleistung — alle Guetemasse sind Zusammenfassungen daraus. Verallgemeinerbar auf $n$ Klassen mit Eintraegen $c_{ij}$.

Wie funktioniert der k-naechste-Nachbarn-Algorithmus (kNN)?::Fuer einen Input $X$: (1) Abstand zu allen Trainingspunkten $X_1, \dots, X_n$ berechnen, (2) die $k$ **naechsten** Punkte auswaehlen, (3) die **haeufigste Klasse** der Nachbarn als Vorhersage $y$ nehmen. kNN ist ein "Lazy Learner" (Training = nur Speichern der Daten).

Auf welcher impliziten Annahme basiert der kNN-Algorithmus?::Auf der Annahme, dass **"nahe" Punkte aehnliche Eigenschaften** haben — raeumliche Naehe im Featureraum bedeutet Aehnlichkeit der Zielgroesse.
