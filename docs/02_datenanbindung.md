# 🔌 Datenanbindung: Lokaler OneDrive-Sync

Diese Anleitung beschreibt die **lokale Verbindung** zur Datenquelle. Dies ist die robusteste Methode für persönliche Dashboards ("Personal BI"), da sie Authentifizierungsprobleme mit privaten Microsoft-Konten umgeht.

## 1. Das Prinzip (Local File)
Anstatt dass Power BI versucht, sich über das Internet in dein OneDrive einzuloggen, nutzen wir den **OneDrive-Synchronisierungs-Client** auf deinem PC.

* **Voraussetzung:** OneDrive läuft auf deinem PC und die Datei `Trainingseintrag.xlsx` ist lokal synchronisiert.
* **Vorteil:** Extrem stabil, keine URL-Manipulation nötig, schnellerer Import.
* **Nachteil:** Aktualisierung funktioniert nur, wenn dein PC läuft und OneDrive aktiv ist.

## 2. Einrichtung in Power BI
1.  Klicke in Power BI Desktop auf **"Daten abrufen"**.
2.  Wähle **"Excel-Arbeitsmappe"** (Nicht "Web"!).
3.  Navigiere in deinem Datei-Explorer zu deinem lokalen OneDrive-Ordner:
    `C:\Users\DeinName\OneDrive\...\Trainingseintrag.xlsx`
4.  Wähle im Navigator **OfficeForms.Table** aus (Das Icon mit dem blauen Header).

## 3. Power Query Transformation (M-Code)
Die Staging-Tabelle **source_Training** lädt die Rohdaten aus Excel und bereitet sie für das Sternschema vor. Dabei werden Spalten umbenannt, ein Integer-Datenschlüssel erstellt und Datentypen korrekt gesetzt.

Hier ist der verwendete **M-Code** für die Tabelle `source_Training`:

```powerquery
let
    // 1. Zugriff auf die lokale Datei (Pfad ist benutzerspezifisch!)
    Quelle = Excel.Workbook(File.Contents("C:\Users\andre\OneDrive\Trainingseintrag.xlsx"), null, true),

    // 2. Navigation zur strukturierten Tabelle aus Forms
    Tabelle1_Table = Quelle{[Item="OfficeForms.Table",Kind="Table"]}[Data],

    // 3. Bereinigung der Dezimaltrennzeichen: Komma durch Punkt ersetzen, 
    // um die Spalte "Distanz (in km)" korrekt in eine Zahl umwandeln zu können
    #"Ersetzter Wert" = Table.ReplaceValue(Tabelle1_Table,",",".",Replacer.ReplaceText,{"Distanz (in km)"}),

    // 4. Spalten umbenennen: "Was hast du gemacht?" wird zu "Trainingsart" und 
    // "Wann war das Training?" zu "Datum", um das Modell lesbarer zu machen
    #"Umbenannte Spalten" = Table.RenameColumns(#"Ersetzter Wert",{{"Was hast du gemacht?", "Trainingsart"}, {"Wann war das Training?", "Datum"}}),

    // 5. Erstellung des Integer-Schlüssels (DateKey): 
    // Berechnung eines YYYYMMDD Wertes aus dem Datum für die performante Relation zur Kalendertabelle
    #"Hinzugefügte benutzerdefinierte Spalte" = Table.AddColumn(#"Umbenannte Spalten", "Datum Nr", each Date.Year([Datum]) * 10000 + 
    Date.Month([Datum]) * 100 + 
    Date.Day([Datum])),

    // 6. Typkonvertierung: Festlegung der Datentypen für alle Spalten, 
    // insbesondere Ganzzahlen für ID/Dauer/Puls und Dezimalzahlen für die Distanz
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Hinzugefügte benutzerdefinierte Spalte",{
        {"Startzeit", type datetime}, 
        {"Fertigstellungszeit", type datetime}, 
        {"Datum", type datetime}, 
        {"Id", Int64.Type}, 
        {"E-Mail", type text}, 
        {"Name", type text}, 
        {"Trainingsart", type text}, 
        {"Dauer (in Minuten)", Int64.Type}, 
        {"Distanz (in km)", type number}, 
        {"Durchschnitts-Puls", Int64.Type}, 
        {"Beschreibung / Notizen", type text}, 
        {"Datum Nr", Int64.Type}
    })
in
    #"Geänderter Typ"

```
> ⚠️ **Wichtiger Hinweis:** Der Pfad unter File.Contents ist absolut. Wenn das Projekt auf einen anderen PC umzieht oder der Benutzername abweicht, muss dieser Pfad in den Datenquelleneinstellungen angepasst werden.


## Alternative: Der "Web"-Connector (Cloud-Direktzugriff)
Es gibt eine alternative Methode, bei der Power BI die Datei direkt aus der Cloud lädt, ohne lokalen Sync. Diese wird oft in Unternehmensumgebungen bevorzugt.

* **Connector:** Web (statt Excel).
* **Vorgehen:** Man verwendet den OneDrive-Link und entfernt den Parameter ?web=1 am Ende.
* **Herausforderung:** Bei privaten Microsoft-Konten (Personal OneDrive) führt diese Methode oft zu Authentifizierungsfehlern oder erfordert spezielle "Embed"-Links, die sich dynamisch ändern können.

**Empfehlung:** Für dieses Projekt wird die lokale Methode (siehe oben) empfohlen, da sie Fehlerquellen minimiert und keine komplexen Workarounds erfordert.