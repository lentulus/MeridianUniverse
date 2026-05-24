# Meridian

A read-only, pre-generated reference database of the stellar neighbourhood — every star system, companion, planet, and world-physical record within 1000 parsecs of Sol, fixed at a canonical time-0.

Built from the ATHYG v3.3 catalogue and extended with synthetic companions and world-physical data generated from GURPS Space 4e rules.

**Key features:**
- ~172 million star systems out to 1000 pc
- 729 sectors on a 9×9×9 grid of 100-parsec cubes
- Bodies, satellites, and world-physical data for every system
- Immutable Parquet files — no write path at runtime

Meridian is the ground truth for fiction, campaigns, and tools. It is not a game engine; narrative state belongs to the campaign layer that references Meridian by system ID.
