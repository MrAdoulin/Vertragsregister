# Vertragsregister – Gesamtbeschreibung der bisherigen Arbeit und Anleitung zur Fortsetzung

Diese Datei ist für Sie oder eine KI, die das Register weiterentwickelt.
Geben Sie einer KI mit Dateizugriff (z. B. Claude Code) diese Datei und den entpackten Inhalt von `Bauquellen_V8_8c.zip`.
Sagen Sie ihr dazu: „Lies zuerst Gesamtbeschreibung_und_Fortsetzung.md und doku/CLAUDE.md.“

## 1. Zweck

Das Register verwaltet Verträge, die vereinbarten Leistungen, Rechnungen und Leistungsanfälle (Störungen).

- Ziel 1: Fristen nicht verpassen (Kündigungsstichtage, Prüfungen, Indexanpassungen).
- Ziel 2: Den richtigen, letztgültigen Vertrag schnell finden.
- Ziel 3: Rechnungen gegen Kostengrenzen und Leistungen prüfen.

Die Nutzer sind Laien unter Windows. Bedienung, Texte und Hilfe sind deshalb bewusst einfach gehalten.

## 2. Verlauf (08.–16.10.2026)

| Stand | Inhalt |
|---|---|
| AP1–AP3 | Erste Fassung als Calc-Tabelle. Sie war zu zerbrechlich und wurde verworfen. |
| B1 | Neubau als LibreOffice Base mit eingebettetem Firebird. |
| V8–V8.5 | Ein-Fenster-Anwendung mit allen Ansichten. Dazu kamen: Leistungen und Protokoll; Notes-Kalenderexport (.ics); Ablageabgleich; Sicherung und Papierkorb; Änderungsspur; Vertretung; Hilfe; Gesamtsuche; Melden-Funktion (🐞). |
| V8.6 | Statistik als Calc-Pivot. Randloses Layout: die Seite passt sich dem Fenster an. Start ohne Flackern. Bau läuft auch unter Windows. |
| V8.7 | Hinweiszeile unten: zeigt Ergebnis, To Do und Hinweise. |
| V8.8/8.8b | UX-Prüfung umgesetzt. Tastenkürzel; Eingabeprüfung für Datum und Zahlen; Ausweg statt Sackgasse bei Pflichtfeldern; Spalten füllen das Raster; Schrift Liberation Sans (auch für Mac geeignet). |
| **V8.8c** (Auslieferung) | Siehe Abschnitt 3. |

Ausführlich steht der Verlauf in `doku/01_Projektbeschreibung.md`, die Entscheidungen in `doku/02_Entscheidungsregister.md` und der Stand in `doku/05_Stand_und_Offen.md`.

## 3. Neu in V8.8c (16.10.2026)

- **Fehler „Elemente nach rechts verschoben“** (V8.8b, nach Absturz oder Wiederherstellung) behoben. `BreiteMerken` rechnet die gebaute Lage aus der aktuellen Seitengröße zurück und merkt sich das Dokument.
- **Rechnungsverlauf:** Ein Doppelklick im Vertrag oder ein Suchtreffer wählt den Eintrag links richtig aus. Kein Durchflackern aller Einträge mehr nach dem Sichern (M1–M3).
- **Gesamtsuche („Alles durchsuchen“):**
  - Groß-/Kleinschreibung und Leerzeichen sind egal.
  - Neuer Schalter „Ähnliche Schreibweisen“: ä/ae, ß/ss, c/k/č, doppelte Buchstaben (M5, M7).
- **Begriffe:**
  - „Reaktion“ heißt jetzt „Erledigung“ bzw. „Erledigt am“, auch bei der Soll-Zeit und im Stand.
  - „Prüfergebnis“ heißt jetzt „Stand“.
  - „Protokoll & Rechnungen“ heißt jetzt „Rechnungsverlauf“.
  - „Suche überall“ heißt jetzt „Alles durchsuchen“.
  - „Storniert (zählt nicht)“ heißt jetzt „Storniert“.
  - Die Partnerlisten tragen ihren Inhalt im Spaltenkopf (M4, M6, M9, M10, M12, M15).
- **Knopf „Stornieren“** im Rechnungsverlauf. Das Kästchen „Storniert“ lässt sich nicht mehr von Hand setzen (M10).
- **Datum überall kurz** im Format 23.12.26 (M14).
- **Hinweiszeile:**
  - Drei Stufen: Aus, Nur Status und To Do (Vorgabe), Mit Bedienhinweisen.
  - Farben lassen sich abschalten.
  - Ergebnisse stehen mit Uhrzeit da und höchstens 10 Sekunden lang (W2, M13).
- **Eigener Tooltip** mit einstellbarer Verzögerung (Vorgabe 1,0 s, Dezimalwerte möglich). Bei „Mit Bedienhinweisen“ ist er aus (W1).
- Einstellungen: neue Karte „Beispieldaten und Anzeige“ (M8, teilweise).
- Hilfe und Tastenkürzel-Liste sind an alles Obige angepasst.

**Geprüft unter Windows** am 16.10. mit einer Kopie (eigenes LibreOffice, Skript `winpruef.py`): 13 von 14 Prüfungen bestanden.
- Bestanden: Lage, Suche („kaffe“, „kaffee service“, „KAFE“), alle Ansichten, Anzeige-Einstellungen, Stornieren-Knopf, Zeitgeber.
- Sichtprüfung: Übersicht, Rechnungsverlauf und Einstellungen in Ordnung.
- Die eine Abweichung ist im Lauf nicht mehr sichtbar gewesen und wurde nicht geklärt.
- Die automatischen Oberflächentests (Linux, Xvfb) liefen für V8.7–V8.8c **nicht**.

