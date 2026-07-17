# Changelog

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
