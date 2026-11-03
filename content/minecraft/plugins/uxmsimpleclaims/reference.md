---
title: "Reference"
order: 20
description: "Every command, permission, placeholder and file of uxmSimpleClaims."
---

## Commands

`/claim` (alias `/claims`, Turkish `/arazi`) needs `uxmsimpleclaims.use`; `auto` needs `uxmsimpleclaims.auto`,
`lang` needs `uxmsimpleclaims.language`. The Turkish word replaces the last word of a subcommand. Every word
can be changed in `commands.conf`.

| `/claim ...` | Turkish | What it does |
|---|---|---|
| `claim` |  | Claim the chunk you stand in, or open the claim's menu |
| `here [name]` | `burasi` | Claim the chunk you stand in |
| `menu` | `menu` | Open the claims menu |
| `radius <radius>` | `yaricap` | Claim the square around you |
| `unclaim` | `birak` | Give up the chunk you stand in |
| `delete` | `sil` | Delete the claim you stand in |
| `info` | `bilgi` | Say whose land you stand on |
| `map` | `harita` | Open the map of the land around you |
| `border` | `sinir` | Show the edges of the claim you stand in |
| `auto [mode]` | `oto` | Claim or give up land as you walk |
| `list` | `liste` | List your claims and the ones you are in |
| `tp <claim>` | `git` | Teleport to a claim |
| `invite <player> [duration]` | `davet` | Invite a player to the claim you stand in |
| `accept [claim]` | `kabul` | Join a claim you were invited to |
| `deny [claim]` | `reddet` | Turn down an invite |
| `leave` | `ayril` | Leave the claim you stand in |
| `kick <player>` | `at` | Take a member out of the claim you stand in |
| `ban <player> [duration]` | `yasakla` | Keep a player out of the claim you stand in |
| `unban <player>` | `affet` | Let a banned player back in |
| `role <player> <role>` | `rol` | Give a member another role |
| `transfer <player>` | `devret` | Give the claim you stand in to a player |
| `rename <name>` | `adlandir` | Rename the claim you stand in |
| `describe [text]` | `aciklama` | Describe the claim you stand in |
| `greeting [text]` | `karsilama` | Set what visitors read when they arrive |
| `farewell [text]` | `veda` | Set what visitors read when they leave |
| `icon` | `simge` | Make the item you hold the claim's icon |
| `colour <colour>` | `renk` | Set the claim's map colour, as #rrggbb |
| `category <category>` | `kategori` | Put the claim in a category |
| `setspawn` | `noktakoy` | Make where you stand the claim's spawn |
| `warp <state> [fee]` | `ziyaret` | Open or close the claim to visitors |
| `flag <flag> [value]` | `ayar` | Change a flag of the claim you stand in |
| `merge <claim>` | `birlestir` | Join another of your claims into this one |
| `split <name>` | `bol` | Split off the part of the claim you face |
| `sell <price>` | `sat` | Put the claim you stand in up for sale |
| `buy` | `satinal` | Buy the claim you stand in |
| `market` | `pazar` | Open the market of claims and sub areas |
| `rent` | `kirala` | Rent the sub area you stand in |
| `unrent` | `kirabitir` | Stop renewing the sub area you rent here |
| `extend <days>` | `uzat` | Pay for more days of the claim you stand in |
| `areacreate <name>` | `bolumac` | Make a sub area of this chunk |
| `areachunk <area>` | `bolumchunk` | Add this chunk to a sub area or take it out |
| `areadelete <area>` | `bolumsil` | Delete a sub area |
| `areamember <area> <player> [role]` | `bolumuye` | Give a player a role in a sub area |
| `areaoffer <area> <price> [days]` | `bolumkira` | Offer a sub area for rent |
| `areaflag <area> <flag>` | `bolumayar` | Change a flag inside a sub area |
| `inbox` | `kutu` | Read what happened while you were away |
| `favourite` | `favori` | Keep the claim you stand in among your favourites |
| `lang` | `dil` | Say what language you read this plugin in |
| `lang set <language>` | `sec` | Read this plugin in one language |
| `lang reset` | `sifirla` | Follow your client's language again |

`/claimadmin` (Turkish `/araziyonetim`) needs `uxmsimpleclaims.admin.<permission>`:

