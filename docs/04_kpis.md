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

**Dauer (Std):** Rechnet die Minuten aus Forms in Stunden um.

```dax
Dauer (Std) = SUM('fact_Training'[Dauer (in Minuten)]) / 60
```

**Distanz (km):** Die Summe der zurückgelegten Kilometer über alle ausgewählten Trainingseinheiten.
```dax
Distanz (km) = SUM('fact_Training'[Distanz (in km)])
```

**Trainingseinheiten:** Zählt die Anzahl der einzigartigen Tage, an denen ein Training stattgefunden hat (nutzt `DISTINCTCOUNT`, um doppelte Einträge pro Tag nur einfach zu zählen)
```dax
Trainingseinheiten = DISTINCTCOUNT( fact_Training[ID])
```

**Ø kmh:** Berechnet die Durchschnittsgeschwindigkeit. Nutzt DIVIDE zur sicheren Division (vermeidet Fehler bei Division durch Null).

```dax
Ø kmh = DIVIDE( SUM('fact_Training'[Distanz (in km)]), [Dauer (Std)], 0 )
```

**Ø Puls:** Ermittelt den durchschnittlichen Puls über die Anzahl der Trainingseinheiten.

```dax
Ø Puls = DIVIDE( SUM('fact_Training'[Durchschnitts-Puls]), [Trainingseinheiten], 0 )
```

***Effizienz:*** Ein Index für das Verhältnis von Leistung (Speed) zu Aufwand (Puls).

```dax
Effizienz = DIVIDE( [Ø kmh], AVERAGE('fact_Training'[Herzfrequenz]), 0 )
```