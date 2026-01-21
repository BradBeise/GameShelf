# Search & Autocomplete

Goals
- Provide fast, local-only autocomplete and search powered by the local SQLite DB.
- Return up to 3 suggestions for the autocomplete UI.

Behavior
- Autocomplete uses only local data (FTS index) and returns max 3 suggestions.
- Search matches on `Title`, `Designer`, and `Publisher`.
- Ranking: exact/prefix title matches first, then designer/publisher, then substring matches.

Implementation notes
- Use SQLite FTS5 for tokenized, fast search. Create an FTS table that indexes `Title`, `Designer`, and `Publisher`.
- Autocomplete query example (FTS prefix search):
  SELECT UniqueId, Title, snippet(fts_table, 0, '...', '...', '...', 10) AS snippet
  FROM fts_table
  WHERE fts_table MATCH 'query*'
  LIMIT 3;

- For fuzzy matches, consider a lightweight Levenshtein fallback on small result sets (optional).
- Ensure queries are parameterized to avoid injection.

UI contract for Builder Agent
- Provide an async `ISearchService.GetAutocompleteAsync(string query, int limit = 3)`.
- Provide local-only `SearchService.SearchGamesAsync(string query, CancellationToken)` which returns paged results.
