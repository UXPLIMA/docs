---
title: Installation
order: 12
description: Requirements, the hooks to choose first, and verifying the install.
icon: download
---

## Requirements

| Requirement | Detail |
|---|---|
| Server | Paper 1.21 or newer (`api-version: 1.21`) |
| Java | Whatever your Paper build requires |
| Folia | Supported (the plugin declares `folia-supported: true`) |
| Region plugin | One of the supported claim or skyblock plugins, see [Integrations](../integrations.md) |
| Economy | Vault or PlayerPoints, if you charge for NPCs |

Everything else the plugin needs (the database driver, the YAML parser, the JSON client) is
downloaded by Paper's library loader on first start. You do not shade or install anything.

## Install

1. Drop the jar into `plugins/` and start the server once. The plugin writes `config.yml`,
   `items.yml` and a `languages/` folder, then disables nothing: it is usable straight away.
2. Set `hooks.region-hook` to the plugin that owns your islands or claims. This is the single
   most important option: without it the plugin does not know where a player's NPC belongs.
3. Set `hooks.money-hook` to `Vault` or `PlayerPoints` if `settings.price` is above zero.
4. Set `hooks.interaction-hook` to the NPC plugin you want to draw the character, or leave it
   empty to use the built-in packet NPC.
5. Reload with `/uxmhelpernpc reload`.

<Callout type="note" title="License key">

`license-key` in `config.yml` is read at startup. Fill it in before you take the server public.

</Callout>

## Choosing how the NPC appears

`interaction-hook` decides what the player actually sees:

| Value | What draws the NPC |
|---|---|
| *(empty)* | The built-in packet NPC. Nothing else to install |
| `Citizens` | A Citizens NPC |
| `FancyNpcs` | A FancyNpcs NPC |
| `ZNPCsPlus` | A ZNPCsPlus NPC |
| `ModelSystem` | A model from ModelEngine, BetterModel or ItemsAdder, chosen in `model-settings` |

The hologram above the NPC is always the plugin's own; `hologram.enabled` turns it off.
