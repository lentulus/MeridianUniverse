# ColonyModels

R&D sandbox for prototyping interstellar-colony simulation models — demographics, economy, infrastructure, governance — with side-by-side comparison in an interactive viewer. Models proven here are promoted into MeridianWorlds; ColonyModels is upstream research, not production.

**Phase 1 model:** Turchin's basic demographic-fiscal ODE (Eq 7.4, *Historical Dynamics* 2003) — population N and accumulated state resources S coupled into a ~200-yr secular cycle. Client runs RK4 integration; server stores the append-only event timeline and a snapshot cache.

**Key features:**
- Differential-equation models advanced in discrete UI ticks (default: 1 month)
- Parameter sliders adjustable mid-run; branching drops future history and replays forward
- Rewind via the replay engine — pure function over (run, events) → snapshots
- 2D recharts plot of N and S over time with click-to-rewind cursor
- SQLite persistence of runs, events, and snapshot cache

**Architecture:**
- npm workspace monorepo: `client/`, `server/`, `shared/`
- Client: Vite + React + Zustand state store (runs RK4, owns simulation)
- Server: Express + better-sqlite3 (durable storage only, no ODE code)
- Shared: TypeScript types for Run, Event, Snapshot, ParamsC
- Port 5173 (client dev), 8001 (server) — separate from MeridianWorlds on 8000

[Backing Maths](turchin-equations.md) from the work of Peter Turchin.

**Current status (as of 2026-05-24):**
- Slice 0 setup complete: test harness (vitest, fast-check, supertest) wired across all workspaces; `shared/` workspace created
- No production code yet; three intentionally-red regression anchors are the next step
- 16 planning docs in `docs/design/` including full Phase 1 design, checklist, test specs, ADRs, and risk register
- Large block of work uncommitted (planning docs + shared workspace + vitest configs)

**Planned slices:**
0. Test harness + red anchors → 1. Types + integrator → 2. Replay engine → 3. Persistence + HTTP → 4. Client store → 5. UI controls → 6. Polish

**Forward seams (not Phase 1):**
- Exogenous resupply event / `resupplyRate` param
- Option D: class-structured model widening StateC to (P, E, S)
- MeridianWorlds integration (ColonyModels snapshot stream as spatial content)
- Non-Turchin models: Allee effect, resource-limited Lotka-Volterra, Ricker/Beverton-Holt
