---
title: language.yml
order: 305
description: Every message, in English and Turkish, in one file.
icon: languages
---

One file holding both languages. `settings.lang` in `config.yml` picks which block is used.

```yaml
messages:
  en:
    prefix: '&8[&6uxmBlacksmith&8] &7'
    noPermission: 'You do not have permission to use this feature.'
    tradeInProgress: 'Your trade is still in progress, please wait.'
    tradeReady: 'Your trade is complete! Click to claim your reward.'
    …
  tr:
    prefix: '&8[&6uxmBlacksmith&8] &7'
    noPermission: 'Bu özelliği kullanma izniniz yok.'
    …
```

Around 124 message keys per language, covering trades, burns, boosts, progression, the binary item
store and every admin subcommand.

## Adding a language

Copy the `en:` block, rename it to your code, translate it, and set `lang:` to match. Any key works:
`de`, `fr`, `pt`. There is nothing to register.

If `lang` names a block that does not exist, the plugin warns, resets the setting to `en` and saves
`config.yml`. The same happens if `lang` is missing entirely.

## Formatting

Both `&` codes and MiniMessage work in every message, and can be mixed in one string: the parser
converts legacy codes to tags before deserialising. Newer messages use MiniMessage with gradients;
older ones use `&`. Both are correct.

`prefix` is prepended to chat messages. Set it to an empty string to remove it.

## Placeholders in messages

Each message takes the placeholders its own call site provides, `{player}`, `{level}`, `{boost}`,
`{amount}`, `{multiplier}`, `{duration}`, `{trade}`, `{slot}`, `{file}` and so on. A placeholder the
call site does not provide is left as literal text, which is how you can tell you used the wrong one.

The shipped default of a message is the best reference for what it accepts. Delete a key and the
plugin writes the default back on the next start, which is also how you recover one you have broken.

## Keys worth knowing

| Key | Shown when |
|---|---|
| `noPermission` | Any refused action |
| `tradeInProgress` | Clicking a slot that is still running |
| `tradeReady` | A trade has finished |
| `insufficientItems` / `insufficientMoney` | A requirement is unmet |
| `startTrade` | A trade started |
| `tradeClaimed` | A reward was handed over |
| `timeBoostAlreadyActive` | A second `TIME` boost was right-clicked |
| `instantBoostAlreadyActive` | A second `INSTANT` boost was right-clicked |
| `progressionPerkMaxed` / `progressionPerkLocked` / `progressionPerkNoPoints` | A perk purchase was refused |
| `adminQueueNoTradeInSlot` | `admin queue cancel` on an empty slot |
| `tradeEmptyPlaceholder` | `%uxmblacksmith_timeleft_<n>%` for an empty slot |

<Callout type="note" title="Your comments will not survive">

The plugin rewrites this file from its own defaults whenever a key is missing. Comments you add are
lost at that point. Keep translation notes in a separate document.

</Callout>
