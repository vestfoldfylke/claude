---
name: web-prototype
description: >
  Use this skill whenever the user wants to build, scaffold, or prototype a web app or website.
  Triggers on: "build a web app", "create a prototype", "scaffold a SvelteKit project", "make a webapp",
  "I need a prototype for user testing", "build a frontend", "create a web interface", "set up SvelteKit",
  "make a clickable prototype", "build an app", or any request to create an interactive web-based UI.
  This skill produces a user-testing-ready SvelteKit SPA using Designsystemet (digdir) components,
  TypeScript 6+, and Nunito Sans. Always use this skill when the user mentions prototyping, user testing,
  SvelteKit, web app scaffolding, or wants a deployable static site.
---

# Web Prototype Skill

Scaffold and build clean, user-testing-ready web app prototypes using SvelteKit as a SPA (no SSR),
TypeScript 6+, and the Norwegian Designsystemet component library.

## Your first action: ask deployment target

Before writing any code, ask the user one question:

> **Will this prototype be run locally or does it need to be available on the web?**

- **Local**: use `@sveltejs/adapter-auto` (default, works with `npm run dev`)
- **Deployed (static)**: use `@sveltejs/adapter-static` — see `references/deploy.md`

Then proceed immediately without further questions unless the user's brief is very vague.

---

## Technology Stack

| Layer | Choice | Version |
|---|---|---|
| Framework | SvelteKit | 2.x (latest) |
| Language | TypeScript | 6.x (latest) |
| UI Components | `@digdir/designsystemet-web` | 1.x (latest) |
| CSS/Tokens | `@digdir/designsystemet-css` | 1.x (latest) |
| Font | Nunito Sans (Google Fonts) | — |
| Build | Vite | bundled with SvelteKit |
| Adapter (local) | `@sveltejs/adapter-auto` | bundled |
| Adapter (deploy) | `@sveltejs/adapter-static` | separate install |

**Package discipline**: Only install packages if strictly necessary. Prefer the packages above. Never install newly-released or obscure packages — require at minimum several months of community usage and active maintenance history.

---

## Project Structure

```
my-app/
├── src/
│   ├── app.html               # HTML shell — load Google Fonts here
│   ├── app.css                # Global styles, Designsystemet CSS import
│   ├── routes/
│   │   ├── +layout.ts         # export const ssr = false; export const prerender = true (if static)
│   │   ├── +layout.svelte     # App shell, nav, global imports
│   │   └── +page.svelte       # Home page
│   └── lib/                   # Shared components and utilities
├── static/                    # Static assets
├── svelte.config.js
├── vite.config.ts
├── tsconfig.json
└── package.json
```

---

## Scaffolding Steps

### 1. Generate the project

```bash
npm create svelte@latest my-app
# Choose: Skeleton project, TypeScript, no additional tools needed
cd my-app
npm install
```

### 2. Install Designsystemet packages

```bash
npm install @digdir/designsystemet-web @digdir/designsystemet-css
```

### 3. Disable SSR (SPA mode)

Create `src/routes/+layout.ts`:

```typescript
// src/routes/+layout.ts
export const ssr = false;
// For deployable static builds, also add:
// export const prerender = true;
```

### 4. Configure adapter

**Local (`svelte.config.js`):**
```javascript
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter()
  }
};

export default config;
```

**For static deploy** — see `references/deploy.md`.

### 5. Set up fonts in `src/app.html`

Only Google Fonts is allowed as external CDN. Load Nunito Sans here:

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%sveltekit.assets%/favicon.png" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <!-- Google Fonts: Nunito Sans only -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Nunito+Sans:ital,opsz,wght@0,6..12,300..900;1,6..12,300..900&display=swap"
      rel="stylesheet"
    />
    %sveltekit.head%
  </head>
  <body data-sveltekit-preload-data="hover">
    <div style="display: contents">%sveltekit.body%</div>
  </body>
</html>
```

### 6. Global CSS (`src/app.css`)

```css
/* Set Nunito Sans as the global font */
:root {
  --ds-font-family: 'Nunito Sans', sans-serif;
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  font-family: var(--ds-font-family);
  margin: 0;
  padding: 0;
}
```

### 7. Layout (`src/routes/+layout.svelte`)

```svelte
<script lang="ts">
  import '../app.css';
  // Import Designsystemet web components (registers custom elements)
  import '@digdir/designsystemet-web';
  // Import Designsystemet CSS (tokens and base styles) and make it hot reload
  import "@digdir/designsystemet-css";
  import "@digdir/designsystemet-css/theme";

  const { children } = $props();
