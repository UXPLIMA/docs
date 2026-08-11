---
title: Command Control
order: 1340
description: 'Command Control decides which commands each player may run, and hides the
  rest. A denied command is cancelled before it reaches its handler and the player is
  shown a custom deny line, so a hidden command can be made to read as though it never
  existed. On top of the gate it filters tab-completion, hides the plugin-listing and
  help commands from prying players, and closes the /minecraft:gamemode namespace
  escape. It is the PlHidePro and CommandWhitelist feature set, built in with no
  separate plugin.'
---

Command Control is its own **`commandcontrol`** module. It persists nothing: the rule
set and the hide policy are derived straight from config, so it owns no database table
and no migrations.

The module ships **enabled**, with the command gate inert and the plugin-hide already
working. The bundled config is a *blacklist with empty lists*, so no command of yours is
blocked until you name one, and an operator who leaves the lists blank pays nothing (the
listener short-circuits before any per-command work). **Plugin-hide is on out of the
box**, so `/plugins`, `/pl` and `/help` are hidden from anyone without
`uxmessentials.commandcontrol.viewplugins` from the first start. Turn the whole feature
off with `enabled = false` in `modules/commandcontrol/config.conf`.

---

## The Whitelist / Blacklist Model

One `mode` chooses how every command list is read, and it is the same for every group:

| `mode` | How the lists read | Use it to |
|--------|--------------------|-----------|
| `blacklist` *(default)* | The listed commands are **denied**; everything else is allowed. | Hide a handful of commands: name only the few to block. |
| `whitelist` | Only the listed commands are **allowed**; everything else is denied. | Lock a group down: name the full list of what it *may* run. |

<Callout type="warning" title="An empty whitelist denies everything">

Because a whitelist allows only what is listed, an *empty* whitelist denies every
command: a deliberate, total lock-down. Populate the list before you flip `mode` to
`whitelist`. (A blank *blacklist*, by contrast, blocks nothing, which is why the
module ships that way.)

</Callout>

### Per-group lists

The command lists live under `commands`, one list per permission group, plus a
`default` fallback:

```hocon
commands {
  default = [ "op", "stop", "reload" ]
  admin   = [ ]
}
```

- A player's **primary permission group** selects its own list. Group resolution reads
  LuckPerms when it is installed; the read is non-blocking (it uses the same cached
  snapshot the platform resolves permission checks against).
- A player whose group has **no list here**, or who is in no group, and *every* player
  when LuckPerms is not installed: falls back to the **`default`** list.
- List a command by its **root label without a leading slash** (`gamemode`, not
  `/gamemode op`). The match is on the root only and is **case-insensitive**, so `/Home`,
  `home`, and a config entry of `"/HOME"` all compare equal.

In the example above (a blacklist), everyone in the `default` list is blocked from
`/op`, `/stop`, and `/reload`, while a player in the `admin` group has an empty list and
so is blocked from nothing.

### Which deny line shows

When a command is blocked, `use-unknown-command-message` picks the line the player sees:

| `use-unknown-command-message` | Line shown | Message key |
|-------------------------------|------------|-------------|
| `true` *(default)* | The vanilla-style *"Unknown command."*: the command reads as though it doesn't exist. | `commandcontrol.unknown-command` |
| `false` | An honest *"You don't have permission to use that command."* | `commandcontrol.no-permission` |

Both lines live in the [message catalog](../config/messages.md)
(`messages_<lang>.conf`), so they are per-locale: edit them there.

<Callout type="note" title="The console is never gated">

The gate listens only to the player command event, so console and command-block
commands run untouched. It also runs at a high event priority, stopping the dispatch
before the vanilla handler while still letting cooperating plugins at normal priority
observe the event.

</Callout>

### Worked example: locking players to a short list

Say you want regular players to reach only a survival-essentials set, while staff keep
everything. Set a whitelist and give the `default` group the allowed roots:

```hocon
mode = whitelist

commands {
  default = [ "spawn", "home", "sethome", "warp", "tpa", "msg", "r", "help", "balance", "pay" ]
  staff   = [ ]        # ignored — staff hold the .bypass node, see below
}
```

