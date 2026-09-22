# Regelstrategie: SmO₂-Zielbereich steuert FTMS-Zielleistung

Entwurf · Stand 22. September 2026 · **Nicht freigegeben, nicht implementiert**

Dieses Dokument füllt die in Abschnitt 5.1 des Gesamtkonzepts geforderte
Definition aus. Alle Zahlenwerte sind **Platzhalter**. Sie sind von Jürg zu
bestimmen und stammen nicht aus einer physiologischen Annahme der Software.

## 1. Ausgangslage

| | Messkanal MOXY | Stellkanal FTMS |
|---|---|---|
| Richtung | SmO₂ → App | App → Rolle |
| Rate | selten, **offen: reale Abtastrate am Gerät bestimmen** | Befehl jederzeit möglich |
| Verzögerung | physiologische Reaktionszeit, **offen** | Sekundenbereich |
| Störungen | Bewegungsartefakte, Sensorlage, Auflagedruck | Bestätigung durch Gerät vorhanden |

Daraus folgt der tragende Entwurfsgrundsatz: **die Taktung der Regelung richtet
sich nach der langsamsten Grösse im Kreis, nicht nach der schnellsten.**

## 2. Sicherheits-Asymmetrie

Lastsenkung und Laststeigerung sind nicht gleichwertig. Eine Senkung ist im
Zweifel unbedenklich, eine Steigerung nicht. Die Regeln sind deshalb bewusst
unsymmetrisch:

- Bei fehlender, veralteter oder qualitativ ungenügender Pflichtmessung findet
  **keine Steigerung** statt (Abnahmekriterium CTRL-02).
- Eine Senkung darf in derselben Lage zulässig sein, wenn Jürg das so festlegt.
- Die Entscheidung «halten oder senken» bei Datenverlust ist eine offene Frage
  (siehe Abschnitt 8, Punkt 5).

## 3. Zustände der Regelung

Der Zustand ist im aktiven Training jederzeit sichtbar.

| Zustand | Bedeutung | Lastverhalten |
|---|---|---|
| `INAKTIV` | Regelung nicht eingeschaltet | Last folgt Phase oder Nutzer |
| `WARTET_AUF_DATEN` | eingeschaltet, Datenqualität noch nicht ausreichend | letzte gültige Last halten |
| `AKTIV` | regelt | nach Abschnitt 5 |
| `HALTEN` | Messwert veraltet oder Qualität ungenügend | einfrieren, keine Steigerung |
| `PAUSIERT` | vom Nutzer pausiert | nach Gerätefestlegung |
| `MANUELL` | Nutzereingriff übersteuert die Regelung | Nutzer bestimmt |
| `FEHLER` | Steuerzustand unbekannt, Befehl nicht bestätigt | keine neuen Sollwerte, Hinweis |

Die Regelung startet immer in `INAKTIV` und muss ausdrücklich eingeschaltet
werden. Ein Übergang nach `AKTIV` erfolgt nie automatisch aus `FEHLER`.

## 4. Eingangsaufbereitung

1. **Plausibilitätsprüfung** je Messwert: Wertebereich, Sprunghöhe gegenüber
   Vorwert, mitgelieferte Qualitätsangabe soweit vorhanden.
2. **Filterung** über ein Fenster `W_filter` = _Platzhalter_. Roh- und
   Filterwert werden getrennt gespeichert; geregelt wird auf dem Filterwert,
   angezeigt werden beide unterscheidbar.
3. **Altersprüfung**: ist der jüngste gültige Wert älter als `T_alt`
   = _Platzhalter_, wechselt die Regelung nach `HALTEN`.
4. **Mindestabdeckung**: liegen im Filterfenster weniger als `N_min` gültige
   Werte, gilt der Filterwert als nicht verwendbar.

Fehlende Werte werden nie durch Null oder durch Fortschreiben ersetzt.

## 5. Entscheidungsregel

Auswertung in festem Takt `T_tick` = _Platzhalter_. Eine Anpassung erfolgt nur,
wenn **alle** Bedingungen erfüllt sind:

- Zustand ist `AKTIV`.
- Der Filterwert liegt ausserhalb des Zielbereichs zuzüglich Toleranz
  `tol` = _Platzhalter_ (Totzone; verhindert dauerndes Nachregeln am Rand).
- Die Abweichung besteht ununterbrochen seit mindestens `T_abw` = _Platzhalter_.
- Seit der letzten Laständerung sind mindestens `T_min` = _Platzhalter_
  vergangen (Einschwingsperre, muss die physiologische Reaktionszeit abdecken).

Die Schrittweite ist begrenzt durch `ΔP_max` je Änderung und zusätzlich durch
eine maximale Änderung pro Zeit. Der resultierende Sollwert wird geklemmt auf
den Schnitt aus:

