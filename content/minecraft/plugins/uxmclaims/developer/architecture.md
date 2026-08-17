---
title: Architecture
order: 1001
description: Hexagonal layers, CQRS commands, and why the API looks the way it does.
icon: layers
---

uxmClaims is a hexagonal (ports and adapters) application with a CQRS split between reads and writes.
Understanding the shape explains why the API is a facade taking command objects rather than a bag of
setters.

## The modules

| Module | Contains | Depends on |
|---|---|---|
| `uxmclaims-domain` | The model — `Claim`, `ClaimMember`, `ClaimRole`, the enums, the events | nothing |
| `uxmclaims-core` | Policies and rules, such as permission resolution | domain |
| `uxmclaims-app` | The facades, the command objects and the ports | domain, core |
| `uxmclaims-infra` | The repositories — jOOQ against the database | app |
| `uxmclaims-platform-bukkit` | Everything Bukkit: commands, menus, listeners, integrations | all |

The Bukkit module is split further into `api`, `bootstrap`, `handlers` and `infra`. `api` is the part
you compile against.

Nothing in `domain` knows Bukkit exists. That is what makes the rules testable without a server.

## Ports and adapters

A **port** is an interface the application needs. An **adapter** implements it for a specific
platform.

| Port | Adapters |
|---|---|
| `EconomyPort` | `VaultEconomy`, `NoopEconomy` |
| `PermissionPort` | `LuckPermsPermission`, `NoopPermission` |
| `RegionPort` | `WorldGuardRegion`, `NoopRegion` |
| `PlaceholderPort` | `PlaceholderAPIPort`, `NoopPlaceholderPort` |
| `MapVisualizerPort` | `DynmapVisualizer`, `BlueMapVisualizer`, `Pl3xMapVisualizer`, `SquareMapVisualizer`, `NoopMapVisualizer` |

Every port has a `Noop` implementation. That is why the plugin runs with none of its soft dependencies
installed — the missing integration is not a special case, it is an adapter that does nothing.

## Commands and queries

Reads and writes go through different shapes.

**Queries** are plain methods returning data:

```java
Optional<Claim> claim = claimFacade.findByChunk(chunk);
List<Claim> owned    = claimFacade.findByOwnerUid(uuid);
int count            = claimFacade.countByOwnerUid(uuid);
```

**Commands** are objects describing an intent, handed to the facade:

```java
ClaimRenameCommand command = ClaimRenameCommand.builder()
        .claimId(claim.getId())
        .actorUid(player.getUniqueId())
        .name("New name")
        .build();

Claim renamed = claimFacade.renameClaim(command);
```

The command carries the actor, which is what lets one code path enforce permissions, charge the
economy, fire the event and post the webhook — rather than every caller remembering to.

## Side effects

Every write has an overload taking `ClaimCommandOptions`, which selects which side effects run:

```java
public enum ClaimSideEffect {
    REGION, ECONOMY, WEBHOOK, LIMITATION, PERMISSION
}
```

| Effect | Controls |
|---|---|
| `REGION` | WorldGuard overlap checks |
| `ECONOMY` | Charging the player |
| `WEBHOOK` | Posting to Discord |
| `LIMITATION` | Enforcing entitlement limits |
| `PERMISSION` | Enforcing ability and role permissions |

```java
claimFacade.createClaim(command, ClaimCommandOptions.builder()
        .without(ClaimSideEffect.ECONOMY)
        .without(ClaimSideEffect.WEBHOOK)
        .build());
```

`ClaimCommandOptions.all()` is the default, and `none()` disables everything.

This is how an admin tool, a world generator or a migration script creates claims without charging
anyone, spamming Discord or tripping a limit — without reimplementing the write path.

<Callout type="warning" title="none() also disables the permission check">

`ClaimCommandOptions.none()` turns off `PERMISSION` and `LIMITATION` along with the rest. That is
correct for a migration and wrong for anything driven by a player. Prefer `builder().without(…)` and
remove exactly the effects you mean.

</Callout>
