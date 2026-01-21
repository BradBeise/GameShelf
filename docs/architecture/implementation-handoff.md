# Implementation Handoff for Builder Agent

This file summarizes actionable tasks for Builder Agents to implement GameShelf from the architecture docs.

Priority (MVP)
1. Local DB scaffolding: EF Core `GameShelfDbContext`, migrations, and models per `data-storage.md`.
2. Search service: FTS5-backed `SearchService` and `ISearchService.GetAutocompleteAsync` returning up to 3 suggestions.
3. Library UI: Shell routes, `CollectionView` list, game detail screen, and Add flow with local-only autocomplete.
4. Collections: create, edit, assign games, default "All Games" and "Wishlist" views.

Secondary
- CSV seed importer (seed one-time import).
- Background job runner for monthly hotlist check (stubbed if no API).
- BGG adapter wiring (implement `IBggClient` when key available).

Interfaces to implement
- `IGameRepository` — CRUD operations and query methods.
- `ISearchService` — `GetAutocompleteAsync`, `SearchGamesAsync`.
- `IBggClient` — stubbed now; real implementation later.

Testing
- Write unit tests for repository and search behavior (use in-memory SQLite for tests).

Deliverables
- EF Core model and migrations
- Search service using FTS5
- Screens per `ui-navigation.md`
- DI registrations and small README on how to seed DB from CSV
