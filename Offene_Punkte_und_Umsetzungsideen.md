# Vertragsregister V8.8c – Offene Punkte und wie sie gedacht waren

Stand 16.10.2026, 17:15. Die Nummern O1–O16 entsprechen `Gesamtbeschreibung_und_Fortsetzung.md`, Abschnitt 5.

Herkunftsangaben:
- M… = Meldung aus `Meldungen\meldungen.txt` vom 16.10.
- P…/D… = `doku/Plan_V8.8_Meldungen_16-10.md`
- Weitere Kürzel stammen aus der UX-Prüfung (`UX-ToDos_Vertragsregister_2026-10-16.md` im Testordner).

Die Umsetzungsideen haben drei Arten von Herkunft:
- Mit Ersteller besprochen: im Plan oder Log festgehalten.
- Mein Vorschlag: noch nicht abgenommen.
- Noch offen: ungeklärt.

---

## A. Gebaut, aber nicht oder nur teilweise geprüft

### O1 · Automatische Oberflächentests nachziehen
- **Stand:**
  - Die Tests `test_*.py` laufen nur unter Linux mit Xvfb. Seit V8.7 liefen sie nicht mehr.
  - Viele prüfen alte Texte und Koordinaten: „Protokoll & Rechnungen“, „Reaktion am“, „Prüfergebnis“, „unbenannt“, Zoomtyp 2 statt 4, Seitenkoordinaten in `v8ui.py`, `NA_lblStatus`.
- **Gedacht:**
  1. Eine Linux-Umgebung nutzen: WSL2 mit Xvfb oder eine Cloud-Sitzung.
  2. Dort `bash alle_tests.sh` laufen lassen.
  3. Fehlschläge einzeln prüfen: Ist es ein Textwechsel (Test anpassen) oder ein echter Fehler (Code beheben)?
  4. Danach DB-Tests und Fristtest laufen lassen (siehe `doku/CLAUDE.md` › Testen).
- **Unter Windows:** `winpruef.py` erweitern. Es öffnet eine Kopie und prüft über UNO; das braucht kein Xvfb.

### O2 · Eingabeprüfung Datum/Zahl (EIN-02/04)
- **Stand:**
  - Eingaben wie „31.02.2026“ oder „-5“ sollen gemeldet werden, statt still ersetzt zu werden.
  - V8.8 wirkte nicht. LibreOffice korrigiert das Feld beim Verlassen und überschreibt dabei den Rohtext.
  - V8.8b merkt sich zusätzlich den vorigen Text und die Zeit (`app/21_eingabe.bas`). Unter Windows ist das ungetestet.
- **Prüfen:** „31.02.2026“ in „Beginn“ tippen und Tab drücken. Erwartet ist die Meldung „… ist kein gültiges Datum – ‚Beginn‘ ist unverändert“, und der Cursor steht wieder im Feld.
- **Falls es nicht greift (mein Vorschlag):** Für betroffene Felder `StrictFormat` abschalten und selbst parsen. Oder die Prüfung in `approveRowChange` verschieben und das gebundene Feld mit dem Rohtext vergleichen.

### O3 · „Verwerfen und die andere Ansicht öffnen?“ → „Ja“ (KON-04)
- **Stand:** Bei „Ja“ wird jetzt verworfen und nachgelagert gewechselt (`Spaeter("BEREICH")`). Unter Windows ist das ungetestet.
- **Prüfen:**
  1. Strg+N im Vertrag drücken, etwas tippen, Pflichtfeld leer lassen.
  2. Strg+2 drücken und „Ja“ wählen.
  3. Erwartet: Die Liste der Verträge erscheint.
- Dazu ein Rest aus V8.8: Der erste Strg+1 nach Strg+N und Tippen wechselte einmal ohne Rückfrage. Das ließ sich nicht nachstellen.

### O4 · Darstellungsreste nach Ansichtswechsel
- **Beobachtet am 16.10.:** In den Einstellungen standen kleine Reste von Knöpfen der vorigen Ansicht, z. B. „…en“ und ein „H“-Kästchen.
- **Mein Vorschlag:**
  - Nach `Bereich()` nachgelagert `ComponentWindow.invalidate(CHILDREN+UPDATE)` aufrufen; das macht `ZoomAnpassen` schon so.
  - Prüfen, ob die Reste vom neuen Tooltip-Label (`NA_lblTipp`) kommen, nämlich vom Verstecken ohne Neuzeichnen. Dann in `TippWeg` das Fenster des Elements neu zeichnen.

### Tooltip (neu in V8.8c) – noch beobachten
- Er liegt als Label über anderen Elementen.
- Unter Windows ist er kurz geprüft (Seitenleiste, Raster). Nicht geprüft: am rechten und unteren Rand, über Listenfeldern und bei Fensterwechsel.
- Ohne Python in der LibreOffice-Installation erscheint automatisch der normale Tooltip ohne Verzögerung.

---

## B. Wünsche, die noch nicht gebaut sind

