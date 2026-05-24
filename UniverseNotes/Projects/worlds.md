# Worlds

A worldbuilding browser to explore star systems, planetary bodies, and ship designs via a web interface backed by the Meridian star database.

**Key features:**
- Star browser: search and filter nearby stars by distance, spectral class, and habitability
- System view: orbital diagram with planetary body details
- Ship design: drag-and-drop GURPS Spaceships design tool

**Architecture:**
- Node.js API server (Hono)
- Vite + Three.js browser client
- Shared TypeScript types
- Python boundary layer to the star database
- DuckDB for star/system data, separate DB for world-specific data

Worlds connects to the Meridian database for star/system data and maintains its own database for world-specific content.
