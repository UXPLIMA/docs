---
title: Player Warps
order: 9160
description: Player-owned public destinations with entry fees, ratings, rent and sponsored slots.
icon: map-pin
---

A player warp is a destination an ordinary player publishes for others to visit: a shop, a farm, a build worth
showing off. It is owned by whoever created it, counts against that player's quota, and carries its own access
rules, price, category, rating and staff list. `/setpwarp` creates one, `/pwarp` visits and manages, `/pwarps`
lists. Two players can both own a warp called `shop`, so a bare name always means your own.

Module `playerwarps` · enabled by default · `modules/playerwarps/config.conf`

## Commands

{/* generated:commands */}
| Command | What it does | Permission |
|---|---|---|
| `/pwarp` | Teleport to a player warp, edit your own warps, or manage their access and price. | `uxmessentials.pwarp.use` |
| `/pwarps` | List your warps or a player's public warps. | `uxmessentials.pwarp.list` |
| `/setpwarp` | Create or move a player warp at your location. | `uxmessentials.pwarp.set` |
{/* /generated */}

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.playerwarps` | op | Hot-reload / inspect the playerwarps module (player-owned warps behind /pwarp). |
| `uxmessentials.pwarp.access` | everyone | /pwarp access \<name> \<PUBLIC\|PASSWORD\|WHITELIST\|PRIVATE> to set a player warp's access axis. |
| `uxmessentials.pwarp.admin` | op | /pwarp admin restore\|purge\|setowner\|reload to manage any player's warp by its id. |
| `uxmessentials.pwarp.ban` | everyone | /pwarp ban\|unban \<name> \<player> to bar a player from one of your warps or lift that bar. |
| `uxmessentials.pwarp.bypass.ban` | op | Enter a player warp you are banned from (skips the ban check on /pwarp). |
| `uxmessentials.pwarp.bypass.cost` | op | Use a priced player warp without paying its entry cost (skips the charge on /pwarp). |
| `uxmessentials.pwarp.bypass.password` | op | Enter a password-protected player warp without the password (skips the check on /pwarp). |
| `uxmessentials.pwarp.bypass.safety` | op | Use a player warp whose destination is unsafe (skips the safe-landing check on /pwarp). |
| `uxmessentials.pwarp.bypass.whitelist` | op | Enter a whitelist-only player warp without being on the whitelist (skips the check on /pwarp). |
| `uxmessentials.pwarp.category` | everyone | /pwarp category \<name> [categoryId] to file a player warp under a browse category. |
| `uxmessentials.pwarp.delete` | everyone | /pwarp del \<name> to remove one of your player warps. |
| `uxmessentials.pwarp.description` | everyone | /pwarp description \<name> [text] to set or clear a player warp's description. |
| `uxmessentials.pwarp.displayname` | everyone | /pwarp displayname \<name> [text] to set or clear a player warp's display name. |
| `uxmessentials.pwarp.edit` | everyone | /pwarp edit \<name> to open one warp's property editor, the click-driven form of the typed verbs. Held by default alongside the use node. |
| `uxmessentials.pwarp.favourite` | everyone | /pwarp favourite\|unfavourite \<name> to star or un-star a player warp. |
| `uxmessentials.pwarp.gui` | op | Manage every player's warps in the /pwarp GUI (a player without it edits only their own). |
| `uxmessentials.pwarp.icon` | everyone | /pwarp icon \<name> [icon] to set or clear a player warp's browse icon. |
| `uxmessentials.pwarp.info` | everyone | /pwarp info \<name> to show a player warp's owner, access, price, visits, and rating. |
| `uxmessentials.pwarp.limit.<n>` | everyone | How many player warps you may own; the largest tier held wins. |
| `uxmessentials.pwarp.list` | everyone | /pwarps [player] to list your warps or a player's public warps. |
| `uxmessentials.pwarp.members` | everyone | /pwarp members \<name> add\|remove \<player> to grant or revoke a co-owner or manager on one of your warps. |
| `uxmessentials.pwarp.move` | everyone | /pwarp move \<name> to re-anchor one of your player warps at your location. |
| `uxmessentials.pwarp.password` | everyone | /pwarp password \<name> \<password>\|clear to set or clear a player warp's password. |
| `uxmessentials.pwarp.price` | everyone | /pwarp price \<name> \<amount> [currency] to set a player warp's entry price. |
| `uxmessentials.pwarp.public` | everyone | /pwarp visibility public\|private \<name> to toggle a player warp's visibility. |
| `uxmessentials.pwarp.rate` | everyone | /pwarp rate \<name> \<1-5> to award a player warp a star rating. |
| `uxmessentials.pwarp.rename` | everyone | /pwarp rename \<name> \<newName> to rename one of your player warps. |
| `uxmessentials.pwarp.set` | everyone | /setpwarp \<name> to create or move a player-owned warp at your location. |
| `uxmessentials.pwarp.sponsor` | everyone | /pwarp sponsor \<name> [days] to buy a paid, time-limited pinned browse slot for one of your warps. |
| `uxmessentials.pwarp.transfer` | everyone | /pwarp transfer \<name> \<player> to hand ownership of one of your warps to another player. |
| `uxmessentials.pwarp.use` | everyone | /pwarp \<name> [owner] to teleport to your own or a player's public warp. |
| `uxmessentials.pwarp.whitelist` | everyone | /pwarp whitelist \<name> add\|remove \<player> to manage a whitelist-access warp's guest list. |
| `uxmessentials.pwarp.withdraw` | everyone | /pwarp withdraw \<name> to pay one of your warps' accrued earnings out to its owner. |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `default-limit` | `3` | player-warps per player without a uxmessentials.pwarp.limit.\<n> node |
| `payout.cut-percent` | `10` | server's share of each entry fee; the rest banks to the owner |
| `payout.auto-payout` | `false` | settle the owner immediately (only for offline-writable currencies) vs. bank it for /pwarp withdraw |
| `ratings.confidence` | `10` | Bayesian smoothing constant C: how many global-mean votes of prior each warp starts with, so a lone 5-star vote cannot outrank a proven favourite. Higher = stronger pull toward the global mean. |
| `ratings.rewards.enabled` | `false` | master switch for the whole rating-reward sub-group; off = no rating ever grants |
| `ratings.rewards.rater.money` | `0` | amount credited to the rater, in the currency below (0 = no money reward) |
| `ratings.rewards.rater.currency` | `"default"` | currency the rater's money reward is credited in |
| `ratings.rewards.rater.command` | `""` | console command run for the rater ("" = none); %player% = the rater's name |
| `ratings.rewards.owner.money` | `0` | amount credited to the warp owner per unique rater (0 = no money reward) |
| `ratings.rewards.owner.currency` | `"default"` | currency the owner's money reward is credited in |
| `ratings.rewards.owner.command` | `""` | console command run for the owner ("" = none); %player% = the owner's name |
| `rent.enabled` | `false` | master switch for the whole rent lifecycle; off = nothing is charged or suspended |
| `rent.amount` | `100` | rent charged every period, in the currency below |
| `rent.currency` | `"default"` | currency the rent is charged in |
| `rent.period-days` | `7` | how long one paid term lasts |
| `rent.grace-days` | `3` | how long a suspended warp waits before it is archived |
| `rent.hard-delete-after-days` | `-1` | -1 = never hard-delete an archived warp (reserved for a later task; archives stay recoverable) |
| `rent.check-interval-minutes` | `60` | how often the off-tick sweep runs (charges, retries, reminders) |
| `rent.reminder-hours` | `[24, 12, 6, 1]` | send the owner a mail this many hours before the term lapses (one per window) |
| `rent.exempt.players` | `[]` | owner names or uuids never charged rent |
| `rent.exempt.categories` | `[]` | category ids never charged rent |
| `rent.exempt.worlds` | `[]` | world names never charged rent |
| `sponsor.enabled` | `false` | master switch for the whole sponsorship sub-group; off = no slot sold, swept or pinned |
| `sponsor.slots` | `5` | how many sponsor slots exist (a purchase takes the lowest free one) |
| `sponsor.duration-days` | `7` | the longest term one purchase may run; an omitted /pwarp sponsor term defaults to this |
| `sponsor.price` | `1000` | the fee charged per purchase, in the currency below |
| `sponsor.currency` | `"default"` | currency the fee is charged in |
| `sponsor.max-concurrent-per-player` | `1` | how many live sponsorships one owner may hold at once |
| `sponsor.cooldown-days` | `3` | how long a warp waits after its sponsorship expires before it can be sponsored again |
| `sponsor.check-interval-minutes` | `60` | how often the off-tick expiry sweep runs (frees lapsed slots, stamps cooldowns) |
| `cross-server.enabled` | `false` | master switch for the whole cross-server sub-group; off = remote warps are refused, no join handling |
| `cross-server.arrival-delay-ticks` | `20` | how long after the target-server join to settle before completing the local hop (20 ticks = 1s) |
| `cross-server.request-ttl-seconds` | `30` | a pending teleport older than this is treated as abandoned: cleared and refunded instead of honoured |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_playerwarp_<warp>_owner%` | Who owns one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_visits%` | How many times one of the player's warps has been used. |
| `%uxmessentials_playerwarp_<warp>_world%` | The world one of the player's warps sits in. |
| `%uxmessentials_playerwarp_<warp>_x%` | The block x of one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_y%` | The block y of one of the player's warps. |
| `%uxmessentials_playerwarp_<warp>_z%` | The block z of one of the player's warps. |
| `%uxmessentials_playerwarps_count%` | How many player warps the player owns. |
| `%uxmessentials_playerwarps_left%` | How many more player warps the player may create. |
| `%uxmessentials_playerwarps_limit%` | How many player warps the player may own; the infinity marker when unlimited. |
| `%uxmessentials_playerwarps_list%` | The names of the player warps the player owns, comma separated. |
{/* /generated */}

