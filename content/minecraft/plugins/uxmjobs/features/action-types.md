---
title: Action types
order: 402
description: All thirty behaviours a price table may name, and what the target word means for each.
icon: list-tree
---

The first word under `actions` is one of these. The target word under it is a material, an entity type, an enchantment,
a job, a visitor number or a placeholder, depending on the type. A material or entity is written as the server names
it, in lower case; a custom item plugin's item is written in its own spelling (`oraxen:ruby`).

| Type | Paid when | Target |
|---|---|---|
| `mine` | A block is broken | The block |
| `block_drop` | A broken block drops items | Each dropped item |
| `place` | A block is placed | The block |
| `strip` | A log is stripped with an axe | The log |
| `wax` | Copper is waxed with a honeycomb | The block |
| `scrape` | Wax or oxidation is scraped off with an axe | The block |
| `brush` | An archaeology block is brushed | The block |
| `kill` | A creature is killed | The entity type |
| `mob_drop` | A killed creature drops items | Each dropped item |
| `kill_job` | A player who works a job is killed | The victim's job id |
| `breed` | Two animals are bred | The entity type |
| `tame` | An animal is tamed | The entity type |
| `milk` | A cow or a goat is milked | The entity type |
| `shear` | A sheep is sheared | The wool that came off |
| `dye` | A sheep is dyed | The dye |
| `fish_item` | An item is pulled out of the water | The item |
| `fish_mob` | A creature is hooked | The entity type |
| `bucket_fill` | A bucket is filled with a creature | The filled bucket |
| `receive` | An item is received from a source the operator prices | The item |
| `craft` | One craft, not one crafted item | The result |
| `smelt` | An item is taken out of a furnace | The item |
| `campfire` | An item is taken off a campfire | The item |
| `brew` | A potion is brewed | The potion item |
| `enchant` | An enchantment is applied | The enchantment |
| `repair` | An anvil restores durability; a rename never pays | The item |
| `smith` | A smithing table upgrade | The result |
| `grind` | A grindstone use | The item |
| `trade` | A villager trade | What was bought |
| `explore` | A chunk is visited for the first time | The visitor index: `"1"` for the first player into it |
| `placeholder` | A number another plugin publishes rises | The placeholder |

## Priced apart

A broken block and what fell out of it are two rows (`mine` and `block_drop`), so silk touch and fortune price correctly
with no special case. A kill and its drops are the same (`kill` and `mob_drop`).

## Ledger

`mine`, `block_drop`, `strip`, `wax`, `scrape` and `brush` ask the placed block ledger, so a block a player placed pays
nothing. `place` is the one action whose payment the ledger records rather than refuses, so a spot that already paid
for a placement pays nothing again inside `ledger.retention-days`. See [Protections](protections.md).

## explore

Ships off in every file. A chunk visited for the first time pays by the visitor index: the first player earns the most.
The table writes a row per chunk per player, bounded by `explore.rows-per-player` (5000) and `explore.retention-days`
(30).

```hocon
explore {
  "1" { money = "3.0", experience = "2.0" }
  "2" { money = "1.5", experience = "1.0" }
}
```

## placeholder

A placeholder is not a payment and cannot be trusted, so it is read at most every `placeholder-actions.minimum-seconds`
(5) per player, and one reading pays for at most `maximum-step` (100) of rise:

```hocon
placeholder {
  "%theirplugin_blocks_walked%" { money = "%amount% * 0.01" }
}
```

## Stacks

With RoseStacker, a kill that takes a stack pays for every creature in it. The bound applies per creature and the
multiplication after it.

Related: [Jobs and prices](jobs-and-prices.md), [Protections](protections.md)
