---
title: currencies.conf
order: 602
description: Every key of a currency, the shipped four, and the placeholder and bridge types.
icon: coins
---

Each block is one currency, keyed by its id. The types and the display keys are in
[Currencies](../features/currencies.md).

| Key | Meaning |
|---|---|
| `type` | One of the nine types |
| `order` | The tab order; the lowest is the default currency |
| `enabled` | Turn it off rather than deleting it |
| `currency-name` | For `ecobits`, `treasury`, `placeholder` and `bridge`: the currency inside that plugin. Not for `vault`: a name there is left out and the console says so |
| `pays-offline` | Whether a seller who is away can be paid straight |
| `material` | For `item`, the item counted |
| `unit` | For `experience`: `levels` (what the bar shows) or `points` (the total, the fair one) |
| `wait-for` | For `treasury`, how long one call may take |
| `whole` | Counts whole numbers only |
| `display` | How an amount is written |

## placeholder

For an economy plugin that publishes no API: a balance read through a placeholder, moved by two commands.

```hocon
tokens {
  type = "placeholder"
  order = 50
  enabled = true
  placeholder = "%tokenmanager_tokens%"
  take = "tm remove {player} {amount}"
  give = "tm add {player} {amount}"
  thousands = ","
  pays-offline = true
  whole = true
}
```

`{player}`, `{uuid}` and `{amount}` are written into a line. A command answers nothing, so the house reads the balance
first and refuses a take the player cannot afford, then trusts the line it sent. A balance it cannot read as a number is
read as zero. PlaceholderAPI must be installed.

## bridge

Any other economy plugin, described rather than compiled against:

| Key | Meaning |
|---|---|
| `plugin` | The plugin's name |
| `class` | The class that answers |
| `access` | `service`, `static`, `plugin` or `class` |
| `accessor` | For `static`, the method that answers; a dot chains two |
| `balance`, `take`, `give` | The method names |
| `argument` | What the methods take for the player: `offline-player`, `player-id` or `player-name` |
| `answer` | What take and give hand back: `boolean`, `vault-response` or `nothing` |
| `currency-class`, `currency-lookup` | For a plugin that turns a currency name into an object |
| `take-negates` | One adjust method and no take: a take is a give of a negative number |
| `naming` | What to call this plugin in a call that asks who moves the money |

Where each value goes in a call is read off the method itself. A method name that is wrong turns the bridge off and the
console says so; it never stops the server.

Related: [Currencies](../features/currencies.md)
