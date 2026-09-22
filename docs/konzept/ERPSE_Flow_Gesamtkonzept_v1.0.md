# ERPSE Flow – Gesamtkonzept und Arbeitsauftrag für Claude Code

Version 1.0 · 22. September 2026 · Auftraggeber: Jürg Hösli

**Dokumenttyp: Produktkonzept und Entwicklungsspezifikation. Keine bestehende Implementierung wird damit als geprüft oder freigegeben erklärt.**

## 1. Auftrag an Claude Code

Entwickle auf Grundlage dieses Konzepts eine hochwertige, bewusst einfach bedienbare Android-App für gerätegestütztes Ausdauertraining. Die zentrale Funktion ist die tatsächliche Steuerung kompatibler Trainingsgeräte. Messdaten, Diagnostik und Auswertung unterstützen diese Funktion.

Das erste nutzbare Gesamtsystem verbindet eine Wahoo-Rolle mit MOXY, steuert Training nach individuell festgelegten SmO₂-Zielbereichen oder Wattvorgaben und führt Rampen- sowie Stufentests durch. Es wird anschliessend um weitere Sensoren, Outdoor-Nutzung, kompatible Laufbänder und Regenerationsdaten über Terra erweitert.

Arbeite auf eine echte Android-Anwendung hin. Eine Website, ein Klickmodell oder ein Simulator allein erfüllt diesen Auftrag nicht. Simulation und Geräte-Replay sind Entwicklungswerkzeuge und müssen als solche erkennbar sein. Ein vorhandener früherer HTML-Prototyp ist keine verbindliche Grundlage für Architektur, Gestaltung, physiologische Annahmen oder Funktionsumfang.

**Erste Antwort auf dieses Dokument:** Fasse das Produktverständnis zusammen, prüfe gegebenenfalls das vorhandene Repository, zeige die Architektur und die Umsetzungsetappen und nenne die wenigen tatsächlich blockierenden Fragen. Beginne die Implementierung erst nach ausdrücklicher Freigabe des Auftraggebers. Dieses Dokument autorisiert zunächst die Planung.

Nach der Freigabe setze in überprüfbaren Schritten um. Trenne bei jeder Lieferung: implementiert, automatisiert geprüft, mit realer Hardware geprüft und noch offen. Erfinde keine Gerätekompatibilität, Testnachweise, physiologischen Schwellen oder Trainingsbewertungen.

## 2. Produktversprechen und Prioritäten

**Ich wähle mein Trainingsziel. Die App verbindet meine Geräte, führt mich durch das Training und zeigt mir verständlich, wie ich mich entwickle.**

Prioritäten in dieser Reihenfolge:

1. Verlässliche Geräteverbindung und kontrollierte Belastungssteuerung.
2. Einfache Bedienung während realer körperlicher Belastung.
3. Vollständige, zeitlich nachvollziehbare Aufzeichnung und Testdurchführung.
4. Verständliche Leistungsanalyse, Stärken und Entwicklungspotenzial.
5. Regenerationsverlauf und begründete Empfehlungen für Trainingsbereiche.

Die App soll sich ruhiger und einfacher bedienen lassen als umfangreiche Trainingsplattformen. Sie benötigt keine virtuelle Landschaft, Rangliste, soziale Timeline oder Sammlung dekorativer Fitnesszahlen.

Der Nutzer soll nicht wissen müssen, welches Funkprotokoll ein Gerät verwendet. Er muss aber erkennen können, ob das Gerät tatsächlich verbunden ist, aktuelle Werte liefert und gesteuert werden kann.

## 3. Nutzer, Plattform und Zielumfang

Erster Nutzer ist Jürg Hösli. Die erste Produktfassung benötigt keine Trainerplattform und keine Mehrmandantenverwaltung. Eine spätere Erweiterung für ERPSE-Kunden darf möglich bleiben, soll die persönliche Anwendung aber nicht verkomplizieren.

- Plattform zuerst Android-Smartphone; Tablet-Darstellung mitdenken.
- Primär Indoor-Radtraining, zusätzlich Outdoor-Radtraining.
- Laufbandsteuerung als eigener Erweiterungsbereich mit modellbezogener Prüfung.
- Sprache Deutsch, Schweizer Schreibweise, Anrede «Du».
- Kerntraining und Aufzeichnung funktionieren ohne Internet und ohne Terra.
- Ein Cloud-Konto darf den Einstieg in ein lokales Training nicht blockieren.
- Keine automatische Veröffentlichung, kein Social Sharing als Standard.

### Geräte und Rollen

| Gerät / Quelle | Erwartete Rolle | Planungsstatus |
|---|---|---|
| Wahoo-Rolle | Leistung und verfügbare weitere Messwerte liefern; Lastvorgaben ausführen | Pflicht für den ersten Hardwareumfang; Modell und Firmware offen |
| MOXY | SmO₂ liefern; weitere herstellerseitig verfügbare Werte optional aufzeichnen | Pflicht für MOXY-Steuerung und MOXY-Testung; Generation und Protokoll offen |
| Assioma DUO | Alternative primäre Leistungsquelle, verfügbare weitere Messwerte | Geplant; genaue Ausführung prüfen |
| Powermeter am Cervélo | Alternative primäre Leistungsquelle | Hersteller und Modell offen |
| Polar / anderer Herzfrequenzsensor | Herzfrequenz erfassen und optional überwachen | Modellbezogen prüfen |
| Weitere Rollen, etwa Tacx | Gleiche Bedienung über eigenen kompatiblen Geräteadapter | Nicht pauschal zusagen |
| Laufband | Geschwindigkeit und Steigung steuern, verfügbare Werte aufzeichnen | Separater Adapter und eigene Abnahme |
| Terra / Wearables | Verfügbare Aktivitäts-, Schlaf- und Erholungsindikatoren importieren | Serveranbindung und Providerprüfung erforderlich |

