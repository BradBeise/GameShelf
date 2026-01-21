# GameShelf Architecture — Overview

Purpose: document the architecture of GameShelf so Builder Agents can implement a modular, maintainable, offline-first mobile app.

Key principles
- Local-first: app must work offline with a local SQLite database as the source of truth.
- Minimal external reliance: BGG API usage is optional and rate-limited; design assumes it's unavailable initially.
- MVVM: clear separation between UI, domain logic, and persistence.
- Small, focused docs: each subsystem has its own file in docs/architecture/.

High-level components
- UI: .NET MAUI Shell-based app, Android-first Material styling.
- Domain: models for `Game`, `Collection`, `CollectionGame` and services for search, collections, and sync.
- Persistence: SQLite (EF Core + SQLite provider) with an FTS5-backed search index.
- API integration: BGG adapter (stubbed until API access approved). Uses background queue and exponential backoff.
- Background tasks: periodic maintenance (seed import, hotlist check) and optional background refresh.

Dependencies (recommended)
- .NET 8+, .NET MAUI
- Microsoft.EntityFrameworkCore.Sqlite
- SQLite FTS5 (via provider or raw SQL)
- CommunityToolkit.Mvvm for ViewModels

Next docs: see data-storage.md, search-autocomplete.md, api-integration.md, ui-navigation.md
