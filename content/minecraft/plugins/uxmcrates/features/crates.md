---
title: Crates
order: 401
description: What a crate is, its two shapes, placing it, the link tool, the hologram, the idle effect, worlds and protection.
icon: package
---

A crate is a file under `crates/`, and its file name is its id. It names its key, its cost and requirements, where it
stands, how it is revealed, who hears about a win, and its reward table. Every key is listed on
[Crate files](../config/crate-files.md); this page explains how the parts behave.

## Two shapes

| `shape.type` | What it is |
|---|---|
| `block` | A block of the `material` you name, at every line of `positions` (`world,x,y,z`). One crate may stand in ten places. Any block works: a head, a barrel, a beacon, a lodestone. A container is never opened as one |
| `menu` | Nothing in the world. It opens from the [crate list](../menus/crates.md) and `/crates open <crate>`. `material` is the icon in the list |

Both shapes open from `/crates` and `/crates open`.

## Placing a crate

| Way | What happens |
|---|---|
| `/uxmcrates place <crate>` | The block you look at becomes the crate. A line is added to `positions` in the crate file, and the file's comments are kept |
| `/uxmcrates remove` | The crate on the block you look at is taken off and its line removed |
| The link tool | See below |

Placing needs `uxmcrates.admin.place` and removing `uxmcrates.admin.remove`. Inside another player's claim placing is
refused unless `claims.place-inside-others-claims` is on.

## The link tool

`/uxmcrates tool <crate>` puts a glowing blaze rod in your hand, named for that crate. Right click a block to put the
crate on it; left click a block to take a crate off. It does what `place` and `remove` do, without aiming a command
at the right block, and needs the same two nodes. The crate is carried in the item's own data, never in its name, so
renaming it on an anvil changes nothing. The item is set under `link-tool.item` in
[config.conf](../config/config-conf.md#link-tool), and `<crate>` in its name or lore is the crate's name in the
reader's language.

## Using a placed crate

| Gesture | Does |
|---|---|
| Right click | Opens the crate |
| Left click | Opens the [preview](../menus/preview.md) |
| Breaking | Cancelled: a crate block cannot be broken, by anyone |
| An explosion | Skips the crate block |

A refused opening at a placed crate fires `pushback`: the player is pushed back with `pushback.velocity` (0.4) and the
`pushback.actions` run, a bass note by default. Set the velocity to `0` for no push.

## The hologram

Lines float over a block crate, drawn per viewer with packets: no entity is spawned and no hologram plugin is used.

| Key | Default | Meaning |
|---|---|---|
| `lines` | the crate's name and a hint | Lines per language. `%keys%` is how many keys the reader holds, `%cooldown%` how long they wait |
| `height` | `1.6` | Blocks above the block |
| `view-distance` | `32` | How far a player may stand and still see them |
| `refresh-millis` | `500` | How often the text is worked out again for each viewer |
| `appearance` | | How the text is drawn; see [Crate files](../config/crate-files.md#hologram) |

## The idle effect

`idle-effect = "orbit"` turns an animation file around the block while nobody is opening it, the same packets and the
same files a reveal uses. A crate looks for a viewer once every `idle.look-for-viewers-ms` (1000) and draws nothing
until a player is within `idle.view-distance` (24) blocks. Two hundred crates with nobody near cost two hundred
distance checks a second. Leave the key out for no effect.

## Cooldown

`cooldown` is in seconds. `0` is no wait, and `-1` opens once per player, forever. `uxmcrates.cooldown.bypass` (op)
waits for neither. `/uxmcrates reset cooldown <player> <crate>` clears one player's wait.

## Worlds

```hocon
worlds { mode = "blacklist", list = ["world_nether"] }
```

`blacklist` works everywhere except the list, `whitelist` only in the list. An empty list means everywhere in either
mode. A crate opened in a world it does not allow answers `open.wrong-world`.

## Permission, requirements and cost

| Key | Charges | Example |
|---|---|---|
| `permission` | nothing | `"uxmcrates.crate.vip"`: a node needed beside the key, never instead of it |
| `requirements` | nothing | `"money vault >= 500"`, `"item DIAMOND >= 3"`, `"placeholder %mcmmo_level_mining% >= 50"` |
| `cost` | each line | `"[take-money] vault 500"`, `"[take-money] exp 250"`, `"[take-item] DIAMOND 3"` |

The whole cost is checked before any of it is taken, so a player short of the second line keeps what the first would
have taken. A crate with `key.amount = 0` is paid by its cost alone. A crate that names an economy the server does
not run is not loaded, and the console names the file. See [Economies](../integrations/economies.md).

## Broadcasts

`broadcast { enabled = true, threshold = 3 }` announces a win whose rarity `order` is at or above the threshold, as
`crate.broadcast`, to every player in their own language. A reward with `announce = true` is announced whatever the
threshold says.

<Callout type="warning" title="Nobody reads a broadcast by default">

A player holding `uxmcrates.broadcast.hide` does not read win broadcasts, and that node defaults to `true`, so every
player holds it. To show broadcasts, negate the node for your players, for example
`lp group default permission set uxmcrates.broadcast.hide false`.

</Callout>

## Effects

`effects.open` fires when an opening is paid for; `effects.win` fires once the reward is handed over, once per
opening. A bulk run fires `win` once, for its rarest win. A win line may name `%reward%`, `%rarity%`, `%crate%` and
`%player%`, in the player's language. A rarity's own `effects` fire after the crate's when the best reward of an
opening has that rarity, so an epic win can be louder than a common one. The line grammar is on
[Effect lines](../config/config-conf.md#effect-lines).

Related: [Crate files](../config/crate-files.md), [Rewards and groups](rewards.md), [Opening](opening.md)