Bluetooth und ANT+ gehören zum Zielkonzept. Welche Kombination ohne oder mit Zusatzhardware funktioniert, muss anhand des Android-Geräts, der Sensoren, offizieller Dokumentation und realer Tests festgestellt werden. Weder einen Gerätekauf noch eine zusätzliche Lizenz oder ein kostenpflichtiges Konto ohne Freigabe veranlassen.

## 4. Bedienkonzept: drei Hauptbereiche

Die untere Navigation enthält ausschliesslich **Training · Testung · Entwicklung**. Geräte, Profil, Datenverwaltung und Einstellungen sind über kleine, eindeutig erkennbare Zugänge erreichbar.

### 4.1 Training – Startansicht

Reihenfolge von oben nach unten:

1. Kompakte Gerätezeile: beispielsweise «Wahoo verbunden · MOXY verbunden».
2. Aktivitätswahl: Indoor Rad, Outdoor Rad, später Laufband.
3. Trainingswahl: Freies Training oder gespeicherte Einheit.
4. Zielsteuerung: MOXY, Watt; weitere Modi nur bei vorhandener Unterstützung.
5. Die für den gewählten Modus notwendigen Eingaben.
6. Grosser Startknopf.

Bekannte Geräte und zuletzt verwendete Einstellungen werden wieder angeboten. Bei einer bereits eingerichteten Gerätekombination soll der Start mit höchstens drei bewussten Bedienhandlungen möglich sein, sobald die Geräte verfügbar sind. Ein automatischer Trainingsstart ohne Nutzerhandlung ist nicht vorgesehen.

Ein kompakter Hinweis zum Erholungsverlauf darf erscheinen, wenn echte Daten verfügbar sind. Er darf den Startbildschirm nicht in ein Analyseportal verwandeln.

### 4.2 Aktives Training

Standardmässig höchstens drei grosse Messfelder:

- SmO₂ mit Zielbereich, wenn dieser Modus gewählt ist.
- Tatsächliche Leistung in Watt.
- Verbleibende Phasenzeit; Herzfrequenz wahlweise als Ersatzfeld oder kleines Zusatzfeld.

Darunter eine gemeinsame Verlaufskurve, aktueller Phasenname und die nächste Phase. Ist-Wert und vom Trainer angeforderter Sollwert sind unterscheidbar. Bei Wattsteuerung wird Watt zum dominierenden Feld.

Bedienung: Pause, Fortsetzen, Beenden sowie gut erreichbarer manueller Eingriff. Keine feinmotorischen Schieberegler als einzige Bedienmöglichkeit. Zielwerte über grosse Plus-/Minusflächen oder Zahleneingabe verändern. Eine Änderungen wirkt eindeutig auf die aktuelle Phase oder auf die gespeicherte Vorlage; beides darf nicht versehentlich vermischt werden.

Outdoor steht eine kurze verständliche Handlungsmeldung im Vordergrund. Gesperrter Bildschirm und fehlendes Internet dürfen die vorgesehene Aufzeichnung nicht beenden.

### 4.3 Testung

Testtyp wählen, gespeichertes Protokoll auswählen oder Parameter bearbeiten, Sensorposition dokumentieren, Geräte prüfen, starten. Während des Tests sind Testphase, aktuelle Last, SmO₂ und Verlauf zentral. Nach dem Test öffnet sich direkt die Auswertung mit Markierungen und Vergleichsmöglichkeit.

### 4.4 Entwicklung – das persönliche Dashboard

Die erste Bildschirmhöhe beantwortet vier Fragen:

- Welche Leistung habe ich bisher gezeigt?
- Was entwickelt sich gut?
- Welcher Bereich verdient für mein Ziel Aufmerksamkeit?
- Was zeigen meine verfügbaren Erholungsindikatoren?

Darunter Leistungs-Dauer-Kurve, Entwicklung über die Zeit und Trainingshistorie. Details erst nach Antippen. Standardzeitraum vier Wochen; zusätzlich zwölf Wochen und gesamter Verlauf. Die Wahl beeinflusst die dargestellte Datenbasis eindeutig.

Keine unlesbare Kombination von zehn Kurven. Unterschiedliche Einheiten brauchen eigene Achsen oder getrennte, zeitlich synchronisierte Diagramme.

## 5. Training und Steuerungsarten

### 5.1 MOXY-Zielbereich

Der Nutzer legt eine SmO₂-Unter- und Obergrenze fest. Die App verwendet MOXY als Rückmeldung und verändert die Lastvorgabe der Rolle innerhalb individuell festgelegter Grenzen.

Die Regelung muss explizit aktiviert sein. Ihr Status ist jederzeit sichtbar: aktiv, wartet auf verlässliche Daten, pausiert, manuell oder Fehlerzustand.

**Keine fest eingebaute, ungeprüfte physiologische Regel:** Die Software darf nicht voraussetzen, dass jeder SmO₂-Anstieg oder -Abfall stets dieselbe Laständerung rechtfertigt. Vor Hardwarefreigabe werden Regelrichtung, anwendbarer Belastungsbereich, Messqualität, Glättung und Reaktionszeiten anhand des vereinbarten Verfahrens geprüft. Die erste Version darf mit einem klar beschriebenen, begrenzten Verfahren beginnen; dessen Grenzen müssen dokumentiert sein.

Die Regelstrategie definiert mindestens:

- Eingangsfilter und Qualitätsprüfung.
- Maximal zulässiges Alter eines Messwerts.
- Toleranz um die Zielgrenzen, damit die Last nicht ständig hin und her wechselt.
- Mindestdauer einer Abweichung vor einer Anpassung.
- Mindestabstand zwischen Laständerungen und maximale Änderung pro Zeit.
- Untere und obere Lastgrenze sowie erlaubter Betriebsbereich.
- Umgang mit ausbleibender oder widersprüchlicher Messreaktion.
- Umgang mit Kadenzabfall, wenn Kadenz verfügbar ist.
- Definiertes Verhalten bei Signalverlust und bei Wiederverbindung.

