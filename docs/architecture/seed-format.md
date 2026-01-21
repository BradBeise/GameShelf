# Seed CSV Format

If you provide a CSV seed, follow this schema so the Builder Agent can write an importer that maps fields correctly.

Required columns
- UniqueId — optional (GUID). If missing, importer should generate one.
- Title — required

Optional columns (recommended names)
- BggId
- YearPublished
- MinPlayers
- MaxPlayers
- AvgRating
- ThumbnailUrl
- ShortDescription
- FullDescription
- Notes
- Tags — comma-separated
- Status — one of Owned|Wishlist|NotOwned
- Designer
- Publisher

Notes
- Use UTF-8 without BOM
- Commas inside fields should be quoted
- Dates in ISO-8601 if included

Importer behavior
- Importer should upsert by `BggId` if present, otherwise by `Title` (case-insensitive). If duplicate titles exist, importer should append a numeric suffix to `Title` or log conflicts.
