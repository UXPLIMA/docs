---
title: Admin
order: 208
description: Inspecting other players' claims, the bulk operations, and reloading the configuration.
icon: shield-check
---

Staff commands need `uxmclaims.admin`, declared in `plugin.yml` with `default: op`. The same node, or
being op, also passes every ability check, role permission, ownership check, limit, cost and cooldown
in the ordinary commands.

## Commands

| Command | What it does | Console |
|---|---|---|
| `/claim admin view <player>` | Opens a menu of every claim that player owns or belongs to | no |
| `/claim admin bulk setflag <flag> <true\|false>` | Sets one flag on every claim | yes |
| `/claim admin bulk setrolepermission <role> <perm> <true\|false>` | Sets one permission on one role in every claim | yes |
| `/claim admin bulk rename <base>` | Renames every claim to `<base>-1`, `<base>-2` and so on | yes |
| `/claim admin bulk setblock <style>` | Switches every claim's block to one style | yes |
| `/claim admin bulk destroyblock` | Removes every claim's block | yes |
| `/claim reload` | Reloads the configuration files | yes |

### Bulk operations

uxmClaims keeps every claim in memory, so a bulk command reaches every claim on the server. Each one
reports how many claims it is starting on and how many it changed. These replies are fixed English
text, whatever `language` is set to.

Bulk changes run as an admin action: no permission, cost, limit or cooldown applies, WorldGuard
regions are still respected, and webhooks still report each change. From the console the actor
is the system id `00000000-0000-0000-0000-000000000000`.

- `setrolepermission` matches `<role>` against each claim's role names and the role types `owner`,
  `member`, `default` and `custom`, and skips claims with no match. Tab completion offers the three
  system roles, the only names every claim shares.
- `setblock` ignores the style's `permission`.
- `rename` numbers claims in no particular order.

<Callout type="danger" title="Bulk commands do not confirm and cannot be undone">

There is no confirmation step and no rollback. `bulk rename` replaces every name players chose, and
`bulk setflag PVP true` turns PvP on in every claim at once. Back up the database first, and run
them when few players are online, since each one saves every claim.

</Callout>

### Reload

`/claim reload` works from the console and in game, and answers `uxmClaims has been reloaded!`.

| Step | What happens |
|---|---|
| 1. Language | If `language` in `config.yml` differs from the language the files are in, the files switch first (see below) |
| 2. Files | `roles.yml`, `config.yml`, `aliases.yml`, `messages.yml`, `webhooks.yml` and `entitlements.yml` are read again |
| 3. Aliases | The old aliases are unregistered, the ones in `aliases.yml` are registered, and online players get their command list again for tab completion |
| 4. Menus | The Java menu files in `menu/` are read again, `common_selection.yml` included; the Bedrock forms in `menu/bedrock/` are read on their next use |

A language switch accepts `en`, `tr`, `es`, `pt-BR`, `de`, `fr`, `ru` and `pl`, in any case, with `_`
for `-`, and plain `pt` for `pt-BR`. An unknown value falls back to English with a console warning.
The switch copies the current files to `backups/language-<from>-<yyyyMMdd-HHmmss>/`, then rewrites
every text you have not edited into the new language and keeps every text you changed.
`entitlements.yml` and `aliases.yml` hold no texts and stay as they are. The language in use is
recorded in `data/metadata.yml`; a file that fails to switch is tried again on the next reload or
restart. The console log reports how many texts changed and how many edited ones were kept.

These need a restart instead of a reload:

| Change | Why |
|---|---|
| `database` settings | The connection is opened at startup |
| `generalSettings.licenseKey` | The licence check is set up at startup |

## Permissions

| Node | Default | Effect |
|---|---|---|
| `uxmclaims.admin` | op | Admin commands, `/claim reload`, and passing every check in the other commands |
| `uxmclaims.bypass.*` | none | Passes every ability check |
| `uxmclaims.bypass.<category>.*` | none | Passes the ability checks of one category, for example `uxmclaims.bypass.warp.*` |
| `uxmclaims.bypass.<category>.<action>` | none | Passes one ability check, for example `uxmclaims.bypass.claim.delete` |
| `uxmclaims.bypass.teleport` | none | Teleports into claims regardless of bans and of `TELEPORT`, `ENDERPEARL_USE` and `CHORUS_TELEPORT` |
| `uxmclaims.bypass.enter` | none | Exempt from the access guard that ejects banned players and those without `MOVE_INSIDE` |

A bypass node replaces the ability check only. Role permissions, ownership checks, limits, costs and
cooldowns still apply, so staff with `uxmclaims.bypass.*` can act only in claims where their role
allows it. The full node list is in [Ability permissions](../permissions/abilities.md).

## Notes

- `/claim transfer` and `/claim member transfer` require the real owner, even for an admin.
- `/claim admin view` finds players uxmClaims has seen, online or not.
- In the admin view menu, a click opens a claim's management menu, shift and left click selects the
  claim, so the ordinary commands then act on it, and shift and right click deselects it.

Related: [Overview](overview.md), [Ability permissions](../permissions/abilities.md), [config.yml](../config/config-yml.md)