</script>

{@render children()}
```

### 8. TypeScript config

Ensure `tsconfig.json` targets modern TypeScript 6 features:

```json
{
  "extends": "./.svelte-kit/tsconfig.json",
  "compilerOptions": {
    "strict": true,
    "moduleResolution": "bundler",
    "target": "ES2022"
  }
}
```

---

## CSS Discipline — Non-Negotiables

**Before writing a single CSS rule, check if Designsystemet already handles it.**

### What you must NEVER do

- **No custom color variables** — never define `--color-*` or `--brand-*` in `:root`. Use DS tokens or `data-color`.
- **No hardcoded hex/rgb colors** — no `#3b82f6`, `rgba(0,0,0,0.5)`, etc. Use DS color tokens instead.
- **No `font-family` declarations in component styles** — it's already inherited from `:root` via `--ds-font-family`. Every repeated declaration is noise.
- **No custom `font-size` for text** — use `data-size` on `ds-heading` / `ds-paragraph` instead.
- **No custom button, card, alert, tag, or tab styling** — use the DS components below.

### Custom `<style>` blocks are for layout only

The only CSS you should write in a component's `<style>` block:
- `display: flex` / `grid` and related properties
- `max-width`, `width`, `height`
- `padding`, `margin` — prefer `var(--ds-spacing-N)` over hardcoded `px` values
- `position`, `z-index` for sticky or overlapping elements
- `overflow`, `text-decoration: none` (for link-as-card patterns)

If you're about to write `background:`, `color:`, `border-color:`, `font-size:`, or `font-family:` in a custom style — stop and use a DS class or token instead.

### DS tokens for custom CSS that's unavoidable

```css
/* Spacing (4px base scale) */
var(--ds-spacing-1)   /* 4px  */
var(--ds-spacing-2)   /* 8px  */
var(--ds-spacing-3)   /* 12px */
var(--ds-spacing-4)   /* 16px */
var(--ds-spacing-6)   /* 24px */
var(--ds-spacing-8)   /* 32px */
var(--ds-spacing-10)  /* 40px */
var(--ds-spacing-12)  /* 48px */

/* Colors — use these, never hardcode hex */
var(--ds-color-neutral-background-default)   /* white surface */
var(--ds-color-neutral-background-subtle)    /* light gray background */
var(--ds-color-neutral-border-subtle)        /* light border */
var(--ds-color-neutral-border-default)       /* standard border */
var(--ds-color-neutral-text-default)         /* primary text */
var(--ds-color-neutral-text-subtle)          /* muted / secondary text */
var(--ds-color-accent-base-default)          /* theme accent */
var(--ds-color-accent-surface-default)       /* light accent background */
var(--ds-color-accent-text-default)          /* text on accent surface */
var(--ds-color-success-base-default)
var(--ds-color-danger-base-default)

/* Border radius */
var(--ds-border-radius-medium)
var(--ds-border-radius-large)
```

### Component substitution table

| Temptation | Use this instead |
|---|---|
| Custom tab / filter buttons | `ds-chip` with `<input type="radio" bind:group={...}>` |
| Custom card with border/shadow | `ds-card` with `data-color="neutral"` |
| Info box / warning / success / error banner | `ds-alert` with `data-color="info\|warning\|success\|danger"` |
| Small label / badge / category pill | `ds-tag` with optional `data-color` |
| Styled `<p>` with custom font-size | `<p class="ds-paragraph" data-size="sm\|md\|lg">` |
| Styled heading with custom font-size | `<h2 class="ds-heading" data-size="md\|lg\|xl">` |
| Small copy / action button | `<button class="ds-button" data-size="sm" data-variant="secondary">` |

---

## Using Designsystemet Components

Most components are **native HTML elements** styled with `class="ds-{name}"` and `data-*` attributes. Only three are genuine web components with a custom element tag.

Reference: https://designsystemet.no/en/components — see `references/components.md` for a quick cheat sheet.

**Web components** (custom element tags — require the `@digdir/designsystemet-web` import):
- `<ds-field>` — wraps a form input with label, description, and error message
- `<ds-fieldset>` — groups related checkboxes or radios
- `<ds-suggestion>` — combobox / autocomplete input

