---
title: Behaviour changes
order: 124
description: What 4.0 does differently from 3.x on purpose, and what the migration keeps the 3.x way.
icon: git-compare
---

4.0 is split into modules, so files, commands and a few defaults moved. Where a 3.x habit would change under
your players, the migration writes the 3.x value for upgraded servers; a clean 4.0 install gets the new
default.

## Admin bypass

| | 3.x | 4.0 clean install | 4.0 after migration |
|---|---|---|---|
| Who bypasses land protection | OP and `uxmclaims.admin`, always | Players with `uxmclaims.admin.bypass` who ran `/uxm bypass on` | OP, `*` and `uxmclaims.admin`, always |
| Setting | none | `protection.admin-bypass: TOGGLE` | `protection.admin-bypass: IMPLICIT` |

The setting is `protection.admin-bypass` in `modules/lands/config.yml`. With `TOGGLE`, `/uxm bypass [on|off]`
switches it for yourself until you turn it off or leave; with `IMPLICIT` the command only says that admins
always bypass. The root `config.yml` no longer has this key: 4.0.0 deletes it on the first start.

## Commands

| 3.x | 4.0 |
|---|---|
| `/claim reload` | `/uxm reload` (config, language and menu files; modules need a restart) |
| `/claim admin ...` tools | Still `/claim admin ...`, and also `/uxm land ...` |
| Admin and server tools | Under `/uxm`: `/uxm modules`, `/uxm db`, `/uxm preset`, `/uxm setup`, `/uxm lang update`, `/uxm migrate`, `/uxm bypass`, `/uxm config check` |
| Command names fixed in English | Local names and aliases per language in `lang/<code>/commands.yml` (needs a restart) |

Player commands under `/claim` keep their 3.x names where the feature exists; each module page lists its own.

## Files

| 3.x | 4.0 |
|---|---|
| `config.yml` (everything) | Root `config.yml` (kernel: language, storage, license) plus `modules/<id>/config.yml` per module |
| Feature switches inside `config.yml` | `modules.yml`, one line per module |
| `roles.yml` | `modules/lands/roles.yml`; the 3.x `default` role is the `visitor` role |
| `claimSettings.defaultFlags` | `modules/lands/flags.yml` (`defaults`) |
| `generalSettings.disabledWorlds` | `modules/lands/worlds.yml` (`claimable: false` per world) |
| `entitlements.yml` | `modules/limits/limits.yml` (limits) and `modules/economy/costs.yml` (costs) |
| `aliases.yml` | `modules/aliases/aliases.yml` |
| `webhooks.yml` | `modules/webhooks/embeds.yml` and `senders.yml` |
| `blocks` in `config.yml` | `modules/claim-block/blocks.yml` |
| `messages.yml`, `lang/` | `lang/<code>/messages.yml` (kernel) and `modules/<id>/lang/<code>/messages.yml` |
| `menu/` | `modules/<id>/lang/<code>/menus/`, only for menus you edited |
| `data/claims.db`, tables `uxmclaims_*` | `data/storage.db`, tables `uxmc_*` |
| Times in seconds (`2592000`) | Duration text (`30d`, `90s`) |

The 3.x files stay in `legacy/v3/` after the move.

## Modules after the migration

| Module | Written as |
|---|---|
| `lands` | on (required) |
| `invites`, `warps`, `vault`, `claim-block`, `holograms`, `borders`, `notifications`, `land-chat`, `limits`, `webhooks`, `aliases`, `legacy-api`, `bans` | on: 3.x could not turn them off |
| `expiry` | from `claimSettings.expireMode` |
| `economy` | from `generalSettings.economySupport`, but off when Vault is missing: paid actions stay free, as they were |
| `luckperms`, `worldguard`, `placeholders` | from `permissionSupport`, `worldGuardSupport`, `placeholderSupport` |
| `webmap`, `dialogs`, `bedrock` | from `visualizer.enabled`, `dialog.enabled`, `bedrock.enabled` |
| `areas`, `towns`, `nations`, `relations`, `bank`, `taxes`, `inbox` | off: new in 4.0 |

Modules not in this list get their own default when `modules.yml` is filled in on the first start.

## Menus

The 4.0 menus have a new layout. The migration carries the titles, item names and lore you edited in 3.x;
slots, materials and layout are not carried, and a text that uses a placeholder the 4.0 menu does not know is
left out so no player sees a raw `%token%`. An edited menu is written whole to
`modules/<id>/lang/<code>/menus/<menu>.yml`, and a menu file on disk replaces the jar copy. The looks of the
3.x Bedrock form files (labels, clicks, content) are not carried; only `claim_map` `gridRadius` becomes the
bedrock module's `map-grid-radius`. See [Menus](../configuration/menus.md).

## Messages

A 3.x message you edited is carried into the 4.0 language file with its placeholders rewritten; a message
equal to the 3.x default takes the 4.0 text. A message object is carried whole when any of its fields was
edited. A custom `claimSettings.defaultName` becomes `land.default-name` in the lands messages.

## Other kept behaviour

| Behaviour | Kept as |
|---|---|
| Admins see every land chat | `admin-spy-default: true` in `modules/land-chat/config.yml` (new installs: off) |
| 3.x keys with no 4.0 target: `hologram.lines`, `visualizer.tooltip.lines`, `border.visualizerType` | Dropped (`I-DROPPED`), kept in `legacy/v3/config.yml` |
| `blocks.<id>.icon` name and lore | 4.0 default texts (`I-BLOCK-ICON-TEXT`) |

## Notes

- 4.0 ships eight languages: `en`, `tr`, `de`, `es`, `fr`, `pl`, `pt-BR` and `ru`. A 3.x server set to one of
  them keeps it; any other language code is kept too, but players see the English texts except for the
  messages you had edited. See [Languages](../configuration/languages.md).
- The 3.x update check (`generalSettings.updateCheck`) is dropped: 4.0 has no update check.

Related: [From 3.x](from-3x.md), [Modules](../modules/index.md), [Kernel config](../configuration/kernel.md)