Es werden keine universellen SmO₂-Prozentwerte als physiologisch gültige Zonen voreingestellt. Beispiele sind als Beispiele markiert. Personenspezifische Zielbereiche werden gespeichert und mit Herkunft versehen.

### 5.2 Wattsteuerung

Die App fordert eine feste Leistung an oder folgt den Wattvorgaben einer Einheit. MOXY und Herzfrequenz können mitlaufen, ohne die Last zu verändern. Eine zusätzlich aktivierte Begrenzungsregel darf die Last reduzieren; dabei wird sichtbar erklärt, warum die Protokollvorgabe verändert wurde.

### 5.3 Freies Training / manuell

Der Nutzer bestimmt die Last selbst. Die App zeichnet auf und überwacht auf Wunsch Bereiche. Eine automatische Übernahme der Steuerung findet nicht statt.

### 5.4 Outdoor

Keine Lastbefehle. Die App zeichnet die ausgewählte Leistungsquelle, Herzfrequenz und optional MOXY auf. Für jeden verfügbaren Messwert lassen sich Untergrenze, Obergrenze und Hinweisart getrennt konfigurieren. Ausserhalb eines Bereichs erscheint eine klare, zum Messwert passende Meldung.

### 5.5 Laufband

Laufbandsteuerung gehört zum Gesamtkonzept, wird jedoch nicht als blosse Umrechnung der Radsteuerung umgesetzt. Zunächst feste Geschwindigkeit und Steigung sowie strukturierte Phasen. Eine physiologisch adaptive Regelung folgt erst nach eigener Prüfung.

Je Trainingsmodus wird festgelegt, ob Geschwindigkeit oder Steigung die primäre Stellgrösse ist. Die App darf nicht beide unbemerkt verändern. Start, Beschleunigung, Verzögerung, physischer Not-Stopp und Kommunikationsverlust werden modellbezogen behandelt. Ein App-Stopp ist kein Ersatz für den Not-Stopp des Laufbands.

## 6. Intervalle und Trainingsvorlagen

Der Nutzer kann selbst Einheiten erstellen und speichern. Das ist unabhängig von den späteren Empfehlungen: Die App empfiehlt Trainingsbereiche, erstellt aber nicht automatisch konkrete Trainingspläne.

Eine Einheit besteht aus Phasen oder wiederholbaren Phasenblöcken. Je Phase:

- Name: Einfahren, Belastung, Erholung, Ausfahren oder frei wählbar.
- Steuerungsart: SmO₂-Bereich, Wattwert oder manuell.
- Zielbereich beziehungsweise Zielwert.
- Dauer oder ausdrücklich gewähltes physiologisches Endkriterium.
- Lastgrenzen und Übergangsverhalten.
- Optional aktivierte Hinweise und begrenzende Nebenbedingungen.

Erholungsphasen können zeitgesteuert oder über ein bestätigtes SmO₂-Kriterium enden. Bei physiologischem Endkriterium sind Mindestdauer, erforderliche Haltedauer im Erholungsbereich und maximale Dauer festzulegen. Bei Nichterreichen wird nicht unbegrenzt gewartet und nicht still in eine neue Belastung gewechselt: Das vereinbarte Verhalten wird angezeigt, etwa Pause und Nutzerentscheidung.

Der Editor zeigt eine einfache Zeitleiste und wenige Eingaben. Phasen duplizieren, verschieben und in einen Wiederholungsblock fassen. Gesamtdauer beziehungsweise bei variabler Erholung erwartete Dauer und Obergrenze anzeigen. Vorlagen sind versioniert; eine gespeicherte Einheit behält das tatsächlich verwendete Protokoll.

## 7. Hinweise und Prioritäten

Für jede Messgrösse getrennt einstellbar: anzeigen, aufzeichnen, überwachen, warnen, zur Regelung verwenden. Nicht verfügbare Funktionen ausblenden oder verständlich deaktivieren.

- Hinweise optisch, optional Vibration und kurze Sprachansage.
- Warnverzögerung, Toleranz und Wiederholungsabstand verhindern ständige Meldungen.
- Auf Wunsch nur Obergrenze oder nur Untergrenze überwachen.
- Normale Bereichshinweise insgesamt oder einzeln stummschalten.
- Verbindungs- und Steuerungsfehler bleiben als Betriebszustand sichtbar.
- Stummschalten deaktiviert keine festgelegten Lastgrenzen.

Priorität bei widersprüchlichen Vorgaben:

1. Manueller Abbruch und gerätespezifisches Stopp-/Fallback-Verhalten.
2. Technische Grenzen und ausdrücklich aktivierte Schutzbegrenzungen.
3. Testprotokoll oder aktuelle Trainingsphase.
4. Adaptive MOXY-Regelung, falls für diese Phase gewählt.
5. Komfortfunktionen und Hinweise.

Kein zweiter Regler darf gleichzeitig widersprechende Lastbefehle senden.

## 8. Geräteverbindung und Datenqualität

Gerätekombinationen speichern: Indoor Rad, Outdoor Rad und später Laufband. Ein Pflichtsensor ist nur der für den gewählten Modus notwendige Sensor. Fehlendes MOXY verhindert MOXY-Regelung; ein Watttraining kann nach bewusster Auswahl weiterhin möglich sein. Fehlende Herzfrequenz blockiert keine Einheit, die sie nicht benötigt.

Die App unterscheidet:

