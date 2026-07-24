# Technical gotchas

Things that cost real debugging time while building/fixing this — recorded
so you don't have to rediscover them.

1. **The bar only triggers once the buy-buttons block is *fully* out of the
   viewport** — both its top and bottom edge above `y = 0`. On a short
   product page (short description, few or no recommended products), there
   may not be enough scrollable height below the buttons for that to ever
   happen. Not a bug — it's the `IntersectionObserver` doing exactly what
   it's told — but easy to mistake for one if you only test on a thin
   catalog.

2. **Incompatible out of the box with a Bundle Selector–style tier picker.**
   `sticky-add-to-cart.js` locates the product form with
   `document.querySelector('product-form-component[data-product-id="..."]')`
   inside the closest `.shopify-section`. If your buy-buttons block swaps in
   a different custom element when some other picker/mode is active (in our
   case `<bundle-selector-component>`), that query returns nothing and the
   bar's `IntersectionObserver` setup silently no-ops — `if (!productForm)
   return;` — no console error, the bar's markup is still in the DOM, it
   just never activates. Fixed here by having `#getProductForm()` also match
   the alternate component, and giving that component the same
   `.buy-buttons-block` anchor class the native form has.

3. **A single `window.scrollTo()` jump can undershoot on pages with
   lazy-loaded sections.** If `document.body.scrollHeight` is measured
   *before* scrolling, and a section further down (e.g. product
   recommendations) lazy-loads more content once it nears the viewport, the
   page grows *after* you've already scrolled — so a one-shot scroll to the
   pre-growth `scrollHeight` lands short of the real bottom, and the buy
   buttons block never fully clears the viewport. When testing
   automatically, scroll in several steps and re-measure `scrollHeight`
   each time, like a real user scrolling would.

4. **A Shopify draft-preview link (`?preview_theme_id=`) overlays its own
   fixed bottom bar** (`<iframe id="PBarNextFrame">`) showing the theme's
   dev/draft status. It can sit visually on top of this component's own
   fixed-bottom bar without any error — `data-stuck="true"`, `opacity: 1`,
   correct position in the DOM, just invisible under the preview chrome.
   Doesn't happen on a real published theme. If testing visually via a
   preview link, hide `#PBarNextFrameWrapper` before taking a screenshot.

5. **Syncing extra state (like a selected bundle tier) into the bar requires
   a real event, not polling.** The bar and a tier picker are two separate
   custom elements — neither is an ancestor of the other, so a plain
   bubbling `CustomEvent` dispatched from the picker won't reach it directly.
   What does work: both happen to sit inside the same `.shopify-section`, so
   dispatching on `this` with `bubbles: true` and listening on
   `this.closest('.shopify-section')` — the same pattern the theme already
   uses for `StandardEvents.productSelect` — gets the event where it needs
   to go without any shared global state.
