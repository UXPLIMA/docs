---
title: Chunks and the claim block
order: 203
description: Growing and shrinking a claim, and the block that marks it.
icon: box
---

## /claim chunk

| Command | What it does | Ability node |
|---|---|---|
| `/claim chunk view` | Open the chunk map for the current claim | n/a |
| `/claim chunk add` | Buy the chunk you are standing on | `uxmclaims.ability.chunk.extend` |
| `/claim chunk remove` | Sell back the chunk you are standing on | `uxmclaims.ability.chunk.shrink` |

Both `add` and `remove` also need the `MANAGE_CHUNKS` role permission.

`add` refuses a chunk that does not touch the claim: *"You can only claim chunks connected to your
claim."*, and one that would break `claimSettings.minDistance` from someone else's land.

`remove` refuses two cases:

- the **main chunk**, which holds the claim block, hologram and spawn; delete the claim instead
- any chunk whose removal would split the claim into two disconnected pieces

The second is the one people hit. A claim shaped like a corridor cannot have its middle removed.

## Costs and limits

| Setting | Node | Default |
|---|---|---|
| Chunks a player may hold in total | `uxmclaims.limit.chunk.<n>` | `10`, stacking |
| Price of one chunk | `uxmclaims.cost.chunk.<count>.<price>` | `25.0`, flat |

The chunk limit is global across all of a player's claims, not per claim.

## /claim block

The claim block is a real block placed in the main chunk that marks the claim in the world and carries
the hologram.

| Command | What it does | Ability node |
|---|---|---|
| `/claim block place <style>` | Place the block where you stand | `uxmclaims.ability.block.place` |
| `/claim block change <style>` | Switch to another style | `uxmclaims.ability.block.change` |
| `/claim block destroy` | Remove it | `uxmclaims.ability.block.destroy` |

All three also need the `MANAGE_BLOCK` role permission.

`<style>` is a key from the `blocks` section of `config.yml`: `block1` and `block2` ship, being
bedrock and obsidian. A style may carry its own permission; `block2` ships gated behind
`claim.blockchange.obsidian`.

The block must be inside the **main chunk**. Anywhere else answers *"You can only place the claim
block inside the main chunk."*

<Callout type="tip" title="The claim block is where the hologram lives">

Holograms are rendered above the claim block, using the lines and styling in the `hologram` section
of [config.yml](../config/config-yml.md). No block, no hologram, which is a reasonable way to let
players opt out of floating text over their base.

</Callout>
