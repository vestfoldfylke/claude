# Designsystemet Components Reference
> Auto-generated 2026-06-01T00:00:00Z
> Source: digdir/designsystemet @ main

---

## Setup & Installation

```bash
npm install @digdir/designsystemet-css @digdir/designsystemet-web @digdir/designsystemet-types
```

### Imports (once, in layout/entry point)
```js
import '@digdir/designsystemet-css';    // component styles
import '@digdir/designsystemet-web';    // web components + observers
```

### TypeScript
```json
{ "compilerOptions": { "types": ["@digdir/designsystemet-web"] } }
```

---

## Types

```typescript
// This file defines public types used in the Designsystemet

// EmptyObject implementation from https://github.com/sindresorhus/type-fest/blob/main/source/empty-object.d.ts
declare const emptyObjectSymbol: unique symbol;
type EmptyObject = { [emptyObjectSymbol]?: never };

/**
 * Base interface for available colors in Designsystemet.
 * The CLI will generate augmentations of this interface to allow
 * type safety of custom color names.
 */

// biome-ignore lint/suspicious/noEmptyInterface: used for interface augmentation
export interface ColorDefinitions {}
// biome-ignore lint/suspicious/noEmptyInterface: used for interface augmentation
export interface SeverityColorDefinitions {}

/**
 * If {@link ColorDefinitions} or {@link SeverityColorDefinitions} has been extended to include color names, return T,
 * otherwise return the arbitrary string type.
 */
type ColorWithFallback<T> = ColorDefinitions extends EmptyObject ? string : T;

/**
 * Represents the available severity colors for the Designsystemet variables.
 * These are predefined colors that can be used to indicate different levels of severity.
 *  - `'info'`: Use the info color.
 *  - `'success'`: Use the success color.
 *  - `'warning'`: Use the warning color.
 *  - `'danger'`: Use the danger color.
 */
export type SeverityColors = ColorWithFallback<keyof SeverityColorDefinitions>;
/**
 * Represents the available color options for the Designsystemet variables.
 *
 * These are augmented based on your theme configuration.
 *
 * Consist of both main and support colors
 * @link https://theme.designsystemet.no
 */
export type Color = ColorWithFallback<keyof ColorDefinitions>;
/**
 * Represents the recommended size options for the Designsystemet variables.
 * - `'sm'`: Use the small size.
 * - `'md'`: Use the medium size.
 * - `'lg'`: Use the large size.
 */
export type Size = 'sm' | 'md' | 'lg';
/**
 * Represents the available color scheme options for the Designsystemet variables.
 * - `'light'`: Use the light color scheme.
 * - `'dark'`: Use the dark color scheme.
 * - `'auto'`: Automatically select the color scheme based on system preferences.
 */
export type ColorScheme = 'light' | 'dark' | 'auto';
```

---

## Component Reference

### base

**CSS classes:** `.ds-sr-only`, `.ds-readonly-icon`, `.ds-print-preserve`, `.ds-focus`, `.ds-focus--inset`, `.ds-focus--middle`, `.ds-focus--visible`, `.ds-focus--visible--inset`, `.ds-focus--visible--middle`, `.ds-checkbox--checked`, `.ds-floating`

**data-* attributes:**
- `[data-color-scheme]` — set color scheme on a container
- `[data-size]` — set size mode on a container (all children inherit)
- `[data-floating]` — placement for floating elements: `top`, `bottom`, `left`, `right`, `top-start`, `top-end`, etc.
- `[data-overscroll='contain']` — on `.ds-floating`, contains overscroll

**Usage:**
```html
<!-- Screen reader only text -->
<span class="ds-sr-only">Accessible label</span>

<!-- Color scheme on a container -->
<div data-color-scheme="dark">...</div>

<!-- Size mode inheritance -->
<div data-size="sm"><!-- all ds- components inside inherit sm size --></div>
```

---

### heading

**CSS classes:** `.ds-heading`

**data-* attributes:**
- `[data-size='2xs' | 'xs' | 'sm' | 'md' | 'lg' | 'xl' | '2xl']` — explicit heading size (overrides inherited size mode)

**Usage:**
```html
<h2 class="ds-heading" data-size="lg">Page heading</h2>
<h3 class="ds-heading" data-size="md">Section heading</h3>
```

---

### label

**CSS classes:** `.ds-label`

**data-* attributes:**
- `[data-weight='semibold' | 'regular']` — font weight override

**Usage:**
```html
<span class="ds-label">Label text</span>
<span class="ds-label" data-weight="regular">Regular weight label</span>
```

---

### paragraph

**CSS classes:** `.ds-paragraph`

**data-* attributes:**
- `[data-size='xs' | 'sm' | 'md' | 'lg' | 'xl']` — text size
- `[data-variant='long' | 'short']` — line height variant

**Usage:**
```html
<p class="ds-paragraph">Body text</p>
<p class="ds-paragraph" data-size="sm" data-variant="long">Small long-form text</p>
```

---

### link

**CSS classes:** `.ds-link`

**data-* attributes:**
- `[data-color='neutral']` — neutral color scheme

**Usage:**
```html
<a href="/page" class="ds-link">Link text</a>
<!-- Also works as button -->
<button class="ds-link">Button styled as link</button>
```

---

### validation-message

**CSS classes:** `.ds-validation-message`

