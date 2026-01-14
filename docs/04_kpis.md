# 🧮 Wichtige Measures & Kennzahlen

Die Berechnungen finden nicht in Excel, sondern dynamisch in Power BI mittels DAX (Data Analysis Expressions) statt.

## 1. Vorbereitung: Die Kennzahlentabelle
Um die Übersicht zu behalten, speichern wir keine Formeln in der Datentabelle, sondern in einer eigenen leeren Tabelle ("Dummy Table").

**M-Code für eine leere Tabelle:**
Nutze "Daten eingeben" oder füge diesen Code in eine leere Abfrage ein:

```powerquery
let
    // Erzeugt eine leere Tabelle als Container für Measures
    Quelle = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i44FAA==", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [#"Spalte 1" = _t]),
    #"Geänderter Typ" = Table.TransformColumnTypes(Quelle,{{"Spalte 1", type text}})
in
    #"Geänderter Typ"	
```

> **Tipp:** Nenne die Tabelle `_Kennzahlen`. Lösche danach die `Spalte 1` aus dem Modell, damit Power BI das Icon zu einem Taschenrechner ändert.


## 2. Die DAX-Formeln
Füge diese Measures der neuen Tabelle hinzu:

### 2.1 Dauer (Std)
**Zweck:** Rechnet die Minuten aus Forms in Stunden um, um eine bessere Lesbarkeit im Dashboard zu erreichen.

```dax
Dauer (Std) = SUM('fact_Training'[Dauer (in Minuten)]) / 60
```

**Erklärung:**
- `SUM('fact_Training'[Dauer (in Minuten)])` aggregiert alle Minutenwerte aus der Faktentabelle im aktuellen Filterkontext (z.B. pro Monat, Woche oder Sportart)
- Die Division durch `60` konvertiert Minuten in Stunden
- Das Measure respektiert automatisch alle aktiven Filter (Slicer, Zeilen/Spalten in Visuals)

---

### 2.2 Distanz (km)
**Zweck:** Summiert die zurückgelegten Kilometer über alle ausgewählten Trainingseinheiten.

```dax
Distanz (km) = SUM('fact_Training'[Distanz (in km)])
```

**Erklärung:**
- Einfache Aggregation aller Distanzwerte im aktuellen Kontext
- Bei nicht-Ausdauersportarten (z.B. Krafttraining) ist der Wert oft 0 oder leer – `SUM` behandelt dies korrekt
- Wird in Kombination mit Filtern dynamisch berechnet (z.B. nur "Radfahren" in KW 12)

---

### 2.3 Trainingseinheiten
**Zweck:** Zählt die Anzahl der einzigartigen Trainings-Sessions.

```dax
Trainingseinheiten = DISTINCTCOUNT( fact_Training[ID] )
```

**Erklärung:**
- `DISTINCTCOUNT` zählt nur eindeutige Werte in der ID-Spalte
- Wichtig: Anders als `COUNT` oder `COUNTROWS` verhindert dies Doppelzählungen, falls die Faktentabelle jemals Duplikate enthält
- Die ID ist der Primary Key der Faktentabelle und repräsentiert genau eine Trainingseinheit

---

### 2.4 Ø km/h
**Zweck:** Berechnet die Durchschnittsgeschwindigkeit über alle Trainings hinweg.

```dax
Ø kmh = DIVIDE( SUM('fact_Training'[Distanz (in km)]), [Dauer (Std)], 0 )
```

**Erklärung:**
- **Formel:** Geschwindigkeit = Distanz / Zeit
- `DIVIDE(Zähler, Nenner, AlternativWert)` ist die sichere DAX-Funktion für Divisionen
- Der dritte Parameter `0` definiert den Rückgabewert bei Division durch Null (verhindert Fehler bei Krafttraining ohne Distanz)
- Das Measure referenziert `[Dauer (Std)]` – ein anderes Measure – was die Modularität erhöht

**Hinweis:** Dieses Measure ist nur bei Ausdauersportarten (Laufen, Radfahren) aussagekräftig.

---

### 2.5 Ø Puls
**Zweck:** Ermittelt den durchschnittlichen Puls über alle Trainingseinheiten im Kontext.

```dax
Ø Puls = DIVIDE( SUM('fact_Training'[Durchschnitts-Puls]), [Trainingseinheiten], 0 )
```

**Erklärung:**
- Berechnet den **gewichteten Durchschnitt** über alle Sessions (nicht den Durchschnitt der Durchschnitte!)
- `SUM('fact_Training'[Durchschnitts-Puls])` addiert alle Pulswerte
- Division durch `[Trainingseinheiten]` ergibt den Mittelwert
- **Warum nicht `AVERAGE`?** Weil `AVERAGE` den Durchschnitt pro Zeile berechnet, was bei unterschiedlich langen Trainings zu Verzerrungen führen kann

**Beispiel:**  
Training 1: 140 bpm  
Training 2: 160 bpm  
→ Ø Puls = (140 + 160) / 2 = 150 bpm

---

### 2.6 Effizienz
**Zweck:** Ein Fitness-Index, der das Verhältnis von Leistung (Geschwindigkeit) zum Aufwand (Herzfrequenz) misst.

```dax
Effizienz = DIVIDE( [Ø kmh], AVERAGE('fact_Training'[Durchschnitts-Puls]), 0 )
```

**Erklärung:**
- **Konzept:** Je höher die Geschwindigkeit bei gleichem Puls, desto effizienter ist das Training
- Zähler: `[Ø kmh]` – die durchschnittliche Geschwindigkeit (Measure-Referenz)
- Nenner: `AVERAGE('fact_Training'[Durchschnitts-Puls])` – der durchschnittliche Puls (direkte Aggregation)
- Ein steigender Effizienz-Wert im Zeitverlauf zeigt Trainingsfortschritt an

**Interpretation:**  
- Effizienz = 0.10 → Bei 150 bpm läuft man 15 km/h  
- Effizienz = 0.12 → Bei 150 bpm läuft man 18 km/h (= bessere Fitness)

**Hinweis:** Dieses Measure ist ein vereinfachter Index und ersetzt keine medizinische Leistungsdiagnostik. Es dient der Trendbeobachtung.


### 2.7 Kraftwerte (Strength Metrics)
Dank der Python-Integration stehen nun auch Kraftwerte zur Verfügung.

**Ø 1RM (kg)**
Das durchschnittliche geschätzte Maximalgewicht über den gewählten Zeitraum.

```dax
Ø 1RM (kg) = AVERAGE('fact_Training'[Estimated_1RM])
```

**Max 1RM (kg)**
Der absolute Rekordwert (Personal Best) im gewählten Zeitraum.
```dax
Max 1RM (kg) = MAX('fact_Training'[Estimated_1RM])
```