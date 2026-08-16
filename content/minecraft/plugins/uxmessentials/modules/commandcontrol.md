---
title: Command Control
order: 9010
description: Decide which commands each player may run, hide the rest, and close the namespace escape.
icon: terminal
---

Command Control decides which commands a player may run and hides the rest. A denied command is cancelled
before it reaches its handler and answered with your own deny line, so a hidden command can be made to read as
though it never existed. It also filters tab-completion, hides the plugin-listing and help commands, and closes
the `/minecraft:gamemode` namespace escape. It stores nothing: every rule comes from config.

Module `commandcontrol` · enabled by default · `modules/commandcontrol/config.conf`

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.commandcontrol.bypass` | op | Exempt from the command whitelist/blacklist gate and the tab-completion / plugin-hide scrub. |
| `uxmessentials.commandcontrol.channelhide.bypass` | op | Exempt from the plugin-channel hider - the full channel-registration list is sent to this player. |
| `uxmessentials.commandcontrol.spam.bypass` | op | Exempt from the command-spam rate limiter - commands are never counted and no spam action fires. |
| `uxmessentials.commandcontrol.viewplugins` | op | See the plugin-listing / help commands (/plugins, /pl, /help, ...) hidden by the plugin-hide feature. |
| `uxmessentials.module.commandcontrol` | op | Hot-reload / inspect the commandcontrol module (command whitelist, tab-completion filter, plugin-hide). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `mode` | `blacklist` | How the command lists below are read: blacklist = the listed commands are DENIED, everything else is allowed (name the few commands to hide). whitelist = only the listed commands are ALLOWED, everything else is denied (a lock-down; name the full list a group may run). An empty whitelist denies every command, so populate it before switching. |
| `use-unknown-command-message` | `true` | Which deny message to show when a command is blocked. true shows the vanilla-style "unknown command" line, so a hidden command reads as though it does not exist; false shows an honest "you don't have permission" line. The text of both lines lives in the message catalog (messages_\<lang>.conf, keys commandcontrol.unknown-command and commandcontrol.no-permission): edit it there. |
| `commands.default` | `[ ]` |  |
| `tab-completion.enabled` | `true` |  |
| `plugin-hide.enabled` | `true` |  |
| `plugin-hide.hidden-commands` | `[ "plugins", "pl", "?", "help", "ver", "version", "about", "icanhasbukkit" ]` |  |
| `plugin-hide.deny-list-commands` | `true` |  |
| `block-namespace-bypass` | `true` | Namespace-bypass block. A player can otherwise dodge the lists above by prefixing a command with its namespace; "/minecraft:gamemode", "/bukkit:pl", "/somePlugin:cmd" all reach the same command as the bare form but read as a different label. With this on, the "namespace:command" form of any command whose bare form is denied is blocked too, so "/minecraft:gamemode" is treated exactly like "/gamemode". Holders of uxmessentials.commandcontrol.bypass may still use namespaced forms. Leave it on unless a plugin of yours legitimately relies on namespaced command input. |
| `auto-lowercase-base-commands` | `true` | Auto-lowercase the base command label. When on, the leading command word is lowercased before every check and before the command runs, so "/GAMEMODE creative" is treated and executed exactly as "/gamemode creative". Only the base label is touched - arguments (a target player, a world name, a message body) keep their casing. Turn it off only if a plugin of yours registers a genuinely case-sensitive command label. |
| `command-spam.enabled` | `false` |  |
| `command-spam.max-per-window` | `40` |  |
| `command-spam.window-seconds` | `2` |  |
| `command-spam.action` | `BLOCK` |  |
| `plugin-channel-hide.enabled` | `false` |  |
| `plugin-channel-hide.allowed-channels` | `[ "minecraft:brand", "bungeecord:main", "velocity:main" ]` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_commandcontrol_allowed_<command>%` | Whether the player may run that command where they stand (yes/no), answered from the rules the gate uses. |
{/* /generated */}

## Notes

- **The module ships inert.** The bundled config is a blacklist with empty lists, so nothing of yours is blocked
  until you name it.
- **`mode` reads every list the same way.** In `blacklist` the listed commands are denied and everything else
  is allowed; in `whitelist` only the listed commands are allowed. Switching to whitelist with an empty list
  denies everything, so fill the list first.
- **Blocking the namespace bypass matters.** Without it `/minecraft:gamemode` reaches the same handler under a
  different root and slips past a rule written for `/gamemode`.
- **Plugin-hide is on by default** and both hides and refuses `/plugins`, `/help`, `/ver` and friends for anyone
  without the view permission, which is what stops a player fingerprinting your server.
- **Spam protection is off by default.** Switched on it rate-limits commands in a sliding window and either
  blocks the offending command, warns, or kicks.
- **Per-world overrides let one world run a different rule set,** a lobby for instance, without a second module.

Related: [Player State](playerstate.md), [Security](security.md), [Server Tweaks](servertweaks.md)
