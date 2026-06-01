# Designsystemet Component Cheat Sheet

Quick reference for `@digdir/designsystemet-css` + `@digdir/designsystemet-web`.
Full docs: https://designsystemet.no/en/components

Most components are **native HTML elements** with `class="ds-{name}"` and `data-*` attributes.
Only `<ds-field>`, `<ds-fieldset>`, and `<ds-suggestion>` are web components (custom element tags).

Import once in your layout:

```svelte
<!-- +layout.svelte -->
<script lang="ts">
  import '@digdir/designsystemet-web';   // registers the three web components
  import '@digdir/designsystemet-css';
  import '@digdir/designsystemet-css/theme';
</script>
```

---

## Typography

```html
<h1 class="ds-heading" data-size="2xl">Page title</h1>
<h2 class="ds-heading" data-size="lg">Section title</h2>
<p class="ds-paragraph" data-size="md">Body text goes here.</p>
<a href="/path" class="ds-link">Link text</a>
```

Sizes: `2xs` | `xs` | `sm` | `md` | `lg` | `xl` | `2xl`

---

## Button

```html
<button class="ds-button" type="button">Default</button>
<button class="ds-button" data-variant="secondary" type="button">Secondary</button>
<button class="ds-button" data-variant="tertiary" type="button">Tertiary</button>
<button class="ds-button" data-variant="danger" type="button">Delete</button>
<button class="ds-button" type="button" disabled>Disabled</button>
```

---

## Forms

### Input / Textfield

```html
<ds-field>
  <label for="email">Email</label>
  <input class="ds-input" id="email" type="email" placeholder="you@example.com" />
</ds-field>
```

### Textarea

```html
<ds-field>
  <label for="message">Message</label>
  <textarea class="ds-input" id="message" rows="4" placeholder="Write something..."></textarea>
</ds-field>
```

### Select

```html
<ds-field>
  <label for="country">Country</label>
  <select class="ds-input" id="country">
    <option value="">Choose...</option>
    <option value="no">Norway</option>
    <option value="se">Sweden</option>
  </select>
</ds-field>
```

### Checkbox

```html
<ds-fieldset>
  <legend>Interests</legend>
  <ds-field>
    <label for="design">Design</label>
    <input class="ds-input" id="design" type="checkbox" value="design" />
  </ds-field>
  <ds-field>
    <label for="code">Code</label>
    <input class="ds-input" id="code" type="checkbox" value="code" />
  </ds-field>
</ds-fieldset>
```

### Radio

```html
<ds-fieldset>
  <legend>Preferred contact</legend>
  <ds-field>
    <label for="contact-email">Email</label>
    <input class="ds-input" id="contact-email" type="radio" name="contact" value="email" />
  </ds-field>
  <ds-field>
    <label for="contact-phone">Phone</label>
    <input class="ds-input" id="contact-phone" type="radio" name="contact" value="phone" />
  </ds-field>
</ds-fieldset>
```

### Switch

```html
<ds-field>
  <label for="notifications">Enable notifications</label>
  <input class="ds-input" id="notifications" type="checkbox" role="switch" />
</ds-field>
```

### Search

```html
<div class="ds-search">
  <input type="text" placeholder="Search..." />
  <button type="reset"></button>
  <button type="submit">Search</button>
</div>
```

### Suggestion (web component)

```html
<ds-suggestion>
  <input class="ds-input" placeholder="Search..." />
  <u-datalist>
    <u-option value="no">Norway</u-option>
    <u-option value="se">Sweden</u-option>
  </u-datalist>
</ds-suggestion>
```

---

## Feedback & Status

```html
<!-- Alert -->
<div class="ds-alert" data-color="success">Your changes have been saved.</div>
<div class="ds-alert" data-color="warning">Check your input.</div>
<div class="ds-alert" data-color="danger">Something went wrong.</div>
<div class="ds-alert" data-color="info">Did you know?</div>

<!-- Badge -->
<span class="ds-badge" data-count="5"></span>

<!-- Tag -->
<div class="ds-tag">Active</div>
<div class="ds-tag" data-color="success">Active</div>
<div class="ds-tag" data-variant="outline">Pending</div>
```

---

## Layout

```html
<!-- Card -->
<div class="ds-card" data-color="neutral">
  <div class="ds-card__block">
    <h2 class="ds-heading" data-size="md">Card title</h2>
    <p class="ds-paragraph">Card content here.</p>
  </div>
</div>

<!-- Divider -->
<hr class="ds-divider" aria-hidden="true" />
```

---

## Table

```html
<table class="ds-table" data-border="true">
  <thead>
    <tr>
      <th>Name</th>
      <th>Status</th>
      <th>Date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ola Nordmann</td>
      <td><div class="ds-tag" data-color="success">Active</div></td>
      <td>2025-01-15</td>
    </tr>
  </tbody>
</table>
```

---

## Chip

```html
<!-- Selectable chip (radio/checkbox) -->
<label class="ds-chip">
  <input type="radio" name="view" value="list" />
  List
</label>
<label class="ds-chip">
  <input type="radio" name="view" value="grid" />
  Grid
</label>

<!-- Removable chip -->
<button class="ds-chip" data-removable="true" type="button">Design</button>
```

---

## Overlays

### Dialog

```html
<button class="ds-button" type="button" command="show-modal" commandfor="my-dialog">Open</button>

<dialog id="my-dialog" class="ds-dialog">
  <div class="ds-dialog__block">
    <h2 class="ds-heading" data-size="md">Confirm action</h2>
  </div>
  <div class="ds-dialog__block">
    <p class="ds-paragraph">Are you sure?</p>
  </div>
  <div class="ds-dialog__block">
    <button class="ds-button" data-variant="danger" type="button">Yes, delete</button>
    <button class="ds-button" data-variant="secondary" type="button" command="close" commandfor="my-dialog">Cancel</button>
  </div>
</dialog>
```

### Popover

```html
<button class="ds-button" type="button" popovertarget="my-popover">Open</button>
<div id="my-popover" popover class="ds-popover" data-placement="bottom">
  Popover content here.
</div>
```

---

## Design Tokens (CSS Variables)

```css
/* Spacing */
var(--ds-spacing-1)   /* 4px */
var(--ds-spacing-2)   /* 8px */
var(--ds-spacing-4)   /* 16px */
var(--ds-spacing-8)   /* 32px */

/* Colors (semantic) */
var(--ds-color-accent-base-default)
var(--ds-color-neutral-text-default)
var(--ds-color-success-base-default)
var(--ds-color-danger-base-default)

/* Border radius */
var(--ds-border-radius-medium)

/* Font */
var(--ds-font-family)
```