## Notes

- **Visibility and access are separate axes.** Visibility decides whether the warp is listed, access decides who
  may enter (`PUBLIC`, `PASSWORD`, `WHITELIST`, `PRIVATE`). A listed warp nobody may enter is a valid setup, and
  so is an unlisted warp anyone with the name may use.
- **Entry fees are split.** `payout.cut-percent` is the server's share; the rest banks on the warp until the
  owner runs `/pwarp withdraw`, or is paid immediately when `auto-payout` is on and the currency can be written
  to an offline player.
- **Rent and sponsorship are off by default and need an economy.** Rent charges the warp's own earnings first,
  then the owner's wallet, and a warp that cannot pay is suspended, then archived. Sponsorship buys a pinned slot
  in the browse for a number of days.
- **Archived is not deleted.** Only `/pwarp admin purge <id> confirm` destroys a warp; everything else is
  reversible with `/pwarp admin restore`.
- **The quota is `uxmessentials.pwarp.limit.<n>`,** optionally suffixed with a world, and the highest tier held
  wins. Without a node `default-limit` decides.
- **Cross-server warps need a proxy, a shared database and a unique `network.server-id`.** The fee is charged
  here, the intent is recorded in the shared table, and the target backend finishes the teleport on join. If the
  warp is gone by then, or no proxy channel is registered, the player is refunded rather than moved.
- **`uxmessentials.pwarp.gui` widens the browse rather than granting it.** Without it a player still opens the
  GUI, scoped to their own warps.

Related: [Warps](warps.md), [Economy](economy.md), [Cross-server](../cross-server/overview.md)
