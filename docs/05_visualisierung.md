# 📊 Dashboard Layout & Visualisierung

Das Dashboard ist in ein übersichtliches **Split-Layout** unterteilt: Auf der linken Seite befinden sich die detaillierten Zahlen (Matrix & Tabelle), während die rechte Seite die grafische Auswertung der Verläufe zeigt.

<img width="1917" height="1074" alt="Dashboard Screenshot" src="https://github.com/user-attachments/assets/85384bbc-6ea4-4235-9168-d3f8b289fdb4" />

## Bereich 1: Header & Filter
Ganz oben befinden sich die globalen Steuerelemente:
* **Titel:** "Personal Fitness BI"
* **Slicer (Datenschnitt):**
    * **Jahr:** Dropdown-Auswahl (z.B. "2025").
    * **Trainingsart:** Filter für spezifische Sportarten (z.B. "Laufen", "Radfahren") oder "Alle".

## Bereich 2: KPIs (Big Numbers)
Die oberste Zeile unter dem Header liefert einen schnellen Überblick über die Gesamtleistung im gewählten Zeitraum:
* **Trainingseinheiten:** Anzahl der durchgeführten Trainings.
* **Distanz (km):** Gesamtkilometer.
* **Dauer (Std):** Gesamtstunden.
* **Effizienz:** Der berechnete Fitness-Indikator.

## Bereich 3: Der Daten-Bereich (Links)
Die linke Seite des Dashboards ist für die genaue Analyse der Zahlen reserviert. Hier gibt es zwei Detail-Ansichten:

### A. Die Hierarchie-Matrix (Mitte Links)
Eine aggregierte Ansicht mit Drill-Down-Funktionalität.
* **Zeilen-Hierarchie:** `Jahr` -> `Monat` -> `Kalenderwoche` -> `Trainingsart`.
* **Werte:** Trainingseinheiten, Dauer (Std), Ø kmh, Distanz (km), Effizienz, Ø Puls.
* **Besonderheit:** Ermöglicht das Aufklappen von Zeiträumen (z.B. "Dezember" -> "KW 50" -> "Laufen").

### B. Die Detail-Tabelle (Unten Links)
Eine flache Liste aller einzelnen Trainingseinheiten für den direkten Zugriff auf das Datum.
* **Spalten:** `Kalenderwoche`, `Datum`, `Trainingsart`, `Dauer (Std)`, `Ø kmh`, `Distanz (km)`.

## Bereich 4: Die Visualisierung (Rechts)
Die rechte Seite visualisiert die Trends über die Zeit mittels zwei **Gestapelter Säulendiagramme**.

### Diagramm 1: Dauer (Std) pro Kalenderwoche
* **X-Achse:** `Kalenderwoche` (aus der Kalendertabelle).
* **Y-Achse:** `Dauer (Std)`.
* **Legende:** `Trainingsart` (Farbliche Unterscheidung der Sportarten).
* **Zweck:** Zeigt auf einen Blick, in welcher Woche am meisten Zeit investiert wurde.

### Diagramm 2: Distanz (km) pro Kalenderwoche
* **X-Achse:** `Kalenderwoche`.
* **Y-Achse:** `Distanz (km)`.
* **Legende:** `Trainingsart`.
* **Zweck:** Visualisiert die Umfänge, z.B. hohe Kilometerzahlen beim Radfahren im Vergleich zum Schwimmen.

## Bereich 5: Footer
Am unteren Rand befindet sich eine Infoleiste mit Autoren-Informationen und Rolle ("Senior Data Scientist").