| `/claimadmin ...` | Turkish | Permission | What it does |
|---|---|---|---|
| `reload` | `yenile` | `reload` | Read the configuration again |
| `version` | `surum` | `version` | Read the version this server runs |
| `doctor` | `durum` | `doctor` | Say how this plugin is |
| `bypass` | `gecis` | `bypass` | Ignore every claim, or stop ignoring them |
| `stick` | `cubuk` | `inspect` | Get the stick that says who owns a block |
| `inspect` | `incele` | `inspect` | Say who owns the chunk you stand in |
| `limits <player>` | `limitler` | `limits` | Say what a player may have |
| `delete` | `sil` | `delete` | Delete the claim you stand in |
| `transfer <player>` | `devret` | `transfer` | Give the claim you stand in to a player |
| `resetflags` | `ayarsifirla` | `flags` | Put the flags of the claim here back |
| `resetflags all` | `hepsi` | `flags` | Put the flags of every claim back |
| `snapshots [player]` | `yedekler` | `snapshots` | List the claims that were deleted |
| `restore <snapshot>` | `geriyukle` | `snapshots` | Put a deleted claim back |
| `profile <player>` | `profil` | `profile` | Open the claims of a player |
| `audit` | `gecmis` | `audit` | Read the history of the claim you stand in |

## Permissions

| Permission | Default | What it gives |
|---|---|---|
| `uxmsimpleclaims.use` | everyone | claim land and manage your claims |
| `uxmsimpleclaims.auto` | everyone | `/claim auto` |
| `uxmsimpleclaims.language` | everyone | choose the language of this plugin |
| `uxmsimpleclaims.flag.<flag>`, `uxmsimpleclaims.flag.*` | everyone | change that flag in your own claims |
| `uxmsimpleclaims.limit.<group>` | nobody | the limits of that group in `config.conf` |
| `uxmsimpleclaims.limit.unlimited` | nobody | no limit at all |
| `uxmsimpleclaims.inactivity.exempt` | op | your claims are never taken for inactivity |
| `uxmsimpleclaims.admin.manage` | op | manage every claim as if you owned it |
| `uxmsimpleclaims.admin.*` | op | every staff command above, `manage` and `bypass` |

The flags are `explosions`, `fire-spread`, `liquid-flow`, `growth`, `monster-spawn`, `animal-spawn`,
`mob-grief`, `pvp`, `mob-damage`, `fall-damage`, `snow-and-ice`, `outside-redstone` and `outside-pistons`.

## Placeholders

With PlaceholderAPI, written `%uxmsimpleclaims_<what>%`. `integrations.placeholders = false` turns them off.

| `<what>` | Answer |
|---|---|
| `here` | the claim you stand in, or the word for wilderness |
| `here_owner`, `here_role`, `here_size`, `here_colour` | its owner, your role there, its chunks, its map colour; empty in the wilderness |
| `in_claim` | `true` or `false` |
| `claims`, `chunks` | how many claims and chunks you own |
| `limit_claims`, `limit_chunks` | your limits |
| `top_<board>_alltime_<n>_name`, `_value`, `_head` | the player at place `n` |
| `rank_<board>_alltime`, `value_<board>_alltime` | your place and your score |

The boards are `chunks` (chunks owned), `members` (members over all your claims) and `visits` (visits through
your public warps). `leaderboard.size` and `leaderboard.refresh-seconds` set how many places they hold and how
often they are read.

## LuckPerms contexts

With `integrations.luckperms-contexts`, each `true` or `false` where the player stands: `in-own-claim`,
`in-claim-member`, `in-wilderness`. For example `/lp group default permission set essentials.fly true
in-own-claim=true`.

## Web maps

With `integrations.web-maps`, claims are drawn on BlueMap, Dynmap, Pl3xMap and squaremap when installed, in
each claim's colour, every `integrations.web-map-seconds`. A claim for sale says its price.

## For other plugins

`com.uxplima.claims.api.SimpleClaims` is on Bukkit's services manager: the claim at a location, a claim by id,
a player's claims, every claim, and whether a player may build at a location. `ClaimChangeEvent` is called on
the global thread after any claim is made, changed or deleted, on this server or another of the network.

## Files

| File | What it holds |
|---|---|
| `config.conf` | language, storage, network, world modes, claim rules, members, economy, tax, expiry, inactivity, rent, flags, display, teleport policy, retention, limits, integrations, leaderboard |
| `roles.conf` | the roles every new claim starts with |
| `commands.conf` | command and subcommand words, per language |
| `menus/*.conf` | the windows: claims, claim, members, bans, roles, role, flags, rules, areas, area, map, market, inbox, audit |
| `messages/messages_<lang>.conf`, `theme.conf` | words and colours |
| `data/claims.db` | the SQLite database, when `storage.type = "sqlite"` |

A database password, Redis URL or webhook URL can be written as `"${VARIABLE}"` and is read from the server's environment; a name that is not set stays as written.
