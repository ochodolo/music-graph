# music-graph
Surfaces lesser-known artists connected to your taste via the
collaboration graph, ranked to favor the unfamiliar — not undiscovered
or unsigned artists.

[ live demo ]  ·  [ how it works ]  ·  [ eval results ]

## The idea
Mainstream recommenders rank by acoustic/behavioral similarity. This ranks
by *cultural* connection (who collaborates with whom) and optimizes for
relevance × novelty, not similarity alone.

## Architecture
ingest (MusicBrainz → SQLite) → build (weighted projected graph) →
serve (rank seeds over the precomputed graph). See docs/architecture.md.

## Eval
Headline: held-out collaboration-edge recovery (relevance the ranker never
saw), reported against popularity-inverse novelty and catalog coverage.
See docs/eval.md.

## Run
<!-- fill as MG-01 lands -->
