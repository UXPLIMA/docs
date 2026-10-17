---
title: Conversions
order: 38
description: Turning stored products into other products inside the farmer.
icon: refresh-cw
---

A conversion turns products the farmer is holding into a different product, inside the storage,
without the player ever holding an item. Nine gold nuggets become an ingot; nine ingots become a
block. It is how you give raw output a reason to be refined instead of sold.

Conversions live in `languages/<lang>/conversions.yml` and appear in the conversions menu.

## A conversion

```yaml
gold-nugget-to-ingot:
  item:
    material: GOLD_INGOT
    display-name: "<gold><b>Gold Ingot"
    lore:
      - "<gray>Convert all nuggets into ingots."
      - "<dark_gray>▪ <gray>Available Operations: <green><possible_conversions>"
      - "<dark_gray>▪ <gray>Available Gold Nuggets: <green><papi:'uxmfarmer_<uuid>_amount_GOLD_NUGGET'>"
  conversion-type: BOTH
  requirements:
    price: 1000
    static-price: false
    products:
      - 9:GOLD_NUGGET
  converts-to:
    products:
      - 1:GOLD_INGOT
    messages:
      - "<prefix> <gray>Processed <white><processed_conversions> <gray>times."
    money: 0
  cooldown: 30
```

The key (`gold-nugget-to-ingot`) is the conversion's id. `item` is what the player sees in the
menu, written in MiniMessage, and `<papi:'...'>` reads a placeholder inline so the lore can show
live stock.

## Conversion type

| Value | Behaviour |
|---|---|
| `SINGLE` | One conversion per click |
| `ALL` | Everything possible in one click |
| `BOTH` | Left-click converts one, shift-click converts all |

## Requirements

| Key | Meaning |
|---|---|
| `price` | Money charged per conversion |
| `static-price` | When `true`, converting all costs `price` once rather than once per conversion |
| `products` | What is consumed, written `amount:MATERIAL` |
| `placeholders` | PlaceholderAPI conditions, same shape as [level requirements](storage-and-levels.md#requirements) |

`static-price: false` with `conversion-type: ALL` is the combination that charges properly for
bulk work. Setting it to `true` makes converting a full storage cost the same as converting nine
nuggets, which is almost never what you want unless the fee is a nominal one.

## Output

| Key | Meaning |
|---|---|
| `products` | What is produced, written `amount:MATERIAL` |
| `money` | Money paid to the player |
| `actions` | Menu actions run afterwards, e.g. `[close]` |
| `player-commands` | Commands run as the player |
| `console-commands` | Commands run from console |
| `messages` | Messages sent to the player |

Each of those last four has a matching `execute-...-per-convert` or `send-messages-per-convert`
flag. Left at `false`, a convert-all runs the commands and sends the messages once. Set to `true`
and they fire per conversion, which for a full storage means hundreds of command executions, so
treat `true` as something you enable deliberately for a cheap action, never for a command.

`<processed_conversions>` in a message is how many conversions actually happened, and
`<possible_conversions>` in lore is how many the player could do right now.

## Cooldown

`cooldown` is in seconds and is per player per conversion. Thirty seconds is enough to stop a macro
from converting in a loop while staying invisible to a player using the menu normally.
