---
title: "Guide"
order: 10
description: "How uxmSimpleClaims works, how to set it up and what each file holds."
---

uxmSimpleClaims protects land by the chunk. A player claims the chunk they stand in, grows the claim chunk by
chunk or by a square, and decides who may do what inside it. Everything a server owner might not want can be
configured or turned off in `config.conf`.

## Install

1. Put the jar in `plugins/`. A currency plugin (Vault, VaultUnlocked, PlayerPoints, EcoBits or Treasury) is
   needed only if claims cost money; PlaceholderAPI, LuckPerms, WorldGuard and the web map plugins are optional.
2. Start the server once. The plugin writes its files into `plugins/uxmSimpleClaims/`.
3. Set `network.server-name` to this server's name, choose the world modes and prices, then `/claimadmin reload`.

Turkish readers see Turkish command words: `/arazi` for `/claim` and `/araziyonetim` for `/claimadmin`.

## Claiming

- `/claim` claims the chunk you stand in, or opens the menu of the claim you stand in.
- A chunk that touches one of your claims joins it; one apart starts a new claim, named `claim1`, `claim2`
  and on unless you name it with `/claim here <name>`. A claim is always one connected piece.
- `/claim radius <n>` claims the square around you, `n` chunks to each side, up to `claims.max-radius`.
- `/claim auto` claims as you walk, `/claim auto unclaim` gives land up as you walk; run it again to stop.
- `/claim unclaim` gives up the chunk you stand in, unless that would cut the claim in two. The last chunk
  deletes the claim. `/claim delete` deletes the whole claim after you type it again within ten seconds.
- `/claim merge <claim>` joins another of your claims that touches this one; `/claim split <name>` cuts off the
  side of the claim you face, the row you stand in included, as a new claim.
- `/claim map` shows the land around you; `/claim border` draws the claim's edges for a few seconds.

Claims keep `claims.min-distance` chunks away from other players' claims and never cover a WorldGuard region.

### What claims cost

`economy.price` sets the price of a chunk by how many a player already owns:

| `curve` | Price of a chunk past the free ones |
|---|---|
| `flat` | `base` |
| `linear` | `base`, plus `step` for every chunk already paid for |
| `exponential` | `base`, times `factor` for every chunk already paid for |
| `tiered` | the price of the first tier whose `up-to` is not below the chunk count |

`free-chunks` are free, `maximum` caps one chunk, and `refund-percent` of a chunk's price comes back when it
is given up. Staff on bypass pay nothing.

### Limits

`limits.default` holds what every player may have: claims, chunks per claim, chunks in all, members, roles of
their own and sub areas. Another group, such as `limits.vip`, applies to players with
`uxmsimpleclaims.limit.vip`; a player with several groups gets the highest of each. `uxmsimpleclaims.limit.unlimited`
lifts every limit.

## Members and roles

- `/claim invite <player> [duration]` sends an invite that lasts `members.invite-minutes`; the player joins with
  `/claim accept`. With `members.direct-add = true` the player joins at once. A duration such as `7d` makes the
  membership end on its own.
- `/claim kick`, `/claim ban <player> [duration]` and `/claim unban` keep players out; a ban takes a member's
  role away. `/claim leave` leaves a claim you are in.
- Every claim starts with the roles in `roles.conf`: `visitor` (everybody who is not a member), `member`,
  `moderator` and `owner`. An owner can add roles of their own in the roles window and choose what each may do.
  A player may act only on members whose role is below their own.
- `/claim transfer <player>` gives the claim away; the old owner is not left inside.

## Flags and lists

Flags decide what happens on its own inside a claim: explosions, fire spread, liquid flow, growth, monster and
animal spawns, mob griefing, PvP, mob damage, fall damage, snow and ice, redstone and pistons from outside.
`flags.claim-defaults` sets them for every claim that has not changed one; `flags.locked` keeps a flag out of
owners' hands. A player changes a flag only with `uxmsimpleclaims.flag.<flag>`.

Lists hold visitors to blocks they may break, place or use, and mobs that may spawn or be handled. A whitelist
lets only what it names through; a blacklist stops what it names. The owner is never held.

## Sub areas and rent

A sub area is a set of chunks inside a claim with its own members and flags. `/claim areacreate <name>` makes
one of the chunk you stand in and `/claim areachunk <area>` adds or takes out a chunk. The owner can offer a sub
area for rent with `/claim areaoffer <area> <price> [days]`; a player rents it with `/claim rent` and holds the
member role inside it. Rent renews when the period ends if the tenant can pay; a tenant who is away is charged
at their next join, and the tenancy ends when `rent.grace-hours` pass unpaid. `/claim unrent` stops renewing.

## The market and warps

`/claim sell <price>` lists a claim and `/claim buy` buys the one you stand in; `/claim market` lists everything
for sale or rent. A buyer gets the claim empty: no members, bans or tenants. A seller who is away is paid at
their next join.

`/claim setspawn` sets where teleports land and `/claim warp open [fee]` lets every player teleport there with
`/claim tp <claim>`, paying the owner the fee. Visits count towards the visits board.

## Taxes, time limits and inactivity

Each is off by default.

- `tax`: every `period-days` the owner pays `per-chunk` for each chunk. An owner who has not paid when
  `grace-hours` run out loses their newest chunk, then the next one a period later.
- `expiry`: a claim lasts `start-days`, and `/claim extend <days>` buys more at `daily-per-chunk` for each chunk,
  up to `max-days` ahead. The owner is warned `warn-days` before it ends.
- `inactivity`: the claims of an owner away for `days` are deleted, or with `action = transfer` given to their
  highest member. `uxmsimpleclaims.inactivity.exempt` keeps a player's claims.

A deleted claim is kept as a snapshot for `retention.snapshots-days`; staff list them with
`/claimadmin snapshots` and bring one back with `/claimadmin restore`.

## Entering and leaving

Walking into a claim can show a title, an action bar line, a chat line, a boss bar and the owner's greeting;
`display` turns each on or off. `teleport.policy` decides whether another plugin's teleport may bring a player
into a claim: `strict` only if the claim lets them teleport there, `consent` also when they teleport to a member,
`free` always.

## Networks

Servers that use the same MySQL, MariaDB or PostgreSQL database share their claims. Give each server its own
`network.server-name` name. Set `network.redis` to a Redis URL so a change reaches the other servers at once; without
it they read changes every `storage.sync-seconds`. With `network.cross-server-teleport` a teleport to a claim on
another server sends the player there through the proxy.

## Troubleshooting

- `/claimadmin doctor` says whether storage, the windows, the claims and the placeholders are working.
- `/claimadmin inspect` and the inspection stick from `/claimadmin stick` say who owns a chunk or a block.
- A menu word or message missing in your language falls back to the default the plugin carries.
