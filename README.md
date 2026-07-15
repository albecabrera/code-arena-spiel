# CODE ARENA 🚀

Browser-Minigame für die Informatik-Belohnungsstunde (WP-Informatik, Klasse 7). Eine einzige HTML-Datei, kein Build-Prozess, keine Installation — einfach öffnen und spielen.

**Live:** https://albecabrera.github.io/code-arena-spiel/

## Konzept

Schüler:innen beantworten Multiple-Choice-Fragen aus dem Informatikunterricht (Caesar-Verschlüsselung, Binär-/Dezimalsystem, Scratch, XLogo). Für jede richtige Antwort wird eines von drei Action-Levels freigeschaltet. Am Ende der Stunde folgt ein anonymes Feedback-Formular, danach die Endauswertung.

## Ablauf

```
Start (Fullscreen) → Frage → Anweisung (2-3s Countdown) → Level (Action) → Level-Ende → nächste Frage → …
      → (nach 25 Min oder Fragenkatalog leer) → Feedback → Danke-Screen (Fullscreen)
```

## Fullscreen-Modus

Ein Klick auf **„▶ Los geht's!“** fordert den Browser-Vollbildmodus an (`requestFullscreen()`). Das Spiel bleibt bis zum Ende — inklusive Feedbackbogen und Danke-Screen — im Vollbild, damit die Schüler:innen alles gut lesen können (z. B. auf dem Beamer). Der Browser verlangt dafür eine echte Nutzer-Interaktion (Klick), daher lässt sich Vollbild nicht automatisch beim Laden aktivieren.

## Spielmodi

| Level | Ziel | Steuerung |
|---|---|---|
| ☄️ Meteorsturm | Meteoriten ausweichen, Datenchips sammeln | `←` `→` **oder** `A` `D` |
| 🤖 Bug-Run | Über Bugs springen, Bits sammeln | `Leertaste` **oder** `W` |
| 🦠 Viren-Abwehr | Bewegen & Viren abschießen | `←` `→`/`A` `D` · Schießen: `Leertaste`/`W` |

Pfeiltasten und WASD sind gleichwertig nutzbar (siehe `normKey()` im Script).

Vor jedem Level erscheint ein 2-3 Sekunden langer Countdown-Screen mit Levelname, klarer Anweisung ("worum geht's") und der passenden Tastenkombination — erst danach startet das Level automatisch (`starteLevel()`).

## Kurzlink & QR-Code

Auf dem Start-Screen steht ein großer Kurzlink plus QR-Code, damit Schüler:innen das Spiel schnell auf dem eigenen Handy öffnen können. Der QR-Code wird zur Laufzeit über die QR-Server-API aus der Kurzlink-URL generiert (`#join-link` im HTML).

**Wichtig:** Ändert sich die gehostete URL (z. B. eigene Domain statt GitHub Pages), muss das `href` von `#join-link` in `index.html` angepasst werden — der QR-Code aktualisiert sich automatisch mit.

## Feedback & Lehrkraft-Ansicht

- Feedback (Sterne-Bewertung + 3 Freitextfelder) wird anonym über `window.storage` gespeichert (geteilter Speicher, keine Namen).
- Die Freitextfelder erlauben normales Tippen (Groß-/Kleinschreibung, Leertaste, `a`/`d`/`w` etc.) — die Spiel-Tastensteuerung (WASD/Pfeile/Leertaste) ist deaktiviert, solange ein Textfeld fokussiert ist (siehe `inTextField()` im Script).
- Zugang zur Auswertung über den 🔑-Button unten rechts, geschützt mit PIN (siehe `KONFIG.LEHRER_PIN`).
- Auswertung zeigt alle Rückmeldungen als Tabelle, inkl. CSV-Export und "Alle löschen".

## Konfiguration

Alles Wichtige steht oben im `<script>`-Block in `index.html`:

```js
const KONFIG = {
  LEVEL_DAUER: 120,        // Spielzeit pro Level in Sekunden
  STUNDEN_DAUER: 25 * 60,  // Nach so vielen Sekunden erscheint der Feedbackbogen
  LEHRER_PIN: "2026",      // PIN für die Lehrkraft-Auswertung
};
```

Fragen liegen im Array `FRAGEN` (Format: `{t: Thema, q: Frage, a: [Antworten], k: Index der richtigen Antwort}`) — neue Fragen einfach ergänzen.

## Hosting via GitHub Pages

1. Repo pushen (Dateiname muss `index.html` heißen, damit die Root-URL funktioniert).
2. GitHub → Repo → **Settings → Pages** → Branch `main` / Ordner `/ (root)` auswählen.
3. Nach ein paar Minuten ist das Spiel unter `https://<username>.github.io/<repo>/` live.

## Struktur

Eine Datei (`index.html`), drei Teile:

- **CSS** (`<style>`) — Theme, Screens, HUD, Konfetti, Kurzlink-/QR-Box.
- **HTML** — Screens (`start`, `question`, `level`, `levelend`, `feedback`, `thanks`, `teacher`) plus PIN-Modal.
- **JS** (`<script>`) — Fragenlogik, Canvas-Spiel-Engine (drei Level-Klassen), Feedback-Speicherung, Lehrkraft-Auswertung, Konfetti, Sternenhimmel-Hintergrund.
