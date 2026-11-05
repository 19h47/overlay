# Overlay

A tiny, single-page tracing app. Lay an A3 or A4 sheet on the screen, and copy the image under it, square by square, on a calibrated grid.

## Features

- **Paper at scale** — The sheet is drawn at the size of an A3 or A4 sheet laid on the screen, in landscape, portrait or the orientation of the screen, once the screen width is calibrated in millimetres.
- **Grid and guides** — A grid of squares from 1 cm to 10 cm, the crosshair at the center and the edge of the sheet to lay the paper on, each with its own color, shown or hidden with **G** and **C**.
- **Pan** — Drag the image with a mouse, a finger or a pen, or move it with the arrow keys. **L** locks it in place, so that a hand or a pen on the paper does not move it.
- **Screen kept on** — The screen does not go to sleep while the page is shown, where the browser allows it (Screen Wake Lock API).
- **Filters** — Contrast, brightness and grayscale.
- **White screen** — **W** lights the paper in white, to check the drawing.
- **Fullscreen** — **F**, where the browser allows it: not on iPhone.
- **Light / dark theme** — Follows the system until you toggle it with the button or **Ctrl+K** (**⌘K** on macOS). The grid, the guides and the white screen keep their colors.
- **Auto-save** — Settings and position are saved to localStorage as you change them, and flushed when the tab is hidden or closed. Open tabs stay in sync. **R** resets the settings, but the image and the theme.
- **Keyboard shortcuts** — Hover, click or tap the bottom-right corner to see the list.
- **Accessible** — Toggle buttons with `aria-pressed`, native popovers for the settings, anchored to their button, and the shortcuts, both kept within the screen, labelled fields. Where `appearance: base-select` is not supported (Firefox), a pill shows a classic select with its value only, still labelled. Respects `prefers-reduced-motion`.
- **No build step** — One `index.html` and its `app-style.css`, vanilla JS, and custom elements.

## Images

The images live in `images/`. To add one, drop the file there and add it to the list in the `overlay-config` JSON, in the `<head>` of `index.html`:

```json
{ "name": "Lamentation, Andrea Solario, c. 1505-1507", "file": "lamentation-andrea-solario-1505-1507.jpg" }
```

The same JSON holds the defaults the settings reset to, the screen width among them. Keep file names in lower case: a server may tell `Solario` from `solario`.

## Run locally

Serve the project root with any static server, then open the URL in your browser.

```bash
# Examples (from the project root)
npx serve .
# or
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly works too.

## Tech

- One HTML file with its JavaScript and the styles of its own, plus `app-style.css` for the base styles.
- One store, `window.store`, created by a classic script in `<head>` before the first paint. It holds the state (the options of the page, `theme`, `themeChosen`), checks every value it is given, saves it to localStorage under `overlay-store` and sets `data-theme` and `data-white-screen` on `<html>`.
- Every custom element is a brick: it reads the store with `select()` and changes it through its actions (`setOption`, `toggle`, `setPan`, `movePan`, `toggleTheme`, `reset`); `setPan` does nothing while the sheet is locked. No brick knows another, so removing one leaves the others working.
- Custom elements: `overlay-viewport`, `overlay-sheet`, `overlay-field`, `overlay-toggle`, `overlay-fullscreen-toggle`, `overlay-theme-toggle`, `overlay-reset`, `overlay-shortcuts`.
- The sheet is sized in CSS: a millimetre is `100vw` divided by the screen width in millimetres; the store only sets the paper, the orientation and that width.
- `app-style.css` is the base shared by the small apps (Tumblr, Messenger, SMS, Tweets, Typewriter, Library, Letterhead, Overlay), copied from [its gist](https://gist.github.com/19h47/27df983fac74beb8d3e0a30688654498) with the revision in its header: the toolbar (`.app-toolbar`), its controls (`.app-control`), the pills (`.app-pill`), whose customizable select (`appearance: base-select`) shows its name and opens from anywhere on the pill, the name too, and the fields (`.app-field`) come from it; `index.html` adds the sheet, the settings and the shortcuts. A golden-ratio scale (φ), colors with `light-dark()` and `color-mix()`, classes prefixed `app-`. To update it, copy a newer revision. No frameworks or build tools.
