# Server-Side Storage Options

**Date:** 2026-05-26  
**Projects:** Meridian, WorldMaps, Worlds, ColonyModels  
**Status:** For consideration

---

## What Needs Storing

The four projects have radically different data profiles. Any storage recommendation must treat them separately, and heterogeneous solutions are likely better than a single unified approach.

| Data | Owner | Profile | Size | Write pattern |
|---|---|---|---|---|
| Meridian parquets | Meridian | Immutable blobs, spatial, compressed | 70–140 GB | Build-time only; never at runtime |
| WorldMaps base worlds | WorldMaps | Immutable typed-array blobs + JSON manifests, sha256 content-addressed | ~10–300 MB/world | Write once per generation |
| WorldMaps annotations | WorldMaps / consumer | Relational mutable; anchored to a worldId (point, region, border, polyline); e.g. colonies, bases, routes | Small–medium | CRUD per user/campaign |
| Worlds campaign data | Worlds | Relational mutable (settlements, ships, slots) | Small–medium | CRUD; reads dominate |
| ColonyModels simulations | ColonyModels | Event-sourced (append-only events, derived snapshots) | Small | Append-heavy during runs; batch read for replay |

---

## Scenario A — Same Machine (Client and Server Co-located)

Everything local. No network, maximum simplicity, full storage budget on local disk.

### Meridian parquets
The current approach is optimal: parquet files on the Lexar drive, queried via embedded DuckDB. The Z-order spatial indexing and predicate pushdown give fast queries with zero index maintenance. The only risk is the Lexar drive being external and removable.

- **Litestream → S3/B2** as a one-way backup of `manifest.db` is cheap insurance against drive loss.

### WorldMaps base worlds (blobs)
Plain filesystem. The sha256 content-addressing means a flat layout of `{worldId}/manifest.json` + `{worldId}/layers/{name}.bin` works perfectly. The studio app and HTTP wrapper serve static files.

Optional: **MinIO** (single binary, S3-compatible object server) if you want a consistent blob API that transfers unchanged to Scenario B/C without client code changes. Mount the Lexar drive as its storage root.

### WorldMaps annotations
SQLite, same database as Worlds or a dedicated `worldmaps.db`. Annotations are small relational rows (worldId, anchorType, anchorIds, payload JSON) — a natural fit alongside Worlds' settlement and ship tables. The key design question is whether annotations belong to a campaign (owned by Worlds) or to a world (owned by WorldMaps); same-machine makes that distinction moot.

### Worlds + ColonyModels (relational/event data)
SQLite. Both projects already target it (`world.db` in Worlds, `better-sqlite3` via ADR-0001 in ColonyModels). Same-machine means no concurrency problem, no network overhead, no ops burden.

**PocketBase** is worth knowing about: a single binary that embeds SQLite + REST API + admin dashboard + optional auth. If you ever want a browser-based admin view of settlements or ships without writing bespoke routes, PocketBase can replace the hand-rolled Express/Hono data layer entirely.

---

## Scenario B — Separate Server, Single User

One server, one user. The write serialization of SQLite is fine; durability and remote access matter more than concurrency.

### Meridian parquets

| Option | Approach | Notes |
|---|---|---|
| Rsync to server | Copy `MeridianData/` once; re-sync on new builds | ~140 GB disk on server; zero ongoing cost; embedded DuckDB works as today |
| Object storage + remote DuckDB | Push parquets to Cloudflare R2 ($0.015/GB, **zero egress**) or Backblaze B2 ($0.006/GB); DuckDB reads over HTTP via `httpfs` extension | ~$0.84–2.10/month storage; removes 140 GB disk requirement from server |
| MotherDuck | Cloud-hosted DuckDB that queries S3/R2 parquets directly; connect via connection string | Per-query pricing; abstracts the DuckDB process entirely |

For single-user, rsync + embedded DuckDB is the simplest path. R2 + httpfs is worth doing if you're already paying for R2 — it eliminates the large disk requirement on the server host.

### WorldMaps base worlds (blobs)

