# Dependency Injection & Registration Examples (Docs-only)

Recommended registrations for `MauiProgram.cs` or similar composition root. These are example registrations for Builder Agents to follow.

Services to register
- `IGameRepository` -> `EfGameRepository` (scoped)
- `ISearchService` -> `SqliteSearchService` (scoped)
- `IBggClient` -> `StubBggClient` (singleton) until real client available
- `ISeedImporter` -> `CsvSeedImporter` (scoped)
- `IImageService` -> `PlatformImageService` (scoped)
- `IBackgroundJobRunner` -> `BackgroundJobRunner` (singleton)

Example (pseudo-code)

1) Register DbContext
- builder.Services.AddDbContext<GameShelfDbContext>(options =>
  options.UseSqlite("Filename=gameshelf.db"));

2) Register repositories and services
- builder.Services.AddScoped<IGameRepository, EfGameRepository>();
- builder.Services.AddScoped<ISearchService, SqliteSearchService>();
- builder.Services.AddSingleton<IBggClient, StubBggClient>();
- builder.Services.AddScoped<ISeedImporter, CsvSeedImporter>();

3) Feature flags / configuration
- Use `IConfiguration` / `IOptions<T>` to make BGG endpoint and rate-limit settings configurable.

Secure storage
- Store API keys in platform secure storage. Register a platform-specific secret provider and use that when resolving `IBggClient`.
