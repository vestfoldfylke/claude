---
name: generate-designsystemet-docs
description: >
  Generates a fresh components.md reference for @digdir/designsystemet by fetching live docs from GitHub. Use this skill when the user wants to update or regenerate the Designsystemet component reference — e.g. after a new release, to sync another skill's references/components.md, or any time they mention "update designsystemet", "regenerate components.md", or "sync designsystemet docs".
---

# Designsystemet Skill

Fetch live documentation from digdir/designsystemet on GitHub and write a
structured `components.md` that other skills (e.g. `web-prototype`) can use
as a component implementation reference.

## Steps — follow in order

### 1. Fetch the CSS index to discover all components

```
GET https://raw.githubusercontent.com/digdir/designsystemet/main/packages/css/src/index.css
```

Parse every `@import url('./X.css')` line to get the full component list.
This is the ground truth — do not hardcode component names.

### 2. Fetch supporting sources in parallel

```
GET https://raw.githubusercontent.com/digdir/designsystemet/main/packages/web/README.md
GET https://raw.githubusercontent.com/digdir/designsystemet/main/packages/types/src/types.ts
```

### 3. Fetch every component CSS file

For each component name discovered in step 1:
```
GET https://raw.githubusercontent.com/digdir/designsystemet/main/packages/css/src/<name>.css
```

From each file extract:
- CSS class selectors (`.ds-*`)
- `data-*` attribute selectors and their values

### 4. Write components.md

Try output locations in this order — use the first that works:

1. Path explicitly given by the user (e.g. another skill's `references/` folder)
2. `references/components.md` inside this skill's own directory
3. Print the full content in the chat if no filesystem write is available

### components.md structure

````markdown
# Designsystemet Components Reference
> Auto-generated {ISO timestamp}
> Source: digdir/designsystemet @ main

---

## Setup & Installation

```bash
npm install @digdir/designsystemet-css @digdir/designsystemet-web @digdir/designsystemet-theme
```

### Imports (once, in layout/entry point)
```js
import '@digdir/designsystemet-theme';  // design tokens
import '@digdir/designsystemet-css';    // component styles
import '@digdir/designsystemet-web';    // web components + observers
```

### TypeScript
```json
{ "compilerOptions": { "types": ["@digdir/designsystemet-web"] } }
```

---

## Types
(paste exported types from types.ts verbatim)

---

## Component Reference

For each component (in import order from index.css):

### <component-name>

**CSS classes:** `ds-foo`, `ds-foo__bar`, …
**data-* attributes:** (every attribute+value combination seen in the CSS)
**Usage:**
```html
(minimal correct HTML example inferred from selectors and web README)
```

---

## Web Components & Behaviors
(paste the full content of packages/web/README.md verbatim — includes ds-field,
ds-tabs, ds-breadcrumbs, ds-pagination, ds-suggestion, ds-error-summary,
polyfills, data-tooltip, data-toggle-group, etc.)

---

## Key Patterns

- **Variants via data-attrs, not BEM:** `data-variant="secondary"` not `ds-button--secondary`
- **Size inheritance:** set `data-size="sm"` on a container, all children inherit
- **Always wrap inputs in `<ds-field>`** for correct label/error ARIA wiring
- **Dialog:** use `command="show-modal"` / `command="close"` with `commandfor="id"`
- **Tooltip:** attribute only — `data-tooltip="text" data-placement="top"` on the element itself
````

## Notes

- Embed all fetched content verbatim where indicated — do not summarise the README or types.
- The per-component usage examples should be minimal but correct (inferred from selectors + README examples where available).
- Record the current UTC timestamp at the top so consumers know when it was generated.
