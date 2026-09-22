# Gerätelage

Stand 22. September 2026 · Etappe A

Diese Liste unterscheidet drei Dinge: **Angabe** (vom Auftraggeber genannt),
**recherchiert** (aus öffentlichen Quellen, nicht am Gerät geprüft) und
**zu messen** (nur an der realen Hardware feststellbar). Ein Eintrag wandert
erst nach einem realen Test in die Kompatibilitätsliste.

## MOXY

| | |
|---|---|
| Angabe | Bluetooth-fähig **und** ANT+-fähig |
| Offen | Genaue Generation und Firmware; die Bezeichnung «Idiag» aus dem Diktat ist ungeklärt |
| Folge | **Der ANT+-Weg wird nicht gebraucht.** Die erste Fassung geht über Bluetooth Low Energy |
| Zu messen | Übertragungsweg und Datenformat von SmO₂ über BLE, reale Abtastrate, geräteseitige Glättung |

Das Datenformat ist der grösste verbleibende technische Unsicherheitsfaktor
für Etappe B. Für SmO₂ existiert kein Bluetooth-SIG-Standarddienst, der hier
ohne Weiteres greifen würde; Herstellerdokumentation und Messung am Gerät
entscheiden.

## Rollen

Zwei Geräte, beide in der jeweiligen Spitzenausführung.

| | Wahoo | Tacx |
|---|---|---|
| Angabe | KICKR, beste Version | Neo (ausdrücklich nicht Flux), beste Version |
| Recherchiert | Aktuelles Spitzenmodell ist die KICKR V6; die KICKR MOVE ist seit Juni 2026 abgekündigt, aber im Handel noch verfügbar | Aktuelles Spitzenmodell ist die Tacx Neo 3M |
| Recherchiert | Steuerung über FTMS oder ANT+ FE-C | Steuerung über Bluetooth FTMS und ANT+ FE-C |
| Offen | Welche der beiden Ausführungen, Firmwarestand | Bestätigung des Modells, Firmwarestand |
| Zu messen | Umgesetzte FTMS-Teilmenge, Verhalten bei Verbindungsabbruch | dasselbe, separat |

### Was daraus folgt

**Ein Adapter, zwei Geräte.** Beide Rollen sprechen FTMS. Die
fähigkeitsbasierte Adapterarchitektur aus dem Architekturentwurf ist damit
nicht Theorie: derselbe Adapter bedient beide, sofern beide die nötige
FTMS-Teilmenge umsetzen. Das ist ein Vorteil – zwei Geräte zwingen die
Abstraktion von Anfang an zur Ehrlichkeit.

**Zwei Geräte bedeuten aber zwei Abnahmen.** Abnahmekriterium CTRL-04
verlangt das dokumentierte reale Verhalten bei Pause, Abbruch und Funkverlust.
Dieses Verhalten ist gerätespezifisch und wird je Rolle einzeln festgestellt,
nie von der einen auf die andere übertragen. Die Tacx Neo arbeitet mit einem
Motorbremsprinzip ohne mechanisches Schwungrad und verhält sich schon deshalb
beim Wegfall der Steuerung voraussichtlich anders als die KICKR – das ist
eine Erwartung, kein Befund.

**Die primäre Leistungsquelle bleibt eine bewusste Wahl.** Sind zwei Rollen
oder Rolle und Powermeter gleichzeitig verbunden, wählt der Nutzer. Keine
Addition, kein stiller Quellenwechsel.

## Telefon

Noch keine Angabe. Modell und Android-Version blockieren die Prüfung der
Bluetooth-Berechtigungen, des Vordergrunddienstes und der Aufzeichnung bei
gesperrtem Bildschirm.

## Noch zu beschaffende Angaben

1. Wahoo: KICKR V6 oder KICKR MOVE, dazu der Firmwarestand aus der Wahoo-App.
2. Tacx: Bestätigung Neo 3M, dazu der Firmwarestand.
3. MOXY: Generation und Firmware; Klärung der Bezeichnung «Idiag».
4. Telefon: Modell und Android-Version.

«Neueste Version» genügt nicht als Angabe. Das Verhalten einer Rolle bei
Verbindungsverlust und die umgesetzte FTMS-Teilmenge hängen am Firmwarestand,
nicht am Produktnamen.

## Quellen der recherchierten Angaben

- Wahoo Fitness, KICKR-Produktinformationen und Modellbestimmung
- Garmin, Tacx NEO 3M Produktseite
- DC Rainmaker, Besprechungen KICKR MOVE und Tacx NEO 3M

Öffentliche Quellen, nicht am Gerät verifiziert.
