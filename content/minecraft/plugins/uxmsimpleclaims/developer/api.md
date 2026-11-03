---
title: API
order: 851
description: The SimpleClaims service, ClaimView and ClaimChangeEvent.
icon: code
---

`com.uxplima.claims.api.SimpleClaims` is on Bukkit's services manager.

| Method | Returns |
|---|---|
| `claimAt(Location)` | `Optional<ClaimView>` at that place |
| `claim(UUID)` | A claim by id |
| `ownedBy(UUID)` | A player's claims |
| `all()` | Every claim |
| `mayBuild(Player, Location)` | Whether the player may build there |

```java
SimpleClaims claims = Bukkit.getServicesManager().load(SimpleClaims.class);
```

`ClaimView` is a record: `id`, `name`, `owner`, `server`, `world`, `members`, `banned`, `chunks` (chunk keys),
`salePrice` (null when not for sale), and `size()`.

`ClaimChangeEvent` is called on the global thread after any claim is made, changed or deleted, on this server or
another of the network. `before()` and `after()` are the views on each side (one is empty for a new or deleted claim);
`claim()` is the id.

Related: [Integrations](../integrations/overview.md)
