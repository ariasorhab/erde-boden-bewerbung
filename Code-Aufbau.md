# So ist der Code aufgebaut

Diese Seite erklärt kurz, wie BoreholeHub (Version 1.0.2) technisch aufgebaut ist. Sie ist für Leser ohne Programmierkenntnisse gedacht.

## Kurz gesagt

BoreholeHub ist eine Web-Anwendung für Bohrdaten. Sie nutzt ASP.NET Core 8 (Razor Pages) und Entity Framework Core. Als Datenbank dient SQL Server oder SQLite. Alle Projektdaten in der Demo sind synthetisch, also erfunden. Die App wurde mit KI-Unterstützung erstellt. Entworfen und geprüft hat sie Sohrab Talebian Far. Das steht auch im Fußbereich jeder Seite.

## Aufbau in Ordnern

| Ordner | Inhalt |
|---|---|
| [src/BoreholeHub.Core](https://github.com/ariasorhab/borehole-hub/tree/main/src/BoreholeHub.Core) | Das Fachliche: Datenobjekte (Projekt, Aufschluss, Schicht, Probe, Laborwert) und die Regeln (Koordinaten, Prüfregeln, Labor, Versickerung). Keine Abhängigkeit von Web oder Datenbank. |
| [src/BoreholeHub.Web](https://github.com/ariasorhab/borehole-hub/tree/main/src/BoreholeHub.Web) | Die Web-Anwendung: Seiten, Datenbankzugriff, CSV-Import, Zeichnungen, PDF-Bericht, Karte. |
| [src/BoreholeHub.Web/Drawing](https://github.com/ariasorhab/borehole-hub/tree/main/src/BoreholeHub.Web/Drawing) | Bohrprofil, Rammdiagramm und Profilschnitt als Vektorgrafik (SVG). |
| [src/BoreholeHub.Web/Reports](https://github.com/ariasorhab/borehole-hub/tree/main/src/BoreholeHub.Web/Reports) | Der PDF-Bericht. |
| [tests/BoreholeHub.Tests](https://github.com/ariasorhab/borehole-hub/tree/main/tests/BoreholeHub.Tests) | Die automatischen Tests. |
| [scripts](https://github.com/ariasorhab/borehole-hub/tree/main/scripts) | Start- und Build-Skripte für Windows. |

## Wie die Daten fließen

Daten kommen auf zwei Wegen in die App: über das Feldformular oder über CSV-Dateien. Beides landet in der Datenbank. Beim Import werden die Daten vorher geprüft. Gibt es Fehler, wird nichts gespeichert. Danach laufen die Prüfregeln über alle Punkte. Aus den gespeicherten Daten entstehen das Bohrprofil, der Profilschnitt, der PDF-Bericht mit seinen Anlagen und die Karte. Die Koordinaten rechnet der Server von ETRS89/UTM 32 (EPSG:25832) in Länge und Breite um. Die Seiten erscheinen auf Deutsch.

## Prüfregeln

Sieben Regeln prüfen jeden Punkt. Jeder Befund ist ein Fehler oder ein Hinweis.

- R1 Schichtfolge: Schichten beginnen bei 0 und haben keine Lücken oder Überlappungen.
- R2 Endteufe: Endteufe und Grundwassertiefe sind plausibel.
- R3 Proben: Proben liegen innerhalb der Bohrung.
- R4 Koordinaten: Der Punkt liegt im erwarteten Gebiet und nicht weit von den anderen Punkten.
- R5 Ansatzhöhe: Die Geländehöhe ist plausibel.
- R6 Rammsondierung: Die Schlagzahlen sind vollständig und plausibel.
- R7 Versickerung: Durchlässige Schichten haben einen kf-Wert.

## Tests

Das Testprojekt hat 30 Dateien mit 235 Testmethoden. Einige Methoden laufen mit mehreren Beispielwerten. Beim letzten Lauf waren es 263 automatische Tests, alle bestanden. Sie prüfen Regeln, Import, Feldformular, Profilschnitt, PDF und Karte.

## Selbst starten

- Live-Demo: [borehole-hub-3a93c.containers.snapdeploy.app](https://borehole-hub-3a93c.containers.snapdeploy.app)
- Windows-Version (64 Bit, ohne Installation): Laden Sie auf GitHub in den Releases die Datei BoreholeHub-1.0.2-win-x64.zip herunter und entpacken Sie sie. Starten Sie dann Start-BoreholeHub.cmd per Doppelklick. Die App läuft mit SQLite, Sie brauchen also keinen SQL Server. Der Browser öffnet http://localhost:5080. Klicken Sie dort auf "Demo-Projekt laden". Die zweite Datei, Start-BoreholeHub-SQLServer.cmd, ist für einen lokalen SQL Server gedacht.

## Ehrliche Hinweise

- Normen werden nur "in Anlehnung, vereinfacht" angewendet.
- Die EBV-Klassen sind erfundene Beispielwerte, keine gesetzlichen Grenzwerte.
- Die Versickerungsprüfung ersetzt keine Bemessung nach DWA-A 138-1.
