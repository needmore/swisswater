# New Dawn Theme Implementation Checklist
## Swiss Water Theme Rebuild - Feature Requirements

Based on comprehensive review of current theme, here's a prioritized checklist for your new Dawn-based theme implementation.

---

## Critical Business Features (Must-Have)

### 1. Trade/B2B Wholesale System
- [ ] Customer tag-based access control (`customer.tags contains 'Trade'`)
- [ ] Trade-only page templates and sections
- [ ] Redirect non-Trade customers from restricted content
- [ ] Trade navigation system (separate nav for wholesale customers)
- [ ] Customer metafields for company info (`customer.metafields.cf_app['company_name']`)

### 2. Brand Materials Ordering System
- [ ] **Custom P3PL API integration** - `https://swisswater-api.herokuapp.com/p3pl`
  - Multi-variant quantity selector
  - Shipping address form (pre-populated from customer data)
  - Country-based shipping method mapping (US: UPSG, Canada: UPSWSTD)
  - AJAX submission to external fulfillment API
- [ ] Slick carousel for product images
- [ ] Access restricted to Trade customers only

### 3. Subscription System Integration
**Choose ONE** (currently running both):

**Option A: Bold Subscriptions** (appears to be primary)
- [ ] Bold Subscriptions app installed
- [ ] `bsub-widget.liquid` snippet - Custom subscription widget UI
- [ ] `bsub.js` - Widget JavaScript logic
- [ ] `bsub.scss.css` - Widget styles
- [ ] Product page: Selling plan selector with radio buttons
- [ ] Cart: Gift subscription validation (prevent mixing with regular items)
- [ ] Cart: Login requirement for subscription items
- [ ] Customer account: Subscription portal integration
- [ ] Variant change listeners to update pricing

**Option B: PayWhirl**
- [ ] PayWhirl app integration
- [ ] Custom snippets for cart/product pages

**Key Subscription Features to Preserve:**
- One-time purchase vs. subscription toggle
- Multiple frequency options (every 2 weeks, monthly, etc.)
- Per-delivery pricing display
- Discount summary for subscription groups
- Gift subscription capability with modal warnings

### 4. Coffee Product Line Structure
- [ ] Four main product line landing pages (Emerald, Evergreens, Ferns, Verdant Reserves)
- [ ] Individual origin pages (13 total - can template these dynamically)
- [ ] Product custom metafields (10+ fields):
  - `roast_level`, `grind`, `origin`, `certifications`
  - `region`, `growing_altitude`, `variety`, `cup`
  - `country`, `roaster_page`, `roaster_ship_day`
- [ ] Sample request forms per product line
- [ ] Product line color schemes (green, sand, default, red)

---

## Third-Party Integrations Requiring Custom Code

### 5. Globo Form Builder
**Decision Point:** Do you still want this? (45+ JavaScript files is heavy)

If YES:
- [ ] Globo Form Builder app installed
- [ ] `globo.formbuilder.scripts.liquid` snippet
- [ ] Form embeds with `data-id` attributes in multiple sections
- [ ] 8 different form IDs to migrate:
  - Emerald sample (MTI1ODgx)
  - Verdant sample (MTI1ODgy)
  - Evergreens main (MTI1ODc5)
  - Sample popup (MTI1OTIy)
  - Ferns main (MTI1ODgw)
  - Webinar (MTE5MzMy)
  - Small Batch Inquiry (31216)
  - European Sample Request (31224)

If NO:
- [ ] Replace with native Shopify forms or lighter alternative
- [ ] Rebuild contact forms
- [ ] Rebuild sample request forms

### 6. WLM (Wholesale Locksmith) Integration
**Decision Point:** Still needed or use native Shopify B2B/Customer Accounts?

If YES:
- [ ] WLM app installed
- [ ] `wlm-head.liquid` snippet
- [ ] `wlm-body.liquid` snippet
- [ ] Password form for locked content
- [ ] Price hiding for unauthorized users
- [ ] Link removal logic
- [ ] Redirect URL configuration

If NO:
- [ ] Implement native Shopify B2B or Customer Accounts 2.0
- [ ] Build custom access control with customer tags

### 7. Google Maps Store Locator
- [ ] Google Maps API key setup
- [ ] `maps.js` - Store locator JavaScript (~340 lines)
- [ ] CSV location data import (or migrate to better format)
- [ ] **Custom features to implement:**
  - Distance calculation (Haversine formula)
  - Geocoding for address search
  - Custom map styling (brand colors)
  - Marker filtering by category
  - Info windows with store details
  - Dynamic marker loading based on viewport
  - Result list synchronized with map
  - Geolocation API for user position
- [ ] Custom zoom controls
- [ ] Search input with geocoding

### 8. Origin Lot Traceability API
- [ ] **Swiss Water API integration** - `https://swisswater-api.herokuapp.com/assets/producers.json`
- [ ] Production ID lookup input
- [ ] JavaScript fetch with caching
- [ ] Result display showing:
  - GreenLot, Country, Region, CoffeeName
  - Process, Certificates, ProductionDate
  - FLOID, Supplier, Relationship Years, DecafProcess
- [ ] Loading state UI
- [ ] "No Results" fallback

---

## Analytics & Tracking

### 9. Analytics Implementation
- [ ] **Google Tag Manager** - Container ID: GTM-KWC274
- [ ] **Google Analytics 4** - Measurement ID: G-ZBT7FF3MWM
- [ ] **Data Layer** with customer information:
  ```javascript
  {
    userType: "member" or "visitor",
    customer: {
      id, lastOrder, orderCount, totalSpent, tags
    }
  }
  ```
- [ ] **Enhanced E-commerce tracking:**
  - Product view events (`view_item`)
  - Add to cart tracking
  - Product data includes: name, id, price, brand, category, variant

