---
title: Your first hide
order: 204
description: Use the tool, read the status, make a player important, and limit the plugin to the lobby.
icon: play
---

This walk-through uses a fresh install, an operator account, and a second player or a friend online.

## 1. Use the tool

Join. Hotbar slot 8 holds a lime dye. Its tooltip is headed `Player Visibility`, shows `Showing` `Everybody`, and ends
with `Right click to hide the crowd`. Right click it:

- the screen shows `Only the people who matter` with `Staff and friends stay visible.` under it,
- the action bar reads `Only staff and friends are visible.`,
- a pling sounds, and the dye turns yellow.

Your friend vanishes, unless they are an operator: operators are important by default. Right click again for `none`
(a glowing grey dye, everybody gone), and once more to see everybody again.

## 2. Read the status

```
/hide status
You are showing nobody. Hidden right now: 1.
```

## 3. Wait out the cooldown

Right click twice quickly. The second click answers `Wait 2 more seconds.` and nothing changes. An operator with
`uxmplayerhider.cooldown.bypass` never waits; that node is `op` by default, so test this with a player account.

## 4. Make a player important

```
lp user Alex permission set uxmplayerhider.important true
```

In the `important` state, Alex stays visible to you and everybody else is hidden.

## 5. Limit it to the lobby

```hocon
worlds = ["lobby"]
```

Run `/uxmplayerhider reload`. In another world, `/hide` answers `You cannot hide players in this world.`; a player who
walks there sees everybody and loses the tool, and gets both back when they return.

## Next

- [The tool](../hiding/tool.md): its slot, its icons and when it is handed out.
- [Effects](../hiding/effects.md): what each state does when a player moves into it.
- [Database](../database/overview.md): sharing the choice across servers.