### O5 · Tabellen: Auto-Zoom und Knöpfe + / − (M14, P6.2)
- **Wunsch Ersteller:** In allen Tabellen standardmäßig ein Auto-Zoom, der möglichst alle Inhalte zeigt. Dazu + und − zum Vergrößern und Verkleinern.
- **Gedacht (mein Vorschlag, nicht besprochen):**
  - Je Raster zwei kleine Knöpfe „−“ und „+“ rechts über dem Raster.
  - Sie ändern `FontHeight` des Rastermodells um ±1 pt (Bereich 8–14). `RowHeight` passt sich proportional an.
  - Der Wert wird je Raster gemerkt, als Dokumenteigenschaft wie bei `HinweisStufe` (`DokWert`/`DokWertMerken` in `19_hinweis.bas`).
  - Auto-Zoom: Beim Betreten der Ansicht die längsten Texte je Spalte schätzen, etwa so:
    - `MAX(CHAR_LENGTH(…))` per SQL über die Spalten der Sicht.
    - × Zeichenbreite (≈ 0,5 × Schriftgröße).
    - Mit der Rasterbreite vergleichen und die Schrift senken, bis es passt (Untergrenze 8 pt).
  - Die Spaltenbreiten verteilt bereits `BreiteAnpassen` (GER-01). Dort statt der festen Anteile die gemessenen Längen einsetzen.

### O6 · Seitenleiste schmaler (M16, P6.3)
- **Wunsch:** Die Seitenleiste nach links verdichten, damit mehr Platz für Tabellen bleibt.
- **Gedacht (mein Vorschlag):**
  - Die Karte der Seitenleiste wird schmaler: Breite 5300 → etwa 4300, `X0` in `forms_def.py` 5900 → etwa 4900.
  - Alle Inhalte rechnen mit `X0`/`CW`, sie werden dadurch automatisch breiter. Vorher nach festen Zahlen um 5900 suchen.
  - Engpass ist „Papierkorb & Sicherung“ in 10,5 pt. Lösung: 10 pt, keine führenden Leerzeichen, oder Umbenennung zu „Papierkorb/Sicherung“.
  - Die Fußzeile (Beenden, 🐞, ⚙) und das Suchfeld bekommen neue x-Werte.
  - Danach die Hintergrundbilder neu erzeugen; das macht der Bau automatisch. Alle Ansichten auf Überlappung prüfen (`test_v86_layout.py` oder Sichtprüfung).

### O7 · Schriftgröße einstellbar, Vollbild zeigt mehr (M17, P6.4)
- **Wunsch:** Die normale Schriftgröße steht in den Einstellungen. Im Vollbild wird die Schrift nicht größer; man sieht **mehr** statt einer vergrößerten 1:1-Kopie.
- **Mit Ersteller besprochen (D5):** Das ist ein eigener Umbau mit Versuch, nach den anderen Paketen.
- **Gedacht:**
  - Heute wächst die Seite mit dem Fenster und der Zoom „Seitenbreite exakt“ vergrößert alles.
  - Neu: Der Zoom bleibt fest. Er ergibt sich aus der eingestellten Schriftgröße, z. B. 100 % = 10,5 pt.
  - Die Seite wird in beiden Richtungen so groß wie das Fenster bei diesem Zoom: Seitenbreite = Fensterbreite in Pixeln × 2540 / (96 × Zoom/100).
  - `BreiteAnpassen` streckt dann nur Raster, Listen und mehrzeilige Felder (Breite und Höhe) und Karten (Kartenbild). Knöpfe und Felder behalten ihre Größe.
  - Rechtsbündige Knöpfe wandern mit der rechten Kante. Ihr Abstand zur rechten Kante wird gemerkt, nicht ihr Anteil.
  - Erst ein Versuch mit 2–3 Ansichten, dann der Umbau.

### O8 · Präzisere Leistungs-Hinweise (M11, P1.8)
- **Wunsch:** Statt „Was bei dieser Leistung zu tun ist.“ ein konkreter Satz, z. B. „Termin der offenen Leistung vereinbaren und im Feld X unter Y eintragen“.
- **Gedacht (Plan P1.8):**
  - Je Zustand der Sicht `V_Leistung` (Spalte `Anzeige`, z. B. „Fällig am …“, „überfällig“, „Rechnung prüfen“) einen Satz mit Feld und Ort.
  - Beispiel: „Fällig am 01.11.26 – Termin mit dem Partner vereinbaren; erbracht: Knopf ‚Heute erbracht‘.“
  - Ort im Code: `AnzeigeTipp` in `schema.py` (V_Leistung) und `LeistungTodo` in `app/19_hinweis.bas`.
  - Die Texte Ersteller zur Abnahme vorlegen (P1.9).

### O9 · Einstellungen verdichten (M8)
- **Erledigt:** Die Anzeige-Einstellungen stehen jetzt in der Karte „Beispieldaten und Anzeige“.
- **Offen:**
  - Leerfläche in „Auswahllisten“ unter dem Raster: Die Erklärung neben die Knopfleiste setzen oder in den Tooltip verschieben, dann das Raster bis zur Kartenunterkante ziehen.
  - Der Text „Löscht alle Beispiele …“ wird abgeschnitten. Entweder in den Tooltip des Knopfs oder die Karte 300 höher machen.

