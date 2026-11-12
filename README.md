# CheckTalk-Widget

Abhakbare Themenliste („Topics of the day“) als OBS-Browserquelle für das Intro in der Vollbild-Cam. Bedienung per Stream Deck über den [Twitch Companion JS server](https://github.com/BombingFritz/Twitch-Companion-JS-server).

> **Schnittstelle und Fahrplan stehen im Server-Repository:** `docs/specs/intro-topics.md` und `docs/FAHRPLAN.md`. Dieses Repository führt keinen eigenen Fahrplan.

## Verhalten
- Klick 1: Thema aktiv (hervorgehoben). Klick 2: erledigt (abgehakt und ausgegraut). Klick 3: nächstes Thema aktiv.
- Eigene Taste zum Ein- und Ausblenden; der Stand bleibt erhalten.
- Sind alle Themen erledigt, blendet die Liste nach 5 Sekunden sanft aus.
- Ohne Server bleibt das Widget unsichtbar.

## Voraussetzungen und Serverkompatibilität
- OBS mit Browserquelle. Entwickelt für eine 2K-Ausgabe (2560 × 1440) und eine zweite, vertikale Ausgabe über ein Dualstream-Plugin im selben OBS.
- Twitch Companion JS server mit dem Modul „Intro-Topics“ (Endpoint `GET /api/topics/stream`, Spec `intro-topics.md`). Erste kompatible Server-Version: [noch nicht veröffentlicht].
- Keine weiteren Abhängigkeiten: eine HTML-Datei, kein Build, keine externen Bibliotheken oder Schriften.

## Einbinden in OBS
Browserquelle, **URL** (nicht „Lokale Datei“, siehe ADR 0013 im Server-Repository):

```
file:///C:/Pfad/zu/CheckTalk.html
```

Größe und Lage legt die Quelle fest; die Schrift passt sich an. Für die vertikale Ausgabe eine zweite Quelle mit derselben Datei anlegen; beide zeigen denselben Stand.

| Parameter | Werte | Standard |
|---|---|---|
| `server` | Adresse des Servers | `http://localhost:3000` |
| `theme` | `brand`, `neutral` | `brand` |
| `anchor` | `tl`, `tr`, `bl`, `br`, `c` (Ecke innerhalb der Quelle) | `tl` |
| `title` | Überschrift; leer = keine | `Topics of the day` |
| `demo` | `1` = ohne Server, Bedienung per Tastatur | aus |
| `items` | nur Demo: Themen, getrennt mit `\|` | Beispielliste |

## Demo ohne Server
`CheckTalk.html?demo=1` im Browser öffnen. Tasten: N, → oder Leertaste = weiter, B oder ← = zurück, H = ein/aus, R = reset.

## Daten, Speicherung, Netzwerk
- Netzwerk: nur eine Verbindung zum eigenen Server unter `localhost` (Server-Sent Events). Keine Verbindungen ins Internet, keine Telemetrie, keine KI- oder Cloud-Dienste.
- Speicherung: keine. Das Widget nutzt weder `localStorage` noch Cookies; die Themenliste liegt im Server.
- Berechtigungen: keine Twitch-Scopes, kein Token, kein Zugriff auf `/token`.

## Fremde Bestandteile
Keine. Die Schriften (Impact bzw. Systemschriften) werden nur über den Namen angefordert und nicht mitgeliefert.

## Lizenz
[MIT](LICENSE). Das Widget ist ein eigenständiges Modul; der Server steht unter einer eigenen Lizenz (siehe Server-Repository).
