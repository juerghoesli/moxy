# Sechs Bildschirmkonzepte

Entwurf Etappe A · Stand 22. September 2026

Beurteilbare Fassung: <https://claude.ai/artifact/QiZ3sxDaMKNzmGSmsz64HH>
(privat; 390 × 844, Telefonformat).

**Das ist ein Beurteilungswerkzeug, keine App.** Keine Geräteverbindung, keine
Steuerung, keine Aufzeichnung. Alle Zahlen sind Beispielwerte zur Prüfung der
Anordnung.

## Die sechs Bildschirme

| # | Bildschirm | Beantwortet |
|---|---|---|
| 1 | Training – Start | Sind meine Geräte bereit, und wie starte ich in drei Handlungen? |
| 2 | Aktives MOXY-Training | Wo stehe ich, was tut die Regelung, wie greife ich ein? |
| 3 | Intervallbearbeitung | Wie sieht meine Einheit aus, und wie lange dauert sie wirklich? |
| 4 | Laufender Test | Folgt das Gerät dem Protokoll, und wo bin ich im Test? |
| 5 | Testauswertung | Wo liegen die Übergänge, wie belastbar ist der Vergleich? |
| 6 | Entwicklungsdashboard | Was zeigt meine Leistung, und was ist noch nicht beurteilbar? |

## Gestaltungsentscheide

**Gemeinsame Sprache.** Alle sechs verwenden dieselben Begriffe aus
`begriffe.md` und dieselben Muster: Abschnittsüberschriften in Versalien,
Karten mit 1 px Rand, Schaltflächen ab 44 px, Zahlen mit fester Ziffernbreite
(springt beim Zählen nicht).

**Ruhiger Grund, ein Akzent.** Heller Grund `#F6F7F5`, Text `#15201B`, dunkles
Grün `#1B4D3E` als einziger Akzent. Statusfarben nur dort, wo sie etwas
bedeuten: Bernstein für Hinweise, Rot für Abbruch. **Jede Farbe ist von Text
begleitet** – kein Zustand wird allein über Farbe mitgeteilt.

**Drei Entscheide, die direkt aus dem Konzept folgen:**

1. **Ist-Wert und Sollwert sind optisch verschieden.** Auf Bildschirm 2 steht
   die gemessene Leistung gross, der angeforderte Sollwert in einem
   gestrichelt gerahmten Feld darunter. Sie können nicht verwechselt werden.
2. **Der Zustand der Regelung steht ganz oben, nicht in einem Menü.** Auf
   Bildschirm 2 als Zeile «Automatische Regelung aktiv», auf Bildschirm 4
   ausdrücklich als «Automatische Regelung inaktiv · Der Test folgt dem
   Protokoll» (Abnahmekriterium TEST-01, sichtbar gemacht).
3. **Datenlage wird angezeigt, nicht verschwiegen.** Auf Bildschirm 6 trägt
   jede Aussage ihre Einstufung: «Datenlage ausreichend» oder «Nicht
   beurteilbar». Fehlende Erholungsdaten erscheinen als «Daten fehlen» und
   nicht als leerer Platz (REC-01, ANA-03).

**Eingriff ohne Feinmotorik.** Zielwerte werden über 46 px grosse Plus- und
Minusflächen verändert, nie über einen Schieberegler als einzige Möglichkeit.
Pause und Beenden sind auf Bildschirm 2 ohne Menü erreichbar (UX-02).

**Variable Dauer wird ehrlich angezeigt.** Bildschirm 3 nennt bei
kriteriumsgesteuerter Erholung «52 min · höchstens 67 min» statt einer
Scheingenauigkeit, und sagt, was bei Nichterreichen des Kriteriums geschieht.

## Was bewusst fehlt

Keine virtuelle Landschaft, keine Rangliste, keine Abzeichen, keine
Prozentbalken für Stärken, keine zusammengesetzten Scores. Kein
Kalorienverbrauch. Die untere Navigation enthält ausschliesslich
Training · Testung · Entwicklung.

## Offen zur Beurteilung

- Sind die drei Messfelder auf Bildschirm 2 auf dem Lenker aus Armlänge
  lesbar? Das entscheidet sich am realen Telefon, nicht am Entwurf.
- Genügt eine gemeinsame Verlaufskurve, oder brauchen SmO₂ und Leistung
  getrennte, zeitlich synchronisierte Diagramme?
- Reicht «MOXY-Übergang 1/2» als Bezeichnung, oder willst Du eigene
  fachliche Begriffe hinterlegen?
