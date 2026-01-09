# 2006 Analog Clock Project

A small, self-contained HTML/CSS/JavaScript project that renders **two animated analog clocks** using SVG:

- **Local Time** (your device’s current local time)
- **UTC Time** (Coordinated Universal Time)

## Structure

```
2006AnalogClockProject/
  index.html
  css/
    style.css
  js/
    script.js
```

## Run locally

- Easiest: open `index.html` in a browser.
- Or run a simple local server:

```bash
python -m http.server 8080
```

Then visit `http://localhost:8080/`.

## Publish on GitHub Pages

This project uses **relative** asset paths (e.g. `./css/style.css`), so it works on GitHub Pages without special configuration.
