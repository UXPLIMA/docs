---
title: Tags
order: 309
description: Tags held by rank, given, bought or made by players; categories, rarities, variants, animation, defaults, surfaces and effects.
icon: tag
---

Module: `tags`. A tag is a word shown before a player's name, in chat and, with `surfaces.tab-list`, in the tab list.

## How a player holds a tag

| Way | Set by |
|---|---|
| A permission | `uxmchat.tag.<id>`, or the tag's own `permission` |
| A group | `groups`: the permission groups that hold it without the permission |
| Given | `/tag give <player> <tag>` |
| Bought | A `price`, paid with `/tag buy` or a click in the window |
| Made | `/tag make`, approved by staff |
| Everyone | `permission = ""` with no price and no groups |

`/tag wear <tag>` or a click in the [tag window](../menus/tags.md) wears it. A player who chose nothing wears the held
tag with the highest `default-priority`, so every member wears `[Member]` and a VIP `[VIP]` without choosing.

## The shipped tags

| Tag | Shows | Category | Rarity | Held by |
|---|---|---|---|---|
| `member` | `[Member]` | ranks | common | group `default`, priority 0 |
| `vip` | `[VIP]` | ranks | rare | group `vip`, priority 10 |
| `staff` | `[Staff]` | ranks | epic | groups `helper`, `moderator`, `admin`, `owner`, priority 100; hidden from others |
| `builder` | `[Builder]` | general | rare | `uxmchat.tag.builder` |
| `pvp` | `[PvP]` | shop | common | 5000 coins |
| `wizard` | `[Wizard]` | shop | epic | 25000 coins |
| `star` | `[★]`, animated | events | legendary | `uxmchat.tag.star`; hidden from others |

## Categories and rarities

The window groups tags by category: `general`, `ranks`, `shop`, `events` and `player` (player made). A category's
`permission` hides it from players without it. A rarity is a word on the tag's button: `common`, `rare`, `epic`,
`legendary`.

## Selling a tag

```hocon
price {
  currency = "coins"
  amount = "5000"
  conditions = []
  on-success = [ "[message] <good>You bought the PvP tag. Wear it with /tag wear pvp." ]
  on-failure = []
}
```

`currency` is an id from [currencies.conf](../config/currencies-conf.md). `conditions` are uxmLib condition lines
that must hold; `on-success` and `on-failure` are action lines. A currency whose plugin is missing is not ready, and a
tag priced in it cannot be bought. Node `uxmchat.tag.buy`.

## Animation and variants

`frames` lists displays shown in turn, `frame-millis` apart: the shipped `star` steps through four colours every
500 ms. `variants` are other looks of one tag, each with its own display and permission, worn with
`/tag wear <tag> <variant>`.

## Player-made tags

`/tag make <text>` writes a tag of the player's own. `player-made`:

| Key | Default | Meaning |
|---|---|---|
| `approval` | `true` | Staff approve a tag before it can be worn |
| `max-length` | `16` | The longest tag, in letters, without its colours |
| `per-player` | `3` | How many tags one player may make |
| `category` | `player` | The category they are listed under |

A made tag passes the chat filters. Staff with `uxmchat.staff.tags` are told one waits, and review them with
`/tag pending`, `/tag approve` and `/tag deny`.

## Favourites

A right click in the window, or `/tag favourite <tag>`, pins a tag to the top of its category.

## Surfaces

| Key | Default | Shows the tag |
|---|---|---|
| `wrapper` | `"<tag> "` | How a worn tag sits before the name |
| `surfaces.tab-list` | `true` | In the tab list, before the name |
| `surfaces.display-name` | `false` | In the player's display name, which other plugins also show |

For a nametag plugin, use the placeholder `%rel_uxmchat_tag_mini%`.

## Effects

| Moment | Shipped lines |
|---|---|
| `set` | `[message] @tag.worn`, a pickup sound |
| `cleared` | `[message] @tag.cleared`, an item frame sound |
| `lapsed` | `[message] @tag.lapsed`: at join, when the player no longer holds the tag or it is gone |

Leave a list out to keep the shipped one; write `[]` for silence.

## Where tags live

The tags of `tags.conf` are read once, on the first start, into the database; staff change them in game with
`/chateditor tags`, and the change reaches every server. Categories, rarities, the wrapper, surfaces, effects and the
player-made rules are read on every reload.

Related: [tags.conf](../config/tags-conf.md), [Tag windows](../menus/tags.md), [Migration](../integrations/migration.md)
