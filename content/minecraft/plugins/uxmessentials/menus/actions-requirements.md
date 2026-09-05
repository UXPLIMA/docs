---
title: Actions & Requirements
order: 1100
description: What a click does, and what has to be true before it draws.
icon: play
---

Everything a menu does is an action, and everything it checks is a requirement. Both are written the same
way, a bare `id:value` reference, and both are shared by the built-in menus and your own.

The same vocabulary drives [custom commands](../modules/customcommands.md): a command file's `actions` and
`requirements` lists take exactly the references on this page, so anything a button can do a command can do. One
token is specific to a command file: `delay:<duration>` (for example `delay:2s`) shifts every step after it, which
is how a chain waits without a per-action `delay` modifier.

<Callout type="warning" title="Bare references, never brackets">

A reference is `id:value`: `message:hi`, `command:spawn`, `has-money:500`, `perm:vip`,
`open:shop`. There is no bracketed `[message]` form. An action or condition that takes
no value is just its id: `close`, `back`, `has-next`. A leading `!` on a **condition**
inverts it: `!perm:uxmessentials.vip`.

</Callout>

## Actions

Actions run in order. Any action reference can also carry per-action modifiers: a
`delay` in ticks, a `chance` percent, and a fallback to run if the chance roll fails:
when written as a block instead of a bare string.

The table below is the **common set**; the engine ships around sixty action ids in total
(plus aliases), and modules and the [Menu API](../developer/menu-api.md) can register
more. Use `/menu dump <menu>` to see exactly what a menu resolves.

| Action | Does |
|---|---|
| `close` | Close the menu. |
| `back` | Return to the previous menu. |
| `open:<menu>` | Open another menu by name. |
| `refresh` | Re-render the whole menu. |
| `refresh-slot:<slot>` | Re-render one slot. |
| `reset-pagination` | Jump back to page one. |
| `message:<text>` | Send the clicker a chat message (MiniMessage). |
| `broadcast:<text>` | Announce to everyone. |
| `action-bar:<text>` | Show an action-bar line. |
| `title:<text>` | Show a title. |
| `toast:<text>` | Pop an advancement-style toast. |
| `sound:<key> [volume] [pitch]` | Play a sound to the clicker. |
| `broadcast-sound:<key> [volume] [pitch]` | Play a sound to everyone, each at their own position. |
| `rawsound:<key> [volume] [pitch]` | Play a key to the clicker exactly as written, for a resource-pack sound. |
| `command:<cmd>` | Run a command **as the player**. |
| `console:<cmd>` | Run a command **as the console**. |
| `command-as-op:<cmd>` | Run a command with a temporary op. |
| `command-random:<a;b;…>` | Run one of several commands at random. |
| `give-money:<amt>` · `take-money:<amt>` · `set-money:<amt>` | Adjust the player's balance. |
| `give-points:<amt>` · `take-points:<amt>` | Adjust PlayerPoints. |
| `give-exp:<amt>` · `take-exp:<amt>` · `give-levels:<n>` | Adjust experience / levels. |
| `give-permission:<node>` | Grant a permission (via Vault). |
| `give-item:<item>` · `take-item:<item>` · `set-item:<item>` | Give / take / set an item (accepts a `b64:` item string). |
| `data-set:` · `data-add:` · `data-sub:` · `data-remove:` | Read/write a per-player data value. |
| `meta-set:` · `meta-add:` · `meta-remove:` | Read/write a per-player metadata value. |
| `set-cooldown:<key>` | Stamp a cooldown the `cooldown:` condition can read. |
| `teleport:<x,y,z[,world]>` | Teleport the player. |
| `connect:<server>` | Send the player to another server (proxy). |
| `list-sort:<source>[:next\|prev\|reset]` | Cycle a paged list's sort order. |
| `list-filter:<source>:<key>=<value>` | Set (or with an empty value, clear) a paged list's filter. |
| `list-search:<source>:<key>` | Prompt for text and store it as a paged list's filter. |
| `input:<key>` | Ask the player to type something, then continue the chain with it. |
| `confirm:<key>` | Ask for a yes/no before the rest of the chain runs. |

**Sound keys take either spelling.** `sound:block.note_block.pling` and
`sound:BLOCK_NOTE_BLOCK_PLING` name the same sound; the constant form is resolved
through the server's sound registry rather than by rewriting the underscores, so a
key with an underscore inside a segment works as well as one without. A name the
registry does not know is passed to the client as written, which is how a
resource-pack key reaches it. Volume and pitch are optional and default to `1`.
`rawsound:` skips the registry entirely and is the one to use when you want a key
sent verbatim.

