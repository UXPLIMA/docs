---
title: Placeholder Reference
order: 1600
description: Every %uxmessentials_% placeholder this build answers, area by area.
---

## Overview

With **PlaceholderAPI** installed, uxmEssentials registers a `%uxmessentials_<key>%` expansion carrying the 216 keys below. Nothing to download and nothing to switch on: the expansion registers itself at startup.

Every key is declared once inside the plugin, in a single catalogue that a build-time guard resolves against the resolver in both directions. That is what this page is generated from, so a key listed here is a key this build answers.

<Callout type="info" title="The plugin can print this page for you">

`/uxmess placeholders` lists the areas in game, `/uxmess placeholders <area> [page]` reads one, and `/uxmess placeholders export` writes the whole catalogue to `placeholders.md` in the plugin folder. All three need `uxmessentials.admin.placeholders`.

</Callout>

### How to read the Reads column

| Reads | Meaning |
|-------|---------|
| `player` | durable, answers for an offline player too |
| `session` | live, reads the dash while the player is offline |
| `global` | server-wide, the requesting player is ignored |

A key whose module is **disabled** renders `-` rather than failing, and so does a key whose data is absent (a home that is not set, a ban that is not held). A key that does not exist at all is left as the raw token, which is how you tell a typo from an empty value.

Keys written with an open segment, like `%uxmessentials_kit_cost_<kit>%`, are families: replace the segment with the id you mean.

---

## Player & Server

