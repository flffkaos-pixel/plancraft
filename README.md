# PlanCraft

[한국어](README.ko.md) · English

Free, no-signup **2D → 3D interior floor plan designer** that runs entirely in your browser.
Draw a plan, place furniture, demolish a wall, measure a room — then switch to 3D and walk through it.

- **No account, no install.** Everything runs client-side; your plan stays in `localStorage`.
- **Interface in 한국어 / English / 日本語.** The language follows your browser on first visit and can be switched in the header (the choice is remembered).
- **MIT licensed.** This project is a rebranded, extended fork of [wy51ai/floorplan-3d](https://github.com/wy51ai/floorplan-3d).

## Project structure

```
plancraft/
├─ index.html          # Landing page (ko / en / ja)
├─ app/
│  └─ index.html       # The planner itself (single file, no build step)
├─ assets/
│  ├─ i18n-app.js      # ko / ja dictionary consumed by the app
│  ├─ favicon.svg
│  └─ og.png
├─ LICENSE             # MIT (original copyright retained)
└─ README.md
```

## Features

**2D plan**

- Displays the original plan at 1:60 / 1:100, all dimensions in mm
- Drag 60+ furniture and appliance items from the library (bedroom, living, dining & kitchen, bath, appliances, study)
- Move, rotate (`Shift` for free angle), resize, with automatic wall snapping
- Measurement tool that snaps to walls; `Shift` locks horizontal / vertical
- Demolish non-bearing walls — bearing walls are marked and protected
- Layer toggles: dimensions, room labels, furniture, grid, bearing walls

**3D scene**

- Orbit, iso and top views; click a room in the list to fly to it
- First-person walk mode: `WASD` + mouse on desktop, virtual joystick on touch, tap doors to open them
- Full-height / cut-away walls, daylight slider, night lighting
- Detailed furniture models with real materials
- Select and drag furniture in 3D — kept in sync with the 2D plan

**Plan & estimates**

- Automatic room areas and net floor area
- Per-room flooring materials (wood, tile, marble, terrazzo, carpet) with a cost estimate including 5% waste
- Undo / redo, automatic local save
- Export a PNG image, export / import the plan as JSON

## Quick start

```bash
git clone https://github.com/<you>/plancraft.git
cd plancraft
python3 -m http.server 8000
```

Then open:

- `http://localhost:8000/` — the landing page
- `http://localhost:8000/app/` — the planner

> The planner uses ES modules and an import map, so it must be served over HTTP — opening `app/index.html` straight from the file system will not work.
> Three.js is loaded from the jsDelivr CDN, so opening the 3D scene for the first time requires a network connection.

## Shortcuts

| Key | Action |
| --- | --- |
| `T` | Toggle 2D / 3D |
| `V` / `M` / `X` | Select / measure / demolish walls |
| `R` / `Shift+R` | Rotate the selection 90° clockwise / counter-clockwise |
| `Delete` / `Backspace` | Delete the selection |
| `Ctrl/⌘ + D` | Duplicate the selection |
| `Ctrl/⌘ + Z`, `Ctrl/⌘ + Shift + Z` | Undo, redo |
| `F` | Fit to window |
| `+` / `-` | Zoom in / out |
| `[` / `]` | Show / hide the library and the properties panel |
| `Shift + F` | Fullscreen |
| `Esc` | Cancel the current action |
| Walk mode: `WASD` / arrows, `Shift`, `E` | Move, run, open doors |

## Customizing the floor plan

All plan data lives in `app/index.html`:

- `ROOMS` — room polygons, names, default flooring
- `WALLS` / `WINS` — walls and window openings
- `MATS` — flooring materials and unit prices
- `LIB` — the furniture library (type, name, default size, colour)
- `buildFurniture()` — the 3D model for each furniture type

Edit that data to swap in your own plan.

### Localization

- `app/index.html` keeps the original Chinese strings as **internal keys**; `tr(zh, en)` looks them up in `assets/i18n-app.js` and falls back to English when a translation is missing.
- Static markup is translated through `data-en` / `data-en-title` attributes.
- Built-in room and furniture names go through `nm()`, which resolves dict → `NAMES_EN` → raw string.
- The landing page has its own dictionary inline in `index.html`, plus `navigator.language` detection.

## Deploying

The site is static — any static host works. On Vercel: import the repository, keep the default framework preset (**Other**), and deploy. No build command, no output directory.

## Credits

- Based on [wy51ai/floorplan-3d](https://github.com/wy51ai/floorplan-3d) — MIT licensed.
- [Three.js](https://threejs.org/) r160 (OrbitControls, PointerLockControls, RoundedBoxGeometry, RoomEnvironment, CSS2DRenderer)
- Fonts: Bricolage Grotesque, IBM Plex Mono, Noto Sans JP, Pretendard

## License

[MIT](LICENSE)
