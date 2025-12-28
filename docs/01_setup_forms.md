# 📱 Einrichtung: Microsoft Forms & Excel

Diese Anleitung erklärt den Prozess, um die Datenerfassung einzurichten. Wir nutzen Excel Online als Datenbank und Microsoft Forms als Eingabe-Maske.

## 1. Das Excel-Grundgerüst (OneDrive)
Wir starten **nicht** in Forms, sondern in Excel Online. Das ist entscheidend, damit die Daten automatisch an einem definierten Ort in deinem OneDrive landen.

1.  Gehe am PC auf [onedrive.live.com](https://onedrive.live.com/) oder office.com.
2.  Erstelle eine **"Neue Excel-Arbeitsmappe"** und nenne sie z.B. `Fitness.xlsx`.
3.  Klicke im Menüband auf **"Einfügen"** -> **"Forms"** -> **"Neues Formular"**.

<img width="1264" height="495" alt="Form erstellen" src="https://github.com/user-attachments/assets/fe3189a3-a774-4942-8154-881411de261e" />

## 2. Formular erstellen — schnell mit Microsoft Copilot

Du kannst ein Formular manuell anlegen — oder deutlich schneller: Microsoft Copilot verwenden, der dir das Formular direkt erstellt oder Schritt-für-Schritt anlegt. Im folgenden Abschnitt findest du eine klare Struktur, eine Beispiel-Prompt und Hinweise zur Validierung der Felder, damit die Daten sauber in Excel/Power BI landen.

### 2.1 Ziel-Felder & Datentypen
Erstelle ein Formular mit diesen Feldern und Typen (Kurzübersicht):

- **Datum** — Typ: Datum, Einstellung: *Erforderlich*
- **Sportart** — Typ: Auswahl (Choice), Optionen: Krafttraining, Laufen, Radfahren, Schwimmen, Sonstiges
- **Dauer (in Minuten)** — Typ: Zahl (Einschränkung auf Zahl)
- **Distanz (in km)** — Typ: Zahl (Einschränkung auf Zahl)
- **Durchschnitts-Puls** — Typ: Zahl (Einschränkung auf Zahl)
- **Beschreibung / Notizen** — Typ: Text, Option: *Lange Antwort*

Warum: Zahl-Felder vermeiden Text-Rauschen (z.B. "1 Std"), sodass Power BI problemlos Rechenoperationen durchführen kann.

### 2.2 Microsoft Copilot: so nutzt du es
1. Starte den Microsoft Copilot-Dialog 
2. Kopiere den untenstehenden Beispiel-Prompt in das Eingabefeld und sende sie an Copilot.

### 2.3 Beispiel-Prompts für Copilot
Ausführliche Variante (inkl. Hinweise zur Validierung):

"Erstelle ein Formular 'Trainingseintrag' mit folgenden Fragen: 1) 'Wann war das Training?' — Typ Datum, erforderlich; 2) 'Was hast du gemacht?' — Typ Auswahl (Krafttraining, Laufen, Radfahren, Schwimmen, Sonstiges), erforderlich; 3) 'Dauer (in Minuten)' — Typ Zahl, erforderlich; 4) 'Distanz (in km)' — Typ Zahl, optional; 5) 'Durchschnitts-Puls' — Typ Zahl, optional; 6) 'Beschreibung' — lange Textantwort, optional. Bitte setze für Zahlenfelder die Validierung auf Zahl und nutze klare, kurze Fragenamen."

### 2.4 Nacharbeiten & Kontrolle
- Prüfe nach Copilot-Erstellung jede Frage auf: Pflichtstatus, Zahl-Einschränkungen, und korrekte Auswahl-Optionen.
- Passe Bezeichnungen an (z. B. 'Dauer (in Minuten)' genau so schreiben), damit die Spaltennamen in Excel später konsistent sind.
- Teste das Formular kurz durch das Eintragen von Beispielwerten (Datum, 45, 5.2, 140, Kurzbeschreibung).


<img width="861" height="1032" alt="Formular Trainingseintrag" src="https://github.com/user-attachments/assets/05514e31-a1c6-4b28-855d-7eac4a5ea50e" />

## 3. Mobile Nutzung ("App"-Feeling)
1.  Klicke in Forms auf **"Antworten sammeln"** und kopiere den Link.
2.  Sende den Link an dein Smartphone.
3.  Öffne den Link im Handy-Browser (Safari/Chrome).
4.  Wähle im Browser-Menü **"Zum Home-Bildschirm hinzufügen"**. Du hast nun ein App-Icon für direkten Zugriff.

Du kannst nun Trainingseinheiten erfassen:

<img width="863" height="1033" alt="Training erfassen" src="https://github.com/user-attachments/assets/97b9c6c6-8e0f-4bb1-b0c7-febac6dfa75f" />

Im Excel `Fitness.xlsx` sind die eingegebenen Trainingsdatensätze dann sichtbar. Von dort werden sie dann nach PowerBI geladen. Siehe **[Datenanbindung](d02_datenanbindung.md)**  – Anleitung für den robusten lokalen Import (vs. Web-Connector).

<img width="1702" height="427" alt="Trainingsdatensatz" src="https://github.com/user-attachments/assets/837b5674-c03d-4b4b-96e6-32cc8bc03764" />