Always answered, whatever is enabled: the kernel owns these. The account, session, held-item and server keys need no feature module at all.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_hand_amount%` | session | How many are in the main-hand stack. |
| `%uxmessentials_hand_damage%` | session | How much durability the main-hand item has spent. |
| `%uxmessentials_hand_durability%` | session | How much durability the main-hand item has left. |
| `%uxmessentials_hand_durability_max%` | session | The main-hand item's durability ceiling. |
| `%uxmessentials_hand_enchants%` | session | The enchantments on the main-hand item, each as name and level, comma separated. |
| `%uxmessentials_hand_enchants_count%` | session | How many enchantments the main-hand item carries. |
| `%uxmessentials_hand_lore%` | session | The lore of the main-hand item, joined into one line. |
| `%uxmessentials_hand_model%` | session | The custom model data on the main-hand item. |
| `%uxmessentials_hand_name%` | session | The display name of the item in the main hand, or its material when it carries none. |
| `%uxmessentials_hand_type%` | session | The material of the item in the main hand. |
| `%uxmessentials_itemcount_<material>%` | session | How many of one material the player carries, counting every stack in their inventory. |
| `%uxmessentials_offhand_amount%` | session | How many are in the off-hand stack. |
| `%uxmessentials_offhand_damage%` | session | How much durability the off-hand item has spent. |
| `%uxmessentials_offhand_durability%` | session | How much durability the off-hand item has left. |
| `%uxmessentials_offhand_durability_max%` | session | The off-hand item's durability ceiling. |
| `%uxmessentials_offhand_enchants%` | session | The enchantments on the off-hand item, each as name and level, comma separated. |
| `%uxmessentials_offhand_enchants_count%` | session | How many enchantments the off-hand item carries. |
| `%uxmessentials_offhand_lore%` | session | The lore of the off-hand item, joined into one line. |
| `%uxmessentials_offhand_model%` | session | The custom model data on the off-hand item. |
| `%uxmessentials_offhand_name%` | session | The display name of the item in the off hand, or its material when it carries none. |
| `%uxmessentials_offhand_type%` | session | The material of the item in the off hand. |
| `%uxmessentials_p_<name>_<key>%` | player | Any other key on this page, answered about the named player instead of the one reading it; the name is resolved the same way a command resolves it, so it works offline and on a cracked server. |
| `%uxmessentials_player_banned%` | player | Whether the server's own ban list holds the account (yes/no); the moderation keys read the plugin's. |
| `%uxmessentials_player_exp_percent%` | session | How far through the current experience level the player is, as a whole percentage. |
| `%uxmessentials_player_exp_progress%` | session | How far through the current experience level the player is, from 0 to 1. |
| `%uxmessentials_player_exp_to_next%` | session | How many experience points remain before the next level. |
| `%uxmessentials_player_exp_total%` | session | The experience points the player holds in total. |
| `%uxmessentials_player_first_join%` | player | When the account first joined, as a date and time. |
| `%uxmessentials_player_first_join_date%` | player | The same first-join stamp, under the spelling a config may prefer. |
| `%uxmessentials_player_last_seen%` | player | When the account was last connected; empty while it is connected now. |
| `%uxmessentials_player_last_seen_date%` | player | The same last-seen stamp, under the spelling a config may prefer. |
| `%uxmessentials_player_level%` | session | The player's experience level. |
| `%uxmessentials_player_op%` | session | Whether the server treats the player as an operator (yes/no). |
| `%uxmessentials_player_ping%` | session | The player's round-trip time, in milliseconds. |
| `%uxmessentials_player_playtime%` | player | How long the account has played, in whole hours. |
| `%uxmessentials_player_playtime_days%` | player | How long the account has played, in whole days. |
| `%uxmessentials_player_playtime_formatted%` | player | How long the account has played, in the compact 1d2h form. |
| `%uxmessentials_player_playtime_hours%` | player | How long the account has played, in whole hours. |
| `%uxmessentials_player_playtime_minutes%` | player | How long the account has played, in whole minutes. |
| `%uxmessentials_player_playtime_seconds%` | player | How long the account has played, in whole seconds. |
| `%uxmessentials_player_sneaking%` | session | Whether the player is crouching (yes/no). |
| `%uxmessentials_player_sprinting%` | session | Whether the player is running (yes/no). |
| `%uxmessentials_player_world%` | session | The world the player stands in. |
| `%uxmessentials_player_world_time%` | session | The time of day in the player's world, in ticks. |
| `%uxmessentials_player_world_time_formatted%` | session | The time of day in the player's world as a 24-hour clock, where tick 0 is 06:00. |
| `%uxmessentials_player_world_weather%` | session | The sky in the player's world: clear, rain or thunder. |
| `%uxmessentials_server_max_players%` | global | The server's player slot count. |
| `%uxmessentials_server_online%` | global | How many players are connected. |
| `%uxmessentials_server_ram_free%` | global | Heap still free, in whole megabytes. |
| `%uxmessentials_server_ram_max%` | global | Heap ceiling, in whole megabytes. |
| `%uxmessentials_server_ram_used%` | global | Heap in use, in whole megabytes. |
| `%uxmessentials_server_tps%` | global | Ticks per second over the last minute. |
| `%uxmessentials_server_tps_15m%` | global | Ticks per second over the last fifteen minutes. |
| `%uxmessentials_server_tps_5m%` | global | Ticks per second over the last five minutes. |
| `%uxmessentials_server_tps_colored%` | global | Ticks per second over the last minute, wrapped in a green, yellow or red MiniMessage colour. |
| `%uxmessentials_server_uptime%` | global | How long the server has been up, in whole minutes. |
| `%uxmessentials_server_uptime_formatted%` | global | How long the server has been up, in the compact 1h30m form. |
| `%uxmessentials_server_version%` | global | The Minecraft version the server runs. |
| `%uxmessentials_server_world_players_<world>%` | global | How many players are in one named world; the dash when no such world is loaded. |

---

## Communication

The chat lock and the rotating announcer.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_communication_broadcasts%` | session | Whether the player receives the rotating announcements (yes/no). |
| `%uxmessentials_communication_chat_enabled%` | global | Whether public chat is open rather than locked by `/togglechat` (yes/no). |

---

## Menus