**Native HTML elements** (`class="ds-*"` + `data-*` — CSS only, no web component):
- Typography: `<h1>–<h6 class="ds-heading" data-size="...">`, `<p class="ds-paragraph">`, `<a class="ds-link">`
- Actions: `<button class="ds-button" data-variant="primary|secondary|tertiary|danger" type="button">`
- Forms: `<input class="ds-input">`, `<textarea class="ds-input">`, `<select class="ds-input">`
- Feedback: `<div class="ds-alert" data-color="...">`, `<span class="ds-badge">`, `<div class="ds-tag">`
- Layout: `<div class="ds-card">`, `<hr class="ds-divider" aria-hidden="true">`
- Data: `<table class="ds-table">`
- Overlay: `<dialog class="ds-dialog">`, `<div popover class="ds-popover">`
- Chip: `<label class="ds-chip">` (with nested radio/checkbox) or `<button class="ds-chip" data-removable="true">`

**`data-size`**
- Global (most components): `sm` | `md` (default) | `lg`
- Typography only (`ds-heading`, `ds-paragraph`): `2xs` | `xs` | `sm` | `md` | `lg` | `xl` | `2xl`

**`data-color`** — `neutral` | `success` | `warning` | `danger` | `info`, plus theme-specific colors (e.g. `accent`, `brand1`, `brand2` if the active theme defines them)

**`data-variant`** — component-specific:

| Component | Supported `data-variant` values |
|---|---|
| `ds-button` | `secondary` · `tertiary` · `danger` (default is primary) |
| `ds-card` | `tinted` (fills background with the current `data-color`) |
| `ds-tag` | `outline` |

**Color inheritance — how `data-color` cascades:**

Most DS components inherit `data-color` from the **nearest ancestor** that sets it. Set it once on a container to theme everything inside:

```html
<div data-color="success">
  <!-- ds-button, ds-chip, ds-tag, ds-card inside here all pick up "success" -->
</div>
```

**Exceptions — must be set directly on the component, do not inherit:**
- `ds-alert` — always requires `data-color` on the element itself
- `ds-validation-message` — same

### Component usage pattern

```svelte
<!-- src/routes/+page.svelte -->
<script lang="ts">
  let name = $state('');
</script>

<main style="max-width: 800px; margin: 2rem auto; padding: 0 1rem;">
  <h1 class="ds-heading" data-size="xl">Welcome</h1>

  <ds-field>
    <label for="name">Your name</label>
    <input
      class="ds-input"
      id="name"
      type="text"
      placeholder="Enter your name"
      bind:value={name}
    />
  </ds-field>

  <button
    class="ds-button"
    data-variant="primary"
    type="button"
    onclick={() => alert(`Hello, ${name}!`)}
  >
    Submit
  </button>
</main>
```

### Tabs / filter buttons — always `ds-chip` with radio

```svelte
<script lang="ts">
  let activeTab = $state<'a' | 'b' | 'c'>('a');
</script>

<div style="display: flex; gap: var(--ds-spacing-2); flex-wrap: wrap; margin-bottom: var(--ds-spacing-6);">
  <label class="ds-chip">
    <input type="radio" name="tab" value="a" bind:group={activeTab} />
    Tab A
  </label>
  <label class="ds-chip">
    <input type="radio" name="tab" value="b" bind:group={activeTab} />
    Tab B
  </label>
  <label class="ds-chip">
    <input type="radio" name="tab" value="c" bind:group={activeTab} />
    Tab C
  </label>
</div>

{#if activeTab === 'a'}
  <!-- Tab A content -->
{:else if activeTab === 'b'}
  <!-- Tab B content -->
{:else}
  <!-- Tab C content -->
{/if}
```

### Cards — always `ds-card`

```svelte
<!-- Content card -->
<div class="ds-card" data-color="neutral">
  <div class="ds-card__block">
    <h3 class="ds-heading" data-size="sm">Card title</h3>
    <p class="ds-paragraph" data-size="sm">Description text.</p>
  </div>
</div>

<!-- Clickable / link card -->
<a href="/somewhere" class="ds-card" data-color="neutral" style="text-decoration: none; color: inherit;">
  <div class="ds-card__block">
    <h3 class="ds-heading" data-size="sm">Go somewhere →</h3>
    <p class="ds-paragraph" data-size="sm">Click to navigate.</p>
  </div>
</a>
```

