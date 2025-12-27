# 📱 Einrichtung: Microsoft Forms & Excel

Diese Anleitung erklärt den Prozess, um die Datenerfassung einzurichten. Wir nutzen Excel Online als Datenbank und Microsoft Forms als Eingabe-Maske.

## 1. Das Excel-Grundgerüst (OneDrive)
Wir starten **nicht** in Forms, sondern in Excel Online. Das ist entscheidend, damit die Daten automatisch an einem definierten Ort in deinem OneDrive landen.

1.  Gehe am PC auf [onedrive.live.com](https://onedrive.live.com/) oder office.com.
2.  Erstelle eine **"Neue Excel-Arbeitsmappe"** und nenne sie z.B. `Fitness_Tracker.xlsx`.
3.  Klicke im Menüband auf **"Einfügen"** -> **"Forms"** -> **"Neues Formular"**.

## 2. Das Formular gestalten
Erstelle folgende Fragen mit den spezifischen Datentypen, um Fehler in Power BI zu vermeiden:

| Frage | Typ | Wichtige Einstellung |
| :--- | :--- | :--- |
| **Datum** | Datum | "Erforderlich" ankreuzen. |
| **Sportart** | Auswahl | Optionen: Laufen, Schwimmen, Radfahren, etc.. |
| **Dauer (in Minuten)** | Text | Klicke auf `...` -> **Einschränkungen** -> Wähle **"Zahl"**. |
| **Distanz (in km)** | Text | Einschränkung: **"Zahl"**. |
| **Durchschnitts-Puls** | Text | Einschränkung: **"Zahl"**. |
| **Beschreibung** | Text | Option **"Lange Antwort"** aktivieren. |

> **Warum Einschränkungen?** Die Einstellung "Zahl" verhindert, dass Texte wie "1 Stunde" eingegeben werden, womit Power BI nicht rechnen könnte.

## 3. Mobile Nutzung ("App"-Feeling)
1.  Klicke in Forms auf **"Antworten sammeln"** und kopiere den Link.
2.  Sende den Link an dein Smartphone.
3.  Öffne den Link im Handy-Browser (Safari/Chrome).
4.  Wähle im Browser-Menü **"Zum Home-Bildschirm hinzufügen"**. Du hast nun ein App-Icon für direkten Zugriff.

## 4. Verbindung zu Power BI
1.  Öffne die Excel-Datei in OneDrive -> **Datei** -> **Informationen** -> **Pfad kopieren**.
2.  Öffne Power BI Desktop -> **Daten abrufen** -> **Web** (Nicht Excel!).
3.  Füge den Link ein und **LÖSCHE** den Teil `?web=1` am Ende.
