---
title: aliases.yml
order: 506
description: Mapping short commands onto /claim subcommands.
icon: link-2
---

An alias is a top-level command that runs a `/claim` subcommand as the player who typed it. Tab
completion follows the target.

```yaml
aliases:
  accept: "claim invite accept"
  deny: "claim invite reject"
  unclaim: "claim delete"
  untrust: "claim kick"
```

| Alias | Runs |
|---|---|
| `/accept <claim>` | `/claim invite accept <claim>` |
| `/deny <claim>` | `/claim invite reject <claim>` |
| `/unclaim` | `/claim delete` |
| `/untrust <player>` | `/claim kick <player>` |

Arguments pass straight through: `/accept MyBase` becomes `/claim invite accept MyBase`. Each alias is
also registered as `/uxmclaims:<alias>`.

Add your own and run `/claim reload`:

```yaml
aliases:
  trust: "claim trust"
  invite: "claim invite send"
  claimchat: "claim chat"
  cc: "claim chat"
  warps: "claim warps"
```

`/claim reload` applies the file without a restart:

| Step | Effect |
|---|---|
| Unregister | Every alias registered before the reload is removed, so a deleted alias stops working |
| Register | Every alias in the file is registered again with its current target |
| Resend | Every online player gets their command list again, so tab completion shows the change |

## Notes

- **An alias that collides with another plugin's command may not register.** The console logs
  `Failed to register alias '<name>'` when it loses. `/accept` in particular is taken by several
  teleport-request plugins. Check for conflicts before adding common verbs.
- **Aliases do not bypass permissions.** `/unclaim` still requires `uxmclaims.ability.claim.delete`
  and still asks for confirmation.
- **There is no per-alias permission.** If you need one, do not add the alias: point players at the
  full command and gate that instead.
- **The file is not translated.** Switching [language](languages.md) leaves it as it is.