### O14 · Gesamtsuche: Treffer im Feld markieren (M7)
- **Ursache von M7** war die Groß-/Kleinschreibung (behoben).
- **Gedacht (mein Vorschlag):**
  - In `TrefferOeffnen` (`app/17_suche.bas`) nach dem Öffnen das Feld fokussieren und den Treffer auswählen, wie es für die Hilfe schon geschieht (`setSelection`).
  - Dafür braucht es eine Zuordnung von Spalte „Feld“ (z. B. „E-Mail“) zum Steuerelement (z. B. `PA_txtEmail`): eine kleine Tabelle im Code.

### Ähnliche Schreibweisen auch in den Ansichtssuchen
- **Heute:** Groß/klein und ä/ae gelten auch in den Suchfeldern der Ansichten (SQL `LIKE`). Leerzeichen-Toleranz und c/k gelten nur in „Alles durchsuchen“.
- **Gedacht:** Die Ansichtssuche filtert per SQL. `REPLACE` im Formularfilter ist zu vermeiden (Fallstrick 3).
  - Besser eine berechnete Suchspalte in den Sichten, z. B. `"SuchText"` = Kleinbuchstaben, ohne Leerzeichen, mit ersetzten Zeichen.
  - Darauf filtern, mit `SuchNorm` als Vorbild.

---

## C. Entscheidungen, die Ersteller noch treffen sollte

### O10 · „unbenannt N“ (SPR-01)
- **Stand:** Verträge ohne Nummer heißen „unbenannt 12“. Das steht im DB-Trigger (`schema.py`), in den Beispieldaten, in den Ablageordnern und in 6 Tests.
- **Optionen:**
  - Bleibt so.
  - Wird zu „ohne Nr. 12“.
  - Nummer wird Pflichtfeld.
- Bei einer Änderung müssen Trigger, Beispieldaten, Ablageabgleich und Tests gemeinsam angepasst werden.

### O13 · „Erledigungszeit“ statt „Reaktionszeit“ (D4)
- **Umgesetzt:** Überall „Erledigung“, auch „Erledigungszeit Soll“. Die Datenbankspalten bleiben `ReagiertAm`/`SollStunden`.
- **Offen:** Unterscheiden Verträge „Reaktionszeit“ (Ansprechzeit) und „Behebungszeit“? Dann eine zweite Soll-Zeit einführen statt nur umzubenennen.

---

## D. Bekannte Fehler und Beobachtungen (Ursache unbekannt)

| Nr. | Was | Idee |
|---|---|---|
| O11 · A1 | Absturz beim Beenden, wenn zwei Register gleichzeitig offen sind | `AppDoc()` nimmt das aktive Register; Beobachter und `Zu_notify` prüfen, ob sie zum richtigen Dokument gehören (RuntimeUID wie jetzt in `BreiteMerken`). Bis dahin: Register nur einmal öffnen. |
| O12 · R1 | Auswahl springt nach einer Größenänderung des Fensters auf eine andere Zeile | In V8.6d nachstellen. Verdacht: `BreiteAnpassen` setzt Spaltenbreiten, das Raster lädt neu. Vorher die Position merken und danach zurücksetzen. |
| O12 · F2 | ~12 px Versatz, wenn die Hinweiszeile aus ist | Nachstellen ohne Makros war begonnen. Seitenvorlagen `B_*_voll` und `HW_PLUS` vergleichen. |
| – | Hover auf „Statistik“ ohne Hinweis | Prüfen, ob der Knopf einen HelpText hat (`tooltips.py`). |
| – | V8.8 stürzte nach dem Test einmal ab | Nicht nachgestellt. Beobachten. |

---

## E. Erst beim Freund prüfbar

- **O15:**
  - Notes-Dubletten bei erneutem Import der `.ics`: Je Termin sollte eine feste UID stehen; prüfen.
  - DTSTAMP-Zeitzone.
  - Link „Ordner öffnen“ (N14) mit seinem echten Ablagelaufwerk.
- Ablagepfad in den Einstellungen setzen und „Ablage prüfen“ einmal laufen lassen.

## F. Pflege und Vereinfachung (O16)

- Analyse „nicht tragende Workarounds“ [GESAGT: Ersteller 16.10.]. Kandidaten:
  - `LageHerstellen`: Ursache der Formverschiebung beim Laden suchen, statt sie zu reparieren.
  - Doppelte Aufrufe von `LeistenAus`/`LeistenWeg`.
  - Die vielen `Spaeter`-Codes.
  - Der Größen-Trick ±1 Punkt beim Start.
- **Ziel:** Ein Nachfolger versteht den Code leichter. Erst eine Liste mit Aufwand und Nutzen, dann einzeln entfernen und jedes Mal unter Windows prüfen.
