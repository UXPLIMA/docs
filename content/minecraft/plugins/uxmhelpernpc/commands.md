---
title: Commands
order: 20
icon: terminal
---

## Player commands

`/helpernpc` is the player command. Turkish aliases are registered alongside the English ones, so
`/helpernpc iade` and `/helpernpc return` are the same command.

| Command | Aliases | Permission | What it does |
|---|---|---|---|
| `/helpernpc` | | — | Sends the help message, or opens the NPC menu if `helpernpc-command-opens-gui` is on |
| `/helpernpc buy` | `satınal` | — | Opens the shop menu. Refused when `disable-buy-command` is true |
| `/helpernpc menu` | `menü` | `uxmhelpernpc.command.menu` | Opens your NPC's main menu |
| `/helpernpc return` | `iade` | `uxmhelpernpc.returnnpc` | Deletes your NPC and refunds `return-percent` of the price |
| `/helpernpc skin <name\|reset>` | | `uxmhelpernpc.changeskin` | Changes the NPC skin, or clears it |
| `/helpernpc teleport` | `getir`, `tp`, `move` | `uxmhelpernpc.command.teleport` | Moves your NPC to where you are standing |
| `/helpernpc disappear` | `yoket`, `getrid` | `helpernpc.command.disappear` | Hides or shows your NPC without deleting it |

<Callout type="warning" title="The disappear node is not the one in plugin.yml">

`plugin.yml` declares `uxmhelpernpc.disappear`, but the command checks
`helpernpc.command.disappear`. Grant the node the command checks — the one in the table above.

</Callout>

Three things are refused before any of these run: a world listed in `settings.disabled-worlds`,
standing outside your own region while a claim hook is active, and not owning an NPC at all.

## Cooldowns

| Cooldown | Option | Default |
|---|---|---|
| Returning an NPC | `cooldown.return-cooldown` | 120s |
| Moving an NPC | `cooldown.move-cooldown` | 180s |
| Changing glow colour | `cooldown.change-glowing-cooldown` | 120s |
| Asking the assistant | `cooldown.ask-question-cooldown` | 15s |

Admins in bypass mode (`/uxmhelpernpc bypass`) skip them.

## Admin commands

`/uxmhelpernpc` requires `uxmhelpernpc.command.admin` for the help output; each subcommand has its
own node.

| Command | Permission | What it does |
|---|---|---|
| `/uxmhelpernpc reload` | `uxmhelpernpc.command.reload` | Reloads config, items, languages and menus |
| `/uxmhelpernpc save` | `uxmhelpernpc.command.save` | Flushes NPC data to the database |
| `/uxmhelpernpc bypass` | `uxmhelpernpc.command.bypass` | Toggles cooldown and ownership bypass for yourself |
| `/uxmhelpernpc uuidof <player>` | `uxmhelpernpc.command.uuidof` | Lists the internal ids of a player's NPCs |
| `/uxmhelpernpc delete <player>` | `uxmhelpernpc.command.delete` | Deletes one NPC belonging to that player |
| `/uxmhelpernpc deleteall <player>` | `uxmhelpernpc.command.deleteall` | Deletes every NPC that player owns |
| `/uxmhelpernpc givenpc <player>` | `uxmhelpernpc.command.givenpc` | Places an NPC for a player, free of charge |
| `/uxmhelpernpc giveegg <player> <amount>` | `uxmhelpernpc.command.giveegg` | Gives NPC eggs. Skipped slots are reported to both of you |
| `/uxmhelpernpc ask` | `uxmhelpernpc.askai` | Starts an assistant question, as if you had clicked the ask button |

## Permission reference

| Node | Default | Grants |
|---|---|---|
| `uxmhelpernpc.command.menu` | everyone | `/helpernpc menu` |
| `uxmhelpernpc.returnnpc` | op | `/helpernpc return` |
| `uxmhelpernpc.changeskin` | op | `/helpernpc skin` |
| `uxmhelpernpc.command.teleport` | op | `/helpernpc teleport` |
| `helpernpc.command.disappear` | op | `/helpernpc disappear` |
| `uxmhelpernpc.askai` | op | The assistant |
| `uxmhelpernpc.command.admin` | op | The admin help output |
| `uxmhelpernpc.command.*` | op | One node per admin subcommand, listed above |

Skins can carry their own node through `npc.permission-skins`:

```yaml
permission-skins:
  "Notch": "uxmhelpernpc.usenotch"
```
