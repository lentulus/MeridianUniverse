# WorldMaps

Procedural world-map generator and annotation platform. WorldMaps is the canonical owner of generated worlds; downstream Meridian\* consumer projects load worlds via a versioned contract and overlay their own annotations (borders, settlements, factions, place names) without touching WorldMaps internals. Mirrors the Meridian → MeridianWorlds upstream/consumer pattern.

**Key features:**
- Voronoi-on-sphere data model (Fibonacci or icosahedral tessellation), projection-agnostic per-region canonical coords
- Physics-based generation: tectonic plates, elevation, wind/ocean currents, temperature, humidity, rivers
- Rivers exposed two ways: per-edge `riverflow` (canonical) and per-region `riverPresence` (convenience scalar)
- Stable `RegionId` and `EdgeId` spaces: a settlement pinned to region 4218 references the same ground forever
- ISEA equal-area reprojection with analytically re-derived vector fields (wind/currents are not rotated from distorted lat/lon)
- Adaptive (region-of-interest) subdivision in the renderer from day one
- Web Worker engine from day one — UI never blocks during generation

**Architecture:**
- TypeScript monorepo, three packages: `world-contract` (types + manifest schema), `world-engine` (generator, server-side), `world-renderer` (browser, Canvas2D/WebGL)
- `apps/studio/` — dev app wiring engine + renderer together
- `apps/service/` — HTTP service exposing engine endpoints
- World Contract: JSON manifest + N binary layer blobs (raw typed-array dumps); `sha256`-tagged, immutable per `worldId`
- `worldId` is load-deterministic (UUID assigned at creation), not generation-deterministic — same seed does not guarantee same world across runs

**Consumer integration pattern:**
- Consumers depend only on `world-contract`; never on `world-engine` or `world-renderer`
- Each consumer holds a `worlds/` boundary module (contract.ts, loader.ts, API_REQUESTS.md)
- Borders snap to `EdgeId[]` (Voronoi edges) in v1; freeform `(lat,lon)` polylines reserved as a future MINOR schema addition
- Annotation storage is consumer's responsibility; WorldMaps provides stable anchor ids

**Current status (as of 2026-05-24):**
- Early scoping phase — no implementation code yet, only README, HANDOVER, and three architecture reports
- All 17 architectural decisions resolved (language, tessellation, vector frames, licensing, river exposure, ISEA defaults, worldId semantics, etc.)
- MIT licensed; algorithm reference is `freezedriedmangos/realistic-planet-generation-and-simulation` (p5.js, treated as reference not fork base)

**Next concrete actions:**
1. Land `packages/world-contract/` — TypeScript types + manifest schema + `docs/world-contract.md`
2. Add `docs/WORLDS_API_REQUESTS.md` change-request template
3. Begin engine port (~20 days to behavioral parity + quality fixes: area-weighting, tangent-frame vectors)
4. Renderer scaffold with adaptive subdivision and configurable ISEA depth

**First consumer:** MeridianWorlds (settlements, civilizations, borders)
