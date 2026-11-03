---
title: Nametags
order: 404
description: Why the glow shares the player's name with a chat tag, and what a clash looks like.
icon: tag
---

The client colours a glowing outline from the scoreboard team that holds the player's name. That team is also where a
prefix above a head is written, and the main scoreboard allows one team per player, so a plugin that took the team for
itself would take it away from whoever had it.

uxmLib owns the team. uxmGlow contributes a colour under its own name, and uxmLib composes it with every other
contribution, such as uxmChat's tag above the head. A tag and a glow live on one name.

| Limit | What it means |
|---|---|
| The colour is global | Every viewer sees the same outline colour. A colour per viewer would need team packets |
| One colour per name | If two plugins both contribute a colour, the one that asked last wins and the other is named in the log once |

Related: [Concepts](../getting-started/concepts.md#how-a-glow-is-drawn), [Integrations](../integrations/overview.md)
