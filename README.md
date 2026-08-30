<p align="right"><a href="README.fr.md">Lire en français</a></p>

# Sticky Add-To-Cart — floating buy bar that survives a bundle picker

[![Theme Check](https://github.com/pteyo032/Sticky-Add-To-Cart/actions/workflows/theme-check.yml/badge.svg)](https://github.com/pteyo032/Sticky-Add-To-Cart/actions/workflows/theme-check.yml)

A fix + extension for Shopify **Horizon**'s native sticky add-to-cart bar:
it shows up once the buy buttons scroll out of view, and unlike the
out-of-the-box version, it keeps working — and stays in sync — when a
tier/bundle picker (like [Bundle
Selector](https://github.com/pteyo032/shopify-bundle-selector)) replaces the
theme's native buy-buttons form.

No third-party app, no new dependency — a small patch to three native
Horizon JS files.

| Default (native variant) | Synced to a selected bundle tier |
|---|---|
| ![Sticky bar showing the product's image, title and native variant](docs/screenshots/sticky-bar-default.png) | ![Sticky bar showing the selected bundle tier's label and price instead](docs/screenshots/sticky-bar-tier-synced.png) |

Also shown on desktop, as a floating pill instead of a full-width bar:

![Sticky bar on desktop, floating bottom-left](docs/screenshots/sticky-bar-desktop.png)

## The problem this fixes

Horizon's sticky bar locates the product's buy-buttons form with a fixed
selector (`product-form-component[data-product-id="..."]`). If your theme
swaps in a different element for that role — like a bundle/tier picker
does — the lookup silently fails and the bar never activates. No console
error, nothing visibly broken, it just never shows up. See
`docs/gotchas.md` for the full breakdown of why.

## What's in this repo

Not the full Horizon theme — just the patched files and the docs to apply
them to yours.

| Path | What it is |
|---|---|
| `assets/sticky-add-to-cart.js` | Patched native file — recognizes an alternate buy-buttons component, syncs display with `BundleTierChangeEvent` |
| `assets/events.js` | Patched native file — adds `ThemeEvents.bundleTierChange` / `BundleTierChangeEvent` |
| `assets/bundle-selector.js` | Patched native file — dispatches the new event on tier change (only needed if you use a bundle/tier picker) |
| `docs/integration-guide.md` | Exact steps to apply this to your own theme, with or without a bundle picker |
| `docs/gotchas.md` | Technical pitfalls found while building this |

## Quick start

1. If you just want the native Horizon sticky bar with no picker involved:
   nothing to install — enable **Sticky add to cart bar** on the product
   information section in the theme editor.
2. If a tier/bundle picker breaks it: see `docs/integration-guide.md` for
   the two changes needed (swap in three JS files, add one CSS class).

## License

MIT — see `LICENSE`.
