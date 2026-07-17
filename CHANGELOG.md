# Changelog

## [0.3.0] - 2026-07-18

### Changed

- **Breaking:** `menu[].modifications` replaced by `menu[].option_groups` — one
  generic construct for every kind of configurable dish. Each group has a
  localized `name`, selection rules (`min_selections`, `max_selections`,
  `included_selections` for "first N included in the price"), optional
  `portions` (`divisions` of 2/3/4/6/8 with `proportional` or `full` pricing —
  pizza halves and quarters), and `options` with localized names,
  `price_adjustment`, `default`, `max_quantity` (e.g. double cheese),
  `allergens`, and diet `tags`. This expresses the old `addition` / `removal` /
  `substitution` / `option` modification types and much more (priced toppings,
  half-and-half pizzas, "pick 3 of 8 sauces", size variants) without
  special-cased shapes, and drops the conditional (`if`/`then`) constraints.

### Migration

- `type: "option"` modification → a group with the same options
  (`is_required` → `min_selections: 1`).
- `type: "addition"` → an option (with its `price_adjustment`) in an
  "Extras" group.
- `type: "removal"` → a zero-priced option in a "Remove" group.
- `type: "substitution"` → a `min_selections: 1, max_selections: 1` group with
  the original as the `default` option and the substitute as the alternative.

## [0.2.0] - 2026-07-16

### Added

- `openfood_version` (required): documents declare which schema version they conform to
- `restaurant.description` — localized
- `restaurant.location` is now structured: `address`, `city`, `country` (ISO 3166-1), `postal_code`, and `geo` (`lat`/`lng`) for map placement
- `restaurant.default_language` and `restaurant.languages` — explicit translation fallback and offered languages
- `restaurant.cuisines` — cuisine styles for discovery and filtering
- `restaurant.contact` (`phone`, `email`, `website`)
- `restaurant.links` — links to external platforms (e.g. `google_maps`, `tripadvisor`)
- `restaurant.hours` — opening hours per weekday with support for split shifts
- `restaurant.image_url` — cover image; `restaurant.logo_url` — logo
- Top-level `categories` array with localized names and `sort_order`; dishes reference them via `category_id`
- `menu[].sort_order` — display position within a category
- `menu[].contains_alcohol` boolean
- Shared `definitions` (`localizedText`, `allergens`, `timeRange`, …) — language tags now allow optional region subtags (e.g. `pt-BR`)

### Changed

- **Breaking:** `restaurant.name` is now a localized object (`{ "en": "…" }`), consistent with dish names
- **Breaking:** `restaurant.language` replaced by `default_language` + `languages`
- **Breaking:** `menu[].category` (free-form string) replaced by `category_id` referencing the `categories` array
- `restaurant.currency` must be an ISO 4217 code; `country` must be ISO 3166-1 alpha-2
- `schemas/latest` is a real directory (copy of the newest schema) instead of a symlink, so the package works on all platforms

## [0.1.0] - 2025-05-26

- Initial release
