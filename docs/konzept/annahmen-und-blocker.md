# Annahmen, geprüfte Fakten und Blocker

Entwurf Etappe A · Stand 22. September 2026

## 1. Geprüft

| Sachverhalt | Beleg | Bemerkung |
|---|---|---|
| Die Rolle wird über FTMS gesteuert | Angabe des Auftraggebers | Modell und Firmware noch offen |
| FTMS ist ein Bluetooth-SIG-Standarddienst (`0x1826`) | Bluetooth SIG, Fitness Machine Service 1.0 | Spezifikations-PDF aus der Entwicklungsumgebung nicht abrufbar |
| Steuerung erfordert vorgängige Übernahme, danach Zielleistung setzbar | Sekundärquellen zum FTMS-Kontrollpunkt | **Vor Implementierung gegen das Original zu verifizieren** |
| Das Gerät meldet seinen zulässigen Leistungsbereich | FTMS `Supported Power Range` | dient als zweite, geräteseitige Lastgrenze |
| Kein bestehendes Repository enthält Vorarbeit zu ERPSE Flow | Prüfung von `gmail-autoresponder-web` und `fatmax-pro60` | keine Treffer für MOXY, SmO₂, Bluetooth, FTMS |
| Die Terra-Anbindung in `fatmax-pro60` ist nicht aktiv | Quelltextkommentar und Platzhalter-Implementierung | als Vorentwurf für Etappe E brauchbar, nicht als Nachweis |

## 2. Annahmen – ausdrücklich unbestätigt

| Annahme | Risiko bei Irrtum | Klärung durch |
|---|---|---|
| Die Rolle setzt den für Zielleistung nötigen Teil von FTMS um | Wattsteuerung nicht möglich | Modellangabe, dann Gerätetest |
| MOXY liefert SmO₂ über Bluetooth in auswertbarer Form | Etappe B nicht erreichbar | Herstellerdokumentation, dann Gerätetest |
| Das Zieltelefon unterstützt die nötigen Bluetooth-Berechtigungen im Hintergrund | Aufzeichnung bricht bei gesperrtem Bildschirm ab | Gerätemodell, dann realer Test |
| SmO₂ reagiert auf Laständerungen langsamer als die Rolle stellt | Regelung schwingt | Jürgs Erfahrungswert |

ANT+ wird **nicht** angenommen und für die erste Fassung nicht vorausgesetzt.

## 3. Blocker

### Blockiert Etappe A nicht mehr
Repository-Entscheidung getroffen (`moxy`). Steuerprotokoll der Rolle geklärt
(FTMS).

### Blockiert Etappe B – Geräteanbindung
1. Wahoo-Modell mit Firmwarestand.
2. MOXY-Generation und Firmware.
3. Android-Handymodell und Betriebssystemversion.
4. Anlage des Repositories `moxy` auf GitHub samt Zugriff (technisch von
   dieser Session aus nicht möglich, `403` beim Erstellen).

### Blockiert Etappe B5 – automatische Regelung
Siehe `regelstrategie-moxy.md`, Abschnitt 8: Regelrichtung, Anwendungsbereich,
Lastgrenzen und Schrittweite, Zeitkonstanten, Verhalten bei Datenverlust,
Erholungskriterium.

### Blockiert spätere Etappen
Powermeter- und Herzfrequenzmodelle, Wearables und Terra-Voraussetzungen,
Laufbandmodell, fachliche Regeln für die zielbezogene Bewertung.

## 4. Was diese Etappe ausdrücklich nicht behauptet

- Keine funktionierende Geräteanbindung.
- Keine geprüfte Kompatibilität mit einem konkreten Modell.
- Keine physiologische Gültigkeit irgendeiner Schwelle oder eines Zielbereichs.
- Keine Aussage darüber, wie sich die Rolle bei Verbindungsabbruch verhält.