What the menu engine has open for this player.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_menu_argument_<name>%` | session | The value of one named argument the open menu was called with. |
| `%uxmessentials_menu_is_in_menu%` | session | Whether the player has a plugin menu open (yes/no). |
| `%uxmessentials_menu_last%` | session | The id of the last menu the player opened, which survives the menu closing. |
| `%uxmessentials_menu_opened%` | session | The id of the menu the player has open. |
| `%uxmessentials_menu_page%` | session | The page the open menu is showing, counting from 1. |
| `%uxmessentials_menu_rows%` | session | How many rows the open menu has. |

---

## Discord Linking

Whether the account is bound, and to whom.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_discordlink_id%` | player | The bound Discord user id. |
| `%uxmessentials_discordlink_linked%` | player | Whether the account is bound to a Discord user (yes/no). |

---

## Economy

Balances in every currency, and the leaderboard.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_balance%` | player | The player's balance in the default currency, as a plain number. |
| `%uxmessentials_balance_formatted%` | player | The player's balance with the currency symbol and grouping applied. |
| `%uxmessentials_baltop_position%` | player | The player's place on the default-currency leaderboard. |
| `%uxmessentials_economy_balance%` | player | The player's balance in the default currency, as a plain number. |
| `%uxmessentials_economy_balance_<currency>%` | player | The player's balance in one named currency, as a plain number. |
| `%uxmessentials_economy_balance_compact%` | player | The player's balance shortened to 1.2k / 3.4M. |
| `%uxmessentials_economy_balance_formatted%` | player | The player's balance with the currency symbol and grouping applied. |
| `%uxmessentials_economy_balance_formatted_<currency>%` | player | The player's balance in one named currency, with its symbol applied. |
| `%uxmessentials_economy_balance_short%` | player | The same shortened balance, under the spelling a config may prefer. |
| `%uxmessentials_economy_baltop_<currency>_<n>_amount%` | global | The balance of the player ranked nth in one named currency, as a plain number. |
| `%uxmessentials_economy_baltop_<currency>_<n>_formatted%` | global | The balance of the player ranked nth in one named currency, with its symbol applied. |
| `%uxmessentials_economy_baltop_<currency>_<n>_name%` | global | The name of the player ranked nth on one named currency's leaderboard. |
| `%uxmessentials_economy_baltop_<currency>_<n>_uuid%` | global | The uuid of the player ranked nth on one named currency's leaderboard. |
| `%uxmessentials_economy_baltop_<n>_amount%` | global | The balance of the player ranked nth, as a plain number. |
| `%uxmessentials_economy_baltop_<n>_formatted%` | global | The balance of the player ranked nth, with the currency symbol applied. |
| `%uxmessentials_economy_baltop_<n>_name%` | global | The name of the player ranked nth on the default-currency leaderboard. |
| `%uxmessentials_economy_baltop_<n>_uuid%` | global | The uuid of the player ranked nth on the default-currency leaderboard. |
| `%uxmessentials_economy_baltop_position%` | player | The player's place on the default-currency leaderboard. |
| `%uxmessentials_economy_currency_name%` | global | The plural name of the default currency. |
| `%uxmessentials_economy_currency_symbol%` | global | The symbol of the default currency. |

---

## Holograms

How many holograms are placed.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_holograms_count%` | global | How many holograms are placed. |

---

## Homes

Counts, the quota, and each home by its position in the grid.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_homes_<n>%` | player | The name of the player's nth home, counting from 1. |
| `%uxmessentials_homes_<n>_world%` | player | The world the player's nth home sits in. |
| `%uxmessentials_homes_<n>_x%` | player | The block x of the player's nth home. |
| `%uxmessentials_homes_<n>_y%` | player | The block y of the player's nth home. |
| `%uxmessentials_homes_<n>_z%` | player | The block z of the player's nth home. |
| `%uxmessentials_homes_count%` | player | How many homes the player has set. |
| `%uxmessentials_homes_exists_<home>%` | player | Whether the player has a home by that name (yes/no). |
| `%uxmessentials_homes_left%` | player | How many more homes the player may set. |
| `%uxmessentials_homes_limit%` | player | How many homes the player may keep; the infinity marker when the quota is unlimited. |
| `%uxmessentials_homes_list%` | player | The player's home names, comma separated. |

---

## Kits

Per-kit availability, cooldown, price and claims left.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_kit_available_<kit>%` | player | Whether the player may claim one kit right now (yes/no). |
| `%uxmessentials_kit_claims_left_<kit>%` | player | How many claims of one kit the player has left; the infinity marker when it repeats. |
| `%uxmessentials_kit_cooldown_<kit>%` | player | The wait left before the player may claim one kit again. |
| `%uxmessentials_kit_cooldown_<kit>_formatted%` | player | The same remaining kit wait, under the spelling a config may prefer. |
| `%uxmessentials_kit_cost_<kit>%` | global | What one kit charges to claim, or free when it charges nothing. |
| `%uxmessentials_kit_has_<kit>%` | player | Whether the player holds one kit's permission (yes/no). |
| `%uxmessentials_kits_list%` | player | The ids of the kits the player may claim, comma separated. |

