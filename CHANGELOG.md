# Changelog

All notable changes to this project are documented here. Dates are when each change shipped to `main`.

## Unreleased

- Added `data-position` on the script tag to move the button to any corner: `bottom-left` (default), `bottom-right`, `top-left`, or `top-right`. The panel opens from the same corner. Sites that don't set it are unchanged.
- The options panel now stays fully on screen on 360px-wide phones and short windows. It used to run about 10px off the edge because its padding wasn't counted in its width. Desktop size is unchanged.
- If the script tag ends up on a page more than once, only the first copy builds the toolbar, so visitors never see two buttons. Site owners no longer need to check whether it is already installed before adding it.
- Toggle button is inserted first in the page's `<body>`, not appended last, so keyboard and screen-reader users can reach it without tabbing through the whole page first.
- A site's `data-brand-accent` is now checked against the panel's white background (WCAG 3:1 minimum for UI components). If it fails, the widget falls back to the default amber instead of shipping an unreadable focus outline.
- Toggle button and its icon are bigger (56px button, 28px icon, up from 48px and 20px) for visibility at a glance.

## 2026-09-09

- Toggle button icon switched to Font Awesome's `fa-solid fa-universal-access`, with `data-icon` to override it to any other Font Awesome icon. Documented in the README.
- Fixed a scrollbar that appeared on the options panel: toggle buttons moved from a stacked icon-over-label layout to a horizontal row, and the panel's max height was raised as a safety margin.
- Added a "Contrast" group label above the three mutually exclusive contrast modes.
- Stepper buttons grew from 32x32 to 40x40 to meet the WCAG-recommended minimum touch target size.
- Added icons to every toggle button, a live numeric readout on the spacing steppers, a checkmark badge on active toggles (not color alone), a softer panel with rounded corners and a hudu-light border, and a one-time attention pulse on a visitor's first-ever page load.
- Added `assets/icon-mark.svg`, a standalone brand-colored logo used in the README and as a favicon/badge source.

## 2026-09-08

- v1 shipped: contrast modes, font and line and word and letter spacing, highlight links and headers, a readable (dyslexia-friendly) font, an enlarged cursor, browser-based text-to-speech, a reset button, and persistent per-visitor settings via `localStorage`.
- Switched from a two-tag install (`<link>` + `<script>`) to a single `<script>` tag: the build now inlines the minified CSS into the JS bundle, injected as a `<style>` tag at init.
- Added `data-brand-primary` and `data-brand-accent` on the script tag so any site can theme the toolbar to match its own brand, applied as CSS custom properties.
