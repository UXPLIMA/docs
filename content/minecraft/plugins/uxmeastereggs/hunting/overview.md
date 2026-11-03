---
title: Eggs and finds
order: 401
description: Placing an egg, what a click does, the protection of an egg block, and what a player is told.
icon: search
---

An egg is a block of your map the plugin remembers. Players find it by clicking it.

## Placing an egg

1. Build the block. A player head with a texture is the usual egg, but any block can be one.
2. Look at it, within six blocks, and run `/uxmeastereggs create <type>`, or `/uxmeastereggs create` to choose the
   type in the [picker](../menus/picker.md).
3. The reply gives the egg's id: *golden is placed at 12 70 -4, as 3f2a91c0.*

| Rule | Why |
|---|---|
| One egg per block | A second `create` on the same block answers `admin.occupied` with the id of the one there |
| The type must have a file | `admin.type-unknown` otherwise |
| The block is not changed | The plugin registers the position; what stands there is yours |

To build a whole hunt from the console or a script, name each block: `/uxmeastereggs create common 12 70 -4 world`.

## Finding an egg

A left or right click with the main hand on an egg block is a find attempt. The click is cancelled, so the block is
not broken, opened or used.

| The player | Is told | Paid |
|---|---|---|
| Finds it for the first time | The type's effects, such as *You found a Golden Egg*, then *You have found 4 of 60 eggs* | The type's reward, and a milestone or the completion reward when this find reaches one |
| Has found it, and the type has no cooldown | *You have already found this egg.* | nothing |
| Has found it, inside the cooldown | *This egg is ready again in 5h 12m.* | nothing |
| Has found it, and the cooldown is over | The type's effects again | The type's reward again |
| Clicks an egg whose type file is gone | *This egg has no type. Tell an operator.* | nothing |

A repeat find, of a type with a cooldown, pays and plays its effects but does not raise the count, the board or the
milestones. The progress line after a first find is the `progress` moment, switched by `find.announce-progress`.

Finding needs no permission, and works in survival, adventure and creative alike.

## Protection

An egg block is protected from everyone, operators included:

| What | Stopped |
|---|---|
| Breaking | yes |
| Burning | yes |
| Water or lava flowing into it | yes |
| An entity changing it, such as an enderman | yes |
| An explosion of an entity or a block | the egg is taken out of the blast |
| A piston pushing or pulling it | the piston does not move |

To change an egg block, take the egg off the map first with `/uxmeastereggs delete`, or move the egg with
`set <egg> position`.

## Removing and moving

| Command | Finds of the egg |
|---|---|
| `/uxmeastereggs delete <egg>` | Deleted from every player. The board rows stay |
| `/uxmeastereggs set <egg> position <block>` | Kept: the egg is the same egg somewhere else |
| `/uxmeastereggs set <egg> type <type>` | Kept |

## Deleting a type

A type file you delete leaves its eggs on the map. They answer *This egg has no type* until you give them a type
with `set <egg> type`, or put the file back and reload. Nothing is lost.

Related: [Types](types.md), [Rewards](rewards.md), [Operator commands](../commands/admin.md)
