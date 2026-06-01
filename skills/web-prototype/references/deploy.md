# Static Deployment Reference

Use this when the user wants to host the prototype on the web (Vercel, Netlify, GitHub Pages, etc.).

---

## Setup: adapter-static

Install the static adapter:

```bash
npm install -D @sveltejs/adapter-static
```

### `svelte.config.js` for static builds

```javascript
import adapter from '@sveltejs/adapter-static';
import { vitePreprocess } from '@sveltejs/vite-plugin-svelte';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  preprocess: vitePreprocess(),
  kit: {
    adapter: adapter({
      // Output directory (default: 'build')
      pages: 'build',
      assets: 'build',
      // Required for SPA routing: serve index.html for all routes
      fallback: 'index.html',
      precompress: false,
      strict: true
    })
  }
};

export default config;
```

### `src/routes/+layout.ts` for static builds

```typescript
// SPA mode: no server-side rendering
export const ssr = false;
// Prerender the shell for static export
export const prerender = true;
```

### Build command

```bash
npm run build
# Output is in ./build/
```

---

## GitHub Pages

1. Add `base` path in `svelte.config.js` if deploying to a sub-path:

```javascript
kit: {
  adapter: adapter({ fallback: '404.html' }),
  paths: {
    base: process.env.BASE_PATH ?? ''
  }
}
```

2. Use a GitHub Actions workflow (`.github/workflows/deploy.yml`):

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build
        env:
          BASE_PATH: '/${{ github.event.repository.name }}'
      - uses: actions/upload-pages-artifact@v3
        with:
          path: build
      - uses: actions/deploy-pages@v4
```

---

## SPA Routing Note

With `fallback: 'index.html'`, all routes are served by the SPA. Deep-linking and browser refresh work correctly on all platforms above.