- Gerät gefunden.
- Verbindung hergestellt.
- Aktuelle Messwerte vorhanden.
- Steuerfähigkeit erkannt.
- Steuerzugriff übernommen.
- Befehl bestätigt beziehungsweise nicht bestätigt.

Geräteansicht für den Nutzer: «Bereit», «Verbindet», «Nur Messwerte», «Signal fehlt», «Steuerung nicht verfügbar». Technische Details liegen hinter einem Diagnosezugang.

Wenn Rolle und Powermeter gleichzeitig verbunden sind, wählt der Nutzer die primäre Leistungsquelle. Der andere Kanal kann separat aufgezeichnet werden. Keine Addition, kein stiller Quellenwechsel. Ein Wechsel wird im Training markiert und in der Auswertung berücksichtigt. Die Abstimmung der Rollensteuerung auf einen externen Powermeter ist eine eigene, zu prüfende Funktion.

Kompatibilitätsliste je Modell und Firmware: Lesen, Steuern, unterstützte Messfelder, Funkweg, getestete Android-Versionen und letzte reale Prüfung. Herstellernamen allein genügen nicht.

## 9. MOXY-Testung und Schwellensetzung

### 9.1 Protokolle

Rampentest: Startleistung, Anstieg pro Zeit, Aufwärmphase, maximale Last, maximale Dauer und Ausfahren.

Stufentest: Startleistung, Stufenhöhe, Stufendauer, maximale Last beziehungsweise Stufenzahl, Aufwärmen und Ausfahren. Optional später variable Stufen.

Im Test folgt das Gerät dem vereinbarten Lastprotokoll. Eine SmO₂-Zielbereichsregelung läuft nicht parallel. Jede manuelle Veränderung, Pause, Sensorlücke oder technische Begrenzung wird sichtbar dokumentiert und kann die Vergleichbarkeit einschränken.

### 9.2 Vor dem Test

Speichern: Protokollversion, Messmuskel, Körperseite, nachvollziehbare Sensorposition, verwendete Geräte, primäre Leistungsquelle und kurze relevante Testnotizen. Dokumentationsfelder sollen schnell aus einem früheren Test übernommen werden können.

### 9.3 Auswertung

- Zeitgleiche Darstellung von SmO₂ und Leistung, optional Herzfrequenz.
- Rampenbeginn, Stufengrenzen, Pausen, fehlende Daten und Ausfahren markieren.
- Rohdaten und geglättete Ansicht getrennt verfügbar; Analysefenster sichtbar.
- Übergang durch Antippen oder Verschieben eines Markers setzen.
- Marker enthält Zeit, zugeordnete Leistung, SmO₂, gegebenenfalls Herzfrequenz und Kommentar.
- Bei Stufen explizit unterscheiden: aktueller Messwert oder Mittelwert eines ausgewählten Abschnitts.
- Frühere Tests mit gleicher beziehungsweise dokumentiert vergleichbarer Methodik einblenden.
- Sensorposition, Datenqualität und Protokollabweichungen beim Vergleich sichtbar machen.

### 9.4 Bedeutung der Schwellen

Die Anwendung verwendet zunächst «MOXY-Übergang 1/2» oder fachlich vom Nutzer gewählte Bezeichnungen. Eine automatische Gleichsetzung mit Laktat- oder ventilatorischen Schwellen ist nicht erlaubt.

Automatische Vorschläge können später als Kandidaten erscheinen. Voraussetzung: festgelegtes Verfahren, definierte Filterung und Mindestdatenqualität, Referenzdatensätze, nachvollziehbare Auswertung und fachliche Validierung. Der Nutzer kann Vorschläge akzeptieren, verschieben oder verwerfen. «Keine zuverlässige Zuordnung möglich» ist ein gültiges Ergebnis.

Eine einzelne Markierung ergibt nicht automatisch einen vollständigen Trainingsbereich. Die Übernahme von Watt- und/oder SmO₂-Bereichen erfordert ausdrückliche fachliche Bestätigung. Gespeichert werden Ursprungstest, Methode, Datum, Bestätiger und Bereichsversion. Alte Einheiten werden durch neue Bereiche nicht rückwirkend umgedeutet; eine erneute Auswertung mit neuen Zonen muss explizit als solche erkennbar sein.

## 10. Auswertung jeder Trainingseinheit

Nach dem Training zunächst eine kompakte Zusammenfassung:

- Dauer: Gesamtzeit, aktive Zeit und Aufzeichnungsabdeckung unterscheiden.
- Leistung: Durchschnitt und beobachtete Bestwerte über sinnvolle Zeiträume.
- SmO₂: Verlauf und Zeit innerhalb, unterhalb und oberhalb der tatsächlich aktiven Zielbereiche.
- Herzfrequenz, falls vorhanden.
- Erfüllung der geplanten Phasen und dokumentierte Abweichungen.
- Datenqualität: Lücken, Quellenwechsel, relevante Regelungsereignisse.
- Freiwillig: subjektive Belastung und kurze Notiz.

Alle Zeitanteile werden zeitgewichtet berechnet. Die Anzahl empfangener Datenpakete ist kein Ersatz für Dauer. Keine fehlenden Messwerte als Null einrechnen. Pausen, Nullleistung und Sensorverlust sind verschiedene Zustände. Regeln für Resampling, kurze Lücken und ungültige Fenster müssen dokumentiert und getestet sein.

Mechanische Arbeit darf aus Leistungsdaten berechnet werden. Sie darf nicht ohne begründetes Modell als metabolischer Kalorienverbrauch bezeichnet werden. Keine automatisch erfundenen VO₂max-, Laktat-, Fettverbrennungs- oder Fitnesswerte.

## 11. Dashboard: Leistung, Stärken und Schwächen

### 11.1 Leistungs-Dauer-Kurve

