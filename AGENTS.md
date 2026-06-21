# AGENTS.md

## Cursor Cloud specific instructions

### Project overview
This repo is a single static client-side web game, **"Кликер 67" / "Clicker 67"**, built for the Yandex Games platform. It is plain HTML/CSS/JS with **no build system, no package manager, and no test framework**. The source ships inside `11668736.zip` (containing `index.html`, `css/`, `js/`).

### Source layout
- The game source lives **inside `11668736.zip`**. The update script extracts it to `extracted/` (git-ignored). Edit files in `extracted/` to iterate, but the zip is the committed source of truth.
- `extracted/serve.ps1` is the original Windows helper (PowerShell); on Linux use `python3` directly (see below).

### Running the app (dev)
There is nothing to build. Serve the extracted static files:

```
python3 -m http.server 8080 --directory extracted
```

Then open `http://localhost:8080/index.html`.

### Non-obvious caveats
- `index.html` loads `/sdk.js` (the Yandex Games SDK). That file only exists on the Yandex platform, so locally it returns **404 — this is expected**. The code (`js/sdk-bridge.js`) detects the missing SDK and runs in "local mode", persisting progress to `localStorage` instead of the cloud.
- UI language auto-detects from the browser/SDK locale (`js/i18n.js`); it shows Russian by default but renders English when the browser locale is English. Both are correct behavior.
- There are **no lint, test, or build commands** for this project. Verification is manual: load the page and confirm clicking the "67" button increases the Points/Clicks counters and unlocks achievements.