**data-* attributes:**
- `[data-color='success' | 'warning' | 'info']` — color/icon variant (default is `danger`)

**Usage:**
```html
<p class="ds-validation-message">This field is required</p>
<p class="ds-validation-message" data-color="success">Valid input</p>
<p class="ds-validation-message" data-color="warning">Check this field</p>
<p class="ds-validation-message" data-color="info">Information message</p>
```

---

### button

**CSS classes:** `.ds-button`

**data-* attributes:**
- `[data-variant='secondary' | 'tertiary']` — style variant (default is primary)
- `[data-icon]` — icon-only button (square, no padding)
- `[data-fullwidth]` — full-width button

**Usage:**
```html
<!-- Primary (default) -->
<button class="ds-button" type="button">Submit</button>

<!-- Secondary -->
<button class="ds-button" data-variant="secondary" type="button">Cancel</button>

<!-- Tertiary -->
<button class="ds-button" data-variant="tertiary" type="button">More info</button>

<!-- Icon only -->
<button class="ds-button" data-icon aria-label="Close" type="button">
  <svg>...</svg>
</button>

<!-- With spinner (loading state) -->
<button class="ds-button" aria-busy="true" type="button">
  <svg class="ds-spinner" aria-hidden="true" viewBox="0 0 50 50">
    <circle class="ds-spinner__background" cx="25" cy="25" r="20" fill="none" stroke-width="5"/>
    <circle class="ds-spinner__circle" cx="25" cy="25" r="20" fill="none" stroke-width="5"/>
  </svg>
  Loading...
</button>
```

---

### input

**CSS classes:** `.ds-input`

**data-* attributes / attributes:**
- `[aria-invalid='true']` — error state
- `[aria-disabled='true']` or `[disabled]` — disabled state
- `[readonly]` or `[aria-readonly='true']` — readonly state
- `type="text" | "email" | "tel" | "url" | "search"` — text-like inputs
- `type="radio"` — radio button
- `type="checkbox"` — checkbox
- `role="switch"` with `type="checkbox"` or `type="radio"` — toggle switch

**Usage:**
```html
<!-- Text input -->
<input type="text" class="ds-input" placeholder="Enter value" />

<!-- Select -->
<select class="ds-input">
  <option>Option 1</option>
</select>

<!-- Textarea -->
<textarea class="ds-input" rows="4"></textarea>

<!-- Checkbox -->
<input type="checkbox" class="ds-input" />

<!-- Radio -->
<input type="radio" class="ds-input" name="group" />

<!-- Switch -->
<input type="checkbox" role="switch" class="ds-input" />

<!-- Invalid state -->
<input type="text" class="ds-input" aria-invalid="true" />
```

---

### field

**CSS classes:** `.ds-field`, `.ds-field-affixes`, `.ds-field-affix`

**data-* attributes:**
- `[data-clickdelegatefor]` — delegate click to child input
- `[data-position='end']` — place toggle input at end (for radio/checkbox)
- `[data-variant='outline']` — outlined tile variant for radio/checkbox fields

**data-field attributes on children:**
- `[data-field='description']` — description text below label
- `[data-field='counter']` with `[data-limit]`, `[data-over]`, `[data-under]` — character counter (requires `<ds-field>` web component)

**Usage:**
```html
<!-- Text input field (use <ds-field> web component for ARIA wiring) -->
<ds-field class="ds-field">
  <label>Label</label>
  <p data-field="description">Description text</p>
  <input type="text" class="ds-input" />
  <p class="ds-validation-message">Error message</p>
</ds-field>

<!-- Checkbox field -->
<ds-field class="ds-field">
  <input type="checkbox" class="ds-input" id="chk1" />
  <label for="chk1">Checkbox label</label>
</ds-field>

<!-- Outline variant checkbox -->
<ds-field class="ds-field" data-variant="outline">
  <input type="radio" class="ds-input" name="grp" />
  <label>Option</label>
</ds-field>

<!-- Input with affix -->
<div class="ds-field-affixes">
  <span class="ds-field-affix">kr</span>
  <input type="text" class="ds-input" />
  <span class="ds-field-affix">,-</span>
</div>

<!-- Counter -->
<ds-field class="ds-field">
  <label>Text</label>
  <textarea class="ds-input"></textarea>
  <p data-field="counter" data-limit="100" data-over="%d tegn for mye" data-under="%d tegn igjen"></p>
</ds-field>
```

---

### fieldset

**CSS classes:** `.ds-fieldset`

**Usage:**
```html
<fieldset class="ds-fieldset">
  <legend>Group label</legend>
  <p data-field="description">Group description</p>
  <ds-field class="ds-field">
    <input type="radio" class="ds-input" name="grp" />
    <label>Option 1</label>
  </ds-field>
  <ds-field class="ds-field">
    <input type="radio" class="ds-input" name="grp" />
    <label>Option 2</label>
  </ds-field>
</fieldset>
```

---

### alert

**CSS classes:** `.ds-alert`

**data-* attributes:**
- `[data-color]` — uses theme color (sets generic color-aware styling)
- `[data-color='warning' | 'success' | 'danger']` — severity color with corresponding icon (default is info)