Beste gültige gleitende Durchschnittsleistung über mehrere Zeiträume; beispielsweise 5 Sekunden, 30 Sekunden, 1, 3, 5, 10, 20 und 60 Minuten. Das sind Darstellungsfenster, keine automatisch festgelegten physiologischen Kategorien.

Darstellung in Watt, optional W/kg mit dokumentiertem Körpergewicht und Datum. Zeitachse sinnvoll skaliert, beispielsweise logarithmisch und klar beschriftet. Vergleich aktueller Zeitraum gegen vorherigen gleich langen Zeitraum oder ausgewählten Referenztest.

Unvollständige Fenster nicht künstlich auffüllen. Ein besserer Trainingsbestwert ist eine beobachtete Verbesserung, aber ohne vergleichbaren maximalen Einsatz nicht automatisch eine entsprechende Kapazitätssteigerung. Modellierte Kurven werden von gemessenen Kurven getrennt; Modelle sind für die erste Fassung nicht erforderlich.

### 11.2 Persönliche Entwicklung

Leistungsentwicklung und Trainingsbelastung getrennt darstellen:

- Gezeigte Leistung über vergleichbare Zeiträume oder standardisierte Tests.
- Trainingsdauer und Verteilung auf bestätigte Bereiche.
- Häufigkeit und Kontinuität des Trainings.
- Später belastbare Veränderungen der Leistungsstabilität im Verlauf langer Einheiten.

Nicht pauschal «mehr trainiert = fitter». Radfahren und Laufen nicht mit ungeprüften gemeinsamen Scores vermischen.

### 11.3 Stärken und Entwicklungspotenzial

Bewertungsgrundlagen in dieser Reihenfolge:

1. Vergleichbare eigene Messungen und Tests.
2. Anforderungen des vom Nutzer festgelegten Ziels.
3. Optional externe Referenzen, aber nur mit dokumentierter Quelle, passender Population und ausdrücklicher Kennzeichnung.

Der Nutzer hinterlegt Sportart, Zieldistanz, erwartete Belastungsdauer und Streckencharakter. Ein 100-km-Ziel allein reicht nicht als vollständiges Anforderungsprofil.

Je Aussage anzeigen:

- Verständliche Beobachtung.
- Zugehörige Messwerte und Vergleichszeitraum.
- Zielrelevanz.
- Datenlage: ausreichend, eingeschränkt oder nicht beurteilbar; dafür transparente Regeln definieren.

Maximal zwei Stärken und zwei Entwicklungsbereiche auf der Hauptansicht. Beispiele für die Form, nicht für bereits nachgewiesene Ergebnisse:

«Deine nachgewiesene 20-Minuten-Leistung ist gegenüber dem vergleichbaren Test gestiegen.»

«Für dein Langstreckenziel fehlt bisher eine ausreichend lange, vergleichbare Belastung. Ermüdungsresistenz noch nicht beurteilbar.»

«Bei vergleichbaren langen Einheiten fällt die spätere Leistung stärker ab als im Referenzzeitraum. Prüfe den Bereich Leistungsstabilität.»

Selten trainiert, nicht gemessen und nachgewiesen schwächer sind drei unterschiedliche Aussagen. Die Oberfläche darf sie nicht vermischen. Keine Prozentbalken für vermeintliche Stärken ohne definierte Bezugsgrösse.

## 12. Regenerationsverlauf und Terra

Terra ist die gewünschte Anbindung für verfügbare Wearable-Daten. Terra ersetzt weder das Auswertungsverfahren noch die direkte Echtzeitverbindung zu Wahoo und MOXY.

Vor Implementierung je tatsächlich genutztem Provider prüfen: verfügbarer Datentyp, Einheit, zeitliche Auflösung, Messdefinition, Aktualität, Datenhistorie, Einwilligung, Synchronisationsweg und kommerzielle Voraussetzungen. Fehlende Felder dürfen nicht als verfügbar angenommen werden.

Mögliche Eingänge, sofern vorhanden:

- HRV mit dokumentierter Kennzahl und Messfenster.
- Ruhepuls mit dokumentierter Definition.
- Schlafdauer und weitere hinreichend interpretierbare Schlafdaten.
- Aktivitäts- und Trainingshistorie.
- Zwei freiwillige Selbsteinschätzungen: allgemeine Erholung und muskuläre Müdigkeit.

Die «Regenerationskurve» wird als Verlauf verfügbarer Erholungsindikatoren und gegebenenfalls als daraus geschätzter Erholungsstatus verstanden. Sie behauptet keine direkte Messung der vollständigen Regeneration.

Erste Fassung: einzelne Indikatoren gegenüber einem persönlichen, ausreichend langen Referenzzeitraum darstellen. Referenzdauer, Mindestabdeckung und Umgang mit Ausreissern vorab festlegen. Unterschiedliche HRV-Kennzahlen, Messfenster oder Geräte nicht still mischen. Gerätewechsel kann einen neuen Referenzaufbau erfordern.

Ein kombinierter Status wird erst eingeführt, wenn seine Regeln fachlich festgelegt und geprüft sind. Kein unbegründeter «Readiness 87/100»-Wert. Bei widersprüchlichen oder veralteten Daten steht «uneinheitlich» beziehungsweise «Daten fehlen».

Serverseitige Zugangsdaten, sichere Kontoverknüpfung, Widerruf, Löschung und idempotente Verarbeitung vorsehen. Trainingsimporte aus mehreren Quellen dürfen dieselbe Einheit nicht mehrfach zur Belastung addieren. Offline werden die letzten verfügbaren Werte mit ihrem Alter angezeigt. Ein Terra-Ausfall blockiert keine lokale Einheit.

## 13. Empfehlungen: welche Bereiche, nicht welcher Trainingsplan

Die App nennt höchstens zwei priorisierte Trainingsbereiche. Jede Empfehlung muss vier Fragen beantworten:

