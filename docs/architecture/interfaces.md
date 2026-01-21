# Public Interfaces & Contracts for Builder Agent

This document lists the key interfaces the Builder Agent should implement. These are contracts only (no code files are changed in the repo by the Architect).

IGameRepository
- Responsibilities: CRUD for `Game` entities, queries for lists and collection membership.
- Methods (suggested signatures):
  - Task<Game> GetByIdAsync(Guid id)
  - Task<Game> GetByBggIdAsync(int bggId)
  - Task<PagedResult<Game>> QueryAsync(QueryOptions options)
  - Task AddAsync(Game game)
  - Task UpdateAsync(Game game)
  - Task DeleteAsync(Guid id)

ISearchService
- Responsibilities: provide fast local autocomplete and search over local store.
- Methods:
  - Task<IEnumerable<AutocompleteResult>> GetAutocompleteAsync(string query, int limit = 3)
  - Task<PagedResult<Game>> SearchGamesAsync(string query, SearchOptions options)

IBggClient
- Responsibilities: abstract BGG API. Provide a mock/stub implementation until API access is available.
- Methods:
  - Task<BggGameDto?> GetGameDetailsAsync(int bggId, CancellationToken ct = default)
  - Task<IReadOnlyList<int>> GetHotListAsync(CancellationToken ct = default)
  - Task<bool> ValidateApiKeyAsync(string apiKey, CancellationToken ct = default)

ISeedImporter
- Responsibilities: import seed data from CSV/JSON into the local DB.
- Methods:
  - Task<int> ImportFromCsvAsync(Stream csvStream, ImportOptions options)

IImageService
- Responsibilities: fetch and cache remote thumbnails into `LocalThumbnail` blob and return cached path.
- Methods:
  - Task<byte[]?> DownloadAndCacheAsync(string url, CancellationToken ct = default)

IBackgroundJobRunner
- Responsibilities: schedule and run periodic background tasks (hotlist check, seed import, stale-data refresh).

Notes
- Keep interfaces small and testable.
- Use DI to register implementations. See `di-and-registration.md` for examples.
