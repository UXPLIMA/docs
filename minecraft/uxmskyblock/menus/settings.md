---
title: Settings
order: 600
description: /is settings opens the Settings menu — where the owner (or anyone with
  the TOGGLESETTINGS permission) controls how the island behaves. The layout lives
  in menus/settings.yml.
---

---

## Flags You Can Toggle

Each flag is a button that flips **ON/OFF**. The current state is shown via a
placeholder like `%skyblock_flag_pvp%`.

| Button | Flag | When ON |
|--------|------|---------|
| **PvP** | `PVP` | Players can damage each other on the island |
| **Mob Spawning** | `MOB_SPAWNING` | Hostile mobs spawn naturally |
| **Creeper Explosions** | `CREEPER_EXPLOSION` | Creepers damage terrain |
| **TNT Explosions** | `TNT_EXPLOSION` | TNT damages terrain |
| **Fire Spread** | `FIRE_SPREAD` | Fire spreads block to block |
| **Visitor Interaction** | `VISITOR_INTERACT` | Visitors can use doors/buttons etc. |
| **Visitors** | *(lock)* | Whether the island is open to visitors at all |

See [Island Flags](../protection/flags.md) for the full description of each.

---

## Other Controls in the Menu

Depending on your menu config, the settings menu can also expose:

- **Island lock** — open or close to visitors (same as `/is lock`)
- **Island time** — lock day/night, requires the `skyblock.time` permission
- **Border color** — BLUE / GREEN / RED

---

## Permissions

Flipping flags requires the `TOGGLE_SETTINGS` permission (moderator rank and above
by default, plus the owner). Locking the island **time** additionally requires the
server permission `skyblock.time` (op/VIP by default).

---

## Customizing

Every button — its slot, material, name, lore, and the `flag:<NAME>` action — is
editable in `menus/settings.yml`. See [Menu Customization](../configuration/menus.md).