### Info / feedback boxes — always `ds-alert`

```svelte
<div class="ds-alert" data-color="info">
  <strong>Pro tip:</strong> Give Claude context about your role and goals.
</div>

<div class="ds-alert" data-color="success">Your answer is correct!</div>
<div class="ds-alert" data-color="danger">That's not quite right.</div>
<div class="ds-alert" data-color="warning">Please review before submitting.</div>
```

### Labels / categories / badges — always `ds-tag`

```svelte
<div class="ds-tag">Category</div>
<div class="ds-tag" data-color="success">Active</div>
<div class="ds-tag" data-variant="outline">Draft</div>
```

### Svelte 5 runes in TypeScript

Use Svelte 5 runes syntax — the current standard:

```typescript
// Reactive state
let count = $state(0);
let doubled = $derived(count * 2);

// Effects
$effect(() => {
  console.log('count changed:', count);
});
```

---

## Prototype Design Principles

These principles keep prototypes lean and user-testing-ready:

1. **DS-first, always** — reach for a DS class or `data-color`/`data-size` attribute before writing any CSS. If a DS component exists for what you need, use it — no exceptions. Custom styles are a last resort, only for layout.
2. **Real interactions** — wire up buttons, forms, and navigation so the tester can actually click through
3. **Fake data is fine** — hardcode realistic sample data; avoid real APIs unless required
4. **Accessible by default** — Designsystemet components are WCAG-compliant; use semantic HTML
5. **One route per screen** — use SvelteKit file-based routing to separate screens naturally
6. **Shared state** — use Svelte stores (`$state` in `.svelte.ts` files or `writable` from `svelte/store`) for cross-page state

---

## Security Review Checklist

Run this checklist and report results to the user before handing off the prototype.

### What to check

- [ ] **No external scripts or stylesheets** except Google Fonts (fonts.googleapis.com, fonts.gstatic.com)
- [ ] **All npm packages are well-established** (check `npm info <pkg>` — look for age, weekly downloads, maintainer reputation)
- [ ] **No `eval()` or `innerHTML` with user-controlled input** in any `.svelte` or `.ts` file
- [ ] **No hardcoded secrets** (API keys, passwords) — prototypes should use `.env` files with `PUBLIC_` prefix for anything non-secret
- [ ] **CSP-friendly** — no inline scripts outside of Svelte-compiled output
- [ ] **No `dangerouslySetInnerHTML` equivalent** — Svelte's `{@html}` should only be used with trusted, sanitized content
- [ ] **Dependencies have no known CVEs** — run `npm audit` and fix or explain any findings
- [ ] **Supply-chain hygiene** — avoid packages < 3 months old, prefer packages with >10k weekly downloads

### How to run the security scan

```bash
npm audit
# If issues found:
npm audit fix
```

Read the codebase and check for security-issues

After review, tell the user:

> ✅ Security review complete. This prototype [passes / has the following notes]: ...
> It is safe to run locally / share with user testers.

---

## Routing Example (Multi-screen Prototype)

```
src/routes/
├── +layout.ts          # ssr = false
├── +layout.svelte      # nav + and shared layout
├── +page.svelte        # home screen "/"
├── step-1/
│   └── +page.svelte    # "/step-1"
├── step-2/
│   └── +page.svelte    # "/step-2"
└── confirmation/
    └── +page.svelte    # "/confirmation"
```

Navigation between screens:

Prefer:
```svelte
<a href="/step-2">Next</a>
```

If you need programmatic or must use a button, use SvelteKit's `goto`:
```svelte
<script lang="ts">
  import { goto } from '$app/navigation';
</script>

<ds-button onclick={() => goto('/step-2')}>Next</ds-button>
```

---

## Finishing Up

After scaffolding, always:

1. Run `npm run dev` to confirm the dev server starts
2. Run `npm audit` and report results
3. Tell the user how to run it locally: `npm run dev`
4. If deploying: run `npm run build`, confirm output, point to `references/deploy.md`
5. Run a security review based on the actual codebase
5. Summarize the security review findings
6. Offer to add more screens, tweak components, or add state management

---

## Reference Files

- `references/deploy.md` — Static deployment config for Vercel, Netlify, GitHub Pages
- `references/components.md` — Designsystemet component cheat sheet with common patterns
