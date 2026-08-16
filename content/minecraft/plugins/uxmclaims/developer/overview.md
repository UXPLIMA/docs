---
title: Overview
order: 410
---

## Architecture

uxmClaims follows **Hexagonal Architecture** (Ports & Adapters) with **CQRS** (Command Query Responsibility Segregation):

```text
+-----------------------------------------------------------+
|                        Your Plugin                          |
+-----------------------------------------------------------+
                              |
+-----------------------------------------------------------+
|                uxmClaims API (Inbound Ports)                |
|  +---------------------------+  +-----------------------+  |
|  | Use Cases                 |  | Query Services        |  |
|  | (Commands)                |  | (ClaimLookupQuery)    |  |
|  +---------------------------+  +-----------------------+  |
+-----------------------------------------------------------+
                              |
+-----------------------------------------------------------+
|                        Domain Layer                         |
|  +---------------------------+  +-----------------------+  |
|  | Claim Entities            |  | Domain Events         |  |
|  | Claim                     |  | (ClaimCreateEvent)    |  |
|  | ClaimMember               |  | (ClaimDeleteEvent)    |  |
|  | ClaimRole                 |  |                       |  |
|  +---------------------------+  +-----------------------+  |
+-----------------------------------------------------------+
```

---

## Key Concepts

### Inbound Ports (Use Cases)

These are the entry points to the application. Each use case represents a single action:

| Use Case                  | Description               |
|---------------------------|---------------------------|
| `ClaimCreateUseCase`      | Create a new claim        |
| `ClaimDeleteUseCase`      | Delete a claim            |
| `ClaimRenameUseCase`      | Rename a claim            |
| `ClaimChunkExtendUseCase` | Add chunks to a claim     |
| `ClaimMemberBanUseCase`   | Ban a player from a claim |
| ...                       | (35+ use cases available) |

### Query Services

For reading data without modifying it:

| Query               | Description                               |
|---------------------|-------------------------------------------|
| `ClaimLookupQuery`  | Find claims by ID, chunk, location, owner |
| `PlayerLookupQuery` | Find player data                          |

### Domain Events

Events are fired when things happen. You can listen to these:

| Event                 | When It Fires            |
|-----------------------|--------------------------|
| `ClaimCreateEvent`    | After a claim is created |
| `ClaimDeleteEvent`    | After a claim is deleted |
| `ClaimMemberBanEvent` | After a player is banned |
| ...                   | (35+ events available)   |

Some events are **cancellable** - you can prevent the action from happening.

---

## API Modules

The API is split into modules you can depend on:

| Module             | Purpose                        |
|--------------------|--------------------------------|
| `uxmclaims-domain` | Core domain models and events  |
| `uxmclaims-app`    | Use cases and query interfaces |

---

## What Can You Do?

### Read Operations

- Find claims by location, chunk, or ID
- Get all claims owned by a player
- List members of a claim
- Check if a player is banned
- Get claim flags and permissions

### Write Operations (via Use Cases)

- Create and delete claims programmatically
- Add/remove members
- Change roles and permissions
- Update flags
- Manage warps

### Event Listening

- React to claim creation/deletion
- Monitor member joins/leaves
- Track chunk expansions
- Cancel unwanted actions

---

## Getting Started

1. Add the dependency to your project
2. Get the API instance from uxmClaims
3. Use queries to read data
4. Listen to events or execute use cases

See the next pages for detailed examples.

---

## Next Steps

- [📦 Adding Dependency](../developer/dependency.md) - Maven/Gradle setup
- [📖 Query API](../developer/queries.md) - Reading claim data
- [⚡ Events](../developer/events.md) - Listening to claim events
