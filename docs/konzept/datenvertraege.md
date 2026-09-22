# Datenverträge

Entwurf Etappe A · Stand 22. September 2026 · **Nicht implementiert**

Fachobjekte gemäss Abschnitt 16 des Gesamtkonzepts. Feldtypen sind fachlich
beschrieben, nicht als Datenbankschema. Die konkrete Ablage folgt in Etappe B.

## Grundregeln

1. **Vier Zeitarten werden nie vermischt.** Jeder Messwert führt mit:
   `geraetezeit` (falls das Gerät eine liefert), `empfangszeit` (Uhr des
   Telefons beim Eintreffen), `sitzungszeit` (monoton, ab Trainingsstart) und
   – nur auf Einheitsebene – `kalenderzeit` mit Zeitzone.
   **Geregelt und ausgewertet wird auf `sitzungszeit`.**
2. **Rohdaten bleiben unverändert.** Geglättete, neu abgetastete oder
   modellierte Werte sind eigene Datensätze mit Verweis auf das Verfahren.
3. **Fehlend ist nicht Null.** Es gibt keinen Wert, der beides bedeuten kann.
4. **Jede Änderung während einer Einheit wird protokolliert**, nicht nur das
   Endergebnis.

## Nutzerprofil

`einheitensystem`, `sprache`, `koerpergewicht[]` (Wert **mit Datum**, Historie),
`ziele[]` → je Ziel: `sportart`, `zieldistanz`, `erwartete_belastungsdauer`,
`streckencharakter`, `zieldatum`.

Ein Gewicht ohne Datum ist ungültig – W/kg ist sonst nicht nachvollziehbar.

## Geräteprofil

`modellbezeichnung`, `kennung`, `firmware` (soweit auslesbar), `funkweg`,
`faehigkeiten[]`, `zuletzt_real_geprueft` (Datum), `getestete_android_version`.

`faehigkeiten` ist eine Menge aus: liefert Leistung, liefert Kadenz, liefert
Herzfrequenz, liefert SmO₂, nimmt Zielleistung entgegen, nimmt Zielwiderstand
entgegen, meldet zulässigen Leistungsbereich. **Die Menge wird am Gerät
ermittelt, nie aus dem Modellnamen abgeleitet.**

## Gerätekombination

`aktivitaet` (Indoor Rad, Outdoor Rad, Laufband), `pflichtgeraete[]`,
`optionale_geraete[]`, `primaere_leistungsquelle`, `zuletzt_verwendet`.

Pflicht ist nur, was die gewählte Steuerungsart wirklich braucht.

## Trainingsvorlage

`version`, `name`, `phasen[]`, wobei jede Phase führt: `name`, `steuerungsart`,
`zielwert` oder `zielbereich`, `endkriterium` (Dauer **oder** ausdrücklich
gewähltes physiologisches Kriterium), `lastgrenze_unten`, `lastgrenze_oben`,
`uebergangsverhalten`, `hinweise[]`, `nebenbedingungen[]`.

Bei physiologischem Endkriterium zusätzlich verpflichtend: `mindestdauer`,
`haltedauer_im_erholungsbereich`, `maximaldauer`, `verhalten_bei_nichterreichen`.

Phasenblöcke: `phasen[]` plus `wiederholungen`.

## Testprotokoll

`version`, `typ` (Rampe, Stufen), `startleistung`, `anstieg_pro_zeit` oder
`stufenhoehe` und `stufendauer`, `maximale_last`, `maximale_dauer`,
`aufwaermphase`, `ausfahrphase`, `abbruchkriterien[]`.

## Einheit

`start`, `ende`, `pausen[]`, `aktivitaet`, `verwendete_geraete[]`,
`primaere_leistungsquelle`, `quellenwechsel[]`, `verwendete_vorlage_version`,
`verwendete_bereichsversion`, `vollstaendigkeit` (vollständig, abgebrochen,
**wiederhergestellt**), `notiz`, `subjektive_belastung`.

`vollstaendigkeit` wird beim Schreiben gesetzt, nicht beim Beenden berechnet.
Eine abgestürzte Einheit kann nie als vollständig erscheinen.

## Messwert

`quelle` (Geräteprofil), `messgroesse`, `einheit`, `wert`, `geraetezeit`,
`empfangszeit`, `sitzungszeit`, `qualitaet`, `ist_rohwert`.

`qualitaet` ist mindestens: gültig, unplausibel, veraltet, vom Gerät als
unsicher gemeldet.

## Steuerungsereignis

`anlass` (Phasenwechsel, automatische Regelung, manueller Eingriff,
Schutzbegrenzung, Wiederaufnahme), `sollwert`, `sendezeit`,
`bestaetigung` (bestätigt, abgelehnt mit Grund, keine Rückmeldung),
`bestaetigungszeit`, `geltungsdauer`.

Ohne Bestätigung gilt ein Befehl als **nicht** wirksam. Die Anzeige sagt dann
«Steuerzustand unbekannt».

## Testmarkierung

`zeitpunkt` oder `fenster`, `zugeordnete_leistung`, `smo2`, `herzfrequenz`,
`wert_art` (Momentanwert **oder** Mittelwert eines Abschnitts), `methode`,
`kommentar`, `bestaetigt_von`, `bestaetigt_am`.

`wert_art` ist verpflichtend – ein Stufenmittelwert und ein Momentanwert
dürfen nie verwechselt werden.

## Trainingsbereich

`version`, `grenzen`, `ursprungstest`, `methode`, `erstellt_am`,
`bestaetigt_von`, `gueltig_ab`, `gueltig_bis`.

Eine Einheit speichert, welche Bereichsversion zur Laufzeit galt. Neue
Bereiche deuten alte Einheiten nicht rückwirkend um; eine Neuauswertung ist
ausdrücklich als solche gekennzeichnet.

## Erholungsdaten

`provider`, `messgroesse`, `messdefinition`, `messfenster`, `einheit`, `wert`,
`bezugsdatum`, `empfangen_am`, `qualitaet`, `geraet`.

Unterschiedliche `messdefinition` oder `geraet` werden nie stillschweigend
verglichen (Abnahmekriterium REC-02).

## Analyse und Empfehlung

`eingangsdatensatz`, `verfahrensversion`, `ergebnis`, `begruendung`,
`datenlage` (ausreichend, eingeschränkt, nicht beurteilbar),
`vergleichszeitraum`.

Jede angezeigte Aussage verweist auf genau einen solchen Datensatz. Gibt es
keinen, wird die Aussage nicht angezeigt (Abnahmekriterium ANA-03).

## Export

Zuerst CSV plus strukturierte Sicherung mit Metadaten, Bereichen und
Markierungen. Ein Export, der Herkunft, Qualität oder Zeitarten verliert, gilt
als unvollständig (Abnahmekriterium DATA-02). FIT/TCX später.
