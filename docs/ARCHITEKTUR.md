# Architektur

```
Stream Deck ──POST──> Twitch Companion JS server ──Server-Sent Events──> CheckTalk.html (OBS, horizontal)  
                                │                                    └──> CheckTalk.html (OBS, vertikal)  
                                └── scheduleEffect ──> Home Assistant (optional)
```

- **Zustand:** liegt im Server (ADR 0021 im Server-Repository). Das Widget empfängt bei jeder Verbindung und jeder Änderung den ganzen Zustand (Ereignis `state`) und stellt ihn dar. Ältere Stände (`revision`) werden verworfen.
- **Verbindung:** `EventSource` auf `<server>/api/topics/stream`. Bricht sie ab, blendet das Widget aus; `EventSource` verbindet selbst neu.
- **Darstellung:** `render(state)` setzt nur `data-status` je Eintrag und die Klasse `visible` an der Platte; alle Bewegungen sind CSS-Übergänge. Die Einträge werden nur neu aufgebaut, wenn sich die Titel ändern, damit laufende Übergänge nicht abbrechen.
- **Ausblenden:** Den 5-s-Timer führt der Server. `hide_reason: "auto"` ergibt eine langsamere Ausblendung als `"manual"`.
- **Demo-Modus:** bildet die Schrittlogik des Servers nach (`step` von 0 bis 2 × Anzahl), damit Design und Ablauf ohne Server prüfbar sind.
- **Herkunft:** Einbindung per `file:///`; Herkunft `null` ist im Server erlaubt (ADR 0013 im Server-Repository).
