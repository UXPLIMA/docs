---
title: Productive Land
order: 1335
description: Faster or slower crops and spawners, and a mob cap per chunk, by a profile each land takes.
icon: wheat
---

Productive land changes how a land works as a farm. Each spot inside a land takes a profile that sets crop
growth, spawner speed and the most mobs one chunk may hold. A land's owner can pick the profile for the whole
land with `/claim productivity set` or from the land menu.

Module `productive-land` · disabled by default · `modules/productive-land/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/claim productivity` | Shows the profile, crop growth, spawner rate and mob cap where you stand. | `uxmclaims.command.productivity` |
| `/claim productivity set <profile>` | Picks the profile of the land you stand in. | `uxmclaims.command.productivity` + role `productive-land:choose` |
| `/claim productivity clear` | Removes the pick, so the plot type, town level or default decides again. | `uxmclaims.command.productivity` + role `productive-land:choose` |
| `/claim productivity choose` | Opens the profile menu of the land you stand in. | `uxmclaims.command.productivity` |

The land menu has a profile button that opens the same menu.

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.command.productivity` | everyone | The `/claim productivity` commands. |
| `uxmclaims.admin.productivity` | op | Picking the profile of any land, without the role permission. `uxmclaims.admin` does the same. |
| `uxmclaims.productive-land.profile.<profile>` | not registered | Picking a profile that is not in `choosable-profiles`. |

| Role permission | Default holder | Allows |
|---|---|---|
| `productive-land:choose` | land owner | Picking and clearing the land's profile. |

## Settings

| Key | Default | What it does |
|---|---|---|
| `profiles.<name>.crop-growth` | `farm`: `2.0`, `ranch`: `1.0` | Crop growth: `1` is vanilla, `2` twice as fast, `0.5` half as fast. |
| `profiles.<name>.spawner-rate` | `farm`: `1.0`, `ranch`: `1.5` | The same for spawners in the land. |
| `profiles.<name>.entity-cap` | `farm`: `40`, `ranch`: `60` | The most mobs one chunk of the land may hold; `0` for no cap. |
| `choosable-profiles` | `[]` | Profiles a land's owner may pick for the whole land. |
| `default-profile` | `""` | The profile every land gets when nothing else names one; empty for none. |
| `plot-types` | `farm: farm` | Plot type to profile, with the [Plots](plots.md) module. |
| `town-levels` | `{}` | Town level id to profile, for the whole town, with the [Levels](levels.md) module. |
| `level-rewards` | `true` | Reads the town level rewards `crop-growth` and `spawner-rate` (multiply the profile's values) and `entity-cap` (added to the cap) from the levels module. |
| `register-farm-plot-type` | `true` | Adds a `farm` plot type to the plots module when it has none. |
| `max-multiplier` | `5` | No multiplier goes above this, whatever profiles and rewards add up to. |
| `capped-spawn-reasons` | `NATURAL`, `SPAWNER`, `BREEDING`, ... | Spawn reasons that count against the mob cap. Mobs from other reasons (commands, plugins) always spawn. |

## Which profile a spot takes

The first match wins:

1. the profile picked for the land,
2. the profile of the plot type at the spot (`plot-types`),
3. the profile of the owning town's level (`town-levels`),
4. `default-profile`.

A picked profile that you later remove from `profiles` is skipped.

## Notes

- With `choosable-profiles` empty, as shipped, only players with a `uxmclaims.productive-land.profile.<profile>` node can pick anything, admins included.
- The mob cap counts every living entity in the chunk except players; there are no caps per mob type.
- Extra growth only applies to crops with growth stages. Saplings, sugar cane and cactus are not sped up.
- A slower spawner skips spawns rather than waiting longer between them.
- A deleted land keeps its pick until it is purged, so a restored land gets it back.
- The profile button on the land menu shows to every viewer; the menu refuses players who may not pick.

Related: [Lands](lands.md), [Plots](plots.md), [Levels](levels.md), [Towns](towns.md)
