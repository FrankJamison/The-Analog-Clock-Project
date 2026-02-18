# The Analog Clock Project (Local + UTC)

A small, self-contained **HTML/CSS/JavaScript** project that renders **two animated analog clocks** using **inline SVG**:

- **Local Time** (your device’s current local time)
- **UTC Time** (Coordinated Universal Time)

Live preview: https://fcjamison.com/projects/2006AnalogClockProject/

## Highlights

- Two clocks on one page: Local + UTC
- SVG face + hands; hands rotate via CSS transforms
- Smooth hour/minute hand motion; second hand updates every second
- Responsive layout
- Local time zone label in the footer (via `Intl.DateTimeFormat` with a safe fallback)

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

## Tech

- HTML5 + SVG
- CSS
- Vanilla JavaScript

## Project structure

```
2006AnalogClockProject/
  index.html
  css/
    style.css
  js/
    script.js
```

## Run locally

- Fastest: open `index.html` in a browser.
- Recommended (avoids `file://` quirks):

```bash
python -m http.server 8080
```

Then visit `http://localhost:8080/`.

### Windows note

If `python` isn’t recognized, try:

```bash
py -m http.server 8080
```

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