**Usage:**
```html
<!-- Info (default) -->
<div class="ds-alert" role="alert">
  <h3>Information heading</h3>
  <p>Alert message.</p>
</div>

<!-- Warning -->
<div class="ds-alert" data-color="warning" role="alert">Warning message</div>

<!-- Success -->
<div class="ds-alert" data-color="success" role="alert">Success message</div>

<!-- Danger -->
<div class="ds-alert" data-color="danger" role="alert">Error message</div>

<!-- With theme color -->
<div class="ds-alert" data-color role="alert">Themed alert</div>
```

---

### popover

**CSS classes:** `.ds-popover`

**data-* attributes:**
- `[data-variant='default' | 'tinted']` — background variant
- `[data-placement='top' | 'bottom' | 'left' | 'right']` — placement (default: `top`)
- `[data-overscroll='contain']` — contain overscroll
- `[data-autoplacement]` — auto-adjust placement

**Also:** `[data-popover='inline']` — inline trigger element

**Usage:**
```html
<button class="ds-button" popovertarget="my-popover" type="button">Open popover</button>
<div class="ds-popover" popover id="my-popover" data-placement="bottom">
  Popover content
</div>
```

---

### skip-link

**CSS classes:** `.ds-skip-link` (`.ds-skiplink` kept for backwards compatibility)

**Usage:**
```html
<a href="#main-content" class="ds-skip-link">Skip to main content</a>
```

---

### details

**CSS classes:** `.ds-details`

**data-* attributes:**
- `[data-variant='default' | 'tinted']` — background variant

**Usage:**
```html
<details class="ds-details">
  <summary>More info</summary>
  <div>Lorem ipsum dolor sit amet.</div>
</details>

<!-- Tinted variant -->
<details class="ds-details" data-variant="tinted">
  <summary>Section</summary>
  <p>Content goes here.</p>
</details>
```

---

### search

**CSS classes:** `.ds-search`

**Usage:**
```html
<!-- Without submit button (shows magnifier icon automatically) -->
<div class="ds-search">
  <input type="search" class="ds-input" placeholder="Search..." />
  <button type="reset" aria-label="Clear search"></button>
</div>

<!-- With submit button -->
<div class="ds-search">
  <input type="search" class="ds-input" placeholder="Search..." />
  <button class="ds-button" type="submit">Search</button>
</div>
```

---

### dialog

**CSS classes:** `.ds-dialog`, `.ds-dialog__block`

**data-* attributes:**
- `[data-placement='bottom' | 'top' | 'left' | 'right']` — drawer placement (default: centered modal)

**Usage:**
```html
<!-- Open/close with invoker commands (polyfilled) -->
<button class="ds-button" type="button" command="show-modal" commandfor="my-dialog">
  Open dialog
</button>
<dialog class="ds-dialog" id="my-dialog" closedby="any">
  <button class="ds-button" command="close" commandfor="my-dialog" aria-label="Close"></button>
  <h2>Dialog title</h2>
  <p>Dialog content.</p>
</dialog>

<!-- Dialog with blocks -->
<dialog class="ds-dialog" id="my-dialog2">
  <div class="ds-dialog__block">
    <h2>Header</h2>
  </div>
  <div class="ds-dialog__block">
    <p>Body</p>
  </div>
  <div class="ds-dialog__block">
    <button class="ds-button" command="close" commandfor="my-dialog2">Close</button>
  </div>
</dialog>

<!-- Drawer (bottom sheet) -->
<dialog class="ds-dialog" data-placement="bottom" id="drawer">...</dialog>
```

---

### list

**CSS classes:** `.ds-list`

**Usage:**
```html
<ul class="ds-list">
  <li>Item one</li>
  <li>Item two</li>
  <li>
    Item three
    <ul class="ds-list">
      <li>Nested item</li>
    </ul>
  </li>
</ul>

<ol class="ds-list">
  <li>First</li>
  <li>Second</li>
</ol>
```

---

### card

**CSS classes:** `.ds-card`, `.ds-card__block`

**data-* attributes:**
- `[data-variant='default' | 'tinted']` — background variant
- `[data-clickdelegatefor]` — delegates click to child anchor/button

**Usage:**
```html
<!-- Basic card -->
<div class="ds-card">
  <h3>Card title</h3>
  <p>Card content.</p>
</div>

<!-- Tinted card -->
<div class="ds-card" data-variant="tinted">Content</div>

<!-- Clickable card (as anchor) -->
<a href="/page" class="ds-card">
  <h3>Link card</h3>
  <p>Click anywhere on this card.</p>
</a>

<!-- Card with blocks -->
<div class="ds-card">
  <div class="ds-card__block">
    <img src="image.jpg" alt="" />
  </div>
  <div class="ds-card__block">
    <h3>Title</h3>
    <p>Content</p>
  </div>
</div>

<!-- Card with clickdelegatefor -->
<div class="ds-card" data-clickdelegatefor="card-link">
  <h3><a id="card-link" href="/page">Card title</a></h3>
  <p>Clicking anywhere triggers the link.</p>
</div>
```

---

### dropdown

**CSS classes:** `.ds-dropdown`

**data-* attributes:**
- `[data-placement='bottom' | 'top' | 'left' | 'right']` — placement (default: `bottom`)

**Usage:**
```html
<button class="ds-button" popovertarget="my-dropdown" type="button">Menu</button>
<ul class="ds-dropdown" popover id="my-dropdown" data-placement="bottom">
  <h3>Section</h3>
  <ul>
    <li><a href="/page1" class="ds-button" data-variant="tertiary">Option 1</a></li>
    <li><button class="ds-button" data-variant="tertiary" type="button">Option 2</button></li>
  </ul>
</ul>
```

