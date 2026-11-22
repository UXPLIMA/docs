---
title: Economy
order: 1314
description: Connects uxmClaims to your economy plugin through Vault and sets the price of every paid action.
icon: coins
---

The economy module takes money for paid actions (claiming a chunk, founding a town, teleporting to a warp)
through Vault and the economy plugin behind it. Every price lives in one file, `costs.yml`, which lists each
paid action the running modules offer. The module has no commands of its own.

Module `economy` · enabled by default · `modules/economy/config.yml` · requires the Vault plugin

## Permissions

The module registers no nodes. A cost entry with a `node` reads the 3.x price nodes, see
[Price nodes](#price-nodes).

## Settings

`config.yml`:

| Key | Default | What it does |
|---|---|---|
| `provider` | `vault` | Where money comes from. `vault` is the only provider; any other value is logged and Vault is used. |
| `vault-provider` | `""` | Which Vault economy to use when several are installed: a plugin name, economy name or currency name (singular or plural), any case. Empty uses Vault's default. 3.x `generalSettings.economyProvider`. |
| `format` | `"#,##0.00"` | How amounts are shown, as a Java DecimalFormat pattern. Empty uses the economy plugin's own format. |
| `format-locale` | `en-US` | Whose decimal and thousands marks the format uses: `en-US` shows `1,234.50`, `tr-TR` and `de-DE` show `1.234,50`. Empty uses the machine's system language. A player reading another language than the server's sees that language's marks. |
| `when-no-provider` | `FREE` | What paid actions do while no economy plugin is registered with Vault: `FREE` charges nothing and logs a warning, `REFUSE` refuses them. |
| `refund.chunk-unclaim-percent` | `0` | Percent of the claim price paid back when a chunk is unclaimed, `0` to `100`. |
| `refund.land-delete-percent` | `0` | Percent of the price paid for its chunks paid back when a land is deleted. |

When `vault-provider` names an economy that is not found, the log lists every economy Vault knows, with plugin
and currency names, so you can copy the right one.

## Prices

`costs.yml` holds one list, `costs`. It ships empty and fills itself: every cost a running module registers is
added at its default price the first time that module runs. Each entry:

| Key | Default | What it does |
|---|---|---|
| `key` | (required) | The cost, as the module registered it, such as `lands:chunk-claim`. Do not change it. |
| `price` | registered default | Price of the first one; `0` makes it free. |
| `description` | registered text | What the cost is for. Only a note. |
| `scaling.mode` | `none` | `none`, `add` (price + step x owned) or `percentage` (price x (1 + step / 100) ^ owned, the 3.x `incrementRate`). |
| `scaling.step` | `0` | Money (`add`) or percent (`percentage`) per item the player already has. |
| `scaling.max` | `0` | Highest price after scaling; `0` is no cap. |
| `node` | none | Optional permission prefix for per-player prices, such as `uxmclaims.cost.chunk`. |

```yaml
costs:
  - key: lands:chunk-claim
    price: 25.0
    description: Claim a chunk (3.x uxmclaims.cost.chunk)
    scaling: { mode: percentage, step: 10, max: 500 }
    node: uxmclaims.cost.chunk
```

Write decimals in `price`, `step` and `max` with a dot. Each module page lists its own cost keys, for example
`lands:chunk-claim` and `lands:land-create` on [Lands](lands.md) and `towns:create` on [Towns](towns.md).

## Price nodes

For an entry with `node: uxmclaims.cost.chunk`, a player's nodes change their price. `_` is the decimal point,
and when several match, the lowest price wins.

| Node | Price |
|---|---|
| `uxmclaims.cost.chunk.3.10` | `10` for the player's 3rd one, no scaling. |
| `uxmclaims.cost.chunk.12_5` or `uxmclaims.cost.chunk.*.12_5` | `12.5` as the base price, then scaled. |
| `uxmclaims.cost.chunk.*` | Free. |

`uxmclaims.admin` also makes every cost with a `node` free. Costs without a `node` ignore permission nodes.

## Notes

- With the economy module turned off, or Vault not installed, every paid action is free. If the module is on but
  fails to start, paid actions are refused with "Payments are not available right now".
- An economy plugin that loads after uxmClaims is picked up once every module has started. An economy that
  registers with Vault or goes away later is followed too.
- `/uxm reload` reads both files again and looks the Vault economy up anew.
- The price nodes are read from the player's own permissions while they are online. They do not go through the
  [Limits](limits.md) module and its `limits.yml`.

Related: [Limits](limits.md), [Bank](bank.md), [Lands](lands.md), [Integrations](../integrations.md)
