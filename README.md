# Swiss Water 2025 Shopify Theme

Swiss Water 2025 is a modern theme based on Dawn and optimized for performance, accessibility, and user experience. It is designed to be fully compatible with Shopify's Online Store 2.0 features. The theme is built by Needmore Designs.

## Development Plan

> Build on a **fresh Dawn fork in the client’s account**. You’ll keep all existing data, avoid legacy debt, and give the client a maintainable, 2.0-native theme with structured content they can actually manage.

1. **Spin up a Dawn fork (unpublished)**
    - In the client’s store, add **Dawn** → create a new GitHub repo → connect that branch to an **unpublished** theme.
    - This keeps work inside their account (real data), while live customers only see the published theme.
2. **Inventory the existing data model**
    - List current **metafield definitions** (products, collections, pages, articles), **any metaobjects**, and app-owned data you must keep.
    - Note legacy “custom page templates” that are really content—plan to replace with **metafields/metaobjects + sections**.
3. **Model first, code second**
    - Add **new** schema additively (e.g., coffee.v2.*) rather than renaming/deleting keys the live theme uses.
    - Create the **metaobjects** you’ll need (e.g., Music Pairing, FAQs, Logos, Process Steps), then wire Dawn sections to them.
4. **Build theme features as sections/blocks**
    - Create reusable sections tied to those fields; expose **dynamic sources** so editors pick content in the Theme Editor.
    - Keep all new templates/sections in the Dawn fork; the live theme can’t “see” them.
5. **Content rehearsal**
    - Populate a handful of **staging products/pages** with the new fields.
    - Assign your **new JSON templates** to only those staging items and share the **preview link** for review.
6. **Cutover plan**
    - Short content freeze window.
    - Bulk-assign new templates to the appropriate items (collections/products/pages).
    - Publish the Dawn-based theme. Roll out in phases if helpful (e.g., start with a collection group).

## Metafields

### Current Product Fields (in Bonify)

These 11 fields have been defined in Shopify now, so we can migrate away from Bonify in the future, so until the new theme launches, it would be best not to change their definitions in Bonify. **Continue to edit the values of these custom fields in Bonify until we switch to the new theme.**

| Field Name | Description | Field Type | Defined in Shopify |
| --- | --- | --- | --- |
| `roast_level` | The roast profile of the coffee (e.g., Light, Medium, Dark) | Single line text | `custom_fields.roast_level` |
| `grind` | How the coffee is processed (e.g., Whole Bean, Ground, Espresso Grind) | Single line text | `custom_fields.grind` |
| `country` | The country where the coffee originates (e.g., Ethiopia, Colombia, Brazil) | Single line text | `custom_fields.country` |
| `region` | The specific growing region within the country (e.g., Yirgacheffe, Huila, Antigua) | Single line text | `custom_fields.region` |
| `origin` | Detailed origin information such as farm, estate, or cooperative name | Single line text | `custom_fields.origin` |
| `cup` | Flavor characteristics and tasting notes (e.g., Bright, Fruity, Chocolate, Nutty) | Single line text | `custom_fields.cup` |
| `roaster_page` | Link to the roaster's website or profile page for additional information | Single line text | `custom_fields.roaster_page` |
| `roaster_ship_day` | Days of the week when the roaster ships orders (e.g., "Tuesdays", "Thursdays") | Single line text | `custom_fields.roaster_ship_day` |
| `certifications` | Official certifications held by the coffee (e.g., Organic, Fair Trade, Rainforest Alliance) | Single line text with validation (5 preset choices) | `custom_fields.certifications` |
| `growing_altitude` | Elevation range where the coffee is grown (e.g., 1200-1800m, 4000-6000ft) | Single line text | `custom_fields.growing_altitude` |
| `variety` | The botanical variety or cultivar of the coffee plant (e.g., Arabica, Bourbon, Typica, Geisha) | Single line text | `custom_fields.variety` |

## Metaobjects

### Roasters (if we decide to use them)

Why Metaobject for Roasters:

1. Rich, structured content - Define fields like:
	- Name
	- Story/description
	- Location
	- Founded date
	- Images/logo
	- Website URL
	- Contact info
	- Roasting philosophy
2. Reusability - One roaster entry can be connected to multiple products without duplicating data
3. Dedicated pages - Metaobjects can have their own pages with custom URLs (e.g., /pages/roasters/intelligentsia)
4. Easy maintenance - Update roaster info once, it reflects everywhere

Implementation Pattern:

1. Create Roaster metaobject definition in Shopify Admin → Content → Metaobjects
2. Add metafield to Products that references the Roaster metaobject (type: metaobject reference)
3. Create template in your theme:
	- templates/metaobject/roaster.json - for individual roaster pages
	- sections/main-roaster.liquid - to display roaster content
4. List all roasters - Create a section that queries all roaster entries
5. Display on product pages - Access via {{ product.metafields.custom.roaster }} and render roaster info