Now:

- A **regular player** (group `default`) may run only the ten listed commands; anything
  else (`/gamemode`, `/give`, an unknown plugin command) is cancelled and reads as
  *"Unknown command."*
- **Staff** who hold `uxmessentials.commandcontrol.bypass` are never gated at all, so
  their `staff` list above is moot; the `.bypass` node is the clean way to exempt a rank
  rather than whitelisting every command they own.

---

## Tab-Completion Filter

```hocon
tab-completion { enabled = true }
```

With the filter on, every command a player may not run is removed from the command list
the server sends the client, so a disallowed command **neither autocompletes nor appears
in the client command graph**. A command's own vanilla permission is respected too: a
command the player couldn't see anyway stays hidden. If the player starts typing a
filtered command's arguments, its **argument suggestions are dropped** as well, so the
arguments of a command they can't run never leak through completion.

The filter is a **no-op while the rule set is inert** (a blacklist with empty lists), so
it is safe to leave on. It ships on.

---

## Plugin-Hide

```hocon
plugin-hide {
  enabled            = true
  hidden-commands    = [ "plugins", "pl", "?", "help", "ver", "version", "about", "icanhasbukkit" ]
  deny-list-commands = true
}
```

Plugin-hide keeps the plugin-listing and help commands away from players who shouldn't
be able to fingerprint your server. It ships **on**, and with `deny-list-commands` on
too, so those commands are both hidden and refused for anyone without the view
permission. Set `enabled = false` to restore the vanilla behaviour.

| Key | What it does |
|-----|--------------|
| `hidden-commands` | The command roots to hide from a player who lacks the view permission. The `bukkit:` / `minecraft:` / `paper:` namespaced forms are matched automatically, so `/bukkit:pl` is hidden by the `pl` entry; you never list every namespace. |
| `deny-list-commands` | Also **blocks** those commands from executing for a player who may not see them, so a `/help` or `/plugins` can't leak plugin names through its *output* (not just its tab entry). The deny line is `commandcontrol.plugin-hidden`, which mirrors the vanilla "unknown command" wording. |

Who sees the hidden commands is decided by **`uxmessentials.commandcontrol.viewplugins`**:
a holder sees and runs them normally; everyone else has them scrubbed from the sent
command list, from tab completion, and (when `deny-list-commands` is on) blocked from
running.

<Callout type="info" title="Hide the list, or hide the whole command">

Set `deny-list-commands = false` and the commands still *run* for everyone; they are
only removed from the client's command graph and completion, so a player who already
knows to type `/plugins` still gets its output. It ships `true`, so the output itself
is withheld as well and the command reads as nonexistent.

</Callout>

<Callout type="warning" title="`/help` is hidden by default">

`help` is in the shipped `hidden-commands` list, so ordinary players cannot use
`/help` on a fresh install. If you want it back, drop `"help"` (and `"?"`) from the
list, or grant `uxmessentials.commandcontrol.viewplugins` to everyone.

</Callout>

---

## Namespace-Bypass Block

```hocon
block-namespace-bypass = true
```

A player could otherwise dodge the lists by prefixing a command with its namespace:
`/minecraft:gamemode`, `/bukkit:pl`, `/someplugin:cmd` all reach the same handler as the
bare form but read as a *different* root, which the rule set (matching on the raw root)
would let through. With the block on, the `namespace:command` form of any command whose
**bare form is denied** is blocked too, so `/minecraft:gamemode` is treated exactly like
`/gamemode`.

Holders of `uxmessentials.commandcontrol.bypass` may still use namespaced forms: the
bypass short-circuits the whole gate. Leave the block on unless a plugin of yours
legitimately relies on namespaced command input. The plugin-hide half of the namespace
escape needs no separate switch: the hide already folds the `namespace:` prefix, so
`/bukkit:pl` is caught directly.

---

## Auto-Lowercase

```hocon
auto-lowercase-base-commands = true
```