| Option | Notes |
|---|---|
| MinIO on the server | S3-compatible, self-hosted, zero per-request cost; consistent API if you built against it in Scenario A |
| Cloudflare R2 | Zero egress fees; important when worlds can be 100 MB+; free tier 10 GB/month then $0.015/GB |
| Backblaze B2 | Cheaper storage ($0.006/GB) but egress fees; neutralise them by fronting with Cloudflare CDN proxy |
| IPFS + Pinata | Content-addressed storage maps cleanly to the sha256 scheme — worlds are naturally CIDs; Pinata pins them; unusual but architecturally very clean |

### WorldMaps annotations + Worlds + ColonyModels (relational/event data)

Annotations are small rows and sit naturally alongside the other mutable relational data. The key ownership question — whether an annotation belongs to a world (WorldMaps) or a campaign (Worlds) — should be resolved before choosing a schema, but storage-wise they land in the same bucket.

| Option | Notes |
|---|---|
| SQLite + Litestream | Keep SQLite on server; Litestream replicates every WAL write to S3/B2/R2; point-in-time recovery; zero code changes |
| Turso / libSQL | SQLite-compatible; adds HTTP API and optional embedded replicas; query from laptop or mobile without a VPN; still single-writer |
| PocketBase | One binary: SQLite + REST + auth + file storage + admin dashboard; runs as a persistent service; good for a server accessed via the web |
| Fly.io + persistent volume | Deploy existing Hono server on Fly.io with a 3 GB persistent volume for `world.db`; $2–5/month; familiar Node.js deployment; SQLite still works fine |

ColonyModels' append-only event log is also a natural fit for **EventStoreDB** if you want pure event sourcing semantics (streams, projections, subscriptions). Likely overkill while it's a research sandbox, but worth noting if it grows into a multi-run analysis tool.

---

## Scenario C — Separate Server, Multiple Users (Public Hosting)

SQLite's single-writer model becomes a hard constraint. Campaign data (Worlds) is now per-user; Meridian is still a shared read-only reference. WorldMaps worlds might be shared or per-user depending on whether users can generate their own.

### Meridian parquets

| Option | Notes |
|---|---|
| Cloudflare R2 + Workers | Parquets in R2; a Cloudflare Worker handles manifest queries and serves binary layers with zero egress fees; DuckDB-WASM can run spatial queries in the browser against R2 if CORS is configured |
| R2 + server-side DuckDB | DuckDB on the API server reads R2 via `httpfs`; all Meridian queries routed through your API which handles auth and user context — recommended |
| MotherDuck | Cloud DuckDB SaaS; queries parquets directly; no DuckDB process to manage; per-query pricing |
| Dedicated server with large disk | Hetzner offers servers with 2–8 TB disks for €30–80/month; 140 GB of Meridian is trivial; simplest ops if self-hosting everything |

R2 + server-side DuckDB is likely the strongest option: zero-egress R2 turns 140 GB of spatial data into a trivially cheap read layer, and all queries are gated through your API.

### WorldMaps base worlds (blobs)

| Option | Notes |
|---|---|
| Cloudflare R2 + CDN caching | Worlds are immutable; R2 + Cloudflare caching means a popular world is cached globally at zero egress cost; presigned URL access works for direct client downloads |
| Supabase Storage | If already on Supabase, storage buckets give S3-compatible blobs with row-level security — world access can be scoped per-user with the same auth tokens |
| IPFS + Pinata | Public worlds become globally addressable permanent links; $0.15/GB/month on Pinata; aligns with sha256 content-addressing |

### WorldMaps annotations + Worlds + ColonyModels (relational/event, now multi-user)

In the multi-user case, annotations gain a `userId` dimension: a colony placed by one user should not be visible to another unless the campaign is shared. This is where row-level security (Supabase RLS) or equivalent per-row ownership becomes valuable — annotations, settlements, and ships all follow the same pattern.

| Option | Notes |
|---|---|