**`input:` and `confirm:` are top-level steps only.** They pause the chain and resume on a later callback, so they only work as a direct
step of a gesture. Put one inside an `else`, a `deny` or a per-requirement list and
there is nothing for it to resume: it is skipped with a console warning. The three
`list-*` actions and the prompt modes are covered on the
[engine page](engine.md).

## Requirements

A requirement decides whether a click's actions run, or whether a tile is even visible.
Every requirement is a condition reference; a leading `!` inverts it.

| Requirement | Passes when |
|---|---|
| `perm:<node>` | The player has the permission. |
| `has-money:<amt>` | Balance is at least the amount. |
| `has-points:<amt>` | PlayerPoints balance is at least the amount. |
| `has-exp:<amt>` · `has-level:<n>` | Enough experience / a high enough level. |
| `has-item:<item>` | The player is holding / carrying the item. |
| `has-empty-slots:<n>` | At least N free inventory slots. |
| `has-meta:<key>` | A metadata value is present / matches. |
| `has-prev` · `has-next` · `on-page:<n>` | Pagination state (used on page-control tiles). |
| `cooldown:<key>` | A named cooldown has elapsed. |
| `expr:<expression>` | A boolean expression (with placeholders) is true. |
| `compare:` · `papi-compare:` | A numeric / placeholder comparison holds. |
| `equals-ignorecase:<a;b>` | Two strings match, ignoring case. |
| `contains:<a;b>` | One string contains the other. |
| `regex:` · `length:` · `is-integer:` | String-shape checks. |
| `world:<name>` | The player is in the named world. |
| `is-near:` · `cuboid:` | The player is within a radius / region. |
| `has-group:<group>` · `job:<name>` · `worldguard-region:<id>` · `weather:<kind>` | Integration checks (LuckPerms / JobsReborn / WorldGuard / world weather). |
| `mcmmo-level:<skill>:<n>` · `mcmmo-power:<n>` | The player's mcMMO skill level / power level is at least N. |
| `client-version:<protocol>` · `protocol:<protocol>` | The player's client speaks at least that protocol version (read through ViaVersion). |

**Integration conditions fail closed.**

A condition that needs a plugin you do not run (`job:`, `worldguard-region:`,
`mcmmo-level:`, `has-group:` without LuckPerms) can't be evaluated, so it is treated
as **false**: the tile hides or the click is denied rather than silently passing.
Install the plugin, or don't use the condition.

`client-version:` is the one exception, and deliberately so. It asks about the
player's client, not about the player, and an unknown answer means there is no
translation layer installed at all: every player is then on the server's own version,
so the check passes. Without that exception every menu using it would empty out on
the ordinary server that has no ViaVersion.

**mcMMO levels.** `mcmmo-level:` takes the skill name mcMMO itself uses (`mining`,
`woodcutting`, `axes`, ...) then the level: `mcmmo-level:mining:50` shows the tile only
to players who have reached mining 50. `mcmmo-power:1000` checks the power level, the sum
across every skill. An unknown skill name is false, not an error.

## Per-gesture click blocks

A gesture can be a bare action list, or a block that gates its actions behind
requirements and runs a `deny` list (or an `else` fallback) when the gate fails.

```hocon
click {
  # Simple form: always runs on left-click.
  left = ["command:kit starter", "close"]

  # Gated form on right-click.
  right {
    requirements = ["perm:uxmessentials.vip", "has-money:100"]
    minimum = 2                       # both must pass (see below)
    actions = ["take-money:100", "command:kit vip"]
    deny = ["message:<red>VIPs only, and it costs 100."]
  }

  # 'any' fires alongside whichever gesture the player used.
  any = ["sound:UI_BUTTON_CLICK"]
}
```

### `minimum`: how requirements combine

The `minimum` key turns a requirement list into AND / OR / N-of-M without a separate
flag:

| `minimum` | Meaning |
|---|---|
| omitted or `0` | **All** requirements must pass (AND). |
| `1` | **Any one** may pass (OR). |
| `N` (between) | At least **N of the M** must pass. |

## View requirements

The same block gates a tile's **visibility**. A flat list is an all-must-pass AND; the
block form lets you use `minimum` and inversion.

```hocon
# Flat form: hide unless BOTH hold.
view = ["perm:uxmessentials.vip", "!world:spawn"]

# Block form: show if the player meets ANY one of the three.
view {
  requirements = ["perm:uxmessentials.tier1", "perm:uxmessentials.tier2", "has-money:1000"]
  minimum = 1
}
```

A tile whose `view` fails is not drawn at all; it does not appear greyed-out, it simply
isn't there.

Related: [Custom Menu Engine](engine.md), [Menu API](../developer/menu-api.md), [Permission Reference](../permissions/reference.md), [Bedrock Forms](bedrock.md)
