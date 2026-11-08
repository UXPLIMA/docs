---
title: Friends
order: 138
description: Letting other players use a spawner without giving them the block, what a friend can do, and how the list is managed.
icon: user-plus
---

An owner can add friends to a spawner. A friend opens its menu as if it were theirs, so a team can
share one farm without handing the spawner over.

## What a friend can do

| Action | Owner | Friend |
|---|---|---|
| Open the menu, take items and XP, sell | Yes | Yes |
| Toggle Auto Kill, Virtual Storage, XP Storage | Yes, with the permission | Yes, with the permission |
| Apply a boost | Yes | Yes |
| Toggle the hologram, stop the spawner | Yes | No |
| Level up | Yes | No |
| Manage friends | Yes | No |
| Break the spawner | Yes | No |

A friend who sells gets the money, at their own multiplier. A friend who takes XP gets the XP. With a
claim plugin installed, a friend also needs the claim's permission to open containers there: a
player removed from the claim loses the spawners in it too.

## Managing the list

Only the owner opens the friends menu. In it:

| Click | Does |
|---|---|
| **Add Friend** | Closes the menu and asks for a name in chat |
| A friend's head, twice within 5 seconds | Removes that friend. The first click asks to click again |
| The arrows | Turn the page. One page holds 28 heads |

After **Add Friend**, the owner's next chat line is read as the name and is not shown in chat. The
player named has to have played on the server before; they do not have to be online. Typing one of
`chat-quit-triggers`, such as `cancel`, stops it, and so does leaving the server.

## Settings

`general-settings.yml`:

```yaml
friends:
  enabled: true
  permission: uxmspawners.friends
  chat-timeout: 15

chat-quit-triggers:
  - "cancel"
  - "iptal"
```

| Setting | Meaning |
|---|---|
| `enabled` | `false` takes every friend's access away and closes the friends menu. The lists are kept for when it is turned on again |
| `permission` | What an owner needs to add friends. Empty lets every owner. The default node is for ops only until you grant it |
| `chat-timeout` | Seconds the name prompt waits. `0` waits until the owner answers, cancels or leaves |
| `chat-quit-triggers` | Words that cancel the prompt, compared without case. An empty list turns cancelling off |

<Callout type="note" title="Friends belong to one spawner">

The list belongs to the spawner, not to the player. Each spawner has its own, and breaking the last
level of a spawner ends it.

</Callout>

Related: [Storage & Selling](storage.md), [Language and Menus](../config/languages.md)
