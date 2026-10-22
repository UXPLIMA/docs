---
title: Core Concepts
order: 920
description: Modules, the config layout, permissions, messages and threading.
icon: lightbulb
---

Five ideas carry through the rest of the documentation.

## Modules

The plugin is 35 modules: teleport, homes, warps, economy, kits, moderation, holograms, worlds and the
rest. Each is self-contained, and a disabled module wires nothing: no commands, no listeners, no tables, no
tasks, no state.

A module is switched by the `enabled` key at the top of its own config file. There is no central
switchboard, and disabling a module never deletes its data.

```
/uxmess status              # which modules are on
/uxmess reload <module>     # re-read one module's files
/uxmess reload              # re-read everything
```

A reload re-reads the config tree and the message catalogs off the main thread, then runs each module's own
re-read step and reports what happened.

| Line | Meaning |
|---|---|
| `[OK]` | Re-read, and the new values are live |
| `[RESTART]` | That module wires itself at startup, so the edit waits for a restart |
| `[FAIL]` | The file could not be read, usually a HOCON syntax error; the old values stay in force |

Changing what is wired (turning a module or a mechanic on, adding a command) needs a restart. Message edits
and anything reported `[OK]` apply at once.

## Config layout

| Path | What lives there |
|---|---|
| `config.conf` | Globals: storage, default language, cross-server, update checks |
| `modules/<module>/config.conf` | One file per module, with `enabled` as the first key |
| `messages/messages_<lang>.conf` | Player-facing text, per language |
| `commands.conf` | Rename, re-alias or disable any command |
| `menus/*.conf` | Your own menus |

Keys are kebab-case. An update appends the keys it added, with their comments, and leaves your values alone;
a key you deleted is not put back. Delete a key to reset it, delete a file to regenerate it. Config is
loaded once and swapped in one go on reload. See [Configuration](../config/overview.md).

## Permissions

Every node lives under `uxmessentials.`. Self-service verbs default to on, so `/home`, `/balance`, `/msg`,
`/kit` and `/warp` work on a fresh install; verbs that touch other players or the server default to op.

Quotas and tiers are numbered nodes rather than config lists:

| Node | Meaning |
|---|---|
| `uxmessentials.home.limit.<n>` | How many homes a player may set |
| `uxmessentials.vault.amount.<n>` | How many vaults a player may own |
| `uxmessentials.tp.cooldown.<seconds>` | Teleport cooldown for that group |
| `uxmessentials.kit.cooldown.<seconds>` | Kit cooldown for that group |

Granting `uxmessentials.home.limit.5` allows five homes. The highest node held wins. A permission plugin is
optional; with LuckPerms installed the same quotas can also come from LuckPerms meta. See the
[Permission Reference](../permissions/reference.md).

## Messages

Every player-facing string is a catalog entry written in MiniMessage, never legacy `&` codes. Catalogs are
per language in `messages/messages_<lang>.conf`; twelve languages ship with the plugin.

```
/lang            # your current language
/lang tr         # switch
/lang reset      # back to the default
```

The chain is your `/lang` choice, then your client locale, then the server default, then English. See
[Messages](../config/messages.md).

## Running commands from the console

Administration commands work from the server console, from a command block and from a remote console the same
way they work in game. Anything that would have needed your position takes it as arguments instead, for example
`/setjail main at world 120 64 -40`, and the same shape applies to spawns, warps, worlds, holograms and NPCs.
Anything that would have acted on you takes the player instead, for example `/heal Notch` or `/gamemode creative
Notch`. Replies are written to the console, and what the console does is attributed to the console in audit
lines and broadcasts.

## Threading

Work is scheduled through Folia-safe schedulers, so the plugin runs on both Paper and Folia. Anything slow
(the `/rtp` location search, a `/baltop` query, every database call) runs off the main thread and hands the
tick loop back quickly. Storage is embedded SQLite unless you point it elsewhere; see
[Database](../database/sqlite.md).
