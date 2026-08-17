---
title: Icon tags
order: 4
description: The device icon, country flags, the chat icon, and condition-driven badges.
icon: badge-check
---

The row of small icons in a nametag. `Shared/Config/Nametags.luau` holds four groups, all
rendered into the `NametagsFrame` row.

## Device

```lua
Device = {
    Enabled = true,
    LayoutOrder = 1,
    Computer = 106290076073871,
    Phone = 94089970073947,
    Console = 139663456027187,
},
```

An icon showing what the player is playing on. The device is detected on their client and
sent to the server.

Three icons, one per platform. This is a genuinely useful tag on a competitive game, where
knowing somebody is on a phone changes how you read what they are doing.

## Country flags

```lua
CountryFlags = {
    Enabled = true,
    LayoutOrder = 99,
    Flags = {
        US = 134558926936054,
        GB = 118277527248746,
        TR = 17077448256,
        …
    },
},
```

Sixty flags ship, keyed by ISO 3166-1 alpha-2 code. The player's country comes from
`LocalizationService`.

A country with no entry shows no flag. Adding one is a key and an asset id.

<Callout type="note" title="The country is where Roblox thinks the player is, not where they are from">

`LocalizationService` reports a region derived from the connection. A player travelling, or
behind a VPN, gets a flag that surprises them.

It is a fun tag, not an identity claim. If your community would treat it as one, consider
turning it off.

</Callout>

## The chat icon

```lua
CantChat = {
    Enabled = true,
    Image = 92589085605063,
    LayoutOrder = 9999,
},
```

Shown to a viewer when **they** cannot chat with that player, usually because one of them
has an account setting restricting it.

This one is client-side and per viewer: two players looking at the same third player can
see different things, which is correct, because the restriction is between a pair.

It stops the most common confusion on a roleplay server, where somebody appears to be
ignoring you and is in fact not receiving anything.

## Custom badges

```lua
Custom = {
    { Name = "Premium",   IconId = 107334426854978, LayoutOrder = 3, CheckTypes = { Premium = true } },
    { Name = "Owner",     IconId = 102665718502699, LayoutOrder = 5, CheckTypes = { Owner = true } },
    { Name = "Developer", IconId = 127056894768945, LayoutOrder = 4,
      CheckTypes = { GroupRank = { ID = 33106532, Rank = 100 } } },
    { Name = "Vip",       IconId = 133106866048276, LayoutOrder = 9,
      CheckTypes = { Gamepass = { ID = 123456 } } },
},
```

| Field | What it is |
|---|---|
| `Name` | The tag's internal name |
| `IconId` | The image |
| `LayoutOrder` | Where it sits in the row |
| `CheckTypes` | The condition. See [Conditions](conditions.md) |

A tag is added when its condition passes and **removed** when it stops passing, so a tag
tied to a team or a level follows the player.

Unlike ranks, any number of custom tags can apply at once. A player can be Premium, a
tester and a VIP simultaneously.

## The shipped examples

Seven tags ship: Premium, Developer, Owner, Tester, Star, Mod and VIP. They demonstrate
four different check types, which makes them a useful template and a bad default.

<Callout type="danger" title="Two of the shipped tags use placeholder ids">

`Star` checks `FriendWith = { ID = 123456 }` and `Vip` checks `Gamepass = { ID = 123456 }`.
Neither is a real id for your game.

The Developer, Tester and Mod tags check ranks in group `33106532`, which is not your
group.

Review every entry before publishing. A tag whose condition is nonsense is not an error;
it just never appears, or appears for the wrong people.

</Callout>

## Ordering the row

`LayoutOrder` sorts the icons. The shipped numbers leave gaps deliberately:

| Order | Tag |
|---|---|
| 1 | Device |
| 3 to 9 | Custom badges |
| 99 | Country flag |
| 9999 | The chat icon |

Device first, flag near the end and the chat icon last. Adding a badge means picking a
number in the 3 to 9 band, or extending it.

## Turning a group off

Each of `Device`, `CountryFlags` and `CantChat` has its own `Enabled`. Setting one to
`false` removes that icon everywhere without deleting the configuration.

Custom tags have no switch: remove the entry, or give it a condition nothing satisfies.