1. Welches Ziel unterstützt sie?
2. Welche Beobachtung spricht dafür?
3. Wie gut ist die Datengrundlage?
4. Passt der Schwerpunkt zu den verfügbaren Erholungsindikatoren?

Mögliche Bereiche: aerobe Ausdauer, längere Dauerleistung, kurze intensive Leistung, Wiederholbarkeit, Ermüdungsresistenz oder vorübergehende Entlastung. Die genaue Zuordnung zu persönlichen Intensitätsbereichen stammt aus bestätigten Vorgaben, nicht aus erfundenen universellen Grenzen.

Keine konkreten Intervalle, Wochenpläne oder Wattvorgaben automatisch als Empfehlung ausgeben. Der Nutzer kann seine Einheiten separat selbst erstellen.

Die erste Empfehlungslogik soll regelbasiert, nachvollziehbar und testbar sein. Ein Sprachmodell ist für die Echtzeitsteuerung und für die Berechnung von Messwerten nicht erforderlich. Wenn später KI zur Erklärung eingesetzt wird, darf sie nur vorhandene Ergebnisse formulieren und keine Daten oder physiologischen Schlussfolgerungen hinzufügen.

Bei unzureichender Datenlage kann die sinnvolle Empfehlung lauten: einen relevanten Bereich zunächst standardisiert erfassen. Eine schwache Datenbasis muss zu zurückhaltenderen Aussagen führen, nicht zu mehr scheinbarer Präzision.

## 14. Gestaltung und Bedienqualität

Visueller Charakter: ruhig, hochwertig, präzise. Viel Weissraum, klare Typografie, dunkles Grün als Akzent und gezielte Statusfarben. Die genaue Farbwelt ist ein Gestaltungsvorschlag; eine spätere ERPSE-Markenprüfung bleibt möglich. Keine erfundenen Logos.

- Grosse, mit schwitzigen Händen bedienbare Schaltflächen; als Entwurfsziel mindestens 48 dp.
- Klare Kontraste, skalierbare Schrift, Unterstützung von Bedienhilfen.
- Farben immer durch Text oder Symbole ergänzen.
- Livewerte ruhig aktualisieren; keine springende Zahlenbreite und keine unnötigen Animationen.
- Gute Darstellung bei heller Umgebung und auf einem Lenkerhalter.
- Kurze Rückmeldung auf Bedienhandlungen; Haptik optional.
- Ein leerer Zustand erklärt den nächsten sinnvollen Schritt.
- Fehlermeldungen nennen Auswirkung und Handlung: «MOXY-Signal fehlt. Automatische Regelung pausiert.»
- Keine Entwicklungssprache wie «Adapter», «Webhook» oder «GATT» in der normalen Oberfläche.

Bildschirmaufbau vor Implementierung als Entwurf prüfen: Start, aktives MOXY-Training, Intervallbearbeitung, laufender Test, Testauswertung, Entwicklungsdashboard. Diese sechs Entwürfe müssen zusammen dieselben Begriffe und Bedienmuster verwenden.

## 15. Architekturvorschlag für die Umsetzung

Vorgeschlagene technische Entscheidung: native Android-App mit Kotlin und einer modernen nativen UI, beispielsweise Jetpack Compose. Konkrete SDK-, Bibliotheks- und Berechtigungsstände vor Implementierung anhand aktueller offizieller Dokumentation prüfen. Keine Versionsnummer aus dem Gedächtnis festschreiben.

Getrennte Verantwortlichkeiten:

1. Oberfläche und Navigation.
2. Trainingszustandsmaschine und Protokollausführung.
3. Regelung und Priorisierung von Vorgaben.
4. Geräteadapter für Messung und Steuerung.
5. Lokale Aufzeichnung und Datenspeicherung.
6. Analyse und Empfehlungen.
7. Synchronisation und Terra-Anbindung.

Geräteadapter deklarieren ihre Fähigkeiten. Ein Sensor ohne Steuerfunktion erhält keine Steueroberfläche. Ein Simulator verwendet dieselben internen Verträge, bleibt aber klar von Hardware getrennt und wird nie unbemerkt als Fallback verwendet.

Die Echtzeitsteuerung ist unabhängig von Netzwerk, Server und Sprachmodell. Android-Lebenszyklus, Bildschirmabschaltung, Vordergrunddienst, Benachrichtigung, Energieverwaltung und Berechtigungen werden als Teil der Kernfunktion behandelt. Konkrete Umsetzung muss zur Zielversion passen.

Für die persönliche Version lokale Datenhaltung bevorzugen. Ein Backend zunächst nur für tatsächlich notwendige externe Integrationen. Keine unnötige Plattformarchitektur, aber klare Schnittstellen für spätere Erweiterungen.

## 16. Datenmodell und Nachvollziehbarkeit

Mindestens folgende Fachobjekte vorsehen:

| Objekt | Zentrale Inhalte |
|---|---|
| Nutzerprofil | Einheiten, Sprache, optional Gewicht mit Datum, Ziele |
| Geräteprofil | Modell, Kennung, Firmware soweit verfügbar, Fähigkeiten, Funkweg |
| Gerätekombination | Aktivität, Pflicht-/optionale Sensoren, primäre Leistungsquelle |
| Trainingsvorlage | Version, Phasen, Wiederholungen, Steuerarten, Grenzen |
| Testprotokoll | Version, Rampe/Stufen, Endkriterien, standardisierte Metadaten |
| Einheit | Start, Ende, Pausen, Aktivität, Quellen, verwendete Konfiguration |
| Messwert | Quelle, Messgrösse, Einheit, Zeit, Qualität, Empfangszeit |
| Steuerungsereignis | Anlass, Sollwert, Sendezeit, Bestätigung, Fehler |
| Testmarkierung | Zeitpunkt/Fenster, Werte, Methode, Kommentar, Bestätigung |
| Trainingsbereich | Version, Grenzen, Herkunft, Gültigkeit, Bestätiger |
| Erholungsdaten | Provider, Messdefinition, Datum, Qualität, Aktualität |
| Analyse/Empfehlung | Eingangsdatensatz, Verfahrensversion, Ergebnis, Begründung |

