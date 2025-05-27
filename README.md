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
- Follows the [OpenFood Schema v1.0](./openfood.schema.json)

### Top-Level Structure

```json
{
  "restaurant": { ... },
  "menu": [ ... ]
}
```

### Example

```json
{
  "restaurant": {
    "id": "r123",
    "name": "Green Garden",
    "location": "Tel Aviv, Israel",
    "currency": "ILS",
    "language": "en"
  },
  "menu": [
    {
      "id": "d1001",
      "name": { "en": "Vegan Buddha Bowl", "he": "קערת בודהה טבעונית" },
      "description": {
        "en": "Quinoa, chickpeas, roasted veggies, tahini dressing"
      },
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

---

## ✅ Supported Tags

- `vegan`
- `vegetarian`
- `gluten-free`
- `halal`
- `kosher`
- `dairy-free`
- `nut-free`

> ⚠️ Subjective tags (e.g. `low-sugar`) are **excluded**. Use `nutritional_info` instead and let apps decide how to interpret it.

---

## 📐 Schema File

The latest schema is available at:

[`openfood.schema.json`](./openfood.schema.json)

You can validate OpenFood menu files using any JSON Schema validator.

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
