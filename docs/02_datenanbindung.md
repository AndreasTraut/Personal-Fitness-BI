# 🔌 Datenanbindung: Lokaler OneDrive-Sync

Diese Anleitung beschreibt die **lokale Verbindung** zur Datenquelle. Dies ist die robusteste Methode für persönliche Dashboards ("Personal BI"), da sie Authentifizierungsprobleme mit privaten Microsoft-Konten umgeht.

## 1. Das Prinzip (Local File)
Anstatt dass Power BI versucht, sich über das Internet in dein OneDrive einzuloggen, nutzen wir den **OneDrive-Synchronisierungs-Client** auf deinem PC.

* **Voraussetzung:** OneDrive läuft auf deinem PC und die Datei `Fitness.xlsx` ist lokal synchronisiert.
* **Vorteil:** Extrem stabil, keine URL-Manipulation nötig, schnellerer Import.
* **Nachteil:** Aktualisierung funktioniert nur, wenn dein PC läuft und OneDrive aktiv ist.

## 2. Einrichtung in Power BI
1.  Klicke in Power BI Desktop auf **"Daten abrufen"**.
2.  Wähle **"Excel-Arbeitsmappe"** (Nicht "Web"!).
3.  Navigiere in deinem Datei-Explorer zu deinem lokalen OneDrive-Ordner:
    `C:\Users\DeinName\OneDrive\...\Trainingseintrag.xlsx`
4.  Wähle im Navigator **OfficeForms.Table** aus (Das Icon mit dem blauen Header).

## 3. Power Query Transformation (M-Code)
Für eine saubere Datenbasis wurden direkt beim Import technische Spalten von Microsoft Forms entfernt und Datentypen angepasst.

Hier ist der verwendete **M-Code** (Erweiterter Editor):

```powerquery
let
    // 1. Zugriff auf die lokale Datei (Pfad ist benutzerspezifisch!)
    Quelle = Excel.Workbook(File.Contents("C:\Users\andre\OneDrive\Trainingseintrag.xlsx"), null, true),
    
    // 2. Navigation zur strukturierten Tabelle aus Forms
    Tabelle1_Table = Quelle{[Item="OfficeForms.Table",Kind="Table"]}[Data],
    
    // 2b. Komma durch Punkt ersetzen (für deutsche Dezimaltrennzeichen)
    #"Ersetzter Wert" = Table.ReplaceValue(Tabelle1_Table,",",".",Replacer.ReplaceText,{"Distanz (in km)"}),
    
    // 3. Datentypen sauber definieren (Text vs. Zahl vs. Datum)
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Ersetzter Wert",{
        {"Id", Int64.Type}, 
        {"Startzeit", type datetime}, 
        {"Fertigstellungszeit", type datetime}, 
        {"E-Mail", type text}, 
        {"Name", type any}, 
        {"Wann war das Training?", type date}, 
        {"Was hast du gemacht?", type text}, 
        {"Dauer (in Minuten)", Int64.Type}, 
        {"Distanz (in km)", Int64.Type}, 
        {"Durchschnitts-Puls", Int64.Type}, 
        {"Beschreibung / Notizen", type text}}
    ),
    
    // 4. Cleanup: Technische Forms-Spalten entfernen (Datenschutz & Speicher)
    #"Entfernte Spalten" = Table.RemoveColumns(#"Geänderter Typ",{"Startzeit", "Fertigstellungszeit", "E-Mail", "Name"}),
    
    // 5. Umbenennung für das Sternschema
    #"Umbenannte Spalten" = Table.RenameColumns(#"Entfernte Spalten",{
        {"Was hast du gemacht?", "Trainingsart"}, 
        {"Wann war das Training?", "Datum"}
    })
in
    #"Umbenannte Spalten"

```
> ⚠️ **Wichtiger Hinweis:** Der Pfad unter File.Contents ist absolut. Wenn das Projekt auf einen anderen PC umzieht oder der Benutzername abweicht, muss dieser Pfad in den Datenquelleneinstellungen angepasst werden.


## Alternative: Der "Web"-Connector (Cloud-Direktzugriff)
Es gibt eine alternative Methode, bei der Power BI die Datei direkt aus der Cloud lädt, ohne lokalen Sync. Diese wird oft in Unternehmensumgebungen bevorzugt.

* **Connector:** Web (statt Excel).
* **Vorgehen:** Man verwendet den OneDrive-Link und entfernt den Parameter ?web=1 am Ende.
* **Herausforderung:** Bei privaten Microsoft-Konten (Personal OneDrive) führt diese Methode oft zu Authentifizierungsfehlern oder erfordert spezielle "Embed"-Links, die sich dynamisch ändern können.

**Empfehlung:** Für dieses Projekt wird die lokale Methode (siehe oben) empfohlen, da sie Fehlerquellen minimiert und keine komplexen Workarounds erfordert.