`/GameMode` and `/gamemode` are the same command to the server but different strings to a
rule set that matches on the raw root. With this on, the base command is lower-cased
before the lists are consulted, so case tricks cannot slip past a blacklist. The
arguments are untouched, so a case-sensitive argument still works.

---

## Command-Spam Protection

Rate-limits how many commands one player may fire inside a sliding window. It ships
**off**.

```hocon
command-spam {
  enabled        = false
  max-per-window = 40      # commands allowed inside the window
  window-seconds = 2       # the window width
  action         = BLOCK   # KICK | BLOCK | WARN
}
```

| `action` | What happens to a player over the limit |
|---|---|
| `BLOCK` (default) | Only the offending command is cancelled, and they are warned |
| `WARN` | The command still runs, but they are nudged |
| `KICK` | They are disconnected |

Holders of `uxmessentials.commandcontrol.spam.bypass` (default `op`) are never counted at
all. The three player-facing lines live in the message catalog.

---

## Plugin-Channel Hider

Client-side mods fingerprint a server by the **plugin-messaging channels** it registers
with the client, which is a way of listing your plugins that command-hiding does not
touch. With this on, the advertised channel list is stripped to what you allow. It ships
**off**.

```hocon
plugin-channel-hide {
  enabled          = false
  allowed-channels = [ "minecraft:brand", "bungeecord:main", "velocity:main" ]
}
```

Holders of `uxmessentials.commandcontrol.channelhide.bypass` (default `op`) keep the full
list.

<Callout type="warning" title="It covers join onward, not the login phase">

The strip is applied by a packet interceptor spliced into the connection when the
player **joins**, so a registration burst emitted during the earlier login and
configuration phase is not intercepted. The interceptor also fails safe: a channel it
cannot rewrite is left alone rather than risking the connection. Treat this as raising
the cost of fingerprinting, not as a guarantee.

Leave `bungeecord:main` / `velocity:main` in the allow list on a network, or proxy
messaging breaks.

</Callout>

---

## Per-World Overrides

A world can run its own lists instead of the base ones:

```hocon
worlds {
  creative {
    mode = blacklist
    commands { default = [ ] }
  }
}
```

Each world block may set its own `mode`, `commands` (a `default` plus per-group lists,
exactly like the top-level block) and `plugin-hide`. Anything it omits **inherits the base
config**, and a world with no entry uses the base config entirely.

A player's visible command list is recomputed when they **change worlds**, so the tab list
always matches the world they are standing in. The example above is the common shape:
block `/fly` and `/gamemode` everywhere in the base config, then let them through in the
creative world.

---

## Bedrock (Geyser) and Cross-Server

The gate and every filter are **client-agnostic**. A Bedrock player routed through
Geyser/Floodgate arrives as an ordinary player, so they are gated, tab-filtered, and
plugin-hidden exactly like a Java client, with **no Geyser dependency** and no client
probe.

On a network the gate runs **per backend**: a command a proxy forwards to a server is
gated on arrival *there*, not at the proxy. So you configure command-control on **each
backend server** you want it to cover, rather than once at the proxy.

---

## The `config.conf`

The complete `modules/commandcontrol/config.conf`, with the shipped defaults:

