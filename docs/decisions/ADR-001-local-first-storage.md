# ADR-001: Local-First Storage (SQLite + EF Core + FTS5)

Status
Accepted

Context
- App must be fully functional offline. Fast local search and autocomplete are core requirements.

Decision
- Use SQLite as the primary local store.
- Use Entity Framework Core with the SQLite provider for data access and migrations.
- Use SQLite FTS5 for the search index (managed via migrations/raw SQL).

Consequences
- Pros:
  - Developer productivity via EF Core and migrations.
  - Robust local queries and transactions.
  - FTS5 enables high-performance search and autocomplete.
- Cons:
  - Slightly larger app footprint than lightweight ORMs, and FTS5 requires some manual SQL.

Implementation notes
- Provide a `GameShelfDbContext` with `Games`, `Collections`, `CollectionGames` and a managed FTS5 table.
- Create migrations that create FTS5 virtual table and triggers or keep it in sync from application code.
