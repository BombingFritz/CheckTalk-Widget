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

Schriften (Theme `panel`): Zilla Slab für die Themen, Big Shoulders Stencil für die Überschrift, beide frei (SIL Open Font License) und in die Datei eingebettet.

| Parameter | Werte | Standard |
|---|---|---|
| `server` | Adresse des Servers | `http://localhost:3000` |
| `output` | `horizontal`, `vertikal`: welcher Hintergrund aus der Webkonsole gilt | `horizontal` |
| `theme` | nur für diese Quelle überschreiben: `panel` (Programm-Tafel), `spargel` (wie SpargelPoll), `neutral` | aus der Webkonsole |
| `background` | nur für diese Quelle überschreiben: `transparent`, `solid` | aus der Webkonsole |
| `anchor` | `tl`, `tr`, `bl`, `br`, `c` (Ecke innerhalb der Quelle) | `tl` |
| `scale` | Schriftgröße als Faktor, 0.5 bis 2 | `1` |
| `title` | nur für diese Quelle überschreiben; leer = keine Überschrift | aus der Webkonsole (`Tagesthemen`) |
| `demo` | `1` = ohne Server, Bedienung per Tastatur | aus |
| `items` | nur Demo: Themen, getrennt mit `\|` | Beispielliste |

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

Beide unverändert als Base64 in `CheckTalk.html` eingebettet; Lizenztexte in [LICENSE-FONTS.txt](LICENSE-FONTS.txt). Die Themes `spargel` und `neutral` nutzen Systemschriften (Impact, Segoe UI), die nicht mitgeliefert werden.

## Lizenz
[MIT](LICENSE) für den Code des Widgets. Die eingebetteten Schriften stehen unter der SIL Open Font License 1.1 ([LICENSE-FONTS.txt](LICENSE-FONTS.txt)). Der Server steht unter einer eigenen Lizenz (siehe Server-Repository).
