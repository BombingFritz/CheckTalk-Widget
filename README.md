# CheckTalk-Widget

Abhakbare Themenliste („Tagesthemen“) als OBS-Browserquelle für das Intro in der Vollbild-Cam. Bedienung per Stream Deck über den [Twitch Companion JS server](https://github.com/BombingFritz/Twitch-Companion-JS-server).

> **Schnittstelle und Fahrplan stehen im Server-Repository:** `docs/specs/intro-topics.md` und `docs/FAHRPLAN.md`. Dieses Repository führt keinen eigenen Fahrplan.

## Verhalten
- Klick 1: Thema aktiv (hervorgehoben). Klick 2: erledigt (abgehakt und ausgegraut). Klick 3: nächstes Thema aktiv.
- Eigene Taste zum Ein- und Ausblenden; der Stand bleibt erhalten.
- Sind alle Themen erledigt, blendet die Liste nach 5 Sekunden sanft aus.
- Ohne Server bleibt das Widget unsichtbar.
- Themen, Überschrift, Design und Hintergrund stellst du in der Webkonsole des Servers ein (Reiter „Topics“); die Widgets übernehmen Änderungen sofort.

## Voraussetzungen und Serverkompatibilität
- OBS mit Browserquelle. Entwickelt für eine 2K-Ausgabe (2560 × 1440) und eine zweite, vertikale Ausgabe über ein Dualstream-Plugin im selben OBS.
- Twitch Companion JS server mit dem Modul „Intro-Topics“ (Endpoint `GET /api/topics/stream`, Spec `intro-topics.md`). Erste kompatible Server-Version: [noch nicht veröffentlicht].
- Keine weiteren Abhängigkeiten: eine HTML-Datei, kein Build, keine externen Bibliotheken. Die Schriften sind eingebettet.

## Einbinden in OBS
Browserquelle, **URL** (nicht „Lokale Datei“, siehe ADR 0013 im Server-Repository):

```
file:///C:/Pfad/zu/CheckTalk.html
```

Größe und Lage legt die Quelle fest; die Schrift passt sich an. Für die vertikale Ausgabe eine zweite Quelle mit derselben Datei und `output=vertikal` anlegen; beide zeigen denselben Stand.

**Hintergrund:** `durchscheinend` legt nur die halbtransparente Tafel über das Bild. `deckend` füllt beim Einblenden die ganze Browserquelle (schwarz, Tafel ohne Durchsicht) und überdeckt so, was darunter liegt, etwa Dreh-Logo und Untertitel im Hochformat. Einstellbar je Ausgabe in der Webkonsole; Standard: horizontal durchscheinend, vertikal deckend.

Empfohlene Quellen (abgestimmt auf die Intro-Szene, 12.11.2026):

| Ausgabe | Quelle (Breite × Höhe) | Position | Parameter |
|---|---|---|---|
| horizontal, 2560 × 1440 | 940 × 720 | links, x 40, y 540 (unter der Landkarte) | `output=horizontal&anchor=tl` |
| vertikal, 1080 × 1920 | 1080 × 730 | unterer schwarzer Bereich, y 1190 | `output=vertikal&anchor=c&scale=1.1` |

Vertikal steht die Liste dort, wo sonst Dreh-Logo und Untertitel stehen. Die Quelle muss in OBS **über** diesen Quellen liegen; mit deckendem Hintergrund verschwinden sie, solange die Liste eingeblendet ist.

Schriften: Theme `panel` nutzt Zilla Slab für die Themen und Big Shoulders Stencil für die Überschrift, Theme `clean` Atkinson Hyperlegible. Alle drei sind frei (SIL Open Font License) und in die Datei eingebettet.

**Theme `clean`:** nur die Liste als Text, ohne Tafel und Rahmen. Weiße Schrift mit dunkler Kontur und weichem Schatten, gut lesbar auf gleichmäßigem hellem wie dunklem Hintergrund. Die Tafel ist vorhanden, aber ganz durchsichtig; mit `&plate=40` bekommt sie 40 % Deckkraft. Im Hintergrund „deckend“ füllt das Widget die Quelle wie bei den anderen Designs schwarz.

| Parameter | Werte | Standard |
|---|---|---|
| `server` | Adresse des Servers | `http://localhost:3000` |
| `output` | `horizontal`, `vertikal`: welcher Hintergrund aus der Webkonsole gilt | `horizontal` |
| `theme` | nur für diese Quelle überschreiben: `panel` (Programm-Tafel), `clean` (nur Text), `spargel` (wie SpargelPoll), `neutral` | aus der Webkonsole |
| `plate` | nur Theme `clean`: Deckkraft der Tafel hinter dem Text in Prozent, 0 bis 100 | `0` (keine Tafel) |
| `background` | nur für diese Quelle überschreiben: `transparent`, `solid` | aus der Webkonsole |
| `anchor` | `tl`, `tr`, `bl`, `br`, `c` (Ecke innerhalb der Quelle) | `tl` |
| `scale` | Schriftgröße als Faktor, 0.5 bis 2 | `1` |
| `title` | nur für diese Quelle überschreiben; leer = keine Überschrift | aus der Webkonsole (`Tagesthemen`) |
| `debug` | `1` = Diagnosefeld unten links: verbunden (grün) oder warum nicht (rot) | aus |
| `demo` | `1` = ohne Server, Bedienung per Tastatur | aus |
| `items` | nur Demo: Themen, getrennt mit `\|` | Beispielliste |

## Fehlersuche: Widget bleibt unsichtbar
Zeigt die Webkonsole im Reiter „Topics“ „Widgets/Plugin: keins verbunden“, erreicht die Quelle den Server nicht. `&debug=1` an die URL der Quelle hängen und die Seite neu laden; das Feld unten links nennt die Ursache:

| Meldung | Lösung |
|---|---|
| „Seite geladen über http://absolute …“ | In den Eigenschaften der Quelle „Lokale Datei“ **aus**schalten und die Datei als URL `file:///C:/…/CheckTalk.html?…` eintragen |
| „Server nicht erreichbar …“ | Server läuft nicht, Port weicht ab (`&server=http://localhost:<PORT>`), oder OBS blockiert die Anfrage |
| „HTTP 404 …“ | Server-Version ohne Intro-Topics: `git pull`, Server neu starten |
| grün „verbunden“, aber nichts zu sehen | Liste ist ausgeblendet (Webkonsole: „Ein/Aus“), Modul aus, oder die Quelle liegt in OBS unter anderen Quellen bzw. außerhalb des Bildes |

Die Datei muss die echte `CheckTalk.html` sein (auf GitHub „Download raw file“), nicht die per Rechtsklick gespeicherte GitHub-Seite.

## Demo ohne Server
`CheckTalk.html?demo=1` im Browser öffnen. Tasten: N, → oder Leertaste = weiter, B oder ← = zurück, H = ein/aus, R = reset.

## Daten, Speicherung, Netzwerk
- Netzwerk: nur eine Verbindung zum eigenen Server unter `localhost` (Server-Sent Events). Keine Verbindungen ins Internet, keine Telemetrie, keine KI- oder Cloud-Dienste.
- Speicherung: keine. Das Widget nutzt weder `localStorage` noch Cookies; die Themenliste liegt im Server.
- Berechtigungen: keine Twitch-Scopes, kein Token, kein Zugriff auf `/token`.

## Fremde Bestandteile
| Bestandteil | Herkunft | Lizenz |
|---|---|---|
| Zilla Slab Bold, Teilmenge latin | The Mozilla Foundation, über `@fontsource/zilla-slab` 5.3.0 | SIL Open Font License 1.1 |
| Big Shoulders Stencil Display Bold, Teilmenge latin | über `@fontsource/big-shoulders-stencil-display` 5.3.0 | SIL Open Font License 1.1 |
| Atkinson Hyperlegible Bold, Teilmenge latin | Braille Institute of America, über `@fontsource/atkinson-hyperlegible` 5.3.0 | SIL Open Font License 1.1 |

Alle unverändert als Base64 in `CheckTalk.html` eingebettet; Lizenztexte in [LICENSE-FONTS.txt](LICENSE-FONTS.txt). Die Themes `spargel` und `neutral` nutzen Systemschriften (Impact, Segoe UI), die nicht mitgeliefert werden.

## Lizenz
[MIT](LICENSE) für den Code des Widgets. Die eingebetteten Schriften stehen unter der SIL Open Font License 1.1 ([LICENSE-FONTS.txt](LICENSE-FONTS.txt)). Der Server steht unter einer eigenen Lizenz (siehe Server-Repository).
