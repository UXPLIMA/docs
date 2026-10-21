---
title: Tool rewards
order: 3
description: Handing out tools per rank, and the attribute that makes it safe.
icon: wrench
---

Each rank can hand the player a set of tools. They are cloned into the backpack when the
player's rank is established and re-synced whenever it changes.

## Where the tools go

Put the `Tool` instances inside the package, at:

```
ServerScriptService/uxrLevelSystem/Storage/Tools/
```

The bootstrap moves that folder to `ServerStorage/uxrLevelSystem/Tools` on start, which is
where the code looks for it. Tools are never replicated to clients from there.

## Naming

```lua
LevelTools = { "Level 1 Tool", "Level 2 Tool" },
```

Names must match the `Tool` instance names exactly, spaces and capitals included. A name
with no matching tool logs `[uxrLS]: tool not found:` in Output and the rest of the list
is still given.

## The list is absolute, not cumulative

The tools a player holds are the `LevelTools` of the rank they are on, and only those. A
rank that lists nothing gives nothing.

That is why the shipped ladders repeat the earlier tools on every rank:

```lua
{ LevelName = "Cadet",    LevelTools = { "Level 1 Tool" } },
{ LevelName = "Officer",  LevelTools = { "Level 1 Tool", "Level 2 Tool" } },
{ LevelName = "Sergeant", LevelTools = { "Level 1 Tool", "Level 2 Tool", "Level 3 Tool" } },
```

If you want ranks to keep what they earned, repeat the list. If you want a rank to trade
one tool for a better one, list only the new one.

## How the sync works

On a rank change the system:

1. destroys every tool carrying the `LevelTool` attribute, in the character and in the
   backpack
2. clones the current rank's tools out of `ServerStorage`
3. sets `LevelTool` on each clone and parents it to the backpack

<Callout type="warning" title="The LevelTool attribute is what stops your tools being deleted">

The clear step only removes tools tagged with the `LevelTool` attribute, which the system
sets on the clones it makes. A tool your own code gives the player is untouched.

The corollary: do not set that attribute on anything of your own. If you do, the next
rank change will silently delete it.

</Callout>

## When it runs

| Event | Sync |
|---|---|
| Rank changes up or down | Yes |
| XP changes without a rank change | No |
| Character respawns | Yes, tools are re-issued |
| Team changes | On the next rank evaluation for the new team |

A player who is demoted loses the higher rank's tools at the same moment they lose the
rank, because the clear step is unconditional and the re-issue uses the new rank.

## When it does not run

The sync needs a character, a team, a ladder for that team, an XP value and a backpack. If
any of those is missing it returns quietly. A player who joins before their character
loads is handled by the respawn path.