---

## Messaging

Mail, ignores, and the private-message session.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_messaging_ignoring_count%` | player | How many players this player is ignoring. |
| `%uxmessentials_messaging_mail_total%` | player | How much mail the player holds. |
| `%uxmessentials_messaging_mail_unread%` | player | How much mail the player has not read. |
| `%uxmessentials_messaging_msgtoggle%` | session | Whether the player accepts private messages (yes/no). |
| `%uxmessentials_messaging_reply_target%` | session | Who `/r` would answer: the last player this one talked to. |
| `%uxmessentials_messaging_socialspy%` | session | Whether the player is watching other players' messages (yes/no). |

---

## Moderation

Sanctions: whether they are held, why, by whom, and for how long.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_jailed%` | player | Whether the player is jailed (yes/no). |
| `%uxmessentials_moderation_ban_issuer%` | player | Who banned the player. |
| `%uxmessentials_moderation_ban_reason%` | player | Why the player was banned. |
| `%uxmessentials_moderation_ban_remaining%` | player | How long is left on the ban, in the compact 1d2h form. |
| `%uxmessentials_moderation_ban_remaining_formatted%` | player | The same remaining ban, under the spelling a config may prefer. |
| `%uxmessentials_moderation_banned%` | player | Whether the player is banned (yes/no). |
| `%uxmessentials_moderation_frozen%` | session | Whether the player is frozen in place by staff (yes/no). |
| `%uxmessentials_moderation_jailed%` | player | Whether the player is jailed (yes/no). |
| `%uxmessentials_moderation_mute_issuer%` | player | Who muted the player. |
| `%uxmessentials_moderation_mute_reason%` | player | Why the player was muted. |
| `%uxmessentials_moderation_mute_remaining%` | player | How long is left on the mute, in the compact 1d2h form. |
| `%uxmessentials_moderation_mute_remaining_formatted%` | player | The same remaining mute, under the spelling a config may prefer. |
| `%uxmessentials_moderation_muted%` | player | Whether the player is muted (yes/no). |
| `%uxmessentials_moderation_warns%` | player | How many warnings the player carries. |
| `%uxmessentials_muted%` | player | Whether the player is muted (yes/no). |

---

## Player State

