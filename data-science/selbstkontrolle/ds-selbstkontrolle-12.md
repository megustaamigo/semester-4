---
course: data-science
type: selbstkontrolle
lecture: 12
date: 2026-06-30
tags:
  - data-science
  - selbstkontrolle
  - supervised-learning
  - knn
  - in-sample
  - out-of-sample
  - train-test-split
  - metrik
  - kernel-regression
  - flashcards/data-science
source: 12_Modellierung.pdf
---

# Fragen zur Selbstkontrolle — Vorlesung 12 (Datengetriebene Modelle)

Wie ist das "Supervised-Learning-Diagramm" zu interpretieren?::Eine unbekannte **Zielverteilung** $\mathbb{P}(y\mid x)$ (Zielfunktion $f:X\to Y$ plus **Rauschen**) erzeugt zusammen mit der Verteilung $P$ auf $X$ einen **Datensatz** $(x_1,y_1),\dots,(x_n,y_n)$ (nur eine **Stichprobe**, nicht die ganze Population). Ein **Lernalgorithmus** ("Training") waehlt aus der Menge der **Hypothesen** $H$ ("Modell") eine finale Hypothese $g\approx f$ ("trainiertes Modell") aus.

Wie erhalten wir beim kNN-Algorithmus ein Modell mit 100% Accuracy?::Mit $k=1$: der naechste Nachbar eines **Trainingspunktes** ist der Punkt selbst (Abstand 0), also wird jeder Trainingspunkt seiner eigenen Klasse zugeordnet. Das ergibt 100% **in-sample** Accuracy — sagt aber nichts ueber neue Daten aus (Overfitting).

Wie sind in-sample und out-of-sample Accuracy definiert? Was ist der Unterschied?::**In-sample Accuracy** = Accuracy auf den **Trainingsdaten** (bekannte Daten). **Out-of-sample Accuracy** $\mathbb{P}(y=g(x))$ = Accuracy auf **neuen, ungesehenen** Daten. Uns interessiert die out-of-sample Accuracy (Generalisierung); die in-sample Accuracy ist nur auf den zum Lernen benutzten Daten gemessen.

Warum koennen wir nicht die in-sample Accuracy als Schaetzer fuer die out-of-sample Accuracy nutzen?::Weil die in-sample Accuracy ein **verzerrter (optimistischer) Schaetzer** ist: das Modell wurde auf genau diesen Daten optimiert und kann sie "auswendig lernen" (z.B. kNN mit $k=1$ → 100%), ohne auf neuen Daten gut zu generalisieren.

Wie koennen wir die out-of-sample Accuracy schaetzen? Warum wollen wir sie ueberhaupt schaetzen?::Indem wir die Stichprobe in **Trainings- und Testdaten** aufteilen (oft 80% / 20%, z.B. `train_test_split`), das Modell nur auf den Trainingsdaten lernen und die Accuracy auf den **Testdaten** messen. Wir wollen sie schaetzen, weil sie angibt, wie gut das Modell auf **neuen** Daten **verallgemeinert** — das ist der eigentliche Zweck des Modells.

Wovon haengt es ab, welche Punkte die naechsten Nachbarn sind?::Von der gewaehlten **Metrik (Abstandsmass)**. Beispiel (naechster Punkt zum Ursprung): **Manhattan** $d_1$, **euklidisch** $d_2$, **Maximum** $d_\infty$ liefern jeweils einen **anderen** naechsten Nachbarn → und damit ggf. eine andere Vorhersage. Ebenso spielt die **Skalierung** der Features eine Rolle.

Wie kann mit dem k-nearest-neighbors-Algorithmus eine stetige Variable vorhergesagt werden (Regression)?::Statt der haeufigsten Klasse bildet man den **Durchschnitt** der Zielwerte der $k$ Nachbarn (`KNeighborsRegressor`). Besser: ein **gewichteter** Durchschnitt abhaengig von der Entfernung → **Kernel Regression** / **Local Polynomial Regression** (z.B. per Faltung `np.convolve(y, kernel)`).