---

### chip

**CSS classes:** `.ds-chip`

**data-* attributes:**
- `[data-removable]` — shows a remove (×) icon; use on non-input chips

**Usage:**
```html
<!-- Filter chip (radio) -->
<label class="ds-chip">
  <input type="radio" class="ds-input" name="filter" value="all" />
  All
</label>

<!-- Toggle chip (checkbox) -->
<label class="ds-chip">
  <input type="checkbox" class="ds-input" />
  Category
</label>

<!-- Removable chip (e.g. selected tag) -->
<button class="ds-chip" data-removable type="button">Selected item</button>
```

---

### divider

**CSS classes:** `.ds-divider`

**Usage:**
```html
<hr class="ds-divider" />
```

---

### tabs

**CSS classes:** `.ds-tabs`

**Usage:**
```html
<!-- Using ds-tabs web component (recommended) -->
<ds-tabs class="ds-tabs">
  <ds-tablist>
    <ds-tab>Tab 1</ds-tab>
    <ds-tab>Tab 2</ds-tab>
    <ds-tab>Tab 3</ds-tab>
  </ds-tablist>
  <ds-tabpanel>Panel 1 content</ds-tabpanel>
  <ds-tabpanel>Panel 2 content</ds-tabpanel>
  <ds-tabpanel>Panel 3 content</ds-tabpanel>
</ds-tabs>

<!-- Using ARIA roles directly -->
<div class="ds-tabs">
  <div role="tablist">
    <button role="tab" aria-selected="true">Tab 1</button>
    <button role="tab" aria-selected="false">Tab 2</button>
  </div>
  <div role="tabpanel">Panel 1</div>
  <div role="tabpanel" hidden>Panel 2</div>
</div>
```

---

### pagination

**CSS classes:** `.ds-pagination`

**data-* attributes (on `<ds-pagination>` web component):**
- `[data-current]` — current page number
- `[data-total]` — total number of pages
- `[data-href]` — URL template with `%d` placeholder
- `[aria-label]` — accessible label (auto-set to Norwegian if `lang="nb|nn|no"`)

**Usage:**
```html
<!-- Web component (auto-fills page numbers) -->
<ds-pagination class="ds-pagination" aria-label="Page navigation" data-href="?page=%d" data-current="2" data-total="10">
  <ol>
    <li><a></a></li>
    <li><a></a></li>
    <li><a></a></li>
    <li><a></a></li>
    <li><a></a></li>
  </ol>
</ds-pagination>

<!-- Manual (no web component) -->
<nav class="ds-pagination" aria-label="Page navigation">
  <ol>
    <li><a href="?page=1" class="ds-button" data-variant="tertiary" aria-label="Previous"></a></li>
    <li><a href="?page=1" class="ds-button" data-variant="tertiary">1</a></li>
    <li><a href="?page=2" class="ds-button" aria-current="true">2</a></li>
    <li><a href="?page=3" class="ds-button" data-variant="tertiary">3</a></li>
    <li><a href="?page=3" class="ds-button" data-variant="tertiary" aria-label="Next"></a></li>
  </ol>
</nav>
```

---

### skeleton

**CSS classes:** `.ds-skeleton`

**data-* attributes:**
- `[data-variant='circle']` — circular skeleton (for avatars)
- `[data-variant='text']` — inline text skeleton with `[data-text]` fallback content

**Usage:**
```html
<!-- Block skeleton (e.g. image placeholder) -->
<div class="ds-skeleton" style="height: 200px;"></div>

<!-- Circle skeleton (avatar) -->
<div class="ds-skeleton" data-variant="circle" style="width: 3rem;"></div>

<!-- Text skeleton -->
<span class="ds-skeleton" data-variant="text" data-text="Loading headline text..."></span>

<!-- With actual content (skeleton wraps it, hides it) -->
<div class="ds-skeleton">
  <img src="..." alt="..." />
</div>
```

---

### tag

**CSS classes:** `.ds-tag`

**data-* attributes:**
- `[data-variant='outline']` — adds a border

**Usage:**
```html
<span class="ds-tag">Default tag</span>
<span class="ds-tag" data-variant="outline">Outline tag</span>

<!-- Tag with icon -->
<span class="ds-tag">
  <svg aria-hidden="true">...</svg>
  Tagged
</span>
```

---

### error-summary

**CSS classes:** `.ds-error-summary`

**Usage:**
```html
<!-- Use <ds-error-summary> web component — auto-focuses and sets aria-labelledby -->
<ds-error-summary class="ds-error-summary">
  <h2>Fix the following errors</h2>
  <ul>
    <li><a href="#field-name">Name is required</a></li>
    <li><a href="#field-email">Enter a valid email</a></li>
  </ul>
</ds-error-summary>
```

---

### toggle-group

**CSS classes:** `.ds-toggle-group` (also `.ds-togglegroup`)

**data-* attributes:**
- `[data-toggle-group]` — accessible label for the group (required; triggers JS observer)
- `[data-variant='secondary']` — secondary style variant

