# Technical Patterns — Design & Documentation Standards

Established conventions for common technical implementation patterns.

## Purpose

Captain specific design and documentation decisions that recur across projects.
Rather than re-decide dark theme implementation or JSDoc syntax each time, these
patterns are established once and reused. This reduces cognitive load and makes
code review faster.

## Why This Matters

- **Patterns reduce decision fatigue.** Once established, they become muscle
  memory.
- **Code review becomes faster.** Reviewers do not need to discuss style choices
  again — they are documented.
- **Projects stay consistent.** Multiple projects implementing the same feature
  follow the same approach.
- **Maintenance is easier.** When all dark themes follow one pattern, fixing
  bugs benefits all projects.
- **New team members learn faster.** Patterns are teachable; ad-hoc decisions
  are not.

---

## Structured data discovery

For published content and external data representations, apply the
[structured data guideline](structured-data-guideline.md) and use the
[schema-discovery skill](../skills/schema-discovery/SKILL.md) when its
assessment triggers apply.

---

## Dark Theme Implementation

Convention established: WCAG Contrast Checker, May 2026.

### Implementation pattern

All projects implement `prefers-color-scheme` using CSS custom property
overrides in `base.css`. The dark block redefines every colour token explicitly
rather than relying on partial overrides or cascade order. This makes the dark
theme self-documenting and safe to edit in isolation.

```css
@media (prefers-color-scheme: dark) {
  :root {
    --colour-foreground: ;
    --colour-background: ;
    --colour-page-bg: ;
    --colour-accent: ;
    --colour-border: ;
    --colour-muted: ;
  }
}
```

### Colour verification

All dark theme colours must be verified for WCAG compliance before committing.
Document the contrast ratios in a comment directly above the media query block:

```css
/*
  Verified contrast ratios against --colour-background:
    --colour-foreground: x.x:1  AAA
    --colour-accent:     x.x:1  AAA
    --colour-muted:      x.x:1  AA
*/
```

