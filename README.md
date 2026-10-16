# Vertragsregister – Schnellstart

Das Vertragsregister ist **eine einzige Datei**: `Vertragsregister.odb`.
Darin stecken die Datenbank, die Oberfläche und die Hilfe. Sie brauchen keinen Server und kein Internet.

Die Datei enthält **Beispieldaten** zum Ausprobieren. Wenn Sie selbst anfangen wollen, löschen Sie diese über
⚙ Einstellungen › Karte „Beispieldaten und Anzeige“ › „Beispieldaten entfernen“.

## 1. Was Sie brauchen

- Einen Windows-PC.
- **LibreOffice**, kostenlos von https://de.libreoffice.org. Bei der Installation „Standard“ wählen. Darin sind Base und Python schon enthalten.

## 2. Einrichten (einmalig, etwa 5 Minuten)

1. Legen Sie einen Ordner an, zum Beispiel `C:\Vertragsregister\`.
2. Kopieren Sie `Vertragsregister.odb` in diesen Ordner.
3. Tragen Sie den Ordner in LibreOffice als **vertrauenswürdig** ein. Ohne diesen Schritt startet das Register nicht.
   1. Öffnen Sie LibreOffice.
   2. Gehen Sie zu Extras › Optionen › LibreOffice › Sicherheit
   3. Wählen Sie den Reiter **Vertrauenswürdige Quellen** (GPG-Zertifikatmanager)
   4. Klicken Sie bei „Vertrauenswürdige Speicherorte“ auf **Hinzufügen** und wählen Sie Ihren Ordner, bzw. das Vertragsregister am konkreten Speicherort aus.
   5. Schließen Sie das Fenster mit OK. 
Optional/Alternativ: in Libre über den Pfad Extras › Optionen › LibreOffice › Sicherheit › Makrosicherheit die Makrosicherheit auf "Mittel" stellen und jedes mal beim Öffnen des Vertragsregisters auf "Makros aktivieren" klicken um das Programm zu starten.

4. Tragen Sie Ihren **Namen** ein. Das Register vermerkt ihn bei jeder Änderung.
   - Der Ort dafür: Extras › Optionen › LibreOffice › Benutzerdaten › Vorname/Name.
5. Öffnen Sie `Vertragsregister.odb` mit einem Doppelklick. Das Register öffnet sich in einem eigenen Fenster.
6. Gehen Sie in die Einstellungen (Zahnrad ⚙ unten links). Tragen Sie dort den **Ablagepfad** ein, also den Ordner, in dem Ihre Vertragsordner liegen. Klicken Sie danach auf „Sichern“.

## 3. Bedienen

- **Links** wählen Sie die Ansicht: Übersicht, Verträge, Leistungen, Rechnungsverlauf, Partner und so weiter.
- **„Sichern“** (oder Strg+S) speichert. Beim Wechsel der Ansicht speichert das Register ohnehin.
- Felder mit **\*** müssen ausgefüllt sein.
- Ein Klick auf das kleine **„H“** setzt das heutige Datum ein.
- **F1** öffnet die Hilfe mit allen Tastenkürzeln. Die wichtigsten:
  - Strg+1 … Strg+9 wechseln die Ansicht.
  - Strg+N legt einen neuen Eintrag an.
  - Strg+F sucht in der aktuellen Ansicht.
  - Strg+Umschalt+F durchsucht alles.
- Die **Zeile ganz unten** zeigt, was zuletzt geschehen ist und was als Nächstes zu tun ist.
- **„Beenden“** unten links speichert und schließt das Register.

## 4. Sicherung

- Das Register legt neben der Datei einen Ordner `Sicherung` an. Darin liegen ein Spiegel und eine Textsicherung.
- Ein anderer Speicherort lässt sich in den Einstellungen festlegen. Am besten ist ein anderes Laufwerk oder ein USB-Stick.
- Kopieren Sie zusätzlich ab und zu die ganze `Vertragsregister.odb` an einen sicheren Ort.

## 5. Wenn etwas nicht geht

| Problem | Lösung |
|---|---|
| Es öffnet sich nur ein graues Base-Fenster, oder eine Makro-Warnung erscheint | Der Ordner ist nicht als vertrauenswürdig eingetragen (Schritt 2.3). |
| „Datei gesperrt“ | Das Register ist schon offen, eventuell in einem anderen Fenster. Öffnen Sie es nur einmal. |
| Etwas sieht falsch aus oder ein Wunsch | Klicken Sie unten links auf den Käfer 🐞 und danach auf das betroffene Element. Schreiben Sie einen Kommentar dazu. Die Meldungen landen im Ordner `Meldungen` neben der Datei. Sie kann später bei der Weiterentwicklung verwendet werden |# Vertragsregister
Vertragsregister zur Erfassung und Verwaltung von Verträgen in Libre Base
