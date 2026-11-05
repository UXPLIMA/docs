---
title: interactables.conf
order: 627
description: Which blocks count as doors, switches, containers and workstations.
icon: mouse-pointer-click
---

Which blocks count as doors, redstone switches, containers and workstations when a visitor or a member uses one on an island. Whether they may is set per role in /is permissions: redstone switches need the redstone permission and containers the chest permission; doors and workstations are open to every member. Add a block by its material name; a name this server does not know is skipped.

The file is `plugins/uxmSkyblock/modules/interactables.conf`. Every default below is the shipped value.

## `categories`

| Key | Default | What it does |
|---|---|---|
| `doors-and-gates` | a list of 35 |  |
| `redstone-triggers` | a list of 28 |  |
| `containers` | a list of 27 |  |
| `workstations` | a list of 10 |  |
