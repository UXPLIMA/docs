---
title: Geyser extension
order: 608
description: "The extension's config.conf: where it finds the build, how often it looks, and the pack host."
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

## loading-protection-seconds

| Key | Default | Meaning |
|---|---|---|
| `loading-protection-seconds` | `300` | Keep a Bedrock player connected while their client still loads the packs after joining, at most this many seconds, 0 to 1800. `0` turns it off |

With Geyser's `forward-player-ping` on, Geyser hands the server's keep-alives to the client and answers only when the
client does. A phone loading a large pack answers nothing for minutes, so the server dropped it as timed out. Until the
player is in the world, the extension answers for it instead. Each load of 10 seconds or more is logged either way, so
you see how long your players wait.

## pack-host

Bedrock players download the packs over HTTP from the extension instead of through Geyser's own transfer, which is
much faster for a large pack, above all on a phone. Geyser fetches each pack from the address once too, and a player
whose device cannot reach the address still gets the packs through Geyser.

| Key | Default | Meaning |
|---|---|---|
| `enabled` | `false` | Serve the packs over HTTP; it opens a port |
| `bind` | `0.0.0.0` | The address the server listens on |
| `port` | `8171` | The port it listens on |
| `address` | empty | The address players and Geyser reach it at, as `http://play.example.com:8171`, or `https://` behind a proxy that forwards to the port. The host stays off while it is empty |

Only the packs of the current build are served, each under the SHA-256 of its bytes, so a new build is a new address
and no client keeps an old pack; any other request gets nothing. When Geyser cannot fetch a pack from the address, the
console says so once, the packs go through Geyser, and the host is tried again after five minutes. Open the port in
the firewall, and on a network keep Geyser and the players able to reach the same address.

## What the extension does

| When | Does |
|---|---|
| Geyser starts | Copies the item and block mappings and the locale overrides into Geyser's folder, registers the heads, checks Geyser's version |
| A player joins | Sends the base packs, then each part the player did not switch off, then their animated pack, and answers the server's keep-alives until the player is in the world |
| The build changes | New packs reach players from their next join; new items, blocks, names or heads need a Geyser restart, and the console says so once. See `new-items-restart` in [pack.conf](pack-conf.md) |
