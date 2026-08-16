---
title: PlaceholderAPI
order: 840
description: uxmSkyblock registers a PlaceholderAPI expansion so other plugins (scoreboards,
  tab lists, chat) can show island data.
---

---

## Installing

Install **PlaceholderAPI**. The `skyblock` expansion registers automatically: no
download needed.

<Callout type="tip" title="Menus work even without PlaceholderAPI">

The plugin resolves its own `%skyblock_...%` placeholders **internally**, so the
in-game menus always show correct values even on servers without PlaceholderAPI.
PAPI is only needed for **other** plugins to read them.

</Callout>

---

## Available Placeholders

| Placeholder | Returns |
|-------------|---------|
| `%skyblock_has_island%` | Whether the player owns or belongs to an island (Yes / No) |
| `%skyblock_total_islands%` | Number of islands on the network (alias `island_count`) |
| `%skyblock_level%` | The player's island level |
| `%skyblock_next_level%` | The next level number |
| `%skyblock_points%` | Total island points |
| `%skyblock_next_points%` | Points required for the next level (`MAX` at the cap) |
| `%skyblock_points_needed%` | Points still missing to reach the next level |
| `%skyblock_progress%` | Progress to the next level as a 0–100 percentage |
| `%skyblock_progress_bar%` | A 10-segment colored progress bar |
| `%skyblock_bank%` | Island bank balance |
| `%skyblock_biome%` | The island's chosen biome (empty if never set) |
| `%skyblock_rating%` | Average island rating (e.g. `4.3`) |
| `%skyblock_rating_count%` | Number of ratings the island received |
| `%skyblock_coop%` | Number of players with coop access |
| `%skyblock_members%` | Number of members (owner included) |
| `%skyblock_team_limit%` | Maximum team size |
| `%skyblock_team_free%` | Remaining free member slots |
| `%skyblock_owner%` | Owner name |
| `%skyblock_name%` | Island name (falls back to the owner name) |
| `%skyblock_rank%` | The player's role display name on the island |
| `%skyblock_top%` | The island's position on the level leaderboard |
| `%skyblock_is_owner%` | Whether the player owns the island (Yes / No) |
| `%skyblock_visitors%` | Visitor state (OPEN / CLOSED) |
| `%skyblock_locked%` | Island lock state (Yes / No) |
| `%skyblock_has_warp%` | Whether a warp is set (Yes / No) |
| `%skyblock_warps%` | Number of named warps |
| `%skyblock_banned%` | Number of banned players |
| `%skyblock_border%` | Per-island border color (BLUE / GREEN / RED) |
| `%skyblock_time%` | Island time mode (NORMAL / DAY / NIGHT) |
| `%skyblock_server%` | Backend server hosting the island |
| `%skyblock_grid_index%` | Internal grid index of the island |
| `%skyblock_id%` | Island UUID |
| `%skyblock_flag_<name>%` | A flag's state, ON / OFF (e.g. `flag_pvp`, `flag_mob_spawning`) |
| `%skyblock_upgrade_<key>%` | The island's level for an upgrade (e.g. `upgrade_size`) |

The ON/OFF, OPEN/CLOSED and Yes/No words come from `messages.yml`, so they match
your language and styling.

---

## Leaderboard Placeholders

`%skyblock_top_<position>_<field>%` returns information about the island ranked at
`<position>` on the level leaderboard (`1` = top island). These are network-wide, so
the viewing player does **not** need an island. Empty positions return `-` (or `0`
for numbers).

| Placeholder | Returns |
|-------------|---------|
| `%skyblock_top_1%` / `%skyblock_top_1_name%` | Name of the #1 island |
| `%skyblock_top_1_owner%` | Owner name of the #1 island |
| `%skyblock_top_1_level%` | Level of the #1 island |
| `%skyblock_top_1_points%` | Points of the #1 island (alias `_point`) |
| `%skyblock_top_1_bank%` | Bank balance of the #1 island |
| `%skyblock_top_1_members%` | Member count of the #1 island |

Replace `1` with any position, for example `%skyblock_top_2_level%`,
`%skyblock_top_3_name%` or `%skyblock_top_10_points%`.

---

## Example: Scoreboard Line

```
&aIsland Level: &f%skyblock_level%
&aPoints: &f%skyblock_points%
&aBank: &f%skyblock_bank%
&aProgress: &f%skyblock_progress%%  %skyblock_progress_bar%
```

## Example: Top 3 Leaderboard

```
&6&lTOP ISLANDS
&e1. &f%skyblock_top_1_name% &7- &aLvl %skyblock_top_1_level%
&e2. &f%skyblock_top_2_name% &7- &aLvl %skyblock_top_2_level%
&e3. &f%skyblock_top_3_name% &7- &aLvl %skyblock_top_3_level%
```

---

## Using Other Plugins' Placeholders in uxmSkyblock Menus

The reverse also works: menus can include **any** PlaceholderAPI placeholder. The
default main menu, for example, shows the island owner's rank with `%vault_group%`.
See [Menu Customization](../configuration/menus.md).
