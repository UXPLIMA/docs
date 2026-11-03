---
title: Command Control
order: 9110
description: Decide which commands each player may run, hide the rest, and close the namespace escape.
icon: terminal
---

Command Control decides which commands a player may run, down to their arguments, and hides the rest. A denied command is cancelled
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
| `enabled` | `true` | Turns the whole module off with `false`. |
| `mode` | `blacklist` | How the command lists are read. `blacklist`: the listed commands are denied, everything else is allowed. `whitelist`: only the listed commands are allowed. An empty whitelist denies every command, so fill it before switching. |
| `use-unknown-command-message` | `true` | `true` answers a blocked command with the vanilla-style unknown command line, so it reads as though it does not exist; `false` with an honest no-permission line. Both lines are `commandcontrol.unknown-command` and `commandcontrol.no-permission` in the messages files. |
| `commands.<group>` | `default = [ ]` | The command lists, one per permission group. A player's primary group (from LuckPerms, when installed) picks its list; a player whose group has none uses `default`. See [Writing the lists](#writing-the-lists). |
| `tab-completion.enabled` | `true` | Removes every command a player may not run from the command list the client is sent, so it neither autocompletes nor shows in suggestions. Bedrock players through Geyser included. |
| `tab-completion.languages.<tag>` | empty | Command words for the readers of one language (`tr`, `en`, `pt-BR`). A word listed under a language is left out of the command list of a player who reads another one, so a Turkish player finds `/ayarlar` and an English player `/settings`. Typing it still runs it for anyone the lists allow. A word in no list, such as `/ping`, is everybody's. Applies to every player, operators too. |
| `tab-completion.hidden` | `[ ]` | Command words kept out of every player's command list that still run when typed. Use it for the plain-letter spelling of a word, such as `yardim` next to `yardım`, so a keyboard without the language's letters still reaches it and the list does not show it twice. |
| `plugin-hide.enabled` | `true` | Hides the plugin-listing and help commands from players without `uxmessentials.commandcontrol.viewplugins`. |
| `plugin-hide.hidden-commands` | `[ "plugins", "pl", "?", "help", "ver", "version", "about", "icanhasbukkit" ]` | The command roots plugin-hide covers. The `bukkit:`, `minecraft:` and `paper:` forms are matched too. |
| `plugin-hide.deny-list-commands` | `true` | Also refuses those commands, so `/plugins` cannot leak plugin names through its output. The deny line is `commandcontrol.plugin-hidden`. |
| `block-namespace-bypass` | `true` | Blocks the `namespace:command` form of any command whose bare form is denied, so `/minecraft:gamemode` is treated exactly like `/gamemode`. |
| `auto-lowercase-base-commands` | `true` | Lowercases the command word before every check and before it runs, so `/GAMEMODE creative` runs as `/gamemode creative`. Arguments keep their casing. |
| `command-spam.enabled` | `false` | Rate-limits the commands a player sends inside a sliding window. |
| `command-spam.max-per-window` | `40` | The most commands allowed inside the window. |
| `command-spam.window-seconds` | `2` | The width of the window, in seconds. |
| `command-spam.action` | `BLOCK` | `KICK` disconnects (`commandcontrol.spam-kick`), `BLOCK` cancels the command and warns (`commandcontrol.spam-blocked`), `WARN` lets it run and nudges (`commandcontrol.spam-warn`). |
| `plugin-channel-hide.enabled` | `false` | Strips the plugin-messaging channels the server advertises to a client down to the allowed ones, so a fingerprinting mod sees only what you permit. Applied from join onward; it needs a restart to turn on or off. |
| `plugin-channel-hide.allowed-channels` | `[ "minecraft:brand", "bungeecord:main", "velocity:main" ]` | The channels a client is still told about. |
| `worlds.<world>` | empty | A world with its own `mode`, `commands` and `plugin-hide`; whatever it leaves out comes from the base. A player's command list is rebuilt when they change worlds. |
{/* /generated */}

## Writing the lists

List a command without its leading slash. It is matched in any case, bare and under any namespace. An entry can
name arguments too, and then covers only those:

| Entry | Covers |
|---|---|
| `"gamemode"` | Every `/gamemode` |
| `"gamemode creative"` | `/gamemode creative` and anything typed after it, but not `/gamemode survival` |
| `"warp * set"` | `*` stands for any one argument: `/warp shop set`, `/warp spawn set` |
| `"!gamemode spectator"` | `!` takes a command back out of the list. With `"gamemode"` in a whitelist, every game mode but spectator; with `"gamemode"` in a blacklist, spectator alone stays allowed |
| `"plugin:uxmCosmetics"` | Every command that plugin registered, its aliases and namespaced forms included. The name is the plugin's own, as `/plugins` shows it. `"!plugin:<name>"` takes them all back out |

When several entries cover a command, the one naming the most arguments wins, and an exception wins a tie. The
command list a client is sent and the suggestions follow the same entries, so a player is offered
`/gamemode creative` and not `/gamemode spectator`. A `plugin:` entry follows the plugin: a plugin enabled or
disabled after the start is picked up without a reload.

```hocon
mode = whitelist
commands {
  default = [ "spawn", "msg", "plugin:uxmCosmetics", "gamemode creative" ]
  vip     = ${commands.default} [ "fly", "hat" ]
}
```

A group list can start from another with `${commands.default}`, as `vip` does above.

<Callout type="warning" title="A blacklisted argument stops only its own spelling">

An argument is matched as it is typed, with quotes taken off, not as the command reads it. A plugin that also
accepts `/gamemode c` or `/gamemode 1` is not stopped by a blacklisted `"gamemode creative"`. To keep a sensitive
argument shut, use a whitelist, which allows only the spellings it names.

</Callout>

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_commandcontrol_allowed_<command>%` | Whether the player may run that command where they stand (yes/no), answered from the rules the gate uses. |
{/* /generated */}

## Notes

- **The module ships inert.** The bundled config is a blacklist with empty lists, so nothing of yours is blocked
  until you name it.
- **`/uxmess reload` applies the lists at once.** Every online player's command list is sent again, so the tab list
  matches the new rules without a rejoin. Only `plugin-channel-hide` waits for a restart.
- **A command word can use any letters.** `/yardım`, `/règles` and `/правила` run and complete like any other word.
  The base label is lowercased with the root locale, so `/YARDIM` reaches `yardim`, which is why the plain spelling is
  worth keeping in `tab-completion.hidden`.
- **Blocking the namespace bypass matters.** Without it `/minecraft:gamemode` reaches the same handler under a
  different root and slips past a rule written for `/gamemode`.
- **Plugin-hide is on by default** and both hides and refuses `/plugins`, `/help`, `/ver` and friends for anyone
  without the view permission, which stops a player fingerprinting your server.
- **The gate runs on each backend.** A Bedrock player is gated like a Java one, and a command a proxy forwards is
  gated on arrival, so on a network you configure the module on every backend.
- **Operators are limited by the Security module**, not here: see
  [Security](security.md#allowed-operators) for the list of names allowed to hold operator.

Related: [Player State](playerstate.md), [Security](security.md), [Server Tweaks](servertweaks.md)
