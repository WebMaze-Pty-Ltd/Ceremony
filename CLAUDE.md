# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Theme Overview

This is **Pipeline v8.1.1**, a commercial Shopify theme by Groupthought.

## Customization Files

**CSS Changes:** All CSS customizations should go in `assets/custom.css`. This file is already loaded in `layout/theme.liquid`. Do not edit `assets/theme.css` as it's compiled from the theme source.

**JavaScript Changes:** Use `assets/custom.js` for custom JavaScript. Note: this file is commented out in `layout/theme.liquid` (lines 302-305) and must be uncommented to be used.

**Font Customizations:** The theme uses custom font definitions. Current setup:
- Body text: Helvetica Neue (300/500/700 weights)
- Headings: Amiri serif font
- Font faces are defined in `assets/custom.css` with Shopify Liquid filters (`font_url`)

## Architecture

### Block System
Blocks are organized by prefix indicating their function:
- `theme-*` - Core theme blocks (badge, button, countdown, heading, image, spacer, text, kicker, group-frame)
- `_pair-*` - Paired content layouts (accordion, before-after, content, hotspots, slideshow, showcase, title)
- `_background-*` - Background treatments (collage, marquee, pattern, split)
- `_text-spotlight`, `_*spotlight-*` - Text highlighting and auto-highlight effects
- `_marquee-*` - Marquee content types (custom-text, image, text)

### Section Groups
Sections are managed via JSON group files that control composition:
- `sections/group-header.json` - Header + Announcement sections
- `sections/group-footer.json` - Footer + Subfooter sections
- `sections/group-overlay.json` - Overlay sections (popups, modals)

These JSON files are **auto-generated** by the Shopify theme editor. Changes made directly may be overwritten.

### Core Snippets
Reusable components in `snippets/`:
- `core-*.liquid` - Core layout components (button, frame, group, heading, kicker, placement, text effects)
- `icon-*.liquid` - Icon sets (core icons, icon-art, icon-fill, icon-media)
- `cart-*.liquid` - Cart-related components
- `collection-*.liquid` - Collection filtering and sorting
- `css-variables.liquid` - CSS variable definitions for the theme
- `css-variables-contrast.liquid` - High-contrast CSS variables

### Configuration
- `config/settings_schema.json` - Theme settings schema (color, typography, layout, etc.)
- `config/settings_data.json` - **Auto-generated** theme settings data. Changes may be overwritten by theme editor.

### Locales
Internationalization files in `locales/`:
- `en.default.json` - Default English translations
- `de.json`, `es.json`, `fr.json`, `it.json`, `pt.json` - Translated locales

## Theme Events

The theme dispatches custom events for extension:
- `theme:variant:change` - When product variant changes (detail.variant available)
- `theme:cart:change` - When cart changes (detail.cart available)
- `theme:cart:init` - Fired on page load to initialize header cart values
- `theme:scroll` - Debounced scroll event
- `theme:scroll:up` - When scrolling up (direction change only)
- `theme:scroll:down` - When scrolling down (direction change only)
- `theme:resize` - Debounced resize event
- `theme:scroll:lock` - Lock page scroll for modals/drawers
- `theme:scroll:unlock` - Unlock page scroll

## CSS Variables

Theme uses CSS variables extensively, defined in `snippets/css-variables.liquid`:
- `--FONT-STACK-BODY`, `--FONT-STACK-HEADING`, `--FONT-STACK-ACCENT` - Font families
- `--color_*` - Color palette variables
- `--button_radius` - Button border radius
- Various spacing and sizing variables

Custom CSS can override these in `assets/custom.css` or via `:root` selector.

## RTL Support

The theme detects RTL languages from `request.locale.iso_code` and adds `dir="rtl"` to the html element. Check `window.isRTL` in JavaScript for RTL-aware code.

## App Integration

App blocks are registered in `settings_data.json` under `blocks` key. The current theme has:
- Judge Me Reviews (`judgeme_core`)
- Fast Bundle (`fast_bundle`)

## File Loading Order in theme.liquid
1. `font-settings.css` - Font preloads and settings
2. `css-variables` liquid render - CSS variable definitions
3. `theme.css` - Main theme stylesheet
4. `custom.css` - Custom stylesheet
5. `vendor.js` - Vendor libraries (deferred)
6. `theme.js` - Main theme JavaScript (deferred)
7. `custom.js` - Custom JavaScript (deferred, requires uncommenting)
