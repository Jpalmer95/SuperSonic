# AGENTS.md — SuperSonic

**A high-speed, browser-based WebXR flight simulator and combat game.** Explore procedural cities,
a planet, or real-world locations generated instantly from OpenStreetMap data.

**Live: https://jpalmer95.github.io/SuperSonic/** (GitHub Pages)

## Flight model — the reusable part
Hand-directed flight, "Iron Man" style: **point your hands where you want to go and hold the
triggers.** No throttle stick, no cockpit abstraction.

This model was prototyped here and **carried into `oracle-playground`** as its dual-hand "Oracle
flight" (independent thrusters). If you change the flight feel, note that a second project
depends on the concept, even though the code is not shared.

## Environments
- **Metro City** — dense procedural metropolis
- **Planet** — globe-scale terrain
- **Real world** — generated from OpenStreetMap data

## Deployment
Static site on GitHub Pages. Local dev server runs on port `5500`.

## Related
Cross-cutting context: `~/dev/knowledge` → `wiki/projects/game-and-xr.md`.