Rohdaten bleiben unverändert. Geglättete, synchronisierte oder modellierte Daten werden separat behandelt. Zeitstempel aus Geräten, Empfangszeit, monotone Sitzungszeit und Kalenderzeit nicht verwechseln. Einstellungen und Änderungen während einer Einheit protokollieren.

Dauerhafte Aufzeichnung erfolgt während des Trainings, nicht erst beim Beenden. Nach Prozessabbruch muss ein bis zum letzten Speicherpunkt wiederherstellbarer Datensatz existieren. Ein abgestürzter Test darf nicht nachträglich als vollständig erscheinen.

Export zunächst CSV und strukturierte Sicherung einschliesslich Metadaten, Bereichen und Markierungen. FIT/TCX als spätere interoperable Erweiterung; beim Import Herkunft, Sportart, Zeitzone und Duplikate berücksichtigen. Nutzer kann Daten exportieren und löschen. Keine echten biometrischen Daten in Diagnoseprotokollen oder Fehlerdiensten ohne ausdrückliche Entscheidung.

## 17. Zustände und Fehlerverhalten

Verbindliche Zustände: bereit, verbindet, startbereit, aktiv, pausiert, Signalproblem, Steuerproblem, beendet, abgebrochen, wiederhergestellt. Übergänge sind eindeutig und testbar.

Bei fehlendem Pflichtsensor keine weitere adaptive Laststeigerung. Was der reale Trainer mit seiner letzten Vorgabe tut, wird ausdrücklich geprüft. «Keine Befehle senden» darf nicht mit «Gerät reduziert die Last» gleichgesetzt werden. Das konkrete Fallback wird je Gerät definiert und sichtbar bestätigt, soweit das Protokoll dies ermöglicht.

Nach Wiederverbindung keine alten wartenden Befehle nachsenden und keine überraschende Fortsetzung. Zuerst aktuelle Daten und Steuerzugriff prüfen; danach bewusste Wiederaufnahme mit passendem Übergang.

Verbindungsverlust zum Trainer: tatsächlichen Gerätezustand nicht erfinden. Anzeige «Steuerzustand unbekannt», wenn keine Rückmeldung vorliegt. Nutzer erhält einen klaren Hinweis zum manuellen Eingriff.

Befehle benötigen eine Gültigkeitsdauer und eine begrenzte Warteschlange. Keine lange Folge verspäteter Laständerungen. Pro Gerät eine eindeutige Reihenfolge von Steuerbefehlen und eine definierte Behandlung fehlender Bestätigung.

## 18. Umsetzungsetappen mit lieferbaren Ergebnissen

### Etappe A – Konzeptfreigabe und Machbarkeit

Sechs Bildschirmkonzepte, verbindliche Begriffe, offene Hardwaredaten, geprüfte Schnittstellen und Datenverträge. Klare Liste von Annahmen und Blockern. Noch keine Behauptung einer funktionierenden Geräteanbindung.

### Etappe B – Kleinstes tatsächlich nutzbares System

Android-App, reales Wahoo-Modell und reales MOXY-Modell verbinden; Werte anzeigen und dauerhaft aufzeichnen; Wattsteuerung; anschliessend freigegebene begrenzte MOXY-Regelung; manuelle Eingriffe, Sensorverlust und Wiederverbindung behandeln. Bedienbarer Trainingsstart und einfache Zusammenfassung.

**Abnahme:** Ein Training auf der realen Hardware kontrolliert durchführen und eine brauchbare Aufzeichnung exportieren. Ein Simulator allein besteht diese Etappe nicht.

### Etappe C – Intervalle und Testung

Trainingsvorlagen, Phasenwiederholungen, zeit- und kriteriumsgesteuerte Erholung; Rampen-/Stufentests; Markierungen, fachliche Bereiche und Vergleich früherer Tests.

**Abnahme:** Eine selbst erstellte Einheit und beide Testtypen durchlaufen; tatsächliche Last und Zeitablauf gegen Protokoll prüfen; bestätigte Bereiche in ein Folgetraining übernehmen.

### Etappe D – Dashboard und Outdoor

Leistungs-Dauer-Kurve, Verlauf, vollständige Einzelauswertung, transparente Stärken-/Schwächenlogik, Zielprofil und Bereichsempfehlungen. Powermeter und Herzfrequenz integrieren; Outdoor-Aufzeichnung mit optionalen Hinweisen.

**Abnahme:** Auswertung auf Referenzdaten reproduzierbar; keine Bewertung aus fehlenden Daten; Outdoor-Aufzeichnung auf echtem Android-Gerät bei gesperrtem Bildschirm prüfen.

### Etappe E – Terra und Regenerationsverlauf

Reale autorisierte Provideranbindung, Synchronisation, persönliche Referenzen, Regenerationsindikatoren und deren Einfluss auf Empfehlungen.

**Abnahme:** Datenquelle, Messdefinition, Alter, fehlende Werte und Widerruf funktionieren nachvollziehbar. Ohne Terra bleibt das Training vollständig nutzbar.

### Etappe F – Weitere Geräte und Laufband

Zusätzliche Modelle einzeln aufnehmen. Laufband mit eigenem Steuerungskonzept und eigener Freigabe. Keine Umbenennung eines Radadapters als Laufbandunterstützung.

Alle Etappen gehören zum Gesamtkonzept. Ihre Reihenfolge begrenzt Entwicklungsrisiken; sie ist keine stille Streichung der späteren Ziele.

