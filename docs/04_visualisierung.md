# 📊 Dashboard Layout & Visualisierung

Das Dashboard folgt einem "Z-Layout" (Leserichtung von oben links nach unten rechts), um die wichtigsten Informationen zuerst zu zeigen.

## Bereich 1: Header & Filter
Ganz oben befinden sich die globalen Steuerelemente:
* **Titel:** "Personal Fitness BI"
* **Slicer (Datenschnitt):** Filter für `Jahr` und `Trainingsart` ("Krafttraining", "Laufen", "Radfahren", "Schwimmen", "Sonstiges" etc.).

## Bereich 2: KPIs (Big Numbers)
Eine Reihe mit **Karten-Visuals** für den schnellen Überblick:
* Anzahl Trainingeinheiten
* Distanz (km)
* Dauer (Stunden)
* Ø Effizienz (Fitness-Level).

## Bereich 3: Detail-Matrix
Eine Tabelle für die genaue Analyse am unteren Rand:
* **Zeilen:** `Jahr - Monat - Kalenderwoche` und `Trainingsart`
* **Spalten:** `Art`
* **Werte:** Trainingseinheiten, Dauer (Std), Ø kmh, Ø Distanz (km), Effizienz, Ø Puls.
* **Formatierung:** Nutzung von **Datenbalken** für die Distanz, um hohe Werte visuell hervorzuheben.

## Bereich 3: Der Haupt-Verlauf
Visualisierung der Zeitreihe mittels **Gestapeltem Säulendiagramm**:
* **X-Achse:** `Jahr - Monat - Kalenderwoche` (aus der Kalendertabelle).
* **Balken:** `Dauer` oder `Distanz`.