**Usage:**
```html
<!-- Radio toggle group (requires data-toggle-group for keyboard nav) -->
<fieldset class="ds-toggle-group" data-toggle-group="Text alignment">
  <label>
    <input type="radio" class="ds-button" name="align" value="left" checked />
    Left
  </label>
  <label>
    <input type="radio" class="ds-button" name="align" value="center" />
    Center
  </label>
  <label>
    <input type="radio" class="ds-button" name="align" value="right" />
    Right
  </label>
</fieldset>

<!-- Secondary variant -->
<fieldset class="ds-toggle-group" data-toggle-group="View" data-variant="secondary">
  <label>
    <input type="radio" class="ds-button" name="view" value="grid" />
    Grid
  </label>
  <label>
    <input type="radio" class="ds-button" name="view" value="list" />
    List
  </label>
</fieldset>
```

---

### tooltip

**CSS classes:** `.ds-tooltip`

**data-* attributes:**
- `[data-tooltip="text"]` — tooltip content (on the trigger element, not the tooltip itself)
- `[data-placement='top' | 'bottom' | 'left' | 'right']` — placement (default: `top`)

**Usage:**
```html
<!-- Attribute-only tooltip (recommended — handled by JS observer) -->
<button class="ds-button" data-tooltip="This is a tooltip" data-placement="top" type="button">
  Hover me
</button>

<!-- Custom tooltip element (requires JS initialization) -->
<button class="ds-button" popovertarget="my-tooltip" type="button">Hover</button>
<div class="ds-tooltip" popover id="my-tooltip" data-placement="bottom">Tooltip text</div>
```

---

### spinner

**CSS classes:** `.ds-spinner`, `.ds-spinner__background`, `.ds-spinner__circle`

**data-* attributes:**
- `[data-size='2xs' | 'xs' | 'sm' | 'md' | 'lg' | 'xl']` — explicit size (overrides inherited size mode)

**Usage:**
```html
<svg class="ds-spinner" aria-label="Loading..." role="img" viewBox="0 0 50 50">
  <circle class="ds-spinner__background" cx="25" cy="25" r="20" fill="none" stroke-width="5" />
  <circle class="ds-spinner__circle" cx="25" cy="25" r="20" fill="none" stroke-width="5" />
</svg>

<!-- Sized spinner -->
<svg class="ds-spinner" data-size="sm" aria-hidden="true" viewBox="0 0 50 50">
  <circle class="ds-spinner__background" cx="25" cy="25" r="20" fill="none" stroke-width="5" />
  <circle class="ds-spinner__circle" cx="25" cy="25" r="20" fill="none" stroke-width="5" />
</svg>
```

---

### table

**CSS classes:** `.ds-table`

**data-* attributes:**
- `[data-border]` — adds outer border with rounded corners
- `[data-sticky-header]` — sticky table header
- `[data-zebra]` — alternating row background
- `[data-hover]` — row hover highlight

**Usage:**
```html
<table class="ds-table" data-border data-zebra>
  <caption>Table caption</caption>
  <thead>
    <tr>
      <th aria-sort="none"><button>Column A</button></th>
      <th>Column B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Cell 1</td>
      <td>Cell 2</td>
    </tr>
  </tbody>
</table>

<!-- Sortable column (ascending) -->
<th aria-sort="ascending"><button>Name</button></th>
<!-- Sortable column (descending) -->
<th aria-sort="descending"><button>Date</button></th>
```

---

### breadcrumbs

**CSS classes:** `.ds-breadcrumbs`

**data-* attributes / attributes:**
- `[aria-label]` — accessible nav label (auto-set to `"Du er her:"` if `lang="nb|nn|no"`)

**Usage:**
```html
<!-- Use <ds-breadcrumbs> web component — auto-manages aria-label and aria-current -->
<ds-breadcrumbs class="ds-breadcrumbs" aria-label="You are here:">
  <!-- Back link shown on mobile, hidden on desktop -->
  <a href="/level-2" aria-label="Back to Level 2">Level 2</a>
  <!-- List shown on desktop, hidden on mobile -->
  <ol>
    <li><a href="/">Home</a></li>
    <li><a href="/level-1">Level 1</a></li>
    <li><a href="/level-2">Level 2</a></li>
    <li><a href="/current">Current page</a></li>
  </ol>
</ds-breadcrumbs>
```

---

### badge

**CSS classes:** `.ds-badge`, `.ds-badge--position`

**data-* attributes:**
- `[data-count]` — count value displayed in the badge (rendered via CSS `content: attr(data-count)`)
- `[data-variant='base' | 'tinted']` — color variant
- `[data-placement='top-right' | 'top-left' | 'bottom-right' | 'bottom-left']` — on `.ds-badge--position` wrapper
- `[data-overlap='circle']` — on `.ds-badge--position` wrapper, insets placement for circular targets

**Usage:**
```html
<!-- Inline badge -->
<span class="ds-badge" data-count="5"></span>

<!-- Tinted variant -->
<span class="ds-badge" data-variant="tinted" data-count="12"></span>

<!-- Positioned on an icon -->
<span class="ds-badge--position" data-placement="top-right">
  <svg aria-hidden="true">...</svg>
  <span class="ds-badge" data-count="3"></span>
</span>

<!-- Positioned on a circular avatar -->
<span class="ds-badge--position" data-placement="top-right" data-overlap="circle">
  <span class="ds-avatar">AB</span>
  <span class="ds-badge" data-count="1"></span>
</span>
```

---

### avatar

**CSS classes:** `.ds-avatar`