```hocon
# Command Control — decide which commands each player may run, and hide the rest. A denied command is cancelled
# before it runs and the player is shown the deny message below, so a hidden command can be made to look as though it
# does not exist.
#
# The module ships ENABLED but inert: the default mode is a blacklist with empty lists, so nothing is blocked until
# you name commands. Turn the whole feature off with enabled = false.
enabled = true

# How the command lists below are read:
#   blacklist = the listed commands are DENIED, everything else is allowed (name the few commands to hide).
#   whitelist = only the listed commands are ALLOWED, everything else is denied (a lock-down; name the full list a
#               group may run). An empty whitelist denies every command, so populate it before switching.
mode = blacklist

# Which deny message to show when a command is blocked. true shows the vanilla-style "unknown command" line, so a
# hidden command reads as though it does not exist; false shows an honest "you don't have permission" line. The text
# of both lines lives in the message catalog (messages_<lang>.conf, keys commandcontrol.unknown-command and
# commandcontrol.no-permission) — edit it there.
use-unknown-command-message = true

# The command lists, per permission group. A player's primary permission group (from LuckPerms, when installed)
# selects its own list; a player whose group has no list here, or who is in no group, falls back to "default". List a
# command by its root label without a leading slash (e.g. "gamemode", not "/gamemode op"); the label is matched
# case-insensitively.
#
# Example — a blacklist that hides a couple of admin commands from everyone, and hides fewer from staff:
#   commands {
#     default = [ "op", "stop", "reload" ]
#     admin   = [ ]
#   }
commands {
  default = [ ]
}

# Tab-completion filter. When enabled, every command a player may not run (per the lists above and the permission the
# command itself carries) is removed from the command list the server sends the client, so a disallowed command neither
# autocompletes nor shows up when the client asks for suggestions. This applies to Bedrock/Geyser players too. It is a
# no-op while the lists above are empty, so it is safe to leave on.
tab-completion {
  enabled = true
}

# Plugin-hide. Hides the plugin-listing and help commands (/plugins, /pl, /help, ...) from players who lack the view
# permission uxmessentials.commandcontrol.viewplugins, so they cannot discover which plugins the server runs. It ships
# ON: the view permission defaults to op, so operators keep seeing and running these commands while non-ops do not.
# Turn it off with enabled = false. deny-list-commands ships ON too, so a hidden command is both invisible and
# unrunnable for a non-op (with only enabled = true, a non-op could still type /plugins and read its output).
#   hidden-commands     : the command roots to hide. The bukkit:/minecraft:/paper: namespaced forms are matched too, so
#                         "/bukkit:pl" is hidden by the "pl" entry — no need to list every namespace.
#   deny-list-commands  : also block those commands from executing for a player who may not see them, so a /help or
#                         /plugins cannot leak plugin names through its output (not just its tab entry). The deny line
#                         is the message catalog key commandcontrol.plugin-hidden.
plugin-hide {
  enabled = true
  hidden-commands = [ "plugins", "pl", "?", "help", "ver", "version", "about", "icanhasbukkit" ]
  deny-list-commands = true
}

# Namespace-bypass block. A player can otherwise dodge the lists above by prefixing a command with its namespace —
# "/minecraft:gamemode", "/bukkit:pl", "/somePlugin:cmd" all reach the same command as the bare form but read as a
# different label. With this on, the "namespace:command" form of any command whose bare form is denied is blocked too,
# so "/minecraft:gamemode" is treated exactly like "/gamemode". Holders of uxmessentials.commandcontrol.bypass may still
# use namespaced forms. Leave it on unless a plugin of yours legitimately relies on namespaced command input.
block-namespace-bypass = true

# Auto-lowercase the base command label. When on, the leading command word is lowercased before every check and before
# the command runs, so "/GAMEMODE creative" is treated and executed exactly as "/gamemode creative". Only the base label
# is touched - arguments (a target player, a world name, a message body) keep their casing. Turn it off only if a plugin
# of yours registers a genuinely case-sensitive command label.
auto-lowercase-base-commands = true

# Command-spam protection. Rate-limits how many commands a player may send inside a sliding window and, on a breach,
# applies the action below. Holders of uxmessentials.commandcontrol.spam.bypass are never counted (default op).
#   enabled        : master switch for the guard (ships OFF).
#   max-per-window : the greatest number of commands allowed inside the window.
#   window-seconds : the width of the sliding window, in seconds.
#   action         : what to do to a player who exceeds the limit -
#                      KICK  = disconnect them with the commandcontrol.spam-kick message;
#                      BLOCK = cancel only the offending command and warn (commandcontrol.spam-blocked) - the default;
#                      WARN  = let the command run but still nudge them (commandcontrol.spam-warn).
# The three player-facing lines live in the message catalog (messages_<lang>.conf) - edit them there, not here.
command-spam {
  enabled = false
  max-per-window = 40
  window-seconds = 2
  action = BLOCK
}

# Plugin-channel hider. Client-side mods fingerprint which plugins a server runs by the plugin-messaging channels the
# server registers to the client (the minecraft:register / minecraft:unregister list). With this on, the channel list
# sent to a client is stripped down to the allowed channels below, so a fingerprinting mod sees only what you permit.
# Holders of uxmessentials.commandcontrol.channelhide.bypass keep the full list (default op).
#
# NOTE ON REACH: the strip is applied by a packet interceptor spliced into a player's connection when they join, so it
# filters the channel advertisements the server sends from join onward. A registration burst emitted during the earlier
# login/configuration phase (before the join) is not intercepted; the strip is also best-effort against the running
# server's exact packet shape and fails safe (a channel it cannot rewrite is left as-is) rather than ever breaking a
# connection. Ships OFF.
plugin-channel-hide {
  enabled = false
  allowed-channels = [ "minecraft:brand", "bungeecord:main", "velocity:main" ]
}

# Per-world overrides. A "worlds" block lets a named world use its own command lists and hide behaviour instead of the
# base ones above; a world with no entry here uses the base config. Each world block may set its own "mode", "commands"
# (default + per-group lists, exactly like the top-level block) and "plugin-hide"; anything it omits inherits the base.
# A player's visible command list is recomputed when they change worlds, so the tab list matches the world they are in.
# Example - allow /fly and /gamemode only in a creative world, while the base config blocks them everywhere else:
#   worlds {
#     creative {
#       mode = blacklist
#       commands { default = [ ] }
#     }
#   }
worlds {
}

# Client and network reach. The gate and the tab/command-list filter are client-agnostic: a Bedrock/Geyser player
# (routed through Floodgate as an ordinary player) is gated and filtered exactly like a Java client, with no Geyser
# dependency. The gate also runs per backend — a command a proxy forwards to this server is gated on arrival here, so on
# a network you configure command-control on each backend rather than at the proxy.
#
# Holders of uxmessentials.commandcontrol.bypass are never gated — every command runs and is visible regardless of the
# lists above. Holders of uxmessentials.commandcontrol.viewplugins see the plugin-hide commands. Both default to op.
```

