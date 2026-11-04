---
title: Geyser extension
order: 608
description: "The extension's config.conf: where it finds the build and how often it looks."
icon: plug
---

The extension writes `config.conf` in its folder under Geyser's `extensions/` on its first start.

## source

| Key | Default | Meaning |
|---|---|---|
| `folder` | `auto` | Where the build is, below |
| `check-every-seconds` | `30` | How often the build is looked at for a change; at least 5 |

| `folder` | Reads |
|---|---|
| `auto` | `plugins/uxmBedrock/output` beside Geyser: the build of the Paper server Geyser runs on, or, on a Velocity proxy with uxmBedrock-Velocity, the network pack |
| `player-connection` | What the Bedrock players' own connections carry, for Geyser Standalone or a proxy without uxmBedrock-Velocity |
| a path | That folder, absolute or relative to Geyser's folder |

A wrong path serves no pack and says so once in the console.

## What the extension does

| When | Does |
|---|---|
| Geyser starts | Copies the item and block mappings and the locale overrides into Geyser's folder, registers the heads, checks Geyser's version |
| A player joins | Sends the base packs, then each part the player did not switch off, then their animated pack |
| The build changes | New packs reach players from their next join; new items, blocks, names or heads need a Geyser restart, and the console says so once |
