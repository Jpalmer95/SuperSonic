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

## Spherical planet (Mars) — invariants to preserve

Two things break the level if changed naively:

1. **Yaw must use the player's local up axis**, i.e. `dolly.quaternion.multiply(yawQuat)` in
   `handleFlightInput`. Writing `dolly.rotation.y` yaws about *world* +Y: it is correct only at the
   north pole, loses authority at the equator and reverses in the southern hemisphere.
2. **Ground collision uses `planetTerrainRadius(dir)`**, the analytic crater+noise height, because
   the sphere mesh in `generateCity('mars')` is displaced. If you change the displacement formula,
   change `planetTerrainRadius` identically or the player clips through hills.

Content anchors to `dolly.position` at build time — `switchLocation()` places the player before
`generateCity()` so rings and drones spawn near the spawn point instead of across the whole globe.

## Flight modes

`MODE_LIST` + `gameState` drive Free / Rings / Hunt / Timed. Mode logic lives in
`startChallenge` / `onChallengeProgress` / `finishChallenge` / `updateChallenge`, deliberately
outside the level builders, so every mode works in every environment — keep it that way when
adding a mode (add an entry to `MODE_LIST`, set its `menu` label, handle any special rule in
`updateChallenge`, and add the row only if it needs menu UI; the HUD reads `MODE_LIST` generically).
Free flight is the default and must stay timer-free. Bests are stored per `envKey:mode` in
`localStorage` under `supersonic_bests_v1`.

## Deployment
Static site on GitHub Pages. Local dev server runs on port `5500`.

## Related
Cross-cutting context: `~/dev/knowledge` → `wiki/projects/game-and-xr.md`.
