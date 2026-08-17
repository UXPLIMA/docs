---
title: aliases.yml
order: 506
description: Mapping short commands onto /claim subcommands.
icon: link-2
---

An alias is a top-level command that runs a `/claim` subcommand. Tab completion follows the target
automatically.

```yaml
aliases:
  accept: "claim invite accept"
  deny: "claim invite deny"
  unclaim: "claim delete"
  untrust: "claim kick"
```

| Alias | Runs |
|---|---|
| `/accept <claim>` | `/claim invite accept <claim>` |
| `/deny <claim>` | `/claim invite deny <claim>` |
| `/unclaim` | `/claim delete` |
| `/untrust <player>` | `/claim kick <player>` |

Arguments pass straight through: `/accept MyBase` becomes `/claim invite accept MyBase`.

Add your own and run `/claim reload`.

```yaml
aliases:
  trust: "claim trust"
  invite: "claim invite send"
  claimchat: "claim chat"
  cc: "claim chat"
  warps: "claim warps"
```

<Callout type="warning" title="The shipped /deny alias points at the wrong subcommand">

`deny: "claim invite deny"` targets a subcommand that does not exist: the registered one is
`/claim invite reject`. Change the value to `claim invite reject`, or remove the alias. The other
three shipped aliases are correct.

</Callout>

## Notes

- **An alias that collides with another plugin's command loses or wins unpredictably.** `/accept` in
  particular is claimed by several teleport-request plugins. Check `/plugins`-level conflicts before
  adding common verbs.

- **Aliases do not bypass permissions.** `/unclaim` still requires
  `uxmclaims.ability.claim.delete` and still asks for confirmation.

- **There is no per-alias permission.** If you need one, do not add the alias: point players at the
  full command and gate that instead.
