---
title: Placeholders
order: 160
description: How uxmClaims placeholders are named, and which module answers each prefix.
icon: braces
---

Every uxmClaims placeholder has the form `%uxmclaims_<prefix>_<key>%` and needs PlaceholderAPI. The
kernel owns the `uxmclaims` expansion; each module adds its own prefix when it starts, so a placeholder
of a module that is off prints nothing. Each module's page lists its keys. This page tells you which
page to open.

## Prefixes

| Prefix | Example | Module |
|---|---|---|
| `land` | `%uxmclaims_land_name%` | [PlaceholderAPI](modules/placeholders.md): the land in context |
| `here` | `%uxmclaims_here_land_name%` | [PlaceholderAPI](modules/placeholders.md): only the land underfoot |
| `player` | `%uxmclaims_player_lands%` | [PlaceholderAPI](modules/placeholders.md) |
| `server` | `%uxmclaims_server_lands%` | [PlaceholderAPI](modules/placeholders.md) |
| `is` | `%uxmclaims_is_in_land%` | [PlaceholderAPI](modules/placeholders.md) |
| `claim` | `%uxmclaims_claim_name%` | [PlaceholderAPI](modules/placeholders.md): the 3.x paths |
| `selected` | `%uxmclaims_selected_...%` | [Lands](modules/lands.md): the land picked with `/claim select` |
| `expiry` | `%uxmclaims_expiry_...%` | [Expiry](modules/expiry.md) |
| `invites` | `%uxmclaims_invites_...%` | [Invites](modules/invites.md) |
| `bans` | `%uxmclaims_bans_...%` | [Bans](modules/bans.md) |
| `warps` | `%uxmclaims_warps_count%` | [Warps](modules/warps.md) |
| `vault` | `%uxmclaims_vault_...%` | [Land Vault](modules/vault.md) |
| `claim-block` | `%uxmclaims_claim-block_...%` | [Claim Block](modules/claim-block.md) |
| `claim-points` | `%uxmclaims_claim-points_...%` | [Claim Points](modules/claim-points.md) |
| `town` | `%uxmclaims_town_...%` | [Towns](modules/towns.md) |
| `nation` | `%uxmclaims_nation_...%` | [Nations](modules/nations.md) |
| `bank` | `%uxmclaims_bank_...%` | [Bank](modules/bank.md) |
| `plots` | `%uxmclaims_plots_...%` | [Plots](modules/plots.md) |
| `levels` | `%uxmclaims_levels_...%` | [Levels](modules/levels.md) |
| `war` | `%uxmclaims_war_...%` | [War](modules/war.md) |
| `jails` | `%uxmclaims_jails_...%` | [Jails](modules/jails.md) |
| `top` | `%uxmclaims_top_...%` | [Leaderboards](modules/leaderboards.md) |

When two prefixes could match, the longest one wins, so `claim-block` is never read as `claim`.

## Which land a placeholder means

"The land in context" is the land the player stands in, else the land they picked with
`/claim select`, else the only land they own. `here_` keys only look underfoot. Outside every land, a
placeholder prints the default from the module's `messages.yml` (`defaults.*`), or nothing. The exact
order and the switch that turns the owned-land fallback off are on
[PlaceholderAPI](modules/placeholders.md).

## Coming from 3.x

3.x placeholders keep working while `legacy-paths` is on in `modules/placeholders/config.yml`:
`%uxmclaims_claim_name%` is answered as the new path. 3.x paths with a modifier, such as
`%uxmclaims_count:claim_members%`, go through the alias table in the same module. Move your
scoreboards to the new names when you can; see [PlaceholderAPI](modules/placeholders.md).

## Inside uxmClaims texts

Messages, menus and holograms use MiniMessage tags such as `<land>` filled by the plugin itself, not
PlaceholderAPI. Where a text accepts PlaceholderAPI placeholders as well, the module page says so.

Related: [PlaceholderAPI](modules/placeholders.md), [Integrations](integrations.md)
