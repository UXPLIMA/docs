---
title: Auto-Kill
order: 34
description: Modern auto-kill, what it costs, and making it work with a stacker.
icon: swords
---

Auto-kill is what turns a spawner into a farm that does not need a player standing on it.

```yaml
auto-kill:
  enabled: true
  enable-for-split-entities: false
  force: false
  loottable-only-autokill: true
  permission: uxmspawners.autokill
```

| Option | What it does |
|---|---|
| `enabled` | The module switch |
| `enable-for-split-entities` | Whether slime and magma cube children are killed too |
| `force` | Auto-kill is always on everywhere, no toggle and no permission check |
| `loottable-only-autokill` | Custom loot tables apply only to auto-killed mobs |
| `permission` | What a player needs to toggle it on their own spawner |

Only the spawner's owner can toggle it, and only from the spawner's main menu.

## Modern auto-kill

`config.yml → use-modern-autokill.enabled` is the version that matters for performance. On 1.16
and newer it skips the entity entirely: no mob is spawned, no mob is killed, the loot is generated
and added to storage directly.

```yaml
use-modern-autokill:
  enabled: true
```

Leave it on unless you are running something older, or you specifically want mobs to exist in the
world for a moment.

## Nerfing what does spawn

When mobs are spawned for real, two options keep them cheap:

```yaml
nerf-spawner-mobs:
  enabled: false
  nerf-every-mob: false
  mobs:
    - ZOMBIE

no-equipment:
  enabled: false
```

`nerf-spawner-mobs` strips AI — no movement, no pathfinding, no attacking. The `mobs` list is read
two ways: with `nerf-every-mob: false` it is the list of mobs to nerf; with it `true` the list
becomes the exceptions that keep their AI. `no-equipment` stops spawned mobs carrying armour or
weapons, which also stops them dropping it.
