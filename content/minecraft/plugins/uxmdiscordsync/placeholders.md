---
title: Placeholders
order: 70
description: The PlaceholderAPI placeholders for other plugins, and which placeholders each of the plugin's own texts accepts.
icon: braces
---

## For other plugins

With PlaceholderAPI installed, the plugin adds placeholders that any plugin reading PlaceholderAPI can
show: scoreboards, tab lists, holograms, chat formats.

| Placeholder | Value |
|---|---|
| `%discordsync_linked%` | `true` or `false` |
| `%discordsync_discord_name%` | The linked Discord username |
| `%discordsync_discord_id%` | The linked Discord user id |
| `%discordsync_discord_mention%` | `<@id>`, for texts that go to Discord |
| `%discordsync_text_level%`, `%discordsync_text_xp%` | Text level and XP |
| `%discordsync_voice_level%`, `%discordsync_voice_xp%` | Voice level and XP |
| `%discordsync_server%` | `plugin.server-name` |
| `%discordsync_online_players%`, `%discordsync_max_players%` | Players online on this server, and the maximum |
| `%discordsync_discord_boosts%` | Boosts of your Discord server |

The player placeholders are empty for a player who is not linked. They never make a scoreboard wait
for the database: until a player's link or levels have been read once, shortly after they join,
they are empty, and the next refresh shows them.

## In the plugin's own texts

Each text accepts its own set. The page of each feature has the details.

| Text | Placeholders |
|---|---|
| `linking.post-link-commands`, `post-unlink-commands` | `%player%`, `%player_uuid%`, `%player_displayname%`, `%discord_id%`, `%discord_user%`, `%server_name%`, `%online_players%`, `%max_players%` |
| The link `success-embed` | `%player%`, `%player_uuid%`, `%discord%`, `%discord_id%`, `%discord_mention%`, `%server_name%` |
| The link `embed` | `%server_name%` |
| `rank-sync.yml` `post-link-commands` | As `linking.post-link-commands` |
| `nickname-sync.format` | `%player_name%`, `%player_displayname%`, `%discord_name%`, `%discord_global_name%`, `%server_name%`, `%primary_rank%`, `%all_ranks%` |
| Boost `rewards`, `boost-end-commands` | `%player%`, `%player_uuid%`, `%discord_id%`, `%discord_user%`, `%server_name%` |
| Level rewards | `%player%` only |
| Level-up message | `{type}`, `{level}` |
| `/level` embed | `%user%`, `%user_mention%`, `%user_id%`, `%user_avatar%`, `%text_level%`, `%text_xp%`, `%text_xp_needed%`, `%voice_level%`, `%voice_xp%`, `%voice_xp_needed%` |
| `/leaderboard` embed | `%type%` |
| 2FA request | `%player_name%`, `%player_uuid%`, `%server_name%`, `%ip_address%`, `%location%`, `%session_code%`, `%timestamp%`, `%timeout%`, `%status%` |
| Bot status | `%online_players%`, `%max_players%`, `%linked_accounts%`, `%server_name%`, `%total_boosts%` |
| Stats message | `%server_name%`, `%online_players%`, `%max_players%` |
| Chat bridge | `{player}`, `{message}` to Discord; `{user}`, `{message}` to the game |
| Chat log | `%player%`, `%player_displayname%`, `%message%`, `%server_name%`, `%world%`, `%timestamp%` |
| Join and leave log | Minecraft: `%player%`, `%player_displayname%`, `%player_uuid%`, `%server_name%`, `%online_players%`, `%max_players%`. Discord: `%user%`, `%user_id%`, `%user_avatar_url%`, `%member_count%` |
| Audit log | `%player_name%`, `%player_uuid%`, `%discord_user%`, `%discord_id%`, `%server_name%`, `%timestamp%`; for rank sync `%minecraft_ranks%` and `%discord_roles%` instead of the Discord ones |
| `/servercommand` confirmation | `%command%`, `%server%`, `%executor_mention%`, `%executor_id%` |
| `/broadcast` confirmation | `%message%`, `%sender%`, `%sender_mention%`, `%sender_id%` |
| Custom commands | See [Custom Discord commands](config/custom-commands.md#placeholders) |

Where a player is online, PlaceholderAPI placeholders work as well in the commands, the link embeds
and the custom commands. Level rewards are the exception: they only fill in `%player%`.

Related: [Concepts](getting-started/concepts.md#two-placeholder-styles), [Languages](config/languages.md)
