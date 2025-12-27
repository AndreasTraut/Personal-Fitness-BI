# 📊 Dashboard Layout & Visualisierung

Das Dashboard folgt einem "Z-Layout" (Leserichtung von oben links nach unten rechts), um die wichtigsten Informationen zuerst zu zeigen.

<img width="1917" height="1074" alt="2025-12-27 19_26_54-" src="https://github.com/user-attachments/assets/85384bbc-6ea4-4235-9168-d3f8b289fdb4" />

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

## Bereich 4: Der Haupt-Verlauf
Visualisierung der Zeitreihe mittels **Gestapeltem Säulendiagramm**:
* **X-Achse:** `Jahr - Monat - Kalenderwoche` (aus der Kalendertabelle).
* **Balken:** `Dauer` oder `Distanz`.





