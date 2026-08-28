# Changelog

## [0.5.0] - 2026-08-28

### Added

- `menu[].option_groups[].sections[]` and `menu[].option_groups[].options[].section_id`
  — optional headings that divide one group's options into labelled lists,
  without dividing its selection rule.

  A dish sold with "two sides of your choice" whose menu then prints *Hot
  sides* and *Cold sides* had no honest representation. Two groups was the
  obvious reading and the wrong one: `max_selections` is per group, so two
  groups of 2 let a diner take four. The only correct encoding was a single
  group of every side with both headings thrown away, which is what consumers
  were told to do — and it loses the one thing the printing was there to say.

  A section is a label, not a container. `min_selections`, `max_selections` and
  `included_selections` stay the group's, so the allowance is shared across
  every section by construction: two sides may be two hot, two cold, or one of
  each, and there is no second place a count could live. Options stay in one
  flat `options` array and name their heading with `section_id`, so consumers
  that ignore sections keep working unchanged and read exactly the group they
  read before.

  An option with no `section_id`, or one naming a section the group does not
  declare, is listed before the first heading rather than dropped — a label is
  never a reason to lose a choice a diner can order.

  Fully backwards compatible in both directions: every 0.4.1 document is valid
  0.5.0, and a 0.5.0 document that uses neither field is valid 0.4.1. A
  consumer pinned to 0.4.1 will reject a document that uses them, since both
  objects are `additionalProperties: false`.

## [0.4.1] - 2026-08-27

### Changed

- `menu[].ingredients[].amount_grams` is no longer required. Only `name` is.

  The field was unusable as written. A printed menu names what is in a dish —
  "tomato, mozzarella, basil" — and never weighs it, so requiring a gram figure
  meant the only honest way to fill `ingredients` was to have weighed the dish
  yourself. In practice that meant nobody filled it at all: consumers extracting
  a menu from a photograph had to drop the ingredient list into `description` as
  prose, and a kitchen typing its own dishes in had to invent numbers or leave
  the field empty. A schema that can only be satisfied by data nobody has is a
  schema that gets skipped.

  Backwards compatible in the direction that matters: every 0.4.0 document is
  still valid 0.4.1, since a document that supplies `amount_grams` continues to
  validate. A 0.4.1 document that omits it will **not** validate against 0.4.0,
  so a consumer pinned to the older schema should move up before accepting one.
## [0.4.0] - 2026-08-18

### Added

- `menu[].advisories` and `option_groups[].options[].advisories` — preparation
  facts a diner may need to avoid: `alcohol`, `raw-fish`, `raw-shellfish`,
  `raw-meat`, `raw-egg`, `unpasteurized-dairy`, `cured-meat`,
  `high-mercury-fish`, `liver`, `sprouts`, `honey`. Each is something a kitchen
  can answer from what it buys and how it cooks. None of them is health advice:
  which matter to whom differs by country and by person, so groupings such as
  "avoid these during pregnancy" belong to the application and are deliberately
  not expressible here. `honey` is an infant advisory (botulism under 12
  months), not a pregnancy one.
- An **absent** `advisories` field and an **empty** one mean different things: no
  field means nobody checked, an empty array means the kitchen checked and none
  apply. Consumers must not read absence as safety.
- Options carry advisories as well as dishes, because a diner can order the
  topping — the same reason options already carry `allergens`.

### Deprecated

- `menu[].contains_alcohol` — use `advisories: ["alcohol"]`. Still accepted, and
  not scheduled for removal: dish objects are `additionalProperties: false`, so
  dropping the property would invalidate every 0.3.0 document that carries it,
  including documents held by consumers this repo does not control.

### Migration

- `contains_alcohol: true` → add `"alcohol"` to `advisories`.
- `contains_alcohol: false` → drop the field. It only ever meant "no alcohol";
  it never meant the dish had been checked against anything else, so it must not
  become an empty `advisories` array.

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
