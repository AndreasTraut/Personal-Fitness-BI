# 📱 Einrichtung: Microsoft Forms & Excel

Diese Anleitung erklärt den Prozess, um die Datenerfassung einzurichten. Wir nutzen Excel Online als Datenbank und Microsoft Forms als Eingabe-Maske.

## 1. Das Excel-Grundgerüst (OneDrive)
Wir starten **nicht** in Forms, sondern in Excel Online. Das ist entscheidend, damit die Daten automatisch an einem definierten Ort in deinem OneDrive landen.

1.  Gehe am PC auf [onedrive.live.com](https://onedrive.live.com/) oder office.com.
2.  Erstelle eine **"Neue Excel-Arbeitsmappe"** und nenne sie z.B. `Fitness.xlsx`.
3.  Klicke im Menüband auf **"Einfügen"** -> **"Forms"** -> **"Neues Formular"**.

<img width="1264" height="495" alt="Form erstellen" src="https://github.com/user-attachments/assets/fe3189a3-a774-4942-8154-881411de261e" />

## 2. Das Formular gestalten
Es muss nun ein Formular erstellt werden, dass die folgenden Datenfelder ("Fragen") enhält mit den spezifischen Datentypen, um Fehler in Power BI zu vermeiden: 

| Frage | Typ | Wichtige Einstellung |
| :--- | :--- | :--- |
| **Datum** | Datum | "Erforderlich" ankreuzen. |
| **Sportart** | Auswahl | Optionen: Krafttraining, Laufen, Radfahren, Schwimmen etc.. |
| **Dauer (in Minuten)** | Text | Klicke auf `...` -> **Einschränkungen** -> Wähle **"Zahl"**. |
| **Distanz (in km)** | Text | Einschränkung: **"Zahl"**. |
| **Durchschnitts-Puls** | Text | Einschränkung: **"Zahl"**. |
| **Beschreibung** | Text | Option **"Lange Antwort"** aktivieren. |

> **Warum Einschränkungen?** Die Einstellung "Zahl" verhindert, dass Texte wie "1 Stunde" eingegeben werden, womit Power BI nicht rechnen könnte.

Dieses Formular erstellt man am einfachsten mit unterstützung einer KI - dem "Microsoft Copilot". Übergibt dieser KI folgende Anweisungen und sie erstellt das Formular automatisch für dich: 

 * Datum
   * Typ: Datum
   * Frage: "Wann war das Training?"
   * Einstellung: "Erforderlich" ankreuzen.
 * Sportart
   * Typ: Auswahl (Choice)
   * Frage: "Was hast du gemacht?"
   * Optionen: Krafttraining, Laufen, Radfahren, Schwimmen, Sonstiges.
   * Warum: So vermeidest du Schreibfehler (kein "Run" mal "Laufen"), was die Auswertung in Power BI viel leichter macht.
 * Dauer (in Minuten)
   * Typ: Text
   * Wichtig: Klicke unten rechts bei der Frage auf die drei Punkte (...) -> Einschränkungen.
   * Wähle: "Zahl".
   * Warum: Damit du nicht aus Versehen "1 Std" schreibst, womit Power BI nicht rechnen kann. Du willst nur die Zahl 60.
 * Distanz (in km)
   * Typ: Text
   * Einstellung: Wieder (...) -> Einschränkungen -> "Zahl".
   * (Tipp: Bei Krafttraining lässt du das Feld einfach leer).
 * Durchschnitts-Puls
   * Typ: Text
   * Einstellung: (...) -> Einschränkungen -> "Zahl".
 * Beschreibung / Notizen
   * Typ: Text
   * Option: "Lange Antwort" aktivieren (für Details wie "Intervalle, sehr anstrengend").

<img width="861" height="1032" alt="Formular Trainingseintrag" src="https://github.com/user-attachments/assets/05514e31-a1c6-4b28-855d-7eac4a5ea50e" />

## 3. Mobile Nutzung ("App"-Feeling)
1.  Klicke in Forms auf **"Antworten sammeln"** und kopiere den Link.
2.  Sende den Link an dein Smartphone.
3.  Öffne den Link im Handy-Browser (Safari/Chrome).
4.  Wähle im Browser-Menü **"Zum Home-Bildschirm hinzufügen"**. Du hast nun ein App-Icon für direkten Zugriff.

Du kannst nun Trainingseinheiten erfassen:

<img width="863" height="1033" alt="Training erfassen" src="https://github.com/user-attachments/assets/97b9c6c6-8e0f-4bb1-b0c7-febac6dfa75f" />

<img width="1702" height="427" alt="Trainingsdatensatz" src="https://github.com/user-attachments/assets/837b5674-c03d-4b4b-96e6-32cc8bc03764" />

