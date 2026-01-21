# EF Core Model Shapes (Docs-only)

Below are the recommended EF Core entity shapes represented as documentation. These are guidance for the Builder Agent when creating actual model classes and DbContext.

Game (entity)
- UniqueId: Guid (PK)
- Title: string (required)
- BggId: int? (nullable)
- YearPublished: int?
- MinPlayers: int?
- MaxPlayers: int?
- AvgRating: double?
- UserRating: int?
- ThumbnailUrl: string?
- LocalThumbnail: byte[]? (blob)
- ShortDescription: string?
- FullDescription: string?
- Notes: string?
- Tags: string? (CSV) or relationship to `Tag` table
- Status: enum { Owned, Wishlist, NotOwned }
- Source: enum { Local, Bgg, Seed }
- CreatedAt: DateTimeOffset
- UpdatedAt: DateTimeOffset

Collection (entity)
- Id: Guid (PK)
- Name: string
- Description: string?
- Color: string? (hex)
- ThumbnailUrl: string?
- IsDefault: bool

CollectionGame (join)
- CollectionId: Guid
- GameId: Guid
- SortOrder: int

FTS Index
- Create an FTS5 virtual table that indexes `Title`, `Designer`, and `Publisher` (if present in dataset).

DbContext guidance
- DbSet<Game> Games
- DbSet<Collection> Collections
- DbSet<CollectionGame> CollectionGames

Notes
- Keep nullable fields nullable to allow partial BGG records.
- Add appropriate indices on `BggId`, `Title` and timestamp fields for performance.
