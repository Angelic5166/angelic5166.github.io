# Handoff: Aquor Hydrants & Valves Collection + Storefront Editor

## Overview
A prototype of the Aquor Water Systems "Hydrants & Valves" product collection page and product detail page (PDP), plus an internal "Storefront Editor" admin layer for in-place content editing (text, images, colors, nav, products) and export (Save View / Share as PDF or JPG).

## About the Design Files
The file in this bundle (`Hydrants Collection.dc.html`) is a **design reference built in HTML** — a working prototype showing intended look, layout, and interaction, not production code to copy directly. The task is to **recreate this design in the target codebase's existing environment** (React, Vue, etc.) using its established component patterns and libraries — or, if no environment exists yet, choose the most appropriate framework and implement there. Do not ship this HTML file as-is.

## Fidelity
**High-fidelity.** Colors, typography, spacing, and copy are final/near-final. Recreate pixel-accurately using the codebase's own design-system components where equivalents exist (Button, Select, Badge, etc.) — the reference CSS tokens in `reference/` describe the exact values to match.

## Screens / Views

### 1. Collection View (Hydrants & Valves)
- **Purpose**: Browse the Hydrants & Valves product grid, filter and sort.
- **Layout**: Sticky header (logo + nav) → promo bar (dark strip, "Free shipping...") → breadcrumb → H1 "Hydrants & Valves" → filter/sort row → 4-column product grid (`grid-template-columns: repeat(4, 1fr)`, `gap: 28px 20px`), max-width container (`var(--container-max)`, centered, `32px` side padding).
- **Filter row**: "Filter ▾ N Results" toggle (collapsible) revealing 4 filter-group buttons (By Project Type, Product Type, Vacuum Breaker, Stem Length) — each a bordered rectangle, square corners (no border-radius), `8px 12px` padding, opens a checkbox dropdown panel showing "{n} selected / Clear". Right-aligned "Sort: {label} ▼" plain-text control opens a dropdown list of sort options (Featured, Best selling, Alphabetically A-Z/Z-A, Price low-high/high-low).
- **Product card**: square image (gray placeholder box `var(--neutral-100)` when empty, "Product photo" label), optional badge top-left (New / Best Seller / Out of Stock, colored per status token), product name (16px/700, `var(--water-ink)`), optional star rating, price (14px/400, `var(--neutral-600)`).
- **Nav**: horizontal nav bar with items incl. "Accessories" mega-menu (grid of category links + a featured accessory image tile), "Hydrants & Valves" as the active/underlined item.

### 2. Product Page View (PDP)
- **Layout**: two-column — left: main image + 4 thumbnails; right: title, rating, price, color swatches, quantity stepper, Add to Cart, then accordions (What's Included, Installation Guide, Tech Specs, CAD & Design Files) and an add-ons list.
- **Color swatches**: circular/rounded swatch + label per color option; edit mode adds a color picker, label field, delete button, and an "add color" tile.

### 3. Accessories category page
- Reached via the mega-menu; same product-grid layout as Collection View, populated with accessory products.

## Admin / Editor Layer
A toolbar strip above the page (`background: var(--neutral-100)`, `border-bottom: 1px solid var(--neutral-300)`) containing, left to right:
- **Edit** button (pencil icon + label) — toggles listing management mode (delete/replace product images, edit nav labels, delete nav items) and PDP edit mode on their respective views. Label flips to "Done Editing" while active.
- **+ Add Product** button (always visible on Collection View) — opens an "Add Product" form (name, price, badge, image).
- **Save View** button — persists all edited content (products, nav items, PDP copy/images/colors) to `localStorage`; briefly shows "Saved" confirmation.
- **Collection View / Product Page View** — plain-text toggle with a blue underline on the active item; switches which screen is shown.
- **Share** button (icon + label) — dropdown with "Download as PDF" / "Download as JPG", captures the page (toolbar hidden) via html2canvas and exports through jsPDF or a direct image download.

In edit mode, every text field becomes an `<input>`/`<textarea>`, and every image slot shows a camera/upload icon overlay to replace the image — all inline in the same visual position as the read-only state (no separate edit screen).

## Interactions & Behavior
- Filter buttons and Sort each toggle their own dropdown independently; clicking a checkbox toggles selection state (no live re-filtering logic wired — visual only).
- Mega-menu (Accessories) opens on click, navigates to the Accessories category page on category click.
- Quantity stepper: -/+ buttons adjust `pdpQty`.
- Accordions expand/collapse one section body at a time per accordion row (independent toggles, not exclusive).
- Add/replace/delete flows for products, nav items, and PDP color swatches are all local state mutations (no backend).

## State Management
State lives in a single component: `products[]`, `navItems[]`, `filterSelections`, `sortValue`, `currentView` ('collection'|'pdp'), `currentPage` ('hydrants'|'accessories'), all PDP copy fields (title, descriptions, sku, price, rating, accordion titles/bodies, addon labels), `pdpColorOptions[]`, image refs, and editor-mode flags (`managing`, `pdpEditing`, `navEditing`, `shareOpen`, `sortOpen`, `filtersOpen`). "Save View" serializes the editable subset of this state to `localStorage` under the key `aquor-storefront-view`; it's read back in on load.

## Design Tokens
See `reference/colors.css`, `reference/typography.css`, `reference/spacing.css` for exact values. Key ones:
- **Water blue** `#008CC7` (CTAs, active states), **Water ink** `#252D37` (headings, dark surfaces).
- **Neutrals**: `#62686F`, `#CCCCCC`, `#F0F0F0`, `#FFFFFF`.
- **Status**: Out of Stock `#E40000`, New `#008CC7`.
- **Type scale used**: H1 45px/650, H4 16px/700, P2 14px/400 (see typography.css for full scale and font stack — brand font is Neue Haas Grotesk Text Pro, substituted with Archivo since no license files were supplied).
- **Radius**: buttons/inputs/badges 4–6px, cards 10px, pills 999px. Filter/sort controls in this design intentionally use **square corners (0px)**, overriding the general card radius.

## Assets
- `assets/aquor-logo-primary.png` — header logo.
- `assets/edit-icon.png` — pencil icon (Noun Project, by novani) used on the Edit buttons.
- `assets/eye-icon.png` — eye icon (Noun Project, by Rizalwale) — was tried on the view toggle, reverted; kept in case it's wanted later.
- `assets/share-icon.png` — share icon (Noun Project, by Timur Minvaleev) used on the Share button.
- Product/PDP images are placeholder drop targets (gray boxes) — the developer should wire these to real product photography.

## Files
- `Hydrants Collection.dc.html` — the full design reference (all screens, admin layer, and interactions in one file).
