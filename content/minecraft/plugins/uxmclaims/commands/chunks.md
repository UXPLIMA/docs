---
title: Chunks and the claim block
order: 203
description: Growing and shrinking a claim, and the block that marks it.
icon: box
---

A claim is a connected group of chunks around a main chunk. `/claim chunk` adds and removes chunks of
the current claim, and `/claim block` manages the claim block, the marker in the main chunk that
carries the hologram.

## Commands

### /claim chunk

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim chunk view` | Opens the chunk map of the current claim | none | none |
| `/claim chunk add` | Buys the chunk you stand in for the current claim | `MANAGE_CHUNKS` | `uxmclaims.ability.chunk.extend` |
| `/claim chunk remove` | Removes the chunk you stand in from the current claim, after confirmation | `MANAGE_CHUNKS` | `uxmclaims.ability.chunk.shrink` |

`add` refuses a chunk when:

| Reason | Message key |
|---|---|
| It is already claimed, or in a WorldGuard region or a disabled world | `errorNotClaimableArea` |
| It does not share a side with the claim | `errorChunkNotConnected` |
| Another player's claim is within `claimSettings.minDistance` chunks | `errorClaimTooClose` |
| The claim already has as many chunks as the owner's chunk limit | `errorChunkLimitExceed` |
| You cannot pay the chunk price | `errorInsufficientFunds` |

`remove` refuses the main chunk (`errorMainChunkRemove`) and any chunk whose removal would split the
claim in two (`errorChunkBreaksConnectivity`). A claim shaped like a corridor cannot lose a chunk from
its middle.

Standing on free land next to your own claim, a bare `/claim` does the same as `/claim chunk add`
without confirmation: see [Claim commands](claim.md#the-bare-command).

### /claim block

| Command | What it does | Claim access | Ability node |
|---|---|---|---|
| `/claim block place <style>` | Places the claim block where you stand | `MANAGE_BLOCK` | `uxmclaims.ability.block.place` |
| `/claim block change <style>` | Switches the block to another style | `MANAGE_BLOCK` | `uxmclaims.ability.block.change` |
| `/claim block destroy` | Removes the block | `MANAGE_BLOCK` | `uxmclaims.ability.block.destroy` |

`<style>` is a key under `blocks` in `config.yml`. `block1` (bedrock) and `block2` (obsidian) ship,
and a new claim gets `claimSettings.defaultBlock`. A style with a `permission` set needs that server
permission: `block2` ships behind `claim.blockchange.obsidian`. Placing the style the claim already
has skips that check.

The block must stand in the main chunk; elsewhere you get `errorBlockOutsideMainChunk`. The hologram
floats above the block, using the `hologram` section of [config.yml](../config/config-yml.md), and
disappears while the claim has no block.

## Permissions

| Node | Applies to | Default |
|---|---|---|
| `uxmclaims.limit.chunk.<n>` | Chunks one claim may hold, read from the owner | `10` |
| `uxmclaims.cost.chunk.<count>.<price>` | Price of a chunk, paid by the player who runs `add` | `25.0` |

Removing a chunk refunds nothing. See [Limits, costs and delays](../permissions/limits.md).

<Callout type="warning" title="Removing a chunk deletes what is inside it">

Every warp in a removed chunk is deleted, and a spawn inside it moves to the centre of the main chunk.
Move warps out before running `/claim chunk remove`.

</Callout>

## Settings

| Key in `config.yml` | Default | Effect |
|---|---|---|
| `claimSettings.minDistance` | `2` | Chunks kept free between your claim and another player's |
| `claimSettings.defaultBlock` | `block1` | Style of a new claim's block |
| `blocks.<style>.permission` | `''`, `claim.blockchange.obsidian` | Server permission a style needs, empty for none |

## Notes

- The chunk limit compares the chunks of the claim you extend with the owner's limit. A player with
  several claims gets the limit for each of them.
- Removing the block is a way to drop the floating text over a claim without changing the
  `hologram` lines for everyone.
- The [chunk map menu](../menus/claim-map.md) adds and removes chunks with the same checks.

Related: [Claim commands](claim.md), [Claim map](../menus/claim-map.md), [config.yml](../config/config-yml.md)
