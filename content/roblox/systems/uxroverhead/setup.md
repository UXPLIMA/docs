---
title: Setup
order: 1
description: Install, replace the example ids, and get a nametag showing your own ranks.
icon: play
---

Install with the standard four steps in
[Installing a system](../platform/installation.md): drag `uxrOverheadSystem` into
**ServerScriptService** and press Play once.

Unlike most systems here, this one shows something immediately: press Play and you have a
nametag.

## 1. Look at what you get

Team, display name, rank and a health bar, with a row of icons. The default Roblox name and
health display is hidden.

Your rank will read **Unranked**, because the shipped rank list matches a group you are not
in.

## 2. Replace the ids

This is the important step. Three files ship with example ids that belong to somebody else.

| File | What to replace |
|---|---|
| `Config/Ranks.luau` | The group id `33106532` and the user id in the developer entry |
| `Config/Nametags.luau` | The same group id, plus two `123456` placeholders |
| `Config/Settings.luau` | `LevelSystemName`, if you use the level system |

<Callout type="danger" title="Leaving the example ids in is not harmless">

They are real ids. A player who happens to be in group `33106532` at the right rank gets a
Developer badge on your server.

Go through every `CheckTypes` block before you publish.

</Callout>

## 3. Write one rank of your own

```lua
{
    Priority = 10,
    RankName = "Member",
    RankColor = "00b4d8",
    NameColor = "ffffff",
    TeamColor = "0353a4",
    CheckTypes = { Group = { ID = <your group> } },
},
```

Press Play. If you are in that group, your nametag reads Member in your colours.

If it does not, turn `Settings.Debug` on: it logs which conditions were evaluated and what
they returned.

## 4. Add a badge

```lua
Custom = {
    { Name = "VIP", IconId = <your image>, LayoutOrder = 9,
      CheckTypes = { Gamepass = { ID = <your gamepass> } } },
},
```

Remember the gamepass cache: a purchase takes up to `GamepassCacheTTL` seconds, one minute
by default, to appear.

## 5. Adjust the layout

`Config/Layout.luau` is the row list. Remove the team row, reorder the name and rank, or
add level text:

```lua
{ Template = "RankLabel", Content = "Lv. {Level} {RankName}", TextColor = "{RankColor}", Order = 5 },
```

`{Level}` needs the uxrLevel system. Without it, it resolves to nothing and the label reads
`Lv.  Member`.

## 6. Check it on a phone

The device icon should change. That confirms the client-side detection is reaching the
server, which is also what drives the AFK state.

## 7. Try going AFK

Click outside the Studio window. Your nametag should show an AFK timer and your character
should turn transparent.

Both are switchable in [Settings](settings.md).

## 8. Publish

Nothing here needs the live game to work: conditions, ranks and tags all behave the same in
Studio, subject to their caches.
