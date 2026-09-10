# Aera — Colour reference

All colours used across the site, split by light / dark theme.
Source of truth is the CSS in each HTML file — this doc mirrors it.

## How theming works

- **`index.html`** — colours are CSS custom properties on `:root`. Light is the default set.
  - System dark: `@media (prefers-color-scheme: dark)` applied to `:root:not([data-theme="light"])`.
  - Manual toggle: the theme button sets `data-theme="light"` or `data-theme="dark"` on `<html>`
    (persisted in `localStorage` under `aera-theme`), which wins over the system setting.
- **`<meta name="theme-color">`** is kept in sync with the active theme by `syncThemeColor()` in JS
  (on load, on toggle, and on system-scheme change).
- **`privacy.html` / `support.html`** — standalone pages with their own hard-coded light/dark
  colours (not the token system), switched purely by `@media (prefers-color-scheme: dark)`.

---

## Landing page tokens — `index.html`

| Token | Light | Dark | Used for |
|---|---|---|---|
| `--bg` | `#F7F3EB` | `#000000` | page background (`<html>` + `<body>`) |
| `--text` | `#1A1712` | `#FFFFFF` | primary text, wordmark body/rim (`currentColor`), active nav dot, top-bar icons |
| `--text-dim` | `rgba(26, 23, 18, 0.62)` | `rgba(255, 255, 255, 0.62)` | subtitle, list text, scroll hint, `›` arrow |
| `--card` | `rgba(253, 250, 243, 0.72)` | `rgba(28, 28, 30, 0.68)` | glass feature cards, icon buttons, TestFlight pill, nav dots (inactive) |
| `--card-border` | `rgba(26, 23, 18, 0.10)` | `rgba(255, 255, 255, 0.12)` | 1px border on all of the above |
| `--glow` | `rgba(0, 0, 0, 0.26)` | `rgba(255, 255, 255, 0.30)` | inner wordmark glow — `drop-shadow(0 0 16px …)` |
| `--glow-soft` | `rgba(0, 0, 0, 0.11)` | `rgba(255, 255, 255, 0.13)` | outer wordmark glow — `drop-shadow(0 0 44px …)` |
| `--shadow` | `rgba(26, 23, 18, 0.14)` | `rgba(0, 0, 0, 0.45)` | box-shadows on cards / buttons / pill |
| `--eq-core` | `42, 37, 29` (`#2A251D`) | `255, 255, 255` (`#FFFFFF`) | equalizer bar colour — an **RGB triplet**, fed into `rgba()` in the canvas JS |
| `--eq-alpha` | `0.5` | `0.6` | equalizer bar base opacity |

Also on `:root`: `color-scheme: light dark` (native form controls, scrollbars).

### Equalizer canvas

The bottom `<canvas>` builds its colour at runtime from the two tokens above. Each bar is a
vertical gradient:

```
rgba(var(--eq-core), var(--eq-alpha))        →  bottom (solid)
rgba(var(--eq-core), var(--eq-alpha) * 0.32) →  55% up
rgba(var(--eq-core), 0)                       →  top (transparent)
```

then blurred `18px`. Intensity (bar height/opacity feel) ramps `0.4 → ~1.2` as you scroll the
slides, but the colour never changes.

---

## `<meta name="theme-color">`

| Light | Dark |
|---|---|
| `#F7F3EB` | `#000000` |

(Matches `--bg`. Controls the tint of the iOS Safari toolbar area.)

---

## Theme-independent colours — `index.html`

These stay the same in both themes.

| Colour | Where |
|---|---|
| `#0A84FF` → `#6E5CE6` | `.slide-cta` "Try the beta" pill — `linear-gradient(135deg, #0A84FF, #6E5CE6)` (iOS blue → violet) |
| `#FFFFFF` | label text on that pill; the TestFlight paper-plane SVG icon |
| `#FFFFFF` at varying opacity | liquid-glass wordmark highlight layers, all `mix-blend-mode: screen`: `#lg-hi` top highlight (`0.95 → 0.16 → 0`), `#lg-lo` bottom edge (`0 → 0.5`), `#lg-sheen` travelling glint (`0 → 1 → 0`) |

The wordmark's **body fill** (`#lg-body`, `currentColor` at `0.14 / 0.52 / 0.22` opacity) and
**rim stroke** (`currentColor` at `0.4`) use `--text`, so they *do* follow the theme.

---

## Privacy & Support pages — `privacy.html`, `support.html`

Standalone, hard-coded (no CSS variables).

| Role | Light | Dark |
|---|---|---|
| text | `#1A1A1A` | `#E6E6E6` |
| background | `#FFFFFF` | `#101012` |
| link | `#0066CC` | `#6AA9FF` |
| back-button background | `rgba(127, 127, 127, 0.14)` | same |
| back-button border | `rgba(127, 127, 127, 0.25)` | same |
| muted text (date, footer) | `#888888` | same |
| back-button shadow | `rgba(0, 0, 0, 0.10)` | same |
| footer divider | `rgba(127, 127, 127, 0.25)` | same |
