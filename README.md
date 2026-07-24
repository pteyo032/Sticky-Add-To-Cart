# Sticky Add-To-Cart

Thème Shopify [Horizon](https://help.shopify.com/en/manual/online-store/themes/horizon) enrichi de deux fonctionnalités :

- **Bundle Selector** — sélecteur de paliers de quantité ("Achetez 1 / 3 / 4...") avec réductions configurables (pourcentage, montant fixe, ou unités offertes) et sélecteurs de variante par unité pour les produits avec options.
- **Barre d'achat flottante (Sticky Add-to-Cart)** — apparaît au scroll une fois les boutons d'achat sortis de l'écran ; compatible avec le Bundle Selector et reflète en direct le palier sélectionné (label + prix).

## Fonctionnalités

### Bundle Selector

- Paliers configurables par le marchand (nombre, quantité, libellé) depuis l'éditeur de thème
- Trois types de réduction par palier : pourcentage, montant fixe, ou N unités offertes
- Sélecteurs de variante (taille, couleur…) par unité, uniquement si le produit a plusieurs variantes
- Badge personnalisable par palier (ex: "Plus populaire", "Meilleure offre")
- Activation en case à cocher opt-in sur le bloc natif **Boutons d'achat**, comportement natif inchangé si désactivé

> **Limite connue :** le prix affiché par le sélecteur est calculé côté thème pour l'affichage uniquement. Il ne force pas automatiquement la réduction correspondante au paiement — le marchand doit configurer une réduction Shopify correspondante dans l'admin (Réductions), ou déployer une Shopify Function pour une synchronisation garantie.

### Barre d'achat flottante

- Réutilise le composant natif du thème Horizon (`enable_sticky_add_to_cart`)
- Se déclenche via `IntersectionObserver` une fois le bloc d'achat entièrement hors du viewport, se cache en bas de page
- Reste compatible avec le Bundle Selector (corrige un conflit natif où la barre ne se déclenchait jamais si le Bundle Selector était actif)
- Se met à jour en direct pour afficher le palier de bundle sélectionné (label + prix) au lieu de la variante native

## Installation

Comme tout thème Shopify, via [Shopify CLI](https://shopify.dev/docs/themes/tools/cli) :

```bash
shopify theme dev --store <votre-boutique>.myshopify.com
```

ou en le connectant à une boutique via l'éditeur de thème (Admin → Boutique en ligne → Thèmes → Ajouter un thème → Connecter depuis GitHub).

---

# Sticky Add-To-Cart (English)

A [Shopify Horizon](https://help.shopify.com/en/manual/online-store/themes/horizon) theme enhanced with two features:

- **Bundle Selector** — a "Buy 1 / 3 / 4…" quantity-tier picker with configurable discounts (percentage, fixed amount, or free units) and per-unit variant selectors for products with options.
- **Sticky Add-to-Cart bar** — appears on scroll once the buy buttons scroll out of view; compatible with the Bundle Selector and live-syncs its display with the selected tier (label + price).

## Features

### Bundle Selector

- Merchant-configurable tiers (count, quantity, label) from the theme editor
- Three discount types per tier: percentage, fixed amount, or N free units
- Per-unit variant selectors (size, color…), shown only when the product actually has multiple variants
- Customizable badge per tier (e.g. "Most popular", "Best deal")
- Opt-in checkbox on the native **Buy Buttons** block — native behavior is unchanged when disabled

> **Known limitation:** the price shown by the selector is calculated theme-side for display only. It does not automatically enforce the matching discount at checkout — the merchant needs to set up a matching Shopify discount in the admin (Discounts), or deploy a Shopify Function for a guaranteed match.

### Sticky Add-to-Cart bar

- Reuses Horizon's native component (`enable_sticky_add_to_cart`)
- Triggers via `IntersectionObserver` once the buy-buttons block is fully out of the viewport, hides again near the page footer
- Stays compatible with the Bundle Selector (fixes a native conflict where the bar never triggered while the Bundle Selector was active)
- Live-updates to show the selected bundle tier (label + price) instead of the native variant

## Installation

Like any Shopify theme, via the [Shopify CLI](https://shopify.dev/docs/themes/tools/cli):

```bash
shopify theme dev --store <your-store>.myshopify.com
```

or by connecting it to a store through the theme editor (Admin → Online Store → Themes → Add theme → Connect from GitHub).
