# ADR-003: Data Model (Game & Collection shape)

Status
Accepted

Context
- Need a clear, minimal, and extensible shape for `Game` and `Collection` entities that supports local creation and BGG-sourced data.

Decision
- Adopt the data model defined in docs/architecture/data-storage.md: `Game` with UniqueId, Title, BggId, thumbnails, players, descriptions, notes, tags, status, source, timestamps; `Collection` with list semantics via `CollectionGame`.

Consequences
- Pros:
  - Meets offline and editing requirements.
  - Clear separation of local vs external data via `Source` enum.
- Cons:
  - Slightly larger schema surface area, but fields are nullable to support partial data.

Migration guidance
- Use EF Core migrations to add fields; keep backward-compatible column additions.
