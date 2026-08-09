---
title: Island Flags
order: 680
description: Flags are island-wide toggles that control what happens inside the protected
  area — separate from roles, which control what members may do. Flip them in /is
  settings (requires the TOGGLESETTINGS permission).
---

---

## The Flags

| Flag | Default | When ON |
|------|---------|---------|
| `PVP` | OFF | Players can damage each other on the island |
| `MOB_SPAWNING` | — | Hostile mobs spawn naturally |
| `CREEPER_EXPLOSION` | OFF | Creeper explosions damage terrain |
| `TNT_EXPLOSION` | OFF | TNT explosions damage terrain |
| `FIRE_SPREAD` | OFF | Fire spreads from block to block |
| `VISITOR_INTERACT` | OFF | Visitors can use doors, buttons, etc. |

Plus the **Visitors** toggle (the island lock), which decides whether non-members
can visit at all — also controlled by `/is lock`.

---

## Toggling Flags

In game:

```
/is settings
```

Then click a flag button to flip it ON/OFF. Each shows its state via a placeholder
like `%skyblock_flag_pvp%`.

---

## Placeholders

Every flag is exposed through [PlaceholderAPI](../integrations/placeholderapi.md):

```
%skyblock_flag_pvp%
%skyblock_flag_mob_spawning%
%skyblock_flag_creeper_explosion%
%skyblock_flag_tnt_explosion%
%skyblock_flag_fire_spread%
%skyblock_flag_visitor_interact%
%skyblock_visitors%
```

These resolve even on servers **without** PlaceholderAPI installed, so the menus
always show the right state.

---

## Flags vs. Roles

| | Flags | Roles |
|---|-------|-------|
| Controls | Island-wide behavior (PvP, explosions, fire...) | What each member can do |
| Scope | The whole island | Per-member |
| Set with | `/is settings` | `/is role` + [roles.yml](../configuration/roles-yml.md) |

Use **flags** for "no TNT on my island"; use **roles** for "this person can't open
my chests."
