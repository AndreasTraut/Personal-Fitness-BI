# 🏃‍♂️ Personal Fitness BI

**Automatisierte Fitness-Analyse: Von der mobilen Eingabe bis zum Dashboard.**

Eine vollständige End-to-End Business Intelligence Lösung für persönliches Fitnesstracking. Dieses Projekt ersetzt manuelle Excel-Listen durch einen automatisierten Workflow mit Microsoft Forms, Excel Online und Power BI.

---

## 🎯 Über das Projekt

Ziel dieses Projekts ist es, die Hürde der Datenerfassung zu minimieren und gleichzeitig professionelle Analysen zu ermöglichen. Anstatt Trainingsdaten mühsam am PC einzutippen, nutzt dieses System eine mobile "App" (MS Forms), die Daten in die Cloud synchronisiert, wo sie von Power BI in ein Sternschema transformiert und visualisiert werden.

### Highlights & Features

* **Mobile-First Datenerfassung:** Eingabe von Dauer, Distanz und Herzfrequenz in < 30 Sekunden via Microsoft Forms.
* **Professionelles Datenmodell:** Nutzung eines Sternschemas (Star Schema) mit dedizierten Fakten- und Dimensionstabellen für performante Abfragen.
* **Soll-Ist-Vergleiche:** Integration von Planzahlen (Zielen) und Visualisierung der Abweichungen mittels Bullet-Charts.
* **Effizienz-Metriken:** Berechnung des "Efficiency Index" (Verhältnis von Geschwindigkeit zu Herzfrequenz) zur objektiven Fitnessbewertung.
* **Sportarten-Normalisierung:** Vergleichbarkeit verschiedener Sportarten (z.B. Schwimmen vs. Laufen) durch gewichtete Intensitätsfaktoren ("Fitness Points").

---

## ⚙️ Architektur & Workflow

Der Datenfluss ist vollständig automatisiert ("Low-Code ETL"):

```mermaid
graph LR
    User[📱 User / Smartphone] -->|Eingabe| Forms[📝 MS Forms]
    Forms -->|Sync| Excel[☁️ Excel auf OneDrive]
    Excel -->|Power Query| PBI[📊 Power BI Desktop]
    PBI -->|Visualisierung| Dash[📈 Dashboard & Reports]
```

1.  **Input:** User trägt Training in Forms ein.
2.  **Storage:** Forms speichert Daten automatisch in einer Excel-Tabelle auf OneDrive.
3.  **Processing:** Power BI zieht die Daten via Web-Connector (ohne lokalen Download).
4.  **Output:** Interaktive Dashboards mit Drill-Down-Funktionen.

---

## 📂 Dokumentation

Die detaillierte Anleitung zur Replikation des Projekts findest du in den Docs:

* **[Einrichtung & Setup](docs/01_setup_forms.md)** – Wie man Forms und Excel verbindet.
* **[Datenmodellierung](docs/02_datenmodell.md)** – Erklärungen zu Fakten, Dimensionen und Beziehungen.
* **[KPIs & Logik](docs/03_kpis.md)** – Deep-Dive in DAX-Formeln für Zielerreichung und Normalisierung.
* **[Visualisierung](docs/Fitness_Visualisierung.md)** – Aufbau des Dashboards und Z-Layout.

---

## 🚀 Quick Start

1.  Repository klonen oder herunterladen.
2.  Excel-Template (`/templates/Fitness_Tracker_Template.xlsx`) auf dein OneDrive hochladen.
3.  Microsoft Form erstellen und mit dem Excel verknüpfen.
4.  Power BI Datei (`Fitness_Dashboard.pbix`) öffnen.
5.  Datenquelle in den Einstellungen auf deinen eigenen OneDrive-Link ändern.
6.  Auf **Aktualisieren** klicken.

---

## 🛠 Tech Stack

* **Frontend:** Microsoft Forms (Web & Mobile)
* **Database:** Microsoft Excel Online (via OneDrive for Business/Personal)
* **Analytics Engine:** Power BI Desktop
* **Languages:** DAX (Data Analysis Expressions), M (Power Query)
