---
title: Overview
order: 701
description: Every plugin uxmHubPvP detects, and what it turns on.
icon: book-open
---

Every integration is optional and none is switched on by hand. uxmHubPvP looks for each plugin at start; with the
plugin absent, no class of it is ever loaded.

| Plugin | Turns on | Page |
|---|---|---|
| PlaceholderAPI | The `%uxmhubpvp_...%` placeholders, other plugins' placeholders in a moment's condition, and the `hands-off-when` guards | [PlaceholderAPI](placeholderapi.md), [Other games](other-games.md) |
| Vault | Kills paying `vault` | [Economies](economies.md) |
| VaultUnlocked | Kills paying `vault-unlocked` | [Economies](economies.md) |
| PlayerPoints | Kills paying `playerpoints` | [Economies](economies.md) |
| EcoBits | Kills paying `ecobits:<currency>` | [Economies](economies.md) |
| Treasury | Kills paying `treasury` | [Economies](economies.md) |
| mcMMO | `%mcmmo_level_<skill>%` in a moment's condition, read from mcMMO without PlaceholderAPI | [Moments](../fighting/moments.md#when-and-how-often) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md#where-a-choice-is-kept) |
| A hologram plugin | The boards on a hologram, through PlaceholderAPI | [Placeholders](../placeholders/reference.md#a-hologram) |

## With other UXM plugins

| Plugin | Together |
|---|---|
| uxmGlow | The colour of a fighter's outline is the colour the player chose in uxmGlow |
| uxmKotl | Its rounds are left alone with `hands-off-when = ["%uxmkotl_arena%"]` ([Other games](other-games.md)) |
| uxmJoinItems | Hands out its own items; give the toggle item a slot no join item uses |
| uxmEssentials | Its holograms draw the boards with heads |
| uxmTheme | Not a plugin: the shared `plugins/uxmTheme/theme.conf` every UXM plugin reads |

Related: [Other games](other-games.md), [Economies](economies.md), [PlaceholderAPI](placeholderapi.md)
