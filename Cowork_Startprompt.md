Du übernimmst die Weiterentwicklung meines **Vertragsregisters**. Es ist eine LibreOffice-Base-Anwendung: eine `.odb` mit eingebetteter Firebird-Datenbank, Basic-Makros und einem kleinen Python-Zeitgeber.Sie wurde durch einen Freund gebaut, jetzt pflege ich sie selbst. Ich bin kein IT-Profi und arbeite unter Windows.

**Jetzt nur aufnehmen, nichts bearbeiten.**
1. Lies im freigegebenen Ordner in dieser Reihenfolge:
   - `Gesamtbeschreibung_und_Fortsetzung.md`
   - `Offene_Punkte_und_Umsetzungsideen.md`
   - `README_Schnellstart.md`
   - `IT_Sicherheitsbeschreibung.md`
2. Entpacke `Bauquellen_V8_8c.zip` in einen Unterordner `quellen\`.
   - Lies dort `doku/CLAUDE.md` und `doku/04_Fallstricke.md`.
   - Verschaff dir einen Überblick über `v8/` (Bau, Formulare, `app/*.bas`).
3. Ändere, baue oder starte noch nichts.
   - Öffne keine `.odb` und lege keine Dateien an. Ausnahme ist der entpackte Quellordner.
4. Antworte mir danach mit höchstens 10 Zeilen:
   - Was das Register kann.
   - Welche offenen Punkte es gibt (nur die Überschriften O1–O16).
   - Dass du bereit bist.
   Dann warte auf meine Nachricht.

**Regeln für später, wenn ich dir Aufgaben gebe:**
- Meine echte Datei mit meinen Daten (`Vertragsregister.odb`) änderst du **nie direkt**.
  - Du arbeitest in den Quellen und baust eine neue Datei.
  - Zum Testen nimmst du immer eine Kopie in einem Ordner `Test\`.
- Ein Neubau enthält nur Beispieldaten, keine echten Daten.
  - Muss eine Änderung in meine echte Datei, schlägst du den sichersten Weg vor und wartest auf mein OK.
  - Mögliche Wege: das Makromodul tauschen, oder Daten exportieren und importieren.
- Lösche oder verschiebe keine Dateien ohne mein ausdrückliches OK.
- Erkläre einfach und in kurzen Sätzen, ohne Fachjargon. Sag mir genau, was ich klicken oder ausführen soll.
- Bevor du größere Änderungen machst, beschreib kurz den Plan und frag nach meinem OK.
- Halte dich an die Bau- und Codemarken-Regeln in `doku/CLAUDE.md`:
  - Neue Version eintragen, zum Beispiel V8.9.
  - Kommentar mit Version an jeder Änderung.
  - Quellen nicht mit PowerShell 5.1 schreiben.
- Führe nach jeder Sitzung eine Datei `Arbeitslog.md` im Ordner fort. Darin steht: was gemacht wurde, was getestet ist, was offen bleibt.
- Meldungen aus dem Register (Käfer 🐞) liegen im Ordner `Meldungen\` neben der `.odb`. Wenn ich „schau dir die Meldungen an“ sage, lies `meldungen.txt` und die Bilder dazu.
