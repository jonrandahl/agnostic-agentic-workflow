---
name: safe-area-audit
description: >
  Audit a web project for CSS safe area inset support and apply fixes where
  missing. Use whenever asked to check or add safe area support, ensure
  notch/home-indicator compatibility, or when a project uses position:fixed or
  position:sticky elements that may overlap device UI chrome. Triggers on
  phrases like "safe area", "notch support", "env() insets", "viewport-fit",
  "home indicator", or "mobile edge support".
---

# Safe Area Audit

Audit and apply CSS safe area inset support (`env()` / `viewport-fit=cover`).

---

## Why This Matters

Modern devices (iPhone notch/Dynamic Island, Android gesture bar) reserve
physical screen space for system UI. Without `viewport-fit=cover` and
`env(safe-area-inset-*)`, a fixed header sits behind the notch and a bottom
dock overlaps the home indicator — making controls untappable or invisible.

The fix is a two-step opt-in:

1. **`viewport-fit=cover`** in the viewport meta → the browser hands full-screen
   ownership to the page, exposing non-zero inset values via `env()`.
2. **`env(safe-area-inset-*)`** in CSS → read the device's inset measurements
   and apply them as additive padding on elements anchored to device edges.

On devices with no notch or home bar, all insets resolve to `0px` — the same
CSS works everywhere with no UA-sniffing or conditional logic.

---

## Phase 1 — Audit

### 1.1  Viewport meta

Read the main HTML entry point(s) and search for `viewport-fit=cover` in the
`<meta name="viewport">` tag. Flag if absent.

### 1.2  CSS env() usage

Grep all `.css` files for `safe-area-inset` or `env(safe`. Flag if absent.

### 1.3  Fixed/sticky element inventory

Grep for `position:\s*fixed` and `position:\s*sticky` across all stylesheets.
For each match, record: the file, the selector, and which edges it is anchored
to (`inset-block-start`, `inset-block-end`, `inset-inline-*`, `top`, `bottom`,
`left`, `right`). Those edges are the ones that need safe area padding.

---

## Phase 2 — Apply Fixes

Apply each fix in order. Each step is independently valid, so a failure mid-way
leaves the file in a working state.

### 2.1  Viewport meta tag

Add `viewport-fit=cover` to the existing viewport `<meta>` content. Do not
create a second meta tag; append to the existing `content` attribute.

```html
<!-- BEFORE -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<!-- AFTER
     viewport-fit=cover: opts into edge-to-edge layout on iOS/Android.
     Without this, the browser reserves the notch/home-bar regions itself
     and env(safe-area-inset-*) returns 0px for everything — the browser
     has already "solved" the problem by clipping your content away from
     the unsafe zones, so there is nothing left to report. -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```

### 2.2  Safe area custom properties in :root

Add the four inset variables to the existing `:root` block in the project's
base token file (whichever file already holds global CSS custom properties).

```css
/* Safe area insets from the browser environment.
   These are NOT author-defined variables — the browser sets them based on
   the physical shape of the screen (notch, Dynamic Island, home bar, rounded
   corners). They are exposed via env() and are only non-zero when
   viewport-fit=cover is present in the viewport meta tag.
   The 0px fallback makes them safe to use in calc() on older browsers that
   pre-date env() support (iOS < 11.2, Chrome < 69). */
--safe-top:    env(safe-area-inset-top,    0px);
--safe-bottom: env(safe-area-inset-bottom, 0px);
--safe-left:   env(safe-area-inset-left,   0px);
--safe-right:  env(safe-area-inset-right,  0px);
```

Define once in `:root`, reference everywhere with `var()`. This avoids
scattering raw `env()` calls across component files and keeps the fallback
value in one place.

### 2.3  Apply safe area to fixed/sticky elements

For each element identified in Phase 1.3, apply the inset variable that
corresponds to the edge the element is anchored to. Use `calc()` so existing
spacing is preserved and safe area is purely additive.

**Pattern:**

```css
/* Fallback for pre-env() browsers — the bare declaration is applied first */
padding-block-start: <existing-value>;
/* env()-aware override — modern browsers take this; calc adds the inset on
   top of the spacing that was already there, so no content is displaced */
padding-block-start: calc(<existing-value> + var(--safe-top));
```

Only apply the variable that maps to the element's anchored edge. An element
anchored at `inset-block-start: 0` needs `--safe-top`; one anchored at
`inset-block-end: 0` needs `--safe-bottom`. Do not apply all four to every
element.

#### Priority order for this project

Work through these in order of user-impact:

**1. `body > header` / `site-header > header`** (`layout.css`)
Fixed at `inset-block-start: 0`. Apply `--safe-top` to `padding-block-start`.
This is the highest-impact fix: a header behind the Dynamic Island is the
canonical notch failure on mobile. The header already sets `padding-block`,
so this becomes a `calc()` addition after the shorthand (the longhand wins
because it is more specific in cascade order).

**2. `.dialog-dock`** (`components/dialog.css`)
Fixed at `inset-block-end: 0` in the bottom-left corner. Apply `--safe-bottom`
to `padding-block-end`. The dock holds minimised-dialog tab controls — if these
sit behind the iOS home indicator they become unreachable.

**3. `.scroll-to-top`** (`layout.css`)
Fixed at `inset-block-end: -0.5rem` in the bottom-right corner. Update
`inset-block-end` rather than padding (padding would shrink the visible button;
the element's bottom offset is what controls its position): use
`calc(-0.5rem + var(--safe-bottom))` so the button shifts upward by the home
bar height on notched devices while remaining at its intentional `-0.5rem`
offset on devices without one.

**4. `.about-panel`** (`components/about.css`)
Fixed with `inset-inline: 0` (full-width). Its `inset-block-start` is already
`4.5rem` (a header-relative offset), so no top correction is needed — the
header's own safe-area fix propagates implicitly. Apply `--safe-left` and
`--safe-right` to `padding-inline` for the inline edges.

**5. `body::before`** (scroll progress bar, `layout.css`)
Fixed at top, `inset-inline: 0`. This is a decorative 3 px bar — it has no
interactive surface and the visual bleed behind a notch is imperceptible.
Leave untouched.

**6. `.pen-viewer` / `.styleguide-viewer`** (`components/dialog.css`)
Both use `inset: 0; margin: auto` to centre within the viewport. `margin: auto`
distributes available space equally, so the dialog is already fully within the
safe area on all four sides without any explicit inset values. No change needed
unless a full-screen (`100dvw × 100dvh`) variant is added without `margin: auto`.

### 2.4  Cross-check: inset edge → variable mapping

After applying changes, verify:

- Every selector with `inset-block-start: 0` (or `top: 0`) uses `--safe-top`.
- Every selector with `inset-block-end: 0` (or `bottom: 0`) uses `--safe-bottom`.
- Every selector spanning full inline width (`inset-inline: 0`) considers
  `--safe-left` / `--safe-right`.

Apply any gaps before moving to Phase 3.

---

## Phase 3 — Report

Output a concise summary:

```
### Safe area audit

Viewport meta         : [PASS | FIXED]  viewport-fit=cover
CSS custom properties : [PASS | ADDED]  --safe-top/bottom/left/right in :root
Fixed/sticky elements : <n> found, <n> updated, <n> already safe or out of scope

Changed files:
- <path>: <description of what changed>
- …

No further gaps found.
```

If run on a project other than this blog, note which files were inspected and
flag anything that could not be checked automatically (inline styles, CSS-in-JS,
shadow DOM).
