---
title: Integrations
order: 290
description: Every hook, what it adds, and the two-step enable.
icon: plug
---

Every integration is a `softdepend` (nothing is required) and every one is a switch under
`settings.Hooks` in `config.yml`. Installing the plugin is not enough; the hook must be **on**.

```yaml
settings:
  Hooks:
    MONEY: true
    PLACEHOLDER: true
    NEXO: true
    SAVE: true
    SKULL: true
    XP: true
    PLAYERPOINTS: false
    COINSENGINE: false
    MMOITEMS: false
    ECOITEM: false
    EXECITEM: false
    IAITEM: false
```

On start, each enabled hook is registered if its plugin is present, and the console says so:

```
Hook MONEY is enabled!
```

A hook that is on but whose plugin is missing is simply not registered. A trade whose requirement
names an unregistered hook is **skipped whole** at load, with a reason in the log.

## Economies

| Hook | Plugin | Use in a trade |
|---|---|---|
| `MONEY` | Vault | `material: MONEY` |
| `PLAYERPOINTS` | PlayerPoints | `material: PLAYERPOINTS` |
| `COINSENGINE` | CoinsEngine | `material: 'COINSENGINE:<currency>'` |
| `XP` | built in | `material: XP` |

`COINSENGINE` needs the currency after a colon. An unknown currency is reported at load: check the
console after adding one.

`XP` is vanilla experience and needs no plugin at all. It is a good second requirement on a trade
that already costs items: it is a resource every player has and none can trade.

An economy requirement is withdrawn when the trade starts, and returned by a refund on cancel.

## Custom items

| Hook | Plugin | `customItemID` | `type` |
|---|---|---|---|
| `MMOITEMS` | MMOItems | The item id | The MMOItems type (required for the display name) |
| `ECOITEM` | EcoItems | The EcoItems id | n/a |
| `EXECITEM` | ExecutableItems | The item id | n/a |
| `IAITEM` | ItemsAdder | The item id | n/a |
| `NEXO` | Nexo | The item id | n/a |
| `SKULL` | built in | A player name | n/a |
| `SAVE` | built in | A saved item key | n/a |
| `CUSTOMREQ` | built in | An editor profile id | n/a |

Each works as both a requirement and a reward. Use them like this:

```yaml
item:
  material: MMOITEMS
  customItemData:
    customItemID: DRAGON_HELMET
    type: ARMOR
```

**`IAITEM` and `NEXO` are interchangeable.** A trade written for `IAITEM` uses Nexo if only Nexo is
installed, and the reverse. You do not have to rewrite your configs to migrate between them.

## PlaceholderAPI

`PLACEHOLDER`, on by default. Two directions:

**Out**: the plugin registers the `uxmblacksmith` expansion. See [Placeholders](placeholders.md).

**In**: `material: PLACEHOLDER` requirements evaluate any expression you can write:

```yaml
requirements:
  1:
    displayName: '&eVIP Required'
    material: PLACEHOLDER
    condition: '%vault_hasgroup_VIP%'
    requiredOutput: 'yes'
```

This is the extension point for everything the plugin does not do natively: quest completion, a
rank, a stat, another plugin's level, the time of day. Install the expansions you need with
`/papi ecloud download Math`, `… Vault`, `… String`.

Requirements are covered in full on [Requirements](features/requirements.md).

## NBTAPI

A `softdepend` used for deeper item matching. There is no hook switch; it is used if present.

## The built-in stores

Three "hooks" have no plugin behind them.

**`SAVE`**: the binary item store. `/blacksmith binary save <name>` serialises the item in your hand
into `binary_files/`, and any trade can then reference it. This is how you use an item from a plugin
that has no hook: make one, save it, reference the key.

**`SKULL`**: player heads as requirements and rewards.

**`CUSTOMREQ`**: requirement profiles built in-game with `/blacksmith editor <id>`. Hold an item,
run the command, tune what must match, save. The profile id then works as
`material: 'CUSTOMREQ:<id>'`.

<Callout type="warning" title="A saved item is a snapshot, not a reference">

`SAVE` stores the bytes of the item as it was. If the plugin that produced it later changes the
item's stats, your saved copy does not follow. That is sometimes exactly what you want: a legacy
reward frozen in time, but it is not what most people expect.

</Callout>

## bStats

Anonymous usage statistics, on by default like every bStats plugin. Turn it off in
`plugins/bStats/config.yml`, which is server-wide.

## Update check

The plugin checks for a newer version on start and logs the result. It does not download anything.
