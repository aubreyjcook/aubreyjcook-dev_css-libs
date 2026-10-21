# Assembler CSS — v0.01

A very lightweight, dependency-free stylesheet with a hard-edged terminal
look: pixel corners, amber phosphor, scanlines, chunky hard shadows, and
configurable color. It is plain CSS with no build step, so it works in any
front-end stack.

---

## Table of contents

1. [What is Assembler CSS?](#what-is-assembler-css)
2. [Quick start](#quick-start)
3. [Theming (custom properties)](#theming-custom-properties)
4. [Components (class reference)](#components-class-reference)
5. [How it works — a guided tour](#how-it-works--a-guided-tour)
6. [What changed in v0.01](#what-changed-in-v001)
7. [Roadmap for v0.02](#roadmap-for-v002)
8. [Design notes](#design-notes)

---

## What is Assembler CSS?

Assembler CSS is a *theme layer*. It does two things:

1. **Styles your semantic HTML automatically** — `<h1>`, `<a>`, `<button>`,
   `<input>`, `<table>`, `<details>`, `<progress>`, and friends all pick up the
   terminal chrome the moment you link the stylesheet. You do not have to add a
   class to get readable, on-theme text and controls.
2. **Provides small layout + "terminal" components** via `asm-` classes —
   panels with corner brackets, meters, tags, notes, navigation, and a
   blinking cursor.

It is deliberately *not* a JavaScript library and *not* a CSS-in-JS system.
That is what makes it portable: link one file, override a few variables, done.

### What it looks like

- **Palette** — near-black background, warm amber phosphor text, amber accent,
  and the green/yellow/red of status lamps.
- **Typography** — monospace everywhere, with a pixel display face for
  headings and keys (falls back to the system mono if you skip the webfonts).
- **Shape** — zero corner radius, 2px strokes, and hard step-shadows (no blur)
  like the UI in a Fallout-style terminal.

---

## Quick start

### Plain HTML

```html
<link rel="stylesheet" href="style.css">
```

Optionally load the bundled Google Fonts (or use any faces you prefer):

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Silkscreen&display=swap">
```

### React

```jsx
import "./style.css";

export default function App() {
  return (
    <main className="asm-shell">
      <header className="asm-top">
        <h1 className="asm-brand">Assembler</h1>
        <span className="asm-dim">v0.01</span>
      </header>
      <button className="asm-solid">Confirm</button>
    </main>
  );
}
```

### Next.js (App Router)

Import the stylesheet once, in the root layout, so it applies to the whole app:

```jsx
// app/layout.js
import "./assembler-css/style.css";

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

| Token               | Dark default | Light default | Purpose                                   |
| ------------------- | ------------ | ------------- | ----------------------------------------- |
| `--asm-bg`          | `#110e09`    | `#f3eee4`     | Page background                           |
| `--asm-fg`          | `#f3e6c5`    | `#1b140c`     | Text color                                |
| `--asm-accent`      | `#ffb347`    | `#6b2e00`     | Frames, keys, links, meters, focus        |
| `--asm-danger`      | `#ff5c33`    | `#9d1c12`     | Fault                                     |
| `--asm-ok`          | `#c6f25c`    | `#2a6412`     | Nominal                                   |
| `--asm-warn`        | `#ffe14a`    | `#8a5600`     | Caution                                   |
| `--asm-font`        | `Share Tech Mono` | (same)   | Body typeface (mono)                      |
| `--asm-display`     | `Silkscreen` | (same)        | Headings and keys (pixel face)            |
| `--asm-px`          | `4px`        | (same)        | Hard-shadow step size                     |
| `--asm-border-w`    | `2px`        | (same)        | Stroke width                              |
| `--asm-scan-image`  | (scanlines)  | (same)        | Set to `none` to drop the scanline overlay|

### Derived tokens

These are computed with `color-mix()` from `--asm-bg`/`--asm-fg` so they stay in
harmony automatically. Override them only if you need an exact value:

`--asm-bg-raised` (elevated surface), `--asm-fg-dim` (muted text), `--asm-line`
(borders), `--asm-shadow` (hard shadow), `--asm-accent-fg` (ink that reads on
the accent), `--asm-track` (empty meter track).

### Example: a "phosphor green" recolor

```css
:root {
  --asm-bg: #05130a;
  --asm-fg: #c9ffd6;
  --asm-accent: #38ff6b;
}
```

Because every rule reads from the variables, that three-line override re-themes
buttons, links, meters, focus rings, and selection at once.

---

## Components (class reference)

### Layout

| Class        | Purpose                                                        |
| ------------ | -------------------------------------------------------------- |
| `.asm-shell` | Page frame: centered, max-width, vertical gap                  |
| `.asm-grid-2`| Two columns on ≥800px, one column below                        |
| `.asm-stack` | Vertical stack of children with even gap                       |
| `.asm-row`   | Horizontal, wrapping row of children                           |
| `.asm-actions`| Like `.asm-row`, intended for button groups                    |
| `.asm-table-wrap` | Horizontal-scroll wrapper for wide tables                 |

### Surfaces + chrome

| Class                 | Purpose                                                        |
| --------------------- | -------------------------------------------------------------- |
| `.asm-top`            | Header bar with a hard accent shadow                           |
| `.asm-brand`          | Title inside `.asm-top`                                        |
| `.asm-panel`          | Panel with corner brackets drawn in CSS                        |
| `.asm-caret`          | Blinking terminal cursor (append to a `<span>`)                |
| `.asm-dim`            | Muted text                                                     |

### Content

| Class            | Purpose                                                     |
| ---------------- | ----------------------------------------------------------- |
| `.asm-meter`     | Segmented gauge; set `--asm-value: 72%` to fill it          |
| `.asm-tag`       | Small framed badge; modifiers `--ok`, `--warn`, `--danger`  |
| `.asm-note`      | Inline callout; modifiers `--ok`, `--warn`, `--danger`      |
| `.asm-nav`       | Horizontal framed row of links                              |
| `.asm-split`     | Label/value readout row (e.g. `Integrity ▏ 72`)             |

### Forms

| Class         | Purpose                                                        |
| ------------- | -------------------------------------------------------------- |
| `.asm-field`   | Labeled input: label on top, styled like a terminal readout    |
| `.asm-check`   | Checkbox/radio next to its label on one line                   |
| `.asm-btn`     | Make any element look like a button (`<a class="asm-btn">`)    |
| `.asm-solid`   | Filled (accent) button variant                                 |

---

## How it works — a guided tour

This section is for readers who know programming fundamentals and want to
understand *why* the code is shaped the way it is.

### 1. Custom properties are the "API"

A CSS custom property is a variable. It is declared with `--name: value` and
read back with `var(--name)`. Assembler CSS declares a palette on `:root` and
then **never hardcodes a color again** — every rule says `var(--asm-accent)`
instead of `#ffb347`.

```css
:root { --asm-accent: #ffb347; }
a { color: var(--asm-accent); }
```

The payoff is exactly the same as using constants in code: change the constant
in one place, and every consumer updates. That is how the three-line "phosphor
green" recolor above re-themes the whole library.

### 2. `color-mix()` derives a second palette for free

Hand-authoring "a slightly lighter version of the background" for every theme
is error-prone. Instead, the derived tokens *compute* the tint:

```css
--asm-bg-raised: color-mix(in srgb, var(--asm-fg) 8%, var(--asm-bg));
```

Read this as: *"blend 8% of the foreground color into the background color,
in the sRGB color space."* Because it is computed from the current theme's
values, it automatically adapts when the theme switches. `color-mix()` is
supported in all current evergreen browsers.

### 3. Dark and light are just two variable assignments

There is no separate "light stylesheet". The light theme is a handful of
overrides scoped by an attribute selector:

```css
:root[data-theme="light"] { --asm-bg: #f3eee4; /* ... */ }
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

### 4. The "hard-edged" look comes from zero blur

A *hard* shadow is just a normal `box-shadow` with **no blur radius**, so the
"shadow" is a solid offset block instead of a soft haze:

```css
box-shadow: var(--asm-px) var(--asm-px) 0 0 var(--asm-accent);
```

The four values are `offset-x`, `offset-y`, `blur`, `spread`. With `blur: 0`
and `spread: 0`, the accent is drawn as a crisp rectangle peeking out from the
corner. `--asm-px` (4px) is the *step size* — how far the block is offset — so
the "chunkiness" of the whole UI is controlled by one token.

### 5. Scanlines and pixels are CSS gradients

The scanline overlay is a single `repeating-linear-gradient` stored in a
variable and applied to the body:

```css
--asm-scan-image: repeating-linear-gradient(
  to bottom,
  transparent 0 2px,
  color-mix(in srgb, var(--asm-fg) 7%, transparent) 2px 3px
);
```

Read this as: *"every 3px, paint 1px of a faint foreground tint, then leave
2px transparent."* Setting `--asm-scan-image: none` removes the effect in one
line. Pixelated images are handled natively with
`img, canvas { image-rendering: pixelated; }`, which tells the browser to keep
hard edges when scaling.

### 6. The panel corner brackets are layered gradients

`.asm-panel` draws four L-shaped corner brackets using eight `linear-gradient`s
(the accent color, drawn as thin horizontal/vertical strips). Each strip has
its own `background-size` and `background-position` so the brackets sit in the
four corners while the panel itself stays translucent. This is the same "stack
several backgrounds in one declaration" technique used by the segmented
`.asm-meter` fill.

### 7. The caret blinks with `steps()`, and respects reduced motion

`.asm-caret::after` renders a literal `_` and animates its opacity:

```css
animation: asm-blink 1s steps(1) infinite;
```

`steps(1)` means the animation jumps between frames instead of fading — a
digital blink, not an analog one. A `prefers-reduced-motion: reduce` block
then turns the animation off entirely for users who request it.

### 8. Progressive enhancement, not lock-in

- Semantic tags are styled **globally**, so content is readable with zero
  classes. Classes only add layout and decoration.
- `:focus-visible` keeps a visible keyboard focus ring without painting one on
  every mouse click.
- `@media (prefers-reduced-motion: reduce)` disables the caret blink.
- `prefers-color-scheme` and `data-theme` both work, so the library honors
  system settings *and* lets apps force a brand theme.

---

## What changed in v0.01

This is the **first alpha** — there is no prior version to diff against.
Everything is new:

- Full semantic element base: body, headings, links, lists, tables, code,
  blockquotes, `hr`, `kbd`, `details`/`summary`, and `fieldset`/`legend`.
- Form controls: buttons (`.asm-btn`, `.asm-solid`), text/textarea/select,
  checkboxes, radios, color, and range inputs, plus a native `progress` gauge.
- Layout primitives: `.asm-shell`, `.asm-grid-2`, `.asm-stack`, `.asm-row`,
  `.asm-actions`, `.asm-table-wrap`.
- Terminal components: `.asm-top`, `.asm-panel`, `.asm-caret`, `.asm-tag`,
  `.asm-note`, `.asm-nav`.
- Readouts: `.asm-split`, `.asm-meter` (with `--asm-value`).
- Theming: a 6-token core palette plus hard-shadow/scanline tokens, dark and
  light themes, `data-theme` overrides, and `prefers-color-scheme` support.
- Accessibility: keyboard focus rings, `aria`-friendly demo, reduced-motion
  handling for the caret blink.

---

## Roadmap for v0.02

Prioritized ideas for the next version. Each is intentionally scoped so it can
ship as its own small release.

1. **Component modifiers** — filled/outline tag and note variants, a
   "danger-flash" panel state, and a full-width terminal "status bar" footer.
2. **More terminal furniture** — a tab-strip component, a blinking "standby"
   indicator, and a log/scrollback container with a fixed-height auto-scroll
   area.
3. **Layout helpers** — `.asm-grid-3`/`.asm-grid-auto`, a `.asm-center`
   utility, and a `.asm-panel--flush` modifier for edge-to-edge panels.
4. **Fine-grained motion** — a cursor-block blink and a meter "charge" shimmer,
   all gated behind `prefers-reduced-motion`.
5. **Utility classes** — spacing and text utilities (`.asm-text-center`,
   `.asm-mt-*`) for one-off layout needs.
6. **Distribution niceties** — a minified build, a `package.json` with an
   `"exports"` map so `import "assembler-css/style.css"` resolves cleanly, and
   a demo that doubles as a visual regression page.

---

## Design notes

- **Zero radius is the point** — the global reset `*, *::before, *::after
  { border-radius: 0; }` enforces the hard-edged look everywhere, and every
  component is built to look good squared off.
- **Hard shadows, not blur** — depth comes from solid offset blocks, keeping
  the "pixel" aesthetic consistent at any DPI.
- **The scanline overlay is opt-out** — it is on by default for the phosphor
  vibe, but one variable (`--asm-scan-image: none`) removes it, and the demo's
  "Scanlines" checkbox toggles it live.
- **No images, no icon fonts** — panel brackets, meter ticks, and scanlines are
  all drawn with CSS gradients and borders, so the library stays a single file
  and scales crisply.
