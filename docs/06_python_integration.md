# 🐍 Python Integration: Synthetic Data & Advanced Metrics

## Warum Python in Power BI?
Während Power Query (M) hervorragend für die Datenreinigung (ETL) geeignet ist, stoßen wir bei komplexer Logik oder statistischen Aufgaben oft an Grenzen. In Version 1.1 dieses Repositories wurde **Python** direkt in die Power Query Pipeline integriert.

Der Anwendungsfall in diesem Template:
1.  **Simulation fehlender Daten (Mocking):** Das aktuelle Excel-Template erfasst nur Cardio-Daten (Dauer, Distanz). Um das Dashboard für Krafttraining zu testen, ohne Wochen auf echte Daten zu warten, nutzen wir Python.
2.  **Advanced Feature Engineering:** Berechnung des "One Rep Max" (1RM) basierend auf wissenschaftlichen Formeln (Epley).

## Der Python-Workflow

Der Python-Step wird ausgeführt, **nachdem** die Daten aus OneDrive geladen wurden, aber **bevor** sie ins Datenmodell geladen werden.

### Voraussetzungen
* Lokale Python-Installation.
* Benötigte Bibliotheken: `pandas`, `numpy`.
* In Power BI unter *Optionen -> Python-Skripterstellung* muss der Pfad hinterlegt sein.

### Das Skript
Das folgende Skript wird im "Transform Data" (Power Query) Editor als "Run Python Script" Schritt eingefügt. 

```python
import pandas as pd
import numpy as np

# 'dataset' ist der DataFrame, der von Power Query übergeben wird
df = dataset.copy()

# 1. Initialisierung der neuen Spalten
df['Weight'] = 0.0
df['Reps'] = 0

# 2. Identifikation der Krafttraining-Sessions
# WICHTIG: Wir nutzen den 'TrainingArt_Key' (4 = Krafttraining) statt Text,
# da die Textspalte im ETL-Prozess ggf. umbenannt wird.
if 'TrainingArt_Key' in df.columns:
    is_kraft = df['TrainingArt_Key'] == 4
    count_kraft = is_kraft.sum()

    # 3. Erzeugung synthetischer Daten (Nur wenn Krafttraining vorhanden ist)
    if count_kraft > 0:
        # Wir simulieren Gewichte zwischen 40kg und 90kg
        df.loc[is_kraft, 'Weight'] = np.random.randint(40, 90, size=count_kraft)
        # Wir simulieren Wiederholungen zwischen 5 und 12
        df.loc[is_kraft, 'Reps'] = np.random.randint(5, 12, size=count_kraft)

# 4. Berechnung des One Rep Max (1RM) nach Epley-Formel
# 1RM = Gewicht * (1 + Reps / 30)
df['Estimated_1RM'] = df.apply(
    lambda row: row['Weight'] * (1 + row['Reps'] / 30) if row['Reps'] > 0 else 0,
    axis=1
)

# Runden für saubere Anzeige
df['Estimated_1RM'] = df['Estimated_1RM'].round(2)

# Rückgabe an Power BI
print(df)
```