| Option | Notes |
|---|---|
| **Supabase** | PostgreSQL + auth + storage + real-time + row-level security; free tier (500 MB DB, 1 GB storage, 50k MAU); TypeScript SDK; RLS scopes settlements to users with no extra middleware — likely the strongest all-in-one answer |
| Neon | Serverless PostgreSQL with database branching; great for dev/prod parity and scale-to-zero billing; no built-in auth — pair with Clerk or Auth0 |
| Railway | Host existing Hono server + PostgreSQL instance; simplest migration from the current codebase; no serverless abstraction |
| PlanetScale | MySQL-compatible serverless (Vitess); excellent sharding story if ColonyModels scales to millions of simulation runs; schema branching; now paid-only |
| CockroachDB Serverless | PostgreSQL-compatible, multi-region, automatic sharding; overkill for current scale but future-proof for global distribution |
| D1 (Cloudflare) | SQLite at the edge inside Cloudflare Workers; zero cold starts; limited to 10 GB per database; only works if the whole server moves to Workers |

ColonyModels' `runs`/`events`/`snapshots` tables with a `user_id` FK migrate cleanly to PostgreSQL — no architectural changes required.

---

## Heterogeneous Combinations Worth Calling Out

### Combo 1 — Local-first, minimal ops (Scenario A)
- SQLite everywhere (Worlds + ColonyModels)
- Filesystem for WorldMaps blobs
- DuckDB + Lexar parquets for Meridian
- Zero hosting cost, zero ops

### Combo 2 — Portable self-hosted (Scenario B)
- Litestream → R2 for SQLite durability (Worlds + ColonyModels) — zero code changes
- MinIO on the server for WorldMaps blobs (consistent blob API now and later)
- DuckDB + rsynced parquets for Meridian (140 GB disk on Hetzner is cheap)
- Estimated cost: ~$6–10/month hosting + $1–2 R2 backup

### Combo 3 — S3-native, no persistent disk (Scenario B or C)
- Cloudflare R2 for Meridian parquets and WorldMaps blobs — zero egress on both
- MotherDuck for DuckDB queries against R2 (no DuckDB process to manage)
- Turso for mutable SQLite data (HTTP API, edge-replicated)
- Entirely network-hosted; no persistent server disk required

### Combo 4 — Supabase core + R2 blobs (Scenario C)
- Supabase (PostgreSQL + RLS + auth) for all mutable campaign data
- Cloudflare R2 for Meridian parquets and WorldMaps blobs
- DuckDB on API server reads R2 parquets via `httpfs`
- Coherent public-facing stack: Supabase controls access; R2 serves the data

### Combo 5 — IPFS for immutables (Scenario B or C — experimental)
- WorldMaps worlds pinned to IPFS via Pinata — sha256 content-addressing maps directly to CIDs; worlds become globally addressable permanent links
- Meridian parquet sectors pinned similarly (sectors are atomic and immutable)
- PostgreSQL or SQLite for mutable data
- Unusual, but architecturally very clean — immutable data lives on the content-addressed web

---

## Security in Remote Server Scenarios

### Scenario B — Single User, Remote Server

The goal here is keeping the server off the public internet entirely, or restricting it to a single known identity without building a full auth system.

**Network-level access control (preferred)**

| Option | Notes |
|---|---|
| **Tailscale** | Zero-config WireGuard mesh VPN; free for personal use (100 devices); server is invisible to the public internet; access from any device (laptop, phone) without opening firewall ports; the strongest single-user option |
| SSH tunnel | `ssh -L 8080:localhost:8080 user@server` proxies the API through an encrypted SSH connection; no extra software; works anywhere SSH is allowed |
| Cloudflare Access | Zero-trust layer in front of any origin; authenticate via Google/GitHub/email OTP before a request reaches the server; free tier covers one application |
| IP allowlist | Firewall rule restricts the API port to your home/office IP; simple but breaks when your IP changes |

Tailscale is the standout option. The server never needs a public port for the API; only SSH (for administration) and optionally HTTPS need to be exposed. Everything else travels over the Tailscale mesh.

**Transport security**

Always TLS, even for a single-user server. **Caddy** is the simplest path: it provisions and renews Let's Encrypt certificates automatically and its config is a few lines. If the server is Tailscale-only, Caddy can use Tailscale's built-in HTTPS provisioning (no public domain required).

**Secrets**

`.env` files are fine at this scale. Never commit them. For a hosted server (Fly.io, Railway), use the platform's secrets store rather than copying `.env` to the server.

---

### Scenario C — Multiple Users, Public Hosting

Public exposure means authentication, authorisation, and isolation are all required.

**Authentication — who is this user?**

