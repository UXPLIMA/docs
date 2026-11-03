---
title: "Crates, keys and rewards"
order: 20
description: "Every crate, key and reward is a HOCON file whose name is its id."
---

Every crate, key and reward is a HOCON file whose name is its id. Nothing keys off a display name, and a name a
player reads takes one of three shapes: a line of the messages files, `name = "@content.crates.common.name"`,
drawn in each reader's language; a block per language, `name { en = "...", tr = "..." }`; or one text everybody
reads. The shipped files use the first, so a new language is one more `messages_<lang>.conf` and not an edit to
every crate; their words are under `content` at the end of each messages file. The shipped `crates/common.conf`,
`crates/elite.conf`, `keys/*.conf` and `rewards/legendary-sword.conf` are commented examples that work on a
bare server; copy one and edit it.

A line the plugin cannot read is refused and named in the console with its file and key, and the rest of the
file still loads. `/uxmcrates doctor` counts the refused lines.

## A crate

`crates/<id>.conf` holds:

| Key | Meaning |
|---|---|
| `name` | The name per language. |
| `key` | `id` of the key file, `amount` per opening, and `kind`: `virtual`, `physical` or `either`. |
| `cost` | Extra cost per opening as action lines, such as `[take-money] vault 500`. Checked before anything runs. |
| `requirements` | Lines a player must satisfy, such as `money vault >= 500`. |
| `permission` | A node the player needs besides the key. Optional. |
| `shape` | `type = "block"` with a `material` and the placed `positions`, or `type = "menu"` for a crate that opens only from `/crates` and `/crates open <crate>`. |
| `visibility` | Who sees the reveal: `opener`, `radius` (within `radius` blocks) or `everyone`. Overrides `opening.default-visibility`. |
| `animation` | The id of a file under `animations/`. |
| `idle-effect` | An animation turned around a placed crate while nobody opens it. Optional. |
| `cooldown` | Seconds between two openings. `0` is no wait and `-1` opens once, forever. |
| `worlds` | `mode` is `blacklist` or `whitelist` over `list`. An empty list means everywhere. |
| `broadcast` | `enabled`, and `threshold`: the rarity `order` at or above which a win is announced. |
| `effects` | `open` and `win` lists of effect lines. A rarity may add its own `effects`. |
| `hologram` | Lines per language, `height`, `view-distance`, `refresh-millis` and an `appearance` block of text display options. |
| `no-repeat` | Whether one opening may give the same reward twice. |
| `bulk` | `cap` for `/crates massopen` on this crate, and `announce`: `each`, `once` or `never`. |
| `selection` | `enabled`, `picks`, `faces` and `face-down`. The player takes rewards instead of rolling. |
| `rarities` | Named rarities with `name`, `colour`, `order`, an optional default `weight` and `effects`. |
| `groups` | The reward table. |

The effect line grammar is the one `pushback.actions` in `config.conf` lists: verbs such as `[message]`,
`[title]`, `[sound]`, `[particle]`, `[console]` and `[take-money]`, with an optional `{delay=, chance=, repeat=,
every=, if=}` block between the verb and the payload. A cost line takes no block.

Opening a placed crate checks the key, the permission, the requirements, the claim rules, the cooldown and the
world. A refusal fires `pushback.actions` and tells the player why.

## Rewards and groups

A crate draws exactly one reward from each group. One group is a flat weighted list; three groups give one
armour piece, one consumable and one sum of money in a single opening. A reward holds:

| Key | Meaning |
|---|---|
| `weight` | A number, never a percentage. A rarity's `weight` is the default when a reward writes none. |
| `display` | `name`, `rarity`, `lore`, `icon { material, glow }` and `order` on the preview page. |
| `items` | Item lines such as `"DIAMOND_HELMET 1"`, or an item block with `material`, `amount`, `enchants` and more. |
| `commands` | Console lines run when it is won. |
| `money` | Payments such as `"vault 500"` or `"exp 500"`, named like a cost. |
| `announce` | Announce the win whatever the threshold says. |
| `permission` | A node needed to be able to win it. |
| `limits` | `player` and `global`, each with `amount` and an optional `reset` period. |
| `linked` | Rewards granted along with this one. A linked reward is never rolled on its own. |
| `guarantee` | `mode = "once"` or `"every"` with an `opening` count. |
| `luck` | `false` keeps rank luck from raising its weight. |
| `from` | Take the definition from `rewards/<id>.conf`. Whatever the crate writes wins over that file. |

A shared reward in `rewards/` cannot name another shared reward.

A reward that is exhausted, locked behind a permission the player lacks, or only linked has an effective weight
of zero. That raises the chance of the others in its group instead of leaving a hole, and the preview shows the
new number. Rank luck is configured under `luck.permission` in `config.conf`: an invented node mapped to a
multiplier, where a player who holds several takes the highest.

## Keys

`keys/<id>.conf` holds `name`, `item` (material, glow and lore for the physical form), `tradeable`, `crafting`
and `menu`.

- A virtual key is a balance; a physical key is an item.
- `tradeable` lets players move the key with `/crates send`. It ships off.
- `crafting` registers a bench recipe: nine entries read left to right and top to bottom with `air` for an
  empty square, `shapeless`, `amount` and a `permission` checked as the result appears.
- `menu` controls the tile in `/crates keys`: `order`, `hidden`, `click.left` and `click.right` effect lines,
  and `store` lines for a player who holds none.

Every grant, spend, transfer and reset is a row in the key ledger, readable with `/uxmcrates key ledger`.

## Rerolls and milestones

`rerolls.groups` in `config.conf` names rank groups, each with a permission, an amount and a priority. The
highest priority wins and groups do not stack. `/uxmcrates reroll give` grants more, and `rerolls.price` makes
a reroll cost a currency with an amount expression over `reroll`.

`milestones.per-crate` counts the openings of each crate on its own and `milestones.every-crate` counts every
opening. Each rung names its reward `items` and `commands` and whether it is `automatic` or waits in
`/crates milestones`. Turning milestones on resets nobody: the counts already exist.
