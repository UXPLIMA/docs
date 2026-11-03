---
title: Overview
order: 801
description: Running on Paper or Velocity, and the other plugins uxmMOTD sits beside.
icon: plug
---

| With | Notes |
|---|---|
| Velocity | Put the Velocity jar on the proxy; it answers every ping for the network and gates logins with the bypass permission |
| Paper, Folia | Put the Paper jar on a lone server |
| A permission plugin | Needed on Velocity to give staff the command nodes and the bypass |
| uxmMultiLobby | Shows counts inside the network; uxmMOTD shows the network on the outside list |
| Other UXM plugins | Share `plugins/uxmTheme/theme.conf`, so the server list and the game use one palette |

Do not run both jars for the same address: on a network the proxy answers, and a Paper jar behind it is never asked.

Related: [Installation](../getting-started/installation.md)
