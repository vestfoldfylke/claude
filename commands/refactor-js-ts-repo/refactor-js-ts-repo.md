---
description: Kjører full opprydding (dependencies, biome, node:test, fetch, typescript)
argument-hint: ""
---

Utfør følgende refaktoreringer trinn for trinn. Dersom et eller flere av stegene skal utføres, opprett og check out en ny branch ved navn `security-fixes` før endringene gjøres. For hvert steg må du verifisere endringene før du går videre til neste steg:

1. **Oppdater dependencies:**
    - Kjør `npm outdated` for å se avhengigheter som kan oppdateres.
    - Oppdater pakkene (f.eks via `npm update` eller ved å heve versjonene i `package.json` og kjøre `npm install`).
        - Dersom en av pakkene er @biomejs/biome skal denne oppdateres via kommando: `npm i -D -E @biomejs/biome@latest`.
            - Deretter må biome konfigurasjonsfilen oppgraderes via kommando: `npx @biomejs/biome migrate --write`.
    - Kjør prosjektets tester og build-kommando (hvis eksisterende) (f.eks `npm test` og `npm run build`) for å verifisere at alt fungerer.
    - Når alle tester kjører uten problemer og eventuel build kommando kjører uten problemer, committes dette med følgende message: `chore: Dependency updates`.

2. **Migrer til Biome:**
    - Sjekk om repoet bruker andre lintere/formaterere (f.eks ESLint, Prettier, Standard, StandardX) eventuelt om det ikke er noen lintere/formaterere installert.
    - Fjern disse pakkene samt deres konfigurasjonsfiler (`.eslintrc*`, `.prettierrc*`, `eslint.config.js` osv.).
    - Installer @biomejs/biome med følgende kommando `npm i -D -E @biomejs/biome@latest`.
    - Opprett `biome.json` i roten av repoet. Innholdet kopieres fra `~/.claude/commands/refactor-js-ts-repo-biome.json` og limes inn i `biome.json`.
    - Bytt ut eller legg til `npx @biomejs/biome check` i test script i `package.json`.
    - Bytt ut eller legg til `npx @biomejs/biome check --write` i lint:fix script i `package.json`.
    - Kjør `npm run lint:fix` for å auto-rette feil.
    - Kjør prosjektets tester og build-kommando (hvis eksisterende) (f.eks `npm test` og `npm run build`) for å verifisere at alt fungerer.
    - Når alle tester kjører uten problemer og eventuel build kommando kjører uten problemer, committes dette med følgende message: `chore: Migrated from %insert-previous-linter/formater-here% to biome`.

3. **Migrer til node:test:**
    - Fjern jest eller andre test-rammeverk og tilhørende konfigurasjoner (`jest.config.js`, osv.).
    - Migrer eksisterende testfiler til å bruke den innebygde `node:test` og `node:assert` fra Node.js.
    - Oppdater `package.json` sine test-scripts til å bruke `node --test`.
    - Kjør prosjektets tester og build-kommando (hvis eksisterende) (f.eks `npm test` og `npm run build`) for å verifisere at alt fungerer.
    - Når alle tester kjører uten problemer og eventuel build kommando kjører uten problemer, committes dette med følgende message: `chore: Migrated from %insert-previous-test-framework-here% to node:test`.

4. **Migrer fra Axios til native fetch:**
    - Sjekk om `axios` er installert i `package.json`.
    - Avinstaller axios `npm uninstall axios`.
    - Erstatt alle `axios`-kall i koden med den globale/innebygde `fetch`-funksjonen.
    - Kjør prosjektets tester og build-kommando (hvis eksisterende) (f.eks `npm test` og `npm run build`) for å verifisere at alt fungerer.
    - Når alle tester kjører uten problemer og eventuel build kommando kjører uten problemer, committes dette med følgende message: `chore: Migrated from axios to fetch`.

5. **Migrer fra @vtfk/logger til @vestfoldfylke/loglady:**
    - Sjekk om `@vtfk/logger` er installert i `package.json`.
    - Avinstaller @vtfk/logger `npm uninstall @vtfk/logger`.
    - Installer @vestfoldfylke/loglady `npm install @vestfoldfylke/loglady@latest`.
    - Erstatt alle `@vtfk/logger`-kall i koden med den @vestfoldfylke/loglady.
        - Hvordan den brukes finner du her: https://github.com/vestfoldfylke/loglady#readme.
    - Kjør prosjektets tester og build-kommando (hvis eksisterende) (f.eks `npm test` og `npm run build`) for å verifisere at alt fungerer.
    - Når alle tester kjører uten problemer og eventuel build kommando kjører uten problemer, committes dette med følgende message: `chore: Migrated from @vtfk/logger to @vestfoldfylke/loglady`.

6. **Migrer JavaScript til TypeScript:**
    - Dersom repoet er skrevet i JavaScript (`.js`), konverter filene til TypeScript (`.ts`) og esm.
    - Hvis types skal exportes, putt dem sammen i en mappe som heter types. Dersom det er en type som bare skal brukes internt i en TypeScript fil kan den opprettes i samme fil, men da helt først i fila, en linje under alle importene.
    - Alle variabler skal ha eksplisitt satt type
    - Alle funksjoner skal ha eksplisitt satt retur type
    - Legg til `typescript` og nødvendige `@types/*`-pakker i dev dependencies dersom de mangler.
    - Opprett `build` script i `package.json` hvis det ikke allerede er der.
    - Opprett/konfigurer en gyldig `tsconfig.json`.
    - Kjør typechecking (`npx tsc --noEmit`), test og build til alt kompilerer og passerer uten feil.
    - Når alle tester kjører uten problemer og build kommando kjører uten problemer, committes dette med følgende message: `chore: Migrated from JavaScript til TypeScript`.

7. **Migrer Azure Functions til 4.x**
    - Dersom `host.json` tilsier at det ikke brukes **Azure Functions 4.x**, oppgrader til **4.x** i `host.json` og alle endepunktene.
    - `app.http.handler / app.timer.handler / app.*.handler` skal ikke skrives inline, men ha en egen arrow funksjon definert i samme fil
    - `app.timer.schedule` skal peke til en environment variabel i stedet for å ha cron expression hardkodet inn
