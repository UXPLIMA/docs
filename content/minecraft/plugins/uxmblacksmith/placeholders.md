---
title: Placeholders
order: 280
description: The PlaceholderAPI expansion, and the tokens available inside menus.
icon: braces
---

Two separate sets. `%uxmblacksmith_…%` works anywhere PlaceholderAPI is read. `{token}` works only
inside `gui.yml` and trade lore.

## PlaceholderAPI

Identifier `uxmblacksmith`. Registered automatically when the `PLACEHOLDER` hook is on and
PlaceholderAPI is installed. Every placeholder returns an empty string for a null player.

### Slots

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_maxslots%` | Slots the player has — **99** for `uxmblacksmith.admin` |
| `%uxmblacksmith_freeslots%` | Slots not currently in use |
| `%uxmblacksmith_usedslots%` | Trades running |
| `%uxmblacksmith_alltrades%` | Pending trades server-wide |

`max_slots`, `free_slots`, `used_slots`, `active_trades` and `all_trades` are accepted spellings of
the same four.

### One slot's timer

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_timeleft_<n>%` | Time left in slot `n`, formatted |
| `%uxmblacksmith_time_left_<n>%` | The same |

`n` is the slot number, 1 to 10. An empty slot returns the `tradeEmptyPlaceholder` message, not a
blank — which is what you want on a scoreboard.

The value reflects any active boost, so a 2x boost visibly halves the countdown.

### Level and XP

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_level%` | Blacksmith level |
| `%uxmblacksmith_xp%` | XP toward the next level |
| `%uxmblacksmith_xp_required%` | XP needed for the next level |
| `%uxmblacksmith_xp_percent%` | Progress to the next level, 0–100 |
| `%uxmblacksmith_xp_total%` | Lifetime XP |
| `%uxmblacksmith_perk_points%` | Unspent perk points |

### Craft counts

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_total_crafts%` | Trades completed, successful or burned |
| `%uxmblacksmith_successful_crafts%` | Successes |
| `%uxmblacksmith_burned_crafts%` | Burns |
| `%uxmblacksmith_success_rate%` | Successes as a percentage, `0` with no crafts |

### Masteries

Suffix with the mastery key — the category key unless a trade overrides it.

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_mastery_level_<key>%` | Level, starting at 1 |
| `%uxmblacksmith_mastery_xp_<key>%` | XP toward the next level |
| `%uxmblacksmith_mastery_required_xp_<key>%` | XP needed for the next |
| `%uxmblacksmith_mastery_total_xp_<key>%` | Lifetime mastery XP |
| `%uxmblacksmith_mastery_percent_<key>%` | Progress, 0–100 |
| `%uxmblacksmith_mastery_name_<key>%` | The category's display name |

`mastery_xp_required_<key>` is accepted as well as `mastery_required_xp_<key>`.

`mastery_name_swords` returns "Blades" on the shipped config — use it so a scoreboard shows the name
players see rather than the internal key.

### Perks

Suffix with the perk key.

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_perk_level_<key>%` | Current level |
| `%uxmblacksmith_perk_max_level_<key>%` | `maxLevel`, or `0` if unknown |
| `%uxmblacksmith_perk_cost_<key>%` | Cost of the next level |
| `%uxmblacksmith_perk_value_<key>%` | Current effect, formatted by type |
| `%uxmblacksmith_perk_next_value_<key>%` | Effect at the next level |
| `%uxmblacksmith_perk_name_<key>%` | Display name |

`perk_value_` formats by effect type: a percentage for `TRADE_SPEED` and the XP boosts, points for
`BURN_RESISTANCE`, a whole number for `EXTRA_SLOT`.

### Server analytics

| Placeholder | Returns |
|---|---|
| `%uxmblacksmith_analytics_today_started%` | Trades started today, UTC |
| `%uxmblacksmith_analytics_today_completed%` | Completed |
| `%uxmblacksmith_analytics_today_burned%` | Burned |
| `%uxmblacksmith_analytics_today_cancelled%` | Cancelled — `canceled` also works |
| `%uxmblacksmith_analytics_today_failed%` | Failed |

Server-wide, not per player, and read from a cached snapshot — safe on a scoreboard that refreshes
every tick.

## GUI placeholders

These resolve inside `gui.yml` layouts and trade lore only.

### The trade

| Token | Returns |
|---|---|
| `{success_chance}` | The **resolved** chance for this player, including multipliers and mastery |
| `{required_time}` | The trade's configured duration |
| `{required_blacksmith_level}` | `reqLevel` — `{requiredblacksmithlevel}` also works |
| `{required_mastery_level}` | `requiredMasteryLevel` |
| `{remaining_trade_slots}` | How many more of this trade the player may run |
| `{reward_as_command}` | Yes or No, from `rewardAsCommandTrue` / `rewardAsCommandFalse` |
| `{amount}`, `{req_name}` | Inside `requirementLineFormat` only |

### A running trade

| Token | Returns |
|---|---|
| `{progress_bar}` | The bar drawn from `progressBar` in `gui.yml` |
| `{time_remaining}` | Time left, boost-adjusted |
| `{time_elapsed}` | Time spent so far |

### The player

| Token | Returns |
|---|---|
| `{player_level}` | Blacksmith level |
| `{player_xp}`, `{player_xp_total}`, `{player_xp_required}`, `{player_xp_percent}` | XP state |
| `{player_perk_points}` | Unspent points |
| `{player_total_crafts}`, `{player_successful_crafts}`, `{player_burned_crafts}` | Craft counts |
| `{player_max_slots}`, `{player_used_slots}`, `{player_free_slots}` | Slot state |

### Mastery, in the trade's own category

| Token | Returns |
|---|---|
| `{mastery_name}` | The mastery this trade feeds |
| `{player_mastery_level}` | The player's level in it |
| `{mastery_speed_bonus}` | Their current speed bonus there |
| `{mastery_success_bonus}` | Their current success bonus there |

### Top masteries

| Token | Returns |
|---|---|
| `{top_mastery_1_name}` … `{top_mastery_3_name}` | The player's three highest masteries |
| `{top_mastery_1_level}` … `{top_mastery_3_level}` | Their levels |

Useful on the main menu — a one-line summary of what the player has specialised in.

<Callout type="tip" title="Show {success_chance}, not the config number">

`{success_chance}` is resolved per player. A donor with `uxmblacksmith.burn.2` and mastery 20 sees
their real odds, not the recipe's base. Writing the base into the lore by hand makes every rank you
sell look worthless.

</Callout>
