---
title: Overview
order: 901
description: Every plugin and service uxmMultiLobby reaches, and troubleshooting.
icon: plug
---

| With | Adds |
|---|---|
| Velocity, BungeeCord | The move, and the `proxy` source, over the `BungeeCord` channel |
| PlaceholderAPI | The [placeholders](../placeholders/reference.md), and placeholders in effect lines |
| Redis with `lettuce-core` | The `redis` source |
| Geyser, Floodgate | The window as a Bedrock form |
| uxmLang | One language choice for every UXM plugin |
| uxmJoinItems | A hotbar item of its own that runs `/lobby`, instead of this plugin's tool |
| uxmMOTD | The network's player count on the server list |

## Troubleshooting

| Symptom | Cause |
|---|---|
| Every lobby shows offline | With `proxy`, a `proxy-name` does not match the proxy's server list. With `ping`, check `host`, `port` and the firewall |
| A crashed lobby shows as empty | That is the `proxy` source; switch to `ping` |
| The console says the source fell back to the proxy | `status.source` is `redis` and `lettuce-core` is not installed |
| Clicking does nothing visible | The player is on that lobby already, or on cooldown; the `effects` lists decide what they are told |
| Moves never happen on Velocity | `bungee-plugin-message-channel` is off in `velocity.toml` |

Related: [Installation](../getting-started/installation.md)
