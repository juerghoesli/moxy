# Verbindliche Begriffe

Entwurf Etappe A · Stand 22. September 2026

Diese Liste ist für Oberfläche, Dokumentation und Code verbindlich. Ein Begriff
hat genau eine Bedeutung. Entwicklungssprache erscheint nie in der normalen
Oberfläche.

## Geräte und Verbindung

| Begriff in der App | Bedeutung | Nicht verwenden |
|---|---|---|
| **Gerät** | Rolle, Sensor oder Laufband aus Nutzersicht | Adapter, Peripheral, Endpoint |
| **Rolle** | steuerbares Indoor-Radgerät | Trainer, Smart Trainer, Hometrainer |
| **Bereit** | verbunden, liefert aktuelle Werte, steuerbar sofern vorgesehen | Connected, Online |
| **Verbindet** | Verbindungsaufbau läuft | Pairing, Scanning |
| **Nur Messwerte** | verbunden, liefert Werte, aber nicht steuerbar | Read-only |
| **Signal fehlt** | verbunden gewesen, aktuell keine gültigen Werte | Timeout, Dropout |
| **Steuerung nicht verfügbar** | Gerät kann oder darf nicht gesteuert werden | No control, Control denied |
| **Steuerzustand unbekannt** | keine Rückmeldung des Geräts; sein Zustand wird nicht behauptet | Unknown state |
| **Gerätekombination** | gespeicherte Zusammenstellung für eine Aktivität | Profil, Setup, Preset |
| **Primäre Leistungsquelle** | das Gerät, dessen Watt gelten | Main source |

Nie in der Oberfläche: Adapter, GATT, Charakteristik, FTMS, Webhook, Payload,
Service, UUID, Fallback.

## Training

| Begriff | Bedeutung |
|---|---|
| **Freies Training** | keine Vorgabe, Nutzer bestimmt die Last |
| **Einheit** | eine durchgeführte oder gespeicherte Trainingseinheit |
| **Vorlage** | gespeicherte, versionierte Einheit zum Wiederverwenden |
| **Phase** | Abschnitt einer Vorlage mit eigener Steuerungsart |
| **Phasenblock** | wiederholbare Gruppe von Phasen |
| **Steuerungsart** | SmO₂-Bereich, Wattwert oder manuell |
| **Zielbereich** | Unter- und Obergrenze einer Messgrösse |
| **Sollwert** | von der App angeforderter Wert |
| **Ist-Wert** | vom Gerät gemessener Wert |
| **Lastgrenze** | nie zu überschreitende Watt-Grenze |
| **Manueller Eingriff** | bewusste Übersteuerung durch den Nutzer |

«Sollwert» und «Ist-Wert» sind in der Oberfläche immer unterscheidbar
dargestellt und werden nie gemischt.

## Regelung

| Begriff | Bedeutung |
|---|---|
| **Automatische Regelung** | die App passt die Last anhand SmO₂ an |
| **Aktiv** | regelt gerade |
| **Wartet auf Daten** | eingeschaltet, Datenqualität noch nicht ausreichend |
| **Pausiert** | vom Nutzer unterbrochen |
| **Gehalten** | Last eingefroren, keine Steigerung |

Nie: Regler, Controller, PID, Loop, Algorithmus.

## Testung

| Begriff | Bedeutung |
|---|---|
| **Test** | standardisierte Belastung nach festem Protokoll |
| **Rampentest** | stetig steigende Last |
| **Stufentest** | stufenweise steigende Last |
| **Protokoll** | die versionierte Vorschrift eines Tests |
| **Markierung** | gesetzter Punkt in der Auswertung |
| **MOXY-Übergang 1 / 2** | vom Nutzer gesetzte oder bestätigte Markierung |
| **Trainingsbereich** | bestätigter Watt- und/oder SmO₂-Bereich mit Herkunft |

**MOXY-Übergang wird nie mit Laktatschwelle, VT1, VT2, aerober oder anaerober
Schwelle gleichgesetzt** – weder in der Oberfläche noch im Code.

## Auswertung

| Begriff | Bedeutung |
|---|---|
| **Gesamtzeit** | Start bis Ende einschliesslich Pausen |
| **Aktive Zeit** | ohne Pausen |
| **Aufzeichnungsabdeckung** | Anteil der aktiven Zeit mit gültigen Messwerten |
| **Datenlücke** | Zeitraum ohne gültige Messwerte |
| **Nullleistung** | gemessene 0 W – nicht dasselbe wie Datenlücke |
| **Beobachtete Bestleistung** | höchster gemessener Wert über ein Zeitfenster |
| **Nicht beurteilbar** | gültiges Ergebnis bei unzureichender Datenlage |

**Mechanische Arbeit** darf ausgewiesen werden. **Kalorienverbrauch,
VO₂max, Laktat, Fettverbrennung und Fitness-Scores nicht.**

## Erholung

| Begriff | Bedeutung |
|---|---|
| **Erholungsindikator** | einzelner importierter Messwert mit Definition und Alter |
| **Referenzzeitraum** | persönlicher Vergleichszeitraum |
| **Uneinheitlich** | widersprüchliche Indikatoren |
| **Daten fehlen** | keine ausreichend aktuellen Daten |

Nie: Readiness, Recovery-Score, Body Battery, Form, Frische.
