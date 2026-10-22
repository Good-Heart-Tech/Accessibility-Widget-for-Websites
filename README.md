# Accessibility Widget for Websites

<img src="assets/icon-mark.svg" alt="" width="72" height="72" align="right">

A free, open-source accessibility toolbar that drops into any website with a single `<script>` tag. Good Heart Tech hosts it for you on Cloudflare, so there is nothing to download, build, or deploy: add one line of code to your site (if it isn't there already) and you are done. Built by Good Heart Tech, a nonprofit MSP, for its own nonprofit partners' sites, but there's nothing nonprofit-specific about it. Any site can use it.

**Live demo:** try it right now on [goodhearttech.org](https://goodhearttech.org) or [wosp.app](https://wosp.app), both run this widget in production. No install needed to see it in action.

**Plain-language guide:** [goodhearttech.org/accessibility-widget](https://goodhearttech.org/accessibility-widget) explains what it does and how to add it, written for site owners rather than developers.

## Why this exists

Paid overlay widgets (EqualWeb, AccessiBe, etc.) charge ongoing subscriptions and market themselves as full ADA/WCAG "compliance," which they are not. This project is a genuinely free, no-lock-in alternative that:

- Adds visible, easy-to-find display controls: contrast modes, text size and spacing, highlighted links and headings, a readable font, a larger cursor, and read-aloud
- Is free and hosted by Good Heart Tech (or by you, if you prefer), with no account, no tracking code, and no recurring cost
- Is small enough for a volunteer engineer to read and modify in one sitting

**Important:** This widget improves usability. It does not make a site WCAG/ADA compliant on its own. Real compliance still requires correct semantic HTML, alt text, color contrast in the base design, and keyboard navigation in the underlying site.

See [CHANGELOG.md](CHANGELOG.md) for what shipped and when.

## Architecture

- Plain JS + CSS in `src/`, no framework, no runtime dependencies. `esbuild` is only a dev-time minifier.
- `npm run build` minifies `src/widget.js` + `src/widget.css` into `dist/`, which is what gets served.
- `demo/index.html` is a sample static page for manually testing every control.
- No fonts, tracking, or analytics of any kind. The one exception is the toggle button's own icon, which loads Font Awesome from a CDN if the host page doesn't already have it. See "Icon" below for the full explanation.

## Installation

**There is nothing to install or deploy.** Good Heart Tech hosts the widget on Cloudflare at `accessibility.goodhearttech.org`, so you only add one line of code to your website. No account, no API key, no build step, no files to upload.

**Paste this one tag** just before the closing `</body>` tag (or into your site's "footer code" or "custom code" area). If you are not sure whether it is already on your site, just add it: if the tag appears on a page twice, the toolbar still only appears once.

```html
<script src="https://accessibility.goodhearttech.org/widget.js" defer></script>
```

That works on anything that lets you add a script tag: plain HTML and static sites (including Cloudflare Pages), WordPress, Squarespace, Wix, and similar. Some platforms limit custom code to paid plans. The widget injects its own styles, so no separate stylesheet link is needed.

Because the file is served from our hosting, improvements and fixes reach every site automatically, usually within about four hours (the file is cached for four hours). If you would rather freeze a version or avoid loading a file from a third party, see [Self-hosting](#self-hosting-optional) below.

### Brand colors

Override the toolbar's button and focus-outline colors globally by adding two data attributes to the script tag. No separate config file, no build step:

```html
<script
  src="https://accessibility.goodhearttech.org/widget.js"
  data-brand-primary="#7189ff"
  data-brand-accent="#a0ddff"
  defer>
</script>
```

- `data-brand-primary`: the toggle button background and pressed-toggle background (defaults to a navy blue)
- `data-brand-accent`: the focus-outline color (defaults to amber)

`data-brand-accent` is checked against the panel's white background before it's applied (WCAG's 3:1 minimum contrast for UI components like focus indicators). If a site's chosen color doesn't pass, the widget quietly falls back to the default amber instead of shipping a focus outline nobody can actually see. An accessibility tool with an inaccessible focus indicator would be a pretty bad look.

Both are plain CSS custom properties (`--ght-a11y-brand-primary`, `--ght-a11y-brand-accent`) set on `<html>`, so they can also be overridden in the host site's own CSS if preferred over data attributes.

Each site that installs the widget sets its own colors independently. There is no shared or global setting. Good Heart Tech's own site and WOSP, for example, each pass their own values on their own script tag.

### Icon

The floating toggle button's icon defaults to Font Awesome's `fa-solid fa-universal-access` icon, the standard "person in a circle" symbol used across accessibility tools, so visitors recognize it immediately.

Override it to any other Font Awesome icon with `data-icon` on the script tag:

```html
<script
  src="https://accessibility.goodhearttech.org/widget.js"
  data-icon="fa-solid fa-wheelchair"
  defer>
</script>
```

The value is exactly the class string you would put on an `<i>` tag, so any free or kit-hosted Font Awesome icon works, not just a fixed set the widget ships with.

**The tradeoff this introduces:** unlike every other part of this widget, the icon is real Font Awesome markup, not an inline SVG, so it needs Font Awesome's CSS and font files to render. The widget checks the page for an existing Font Awesome `<link>` or Kit `<script>` tag first. If the host site already loads Font Awesome (Good Heart Tech's site does), nothing extra happens. If it does not, the widget adds one `<link>` to Font Awesome's CDN (`cdnjs.cloudflare.com`) itself. That is the one exception to this project having no other external network calls.

## Self-hosting (optional)

**Most people should skip this section.** It is only for you if you want your own copy of the widget on your own domain (for example, to pin a specific version or to avoid loading a script from another host). It is not needed to use the widget.

This repo can be connected once to a Cloudflare Pages project so every site you manage shares one hosted copy. This is how Good Heart Tech runs the hosted version above:

1. In the Cloudflare dashboard, create a new Pages project and connect it to this GitHub repo's `main` branch.
2. Build command: `npm run build`
3. Build output directory: `dist`
4. Add a custom domain (e.g. `accessibility.goodhearttech.org`) pointing at the Pages project.
5. Every push to `main` auto-deploys. No manual upload step, no GitHub Actions needed.

## Local development

```bash
npm install
npm run build
```

Then open `demo/index.html` through a local static server (not `file://`, since the demo needs relative script execution) to try every control.

## License

GNU AGPL-3.0, see [LICENSE](LICENSE).
