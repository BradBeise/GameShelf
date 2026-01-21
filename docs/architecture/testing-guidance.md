# Testing Guidance for Builder Agent

Unit tests
- Use xUnit or NUnit.
- For repository tests, use an in-memory SQLite connection opened for the lifetime of the test to allow EF Core migrations and FTS queries.
- Mock `IBggClient` and `IImageService` for deterministic tests.

Integration tests
- Add integration tests that run migrations against a temp SQLite file and exercise `SearchService` and `IGameRepository` together.

Search tests
- Seed a few known records and assert `GetAutocompleteAsync("Catan")` returns expected results (limit=3).

CI considerations
- Run tests on Windows and Linux agents.

Test data
- Keep small seed fixtures in `tests/fixtures/` with CSV that matches `seed-format.md`.