**data-* attributes:**
- `[data-initials="AB"]` — initials when element is empty (rendered via CSS)
- `[data-variant='square']` — square (rounded) avatar instead of circle
- `[data-size='xs' | 'sm' | 'md' | 'lg']` — explicit size

**Usage:**
```html
<!-- With initials via attribute -->
<span class="ds-avatar" data-initials="AB"></span>

<!-- With initials via span child -->
<span class="ds-avatar"><span>AB</span></span>

<!-- With image -->
<span class="ds-avatar">
  <img src="profile.jpg" alt="Name Surname" />
</span>

<!-- Default person icon (empty, no initials) -->
<span class="ds-avatar"></span>

<!-- Square variant -->
<span class="ds-avatar" data-variant="square" data-initials="DS"></span>

<!-- Sized -->
<span class="ds-avatar" data-size="lg" data-initials="JD"></span>
```

---

### avatar-stack

**CSS classes:** `.ds-avatar-stack`

**data-* attributes:**
- `[data-expandable]` — avatars spread out on hover/focus
- `[data-expandable='fixed']` — fixed width stack (requires `--dsc-avatar-count` CSS var)
- `[data-suffix="+ 3 more"]` — text appended after the stack

**Usage:**
```html
<!-- Basic stack -->
<figure class="ds-avatar-stack">
  <span class="ds-avatar" data-initials="A1"></span>
  <span class="ds-avatar" data-initials="A2"></span>
  <span class="ds-avatar" data-initials="A3"></span>
</figure>

<!-- Expandable on hover -->
<figure class="ds-avatar-stack" data-expandable data-suffix="+ 2 more">
  <span class="ds-avatar" data-initials="AB"></span>
  <span class="ds-avatar" data-initials="CD"></span>
  <span class="ds-avatar" data-initials="EF"></span>
</figure>
```

---

### suggestion

**CSS classes:** `.ds-suggestion`

**data-* attributes:**
- `[data-multiple]` — enables multi-select mode with chips

**Usage:**
```html
<!-- Single select suggestion/autocomplete -->
<ds-field class="ds-field">
  <label>Search</label>
  <ds-suggestion class="ds-suggestion">
    <input type="search" class="ds-input" placeholder="Type to search..." />
    <del aria-label="Clear"></del>
    <u-datalist>
      <u-option value="option-1">Option 1</u-option>
      <u-option value="option-2">Option 2</u-option>
      <u-option value="option-3">Option 3</u-option>
    </u-datalist>
  </ds-suggestion>
</ds-field>

<!-- Multi-select -->
<ds-field class="ds-field">
  <label>Select multiple</label>
  <ds-suggestion class="ds-suggestion" data-multiple>
    <input type="search" class="ds-input" />
    <del aria-label="Clear"></del>
    <u-datalist>
      <u-option value="a">Option A</u-option>
      <u-option value="b">Option B</u-option>
    </u-datalist>
  </ds-suggestion>
</ds-field>
```

---

### combobox

> **Note:** `.ds-combobox` is a legacy React-era component. Prefer `<ds-suggestion>` for new implementations.

**CSS classes:** `.ds-combobox`, `.ds-combobox__input__wrapper`, `.ds-combobox__input`, `.ds-combobox--sm`, `.ds-combobox--md`, `.ds-combobox--lg`, `.ds-combobox--error`, `.ds-combobox--readonly`, `.ds-combobox__chip-and-input`, `.ds-combobox__arrow`, `.ds-combobox__label`, `.ds-combobox__description`, `.ds-combobox__clear-button`, `.ds-combobox__disabled`, `.ds-combobox__options-wrapper`, `.ds-combobox__option`, `.ds-combobox__option--active`, `.ds-combobox__option--multiple`, `.ds-combobox__option__label`, `.ds-combobox__option__icon-wrapper`, `.ds-combobox__option__icon-wrapper--selected`, `.ds-combobox__option__icon-wrapper__icon`, `.ds-combobox__option__description`, `.ds-combobox__loading`, `.ds-combobox__error-message`, `.ds-combobox__empty`, `.ds-combobox__custom`, `.ds-combobox__readonly__icon`

**Also includes textfield classes:** `.ds-textfield`, `.ds-textfield__adornment`, `.ds-textfield__input`, `.ds-textfield--readonly`, `.ds-textfield__field`, `.ds-textfield--sm`, `.ds-textfield--md`, `.ds-textfield--lg`, `.ds-textfield__label`, `.ds-textfield__description`, `.ds-textfield--error`, `.ds-textfield__input--with-prefix`, `.ds-textfield__input--with-suffix`, `.ds-textfield__prefix`, `.ds-textfield__suffix`, `.ds-textfield__readonly__icon`, `.ds-textfield__error-message`

**Usage:**
```html
<!-- Prefer ds-suggestion instead. Legacy combobox structure: -->
<div class="ds-combobox ds-combobox--md">
  <label class="ds-combobox__label">Label</label>
  <div class="ds-combobox__input__wrapper">
    <div class="ds-combobox__chip-and-input">
      <input class="ds-combobox__input" type="text" />
    </div>
    <button class="ds-combobox__clear-button" type="button" aria-label="Clear"></button>
    <span class="ds-combobox__arrow" aria-hidden="true">▾</span>
  </div>
</div>
```

---

## Web Components & Behaviors

## `@digdir/designsystemet-web`

### Table of contents

