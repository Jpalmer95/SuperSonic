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

## HUD layout

The HUD is four camera-space quads (`createHudPanel`) placed by `layoutHud()` from the frustum at
z = -0.5 — aspect is clamped to 1.0–1.6 because a headset eye is near-square while a desktop canvas
is wide, and the unclamped value pushes the side panels off-screen in VR. Add a new readout by
creating a panel in `createHUD()`, positioning it in `layoutHud()`, and drawing it in `updateHUD()`;
don't reintroduce a single full-width slab. Panels use `depthTest: false` and `renderOrder: 999`
so they always draw over the world and the speed vignette (996).

## Performance budget (this scene is VR-first)

Measured on a software renderer, so ratios matter more than absolute fps — each rule below was
worth a measurable chunk of frame time:

- **Never add a light per prop.** Every `PointLight` is evaluated per fragment in every material;
  50 lamp lights cost ~45% of the frame (8.8 → 16.4 fps with them off). Use an emissive mesh plus an
  additive `Points` glow — one draw call, no shader cost.
- **Freeze the shadow map.** All casters are static, so `renderer.shadowMap.autoUpdate = false` and
  `needsUpdate = true` once per `generateCity()`. 2048² is plenty.
- **Gate HUD redraws.** Each redraw re-uploads a canvas texture; `updateHUD()` is throttled to ~15 Hz
  and skips panels whose content signature is unchanged. Never draw the HUD every frame.
- **Reuse scratch vectors** (`_sUp`, `_sDir`, `_sYaw`, …) in the per-frame path. Allocating
  `new Vector3()` per frame produces periodic GC hitches on long flights.
- **Cache HDR environments by URL** (`envCache`) — refetching + PMREM per level switch causes a
  visible hitch every time you travel.
- **No `backdrop-filter` over the canvas** — it forces the compositor to re-blur every frame.

## Deployment
Static site on GitHub Pages. Local dev server runs on port `5500`.

## Related
Cross-cutting context: `~/dev/knowledge` → `wiki/projects/game-and-xr.md`.
