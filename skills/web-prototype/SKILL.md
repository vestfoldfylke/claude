---
name: web-prototype
description: >
  Use this skill whenever the user wants to build, scaffold, or prototype a web app or website.
  Triggers on: "build a web app", "create a prototype", "scaffold a SvelteKit project", "make a webapp",
  "I need a prototype for user testing", "build a frontend", "create a web interface", "set up SvelteKit",
  "make a clickable prototype", "build a web interface", or any request to create an interactive web-based UI.
  This skill produces a user-testing-ready SvelteKit SPA using Designsystemet (digdir) components,
  TypeScript 6+, and Nunito Sans. Always use this skill when the user mentions prototyping, user testing,
  SvelteKit, web app scaffolding, or wants a deployable static site.
---

# Web Prototype Skill

Scaffold and build clean, user-testing-ready web app prototypes using SvelteKit as a SPA (no SSR),
TypeScript 6+, and the Norwegian Designsystemet component library.

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
npx sv create my-app
# Choose: Skeleton project, TypeScript, no additional tools needed
cd my-app
npm install
```

### 2. Install Designsystemet packages

```bash
npm install @digdir/designsystemet-web @digdir/designsystemet-css @digdir/designsystemet-types
```

### 3. Disable SSR (SPA mode)

Create `src/routes/+layout.ts`:

```typescript
// src/routes/+layout.ts
export const ssr = false;
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

### 5. Set up fonts in `src/app.html`

Only Google Fonts is allowed as external CDN. Load Nunito Sans here:

```html
<!doctype html>
<html lang="nb">
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
  --ds-font-family: 'Nunito Sans', system-ui, sans-serif;
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
  // Import Designsystemet web components (registers custom elements)
  import '@digdir/designsystemet-web';
  // Import Designsystemet CSS (tokens and base styles) and make it hot reload
  import "@digdir/designsystemet-css";
  import "@digdir/designsystemet-css/theme";
  import '../app.css';

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
    "target": "ES2022",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "types": ["@digdir/designsystemet-web"]
  }
}
```

---

## CSS Discipline — Non-Negotiables

**Before writing ANY HTML element or CSS rule: look up the DS reference files. If a DS class or component exists for it, you MUST use it — no exceptions.**

### Mandatory DS class lookup

**ALWAYS** see - `references/components.md` — DS classes for usage of semantic elements, custom html elements, and common patterns

### Web components vs CSS-only — exhaustive whitelist

`@digdir/designsystemet-web` registers exactly these custom elements:

| Custom element | Also needs CSS class? |
|---|---|
| `<ds-breadcrumbs>` | yes — `class="ds-breadcrumbs"` |
| `<ds-error-summary>` | yes — `class="ds-error-summary"` |
| `<ds-field>` | yes — `class="ds-field"` |
| `<ds-pagination>` | yes — `class="ds-pagination"` |
| `<ds-suggestion>` | no |
| `<ds-tabs>`, `<ds-tablist>`, `<ds-tab>`, `<ds-tabpanel>` | `<ds-tabs>` needs `class="ds-tabs"` |

**Everything else is CSS-only.** If the component name is not in the table above, you MUST use a standard HTML element with the DS class — never invent a `<ds-X>` custom element for it. Examples:

```html
<!-- CORRECT -->
<div class="ds-alert" data-color="info" role="alert">...</div>
<button class="ds-button">...</button>
<div class="ds-card">...</div>

<!-- WRONG — these custom elements do not exist -->
<ds-alert class="ds-alert">...</ds-alert>
<ds-button class="ds-button">...</ds-button>
<ds-card class="ds-card">...</ds-card>
```

### What you must NEVER do

- **No naked semantic elements** — never write `<h2>`, `<p>`, `<label>`, `<a>`, `<button>`, or a card `<div>` without the corresponding DS class from the table above.
- **No custom color variables** — never define `--color-*` or `--brand-*` in `:root`. Use DS tokens.
- **No hardcoded hex/rgb colors** — no `#3b82f6`, `rgba(0,0,0,0.5)`, etc. Use DS color tokens instead.
- **No `font-family` declarations in component styles** — it's already inherited from `:root` via `--ds-font-family`. Every repeated declaration is noise.
- **No custom `font-size` for text** — use `data-size` on `.ds-heading` / `.ds-paragraph` instead.
- **No custom button, card, alert, tag, or tab styling** — use the DS components.

### Custom `<style>` blocks are for layout only

The only CSS you should write in a component's `<style>` block:
- `display: flex` / `grid` and related properties
- `max-width`, `width`, `height`
- `padding`, `margin` — prefer `var(--ds-size-N)` tokens over hardcoded `px` values
- `position`, `z-index` for sticky or overlapping elements
- `overflow`, `text-decoration: none` (for link-as-card patterns)

If you're about to write `background:`, `color:`, `border-color:`, `font-size:`, or `font-family:` in a custom style — stop and use a DS class or token instead.

---

## Svelte 5 runes in TypeScript

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

1. **DS-first, always** — reach for a DS class or designsystemet attribute before writing any CSS. If a DS component exists for what you need, use it — no exceptions. Custom styles are a last resort, only for layout.
2. **Real interactions** — wire up buttons, forms, and navigation so the tester can actually click through
3. **Fake data is fine** — hardcode realistic sample data; avoid real APIs unless required
4. **Accessible by default** — Designsystemet components are WCAG-compliant; use semantic HTML
5. **One route per screen** — use SvelteKit file-based routing to separate screens naturally
6. **Shared state** — use shared reactive state in `.svelte.ts` files with `$state` for cross-page state
7. **Norwegian language** — all UI text, labels, headings, and placeholder content must be written in Norwegian Bokmål (nb). Never use English as the display language.
8. **Not for production** — every prototype must display a clearly visible disclaimer, e.g. a banner at the top of the page: "Dette er en prototype laget av Claude og kan ikke brukes i produksjon." Use `<div class="ds-alert" data-color="info" role="alert">` for this (`ds-alert` is CSS-only — never `<ds-alert>`).

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
4. Run the security review checklist below against the actual codebase
5. Summarize the security review findings
6. Offer to add more screens, tweak components, or add state management
7. Provide the user with a zip file of the full project

---

## Security Review Checklist

Run this checklist after scaffolding and report results to the user.

### What to check

- [ ] **No external scripts or stylesheets** except Google Fonts (fonts.googleapis.com, fonts.gstatic.com)
- [ ] **All npm packages are well-established** (check `npm info <pkg>` — look for age, weekly downloads, maintainer reputation)
- [ ] **No `eval()` or `innerHTML` with user-controlled input** in any `.svelte` or `.ts` file
- [ ] **No hardcoded secrets** (API keys, passwords) — prototypes should use `.env` files with `PUBLIC_` prefix for anything non-secret
- [ ] **CSP-friendly** — no inline scripts outside of Svelte-compiled output
- [ ] **No `dangerouslySetInnerHTML` equivalent** — Svelte's `{@html}` should only be used with trusted, sanitized content
- [ ] **Dependencies have no known CVEs** — run `npm audit` and fix or explain any findings
- [ ] **Supply-chain hygiene** — avoid packages < 3 months old, prefer packages with >10k weekly downloads

### How to run

```bash
npm audit
# If issues found:
npm audit fix
```

Read the codebase and check for security issues. After review, tell the user:

> ✅ Security review complete. This prototype [passes / has the following notes]: ...
> It is safe to run locally / share with user testers.