## 19. Überprüfbare Abnahmekriterien

| ID | Kriterium |
|---|---|
| UX-01 | Wiederkehrendes Training nach Gerätebereitschaft mit höchstens drei bewussten Bedienhandlungen starten |
| UX-02 | Aktives Training ohne verschachtelte Menüs pausieren und beenden |
| UX-03 | Hinweise pro Messgrösse deaktivierbar; aktive Steuerung und Lastgrenzen bleiben eindeutig |
| DEV-01 | Verbunden, aktuelle Messwerte und aktive Steuerfähigkeit getrennt geprüft |
| DEV-02 | Primäre Leistungsquelle eindeutig; Quellenwechsel dokumentiert |
| CTRL-01 | Kein Sollwert ausserhalb festgelegter Grenzen; Änderungsgeschwindigkeit begrenzt |
| CTRL-02 | Kein adaptiver Lastanstieg bei veralteter oder fehlender Pflichtmessung |
| CTRL-03 | Keine konkurrierenden Regler, keine alten Befehle nach Wiederverbindung |
| CTRL-04 | Reales Geräteverhalten bei Pause, Abbruch und Funkverlust dokumentiert |
| TEST-01 | Rampen-/Stufentest folgt dem Protokoll und wird nicht vom SmO₂-Regler verändert |
| TEST-02 | Markierungen bearbeiten und mit Quelle/Methode speichern; Zonen nur bewusst übernehmen |
| DATA-01 | Teilaufzeichnung nach Prozessabbruch wiederherstellbar und als unvollständig erkennbar |
| DATA-02 | Export und Wiederimport erhalten relevante Rohdaten und Metadaten |
| ANA-01 | Leistungs-Dauer-Berechnung stimmt mit unabhängig berechneten Referenzfällen überein |
| ANA-02 | Ungültige Messfenster, Pausen, Nullleistung und Datenlücken korrekt unterschieden |
| ANA-03 | Jede Stärke, Schwäche und Empfehlung hat Datenbasis und Begründung |
| REC-01 | Fehlende/alte Terra-Daten erzeugen keine fiktive Regenerationsbewertung |
| REC-02 | Unterschiedliche Messdefinitionen werden nicht unbemerkt verglichen |
| HW-01 | Simulator-, Replay-, Emulator- und reale Hardwaretests getrennt ausgewiesen |

Für Tests synthetische Daten und ausdrücklich freigegebene Referenzdaten nutzen. Kernlogik automatisiert prüfen, Bedienabläufe auf einem Emulator, Funk- und Steuerverhalten auf realer Hardware. Visuell auf dem Zielhandy, mit vergrösserter Schrift und unter Trainingsbedingungen prüfen. Kein Freigabevermerk ohne den jeweiligen Nachweis.

## 20. Offene Entscheidungen – gezielt klären

Vor der Geräteimplementierung benötigt:

1. Genaues Wahoo-Modell mit Firmware.
2. MOXY-Generation und Firmware.
3. Android-Handymodell und Betriebssystemversion.

Vor Freigabe der physiologischen Regelung benötigt:

4. Jürgs Verfahren für MOXY-Zielbereiche und Übergangsbestimmung.
5. Regelrichtung, zulässiger Anwendungsbereich, individuelle Lastgrenzen und Verhalten bei unklarer Reaktion.
6. Gewünschtes Kriterium für MOXY-gesteuerte Erholung.

Für die späteren Erweiterungen benötigt:

7. Exakte Powermeter- und Herzfrequenzmodelle.
8. Verwendete Wearables und Terra-Zugangsvoraussetzungen.
9. Laufbandmodell und dokumentierte Steuerfunktionen.
10. Datenbasis und fachliche Regeln für zielbezogene Stärken-/Schwächenbewertung.

Nicht alle Fragen gleichzeitig zum Projektstart stellen. Zuerst klären, was die nächste freigegebene Etappe tatsächlich blockiert. Fehlende Hardwaredaten verhindern nicht das Ausarbeiten der Bedienung und der Datenverträge, wohl aber eine seriöse Zusage der konkreten Gerätekompatibilität.

## 21. Leitplanken für Claude Code

- Dieses Dokument vollständig lesen und Anforderungen mit den obigen Bereichen verknüpfen.
- Zunächst Planung liefern; keine Programmierung ohne anschliessende ausdrückliche Freigabe.
- Nach Freigabe kleine überprüfbare Umsetzungsschritte mit einem klaren nächsten Ergebnis.
- Offizielle aktuelle Quellen für Funkprotokolle, Android-Anforderungen, Herstellerfunktionen und Terra prüfen; kompatible Versionen dokumentieren.
- Unsichere Punkte als offen kennzeichnen, keine plausibel klingenden Schnittstellen erfinden.
- Keine universellen SmO₂-Schwellen oder Erholungsscores erfinden.
- Simulation nicht als Hardwareerfolg ausgeben.
- Datenqualität und Fehlersituationen als Kernfunktion behandeln.
- Nutzeroberfläche einfach halten; technische Komplexität in die interne Architektur verlagern.
- Keine neuen Funktionsbereiche ergänzen, bevor der vereinbarte Kern zuverlässig funktioniert.
- Keine Veröffentlichung, kostenpflichtigen Dienste oder Übertragung realer Gesundheitsdaten ohne passende Freigabe.
- Am Ende jeder Etappe einen kurzen, ehrlichen Nachweis liefern: Was funktioniert, womit wurde es geprüft, was fehlt?

**Erfolgsdefinition:** Jürg kann mit wenigen Handgriffen seine Geräte verbinden, ein MOXY-gesteuertes Training oder einen standardisierten Test durchführen und anschliessend verstehen, was seine Daten belegen, wo Entwicklungspotenzial besteht und welche Trainingsbereiche zu seinem Ziel passen.
