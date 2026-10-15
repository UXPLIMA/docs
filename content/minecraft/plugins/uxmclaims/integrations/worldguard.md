---
title: WorldGuard
order: 803
description: Keeping claims out of WorldGuard regions and disabled worlds.
icon: shield-alert
---

With WorldGuard installed, uxmClaims refuses to claim land that lies in a WorldGuard region. It never
creates or changes regions. Worlds in `disabledWorlds` are refused at the same points, with or without
WorldGuard.

## Settings

```yaml
generalSettings:
  worldGuardSupport: true
  disabledWorlds:
    - 'arena'
    - 'spawn'
    - 'world_nether'
```

| Key | Default | Meaning |
|---|---|---|
| `worldGuardSupport` | `true` | Check WorldGuard regions. Read at startup |
| `disabledWorlds` | `arena`, `spawn`, `world_nether` | Worlds where nobody may claim, with or without WorldGuard. Matched ignoring case, applied by `/claim reload` |

## What is checked

| Action | Points checked |
|---|---|
| Creating a claim | The spot where the claim is created |
| Adding a chunk | The four corner columns of that chunk, at Y 64 |
| Placing the claim block | The block's position |
| The claim map menu | Shows a chunk as not claimable by the same corner test |

A point counts when it is inside any region, whatever that region's flags, owners or members. A
refused action shows `errorNotClaimableArea` (*"This area cannot be claimed."*). Admin commands are
checked too.

Because the test samples points, a small region that covers none of them, such as a spawn platform
in the middle of a chunk, does not stop that chunk from being added. Draw protected regions so they
reach the chunk corners, or list the world in `disabledWorlds`.

## Notes

- **Existing claims are not re-checked.** Drawing a region over land somebody already claimed does
  not remove their claim.
- **Turn it off** with `worldGuardSupport: false` where regions and claims are meant to overlap, such
  as a plot world inside one large region. `disabledWorlds` keeps working.
- **The [API](../developer/api.md)** skips both checks for a write sent without the `REGION` side
  effect.

Related: [Overview](overview.md), [config.yml](../config/config-yml.md)
