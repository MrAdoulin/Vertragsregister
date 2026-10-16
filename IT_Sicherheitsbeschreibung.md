# Vertragsregister V8.8c – Technische Beschreibung für die IT-Sicherheitsprüfung

Stand 16.10.2026. Die Quellen liegen bei: `Bauquellen_V8_8c.zip`. Die `.odb` wird vollständig aus diesen Quellen erzeugt (`build_base.py`). Darin gibt es keine händischen Änderungen.

## 1. Bestandteile

| Teil | Technik | Ort |
|---|---|---|
| Datenbank | Firebird 3, eingebettet (LibreOffice-Treiber `sdbc:embedded:firebird`) | In der `.odb` (`database/firebird.fbk`). Kein Server, kein Netzwerkport. |
| Oberfläche | Ein LibreOffice-Base-Formulardokument | In der `.odb` (`forms/Obj11`) |
| Logik | LibreOffice-Basic-Bibliothek `Standard`, Modul `App` (≈ 9 000 Zeilen) | In der `.odb` (`Basic/Standard/App.xml`). Klartext, Quellen siehe `v8/app/*.bas`. |
| Zeitgeber | Python-Skript `zeitgeber.py` (40 Zeilen) | In der `.odb` (`Scripts/python/zeitgeber.py`) |

- Es gibt keine Add-ons, keine Extensions, kein Java und keine DLLs.
- Es werden keine Fremdbibliotheken nachgeladen.

## 2. Ausführung und Rechte

- **Makros sind nötig.** Ohne Makros startet die Oberfläche nicht.
  - Vorgesehen ist die LibreOffice-Makrosicherheit „Hoch“ mit genau einem **vertrauenswürdigen Speicherort**: dem Ordner der `.odb`.
  - Die Stufen „Niedrig“ oder „Mittel“ sind nicht erforderlich.
- Die Makros laufen mit den Rechten des angemeldeten Windows-Benutzers. Eine Rechteerhöhung gibt es nicht.
- Startereignis: `OnLoad` der `.odb` → `AppStart`. Das öffnet das Formular und blendet das Base-Fenster aus.
- Python wird nur für eine Verzögerung genutzt.
  - `threading.Timer`, danach Rückruf ins Basic über `com.sun.star.awt.AsyncCallback`.
  - Das Skript liest und schreibt keine Dateien und öffnet keine Verbindungen.
  - Fehlt Python, zeigt LibreOffice seinen normalen Tooltip ohne einstellbare Verzögerung.

## 3. Dateizugriffe (alle lokal oder auf eingetragene Laufwerke)

| Zweck | Pfad | Art |
|---|---|---|
| Daten speichern | die `.odb` selbst | lesen und schreiben |
| Sicherung (Spiegel, Textsicherung, Prüfprotokoll) | `Sicherung\` neben der `.odb` oder der Ordner aus den Einstellungen | schreiben |
| Exporte (Kalender `.ics`, Tabellen `.ods`, Statistik) | `Export\` neben der `.odb` | schreiben |
| Fehler- und Wunschmeldungen (Text, Bildschirmfoto `.png`) | `Meldungen\` neben der `.odb` | schreiben, nur auf Klick |
| Ablage prüfen, Ordner und Dokumente öffnen | Ablagepfad aus den Einstellungen (z. B. Netzlaufwerk) | Verzeichnis lesen. Öffnen über `com.sun.star.system.SystemShellExecute`, also die Windows-Zuordnung, nur auf Klick. |

## 4. Netz und Datenschutz

- Das Register baut **keine Netzwerkverbindungen** auf.
  - Es gibt keine Telemetrie, keine Updates und keinen Mailversand.
  - Die Notes-Termine sind eine `.ics`-Datei, die der Benutzer selbst importiert.
- Gespeichert werden personenbezogene Daten:
  - Namen und Kontaktdaten von Ansprechpersonen der Partner.
  - Der Windows- bzw. LibreOffice-Benutzername in der Änderungsspur.
- Bildschirmfotos (Funktion „Melden“) können Vertragsdaten zeigen. Sie bleiben im Ordner `Meldungen`.
- **Keine Verschlüsselung.** Die `.odb` ist ein ZIP-Archiv. Schützen Sie Ordner und Laufwerk über NTFS-Rechte oder BitLocker.

## 5. Integrität

- Die Datenbank prüft Pflichtfelder und Plausibilität über Trigger und Prüfregeln. Zusätzlich gibt es Prüfmakros vor dem Speichern.
- Löschen geschieht mit Rückfrage, Gelöschtes kommt in den Papierkorb. Einträge werden storniert statt verändert (Korrigieren/Stornieren).
- Die Änderungsspur protokolliert Datum, Benutzer, Feld, alten und neuen Wert.
- Mehrbenutzerbetrieb ist **nicht** vorgesehen. Die Datei darf nur einmal gleichzeitig geöffnet sein, denn die eingebettete Datenbank sperrt sie.

## 6. Prüfhinweise

- Makros einsehen: in LibreOffice unter Extras › Makros › Basic verwalten › Vertragsregister.odb › Standard › App. Alternativ in den Quellen `app/*.bas`.
- Gesucht werden kann nach `SystemShellExecute`, `Open … For Output` und `store`. Das sind alle Stellen mit Wirkung nach außen.
- Getestet ist das Register mit LibreOffice 26.2 (Windows 11) und 24.2.7 (Linux).
