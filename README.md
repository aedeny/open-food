# 🥗 OpenFood Menu Format

**OpenFood** is an open, structured, and multilingual format for restaurant menus. It standardizes how food items are described—so that customers, apps, and restaurants can share, filter, and understand menus effortlessly across languages, platforms, and dietary needs.

---

## 📌 Why OpenFood?

Restaurant menus today are inconsistent, language-dependent, and not machine-readable. OpenFood changes that by offering:

✅ A universal, open format for describing food  
✅ Multilingual dish names and descriptions  
✅ Nutrition facts and allergen data  
✅ Ingredient lists with quantities  
✅ Smart filtering for dietary needs  
✅ Seamless ordering via QR code or app integration

---

## 🌍 Use Cases

- **Travelers**: Understand menus and order food in any language.
- **Diet-conscious users**: Track calories, macros, or avoid allergens.
- **Restaurants**: Standardize menus across platforms and languages.
- **Health & nutrition apps**: Parse food data and sync with diaries.
- **Ordering platforms**: Integrate clean, structured data directly.

---

## 🧠 How It Works

Restaurants create a `.json` menu file following the OpenFood schema. Apps and tools can parse it, render it visually, or convert it into QR codes for fast access or ordering.

---

## 🔧 Technical Details

### File Format

- **JSON**
- UTF-8 encoded
- Follows the [latest OpenFood Schema](./schemas/latest/openfood.schema.json) (versioned copies live in [`schemas/`](./schemas))

### Top-Level Structure

```json
{
  "openfood_version": "0.6.0",
  "restaurant": { ... },
  "services": [ ... ],
  "categories": [ ... ],
  "menu": [ ... ]
}
```

### Several menus

A kitchen that serves lunch, dinner and a bar list does not have one menu with
three sections; it has three menus, and the same room prints all of them.
`services` names them, and categories and dishes say which they are printed in:

```json
{
  "services": [
    {
      "id": "lunch",
      "name": { "en": "Lunch" },
      "hours": { "fri": [{ "start": "12:00", "end": "15:00" }] }
    },
    { "id": "bar", "name": { "en": "Bar" } }
  ],
  "categories": [{ "id": "c2", "name": { "en": "Mains" }, "service_ids": ["lunch"] }],
  "menu": [{ "id": "d1", "name": { "en": "Pad Thai" }, "price": 52, "service_ids": ["lunch"] }]
}
```

Resolution is three steps and no more: a dish is printed in the services its own
`service_ids` names; failing that, in the ones its category names; failing both,
in every service. An id naming a service the document does not declare is
ignored, and an empty array reads as an absent one, so no arrangement of these
fields can produce a dish that appears nowhere.

Every dish stays in the one flat `menu` array, so a consumer that ignores
`services` reads exactly the menu it read before, and every question answered
from the whole menu — allergens, diets, search, the price range — keeps its
answer. `hours` is optional: a bar or children's menu is a menu, not a sitting.

A dish that costs less at lunch is **two entries with two ids**, not one entry
with two prices — an order line resolves by dish id. See
[`examples/v0.6.0-example.json`](./examples/v0.6.0-example.json).

### Example

```json
{
  "openfood_version": "0.2.0",
  "restaurant": {
    "id": "r123",
    "name": { "en": "Green Garden", "he": "גן ירוק" },
    "location": {
      "address": "Rothschild Blvd 12",
      "city": "Tel Aviv",
      "country": "IL",
      "geo": { "lat": 32.0632, "lng": 34.7719 }
    },
    "currency": "ILS",
    "default_language": "en",
    "languages": ["en", "he"]
  },
  "categories": [
    { "id": "c2", "name": { "en": "Main Dishes", "he": "עיקריות" }, "sort_order": 1 }
  ],
  "menu": [
    {
      "id": "d1001",
      "name": { "en": "Vegan Buddha Bowl", "he": "קערת בודהה טבעונית" },
      "description": {
        "en": "Quinoa, chickpeas, roasted veggies, tahini dressing"
      },
      "category_id": "c2",
      "price": 48.0,
      "nutritional_info": {
        "calories": 520,
        "protein": 18,
        "fat": 20,
        "carbohydrates": 60,
        "sugar": 6
      },
      "allergens": ["sesame"],
      "tags": ["vegan", "gluten-free"],
      "ingredients": [
        { "name": "quinoa", "amount_grams": 150 },
        { "name": "chickpeas", "amount_grams": 100 },
        { "name": "tahini", "amount_grams": 30 }
      ]
    }
  ]
}
```

See the full example at [`examples/v0.2.0-example.json`](./examples/v0.2.0-example.json).

---

## ✅ Supported Tags

Dish-level `tags`:

- `vegan`
- `vegetarian`
- `gluten-free`
- `nut-free`
- `dairy-free`
- `organic`

Restaurant-level `certifications`: `kosher`, `halal`.

> ⚠️ Subjective tags (e.g. `low-sugar`) are **excluded**. Use `nutritional_info` instead and let apps decide how to interpret it.

---

## 📐 Schema File

The latest schema is available at:

[`schemas/latest/openfood.schema.json`](./schemas/latest/openfood.schema.json)

You can validate OpenFood menu files using any JSON Schema validator.

### Using as an npm dependency

```json
"dependencies": {
  "openfood-schema": "github:aedeny/open-food#v0.2.0"
}
```

---

## 📦 Developer Tools

- Use [jsoneditoronline.org](https://jsoneditoronline.org) or [rjsf playground](https://rjsf-team.github.io/react-jsonschema-form/) to edit menus visually.
- Use VS Code with the JSON Schema extension for autocomplete and validation.
- Integrate OpenFood menus into your apps with a few lines of JSON parsing.

---

## 🔄 Roadmap

- ✅ Schema v1.0
- 🔜 Hosted form editor for restaurants
- 🔜 OpenFood app (cross-platform) for customers
- 🔜 Restaurant dashboard + QR code generator
- 🔜 Schema extensions: sustainability tags, regional sourcing

---

## 🙌 Get Involved

This standard is open source. Contributions, feedback, and collaborators are welcome!

- Submit an issue or pull request
- Build an OpenFood-compatible app
- Help onboard restaurants

---

## 📄 License

OpenFood is licensed under the [MIT License](./LICENSE).

---

**OpenFood** — making food menus transparent, accessible, and universal.
