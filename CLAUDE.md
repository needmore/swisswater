# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Shopify theme based on **Dawn** (v15.4.0), Shopify's official reference theme. Dawn follows a "web-native, HTML-first, JavaScript-only-as-needed" philosophy with server-side rendering via Liquid templating and progressive enhancement.

## Development Commands

### Theme Development
- `shopify theme serve` - Launch local development server
- `shopify theme check` - Run Theme Check linter for Liquid/theme validation
- `shopify theme push` - Push theme to Shopify store
- `shopify theme pull` - Pull theme from Shopify store

### CI/CD
The repository uses GitHub Actions with two workflows:
- **Lighthouse CI** - Performance audits for home, product, and collection pages
- **Theme Check** - Automated linting on every push

## Architecture

### Theme Code Principles (from Dawn)

1. **Web-native First**: No frameworks or libraries. Pure HTML, CSS, and vanilla JavaScript using Web Components (Custom Elements). Avoid polyfills - use progressive enhancement instead.

2. **Server-Rendered with Liquid**: All HTML is rendered server-side using Shopify's Liquid templating language. Avoid client-side business logic or platform primitives (translations, money formatting).

3. **Performance Targets**:
   - Zero Cumulative Layout Shift (CLS)
   - No DOM manipulation before user input
   - No render-blocking JavaScript
   - No long tasks

4. **Functional, Not Pixel-Perfect**: Use semantic markup and progressive enhancement. Don't burden legacy browsers with polyfills.

### Directory Structure

- **`layout/`** - Theme layouts (theme.liquid, password.liquid)
- **`sections/`** - Reusable sections for Online Store 2.0 (announcement-bar.liquid, featured-product.liquid, etc.)
- **`snippets/`** - Reusable Liquid partials (~39 snippets including cart-drawer.liquid, product-thumbnail.liquid)
- **`templates/`** - Page templates in JSON format (product.json, collection.json, etc.)
- **`assets/`** - Static assets (CSS, JavaScript, images)
- **`config/`** - Theme configuration (settings_schema.json, settings_data.json)
- **`locales/`** - Translation files for internationalization

### JavaScript Architecture

JavaScript is organized as **Web Components** (Custom Elements). Each component extends `HTMLElement` and is registered with `customElements.define()`.

Examples:
- `cart-drawer.js` defines `CartDrawer` and `CartDrawerItems` components
- `product-form.js` defines product form interaction
- `details-modal.js` and `details-disclosure.js` handle disclosure patterns

Common patterns:
- Components attach event listeners in `constructor()`
- Use `trapFocus()` and `removeTrapFocus()` from `global.js` for accessibility
- Section rendering uses `getSectionsToRender()` and `getSectionInnerHTML()` patterns
- Dynamic content uses `fetch()` to Shopify APIs and updates DOM via `innerHTML`

### CSS Architecture

- **`base.css`** - Core styles (~78KB)
- Component-specific CSS files (e.g., `component-cart-drawer.css`, `component-card.css`)
- Styles are conditionally loaded based on settings (e.g., cart type, predictive search)

### Liquid Architecture

**Settings and Customization**:
- Theme settings are defined in `config/settings_schema.json` with extensive customization options for:
  - Color schemes (defined via `color_scheme_group`)
  - Typography (header and body fonts with Google Fonts integration)
  - Layout (page width, spacing, grid settings)
  - Component styles (buttons, cards, inputs, badges, etc.)
- Settings are accessed in Liquid via `{{ settings.property_name }}`
- CSS custom properties are generated dynamically from settings in `layout/theme.liquid`

**Online Store 2.0 Features**:
- Sections can be added/removed/reordered in theme editor
- JSON templates define page structure with section references
- Section groups (header-group, footer-group) for consistent layouts

**Translation System**:
- Translation keys use format `t:path.to.key`
- Default translations in `locales/en.default.json`
- Additional languages supported (cs, da, de, el, bg, etc.)

### Theme Configuration

**Theme Check** (`.theme-check.yml`):
- `MatchingTranslations`: disabled
- `TemplateLength`: disabled

These rules are intentionally disabled in Dawn's base configuration.

## Important Conventions

### Liquid Best Practices
- Use `{%- liquid ... %}` tag for multi-line Liquid logic blocks
- Use `{% comment %}...{% endcomment %}` for comments
- Use `render` for snippets: `{% render 'snippet-name' %}`
- Server-rendered content via `{{ content_for_layout }}`, `{{ content_for_header }}`

### JavaScript Best Practices
- Always use vanilla JavaScript - no jQuery, no frameworks
- Define Web Components for interactive elements
- Use `defer` attribute on all script tags
- Leverage `fetch()` for AJAX, not XMLHttpRequest
- Use `DOMParser()` for parsing HTML strings

### CSS Best Practices
- Use CSS custom properties (CSS variables) defined in theme.liquid
- Mobile-first responsive design with `@media screen and (min-width: 750px)`
- Leverage CSS Grid and Flexbox (no floats)
- Follow Dawn's naming conventions (BEM-like patterns)

### Accessibility Requirements
- Proper ARIA attributes on interactive elements
- Focus management with `trapFocus()` / `removeTrapFocus()`
- Keyboard navigation support (Escape key, Space key, etc.)
- Screen reader announcements for dynamic content changes

## Common Development Patterns

### Adding a New Section
1. Create `.liquid` file in `sections/` directory
2. Define section schema with `{% schema %}` JSON block
3. Add section to template JSON files as needed
4. Section automatically appears in theme editor

### Modifying Cart Behavior
- Cart drawer: `snippets/cart-drawer.liquid` + `assets/cart-drawer.js`
- Cart notification: `sections/cart-notification-*.liquid` + `assets/cart-notification.js`
- Cart page: `templates/cart.json` + `sections/main-cart-*.liquid`
- Cart type controlled by `settings.cart_type` setting

### Working with Product Data
- Product sections render via section rendering API
- Use `product.metafields` for custom data
- Variant selection handled by `product-form.js` and `product-info.js`
- Media gallery in `media-gallery.js` with accessibility support

### Internationalization
- Add translation keys to appropriate locale files
- Reference via `{{ 'key.path' | t }}`
- Fallback to default locale if translation missing
- Use `localization-form.js` for country/language selectors
