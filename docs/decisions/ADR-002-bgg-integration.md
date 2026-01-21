# ADR-002: BGG Integration (deferred/stubbed)

Status
Proposed

Context
- BGG API access is pending. The app should be designed to accept a BGG adapter but be fully functional without it.

Decision
- Define an `IBggClient` abstraction and provide a no-op/mock implementation until API access and a key are available.
- When key is available, implement a production adapter behind `IBggClient` and register via DI.

Consequences
- Pros:
  - Clear separation of concerns; easy to replace stub with real client.
  - App remains fully functional offline and with seeded data.
- Cons:
  - Integration testing with the real API must be deferred until approval.

Operational notes
- Respect rate-limits and Cloudflare; implement queueing and exponential backoff in production client.
