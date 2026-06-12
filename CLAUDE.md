# music-graph — CLAUDE.md

Slim top-level. Detail lives in `docs/`. Provide this file plus the
relevant `docs/` file for the active ticket.

## Status
_MG-00 scaffold ✓ · branch protection on main + develop ✓ (PR + CI,
admins excluded)._ NEXT: MG-01 — MusicBrainz ingestion (raw bipartite layer).

## Scope
Surfaces lesser-known artists *connected to your taste* via the collaboration
graph, ranked to favor the unfamiliar — NOT undiscovered/unsigned artists.
This honest boundary is load-bearing; don't let copy drift toward "underground."

## Architecture
Three-stage pipeline: `ingest` (MusicBrainz → SQLite raw) → `build` (raw →
weighted projected artist↔artist graph + popularity) → `serve` (load graph
into networkx once; per request: seeds → ego-subgraph → relevance × novelty →
ranked list). Static graph precomputed offline; only ranking is request-time.
Detail: docs/architecture.md.

## Decisions (locked)
- v1 source = MusicBrainz collaboration edges ONLY. ListenBrainz popularity =
  first post-v1 enrichment.
- co_bill (Setlist.fm/Songkick/Bandsintown) and SoundCloud shelved — no clean,
  ToS-safe, joinable per-event/underground source. Coverage is a source
  decision, not a ranking knob.
- Storage = SQLite (raw + projected) + networkx in-memory. NOT a graph DB.
- Popularity = derived structural proxy (degree / release count) primary;
  ListenBrainz secondary/later. Ranking penalizes popular nodes (Path C).
- Edge weight + surprise = PMI over shared connectors. Edges carry `evidence`
  for explainability.
- Eval = held-out collaboration-edge recovery (headline, non-circular) +
  popularity-inverse novelty + catalog coverage (descriptors). Detail: docs/eval.md.

## Workflow rules
- One ticket at a time. After closing, stop and wait for the next pick.
- Two-phase CC: Phase 1 = discovery + design + failing tests, STOP; Phase 2 =
  implement, STOP. Trivial fixes may collapse to one phase.
- GitFlow: `feature/<TICKET>-desc` and `docs/<TICKET>` off `develop`; PRs target
  `develop`. Never commit directly to `develop`/`main`.
- gitleaks is the one required check. TDD on pipeline logic.
- Roles: Claude diagnoses + drafts CC prompts; CC implements (never commits or
  pushes); Armani owns all git ops and decisions.

## Stack
Python 3.12 · requests · networkx · SQLite · pytest · ruff. Web/deploy layer
(Render or Vercel free tier) lands in a later ticket.

## Pointers
- docs/architecture.md — pipeline, schema, ranking model
- docs/eval.md — held-out edge recovery methodology
