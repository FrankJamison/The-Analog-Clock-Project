# The Analog Clock Project

A small, self-contained **HTML/CSS/JavaScript** project that renders **two animated analog clocks** using **inline SVG**:

- **Local Time** (your device’s current local time)
- **UTC Time** (Coordinated Universal Time)

Live preview: https://analogclock.fcjamison.com/

## Highlights

- Two clocks on one page: Local + UTC
- SVG face + hands; hands rotate via CSS transforms
- Smooth hour/minute hand motion; second hand updates every second
- Responsive layout
- Local time zone label in the footer (via `Intl.DateTimeFormat` with a safe fallback)

## Quick start (developers)

This is a static site (no build step).

1. Clone the repo.
2. Serve the folder with any static server.

### Option A: open the file directly

You can open `index.html` in a browser.

### Option B: run a local web server (recommended)

```bash
python -m http.server 8080
```

Then visit `http://localhost:8080/`.

#### Windows note

If `python` isn’t recognized:

```bash
py -m http.server 8080
```

### VS Code task

This workspace includes a task named **Open in Browser** that opens:

- `http://theanalogclockproject.localhost/`

If you don’t use that local domain, run the `http.server` command above (or adjust the task).

## How it works

Each hand is an SVG group (`<g>`) that rotates around the center of the clock.

Angles used:

- **Hour hand**: 30° per hour, plus a minute offset
- **Minute hand**: 6° per minute, plus a second offset
- **Second hand**: 6° per second

The JavaScript calculates these angles from either local time (`getHours()` / `getMinutes()` / `getSeconds()`) or UTC time (`getUTCHours()` / `getUTCMinutes()` / `getUTCSeconds()`), then applies:

```js
hand.style.transform = `rotate(${degrees}deg)`;
```

An interval updates both clocks once per second.

## Code tour

### Markup (SVG)

The page contains two inline SVG clocks in `index.html`:

- Clock 1 (local time): `#clock1` with hands `#hour1`, `#minute1`, `#second1`
- Clock 2 (UTC): `#clock2` with hands `#hour2`, `#minute2`, `#second2`

Each “hand” is a `<g>` group that contains a visible path (the arm) plus a transparent `.sizing-box` circle.
The `.sizing-box` keeps the group’s bounding box consistent so rotation behaves predictably.

### Script

`js/script.js`:

- Caches the six hand elements with `document.querySelector()`.
- Updates local and UTC clocks with `runClock1()` and `runClock2()`.
- Runs updates every second via `setInterval()`.
- Sets the footer label (`#localTzAbbr`) once on load.

Time zone label behavior:

- First attempt: `Intl.DateTimeFormat(..., { timeZoneName: "short" }).formatToParts()`
- Fallback: `Intl.DateTimeFormat().resolvedOptions().timeZone`
- Final fallback: the literal `"Local"`

### Styles

`css/style.css`:

- Uses CSS custom properties in `:root` for colors.
- Sets `transform-origin: 300px 300px` for all six hand groups.
- Uses transitions for hour/minute hands only (second hand updates discretely).

## Maintenance notes

- **If you change the SVG size or `viewBox`**: update the `transform-origin` values in CSS. They’re hard-coded to the center of the current `viewBox` (600×600 → center at 300,300).
- **If you rename SVG ids**: update the selectors at the top of `js/script.js`.
- **Performance**: the interval is 1000ms; if you want smoother seconds, you’ll need a different update strategy (and likely different CSS transitions).

## Tech

- HTML5 + SVG
- CSS
- Vanilla JavaScript

## Project structure

```
/
  index.html
  css/
    style.css
  js/
    script.js
```

## Run locally

See **Quick start (developers)** above.

## Publish on GitHub Pages

This project uses **relative** asset paths (for example `./css/style.css`), so it works on GitHub Pages without special configuration.

Suggested setup:

1. Push the repo to GitHub.
2. GitHub → **Settings → Pages**.
3. **Deploy from a branch** → pick your default branch → `/ (root)`.

## Troubleshooting

- **Clocks not moving**: open DevTools → Console and check for script errors.
- **Styling missing**: confirm `./css/style.css` is loading (Network tab) and paths are correct.
- **Time zone label differs**: abbreviations vary by OS/browser; the code falls back to the full time zone name.

## License

No `LICENSE` file is included in this repository.

If you plan to redistribute or reuse this code beyond personal use, add an explicit license file (for example MIT or Apache-2.0) and update this section.
