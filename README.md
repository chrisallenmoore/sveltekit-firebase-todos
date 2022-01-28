# SvelteKit Firebase Todos

This is a project testing out Svelte and Firebase with offline capabilites (indexeddb persistence is enabled with enableIndexedDbPersistence).

It's really, really basic. ;)

## Running the app

Once you've cloned or downloaded the project, install dependencies with `npm install` (or `pnpm install` or `yarn`), then start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

Before creating a production version of your app, install an [adapter](https://kit.svelte.dev/docs#adapters) for your target environment. Then:

```bash
npm run build
```

> You can preview the built app with `npm run preview`, regardless of whether you installed an adapter. This should _not_ be used to serve your app in production.
