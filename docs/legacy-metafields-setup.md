# Existing Metafields Setup - Bonify Custom Fields

## Overview

This Shopify theme uses **Bonify Custom Fields** (by Bonify, LLC), a third-party Shopify app for managing product metafields. This is **not** using native Shopify metafields, but rather a proprietary system managed through the Bonify app dashboard.

## App Information

- **App Name**: Bonify Custom Fields
- **Vendor**: Bonify, LLC
- **Dashboard**: https://customfields.bonify.io/dashboard/swisswater.myshopify.com/
- **Support**: appsupport@bonify.io
- **CDN**: https://cdn.apps.bonify.io/

## Metafield Namespace

All custom fields are stored under the `custom_fields` namespace and accessed via:

```liquid
{{ product.metafields.custom_fields["field_name"] }}
```

## Current Product Fields

| Field Name | Purpose | Example Usage |
|------------|---------|---------------|
| `roast_level` | Roast level information | Light, Medium, Dark |
| `grind` | Grind type | Whole Bean, Ground |
| `country` | Country of origin | Ethiopia, Colombia |
| `region` | Regional information | Yirgacheffe, Huila |
| `origin` | Specific origin details | Single-origin estate |
| `cup` | Cup profile/tasting notes | Bright, Fruity, Chocolate |
| `roaster_page` | URL to roaster page | Link to roaster details |
| `roaster_ship_day` | Shipping schedule | "Tuesdays", "Thursdays" |
| `certifications` | Product certifications | Organic, Fair Trade |
| `growing_altitude` | Altitude information | 1200-1800m |
| `variety` | Coffee variety | Arabica, Bourbon |

## Auto-Generated Snippets

Bonify generates snippet files in `/snippets/` for each custom field:

- `custom_fields.products.roast_level.liquid`
- `custom_fields.products.grind.liquid`
- `custom_fields.products.country.liquid`
- `custom_fields.products.region.liquid`
- `custom_fields.products.cup.liquid`
- `custom_fields.products.origin.liquid`
- `custom_fields.products.roaster_page.liquid`
- `custom_fields.products.roaster_ship_day.liquid`

### Important Warning

Each snippet includes this warning:

```liquid
{% comment %}
  DO NOT MODIFY THIS FILE DIRECTLY. THIS FILE WILL BE OVERWRITTEN.
  YOU CAN MODIFY THE LIQUID CODE FOR THIS CUSTOM FIELD HERE:
  https://customfields.bonify.io/dashboard/swisswater.myshopify.com/products/configure/[ID]/edit
{% endcomment %}
```

## Implementation Locations

### Product Templates

**Primary Product Template** (`templates/product.liquid:62-123`)
```liquid
<p class='vendor'>
  <a href="{{ product.metafields.custom_fields['roaster_page'] }}">
    {{ product.vendor }}
  </a>
</p>

{% if product.metafields.custom_fields["roast_level"] %}
  <div class="product-field">
    <span>Roast Level: </span>
    {{ product.metafields.custom_fields["roast_level"] }}
  </div>
{% endif %}
```

**Swatch Product Template** (`templates/product.swatch-product.liquid:62-123`)
- Similar implementation with additional fields like `certifications`, `growing_altitude`, `variety`

**Migrate Template** (`templates/product.migrate.liquid:30-61`)
- Legacy template with field display

### Product Cards

**Product Card Snippet** (`snippets/product-card.liquid:11-40`)
```liquid
<ul class="product-fields">
  {% if product.metafields.custom_fields["roast_level"] != blank %}
    <li class="product-field-item">
      <span class="field-label">Roast Level: </span>
      {{ product.metafields.custom_fields["roast_level"] }}
    </li>
  {% endif %}
  <!-- Additional fields... -->
</ul>
```

### Collection Pages

**Standard Collection** (`templates/collection.liquid:78`)
```liquid
m: {{ product.metafields.custom_fields | json }}
```

**Alternate Collection** (`templates/collection.alternate.liquid:64`)
- Similar debug output

### Cart & Orders

**Cart Template** (`templates/cart.liquid:124-125`)
```liquid
{% if item.product.metafields.custom_fields['roaster_ship_day'] %}
  <p class='ship'>
    *Product ships from roaster on {{ item.product.metafields.custom_fields['roaster_ship_day'] }}
  </p>
{% endif %}
```

**Order Template** (`templates/customers/order.liquid:35-36`)
- Similar shipping day display

### Roaster Pages

**Roaster Template** (`templates/page.roaster.liquid:34-61`)
- Displays all relevant fields for roaster-specific products

### Related Products

**Related Products Snippet** (`snippets/related-products.liquid:16`)
- Debug output of metafields

**Coffee Quiz** (`snippets/coffee-quiz.liquid:21`)
- Metafields used for product matching/filtering

## Customer Fields Integration

Bonify also provides customer field functionality:

**Customer Registration** (`snippets/cf-app.customers.register.liquid:6-24`)
```liquid
<script src="https://cdn.apps.bonify.io/customer_fields/public/live/CustomerFieldsApp-min.js"></script>
<script>
  BonifyCustomerFields.init({
    shop_name: "{{ shop.permanent_domain }}",
    base_url: "https://apps.bonify.io/",
    styles_url: "https://cdn.apps.bonify.io/customer_fields/public/live/cf-app-form.css",
    // ...
  });
</script>
```

## Advantages of Current Setup

1. **UI Management**: Fields can be managed through Bonify dashboard
2. **Auto-generation**: Snippet code is automatically maintained
3. **Customer Fields**: Also provides customer-level custom fields

## Disadvantages

1. **Third-party Dependency**: Requires ongoing Bonify subscription
2. **Limited to Bonify API**: Can't leverage native Shopify metafield features
3. **No Native Admin UI**: Not integrated with Shopify's native metafield interface
4. **Potential Lock-in**: Migration requires data export/import
5. **Code Overwriting**: Direct snippet modifications get overwritten by app

## Migration Considerations

To migrate to native Shopify metafields, the following would be required:

1. **Export Data**: Extract all custom field data from Bonify system
2. **Define Metafields**: Create native metafield definitions in Shopify
3. **Import Data**: Populate native metafields with exported data
4. **Update Templates**: Modify all Liquid code to use native metafield syntax
5. **Testing**: Verify all displays and functionality work correctly

## Related Files

### Configuration Files
- `snippets/products.custom_fields.liquid` - Main wrapper including all field snippets

### Debug/Development
- Multiple templates output metafields as JSON for debugging (search for `| json`)

### Third-party Integration
- Bonify CDN resources loaded for customer field functionality
- App initialization in customer registration flow
