# Claude User Configuration

## Identity & Defaults

You are working with a senior engineer. Skip beginner explanations.
Prefer precision over politeness — be direct, not terse.
When uncertain, say so explicitly rather than hedging with filler language.

---

## Stack

### Infrastructure
- Cloud: **Azure**
- IaC: **Terraform**
- Default to Azure-native services; don't suggest AWS/GCP equivalents

### Web Apps
- Framework: **SvelteKit** with **TypeScript 6**
- Hosting: **Azure Web App** (containerised or Node adapter)
- Routing: **API routes over form actions** — use `+server.ts` endpoints; avoid form actions except for progressive enhancement
- Svelte 5 runes syntax (`$state`, `$derived`, `$effect`) — never legacy stores unless interoperating
- Middleware: use hooks (`handle`, `handleFetch`) in `src/hooks.server.ts`

### Backends / APIs
- Runtime: **Azure Functions v4** with **TypeScript 6**
- Programming model: v4 (file-based registration, `app.http()`, `app.timer()` etc.) — never v3 style
- Middleware: compose handler middleware explicitly, no framework magic

### Scripts
- **Node.js** with **TypeScript 6**
- Use native `node` if available, or `tsx` to run; no `ts-node`

---

## Dependencies

- **Prefer the platform and standard library** — reach for a package only when the native API is genuinely insufficient
- When a package is warranted: it must be well-known, from a trusted publisher, actively maintained, and have as few transitive dependencies as possible
- Scrutinise dependency trees before introducing anything new — a package that pulls in 50 transitive deps is rarely worth it
- If two options solve the problem equally well, always prefer the one with fewer dependencies

---

## Code Quality Standards

### Non-Negotiables
- Functions do one thing. If you need "and" to describe it, split it.
- No commented-out code in final output — delete it or keep it, never comment it.
- No `any` in TypeScript, use `unknown` at true boundaries (external JSON, legacy interop).
- Error paths are first-class. Every error message must answer: what failed, why, what to do.
- Magic numbers get named constants. Magic strings get enums or union types.

### Naming
- Variables: what it **is**, not what it **does** (`userList`, not `getUsers`)
- Booleans: `is*`, `has*`, `can*`, `should*` prefixes always
- Functions: verb-first (`fetchUser`, `parseConfig`, `buildQuery`)
- Avoid abbreviations except industry-standard ones (`id`, `url`, `api`, `db`, `ctx`)

### File & Module Structure
- One primary export per file (default export for components, named for utilities)
- Barrel files (`index.ts`) only at public API boundaries, never internally
- Co-locate tests with source: `foo.ts` → `foo.test.ts`
- Max ~300 lines per file; refactor before adding more

---

## Language-Specific Conventions

### TypeScript (all targets)
- `const` by default; `let` only when reassignment is unavoidable; never `var`
- Arrow functions for callbacks and closures; `function` declarations for top-level
- Explicit return types on all exported functions
- Prefer `type` for object shapes, unions, aliases, mapped types; `interface` when needed
- `async/await` everywhere; no raw `.then()` chains unless composing streams
- Null handling: prefer explicit `null` over `undefined` for intentional absence
- `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes` — always on
- Linting: **Biome** — follow its ruleset; don't introduce ESLint or Prettier

### SvelteKit-Specific
- Load functions (`+page.ts`, `+layout.ts`) are pure — no side effects, no mutations
- Server-only logic goes in `+page.server.ts` or `+server.ts`, never leaks to client bundles
- Type `$props()` explicitly; no untyped component interfaces
- Use `error()` and `redirect()` from `@sveltejs/kit` — never throw raw errors in load functions
- Every protected `+page.server.ts` / `+server.ts` must validate the caller independently — never rely solely on a parent layout check

### Logging (Node.js — all targets)
- Use `@vestfoldfylke/loglady` for all logging — never `console.log`
- `logger.debug / .info / .warn / .error` for standard levels
- `logger.errorException(error, message, ...params)` when logging a caught `Error` object

### Azure Functions v4-Specific
- Register all triggers in a single `src/functions/` directory, one file per function
- Input/output bindings declared in registration, not inline
- Always set `authLevel` explicitly (`anonymous`, `function`, `admin`)

---

## Architecture Preferences

- **Functional core, imperative shell**: keep pure business logic side-effect free
- **Flat over nested**: prefer flat data structures and early returns over deep nesting
- **Explicit over implicit**: configuration, dependencies, and contracts should be visible
- **Fail fast**: validate and type all external input (HTTP bodies, query params, trigger payloads) at the boundary before it enters the domain — never deep in the call stack
- **Result types over throwing**: use a discriminated union (`{ ok: true; value: T } | { ok: false; error: E }`) for expected failure cases; reserve `throw` for truly unexpected failures

### Dependency Injection
- No DI framework — pass dependencies explicitly as function arguments or constructor parameters
- Never import singletons (DB clients, loggers, config) deep in business logic; receive them from the caller

### On Abstraction
- Don't abstract until you have 3 real use cases, not hypothetical ones
- Prefer duplication over the wrong abstraction
- Interfaces/protocols should be discovered from usage, not designed upfront

---

## Testing

- Tests are documentation — test names should read as sentences describing behavior
- Arrange / Act / Assert structure, with a blank line between sections
- One logical assertion per test (multiple `assert` calls fine if testing one behavior)
- Mock at architectural boundaries only (network, DB, time, randomness)
- Test the contract, not the implementation — refactoring shouldn't break tests

```typescript
// Good
test("returns null when the user does not have an active subscription", () => { ... });

// Bad
test("getUserSubscription works correctly", () => { ... });
```

For `node:test`:
- Use `describe` / `it` for grouping; `before` / `after` for setup/teardown
- Use `mock.fn()` and `mock.method()` for mocking — no external mock libraries

---

## Communication Style

- Lead with the answer, then the reasoning — not the other way around
- When proposing a refactor, explain the problem it solves before showing the solution
- Flag tradeoffs explicitly: "this is simpler but won't scale past ~10k records"

### Code Review Mode
When I share code for review:
1. Critical issues (bugs, security, data loss risk) — call these out first
2. Design issues (wrong abstraction, architectural smell)
3. Convention issues (style, naming)
4. Nits — mark clearly as `[nit]`, skip if there are higher-priority items

---

## What to Skip

- Don't add `// TODO` comments unless I ask
- Don't wrap responses in "Great question!" or similar filler
- Don't add error handling for truly unrecoverable cases (let it crash and surface the real error)