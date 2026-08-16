---
title: Vote
order: 9320
description: Turn votes on listing sites into rewards, streaks, milestones and a server-wide vote party.
icon: thumbs-up
---

The vote module turns a vote on a server listing into something in game. A listing site confirms the vote
through NuVotifier, and uxmEssentials pays the reward, advances the vote-party counter and updates totals,
streaks and the leaderboards. A player who voted while offline does not lose out: the reward waits to be
claimed.

Module `vote` · disabled by default · `modules/vote/config.conf`, `vote.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/vote` | Show the server's vote links. | `uxmessentials.vote.use` |
| `/voteparty` | Show progress towards the next vote party. | `uxmessentials.voteparty.use` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.vote` | op | Hot-reload / inspect the vote module (Votifier vote rewards and vote party). |
| `uxmessentials.vote.admin` | op | /vote admin givevote \<player> [amount] and /vote admin reset \<player>: inject or clear votes for any player. |
| `uxmessentials.vote.testreward` | op | /vote testreward to simulate a vote for yourself and verify the configured rewards. |
| `uxmessentials.vote.top` | everyone | /vote top [period] to see the vote leaderboard for the given period. |
| `uxmessentials.vote.use` | everyone | /vote to see the server's vote links. |
| `uxmessentials.voteparty.admin` | op | /voteparty force\|set\|add: force the party or adjust the party counter. |
| `uxmessentials.voteparty.use` | everyone | /voteparty to see progress towards the next vote party. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `vote-links` | `[]` | The vote-site links shown by /vote, one per line. Plain text or MiniMessage; rendered as written. Kept for back-compat: if `sites` below is empty, vote-links also seeds the per-site cooldown catalog (one VoteSiteSpec per link, using the default cooldown). Remove this list once you configure `sites`. |
| `sites` | `[]` | Structured site definitions for per-site cooldown tracking (/vote next, /vote last, reminders). Each entry requires a `name` (the display label shown in /vote next, /vote last, and the GUI) and optionally: service - the Votifier vote-list/service string this site's cooldown is matched on (case-insensitive). MUST equal the service the vote actually arrives with, or the cooldown never registers. If omitted, it defaults to `name`: so a site whose display label already equals the Votifier service needs no `service`. url - the public vote URL shown in /vote and the GUI. cooldown-minutes - per-site override (falls back to default-cooldown-minutes). Migration from vote-links: copy each URL here with a display name, the matching Votifier service, and a cooldown, then clear vote-links. Both keys can coexist during migration; sites takes precedence for cooldown tracking. (A legacy vote-links entry derives its service from the URL domain, which almost never matches the real Votifier service, so its cooldown stays dead until migrated here.) Example: sites = [ \{ name = "PlanetMinecraft", service = "PMC", url = "https://www.planetminecraft.com/server/...", cooldown-minutes = 1440 } \{ name = "minecraft-mp", url = "https://minecraft-mp.com/server/...", cooldown-minutes = 1440 } ] |
| `default-cooldown-minutes` | `1440` | The cooldown (in minutes) applied to any site that does not declare its own cooldown-minutes. Default 1440 = 24 hours. Must be at least 0; 0 means the site is always votable. |
| `name-validation.max-length` | `16` | The longest a voter name may be, inclusive (at least 1). 16 = the vanilla Minecraft name limit. On a Bedrock/Floodgate server, prefixed Bedrock names can exceed 16 characters or contain a ".": raise max-length and/or set a pattern that allows those names, or any name over the limit is rejected. |
| `name-validation.pattern` | `""` | An optional whitelist regex the WHOLE name must match (java.util.regex). Blank = no pattern, so only the length check plus the always-on blank/"null" rejection apply. A malformed regex is logged once at startup and treated as blank (the whitelist is then off until you fix it). Example: "[A-Za-z0-9_]+". |
| `offline-vote-limit` | `0` | Cap how many queued reward commands a single player may stack up offline while they are away (each offline vote's reward contributes its commands to this count, stored one row per command). 0 (the default) = unlimited. When a voter at or over the cap votes again while offline, the vote still counts toward totals and the vote party; only that vote's durable offline reward is dropped (logged at debug). Protects against a never-logging-in account hoarding an unbounded queue. |
| `reward.disabled-worlds` | `[]` |  |
| `claim.auto` | `true` | true (the default) drains a returning player's offline reward queue automatically on join. false leaves the queue untouched on join so the player pays it out on demand with /vote claim. |
| `gui.list-display` | `"gui"` | The display mode for /vote (no-args) and /vote sites: gui \| chat. "chat" restores the pre-GUI chat-link list for all sources. |
| `gui.votable-material` | `"PAPER"` | Material shown for a site the player can vote on right now. |
| `gui.cooldown-material` | `"CLOCK"` | Material shown for a site still on cooldown. |
| `reminders.enabled` | `false` | Master switch. When false, no reminder tasks or login nags are registered. |
| `reminders.login` | `true` | Send a one-shot reminder shortly after a player logs in (requires enabled = true). |
| `reminders.login-delay-seconds` | `5` | Delay before the login reminder fires, in seconds. Allows the join sequence to settle first. |
| `reminders.interval-minutes` | `0` | Send a reminder on a recurring interval while the player is online. Set to 0 to disable interval reminders (login nag only). Value is in minutes. |
| `discord.webhook-url` | `""` | The Discord channel's incoming-webhook URL (Channel Settings -> Integrations -> Webhooks). Empty = off. |
| `discord.vote.enabled` | `true` |  |
| `discord.vote.template` | `"{player} just voted on {service}!"` |  |
| `discord.party.enabled` | `true` |  |
| `discord.party.template` | `"The vote party fired at {threshold} votes!"` |  |
| `discord.top-voter.enabled` | `false` |  |
| `discord.top-voter.period` | `"MONTHLY"` | The window to rank over: DAILY \| WEEKLY \| MONTHLY \| ALLTIME. An unknown value falls back to MONTHLY. |
| `discord.top-voter.limit` | `10` | How many ranked players to list (at least 1). |
| `discord.top-voter.interval-minutes` | `1440` | How often to post, in minutes (at least 1). 1440 = once a day. |
| `discord.top-voter.title` | `"Top Voters"` | The embed title. |
| `rewards.per-vote` | `[...]` | Paid on EVERY received vote. |
| `rewards.first-vote` | `[...]` | Paid only on a player's very first ever vote on this server. |
| `rewards.milestones` | `[...]` | Paid off the voter's all-time vote count. Each milestone has exactly ONE of: at = pay once, the single time the count equals this value, or every = pay each time the count is a positive multiple of this value. |
| `rewards.streaks` | `[...]` | Paid off the voter's current consecutive-day voting streak; only when the vote actually advanced the streak (a second vote the same day never re-pays). Each streak reward has exactly ONE of: at = pay once, the single time the streak reaches this length, or every = pay each time the streak is a positive multiple of this value. The streak's tolerance for missed days is controlled by streak.grace-days below. |
| `streak.grace-days` | `0` | How many missed days are tolerated before a streak breaks. 0 = strict consecutive days; a vote must land the very next day to extend the streak. N = allow up to N missed days between votes without breaking the run. Must be at least 0. |
| `broadcast.type` | `"EVERY_VOTE"` | Who/when a credited vote announces itself: NONE - never broadcast (the vote is still recorded and rewarded silently). EVERY_VOTE - broadcast every credited vote, online or offline. ONLINE_ONLY - broadcast only when the voter is online. COOLDOWN_PER_PLAYER - broadcast for an online voter at most once per cooldown-seconds. FIRST_VOTE_OF_DAY - broadcast only the voter's first vote of the calendar day. An unknown value falls back to EVERY_VOTE. |
| `broadcast.cooldown-seconds` | `0` | The per-player window (in seconds) used by COOLDOWN_PER_PLAYER. Ignored by the other types. |
| `broadcast.channels` | `["CHAT"]` | The surfaces every broadcast is shown on. Any of: CHAT, ACTION_BAR, TITLE, SUBTITLE, BOSS_BAR. Unknown entries are skipped; an empty list falls back to CHAT. |
| `broadcast.title.fade-in-ms` | `500` |  |
| `broadcast.title.stay-ms` | `2000` |  |
| `broadcast.title.fade-out-ms` | `500` |  |
| `broadcast.boss-bar.color` | `"PURPLE"` |  |
| `broadcast.boss-bar.overlay` | `"PROGRESS"` |  |
| `broadcast.boss-bar.seconds` | `4` |  |
| `broadcast.sound` | `""` | Sound played to each recipient on a broadcast. Bukkit UPPER_SNAKE (ENTITY_PLAYER_LEVELUP) or dot-notation (entity.player.levelup). Blank = no sound. |
| `broadcast.hidden-voters` | `[]` | Voter names (case-insensitive) whose votes are never announced; e.g. staff testing votes. |
| `voteparty.threshold` | `25` | How many votes accumulate before a party fires. Must be at least one. |
| `voteparty.rewards` | `[]` | The reward given to every eligible online player when the party fires. Uses the same spec format as the per-vote rewards above: chance, commands, messages, broadcast, items, worlds. \{player} is substituted per player. Example: reward \{ commands = [ "eco give \{player} 500" ] messages = [ "\<gold>Vote party! You received 500 coins!" ] broadcast = [ "\<gold>A vote party just fired!" ] } Migration note: the old flat `voteparty.rewards = ["eco give {player} 500"]` still works as a fallback but is superseded by this block. Remove the flat list when you configure reward \{}. |
| `voteparty.only-voters` | `false` | When true, only players who voted during the current party window receive rewards. When false (default), all online players are rewarded regardless of whether they voted. |
| `voteparty.escalate-by` | `0` | Votes added to the threshold after each party fires (0 = no escalation). The stored override is cleared (threshold reverts to base) in two ways: - An admin runs /voteparty set \<n>; always clears the override regardless of n. - escalate-by is changed to 0 in config and a party fires; the next fire clears the override. |
| `voteparty.reset` | `"none"` | Auto-reset the counter on a calendar boundary: none \| daily \| weekly. |
| `voteparty.announce-at` | `[]` | Intermediate vote counts at which to broadcast a milestone announcement (VOTEPARTY_ANNOUNCE key). Empty list = no announcements. Example: [10, 20] announces when the counter reaches 10 and 20. |
| `voteparty.sound` | `""` | Sound played to every online player when a party fires. Accepts Bukkit UPPER_SNAKE names (e.g. ENTITY_PLAYER_LEVELUP) or dot-notation keys (e.g. entity.player.levelup). Blank = no sound. |
| `voteparty.particle` | `""` | Particle spawned around every online player when a party fires. Accepts UPPER_SNAKE or lowercase names (e.g. FIREWORK or firework). Blank = no particle. |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_voteparty_current%` | How many votes the party has collected. |
| `%uxmessentials_voteparty_remaining%` | How many votes the party still needs. |
| `%uxmessentials_voteparty_required%` | How many votes the party needs to fire. |
| `%uxmessentials_votes_<period>%` | The player's vote count for one period: daily, weekly, monthly or alltime. |
| `%uxmessentials_votes_position_<period>%` | Where the player sits on one period's vote leaderboard. |
| `%uxmessentials_votes_streak_best%` | The player's longest voting streak. |
| `%uxmessentials_votes_streak_current%` | How many days in a row the player has voted. |
| `%uxmessentials_votes_top_<period>_<n>_name%` | The name of the player ranked nth on one period's vote leaderboard. |
| `%uxmessentials_votes_top_<period>_<n>_votes%` | The vote count of the player ranked nth on one period's leaderboard. |
{/* /generated */}

## Notes

- **Votifier is the delivery mechanism, not part of this module.** Without NuVotifier installed the listener
  stays dormant: `/vote`, `/vote top` and `/voteparty` still work, incoming site rewards do not.
- **Rewards stack in four layers:** every vote, a per-site bonus, the player's first vote ever, and lifetime
  milestones. Rewards pay immediately when the player is online and wait for `/vote claim` otherwise.
- **The name a site sends back is whatever the voter typed,** so the match is made against the accounts the
  server already knows, in any case, rather than requiring an exact spelling.
- **Streaks read consecutive days, milestones read the lifetime total,** which is why missing a day resets one
  and not the other.
- **A vote party is a server-wide goal,** and staff can force it, set the counter or add to it.
- **Run `/vote testreward` after every reward edit.** It runs the whole pipeline against you, so a broken
  reward shows up before a player finds it.

Related: [Economy](economy.md), [Discord Link](discordlink.md), [Communication](communication.md)