Use the [WCAG Contrast
Checker](https://codepen.io/editor/jonrandahl/pen/019e25b8-38f8-75b4-bd36-204689d0cba7/3d1e714ab4f7316dfdcd622a903a3eb0)
itself to verify. This keeps the tool honest and acts as a live regression test
for its own calculations.

### Accent colour

The light theme accent must be a dark value (for contrast against light
backgrounds). The dark theme accent must be a lighter value in the same hue
family. Both should aim for AAA (7:1) where achievable. Established pair:
`#1a5c6e` light / `#63c5da` dark, teal family.

### Tokens to always override

At minimum: `--colour-foreground`, `--colour-background`, `--colour-page-bg`,
`--colour-accent`, `--colour-border`, `--colour-muted`. Pass and fail state
colours (`--colour-pass`, `--colour-fail` and their background variants) must
also be overridden on any project that uses them.

### Shadow tokens

Box shadows using `rgba(0, 0, 0, ...)` become invisible on dark backgrounds.
Extract shadow values to a token (`--shadow-card` etc.) and override opacity in
the dark block.

---

## Accessibility: Focusable Elements Inside `aria-hidden`

Convention established: jonhumphrey.dev, July 2026 (skip-link experiment
surfaced this as a live `aria-hidden-focus` violation).

### The rule

Never place a focusable element (links, buttons, form controls, or anything
with a positive `tabindex`) inside an ancestor carrying `aria-hidden="true"`.
Screen readers remove the entire subtree from the accessibility tree when an
ancestor is `aria-hidden`, regardless of whether a descendant is individually
focusable — so sighted keyboard users can still tab to it while screen reader
users get nothing. This is silent: nothing looks wrong visually, and it only
surfaces via an automated accessibility scan or a screen reader session, not
by eyeballing the page.

### Where this bites in practice

Layout/utility wrapper elements are the usual trap — e.g. an empty anchor
target div used for a "back to top" link (`<div id="top" aria-hidden="true">`)
is reasonably marked `aria-hidden` when it's empty and non-interactive. The
bug appears when something focusable (a skip-link, an icon button, anything)
is later added *inside* that same wrapper without revisiting the
`aria-hidden` attribute on the parent.

### How to catch it

- Run axe-core (`@axe-core/playwright` or equivalent) against any page where
  a focusable element was added near an existing `aria-hidden` wrapper; the
  rule ID is `aria-hidden-focus`, reported at `serious` impact
  - Reference: <https://dequeuniversity.com/rules/axe/4.10/aria-hidden-focus>
- When adding a new interactive element to an existing layout, check whether
  it inherited an `aria-hidden` ancestor from a container that predates it
- Prefer a dedicated, purpose-built regression test (assert the specific
  element has no `[aria-hidden="true"]` ancestor) over relying solely on a
  broad sitewide a11y sweep — a targeted test documents *why* the check
  exists and survives even if the sweep's scope or tags change later

---

## CSS Cascade Layers: Overrides Must Live in the Same Layer

Convention established: jonhumphrey.dev, July 2026 (embed-mode skip-link fix —
an override placed in the wrong `@layer` silently lost the cascade).

### The rule

When writing a conditional override for a property (a state class, a
`data-*` attribute selector, a media query, anything that overrides a base
rule elsewhere), the override must live in the **same `@layer`** as the rule
it needs to beat — not wherever the *conditional context* conventionally
lives. Cascade layers are resolved by declaration order first: a rule in a
later-declared layer always wins over a rule in an earlier-declared layer,
regardless of selector specificity or source order within the file. Adding
`!important` or a more specific selector does not fix a cross-layer loss;
only moving the override into the correct layer does.

### Where this bites in practice

A codebase organises files by *concern* (e.g. `layout.css` for structural/
embed-mode rules, `components/*.css` for individual components), but a
component's own base styling and a later conditional override for that same
component can end up in different layers if the override is added to
whichever file "feels right" for the conditional context rather than the
file/layer that owns the property being overridden. Symptom: the override
compiles, the selector matches (confirmed via devtools), but the computed
style never changes — because a same-property rule in a higher-priority
layer wins regardless.

### How to catch it

- Before adding a conditional override, check which `@layer` the base rule
  for that property is declared in (search for the class/element's own
  styling, not just the selector you're about to add)
- Check the project's layer order declaration (commonly one `@layer a, b,
  c;` statement near the top of the stylesheet entry point) — later layers
  in that list win ties
- If the override must live in a conventionally-organised file that's in a
  *different* layer than the property owner, either move the override to
  the owning layer's file, or wrap it in an explicit `@layer <name> { ... }`
  block naming the correct layer directly in that file
- Write a test asserting the *computed* style (e.g. `getComputedStyle(el)
  .display`), not just that the selector exists in source — a cross-layer
  loss looks identical to a correct override until the computed value is
  actually checked

---

## JavaScript Documentation (JSDoc)

JSDoc comments use `/** ... */` syntax and are placed immediately before the
item being documented. Use `//` for inline implementation notes only.

### File header

Every JavaScript file should open with a file overview:

```js
/**
 * @fileOverview Brief description of the file's purpose.
 * @see https://link-to-relevant-spec-or-reference.com
 */
```

### Function documentation

Document every exported function. Internal functions should be documented where
their purpose is not immediately obvious from the code.

```js
/**
 * Brief description of what the function does. Use third-person singular.
 * @param {type} name - Description of the parameter.
 * @param {type} [name] - Optional parameter indicated by square brackets.
 * @returns {type} Description of the return value.
 */
```

### Common tags

- `@param {type} name - description` – function parameter
- `@returns {type} description` – return value
- `@fileOverview description` – file-level description
- `@see url` – link to relevant specification or reference
- `@throws {Error} description` – error the function may throw
- `@deprecated description` – mark as deprecated with reason

### Rules

- Descriptions use British English throughout
- End descriptions with a full stop
- Use third-person singular verbs: "Calculates", "Returns", "Converts"
- Be descriptive but concise – document what and why, not how
- Keep JSDoc updated when code changes – outdated docs are worse than none
- Do not use JSDoc for constants, DOM references, or obvious one-liners
- Use `//` for inline notes, `/** */` for documentation only
- Describe what and why, not how or implementation detail
- Consolidate related aspects into a single meaningful entry

---

*Last updated: July 2026*
