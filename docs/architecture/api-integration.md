# API Integration (BGG) — Design & Stub

Context
- BGG API access is pending approval. This document outlines how integration will work and provides a stubbed adapter so the app can be wired without the key.

Principles
- Minimize calls: fetch details on-demand or when user explicitly requests.
- Cache aggressively: persist any fetched game details to local DB.
- Respect rate limits: queue requests and use exponential backoff; runtime should be configurable.

Adapter responsibilities
- `IBggClient` interface (stubbed until key available):
  - Task<GameDto> GetGameDetailsAsync(int bggId)
  - Task<List<int>> GetHotListAsync()
  - Task<bool> ValidateApiKeyAsync(string apiKey)

Stub behavior until API granted
- Return NotImplemented / Mock data for `GetGameDetailsAsync` when no API key.
- Allow the Builder Agent to swap a real implementation behind `IBggClient` using DI once key is available.

Background tasks & seeding
- If a CSV seed is provided, prefer import over remote queries.
- If CSV unavailable, a periodic background job may query a BGG hotlist endpoint (monthly) to fetch candidates for seeding.

Error handling
- Exponential backoff on transient failures and Cloudflare blocks.
- Provide clear telemetry: last-success, last-error, rate-limit counters.

Security
- Store API keys in secure storage (platform-specific secure vault). Not in plaintext.
