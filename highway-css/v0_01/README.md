# Highway CSS — v0.01

A small, dependency-free stylesheet with the look of the open road: asphalt,
faded yellow paint, green guide signs, route shields, mile markers, and the
steady amber glow of an odometer. It is plain CSS with no build step, so it
works in any front-end stack.

---

## Table of contents

1. [What is Highway CSS?](#what-is-highway-css)
2. [Quick start](#quick-start)
3. [Theming (custom properties)](#theming-custom-properties)
4. [Components (class reference)](#components-class-reference)
5. [How it works — a guided tour](#how-it-works--a-guided-tour)
6. [What changed in v0.01](#what-changed-in-v001)
7. [Roadmap for v0.02](#roadmap-for-v002)
8. [Design notes](#design-notes)

---

## What is Highway CSS?

Highway CSS is a *theme layer*. It does two things:

1. **Styles your semantic HTML automatically** — `<h1>`, `<a>`, `<button>`,
   `<input>`, `<table>`, `<details>`, `<progress>`, and friends all pick up the
   road aesthetic the moment you link the stylesheet. You do not have to add a
   class to get readable, on-theme text and controls.
2. **Provides small layout + "sign" components** via `hw-` classes — panels,
   route shields, mile markers, gauges, tags, notes, navigation, and a
   decorative "road" divider.

It is deliberately *not* a JavaScript library and *not* a CSS-in-JS system.
That is what makes it portable: link one file, override a few variables, done.

### What it looks like

- **Palette** — asphalt black, warm headlight white, amber accent, and the
  muted green/blue/yellow of real highway signage.
- **Typography** — condensed, bold, uppercase display type for headings and
  signs (like roadside lettering), a clean grotesque for body text, and a
  monospace face for odometer/readout numbers.
- **Shape** — slightly rounded corners (real signs are rounded rectangles),
  crisp 2px strokes, and a soft amber glow on readouts.

---

## Quick start

### Plain HTML

```html
<link rel="stylesheet" href="style.css">
```

Optionally load the bundled Google Fonts (or use any faces you prefer):

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Barlow:ital,wght@0,400;0,600;0,700;1,400&family=Barlow+Condensed:wght@600;700&family=IBM+Plex+Mono:wght@400;600&display=swap">
```

### React

```jsx
import "./style.css";

export default function App() {
  return (
    <main className="hw-shell">
      <header className="hw-top">
        <h1 className="hw-brand">Highway</h1>
        <span className="hw-dim">v0.01</span>
      </header>
      <button className="hw-solid">Depart</button>
    </main>
  );
}
```

### Next.js (App Router)

Import the stylesheet once, in the root layout, so it applies to the whole app:

```jsx
// app/layout.js
import "./highway-css/style.css";

export default function RootLayout({ children }) {
  return <html lang="en" data-theme="dark">{children}</html>;
}
```

> For Next.js you can also `import "./style.css"` from a `_app.js` (Pages
> Router) or `app/layout.js` (App Router). It is one global stylesheet; there
> is nothing to configure on the server.

### Vue / Svelte / Astro / etc.

```js
import "./style.css"; // anywhere you import global CSS
```

The point: **there is no framework API**. It is a stylesheet.

### Theme switching

| What you want                         | What you write                  |
| ------------------------------------- | ------------------------------- |
| Follow the OS light/dark setting      | `<html>` (no attribute)         |
| Force dark (the default look)         | `<html data-theme="dark">`      |
| Force light                           | `<html data-theme="light">`     |

---

## Theming (custom properties)

Everything is driven by custom properties declared on `:root`. Override them
in your own stylesheet (loaded *after* this one) to recolor the whole library.

### Core tokens (the ones you will actually change)

| Token             | Dark default | Light default | Purpose                                   |
| ----------------- | ------------ | ------------- | ----------------------------------------- |
| `--hw-bg`         | `#0d0f13`    | `#f2eee4`     | Page background                           |
| `--hw-fg`         | `#ece7dc`    | `#211d16`     | Text color                                |
| `--hw-accent`     | `#ffb52e`    | `#c86a00`     | Links, buttons, meters, focus             |
| `--hw-danger`     | `#ff5b4d`    | `#b2361f`     | Errors / faults                           |
| `--hw-ok`         | `#5fd98a`    | `#1d7a3d`     | Success / nominal                         |
| `--hw-warn`       | `#ffd23f`    | `#9a6a00`     | Caution                                   |
| `--hw-font`       | `Barlow`     | (same)        | Body typeface                             |
| `--hw-display`    | `Barlow Condensed` | (same)  | Headings and signs                        |
| `--hw-mono`       | `IBM Plex Mono` | (same)     | Odometer / readouts / code                |
| `--hw-radius`     | `6px`        | (same)        | Corner radius of cards and controls       |
| `--hw-border-w`   | `2px`        | (same)        | Stroke width                              |

### Sign + road tokens

These are **constant across themes** on purpose — a green guide sign is green
in daylight, too.

| Token                 | Value     | Used by                                        |
| --------------------- | --------- | ---------------------------------------------- |
| `--hw-sign`           | `#1f6f3d` | Guide-sign green (`.hw-sign`, `.hw-route`)     |
| `--hw-sign-info`      | `#2f6fa8` | Information-sign blue                          |
| `--hw-sign-warn`      | `#ffd23f` | Warning-sign yellow                            |
| `--hw-sign-warn-fg`   | `#1c1a16` | Dark text on the yellow sign                   |
| `--hw-sign-fg`        | `#ffffff` | Text on green/blue signs                       |
| `--hw-asphalt`        | `#22252c` | The road surface behind the center line        |
| `--hw-centerline`     | `#ffd23f` | The dashed yellow line in `.hw-road`           |

### Derived tokens

These are computed with `color-mix()` from `--hw-bg`/`--hw-fg` so they stay in
harmony automatically. Override them only if you need an exact value:

`--hw-bg-raised` (elevated surface), `--hw-fg-dim` (muted text), `--hw-line`
(borders), `--hw-track` (empty gauge track), `--hw-shadow` (soft shadow),
`--hw-accent-fg` (ink that reads on the accent color).

### Example: a "dusk" recolor

```css
:root {
  --hw-bg: #12100e;
  --hw-fg: #f5e9d6;
  --hw-accent: #ff8a3d;
}
```

Because every rule reads from the variables, that three-line override re-themes
buttons, links, meters, focus rings, and selection at once.

---

## Components (class reference)

### Layout

| Class        | Purpose                                                        |
| ------------ | -------------------------------------------------------------- |
| `.hw-shell`  | Page frame: centered, max-width, vertical gap                  |
| `.hw-grid-2` | Two columns on ≥800px, one column below                        |
| `.hw-stack`  | Vertical stack of children with even gap                       |
| `.hw-row`    | Horizontal, wrapping row of children                           |
| `.hw-actions`| Like `.hw-row`, intended for button groups                     |
| `.hw-table-wrap` | Horizontal-scroll wrapper for wide tables                 |

### Surfaces + signage

| Class                 | Purpose                                                        |
| --------------------- | -------------------------------------------------------------- |
| `.hw-top`             | Header bar styled as an overhead highway sign                  |
| `.hw-brand`           | Title inside `.hw-top`                                         |
| `.hw-panel`           | Standard raised "tarmac" card                                  |
| `.hw-sign`            | Guide sign (green); add `--info` or `--warn` modifiers         |
| `.hw-sign--info`      | Blue information sign                                          |
| `.hw-sign--warn`      | Yellow warning sign                                            |
| `.hw-route`           | Route shield badge; modifiers `--info`, `--warn`               |
| `.hw-mile`            | Small mono mile-marker chip for metadata                       |
| `.hw-road`            | Decorative road divider (use as `<hr class="hw-road">`)        |

### Content

| Class            | Purpose                                                     |
| ---------------- | ----------------------------------------------------------- |
| `.hw-dim`        | Muted text                                                  |
| `.hw-odometer`   | Mono, amber, glowing numeric readout                        |
| `.hw-tag`        | Sign-style badge; modifiers `--ok`, `--warn`, `--danger`    |
| `.hw-note`       | Callout with a colored left edge; modifiers `--ok`, `--warn`, `--danger` |
| `.hw-nav`        | Horizontal "route map" of links                             |
| `.hw-split`      | Label/value readout row (e.g. `Fuel ▏ 72`)                  |
| `.hw-meter`      | Segmented gauge; set `--hw-value: 72%` to fill it           |

### Forms

| Class         | Purpose                                                        |
| ------------- | -------------------------------------------------------------- |
| `.hw-field`    | Labeled input: label on top, styled like signage               |
| `.hw-check`    | Checkbox/radio next to its label on one line                   |
| `.hw-btn`      | Make any element look like a button (`<a class="hw-btn">`)     |
| `.hw-solid`    | Filled (accent) button variant                                 |

---

## How it works — a guided tour

This section is for readers who know programming fundamentals and want to
understand *why* the code is shaped the way it is.

### 1. Custom properties are the "API"

A CSS custom property is a variable. It is declared with `--name: value` and
read back with `var(--name)`. Highway CSS declares a palette on `:root` and
then **never hardcodes a color again** — every rule says `var(--hw-accent)`
instead of `#ffb52e`.

```css
:root { --hw-accent: #ffb52e; }
a { color: var(--hw-accent); }
```

The payoff is exactly the same as using constants in code: change the constant
in one place, and every consumer updates. That is how the three-line "dusk"
recolor above re-themes the whole library.

### 2. `color-mix()` derives a second palette for free

Hand-authoring "a slightly lighter version of the background" for every theme
is error-prone. Instead, the derived tokens *compute* the tint:

```css
--hw-bg-raised: color-mix(in srgb, var(--hw-fg) 7%, var(--hw-bg));
```

Read this as: *"blend 7% of the foreground color into the background color,
in the sRGB color space."* Because it is computed from the current theme's
values, it automatically adapts when the theme switches. `color-mix()` is
supported in all current evergreen browsers.

### 3. Dark and light are just two variable assignments

There is no separate "light stylesheet". The light theme is a handful of
overrides scoped by an attribute selector:

```css
:root[data-theme="light"] { --hw-bg: #f2eee4; /* ... */ }
```

That selector matches `<html data-theme="light">`. The media query below it
does the same thing but only when the OS *prefers* light and the user has not
explicitly pinned a theme with `data-theme="dark"`:

```css
@media (prefers-color-scheme: light) {
  :root:not([data-theme="dark"]) { /* same overrides */ }
}
```

`color-scheme: dark` / `light` tells the browser to render native widgets
(scrollbars, form controls) in a matching tone.

### 4. Layered backgrounds build the road divider

`.hw-road` draws four layers in one `background` shorthand, listed topmost
first:

1. a `repeating-linear-gradient` of yellow dashes (the center line),
2. two thin `linear-gradient`s for the white edge lines,
3. a solid `linear-gradient` of asphalt at the bottom.

Each layer gets its own `background-size` and `background-position`, so the
dashes sit at `center`, the edge lines at `top`/`bottom`, and the asphalt
fills the whole element. This is the same technique used to build the gauge's
segmented fill in `.hw-meter`.

### 5. Progressive enhancement, not lock-in

- Semantic tags are styled **globally**, so content is readable with zero
  classes. Classes only add layout and decoration.
- `:focus-visible` keeps a visible keyboard focus ring without painting one on
  every mouse click.
- `@media (prefers-reduced-motion: reduce)` is already wired so future
  animations will respect the user's motion preference.
- `prefers-color-scheme` and `data-theme` both work, so the library honors
  system settings *and* lets apps force a brand theme.

---

## What changed in v0.01

This is the **first alpha** — there is no prior version to diff against.
Everything is new:

- Full semantic element base: body, headings, links, lists, tables, code,
  blockquotes, `hr`, `kbd`, `details`/`summary`, and `fieldset`/`legend`.
- Form controls: buttons (`.hw-btn`, `.hw-solid`), text/textarea/select,
  checkboxes, radios, color, and range inputs, plus a native `progress` gauge.
- Layout primitives: `.hw-shell`, `.hw-grid-2`, `.hw-stack`, `.hw-row`,
  `.hw-actions`, `.hw-table-wrap`.
- Signage components: `.hw-top`, `.hw-panel`, `.hw-sign` (+ info/warn),
  `.hw-route` (+ info/warn), `.hw-mile`, `.hw-tag`, `.hw-note`, `.hw-nav`,
  `.hw-road`.
- Readouts: `.hw-odometer`, `.hw-split`, `.hw-meter` (with `--hw-value`).
- Theming: a 6-token core palette plus sign/road tokens, dark and light themes,
  `data-theme` overrides, and `prefers-color-scheme` support.
- Accessibility: keyboard focus rings, `aria`-friendly demo, reduced-motion
  guard.

---

## Roadmap for v0.02

Prioritized ideas for the next version. Each is intentionally scoped so it can
ship as its own small release.

1. **Component modifiers** — filled/outline tag variants, a "gantry" full-width
   top bar option, and left/right-aligned route shields.
2. **More readouts** — a speedometer-style radial gauge (CSS `conic-gradient`)
   and a "distance to next exit" ticker.
3. **Layout helpers** — a `.hw-grid-3`/`.hw-grid-auto` and a `.hw-center`
   utility, plus a `.hw-card--flush` modifier for edge-to-edge panels.
4. **Fine-grained motion** — a subtle headlight-flicker or odometer-roll
   animation, always gated behind `prefers-reduced-motion`.
5. **Utility classes** — spacing and text utilities (`.hw-text-center`,
   `.hw-mt-*`) for one-off layout needs.
6. **Distribution niceties** — a minified build, a `package.json` with an
   `"exports"` map so `import "highway-css/style.css"` resolves cleanly, and a
   demo that doubles as a visual regression page.

---

## Design notes

- **"Sign" colors are theme-constant** so signage stays recognizable in both
  modes, while the *page* palette flips. This keeps the road metaphor legible.
- **The road divider is decorative**, not a real `<hr>` separator for
  semantics — use a plain `<hr>` for meaningful section breaks and reserve
  `.hw-road` for flavor.
- **No images, no icon fonts** — everything (route shields, road dashes, gauge
  ticks) is drawn with CSS gradients and borders so the library stays a single
  file and scales crisply.
