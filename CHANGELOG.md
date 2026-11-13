# Changelog

Format: [Keep a Changelog](https://keepachangelog.com/de/1.1.0/), Versionen nach [Semantic Versioning](https://semver.org/lang/de/).

## [Unreleased]

## [0.2.1] – 2026-11-13
### Added
- Diagnosemodus `?debug=1`: Feld unten links zeigt, ob die Quelle mit dem Server verbunden ist, und sonst den wahrscheinlichen Grund (über „Lokale Datei“ geladen, Server nicht erreichbar, falsche Server-Version). README mit Abschnitt „Fehlersuche“.

## [0.2.0] – 2026-11-12
### Added
- Anzeige-Einstellungen vom Server: Überschrift, Design und Hintergrund je Ausgabe kommen aus der Webkonsole (Reiter „Topics“) und gelten sofort. Neuer Parameter `output=horizontal|vertikal`; `theme`, `background` und `title` überschreiben nur noch für eine Quelle.
- Hintergrund „deckend“: Beim Einblenden füllt das Widget die ganze Browserquelle und überdeckt darunterliegende Quellen (z. B. Untertitel im Hochformat).
- Freie Schriften eingebettet: Zilla Slab (Themen) und Big Shoulders Stencil (Überschrift), SIL Open Font License 1.1, siehe `LICENSE-FONTS.txt`.
### Changed
- Standard-Überschrift „Tagesthemen“.
- Ein- und Ausblenden betrifft die ganze Quelle statt nur der Tafel.
### Fixed
- Demo-Modus: Ein weiterer Klick auf „weiter“ bei fertiger Liste hielt das automatische Ausblenden an.

## [0.1.1] – Entwurf
### Added
- Theme `panel` als Standard: Schiefertafel mit Eisenrahmen und Nieten, bernsteinfarbenes Leuchtschild als Überschrift, cremefarbene Serifenschrift, aktives Thema mit glimmendem Kästchen. Abgestimmt auf die Programm-Tafel der Intro-Szene.
- URL-Parameter `scale` (0.5–2) zum Feinjustieren der Schriftgröße.
### Changed
- Bisheriges Standard-Theme heißt jetzt `spargel`; `theme=brand` entfällt.

## [0.1.0] – Entwurf
### Added
- `CheckTalk.html`: Liste mit den Status offen, aktiv, erledigt (abgehakt und ausgegraut); Animationen für Abhaken, Zurücktreten, Hervortreten und Ein-/Ausblenden.
- Anbindung per Server-Sent Events an `/api/topics/stream` (Server-Seite noch nicht umgesetzt, siehe Fahrplan im Server-Repository).
- Demo-Modus `?demo=1` mit Tastaturbedienung und derselben Schrittlogik wie der Server.
- URL-Parameter `server`, `theme`, `anchor`, `title`, `items`.
- MIT-Lizenz.
