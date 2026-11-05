---
title: rewards.conf
order: 640
description: The reward inbox for rewards a player could not be handed.
icon: inbox
---

The reward inbox: a reward a player could not be handed, because they were offline or their inventory was full, waits there until they claim it.

The file is `plugins/uxmSkyblock/modules/rewards.conf`. Every default below is the shipped value.

## `rewards`

| Key | Default | What it does |
|---|---|---|
| `enabled` | `true` | false turns the inbox off. |
| `expiry-check-interval` | `5m` | How often rewards that waited past their time are taken out of the inbox. |
| `claim-recovery-window` | `2m` | How long a claim may sit unfinished before another claim may take it over. A claim that is still working is well inside this window. A node that died halfway through one is well outside it. Shorten it and two claims of one reward can overlap; lengthen it and a crashed claim waits longer to be freed. |