**Decision:** Remove Universal Analytics (UA-2831558-1) - it's deprecated

---

## Product & Collection Features

### 10. Collection Filtering
- [ ] Client-side JavaScript filtering by:
  - Roast level
  - Origin
  - Grind
  - Certifications
- [ ] Dynamic filter generation from product metafields
- [ ] Special handling for:
  - Gift cards (different spacing)
  - Subscription packs
  - Brand materials (Trade-only redirect)
- [ ] Pagination (18 products per page)

### 11. Product Page Enhancements
- [ ] Variant swatches with radio buttons
- [ ] Custom field display section
- [ ] Access control for Brand Materials products
- [ ] Related products section
- [ ] Gift card special handling
- [ ] Subscription widget integration
- [ ] GTM tracking on product view

### 12. Cart Features
- [ ] Gift subscription modal warning
- [ ] Prevent mixing gift subscriptions with other items
- [ ] Login requirement for subscription items
- [ ] Roaster ship day display from metafields
- [ ] Bold Subscriptions cart integration

---

## Content & Educational Features

### 13. Educational Resources Section
- [ ] Video grid with Vimeo integration
- [ ] **Vimeo thumbnail API** - `https://vumbnail.com/{vimeo_id}`
- [ ] PDF guide links (7 languages for roasting guides)
- [ ] Responsive grid layout
- [ ] Play icon overlays

### 14. Process Section (Interactive)
- [ ] **Slick carousel** for multi-step process
- [ ] Desktop: Linked navigation + content carousel
- [ ] Mobile: Single carousel with dots
- [ ] Share functionality (copy-to-clipboard)
- [ ] Infographic download (toDataURL blob conversion)
- [ ] Visual legend with icons
- [ ] 5-step process display

### 15. Team Section
- [ ] Team member modal display
- [ ] Department navigation with anchors
- [ ] Mobile dropdown department filter
- [ ] 7 department categories
- [ ] Modal with full bio on click

### 16. Partner Roasters Carousel
- [ ] Slick carousel implementation
- [ ] Config: 4 slides desktop, 2 slides mobile
- [ ] Custom SVG arrow navigation
- [ ] Infinite loop
- [ ] Logo + URL + country data

---

## Blog Features

### 17. Blog Enhancements
- [ ] Category filtering (tags starting with capital letters)
- [ ] Archive date filtering via URL parameters
- [ ] 8 articles per page
- [ ] Featured image display
- [ ] Tag parsing logic

**Optional:**
- [ ] Elfsight widget integration (if needed)

---

## Settings & Configuration

### 18. Theme Settings
- [ ] **Trade Page Links** settings group:
  - Roaster's Toolkit URL
  - Foodservice Toolkit URL
  - Specialty Consumer Toolkit URL
  - Social Media Kit URL
  - Dropbox Images Link URL

- [ ] **Education Resources** settings group:
  - Roasting guides (7 languages)
  - Brewing guide
  - 5 video URLs

### 19. Customer Account Enhancements
- [ ] Address management modal
- [ ] Order history (50 per page)
- [ ] Subscription management tab
- [ ] Company info display (from metafields)

---

## Optional/Nice-to-Have

### 20. Webinar System
- [ ] Webinar landing page
- [ ] Signup form integration
- [ ] Contact form

### 21. Campaign Pages
- [ ] DecafMan campaign template (if still active)
- [ ] Decaf Project partnership page
- [ ] Small Batch Series page

### 22. FAQ Section
- [ ] Accordion-style FAQ
- [ ] One-open-at-a-time behavior
- [ ] jQuery toggle logic (or replace with native HTML `<details>`)

---

## Performance & Modern Best Practices

### 23. Updates from Legacy Theme
- [ ] Replace jQuery 1.12.4 with vanilla JS or keep Dawn's minimal approach
- [ ] Remove IE11 polyfills
- [ ] Optimize/bundle JavaScript (Dawn handles this)
- [ ] Use Dawn's native section rendering
- [ ] Implement lazy loading for images
- [ ] Use modern image formats (WebP)

### 24. Code Quality
- [ ] No inline JavaScript in templates
- [ ] No inline CSS in templates
- [ ] Use Dawn's CSS custom properties
- [ ] Implement proper accessibility (ARIA labels, focus management)
- [ ] Test keyboard navigation

---

## Prioritization Recommendation

### Phase 1 (MVP - Launch Blockers):
- Items 1-4: Trade system, subscriptions, product structure
- Items 9-12: Analytics, products, collections, cart
- Item 5 or alternative: Forms (decide on Globo vs. native)

### Phase 2 (Core Features):
- Items 7-8: Store locator, traceability
- Items 13-16: Educational content, process, team, partners
- Item 18: Theme settings

### Phase 3 (Polish):
- Items 17, 19-22: Blog, account enhancements, optional features
- Items 23-24: Performance and code quality

---

## Key Decisions Needed Before Starting:

1. **Subscriptions:** Bold or PayWhirl? (Recommend Bold based on current primary usage)
2. **Forms:** Keep Globo (45+ JS files) or replace with lighter solution?
3. **Access Control:** Keep WLM or use native Shopify B2B?
4. **Location Data:** Migrate from CSV to JSON/metaobjects or keep CSV?
5. **Analytics:** GA4 only or keep GTM + GA4?

---

## Notes

This checklist covers everything custom that won't be in Dawn by default. Each item represents functionality that requires either:
- Custom Liquid code
- Custom JavaScript
- Third-party app integration with theme customization
- External API integration
- Custom section development

Refer to `docs/current-theme-state.md` for detailed implementation information on each feature, including file paths, code samples, and integration patterns from the current theme.