The live session: mode, health, position, playtime.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_playerstate_biome%` | session | The biome the player is standing in. |
| `%uxmessentials_playerstate_fly%` | session | Whether the player may fly (yes/no). |
| `%uxmessentials_playerstate_fly_speed%` | session | The player's fly speed. |
| `%uxmessentials_playerstate_flying%` | session | Whether the player is flying right now (yes/no). |
| `%uxmessentials_playerstate_food%` | session | The player's food level. |
| `%uxmessentials_playerstate_gamemode%` | session | The player's game mode. |
| `%uxmessentials_playerstate_god%` | session | Whether the player takes no damage (yes/no). |
| `%uxmessentials_playerstate_health%` | session | The player's current health. |
| `%uxmessentials_playerstate_level%` | session | The player's experience level. |
| `%uxmessentials_playerstate_max_health%` | session | The player's maximum health. |
| `%uxmessentials_playerstate_playtime%` | session | How long the player has played, in whole hours. |
| `%uxmessentials_playerstate_playtime_formatted%` | session | How long the player has played, in the compact 1d2h form. |
| `%uxmessentials_playerstate_speed%` | session | The speed that applies to how the player is moving: fly speed while flying, walk speed otherwise. |
| `%uxmessentials_playerstate_walk_speed%` | session | The player's walk speed. |
| `%uxmessentials_playerstate_world%` | session | The world the player is in. |
| `%uxmessentials_playerstate_x%` | session | The player's block x. |
| `%uxmessentials_playerstate_xp%` | session | How far the player is through the current experience level, from 0 to 1. |
| `%uxmessentials_playerstate_y%` | session | The player's block y. |
| `%uxmessentials_playerstate_z%` | session | The player's block z. |

---

## Player Warps

The warps this player owns.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_playerwarp_<warp>_owner%` | player | Who owns one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_visits%` | player | How many times one of the player's warps has been used. |
| `%uxmessentials_playerwarp_<warp>_world%` | player | The world one of the player's warps sits in. |
| `%uxmessentials_playerwarp_<warp>_x%` | player | The block x of one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_y%` | player | The block y of one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_z%` | player | The block z of one of the player's warps. |
| `%uxmessentials_playerwarps_count%` | player | How many player warps the player owns. |
| `%uxmessentials_playerwarps_left%` | player | How many more player warps the player may create. |
| `%uxmessentials_playerwarps_limit%` | player | How many player warps the player may own; the infinity marker when unlimited. |
| `%uxmessentials_playerwarps_list%` | player | The names of the player warps the player owns, comma separated. |

---

## Sitting & Poses

What pose the player holds.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_poses_pose%` | session | The pose the player holds: sit, lay, bellyflop, spin or none. |
| `%uxmessentials_poses_posing%` | session | Whether the player holds a free pose: lay, bellyflop or spin (yes/no). |
| `%uxmessentials_poses_sitting%` | session | Whether the player is sitting (yes/no). |
| `%uxmessentials_poses_toggle%` | session | Whether the player lets others sit on them: allow or refuse. |

---

## Presence

Away, vanished, and what name they wear.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_afk%` | session | Whether the player is away (yes/no). |
| `%uxmessentials_afk_duration%` | session | How long the player has been away, in the compact 1h30m form. |
| `%uxmessentials_presence_afk%` | session | Whether the player is away (yes/no). |
| `%uxmessentials_presence_afk_duration%` | session | How long the player has been away, in the compact 1h30m form. |
| `%uxmessentials_presence_afk_reason%` | session | The reason the player gave when they went away. |
| `%uxmessentials_presence_afk_since%` | session | The same away duration, under the spelling a config may prefer. |
| `%uxmessentials_presence_nickname%` | session | The player's nickname, or their name when they have not set one. |
| `%uxmessentials_presence_realname%` | session | The player's account name, whatever nickname they wear. |
| `%uxmessentials_presence_vanished%` | session | Whether the player is vanished (yes/no). |
| `%uxmessentials_vanished%` | session | Whether the player is vanished (yes/no). |

---

## Ranks

Where the player stands on the ladder.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_prestige%` | player | How many times the player has prestiged. |
| `%uxmessentials_rank%` | player | The player's current rank. |
| `%uxmessentials_rank_next%` | player | The rank above the player's, or max when they are at the top of the ladder. |

---

## Scoreboard

Whether the sidebar is showing.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_scoreboard_visible%` | session | Whether the player has the sidebar showing (yes/no). |

---

## Staff

Staff mode, and how many staff are on.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_staff_count%` | global | The same connected-staff count, under the spelling a config may prefer. |
| `%uxmessentials_staff_mode%` | session | Whether the player is in staff mode (yes/no). |
| `%uxmessentials_staff_online%` | global | How many staff members are connected. |

---

## Teleport

