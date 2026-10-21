# Nightingale CSS — v0.01

A small, dependency-free stylesheet with the feel of a late, comfortable
night: deep indigo, soft lavender, warm candlelight, rounded corners, gentle
shadows, and a sense of dreaminess. It is plain CSS with no build step, so it
works in any front-end stack.

---

## Table of contents

1. [What is Nightingale CSS?](#what-is-nightingale-css)
2. [Quick start](#quick-start)
3. [Theming (custom properties)](#theming-custom-properties)
4. [Components (class reference)](#components-class-reference)
5. [How it works — a guided tour](#how-it-works--a-guided-tour)
6. [What changed in v0.01](#what-changed-in-v001)
7. [Roadmap for v0.02](#roadmap-for-v002)
8. [Design notes](#design-notes)

---

## What is Nightingale CSS?

Nightingale CSS is a *theme layer*. It does two things:

1. **Styles your semantic HTML automatically** — `<h1>`, `<a>`, `<button>`,
   `<input>`, `<table>`, `<details>`, `<progress>`, and friends all pick up the
   night look the moment you link the stylesheet. You do not have to add a
   class to get readable, on-theme text and controls.
2. **Provides small layout + "dream" components** via `ng-` classes — soft
   cards, glowing meters, pastel tags, gentle notes, navigation, a glowing
   moon, and a handwritten flourish.

It is deliberately *not* a JavaScript library and *not* a CSS-in-JS system.
That is what makes it portable: link one file, override a few variables, done.

### What it looks like

- **Palette** — deep indigo, lavender-white, lilac accent, and a warm
  candlelight/moon glow.
- **Typography** — an elegant serif for headings, a comfortable humanist sans
  for body text, and a handwritten script for dreamy flourishes.
- **Shape** — large rounded corners, thin 1px strokes, soft blurred shadows,
  and a gentle lilac halo on focus and readouts.

---

## Quick start

### Plain HTML

```html
<link rel="stylesheet" href="style.css">
```

Optionally load the bundled Google Fonts (or use any faces you prefer):

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,500;0,600;1,500&family=Karla:ital,wght@0,400;0,600;1,400&family=Caveat:wght@500;600&display=swap">
```

### React

```jsx
import "./style.css";

export default function App() {
  return (
    <main className="ng-shell">
      <header className="ng-top">
        <h1 className="ng-brand">Nightingale</h1>
        <span className="ng-dream">a late-night lullaby</span>
      </header>
      <button className="ng-solid">Rest</button>
    </main>
  );
}
```

### Next.js (App Router)

Import the stylesheet once, in the root layout, so it applies to the whole app:

```jsx
// app/layout.js
import "./nightingale-css/style.css";

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
| Force light ("dawn")                  | `<html data-theme="light">`     |

---

## Theming (custom properties)

Everything is driven by custom properties declared on `:root`. Override them
in your own stylesheet (loaded *after* this one) to recolor the whole library.

### Core tokens (the ones you will actually change)

| Token             | Dark default | Light default | Purpose                                   |
| ----------------- | ------------ | ------------- | ----------------------------------------- |
| `--ng-bg`         | `#191330`    | `#f6f1ea`     | Page background                           |
| `--ng-fg`         | `#ece6f7`    | `#2c2540`     | Text color                                |
| `--ng-accent`     | `#c3a3f2`    | `#7b5fb8`     | Links, buttons, meters, focus             |
| `--ng-warm`       | `#f0d9a6`    | `#b98a2f`     | Candlelight / moon glow                   |
| `--ng-danger`     | `#ff8f8f`    | `#c04b5e`     | Errors / faults                           |
| `--ng-ok`         | `#9fe0b4`    | `#3d8a5c`     | Success / calm                            |
| `--ng-warn`       | `#f2cf8a`    | `#a8781f`     | Caution                                   |
| `--ng-font`       | `Karla`      | (same)        | Body typeface                             |
| `--ng-display`    | `Cormorant Garamond` | (same) | Headings                          |
| `--ng-script`     | `Caveat`     | (same)        | Handwritten flourish                      |
| `--ng-radius`     | `14px`       | (same)        | Corner radius of cards and controls       |
| `--ng-border-w`   | `1px`        | (same)        | Stroke width                              |

### Derived tokens

These are computed with `color-mix()` from `--ng-bg`/`--ng-fg` so they stay in
harmony automatically. Override them only if you need an exact value:

`--ng-bg-raised` (elevated surface), `--ng-fg-dim` (muted text), `--ng-line`
(borders), `--ng-track` (empty gauge track), `--ng-shadow` (soft shadow),
`--ng-glow` (lilac halo), `--ng-accent-fg` (ink that reads on the accent).

### Example: a "rose quartz" recolor

```css
:root {
  --ng-bg: #1d1220;
  --ng-fg: #f4e9ee;
  --ng-accent: #e8a9c4;
  --ng-warm: #f2cf9a;
}
```

Because every rule reads from the variables, that four-line override re-themes
buttons, links, meters, focus rings, and selection at once.

---

## Components (class reference)

### Layout

| Class        | Purpose                                                        |
| ------------ | -------------------------------------------------------------- |
| `.ng-shell`  | Page frame: centered, max-width, vertical gap                  |
| `.ng-grid-2` | Two columns on ≥800px, one column below                        |
| `.ng-stack`  | Vertical stack of children with even gap                       |
| `.ng-row`    | Horizontal, wrapping row of children                           |
| `.ng-actions`| Like `.ng-row`, intended for button groups                     |
| `.ng-table-wrap` | Horizontal-scroll wrapper for wide tables                 |

### Surfaces + mood

| Class                 | Purpose                                                        |
| --------------------- | -------------------------------------------------------------- |
| `.ng-top`             | Soft header card                                               |
| `.ng-brand`           | Title inside `.ng-top`                                         |
| `.ng-card`            | Standard soft, floating card                                   |
| `.ng-moon`            | Decorative glowing moon                                        |
| `.ng-dream`           | Handwritten script flourish (slogans, asides)                  |

### Content

| Class            | Purpose                                                     |
| ---------------- | ----------------------------------------------------------- |
| `.ng-dim`        | Muted text                                                  |
| `.ng-tag`        | Pastel pill badge; modifiers `--ok`, `--warn`, `--danger`   |
| `.ng-note`       | Soft callout with a colored left edge; modifiers `--ok`, `--warn`, `--danger` |
| `.ng-nav`        | Soft pill navigation                                        |
| `.ng-split`      | Label/value readout row (e.g. `Moon phase ▏ 72`)            |
| `.ng-meter`      | Glowing bar; set `--ng-value: 72%` to fill it               |

### Forms

| Class         | Purpose                                                        |
| ------------- | -------------------------------------------------------------- |
| `.ng-field`    | Labeled input: label on top, quiet and comfortable             |
| `.ng-check`    | Checkbox/radio next to its label on one line                   |
| `.ng-btn`      | Make any element look like a button (`<a class="ng-btn">`)     |
| `.ng-solid`    | Filled (accent) button variant                                 |

---

## How it works — a guided tour

This section is for readers who know programming fundamentals and want to
understand *why* the code is shaped the way it is.

### 1. Custom properties are the "API"

A CSS custom property is a variable. It is declared with `--name: value` and
read back with `var(--name)`. Nightingale CSS declares a palette on `:root`
and then **never hardcodes a color again** — every rule says
`var(--ng-accent)` instead of `#c3a3f2`.

```css
:root { --ng-accent: #c3a3f2; }
a { color: var(--ng-accent); }
```

The payoff is exactly the same as using constants in code: change the constant
in one place, and every consumer updates. That is how the four-line "rose
quartz" recolor above re-themes the whole library.

### 2. `color-mix()` derives a second palette for free

Hand-authoring "a slightly lighter version of the background" for every theme
is error-prone. Instead, the derived tokens *compute* the tint:

```css
--ng-bg-raised: color-mix(in srgb, var(--ng-fg) 8%, var(--ng-bg));
```

Read this as: *"blend 8% of the foreground color into the background color,
in the sRGB color space."* Because it is computed from the current theme's
values, it automatically adapts when the theme switches. `color-mix()` is
supported in all current evergreen browsers.

### 3. Dark and light are just two variable assignments

There is no separate "light stylesheet". The light ("dawn") theme is a handful
of overrides scoped by an attribute selector:

```css
:root[data-theme="light"] { --ng-bg: #f6f1ea; /* ... */ }
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

### 4. Layered radial gradients paint the "moonlit" vignette

The body background is not a flat color. Two large `radial-gradient()`s — one
lilac glow in the upper-right, one candlelight glow in the lower-left — are
stacked so the page fades gently toward the edges. Each gradient fades to
`transparent` so the base color shows through in the middle.

```css
background-image:
  radial-gradient(..., color-mix(in srgb, var(--ng-accent) 16%, transparent), transparent 62%),
  radial-gradient(..., color-mix(in srgb, var(--ng-warm) 12%, transparent), transparent 62%);
```

The same "stack several backgrounds in one declaration" idea powers the
glowing `.ng-moon` (a circle plus two expanding halo shadows).

### 5. Softness is a system, not an accident

- **Radius** — a single `--ng-radius` token (14px) is reused on cards, inputs,
  notes, and details so every corner shares the same gentle curve.
- **Shadows** — a single `--ng-shadow` token (a translucent black) is reused
  everywhere, keeping depth consistent.
- **Focus** — instead of a hard outline, inputs get a 4px `color-mix()` halo in
  the accent color, which reads as "glow" rather than "selection box".

### 6. Progressive enhancement, not lock-in

- Semantic tags are styled **globally**, so content is readable with zero
  classes. Classes only add layout and decoration.
- `:focus-visible` keeps a visible keyboard focus ring without painting one on
  every mouse click.
- `@media (prefers-reduced-motion: reduce)` is already wired so any hover or
  glow animation respects the user's motion preference.
- `prefers-color-scheme` and `data-theme` both work, so the library honors
  system settings *and* lets apps force a brand theme.

---

## What changed in v0.01

This is the **first alpha** — there is no prior version to diff against.
Everything is new:

- Full semantic element base: body, headings, links, lists, tables, code,
  blockquotes, `hr`, `kbd`, `details`/`summary`, and `fieldset`/`legend`.
- Form controls: buttons (`.ng-btn`, `.ng-solid`), text/textarea/select,
  checkboxes, radios, color, and range inputs, plus a native `progress` bar.
- Layout primitives: `.ng-shell`, `.ng-grid-2`, `.ng-stack`, `.ng-row`,
  `.ng-actions`, `.ng-table-wrap`.
- Mood components: `.ng-top`, `.ng-card`, `.ng-moon`, `.ng-dream`, `.ng-tag`,
  `.ng-note`, `.ng-nav`.
- Readouts: `.ng-split`, `.ng-meter` (with `--ng-value`).
- Theming: a 7-token core palette, dark and light themes, `data-theme`
  overrides, and `prefers-color-scheme` support.
- Accessibility: keyboard focus rings, `aria`-friendly demo, reduced-motion
  guard.

---

## Roadmap for v0.02

Prioritized ideas for the next version. Each is intentionally scoped so it can
ship as its own small release.

1. **Component modifiers** — filled/outline tag and note variants, a
   "moonlit" card variant with a stronger top glow, and a centered hero card.
2. **More mood pieces** — a crescent-moon decorative element (carved with a
   shadow, no images) and a "starlight" particle backdrop.
3. **Layout helpers** — `.ng-grid-3`/`.ng-grid-auto`, a `.ng-center` utility,
   and a `.ng-card--flush` modifier.
4. **Fine-grained motion** — a slow candle-flicker or moon-pulse animation,
   always gated behind `prefers-reduced-motion`.
5. **Utility classes** — spacing and text utilities for one-off layout needs.
6. **Distribution niceties** — a minified build, a `package.json` with an
   `"exports"` map so `import "nightingale-css/style.css"` resolves cleanly,
   and a demo that doubles as a visual regression page.

---

## Design notes

- **Dark is the default** — "late nights" is the brief, so the default look is
  the midnight palette; "dawn" is the light alternative.
- **Contrast stays soft but legible** — nothing is pure black on pure white;
  even the danger red is muted so the interface never glares at night.
- **No images, no icon fonts** — the moon, glows, and vignettes are all drawn
  with CSS gradients and shadows, so the library stays a single file and
  scales crisply.
