# Overlay

A tiny, single-page tracing app. Lay an A3 or A4 sheet on the screen, and copy the image under it, square by square, on a calibrated grid.

## Features

- **Paper at scale** — The sheet is drawn at the size of an A3 or A4 sheet laid on the screen, in landscape, portrait or the orientation of the screen. To calibrate it, lay a bank card on the screen and size the rectangle drawn under it to the card: the scale holds when the screen turns or the window changes.
- **Your own image** — The ⤒ button opens an image of the device, without adding it to the project: it joins the list of the images until the page is closed, and is not saved.
- **Grid and guides** — A grid of squares from 1 cm to 10 cm, the crosshair at the center and the edge of the sheet to lay the paper on, each with its own color, shown or hidden with **G** and **C**.
- **Pan** — Drag the sheet with a mouse, a finger or a pen, or move it with the arrow keys. **L** locks it in place, so that a hand or a pen on the paper does not move it.
- **Framing** — Zoom, turn and mirror the image in the settings, or zoom it with **+** and **−** and mirror it with **M**; with ✥ or **I**, the drag and the arrows move the image on the sheet rather than the sheet. The image ends at the edge of the sheet. The lock holds it too: its zoom, its rotation and its mirror can't be changed while it is on.
- **Screen kept on** — The screen does not go to sleep while the page is shown, where the browser allows it (Screen Wake Lock API).
- **Filters** — Contrast, brightness and grayscale.
- **White screen** — **W** lights the paper in white, to check the drawing.
- **Fullscreen** — **F**, where the browser allows it: not on iPhone.
- **Light / dark theme** — Follows the system until you toggle it with the button or **Ctrl+K** (**⌘K** on macOS). The grid, the guides and the white screen keep their colors; the favicon follows the theme.
- **Auto-save** — Settings and position are saved to localStorage as you change them, and flushed when the tab is hidden or closed. Open tabs stay in sync. **R** resets the settings, but the image and the theme.
- **Keyboard shortcuts** — Hover, click or tap the bottom-right corner to see the list.
- **Accessible** — Toggle buttons with `aria-pressed`, native popovers for the settings, anchored to their button, and the shortcuts, both kept within the screen, labelled fields. Where `appearance: base-select` is not supported (Firefox), a pill shows a classic select with its value only, still labelled. Respects `prefers-reduced-motion`.
- **No build step** — One `index.html` and its `app-style.css`, vanilla JS, and custom elements.

## Images

The images live in `images/`. To add one, drop the file there and add it to the list in the `overlay-config` JSON, in the `<head>` of `index.html`:

```json
{ "name": "Lamentation, Andrea Solario, c. 1505-1507", "file": "lamentation-andrea-solario-1505-1507.jpg" }
```

The same JSON holds the defaults the settings reset to, among them `cardWidthPx`, the width of a bank card on the screen in CSS pixels: 324 is its width in CSS millimetres. Keep file names in lower case: a server may tell `Solario` from `solario`.

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
- One store, `window.store`, created by a classic script in `<head>` before the first paint. It holds the state (the options of the page, `theme`, `themeChosen`, and `localImage`, the image opened from the device, which it does not save), checks every value it is given, saves it to localStorage under `overlay-store` and sets `data-theme` and `data-white-screen` on `<html>`.
- Every custom element is a brick: it reads the store with `select()` and changes it through its actions (`setOption`, `toggle`, `step`, `openImage`, `setPan`, `movePan`, `toggleTheme`, `reset`); `setPan` moves the sheet or the image, as `moveImage` says, and does nothing while they are locked; `isHeld` tells the options the lock holds, which the fields disable. No brick knows another, so removing one leaves the others working.
- Custom elements: `overlay-viewport`, `overlay-sheet`, `overlay-field`, `overlay-toggle`, `overlay-step`, `overlay-image-opener`, `overlay-fullscreen-toggle`, `overlay-theme-toggle`, `overlay-reset`, `overlay-shortcuts`.
- The sheet is sized in CSS: a millimetre is the width of the card, as calibrated, divided by 85.6 mm (ISO/IEC 7810 ID-1); the store only sets the paper, the orientation and that width, on `<html>`. The calibration is a modal `<dialog>`, opened and closed by invoker commands (`command`, `commandfor`).
- Breaking change: the screen width (`screenWidthMm`) is replaced by the width of the card (`cardWidthPx`), as a width of the screen scaled the sheet only while the window took the whole screen in the orientation it was measured in. A screen width saved before is dropped: calibrate once with a card.
- The favicon is the `grid_on` icon of [Material Symbols](https://github.com/google/material-design-icons), by Google, under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0), grey on the background of the theme like the icons of the other apps.
- `app-style.css` is the base shared by the small apps (Tumblr, Messenger, SMS, Tweets, Typewriter, Library, Letterhead, Overlay), copied from [its gist](https://gist.github.com/19h47/27df983fac74beb8d3e0a30688654498) with the revision in its header: the toolbar (`.app-toolbar`), its controls (`.app-control`), the pills (`.app-pill`), whose customizable select (`appearance: base-select`) shows its name and opens from anywhere on the pill, the name too, and the fields (`.app-field`) come from it; `index.html` adds the sheet, the settings, the calibration and the shortcuts, and the bar that holds the pills and the toolbar, where the toolbar goes under the pills on a narrow screen. A golden-ratio scale (φ), colors with `light-dark()` and `color-mix()`, classes prefixed `app-`. To update it, copy a newer revision. No frameworks or build tools.
