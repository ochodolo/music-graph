# Architecture

**Scope: Path C.** A single-machine, batch-built collaboration graph — no
streaming, no live model serving, no external recommender APIs at rank time.

## Three-stage pipeline
1. **ingest** — pull artist/collaboration data (MusicBrainz) into SQLite as
   raw bipartite tables.
2. **build** — project the bipartite data into a weighted artist–artist
   collaboration graph (networkx).
3. **serve** — rank seed artists over the precomputed graph.

## Storage & graph
- **SQLite** holds the raw ingested tables and is the durable layer.
- **networkx** holds the projected, weighted graph used for ranking.

## Popularity
Popularity is **derived from graph structure** (e.g. connectivity /
centrality), not taken from an external chart or play-count feed. This keeps
the novelty signal self-contained and reproducible from the graph alone.