| Option | Notes |
|---|---|
| **Supabase Auth** | Built-in if using Supabase; email/password, magic link, OAuth (Google, GitHub, Discord); JWTs issued automatically; tightest integration with RLS |
| **Clerk** | Modern hosted auth; excellent DX; handles sessions, JWTs, MFA, social login; framework-agnostic; works well with any backend including Hono |
| **Better Auth** | TypeScript-native, self-hosted, framework-agnostic; good fit if you want auth in your own codebase without a third-party service |
| Auth0 | Enterprise-grade; more complex and expensive than needed here |
| Passkeys / WebAuthn | Passwordless, phishing-resistant; can be layered on top of any of the above as a credential type |

If the stack is Supabase, use Supabase Auth — the JWTs it issues flow directly into RLS policies with no extra wiring. Otherwise Clerk is the lowest-friction standalone option.

**Authorisation — what can this user see?**

The data splits into two access classes:

*Shared read-only data (Meridian parquets, WorldMaps base worlds)*  
All authenticated users read the same data. The main concerns are rate limiting and preventing expensive query abuse (DuckDB spatial queries on 140 GB of parquets can be slow). An API middleware layer that validates the JWT and enforces per-user request quotas is sufficient.

*User-owned mutable data (annotations, settlements, ships, simulation runs)*  
Each row must be scoped to its owner. Two patterns:

- **PostgreSQL row-level security (RLS):** Policy on every table: `USING (user_id = auth.uid())`. The database enforces isolation; no application-layer mistake can leak another user's data. Supabase exposes this directly. With Neon or raw PostgreSQL, RLS policies are written manually but work the same way.
- **Middleware ownership checks:** Simpler to understand; every route handler verifies `row.user_id === req.user.id` before returning data. More code, and a missed check is a data leak — less safe than RLS as a single point of enforcement.

**Shared campaigns** (multiple users annotating the same world together) require a `campaign` entity with a membership table. Annotations then scope to `campaign_id` rather than `user_id` directly, and the RLS policy checks campaign membership. Worth designing for even if not needed on day one.

**WorldMaps base world access**  
Base worlds (immutable blobs) are either public or access-gated:
- **Public worlds:** Serve directly from R2 with a public bucket or presigned URLs; no auth check needed on the blob itself.
- **Private/generated worlds:** Issue short-lived presigned URLs from the API after the JWT is validated; the blob URL itself is time-limited and unguessable.

**Transport and origin security**

- HTTPS everywhere — non-negotiable for a public server. Caddy or the hosting platform (Fly.io, Railway, Cloudflare) handles certificate provisioning.
- CORS: lock `Access-Control-Allow-Origin` to your client domain(s) only; do not use wildcard `*` once auth is in place.
- Set `Secure`, `HttpOnly`, and `SameSite=Strict` on any session cookies.

**Rate limiting**  
Meridian spatial queries and WorldMaps world generation are CPU/IO-heavy. Add per-user rate limiting on those endpoints (a simple in-memory token bucket via `hono/middleware` or a Redis-backed counter for multi-instance deployments) to prevent a single user from monopolising the server.

**Secrets management at hosting scale**

| Option | Notes |
|---|---|
| Platform secrets store | Fly.io, Railway, Render all have first-class secret injection; no `.env` file on the server |
| Doppler / Infisical | Centralised secrets manager; syncs to hosting platforms; useful once secrets span multiple services |
| Supabase service role key | Never expose this to the client — it bypasses RLS; server-side only |

---

## The Central Tension

The main fork is whether **Meridian data lives on the server's disk or in object storage**. At 70–140 GB it drives server sizing decisions, but its immutable, read-heavy nature makes it a natural fit for CDN semantics.

For Scenario C specifically, R2 + DuckDB httpfs is strongly preferable over mounting parquets on the application server disk. Zero-egress R2 turns 140 GB of spatial data into a trivially cheap read layer.

For mutable application data the progression is clear:
- **Scenario A:** SQLite
- **Scenario B:** SQLite + Litestream, or Turso
- **Scenario C:** PostgreSQL via Supabase or Neon

The schema between ColonyModels and Worlds does not need to be unified — they can share a server but use separate databases. Both are small enough that consolidation would be premature.