Cooldown, warmup, requests, and the /back location.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_teleport_accepting%` | session | Whether the player accepts incoming teleport requests (yes/no). |
| `%uxmessentials_teleport_back_available%` | session | Whether the player has a location to return to with `/back` (yes/no). |
| `%uxmessentials_teleport_back_world%` | session | The world the player's `/back` location sits in. |
| `%uxmessentials_teleport_back_x%` | session | The block x of the player's `/back` location. |
| `%uxmessentials_teleport_back_y%` | session | The block y of the player's `/back` location. |
| `%uxmessentials_teleport_back_z%` | session | The block z of the player's `/back` location. |
| `%uxmessentials_teleport_cooldown_remaining%` | session | The wait left before the player may teleport again, in the compact 1m30s form. |
| `%uxmessentials_teleport_cooldown_remaining_formatted%` | session | The same remaining teleport cooldown, under the spelling a config may prefer. |
| `%uxmessentials_teleport_tpa_incoming%` | session | How many teleport requests are waiting for the player's answer. |
| `%uxmessentials_teleport_tpa_pending%` | session | How many teleport requests the player has sent and not had answered. |
| `%uxmessentials_teleport_warmup_remaining%` | session | The stand-still countdown left on the teleport in progress. |
| `%uxmessentials_teleport_warmup_remaining_formatted%` | session | The same remaining warmup, under the spelling a config may prefer. |

---

## Vaults

Vault count, quota and size.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_vaults_count%` | player | How many vaults the player holds. |
| `%uxmessentials_vaults_left%` | player | How many more vaults the player may open. |
| `%uxmessentials_vaults_max%` | player | How many vaults the player may open; the infinity marker when unlimited. |
| `%uxmessentials_vaults_size%` | player | How many rows each of the player's vaults holds. |

---

## Vote

Vote counts, streaks, leaderboards and the party.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_voteparty_current%` | global | How many votes the party has collected. |
| `%uxmessentials_voteparty_remaining%` | global | How many votes the party still needs. |
| `%uxmessentials_voteparty_required%` | global | How many votes the party needs to fire. |
| `%uxmessentials_votes_<period>%` | player | The player's vote count for one period: daily, weekly, monthly or alltime. |
| `%uxmessentials_votes_position_<period>%` | player | Where the player sits on one period's vote leaderboard. |
| `%uxmessentials_votes_streak_best%` | player | The player's longest voting streak. |
| `%uxmessentials_votes_streak_current%` | player | How many days in a row the player has voted. |
| `%uxmessentials_votes_top_<period>_<n>_name%` | global | The name of the player ranked nth on one period's vote leaderboard. |
| `%uxmessentials_votes_top_<period>_<n>_votes%` | global | The vote count of the player ranked nth on one period's leaderboard. |

---

## Warps

The server warps this player may use.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_warp_<warp>_cost%` | global | What one warp charges to use. |
| `%uxmessentials_warp_<warp>_owner%` | global | Who created one warp. |
| `%uxmessentials_warp_<warp>_visits%` | global | How many times one warp has been used. |
| `%uxmessentials_warp_<warp>_world%` | global | The world one warp sits in. |
| `%uxmessentials_warp_<warp>_x%` | global | The block x of one warp. |
| `%uxmessentials_warp_<warp>_y%` | global | The block y of one warp. |
| `%uxmessentials_warp_<warp>_z%` | global | The block z of one warp. |
| `%uxmessentials_warps_count%` | player | How many warps the player may use. |
| `%uxmessentials_warps_list%` | player | The names of the warps the player may use, comma separated. |

---

## Worlds

How many worlds are managed and loaded.

| Placeholder | Reads | What it renders |
|-------------|-------|-----------------|
| `%uxmessentials_worlds_default%` | global | The name of the default world. |
| `%uxmessentials_worlds_default_players%` | global | How many players are in the default world. |
| `%uxmessentials_worlds_loaded_count%` | global | How many worlds are loaded right now. |
| `%uxmessentials_worlds_managed_count%` | global | How many worlds the plugin's registry holds. |

---

## Next Steps

- [PlaceholderAPI](../integrations/placeholderapi.md): both directions, and what happens without it
- [Scoreboard, Tablist & Nametags](../features/hud.md): where these show up most
- [Holograms](../features/holograms.md): live placeholder lines
