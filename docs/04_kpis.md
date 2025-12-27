# 🧮 Wichtige Measures 

Berechnungen finden nicht in Excel, sondern dynamisch in Power BI statt:

**Dauer (Std):**
```dax
Dauer (Std) = SUM('Training'[Dauer_Minuten]) / 60
```

**Ø kmh:**
```dax
Ø kmh = DIVIDE( SUM('Training'[Distanz_km]), [Dauer (Std)], 0 )
```

**Effizienz:**
```dax
Effizienz = DIVIDE( [Ø kmh], AVERAGE('Training'[Herzfrequenz]), 0 )
```
