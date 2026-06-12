# Evaluation

## Headline metric
**Held-out collaboration-edge recovery.** Hide a subset of real
collaboration edges before building the graph, then measure how well the
ranker surfaces those held-out connections — relevance the ranker never saw
at build time.

## Descriptors
Reported alongside the headline to characterize *what kind* of results the
ranker produces:
- **Novelty** — popularity-inverse: rewards surfacing less-connected
  (lesser-known) artists over hubs.
- **Catalog coverage** — the share of the catalog the ranker is capable of
  surfacing across seeds, not just a popular core.
