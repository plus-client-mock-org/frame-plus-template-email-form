# Frame+ Client Template Guide

## Purpose

This guide explains what you (the client) need to do to host a frontend template in GitHub so it works with Frame+.

You are free to use:

- Vanilla HTML/CSS/JS
- React, Vue, Svelte, Angular, etc.
- Vite, Webpack, Parcel, Next export/static builds, or any other static build flow

Frame+ only needs a static artifact output it can host.

---

## 1) Core Requirements

Your repository must provide a static frontend output that includes an `index.html`.

Frame+ build resolver checks in this order:

1. Configured `static_root` (recommended)
2. `dist/`
3. `build/`
4. Root `index.html`
5. If none found, Frame+ runs:
   - `npm ci`
   - `npm run build`
   - then checks again

Minimum rule:

- Final artifact folder must contain `index.html` plus all referenced assets.

---

## 2) GitHub Repo Requirements

- Public GitHub repo (v1 constraint)
- Branch configured in template (usually `main`)
- Build should succeed non-interactively
- No manual secret input during build

Recommended:

- Commit lockfile (`package-lock.json` / `pnpm-lock.yaml` / `yarn.lock`)
- Keep deterministic builds

---

## 3) Suggested Project Structures

## Option A: Vanilla static site (no build step)

```text
repo/
  index.html
  app.js
  styles.css
  assets/
```

Set template `static_root` to empty/null and Frame+ can serve root files directly.

## Option B: Framework with build output in `dist`

```text
repo/
  src/
  public/
  package.json
  package-lock.json
  dist/   (created by build)
```

Set template `static_root` to `dist`.

## Option C: Framework with build output in `build`

```text
repo/
  src/
  package.json
  build/  (created by build)
```

Set template `static_root` to `build`.

---

## 4) Framework Notes

## Vite (React/Vue/Svelte/etc.)

- Default output is `dist/`
- Usually works out of the box

`package.json` must include:

```json
{
  "scripts": {
    "build": "vite build"
  }
}
```

## Create React App

- Default output is `build/`

`package.json` must include:

```json
{
  "scripts": {
    "build": "react-scripts build"
  }
}
```

## Next.js

Frame+ hosts static artifacts, so use static export mode.

Use a build/export flow that outputs static files with `index.html` entry points.

---

## 5) Frontend Runtime Contract with Frame+

When Frame+ serves your app at:

- `/frame+/{urlToken}`

Your frontend should:

1. Read `{urlToken}` from current path
2. Call Frame+ runtime APIs with:
   - `Authorization: Bearer {urlToken}`

Runtime APIs:

- `POST /frame+/events`
- `POST /frame+/actions/{action_identifier}`

Your app is also free to call your own APIs directly.

---

## 6) Asset Path Guidance

Prefer relative asset paths so hosted paths work under `/frame+/{urlToken}`.

Good:

- `./assets/logo.png`
- `assets/main.js`

Avoid hardcoding absolute site-root paths like:

- `/assets/main.js`

unless your build tool is explicitly configured for subpath hosting.

---

## 7) What Clients Can Customize Freely

- Framework and tooling choice
- Folder structure
- Styling system
- API integrations
- Build tooling

As long as the final output is static and includes `index.html`, Frame+ can host it.

---

## 8) Quick Client Checklist

1. Push frontend repo to public GitHub.
2. Ensure build produces static output (`dist`/`build`/custom `static_root`).
3. Ensure `npm run build` works in CI.
4. Register template in Frame+ with:
   - `repo_url`
   - `branch`
   - `static_root` (recommended)
5. Trigger first build.
6. Open a Frame+ session URL and validate runtime event/action calls.