# Database Migrations & FTS5 Setup Guidance

This document describes recommended migration steps and patterns for maintaining the SQLite schema and the FTS5 search index.

1) Use EF Core migrations
- Use `dotnet ef migrations add` during development to produce migration files.
- Keep migration steps small and additive (add columns, create tables). Avoid destructive changes in a single migration.

2) Creating FTS5
- EF Core doesn't directly scaffold FTS5 virtual tables. Create the FTS5 table in a migration using `migrationBuilder.Sql("CREATE VIRTUAL TABLE ... USING fts5(...)");`.
- Example FTS5 creation (conceptually):
  CREATE VIRTUAL TABLE games_fts USING fts5(Title, Designer, Publisher, content='Games', content_rowid='rowid');

3) Keeping FTS5 in sync
- Option A (recommended): app-managed sync. Whenever a `Game` is added/updated/deleted, update the FTS table via SQL statements in the repository or a dedicated service. This avoids complex triggers.
- Option B: use SQLite triggers in a migration to keep FTS table updated automatically. Triggers must be carefully tested across migrations.

4) Migrations & Backups
- Provide a lightweight migration runner at app startup that runs pending migrations once.
- For large migrations, provide a progress UI and/or run on background thread to avoid blocking UI startup.

5) Testing Migrations
- Use an in-memory SQLite file for unit tests with `SqliteConnection` kept open to maintain schema across contexts.
