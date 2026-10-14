# UI/UX Mobile-First Guideline

## Scope

This guideline applies only to UI/UX development and interface changes:

- visual design and layout
- responsive behaviour
- CSS and presentation-layer structure
- interaction surfaces and controls
- navigation, dialogs, forms, and content presentation
- front-end usability and accessibility at different viewport sizes

It does not apply to systems or functionality work unless that work changes a
user-facing interface. It does not govern back-end services, APIs, data models,
infrastructure, CI/CD, package management, domain logic, or non-visual
application behaviour.

## Mobile-First Rule

Implement and validate the smallest supported viewport before adding wider-layout
enhancements. Treat mobile as the foundation of the interface, not as a later
compatibility pass.

Responsive UI must be:

- fluid and content-driven rather than dependent on arbitrary device widths
- free from horizontal overflow, clipping, overlap, and unintended resizing
- usable with touch, keyboard, screen readers, and reduced motion
- resilient to long labels, translated text, dynamic content, and narrow widths
- progressively enhanced for tablets and desktops

Do not make core functionality depend on hover, large-screen space, pointer
precision, or desktop-only interaction patterns.

## Implementation Expectations

Before considering a UI change complete:

1. Establish the narrowest supported viewport and make the core experience work
   there first.
2. Add wider-layout enhancements deliberately through fluid sizing, content-
   driven breakpoints, grid, flexbox, or other stable responsive constraints.
3. Check text, controls, dialogs, navigation, embeds, fixed elements, sticky
   elements, and animations at narrow and wide viewports.
4. Verify keyboard focus, touch targets, safe areas, zoom behaviour, and reduced-
   motion behaviour where relevant.
5. Add focused regression coverage for responsive defects rather than relying
   only on a broad desktop test.
6. Validate at representative mobile and desktop sizes before publishing the
   change.

Desktop layouts must remain intentional and polished. Mobile-first does not mean
mobile-only, a single fixed breakpoint, or a desktop experience assembled from
leftover mobile rules.

## Review Question

For every UI/UX change, ask:

> Does the narrowest supported viewport establish a usable, accessible baseline,
> with larger viewports enhancing the experience without changing its core
> usability?