---

## Permissions

| Node | Default | What it grants |
|------|---------|----------------|
| `uxmessentials.commandcontrol.bypass` | `op` | Exempt from the whitelist/blacklist gate **and** the tab-completion / plugin-hide scrub: every command runs and is visible, namespaced forms included |
| `uxmessentials.commandcontrol.channelhide.bypass` | `op` | Exempt from the plugin-channel hider: the full channel list is sent to this player |
| `uxmessentials.commandcontrol.spam.bypass` | `op` | Exempt from the command-spam rate limiter: commands are never counted and no action fires |
| `uxmessentials.commandcontrol.viewplugins` | `op` | See the plugin-listing / help commands (`/plugins`, `/pl`, `/help`, ...) hidden by plugin-hide |
| `uxmessentials.module.commandcontrol` | `op` | Reload / inspect the module (`/uxmess reload commandcontrol`) |

Both `.bypass` and `.viewplugins` default to `op`, so a fresh install gates and hides
nobody who is an operator; you grant the two nodes to the staff ranks that should keep
seeing everything, then name the commands (or switch the hide on) that everyone else
loses.

---

## Next Steps

- [🔑 Permission Reference](../permissions/reference.md): the full `uxmessentials.commandcontrol.*` node list
- [🧩 Per-Module Config](../config/per-module.md): where `modules/commandcontrol/config.conf` lives and how it's loaded
- [💬 Messages & Languages](../config/messages.md): the deny lines (`commandcontrol.unknown-command`, `.no-permission`, `.plugin-hidden`)
- [🔌 LuckPerms](../integrations/luckperms.md): how per-group command lists resolve a player's primary group
- [🌐 Cross-Server: Velocity & Redis](../cross-server/overview.md): why command-control is configured per backend
