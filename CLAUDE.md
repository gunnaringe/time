# time

Minimal world clock at time.apphub.casa. (The older clock at clock.apphub.casa
is the separate `gunnaringe/world-clock` repo — leave it alone.)

- **No build:** the app is `public/index.html` (markup, CSS, JS inline)
  plus `manifest.webmanifest`, `sw.js` and `icons/`. No dependencies, no
  third-party requests (system fonts), so it works offline. Keep it that way.
- **Deploy:** Cloudflare Worker with static assets. `wrangler.jsonc` pins the
  Worker name `time` and the `time.apphub.casa` custom domain — never rename
  the Worker, the domain is bound to it. Only `public/` is uploaded. Workers
  Builds deploys on push to `main`. Agent sessions can't deploy by hand
  (Cloudflare credentials there are read-only).
- **Service worker:** page network-first, other files cache-first with
  background refresh. Bump `CACHE` in `sw.js` only when changing the precache
  list or strategy.
- **Settings** persist in `localStorage` under `time-settings-v1`: `places`
  (`{ zone, label, name }` — `label` is the default name chosen when added,
  `name` a user override, empty = use `label`), `format` (`"24"`/`"12"`),
  `seconds`, `lang` (`auto`/`no`/`en`), `theme` (`auto`/`light`/`dark`/`hacker`). `load()` merges over `defaults()`, so a
  new setting just needs a default; bump the key only for incompatible changes.
  Render user text with `textContent`.
- **Time zones:** compare zones via `canon()` — Chrome reports legacy IDs
  (`Asia/Calcutta` for `Asia/Kolkata`). Offsets come from `longOffset` so
  half-hour zones work; day labels compare calendar dates, not offsets.
  `Etc/GMT-5` is UTC**+5**. Extra searchable cities go in `ALIASES`.
- **Defaults** (first visit): your own city, New York, Berlin, Kolkata, skipping
  duplicates of your zone — same as world-clock.
- **Themes** are `[data-theme]` blocks of CSS variables; `auto` resolves to
  light/dark from `prefers-color-scheme`. A tiny script in `<head>` sets
  `data-theme` before first paint (reads the storage key directly — keep it in
  sync if the key changes). Use the variables, not hard-coded colours.
- **Share links:** `encodeSetup()` / `decodeSetup()`, format documented above
  them, same shape as world-clock's links (`p=`, `f=`, `o=`, `t=`, `l=`). Links
  are a **public contract**: only add keys, never change existing ones.
  `takeHash()` applies a link once, strips the fragment and offers undo. Names
  in links are user input — `textContent` only.
- **i18n:** all UI strings in `I18N` (`no`, `en`), wired via `data-i18n*`.
- **Testing:** serve `public/` over http and drive it with Playwright,
  setting `timezoneId` on the context for deterministic local-zone checks.