- [`@digdir/designsystemet-web`](#digdirdesignsystemet-web)
  - [Table of contents](#table-of-contents)
- [Get started](#get-started)
  - [Individual imports](#individual-imports)
  - [Types](#types)
  - [Warnings:](#warnings)
- [`<ds-breadcrumbs>`](#ds-breadcrumbs)
- [`<ds-error-summary>`](#ds-error-summary)
- [`<ds-field>`](#ds-field)
  - [Counter](#counter)
- [`<ds-pagination>`](#ds-pagination)
- [`<ds-suggestion>`](#ds-suggestion)
- [`<ds-tabs>`](#ds-tabs)
- [`data-toggle-group`](#data-toggle-group)
- [`data-tooltip`](#data-tooltip)
- [`data-clickdelegatefor`](#data-clickdelegatefor)
- [`readonly`](#readonly)
- [`fieldset`](#fieldset)
- [Polyfills](#polyfills)
  - [invokers-polyfill](#invokers-polyfill)
  - [`<dialog>`](#dialog)
    - [open & close](#open--close)
  - [`details` and `summary`](#details-and-summary)
  - [`popover`](#popover)


## Get started

We recommend to import the whole package.
This will register all web components and observers globally, so you only need to do this once.
```ts
import '@digdir/designsystemet-web';
```

### Individual imports

The package supports sub-path exports which means you can import individual parts of the package if needed, but this is used at own risk.

For example - importing `tooltip`, you need to also import `popover` as its built using native popover functionality.

The [invokers-polyfill](#invokers-polyfill) will **not be automatically attached using individual imports**.

### Types
Add the package to your `types` for types:
```json
{
  "compilerOptions": {
    "types": ["@digdir/designsystemet-web"]
  }
}
```

### Warnings:

`@digdir/designsystemet-web` will warn you about deprecations and missing attributes.
This can come in handy while developing, but can also easily be hidden, for example in production:

```
import `@digdir/designsystemet-web`;
if (typeof window !== 'undefined' && isProduction()) window.dsWarnings = false;
```



## `<ds-breadcrumbs>`
Automatically hides/shows `aria-label` on desktop/mobile and `aria-current="page"` on last link in list. No API.

```html
<ds-breadcrumbs class="ds-breadcrumbs" aria-label="You are here:">
  <a href="#none" aria-label="Back to level 3">
    Level 3
  </a>
  <ol>
    <li><a href="#none">Level 1</a></li>
    <li><a href="#none">Level 2</a></li>
    <li><a href="#none">Level 3</a></li>
  </ol>
</ds-breadcrumbs>
```

## `<ds-error-summary>`
Automatically takes focus when visible and sets `aria-labelledby` to the first child heading. No API.

```html
<ds-error-summary class="ds-error-summary">
  <h2>Summary</h2>
  <ul>
    <li><a href="#none">Error 1</a></li>
    <li><a href="#none">Error 2</a></li>
    <li><a href="#none">Error 3</a></li>
  </ul>
</ds-error-summary>
```

## `<ds-field>`
Connects inputs, labels and error messages.

```html
<ds-field class="ds-field">
  <label>Label</label>
  <input type="text" placeholder="Placeholder" class="ds-input" />
  <div class="ds-validation-message" data-field="validation">
    This is a validation message.
  </div>
</ds-field>
```

### Counter
You can add a counter to inputs and textareas by adding the `data-field="counter"` attribute to a `<p>` element inside a `ds-field`.

```html
<ds-field class="ds-field">
  <label>Label</label>
  <textarea class="ds-input"></textarea>
  <p data-field="counter" data-limit="20" data-over="%d tegn for mye" data-under="%d tegn igjen"></p>
</ds-field>
```

| attribute  | type   | default               | required |
|------------|--------|-----------------------|----------|
| data-limit | number | undefined             | true     |
| data-over  | string | %d tegn for mye       | false    |
| data-under | string | %d tegn igjen         | false    |

## `<ds-pagination>`
Implements pagination, fills buttons with text.
You can use both `<a>` and `<button>` elements inside the pagination.

If you don't pass any attributes you can implement your own logic for current page and total pages.

```html
<ds-pagination class="ds-pagination" aria-label="Bla i sider:" data-href="?page=%d" data-current="2" data-total="100">
  <ol>
    <li><a>1</a></li>
    <li><a>2</a></li>
    <li><a>3</a></li>
    <li><a>4</a></li>
  </ol>
</ds-pagination>
```

## `<ds-suggestion>`
Extends `u-combobox` from u-elements. See documentation for [u-combobox](https://u-elements.github.io/u-elements/elements/u-combobox).

```html
<ds-field class="ds-field">
  <label>Label</label>
  <ds-suggestion class="ds-suggestion">
    <input type="search" class="ds-input" />
    <del aria-label="Fjern innhold"></del>
    <u-datalist>
      <u-option value="option-1">Option 1</u-option>
      <u-option value="option-2">Option 2</u-option>
      <u-option value="option-3">Option 3</u-option>
    </u-datalist>
  </ds-suggestion>
</ds-field>
```


## `<ds-tabs>`
Extends `u-tabs` from u-elements. See documentation for [u-tabs](https://u-elements.github.io/u-elements/elements/u-tabs).

```html
<ds-tabs class="ds-tabs">
  <ds-tablist>
    <ds-tab>Tab 1</ds-tab>
    <ds-tab>Tab 2</ds-tab>
    <ds-tab>Tab 3</ds-tab>
  </ds-tablist>
  <ds-tabpanel>Panel 1</ds-tabpanel>
  <ds-tabpanel>Panel 2</ds-tabpanel>
  <ds-tabpanel>Panel 3</ds-tabpanel>
</ds-tabs>
```

## `data-toggle-group`
This is implemented differently from `ToggleGroup` in the react package.

An observer will look for `data-toggle-group` and add proper arrow navigation plus Enter-key support.

```html
<fieldset class="ds-toggle-group" data-toggle-group="Text alignment" data-variant="secondary">
  <label>
    <input type="radio" name="alignment-two" value="left" checked />
    Left aligned
  </label>
  <label>
    <input type="radio" name="alignment-two" value="center" />
    Center aligned
  </label>
  <label>
    <input type="radio" name="alignment-two" value="right" />
    Right aligned
  </label>
</fieldset>
```

## `data-tooltip`
Using a single element for rendering next to elements with `data-tooltip` attribute.
Also automatically sets `aria-label` or `aria-description` as needed.
Uses native popover functionality with our [`popover`](#popover) polyfill.

```html
<button data-placement="left" data-tooltip="left" class="ds-button">left</button>
```

## `data-clickdelegatefor`
Used for delegating click event. For example, you can use this to delegate click events from a parent element to child elements that are added dynamically.

```html
<div class="ds-card" data-clickdelegatefor="target">
  <a id="target" href="https://example.com" rel="noopener">Go to example</a>
  <span>Clicking this card will open example in a new tab</span>
</div>
```

## `readonly`
Used for fixing `readonly` support on `select` and `input` elements. Add `aria-readonly="true"` to make the element behave as readonly, which means that it will not be editable by the user or call any change events.

```html
<select aria-readonly="true">
  <option value="1">Option 1</option>
  <option value="2">Option 2</option>
  <option value="3">Option 3</option>
</select>
```

## `fieldset`

An observer will look on `fieldset` element, add an id to combine children element with `data-field="description"` and `legend` into `aria-labeledby` on `fieldset`.

```html
<fieldset>
    <legend>Delivery method</legend>
    <p data-field="description">Choose one option</p>
</fieldset>
```


## Polyfills

### invokers-polyfill
We automatically attach [invokers-polyfill](https://www.npmjs.com/package/invokers-polyfill/v/0.5.2), which means that you get support for `command` and `commandfor`.

### `<dialog>`
Use the native `<dialog>` element. We polyfill support for [`closedby="any"`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDialogElement/closedBy#any).

```html
<dialog class="ds-dialog" closedby="any" id="my-dialog">
my dialog
</dialog>
```

#### open & close

Use invokers  `command` and `commandfor`, to open and close dialog.
```html
<button class="ds-button" type="button" command="show-modal" commandfor="my-dialog">
  Open dialog
</button>
<dialog id="my-dialog" class="ds-dialog">
  <button class="ds-button" command="close" commandfor="my-dialog">Close</button>
</dialog>
```

### `details` and `summary`
Use native elements. We polyfill a bug in Firefox when combined with Android Talkback screen reader to announce state and role properly.

```html
<details class="ds-details">
  <summary>More info</summary>
  <div>Lorem ipsum dolor sit amet.</div>
</details>
```


### `popover`
We use native popover functionality, but we attach an event listener that fixes placement of designsystem components.

```html
<button class="ds-button" popovertarget="popover">Open popover</button>
<div class="ds-popover" popover id="popover" data-placement="left">
  This is some popover content. It can be very long, but it will wrap and
  stay within the viewport.
</div>
```

| attribute     | type   | default    | required |
|---------------|--------|------------|----------|
| data-placement | string | top        | false    |
| data-overscroll | 'contain' | undefined | false    |
| data-autoplacement | boolean | true | false    |

**If you don't use the class `ds-popover` you need to add the CSS property `--_ds-floating` to the popover element.** This can be `top`, `bottom`, `left` or `right`.

---

## Key Patterns

- **Variants via data-attrs, not BEM:** `data-variant="secondary"` not `ds-button--secondary`
- **Size inheritance:** set `data-size="sm"` on a container, all children inherit — except `.ds-heading`, `.ds-paragraph`, `.ds-spinner`, `.ds-avatar` which always default to `md` and must be sized explicitly with `data-size`
- **Always wrap inputs in `<ds-field>` (web component)** for correct label/error ARIA wiring
- **Dialog:** use `command="show-modal"` / `command="close"` with `commandfor="id"` (polyfilled via invokers-polyfill)
- **Tooltip:** attribute only — `data-tooltip="text"` and optionally `data-placement="top"` on the trigger element itself
- **Toggle group:** add `data-toggle-group="label"` to `.ds-toggle-group` — the JS observer wires up keyboard navigation
- **Readonly inputs:** use `aria-readonly="true"` instead of HTML `readonly` for select elements; the web component observer handles correct behavior
- **Colors:** `data-color` activates theme-color styling; specific values `warning`, `success`, `danger`, `info` select severity colors
- **Floating elements (popover/dropdown/tooltip):** placement is set via `data-placement` on the element itself
- **Card as link:** use `<a class="ds-card">` or `<button class="ds-card">` directly — heading text gets automatic underline on hover
- **Card with delegated click:** add `data-clickdelegatefor="link-id"` on `.ds-card` to make the whole card clickable via a specific child link
