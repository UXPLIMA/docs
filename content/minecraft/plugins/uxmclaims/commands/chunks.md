---
title: Chunk Commands
order: 110
description: Claiming, unclaiming and inspecting the 16x16 chunks a claim is made of.
icon: grid-3x3
---

## Available Commands

| Command | Description |
|---------|-------------|
| `/claim chunk view` | Opens the chunk map menu |
| `/claim chunk add` | Adds your current chunk to the claim |
| `/claim chunk remove` | Removes the most recently added chunk |

---

## What is a Chunk?

A chunk is a 16×16 block area extending from bedrock to sky. When you claim land, you claim whole chunks.

<Callout type="info" title="How Big is a Chunk?">

16×16 blocks = **256 blocks of area**. A small house needs 1 chunk, a large base might need 4-9 chunks.

</Callout>

---

## Command Details

### `/claim chunk view`

Opens a visual map showing:

- Your claimed chunks (green)
- Available chunks to claim (gray)
- Other players' claims (red)

From this menu, you can click to add or remove chunks.

---

### `/claim chunk add`

Adds the chunk you're currently standing in to your claim.

**Requirements:**

- Chunk must be adjacent to an existing claimed chunk
- Chunk must not be claimed by another player
- You must have available chunk slots
- May cost money (server configurable)

---

### `/claim chunk remove`

Removes the **most recently added** chunk from your claim.

<Callout type="warning">

You cannot remove the main chunk (where the claim was originally created).

</Callout>

---

## Examples

### Expanding Your Claim

```
# Stand in the new chunk you want to add
/claim chunk add
```

### Using the Map

```
/claim chunk view
```

---

## Next Steps

- [🗺️ Claim Map Menu](../menus/claim-map.md) - Visual chunk management
- [📖 Basic Concepts](../getting-started/concepts.md) - Understanding chunks
