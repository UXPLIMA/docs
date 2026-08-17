---
title: The chunk map
order: 404
description: The grid of chunks around your claim, and buying land from it.
icon: map
---

`menu/claim_map.yml`, opened from claim management or by `/claim chunk view`.

A grid of glass panes centred on your claim. Each pane is one chunk, coloured by who owns it.

| Colour | Meaning |
|---|---|
| Your claim's main chunk | The chunk holding the claim block, hologram and spawn |
| Your claim's other chunks | Land you already own |
| An ally's chunk | A claim you are a member of |
| An enemy chunk | Someone else's claim |
| Empty | Wilderness |

Clicking a free chunk that touches your claim buys it. Clicking one of your own removes it — subject
to the connectivity rule.

## Rules the map enforces

- **New chunks must touch the claim.** A pane not adjacent to your land answers *"You can only claim
  chunks connected to your claim."*
- **A chunk holding the claim together cannot be removed.** *"Cannot delete this chunk! It connects
  other chunks to the claim."*
- **The main chunk cannot be removed at all.** Delete the claim instead.
- **`claimSettings.minDistance` still applies.** Default `2` chunks of clearance from another player's
  land.

## Costs and limits

| | Node | Default |
|---|---|---|
| Price per chunk | `uxmclaims.cost.chunk.<count>.<price>` | `25.0` |
| Chunks a player may hold | `uxmclaims.limit.chunk.<n>` | `10`, stacking |

The limit is across all of a player's claims, not per claim.

<Callout type="tip" title="The map is also the fastest way to see what is around you">

`/claim showborders` draws the same information in the world, but the map shows chunks you cannot see
from where you stand — which is what you want before deciding where to expand.

</Callout>
