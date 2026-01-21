# Data Storage & Local Model

This document defines the local data model, persistence strategy, and migration guidance.

Core entities

- Game
  - UniqueId (GUID) — required, primary key
  - Title (string) — required
  - BggId (nullable int) — external id if available
  - YearPublished (nullable int)
  - MinPlayers (nullable int)
  - MaxPlayers (nullable int)
  - AvgRating (nullable double) — BGG rating when available
  - UserRating (nullable int) — 1..10, optional
  - ThumbnailUrl (string) — remote URL if available
  - LocalThumbnail (blob) — optional local cache for offline
  - ShortDescription (string) — display snippet
  - FullDescription (string) — from BGG or user-entered
  - Notes (string) — user editable
  - Tags (string) — normalized CSV or separate table for queries
  - Status (enum) — Owned, Wishlist, NotOwned
  - Source (enum) — Local, Bgg, Seed
  - CreatedAt, UpdatedAt (timestamps)

- Collection
  - Id (GUID)
  - Name (string)
  - Description (string)
  - Color (string) — optional accent color
  - ThumbnailUrl (string) — optional
  - IsDefault (bool) — e.g., "All Games" or "Wishlist"

- CollectionGame (join)
  - CollectionId
  - GameId
  - SortOrder (int)

Search index
- Use SQLite FTS5 virtual table for `Game` title, designers, publishers, and aliases.
- Populate and maintain FTS5 table whenever a Game is inserted/updated.

Persistence tech
- Recommended: Entity Framework Core with Microsoft.EntityFrameworkCore.Sqlite.
- Use migrations for schema evolution.
- For FTS5, create the FTS table using raw SQL migrations and keep it in sync with triggers or app-managed updates.

Storage rules & behavior
- Games pulled from BGG are persisted immediately (full snapshot) with Source=Bgg.
- Local-created games use Source=Local and are fully editable.
- Local edits override remote fields; background sync only updates fields the user hasn't overridden.

Import / Seeding
- Support CSV import for initial seed. Place seeders under a `Seed` service that can be run once or on-demand.
