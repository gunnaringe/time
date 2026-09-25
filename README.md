# 🕐 Tid · Time

A minimal world clock: one row per place, sized to fill the screen, made for a
wall-mounted tablet or TV as much as a phone.

**▶ [time.apphub.casa](https://time.apphub.casa)**

## Features

- 🌍 **Your places** — search by city (`Mumbai`, `San Jose`, `Trondheim`), time
  zone ID (`America/Denver`) or offset (`UTC+5`). Rename any place; clear the
  name to get the default back. Reorder with the arrows.
- 🕒 **At a glance** — each row shows today/tomorrow/yesterday and the offset from
  your own time (half-hour zones included). Your own zone is marked with a dot and
  shows the full date. Night hours are dimmed a little.
- ⚙️ **12/24 h, optional seconds, Norwegian or English.**
- 📺 **Made for always-on screens** — fullscreen button, keeps the screen awake,
  controls fade away, and the layout shifts by a pixel now and then against burn-in.
- 📲 **Installable, works offline**, no accounts, no tracking — settings live in
  `localStorage`.

## Development

Everything is in [`public/index.html`](public/index.html) (plus manifest, service worker and
icons). No build step, no dependencies:

```sh
npx wrangler dev          # or: cd public && python3 -m http.server
```

## Deploy

Served as a Cloudflare Worker with static assets, with `time.apphub.casa` as a
custom domain (see [`wrangler.jsonc`](wrangler.jsonc)). Workers Builds deploys
on push to `main`, or by hand:

```sh
npx wrangler deploy
```
