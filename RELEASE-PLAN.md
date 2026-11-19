# Roadmap

Stand: 19. November 2026 · Version **0.1.6**

**Leitbild „Fire and Forget“ (Nutzer 18.11.):** Die App soll möglichst autonom arbeiten. Der Nutzer wählt nur Zähler und Kamera; alles andere (Grob- und Feineinrichtung, letzte Stelle, Genauigkeit, Fehlerkorrektur) erledigt sie selbst und zeigt Unsicherheit offen an. Manuelle Knöpfe sind nur Notfallwerkzeuge. Stabilität geht vor neuen großen Funktionen.

## Als Nächstes

| Aufgabe | Fertig, wenn … |
| --- | --- |
| **Praxis 0.1.6 beobachten** | Nachschieben fehlender Ziffern (bis 3 Stellen, max. 10 Lesungen) im Alltag beobachten: hilft es bei `?`, ohne Fehlwerte zu übernehmen? |
| **Aus 0.1.4 beobachten** | Verbrauch wird erst nach Bestätigung gebucht; die 8. Ziffer wird über die Rollenmitte gefunden (bisher nur an synthetischen Bildern getestet, am echten Zähler beobachten: findet die Selbstprüfung die Mitte, sitzt der Rahmen danach mittig?); „Rahmen sitzt daneben“ löst die neue Suche aus; einzelne Fehlprüfungen bleiben dezent. |
| **Letzten bestätigten Stand als Lesehilfe nutzen** | Beobachtung 19.11., 01:53–02:03: vier Fehlprüfungen in Folge, weil die 5. Ziffer „8“ abwechselnd als 8, 6 oder ? gelesen wurde (z. B. `10478.65?`, `10476.65?`, `1047?.?5?`), obwohl kurz vorher `10478.548` bestätigt war. Eine 6 ist dort unmöglich (Stand kann nicht sinken, max. Durchfluss erlaubt nur 8 oder 9). Bei uneinigen Lesungen nur Ziffern zulassen, die zum letzten bestätigten Stand und zum Durchfluss passen; gleichen sich die übrigen Stellen, den Wert so auflösen. Prüfen, ob der Rahmen der 5. Ziffer rechts etwas abschneidet (8 → 6). |
| **Bilddrehung fließend statt in ganzen Grad** | Vorschlag Nutzer 19.11.: Statt feste Gradschritte (±1°, ±6°) per OCR auszuprobieren, den Winkel direkt aus dem Bild messen: Ausschnitt verkleinert in feinen Schritten (z. B. 0,1°) drehen und den Winkel nehmen, bei dem Ober- und Unterkante der Ziffernreihe am schärfsten waagerecht liegen (Zeilenprofil), bzw. eine Gerade durch die Ziffernmitten legen. Ohne OCR, daher schnell; Ergebnis als Kommawert speichern (Eingabefeld und Einstellungen auf 0,1° umstellen). Kleine Abweichungen laufend nachführen, große Sprünge nur nach Bestätigung durch erfolgreiche Lesungen übernehmen. Anlass: 19.11. lag der beste Winkel vermutlich zwischen −2° und −1°; eine 8 wurde als 6 gelesen. **Test 19.11.:** Kantenprofil an künstlich gedrehten Bildern sehr genau (Ø 0,07°, max. 0,13°), an 12 echten Demo-Bildern von AI on the Edge (Wasserzähler, feste Kamera) aber unbrauchbar (Ergebnis hing vom Startwinkel ab, ließ sich von Gehäusekanten ablenken). Gerade durch die Ziffern-Schwerpunkte war unabhängig vom Startwinkel, schwankte aber je Bild um ±1–2° (drehende Rollen verschieben die Schwerpunkte); mit Kantenprofil nachverfeinert noch ±0,8°. Einzelbild also nicht genauer als ganze Grad. Weiter nur mit Mittelwert (Median) über viele Prüfungen und erst im Protokoll mitlaufen lassen, bevor es automatisch dreht. |
| **Fehlertext ohne falsche Ratschläge** | Bei „Ziffern schwer lesbar“ steht „Prüfe Bilddrehung, Kamerawinkel, Fokus und Beleuchtung“, obwohl sich die Kamera meist nicht verändert hat. Erst raten, wenn viele Stellen unklar sind oder sich das Bild sichtbar verändert hat; sonst „Einzelne Ziffern waren uneindeutig, die App versucht es weiter“. Pause nach drei Fehlprüfungen (60 min) überdenken, wenn die Fehler nur eine Stelle betreffen. |
| **Laufende Rolle / Genauigkeit nach Durchfluss (beobachten)** | Die Fehlprüfungen am 18.11. kamen nicht vom Durchfluss, sondern vom falschen Startwert (in 0.1.1 behoben). Nur umsetzen, wenn das Debug-Protokoll zeigt, dass eine drehende Rolle bei hohem Verbrauch Prüfungen scheitern lässt: dann niedrigeren Wert nehmen bzw. vorübergehend eine Stelle weniger vergleichen, mit Hinweis „Zähler läuft gerade schnell“. |
| **Fremde Zähler und Wasserzähler** | Test 18.11. mit ca. 2000 öffentlichen Ziffernbildern (Wasser-/Gaszähler): Einzelziffern nur zu etwa 45 % richtig, „1“ besonders schwach (oft unklar oder als 4 gelesen). Praktisch getestet ist nur der eigene Gaszähler (Stromzähler bereits vernetzt, Wasserzähler nicht zugänglich). Verbesserung über einen Einzelziffern-Klassifikator prüfen (siehe Später). |
| **Vorprüfung ohne geöffnete App** | Sobald Bildquelle bzw. Kamera feststeht, startet die Grob-Einrichtung von selbst im Hintergrund, auch wenn niemand die Weboberfläche offen hat. |
| **Auffindbarkeit** | Repository-Beschreibung und Topics (home-assistant, ocr, gas-meter …), Screenshot mit Demo-Daten im README, Vorstellung im Home-Assistant-Forum/Community-Store vorbereiten. |
| **Manuelle Korrektur in der Selbsteinrichtung** | Beim Markieren des Rahmens pausiert die Einrichtung; Winkel einstellbar; Ziffernrahmen verschieben und in der Größe ändern; danach geht es mit den korrigierten Rahmen weiter. |
| **Plausibilität nach eigenem Durchschnittsverbrauch** | Große Sprünge zählen nur, wenn sie zum bisherigen Verbrauch passen; bis genug Daten vorliegen, gilt nur der maximale Durchfluss. |
| **Anleitung mit Bildern neu** | Kurze bebilderte Anleitung aus der Demo (keine privaten Zählerfotos). |
| **Einstellungen nachvollziehbar** | Sichtbar, ob ein Wert aus der App-Konfiguration oder aus der Weboberfläche stammt. |
| **Foto-Regressionstests** | Eigene Testbilder mit Sollwerten laufen in GitHub Actions. |

## Mit der ESPHome-Kamera

- Echter Test mit höherer Auflösung am eigenen Zähler (bisher nur simuliert).
- Optionale LED-Steuerung: Licht nur für die Aufnahme ein- und danach sicher wieder ausschalten.

## Später

- Mehrere Zähler mit eigener Quelle, eigenen Rahmen und eigener Historie.
- Hallsensor / Impulszähler über ESPHome als Kontrollquelle.
- Einzelziffern-Klassifikator als Machbarkeitsstudie gegen Tesseract.
- Weitere Zählertypen nach praktischen Tests.

## Regeln

- App-Slug `rf1_ocr_test` und die `rf1_*`-Entitäten bleiben gleich, damit Dashboards und Energie-Statistik weiterlaufen.
- Nur Stable-Releases; jede Änderung über Branch, Pull Request und grüne Prüftests.
- Roadmap und Zwischenstände werden vor jedem größeren Schritt gesichert.
- Erledigtes steht im [Changelog](rf1_ocr_test/CHANGELOG.md), die Zeit vor 0.1.0 im [Rückblick](docs/HISTORY.md).
