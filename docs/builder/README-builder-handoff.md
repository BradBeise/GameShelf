# Builder Handoff — GameShelf

Purpose
- Provide a concise checklist and step-by-step implementation plan for Builder Agents to implement GameShelf per Architect guidance.

Priority (MVP)
1. Scaffold EF Core models and `GameShelfDbContext` per `efcore-models.md`.
2. Add migrations and ensure FTS5 table is created (see `db-migrations.md`).
3. Implement `IGameRepository` with upsert semantics and FTS maintenance hooks.
4. Implement `ISearchService` using FTS5 and provide `GetAutocompleteAsync(string, int=3)`.
5. Implement UI pages per `ui-navigation.md` and wire search/autocomplete to UI.
6. Implement local-only `Add new game` flow and collection management (default `All Games` and `Wishlist`).

Secondary tasks
- CSV seed importer (`ISeedImporter`) and a one-shot import command.
- Background job runner for periodic hotlist checks (stub `IBggClient` until API key available).
- Image caching via `IImageService`.

Developer notes
- Keep all logic testable and register services via DI. Use the examples in `di-and-registration.md`.
- Do not hard-code API keys; use platform secure storage.

How to verify MVP
- App runs offline and shows seeded games.
- Autocomplete returns up to 3 local suggestions.
- User can create, edit, and assign games to collections.

Deliverables
- EF Core model classes and migrations
- Implementations for `IGameRepository`, `ISearchService`, `ISeedImporter`
- UI pages and navigation
- Unit and integration tests per `testing-guidance.md`
