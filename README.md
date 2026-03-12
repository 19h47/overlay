## Overlay

Overlay is a tiny, framework‑free web app to help you trace or copy images on A3/A4 paper using a calibrated screen grid.

### Features

- **Paper frame**: fixed A3/A4 frame with a 1 cm × 1 cm grid.
- **Pan & drag**: move the image under the frame with mouse or touch.
- **Image list**: quick switch between bundled reference images.
- **Filters**: contrast, brightness, grayscale.
- **Guides**: center crosshair and toggleable grid/guides.
- **Screen calibration**: adjust screen width in mm for accurate scaling.
- **Persistence**: configuration (including position) is saved in `localStorage`.
- **Keyboard shortcuts**:
  - Arrows: move image.
  - G: toggle grid.
  - C: toggle center guides.
  - W: white screen.
  - F: fullscreen.
  - R: reset settings.

### Theme

Overlay supports a **dark/light theme toggle** (via the lil‑gui control panel).  
The theme is persisted across reloads using `localStorage`.

### Usage

1. Open `index.html` in a modern browser (Chrome, Firefox, Edge, Safari).
2. Use the lil‑gui panel (top‑right) to:
   - choose an image,
   - adjust filters,
   - toggle grid/guides,
   - switch theme (dark/light),
   - calibrate screen width,
   - go fullscreen or enable the white screen overlay.
3. Use the keyboard shortcuts (see bottom‑right panel) for quick actions.

### Development Notes

- The app is written in **vanilla JavaScript** and CSS.
- Core pieces are split into:
  - `OverlayConfig`: centralized configuration and helpers,
  - `AppStore`: small `localStorage` wrapper,
  - web components: `overlay-app`, `overlay-viewport`, `overlay-toast`, `overlay-controls`.
- State (including theme and pan position) is stored under the `overlay-store` key in `localStorage`.

