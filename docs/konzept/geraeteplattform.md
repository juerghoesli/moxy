# Ein Protokoll, mehrere Gerätearten

Stand 22. September 2026 · Erweiterung des Architekturentwurfs

## Der Gedanke

Statt je Gerät einen eigenen Weg zu bauen, wird **eine** Protokollschicht
gebaut, die jedes Gerät bedient, das FTMS korrekt umsetzt. Rollen heute,
Laufband später, weitere Gerätearten ohne Umbau.

Das ist richtig, und der Standard trägt es: FTMS ist nicht als Radprotokoll
entworfen, sondern als Dienst für Trainingsgeräte allgemein.

## Was der Standard je Geräteart bietet

| Geräteart | Messwerte | Stellgrössen |
|---|---|---|
| Rolle / Indoor Bike | Indoor Bike Data `0x2AD2` | Zielleistung `0x05`, Zielwiderstand `0x04` |
| Laufband | Treadmill Data `0x2ACD` | Zielgeschwindigkeit `0x02`, Zielsteigung `0x03` |
| Rudergerät | Rower Data `0x2AD1` | dieselben allgemeinen Steuerbefehle |
| Crosstrainer | Cross Trainer Data `0x2ACE` | dieselben allgemeinen Steuerbefehle |

Gemeinsam für alle: Fähigkeitsabfrage `0x2ACC`, Kontrollpunkt `0x2AD9`,
Statusrückmeldung `0x2ADA`, Steuerungsübernahme `0x00`, Start `0x07`,
Stopp/Pause `0x08`, sowie die gerätegemeldeten zulässigen Bereiche
(`0x2AD8` Leistung, `0x2AD4` Geschwindigkeit, `0x2AD5` Steigung,
`0x2AD6` Widerstand).

Angaben aus öffentlichen Quellen zur FTMS-Spezifikation, **vor der
Implementierung gegen das Original beim Bluetooth SIG zu verifizieren.**

## Was sich verallgemeinern lässt

Die gesamte Protokollschicht ist geräteartunabhängig und wird einmal gebaut:

- Auffinden, Verbinden, Wiederverbinden.
- Fähigkeiten am Gerät abfragen statt aus dem Modellnamen ableiten.
- Steuerung übernehmen, Befehl senden, auf Bestätigung warten.
- Gerätegemeldete Grenzen einlesen und als äussere Schranke verwenden.
- Befehlsgültigkeit, Warteschlange von höchstens einem Sollwert,
  Steuerungsereignis mit Anlass, Sendezeit und Bestätigung protokollieren.
- Zustände: bereit, verbindet, nur Messwerte, Signal fehlt, Steuerung nicht
  verfügbar, Steuerzustand unbekannt.

Ein Laufband bringt danach keine neue Protokollarbeit mehr mit.

## Was sich ausdrücklich nicht verallgemeinern lässt

Hier widerspricht die Sache dem ersten Eindruck, und das Gesamtkonzept sagt es
in Abschnitt 5.5 und in Etappe F schon: **keine Umbenennung eines Radadapters
als Laufbandunterstützung.**

1. **Die Folgen eines Fehlers sind nicht dieselben.** Eine Rolle, die eine
   falsche Last hält, ist unangenehm. Ein Laufband, das die Geschwindigkeit
   unerwartet ändert, wirft den Läufer vom Band. Die Sicherheitsgrenzen sind
   je Geräteart eigene Regeln, keine Parameter derselben Regel.
2. **Es gibt zwei Stellgrössen statt einer.** Je Trainingsmodus wird
   festgelegt, ob Geschwindigkeit oder Steigung führt. Die App darf nie beide
   unbemerkt verändern.
3. **Übergänge sind physisch.** Start, Beschleunigung und Verzögerung eines
   Laufbands brauchen Zeit und Vorankündigung; eine Rolle folgt einem
   Lastsprung sofort.
4. **Der Ausfall wirkt anders.** «Keine Befehle mehr» heisst bei einer Rolle,
   dass die Last irgendwo stehen bleibt. Bei einem Laufband heisst es, dass
   das Band weiterläuft. Ein App-Stopp ersetzt den Not-Stopp des Geräts nicht.

**Folgerung:** eine gemeinsame Protokollschicht, aber je Geräteart ein eigener
Sicherheitsrahmen und eine eigene Abnahme. Das Laufband bleibt eine eigene
Freigabe – es erbt die Verbindung, nicht das Vertrauen.

## Welche Zusage zulässig ist

| Zulässig | Nicht zulässig |
|---|---|
| «Geräte, die FTMS umsetzen, werden über denselben Weg bedient» | «Alle aktuellen elektronischen Geräte werden unterstützt» |
| «Modell X, Firmware Y, am TT.MM.JJJJ real geprüft: lesen und steuern» | «Hersteller Z wird unterstützt» |
| «Die Fähigkeit wird am Gerät abgefragt» | «Das Modell kann erfahrungsgemäss …» |

Viele Geräte setzen FTMS nur teilweise um oder daneben ein eigenes Protokoll.
Deshalb bleibt die Kompatibilitätsliste modell- und firmwarebezogen, mit dem
Datum der letzten realen Prüfung. Ein Gerät, das nur Messwerte liefert,
bekommt keine Steueroberfläche.

## Folge für die Etappen

Die Protokollschicht wird in Etappe B **geräteartunabhängig** gebaut, nicht
radspezifisch. Damit ist das Laufband später kein Umbau, sondern ein neuer
Sicherheitsrahmen plus eigene Abnahme. Die Reihenfolge der Etappen ändert sich
nicht: Laufbandsteuerung bleibt Etappe F mit eigener Freigabe.

Kosten dieser Verallgemeinerung jetzt: gering, weil die Fähigkeitsabfrage
ohnehin gebaut wird. Kosten später, wenn wir sie unterlassen: die halbe
Geräteschicht neu.
