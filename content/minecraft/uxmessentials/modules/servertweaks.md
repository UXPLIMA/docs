---
title: Server Tweaks
order: 9230
description: 'Small server-side adjustments: the F3 brand, a console log filter, unsigned chat and SignedVelocity.'
---

Server Tweaks is the handful of low-level behaviours that belong to no single feature but that most servers
eventually want to change: the brand shown on the debug screen, a console spam filter, an unsigned-chat option,
and a SignedVelocity backend companion. There are no player commands here: every tweak is a silent server-side
effect.

Module `servertweaks` · enabled by default · `modules/servertweaks/config.conf`

## Permissions

{/* generated:permissions */}
| Node | Default | Grants |
|---|---|---|
| `uxmessentials.module.servertweaks` | op | Hot-reload / inspect the servertweaks module (the small server-behaviour switches). |
{/* /generated */}

## Settings

{/* generated:settings */}
| Key | Default | What it does |
|---|---|---|
| `f3-brand.enabled` | `true` |  |
| `f3-brand.brand` | `"uxmEssentials"` |  |
| `console-filter.enabled` | `false` |  |
| `console-filter.patterns` | `[ ]` |  |
| `no-chat-reports.enabled` | `false` |  |
| `signed-velocity.enabled` | `false` |  |
{/* /generated */}

## Placeholders

{/* generated:placeholders */}
| Placeholder | Renders |
|---|---|
| `%uxmessentials_servertweaks_brand%` | The server brand reported to clients on the F3 screen, or a dash when the tweak is off. |
{/* /generated */}

## Notes

- **Everything inside ships off except the F3 brand,** which is on so the debug screen names your server out of
  the box. The rest you opt into.
- **The console filter drops lines by pattern,** which is how a noisy third-party warning stops burying the logs
  you actually read.
- **No-chat-reports is a client-visible promise:** it tells clients the server does not carry signed chat, so
  players are not prompted about reporting. Turn it on deliberately.
- **The SignedVelocity companion belongs on the backend,** and only matters on a proxy network running that
  plugin.

Related: [Command Control](commandcontrol.md), [Communication](communication.md), [Cross-server](../cross-server/overview.md)
