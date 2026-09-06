# Dust &amp; Dead

A first-person zombie wave-survival shooter set in a sun-baked desert arena.
Touch-first, single HTML file, no build step, no downloads.

**Play:** https://anthonymancino2.github.io/zombie/

## What it is

The whole game is one file — [`index.html`](index.html). Everything is generated at
runtime: textures, geometry, audio, the arena layout, the zombies. The only external
dependency is Three.js r128, loaded from a CDN with two fallbacks.

- Procedural desert arena, re-generated from a seed every match
- Flow-field zombie AI with three types (walker, runner, brute)
- Six weapons unlocked by wave milestone or bought with points
- Explosive barrels, chain reactions, pickups, power-ups
- Virtual joystick + swipe-look on touch, WASD + mouse on desktop
- Dynamic resolution scaling to hold framerate on phones

## Running locally

Open `index.html` in a browser. That's it — no server required.

## Deployment

Every push to `main` triggers [`.github/workflows/pages.yml`](.github/workflows/pages.yml),
which publishes the repo root to GitHub Pages. Pages is enabled automatically by the
workflow, so there is nothing to toggle in repo settings.
