# music-graph — CLAUDE.md

Slim top-level. Detail lives in `docs/`. Provide this file plus the
relevant `docs/` file for the active ticket.

## Purpose
Portfolio-grade coding project — a working discovery tool that also demonstrates
engineering craft: clean architecture, TDD, honest eval, right-sized tooling.
"Best practice" here = judgment and restraint, not maximal process.

## Status
_MG-00 scaffold ✓ · branch protection on main + develop ✓ (PR + CI,
admins excluded)._ NEXT: MG-01 — MusicBrainz ingestion (raw bipartite layer).
Ticket arc: MG-01 ingest → MG-03 build (project + weight + popularity) →
MG-04 serve/rank → MG-05 web demo → MG-06 eval. (MG-02 = this CLAUDE.md.)

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
- v1 source = MusicBrainz collaboration edges ONLY — Spotify's recommendation/
  audio-feature endpoints were deprecated for new apps (Nov 2024); MusicBrainz
  is the open, MBID-joinable backbone. ListenBrainz popularity = first post-v1
  enrichment.
- co_bill (Setlist.fm/Songkick/Bandsintown) shelved — Setlist.fm has no
  event-roster endpoint, Bandsintown ToS forbids third-party use, Songkick is
  gated behind slow manual approval. SoundCloud shelved — gated access + ToS
  bars persistent storage/competing services + no MBID join. Coverage is a
  source decision, not a ranking knob.
- Storage = SQLite + networkx, NOT a graph DB — graph is small (low thousands
  of nodes); networkx reads more legibly than recursive SQL or a DB service.
- Popularity = derived structural proxy (degree / release count) primary;
  ListenBrainz secondary — its counts are scrobbler-userbase-biased, wrong as a
  sole global popularity-inverse signal.
- Edge weight + surprise = PMI over shared connectors; edges carry `evidence`.
- Eval = held-out collaboration-edge recovery (headline) + popularity-inverse
  novelty + catalog coverage — non-circular because the headline grades
  relevance the ranker never optimized for.

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
