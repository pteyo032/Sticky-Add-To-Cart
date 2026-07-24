# Integration guide

## Base sticky bar — nothing to install

Horizon already ships this component natively. There's nothing to copy for
the base behavior — just enable it:

1. Theme editor → the product page's **Product information** section →
   turn on **Sticky add to cart bar**.
2. Make sure the product page has enough content below the buy buttons
   (description, recommendations…) for them to actually scroll fully out of
   view — see gotcha 1 in `docs/gotchas.md`.

This repo exists for the two things Horizon doesn't do out of the box:
**fixing** the bar so it still works when a tier/bundle picker replaces the
native buy-buttons form, and **syncing** its display with the picker's
current selection.

## If you use a tier/bundle picker (e.g. [Bundle Selector](https://github.com/pteyo032/Sticky-Add-To-Cart))

If your buy-buttons block conditionally renders a different custom element
instead of the theme's native `<product-form-component>` — the way a
"buy more, save more" tier picker typically does — the native sticky bar
breaks silently (gotcha 2). Two changes fix and extend it:

### 1. Replace the JS files

Copy these three files into your theme as-is, replacing the native ones:

- `assets/sticky-add-to-cart.js`
- `assets/events.js`
- `assets/bundle-selector.js` (only relevant if you're also using the
  Bundle Selector feature — skip it otherwise, and skip the
  `ThemeEvents.bundleTierChange` listener wiring below too)

What actually changed in each, if you'd rather patch your own copies by hand
instead of replacing the files outright:

- **`events.js`** — adds `ThemeEvents.bundleTierChange` and a
  `BundleTierChangeEvent` class (detail: `{ tierLabel, tierPrice }`),
  following the same pattern as the theme's existing
  `QuantitySelectorUpdateEvent`.
- **`sticky-add-to-cart.js`** — `#getProductForm()` now matches
  `product-form-component` **or** `bundle-selector-component` (swap in
  whatever custom element your own picker uses); a new
  `#handleBundleTierChange` listener updates `.sticky-add-to-cart__variant`
  and `.sticky-add-to-cart__price` from the event detail.
- **`bundle-selector.js`** — `onTierChange()` now also dispatches
  `BundleTierChangeEvent`, reading the label and price straight out of the
  already-rendered tier markup (`radio.dataset.tierLabel`,
  `.bundle-tier__price-current`) rather than recomputing pricing logic in
  JS.

### 2. Add one class to your picker's root element

Whatever element your picker renders instead of `<product-form-component>`
needs the `.buy-buttons-block` class so the sticky bar can find its anchor
for the `IntersectionObserver` (gotcha 2). In `blocks/buy-buttons.liquid`:

```diff
- class="bundle-selector spacing-style"
+ class="bundle-selector buy-buttons-block buy-buttons-block--{{ block.id }} spacing-style"
```

`.buy-buttons-block` only contributes `width: 100%` in CSS — safe to add
without visual side effects.

That's it. No other files need touching. If your picker doesn't dispatch
`BundleTierChangeEvent`, the bar simply falls back to showing the product's
native variant, same as before.

### Known limitation

On first page load, the bar still shows the native variant until the
shopper actively picks a tier — a radio that's already `checked` on load
doesn't fire a `change` event, so nothing tells the bar what the default
tier is. Fixable by reading the checked tier at `connectedCallback()`
instead of waiting only for the event, but not implemented here.
