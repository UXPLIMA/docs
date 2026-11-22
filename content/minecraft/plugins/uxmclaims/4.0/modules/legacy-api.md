---
title: Legacy API
order: 1353
description: The bridge that keeps add-ons built for the uxmClaims 3.x API working on 4.0, until 5.0.
icon: history
---

The legacy-api module keeps plugins compiled against the uxmClaims 3.x API (`com.uxplima.claim`) working on
4.0 without a rebuild. The old classes are thin facades over the 4.0 API: they are frozen, deprecated, and
removed in uxmClaims 5.0. Leave the module on while any of your add-ons still uses the old API.

Module `legacy-api` · enabled by default · `modules/legacy-api/config.yml` · requires [Lands](lands.md)

## Commands

| Command | What it does | Permission |
|---|---|---|
| `/uxm platform deprecations` | Lists the plugins that used the old API since the server started, since when, and which parts of it. | `uxmclaims.admin.legacy-api` |

## Permissions

| Node | Default | Grants |
|---|---|---|
| `uxmclaims.admin.legacy-api` | op | `/uxm platform deprecations` |

## Settings

| Key | Default | What it does |
|---|---|---|
| `log-callers` | `true` | Logs the name of each plugin that uses the old API, once, the first time it does. Refusals are always logged once. |
| `write-timeout` | `10s` | How long a write called from a thread other than the server thread waits for its result, as a duration (`10s`, `1m`; a plain number counts as seconds, at least 1 second). On time-out the call throws `ClaimException(ERROR_TIMEOUT)`; the write may still finish in the background, which is logged. |
| `slow-handler-millis` | `50` | A 3.x event handler (`@ClaimEventHandler`) running longer than this is logged. |

## What the bridge covers

| 3.x part | On 4.0 |
|---|---|
| `ClaimFacade` reads | Answered from 4.0 lands. |
| `ClaimFacade` writes | Go to the matching 4.0 module ([Lands](lands.md), [Warps](warps.md), [Claim Block](claim-block.md), [Bans](bans.md), [Invites](invites.md), [Expiry](expiry.md)). Personal lands only: town, nation and server lands are read-only. |
| Writes to town, nation or server lands, or whose module is off | Refused with `ERROR_UNSUPPORTED_IN_V4`. |
| `PlayerFacade` | Reads 4.0 player profiles. |
| 3.x events | Made from the 4.0 land events and from the bans, invites, expiry, warps, vault and claim-block modules. A module that is off sends none. |
| `ClaimPermissionPolicy` | For an online player inside the claim, the 4.0 protection rules answer (admin bypass, towns, relations, WorldGuard). Ability checks read the permission nodes in the 3.x order. |
| `BorderVisualizer` | Shows the [borders](borders.md) of the land the player stands in, in the configured colours. With the borders module off, requests are ignored. |
| `InputMenu` | Asks through the 4.0 text input (chat, [dialog](dialogs.md) or [Bedrock](bedrock.md) form). |

For the class by class list of old and new calls, read the developer page
[Moving from the 3.x API](../developer/legacy-api.md).

## Behaviour changes from 3.x

- A write called on the server thread runs at once and returns the committed result, or throws the refusal.
  Called from any other thread, it is handed to the server thread and the caller waits at most
  `write-timeout`.
- A handler that throws no longer stops the write. The error is logged with the plugin's name and the next
  handler runs.
- Handlers run on the server thread (on Folia, the land's region thread).

<Callout type="warning" title="Removed in 5.0">

uxmClaims 5.0 ships without this module. Run `/uxm platform deprecations` after your server has been up for a
while and ask the authors of every plugin it lists to move to the 4.0 API (`com.uxplima.uxmclaims.api`).

</Callout>

## Notes

- Turning the module off makes every plugin that uses the old API fail. Check `/uxm platform deprecations`
  first.
- `updateRolePriority` moves the role one place at a time until it reaches the new priority.
- `BorderVisualizer` ignores the colour and chunk set a plugin passes (logged once per plugin).
- `InputMenu.close()` cannot withdraw a question that was already asked.

Related: [Moving from the 3.x API](../developer/legacy-api.md), [API](../developer/api.md),
[Upgrading from 3.x](../upgrading/from-3x.md)