## 4. Aufbau der Quellen (`v8/`)

| Datei | Inhalt |
|---|---|
| `build_base.py` | Baut die `.odb`: DB → Formular → Makros → Python-Zeitgeber. |
| `schema.py`, `db_build.py` | Tabellen, Sichten (V_*), Trigger, Beispieldaten. |
| `forms_def.py`, `formkit.py` | Alle Ansichten. Koordinaten in 1/100 mm auf einer Seite von 32000 × 18000. |
| `tooltips.py`, `anleitung.py` | Tooltip-Texte und Hilfetext. |
| `app/NN_*.bas` | Basic-Logik. Beim Bau wird sie zu einem Modul „App“ zusammengefügt. |
| `py/zeitgeber.py` | Zeitgeber für den Tooltip. |
| `winui.py`, `winpruef.py` | Prüfhilfen für Windows (öffnen eine Kopie im eigenen LibreOffice). |
| `test_*.py`, `alle_tests.sh` | Oberflächen- und DB-Tests. Die Oberflächentests laufen nur unter Linux mit Xvfb. |

Bauen unter Windows (im Ordner `v8`, PowerShell):
```
& "C:\Program Files\LibreOffice\program\python.exe" -m pip install --target _pylib pillow   # einmalig
$env:PYTHONPATH = (Resolve-Path _pylib).Path
& "C:\Program Files\LibreOffice\program\python.exe" build_base.py v8.odb
```

Wichtige Regeln stehen in `doku/CLAUDE.md` und `doku/04_Fallstricke.md`:
- Quellen nie mit PowerShell 5.1 schreiben, sonst entsteht eine BOM.
- Nachgelagerte Schritte laufen über `Spaeter(code)`.
- `AppDoc()` statt `ThisComponent` verwenden.
- Jede Änderung bekommt eine Codemarke mit Version.

## 5. Offene Punkte (priorisieren, dann umsetzen)

Ausführlich mit Umsetzungsideen: `Offene_Punkte_und_Umsetzungsideen.md`.

| Nr. | Punkt | Herkunft |
|---|---|---|
| O1 | Automatische Oberflächentests nachziehen und voll rücktesten (Linux/Xvfb oder WSL). Viele Tests prüfen alte Texte und Koordinaten. | Stand |
| O2 | Eingabeprüfung Datum und Zahl (EIN-02/04): die V8.8b-Logik ist unter Windows ungetestet. | V8.8 |
| O3 | „Verwerfen und wechseln?“ → „Ja“: der Wechsel ist seit V8.8b nachgelagert, aber ungetestet. | KON-04 |
| O4 | Darstellungsreste nach einem Ansichtswechsel (kleine Knopfreste, z. B. in den Einstellungen gesehen). | Test 16.10. |
| O5 | Tabellen: Auto-Zoom und Knöpfe +/− für die Schriftgröße. | M14, P6.2 |
| O6 | Seitenleiste schmaler, mehr Platz für Tabellen. | M16, P6.3 |
| O7 | Normale Schriftgröße in den Einstellungen; Vollbild zeigt mehr statt größer. Das ist ein Umbau des Grundprinzips. | M17, P6.4 |
| O8 | Präzisere Leistungs-Hinweise je Zustand („Termin vereinbaren und eintragen in …“). | M11, P1.8 |
| O9 | Einstellungen weiter verdichten (Leerfläche unter „Auswahllisten“). | M8 |
| O10 | „unbenannt N“ als Name für Verträge ohne Nummer: entscheiden, ob er bleibt. | SPR-01 |
| O11 | Absturz beim Beenden, wenn zwei Register offen sind. | A1 |
| O12 | Auswahl springt nach Fenstergrößenänderung (R1); 12-px-Versatz bei abgeschalteter Hinweiszeile (F2). | Stand |
| O13 | „Soll-Erledigungszeit“: Prüfen, ob der Vertragsbegriff „Reaktionszeit“ wieder getrennt gebraucht wird. | D4 |
| O14 | Gesamtsuche: Treffer im Vertrag direkt im Feld markieren (heute öffnet sie den Datensatz). | M7 |
| O15 | Notes-Dubletten beim Neuimport, DTSTAMP, N14-Link: erst beim Freund prüfbar. | Stand |
| O16 | Analyse: nicht tragende Workarounds vereinfachen (LageHerstellen, Leisten, Spaeter-Codes). | Nico 16.10. |

## 6. So arbeitet man am besten weiter

1. Arbeiten Sie an einem Punkt nach dem anderen.
2. Ändern Sie die Quellen, bauen Sie neu und öffnen Sie eine **Kopie** zum Testen. Die echte Datei mit Daten bleibt unberührt.
3. Ein Neubau erzeugt eine leere Datenbank mit Beispieldaten. **Echte Daten übernimmt er nicht.**
   - Wer die Logik einer bestehenden Datei ändern will, tauscht das Basic-Modul (Extras › Makros › Basic verwalten) gegen die neue `App.bas`.
   - Formular- und Tabellenänderungen brauchen eine Datenübernahme per Export/Import. Lassen Sie die KI dafür ein Übernahmeskript schreiben.
4. Meldungen aus dem Käfer 🐞 (`Meldungen\meldungen.txt` mit Bildern) sind die beste Grundlage für neue Aufträge an die KI.
