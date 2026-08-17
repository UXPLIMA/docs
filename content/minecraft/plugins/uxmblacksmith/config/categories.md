---
title: Category files
order: 302
description: The trade files under categories/, how they are generated, and their shape.
icon: folder-open
---

Every category names a file under `categories/`. That file holds its trades and nothing else.

```yaml
categories:
  swords:
    trades: swords.yml     # → plugins/uxmBlacksmith/categories/swords.yml
```

## They are generated

If the named file does not exist when the category loads, the plugin **writes it** with worked
defaults matched to the key and logs:

```
Generated category file: categories/swords.yml with rich defaults for swords
```

The defaults it picks depend on the key:

| Key it recognises | What it writes |
|---|---|
| `swords`, `blades`, `weapons` | Four weapon contracts, iron through netherite |
| `armors`, `armor` | Four armour contracts |
| `tools`, `tool` | Four tool contracts |
| `artifacts`, `artifact`, `relics`, `relic` | Four relics, one of them a command reward |
| Anything else | Three generic starter contracts |

That is the intended way to start a category: declare it in `config.yml`, restart, then edit what
was written. The generated trades are real and balanced against each other: worth reading before
you replace them.

## Shape

```yaml
trades:
  1:                          # page
    1:                        # trade id within the page
      item:
        material: IRON_SWORD
        name: '<gray>Iron Vanguard Blade'
        lore:
          - '<dark_gray>A training commission for frontline recruits.'
        enchantments:
          - 'UNBREAKING:1'
        time: 45
        slot: 10
        successChance: 95
        sameTimeCraftCount: 2
        reqLevel: 1
        xpReward: 32
        masteryKey: swords
        masteryXpReward: 20
      requirements:
        1:
          displayName: '<gray>Iron Ingot'
          material: IRON_INGOT
          amount: 3
        2:
          displayName: '<gray>Oak Handle'
          material: STICK
          amount: 1
  2:                          # page 2
    1:
      item: { … }
```

**Two levels of number.** The outer key is a page in the category menu; the inner one is the trade's
id on that page. Neither has to be contiguous, and the trade id only has to be unique within its
page.

Field-by-field reference: [Trades](../features/trades.md) and
[Requirements](../features/requirements.md).

## The database key

A trade's identity in the database is `<category>.<page>.<id>`. That string is what
`pending_trades`, `trade_events` and `trade_daily_stats` store.

<Callout type="danger" title="Moving a trade between pages breaks its pending crafts">

Change a trade's page or id and its key changes with it. Existing pending crafts now point at a
trade that does not resolve, and with `removePendingInvalidTrade: true`, the default, they are
discarded on the next start. Its analytics history also splits across two keys.

Renumber during a maintenance window with no crafts running, or accept the loss deliberately.

</Callout>

## Failure modes at load

| Log line | Cause |
|---|---|
| `Generated category file: …` | The file was missing and has been written |
| `Please add a trades config file. File: categories/x.yml` | The file exists but has no `trades` |
| A hook usage message, then the trade is skipped | A requirement or reward names a hook that is disabled, or an item id that does not resolve |

A trade with a broken requirement is skipped **whole**, not partially loaded. Check the console
after every edit; a trade missing from the menu is almost always a line in the log.

## Two categories cannot share a file

Each is loaded under its own key and the trades take their mastery key and database prefix from it.
Pointing two categories at one file gives you duplicate trades with conflicting identities. Copy the
file instead.
