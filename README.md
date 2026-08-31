# Aktien-Analyse

Ein Werkzeug für technische Kursanalyse — eine einzige HTML-Datei, die im Browser läuft.
Kein Server, kein Build, keine externen Bibliotheken: Charts, Indikatoren und Berechnungen
stecken vollständig in `index.html`.

## Was es kann

**Chart**
- Kerzen- oder Liniendarstellung
- Gleitende Durchschnitte SMA 20 / 50 / 200 und EMA 20
- Bollinger-Bänder (20 Perioden, 2 Standardabweichungen)
- Zeitraum von 3 Monaten bis Gesamthistorie, Intervall Tag / Woche / Monat
- Mausrad zoomt, Ziehen verschiebt den Ausschnitt, Doppelklick setzt zurück
- Fadenkreuz mit OHLC-Werten, Kursmarke an der Achse

**Zusatzfenster**
- Volumen, eingefärbt nach Auf- und Abwärtstagen
- RSI (14) nach Wilder, mit den Schwellen 30 und 70
- MACD (12/26/9) mit Signallinie und Histogramm

**Kennzahlen und Auswertung**
- Letzter Kurs, Tagesveränderung, Performance im gewählten Zeitraum
- 52-Wochen-Hoch und -Tief, Abstand zum Hoch, Lage in der Jahresspanne
- Annualisierte Volatilität aus täglichen Log-Renditen
- ATR (14), absolut und in Prozent des Kurses
- Kurzauswertung in Worten: Trendlage, RSI-Zustand, MACD-Stellung, Position im Bollinger-Band

**Bedienung**
- Watchlist, Indikator-Auswahl und Darstellung bleiben lokal im Browser gespeichert
- Symbolsuche mit Vorschlägen
- CSV-Import als Alternative zur API

## Einrichten

Die Datei kann direkt geöffnet werden (Doppelklick) oder über GitHub Pages laufen.
Für Kursdaten wird ein kostenloser Schlüssel von [Twelve Data](https://twelvedata.com/pricing) gebraucht:

1. Auf twelvedata.com den Basic-Plan wählen und registrieren — 800 Abrufe pro Tag, 8 pro Minute
2. Im Dashboard den API-Key kopieren
3. Im Tool oben rechts auf **API-Key** klicken und einfügen

Der Schlüssel wird ausschließlich im `localStorage` des eigenen Browsers abgelegt und
nur an Twelve Data geschickt. Er landet nie im Repository — im Code steht kein Schlüssel.

Abgedeckt sind im kostenlosen Plan US-Aktien und ETFs. Deutsche Börsenplätze gehören
dort zum Bezahlplan; dafür ist der CSV-Weg gedacht.

## Ohne API-Key: CSV

Über den Knopf **CSV** lässt sich ein Kursexport laden, etwa aus dem Broker-Depot oder
von Yahoo Finance. Erwartet werden Spalten mit Datum und Schlusskurs, Open/High/Low/Volume
werden mitgenommen, wenn vorhanden:

```
Date,Open,High,Low,Close,Volume
2026-08-28,229.10,231.44,228.67,231.02,41230000
```

Komma und Semikolon als Trennzeichen funktionieren beide, ebenso deutsche Datums-
und Zahlenformate (`28.08.2026`, `231,02`).

## Auf GitHub Pages veröffentlichen

Damit die Seite direkt unter der Repo-Adresse erscheint, muss die Datei im Wurzelverzeichnis
`index.html` heißen — sonst rendert GitHub Pages stattdessen diese README.

> Settings → Pages → Source: `Deploy from a branch`, Branch: `main`, Ordner `/ (root)`

## Rechenwege

Die Indikatoren sind bewusst nach den gängigen Definitionen implementiert und gegen
Referenzwerte geprüft:

| Indikator | Definition |
|---|---|
| SMA | einfacher Durchschnitt der letzten n Schlusskurse |
| EMA | exponentiell, Glättungsfaktor 2/(n+1), Startwert ist der SMA über die ersten n Werte |
| Bollinger | SMA(20) ± 2 × Standardabweichung (Populationsformel) |
| RSI | Wilder-Glättung der Auf- und Abwärtsbewegungen über 14 Perioden |
| MACD | EMA(12) − EMA(26), Signallinie EMA(9) darauf, Histogramm als Differenz |
| ATR | Wilder-Glättung der True Range über 14 Perioden |
| Volatilität | Standardabweichung der täglichen Log-Renditen des letzten Jahres, mit √252 skaliert |

## Hinweis

Das Tool beschreibt, was die Indikatoren zu einem Zeitpunkt anzeigen. Es gibt keine
Empfehlung ab, trifft keine Prognose und ersetzt keine Anlageberatung. Kursdaten können
verzögert, lückenhaft oder fehlerhaft sein — vor einer Entscheidung gehören sie gegen
die Quelle des Brokers geprüft.
