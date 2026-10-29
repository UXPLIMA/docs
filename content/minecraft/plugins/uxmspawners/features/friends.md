---
title: Friends
order: 38
description: Letting other players use a spawner without giving them the block, what a friend can do, and how the list is managed.
icon: user-plus
---

An owner can add friends to a spawner. A friend opens its menu as if it were theirs, so a team can
share one farm without handing the spawner over.

## What a friend can do

| Action | Owner | Friend |
|---|---|---|
| Open the menu, take items and XP, sell | Yes | Yes |
| Toggle auto-kill, virtual storage, XP storage | Yes, with the permission | Yes, with the permission |
| Toggle the hologram | Yes | Yes |
| Level up | Yes | No |
| Manage friends | Yes | No |
| Break the spawner | Yes | No |

A friend who sells gets the money, at their own multiplier. A friend who takes XP gets the XP.

## Managing the list

Only the owner opens the friends menu. In it:

| Click | Does |
|---|---|
| **Add Friend** | Closes the menu and asks for a name in chat |
| A friend's head | Removes that friend |

After **Add Friend**, the owner's next chat message is read as the name and is not shown in chat.
`cancel`, a message with a space in it, or their own name cancels it. The player does not have to be
online, but the name has to be spelled exactly.

The message promises ten seconds, but there is no time limit in 4.2.0: whatever the owner types next
is taken as the name, even an hour later. Tell players to finish or type `cancel`.

## The module file

```yaml
friends:
  enabled: true
  permission: uxmspawners.friends
```

Neither setting is read in 4.2.0: every owner can add friends. To remove the feature, set
`gui: false` on the `friends` item in the language file, which hides the button.

<Callout type="note" title="Friends belong to one spawner">

The list belongs to the spawner, not to the player. Each spawner has its own, and breaking the last
level of a spawner ends it.

</Callout>

Related: [Storage & Selling](storage.md), [Language and Menus](../config/languages.md)
