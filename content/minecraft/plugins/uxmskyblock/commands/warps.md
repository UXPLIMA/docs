---
title: Warps
order: 540
description: 'Public warps let other players teleport to specific spots on your island:
  a shop, a farm, an arena. An island can have several named warps.'
---

---

## `/is setwarp <name>`

Creates (or moves) a named warp at your current position. Requires the `SET_WARP`
permission.

```
/is setwarp shop
/is setwarp farm
```

## `/is delwarp <name>`

Deletes a named warp.

```
/is delwarp shop
```

## `/is warp <player> [name]`

Teleports you to another island's warp. If you omit `[name]`, the island's first
warp is used.

```
/is warp Steve shop
/is warp Steve
```

You can also browse and click warps in the [Warp menu](../menus/warps.md).

---

## Warp Limits

How many warps an island may have is controlled by:

```yaml
warp:
  default-limit: 1
  permission-prefix: "skyblock.warps"
```

- Everyone gets `default-limit` warps.
- A player raises their limit with the permission `skyblock.warps.<number>`: the
  **highest** number they hold wins.

| Permission | Warp limit |
|------------|-----------|
| *(none)* | `default-limit` (1) |
| `skyblock.warps.3` | 3 |
| `skyblock.warps.5` | 5 |

Give these through your permission/rank plugin (e.g. as a VIP perk).

---

## Warp Signs

You can also create a warp by placing a **sign** on your island:

1. Write the sign tag on the **first line** (default `[warp]`).
2. The warp point is set to where you stand.

After it activates, the first line is rebranded (default `&9[Warp]`). Configure
both in `config.yml`:

```yaml
warp:
  sign-tag: "[warp]"
  sign-display: "&9[Warp]"
```