- Jürgs konfigurierter Lastgrenze für diese Phase, und
- dem vom Gerät gemeldeten Bereich (FTMS `Supported Power Range`, `0x2AD8`).

**Die Richtung der Anpassung ist bewusst nicht festgelegt.** Sie wird als
konfigurierte, vorzeichenbehaftete Zuordnung geführt und stammt aus Jürgs
Verfahren (Abschnitt 8, Punkt 1).

## 6. Ausbleibende oder widersprüchliche Messreaktion

Nach jeder Laständerung wird geprüft, ob SmO₂ innerhalb von `T_reaktion`
= _Platzhalter_ überhaupt eine Bewegung zeigt.

- Bleibt die Reaktion nach `N_ohne_reaktion` aufeinanderfolgenden Änderungen in
  derselben Richtung aus, hört die Regelung auf zu steigern, meldet dies
  verständlich und geht nach `HALTEN`.
- Bewegt sich SmO₂ entgegen der erwarteten Richtung, wird nicht verstärkt
  nachgeregelt. Das vereinbarte Verhalten ist anzuzeigen.
- Das Ereignis wird protokolliert und erscheint in der Auswertung der Einheit.

Diese Regel ist der Schutz gegen die gefährlichste Fehlannahme: dass die
Software die Ursache einer SmO₂-Änderung kennt.

## 7. Kadenz, Signalverlust, Wiederverbindung

- **Kadenzabfall** (sofern Kadenz verfügbar): unterschreitet die Kadenz
  `rpm_min` = _Platzhalter_ für `T_kadenz`, gilt die SmO₂-Reaktion als nicht
  interpretierbar; keine Steigerung.
- **Signalverlust MOXY**: `HALTEN`, Hinweis «MOXY-Signal fehlt. Automatische
  Regelung pausiert.», keine Steigerung.
- **Signalverlust Rolle**: Anzeige «Steuerzustand unbekannt». Der tatsächliche
  Gerätezustand wird nicht erfunden; Hinweis auf manuellen Eingriff.
- **Wiederverbindung**: wartende Befehle werden verworfen, nicht nachgesendet.
  Zuerst aktuelle Messwerte und Steuerzugriff prüfen, danach bewusste
  Wiederaufnahme durch den Nutzer.

Befehle haben eine Gültigkeitsdauer; die Warteschlange je Gerät ist auf einen
ausstehenden Sollwert begrenzt.

## 8. Offene Entscheidungen – blockieren Etappe B5

1. **Regelrichtung.** Was bedeutet ein Verlassen des Zielbereichs nach oben
   beziehungsweise nach unten für die Last? Naheliegend, aber ausdrücklich
   unbestätigt: SmO₂ über der Obergrenze → Last erhöhen; SmO₂ unter der
   Untergrenze → Last senken. Ohne Jürgs Bestätigung wird keine Richtung
   eingebaut.
2. **Anwendungsbereich.** In welchem Belastungsbereich ist das Verfahren
   gültig? Ausserhalb wird nicht geregelt, sondern nur überwacht.
3. **Lastgrenzen und Schrittweite.** Absolute Unter-/Obergrenze in Watt,
   `ΔP_max` je Änderung, maximale Änderung pro Zeit.
4. **Zeitkonstanten.** Erwartete Reaktionszeit von SmO₂ auf eine Laständerung.
   Daraus folgen `T_min`, `T_reaktion` und die Einschwingsperre.
5. **Verhalten bei Datenverlust.** Halten oder senken – und falls senken, um
   wie viel und bis wohin?
6. **Erholungskriterium.** Für MOXY-gesteuerte Erholungsphasen: SmO₂-Schwelle,
   erforderliche Haltedauer im Erholungsbereich, Mindest- und Maximaldauer,
   Verhalten bei Nichterreichen.

## 9. Offene Fakten – nicht zu erraten

- Reale Abtastrate und allfällige geräteseitige Glättung von MOXY.
- Übertragungsweg und Datenformat von MOXY über Bluetooth.
- Verhalten der Rolle mit dem letzten Sollwert nach Verbindungsabbruch
  (Abnahmekriterium CTRL-04, nur an realer Hardware feststellbar).

## 10. Prüfung vor Hardwarefreigabe

Die Regelung wird zuerst gegen aufgezeichnete und synthetische Verläufe
geprüft: Totzone, Ratenbegrenzung, Grenzenklemmung, Verhalten bei Datenlücke,
bei ausbleibender Reaktion und bei Wiederverbindung. Erst danach folgt ein
Lauf an realer Hardware mit jederzeit erreichbarem manuellem Eingriff.

Ein bestandener Simulatorlauf ist kein Hardwarenachweis.
