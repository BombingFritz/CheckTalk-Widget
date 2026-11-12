# AGENTS.md – Regeln für KI-Assistenten

## Vor jeder Änderung
- Schnittstelle, Verhalten und Fahrplan stehen im Server-Repository `BombingFritz/Twitch-Companion-JS-server`: `docs/specs/intro-topics.md`, `docs/FAHRPLAN.md`, ADR 0021. Dort zuerst lesen und den aktuellen Stand auf GitHub prüfen.
- Braucht das Widget etwas Neues vom Server (Feld, Ereignis, Endpoint), zuerst die Spec im Server-Repository ändern, nicht hier erfinden.
- Lies `docs/ARCHITEKTUR.md` und `docs/OFFEN.md` in diesem Repository.

## Konventionen
- Eine HTML-Datei, CSS und JS inline, keine Abhängigkeiten, keine Build-Schritte.
- Das Widget zeigt nur an. Der Zustand gehört dem Server; der Demo-Modus spiegelt dessen Schrittlogik nur zum Testen.
- Animiert werden nur: Abhaken (Haken, Text ausgegraut, nie durchgestrichen), Zurücktreten erledigter Themen, Hervortreten des aktiven Themas, Ein- und Ausblenden der Liste. Keine weiteren Bewegungen ohne Auftrag.
- Ein Design für horizontal und vertikal; Unterschiede nur über URL-Parameter und die Größe der OBS-Quelle.
- Farben und Schriften nur über die CSS-Variablen in `:root`.
- Keine Tokens, IDs oder Secrets; das Widget braucht kein `/token`.

## Lizenz
- MIT (eigenständiges Modul, Projektvorgabe „Lizenz- und Entwicklungskontext“ vom 12.11.2026). Keinen Servercode hierher kopieren; nur die dokumentierte Schnittstelle nutzen.
- Keine Zusatzklauseln an MIT anhängen, Lizenztext nicht ändern.
- Neue Datenflüsse (Netzwerk, Speicherung, externe Dienste) im README unter „Daten, Speicherung, Netzwerk“ nennen.

## Pflicht-Checks
- Script extrahieren und mit `node --check` prüfen.
- Demo-Modus im Browser: 3 Themen, 6 × weiter, Ausblenden nach 5 s; zurück während der 5 s bricht ab; ein/aus nach dem Ausblenden zeigt die fertige Liste dauerhaft.
- In OBS horizontal und vertikal prüfen, Lesbarkeit vor bewegtem Kamerabild.
- `WIDGET_VERSION` und `CHANGELOG.md` gemeinsam anpassen; Versionen über Git-Tags (`vX.Y.Z`).
