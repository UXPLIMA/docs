---
title: Travel and social windows
order: 505
description: Homes, warps, the warp directory, the social menu, guestbook, bookmarks, alliances and the activity feed.
icon: map-pin
---

## Homes

The file is `menus/island-homes.conf`. The window is titled "Homes" and has 3 rows.
The homes of the player, the window the homes tile of the island menu opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10 | `COMPASS` | Your homes: Every place you have marked, and where each one is. | runs `/is homes list` |
| 12 | `RED_BED` | Set a home: Marks where you stand, so you can come straight back to it. | asks you to type, runs `/is sethome %input%` |
| 14 | `ENDER_PEARL` | Island spawn: Takes you to where your island begins. | runs `/is home` |
| 16 | `BEACON` | Move the spawn: Makes where you stand the place every member and visitor arrives at. | runs `/is setspawn` |
| 22 | `FEATHER` | « Back | opens the [island menu window](island.md#island-menu) |

## Home list

The file is `menus/island-home-list.conf`. The window is titled "Your homes" and has 4 rows.
Your homes, one tile each: the window the list button of the homes menu and /is homes list open.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `RED_BED` | Your homes: Every place you marked to come back to. Mark another from the homes menu. |  |
| 10-16, 19-25 | `WHITE_BED` | One tile for each entry. A place you marked to come back to. | Left: takes you to the home; Right: takes you to the home; Shift right: deletes the home |
| 30 | `ARROW` | ← Previous page |  |
| 30 | `ARROW` | ← No page before this one |  |
| 31 | `FEATHER` | « Back | opens the [homes window](#homes) |
| 32 | `ARROW` | → Next page |  |
| 32 | `ARROW` | → No page after this one |  |

## Warps

The file is `menus/island-warps.conf`. The window is titled "Warps and travel" and has 3 rows.
Warps and travel, the window the warps tile of the island menu opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10 | `COMPASS` | Your warps: The warps your island has opened. | runs `/is warp list` |
| 12 | `ENDER_EYE` | Set a warp: Marks where you stand as a warp other players can travel to. | asks you to type, runs `/is warp create %input%` |
| 14 | `ENDER_PEARL` | Visit an island: Takes you to another player's island, if they let visitors in. | asks you to type, runs `/is visit %input%` |
| 16 | `MAP` | Open warps: Every warp on this server that is open to visitors. | runs `/is warp browse` |
| 22 | `FEATHER` | « Back | opens the [island menu window](island.md#island-menu) |

## Warp directory

The file is `menus/island-warp-directory.conf`. The window is titled "Island warps" and has 6 rows.
The warp directory: every warp an island opened to visitors.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25, 28-34 | `%entry_material%` | One tile for each entry. A place an island opened to visitors. | takes you to the warp |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 49 | `FEATHER` | « Back | opens the [warps window](#warps) |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |

## Social

The file is `menus/island-social.conf`. The window is titled "Social" and has 5 rows.
The social side of the island, the window the social tile of the island menu opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 11 | `WRITABLE_BOOK` | Guestbook: What visitors wrote about your island. | runs `/is guestbook` |
| 13 | `NETHER_STAR` | Rate an island: Gives the island you stand on a score out of five. | runs `/is rate` |
| 15 | `MAP` | Bookmarks: The islands you saved to come back to. | runs `/is bookmarks` |
| 20 | `SHIELD` | Alliances: The islands yours is allied with. Allies do not hurt each other. | runs `/is alliance` |
| 22 | `PAPER` | Island chat: Talk to the members of your island only. | runs `/is chat` |
| 24 | `CLOCK` | Activity: What happened on your island lately. | runs `/is activity` |
| 40 | `FEATHER` | « Back | opens the [island menu window](island.md#island-menu) |

## Guestbook

The file is `menus/island-guestbook.conf`. The window is titled "Guestbook" and has 4 rows.
The guestbook of the island you stand on, the window /is guestbook opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `FEATHER` | Sign the book: Leave a line for the island you are standing on. | asks you to type, runs `/is guestbook sign %input%` |
| 10-16, 19-25 | `%entry_material%` | One tile for each entry. A line in the book | Left: pins or unpins the entry; Right: hides or shows the entry; Shift right: deletes the entry |
| 30 | `ARROW` | ← Previous page |  |
| 30 | `ARROW` | ← No page before this one |  |
| 32 | `ARROW` | → Next page |  |
| 32 | `ARROW` | → No page after this one |  |
| 31 | `FEATHER` | « Back | opens the [social window](#social) |

## Bookmarks

The file is `menus/island-bookmarks.conf`. The window is titled "Your bookmarks" and has 4 rows.
The islands you bookmarked, the window /is bookmarks opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 10-16, 19-25 | `GRASS_BLOCK` | One tile for each entry. An island you marked to come back to. | visits the island |
| 30 | `ARROW` | ← Previous page |  |
| 30 | `ARROW` | ← No page before this one |  |
| 32 | `ARROW` | → Next page |  |
| 32 | `ARROW` | → No page after this one |  |
| 31 | `FEATHER` | « Back | opens the [social window](#social) |

## Alliances

The file is `menus/island-alliances.conf`. The window is titled "Alliances" and has 5 rows.
Your island's allies and the offers of alliance waiting for you, the window /is alliance opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `WRITABLE_BOOK` | Offer an alliance: Allies visit each other's islands as friends. | asks you to type, runs `/is alliance invite %input%` |
| 10-16 | `SHIELD` | One tile for each entry. An island allied with yours. | Left: visits the island; Right: visits the island; Shift right: breaks the alliance |
| 28-34 | `PAPER` | One tile for each entry. This island offers your island an alliance. | Left: accepts the alliance; Right: turns the alliance down |
| 39 | `ARROW` | ← Previous page |  |
| 39 | `ARROW` | ← No page before this one |  |
| 41 | `ARROW` | → Next page |  |
| 41 | `ARROW` | → No page after this one |  |
| 40 | `FEATHER` | « Back | opens the [social window](#social) |

## Activity

The file is `menus/island-activity.conf`. The window is titled "Lately on your island" and has 6 rows.
What happened on your island lately, the window /is activity opens.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 0 | `NAME_TAG` | One member: Show only what one member did. | Left: shows the next member; Right: shows every member |
| 2 | `BOOK` | Everything: Show every kind of line. | shows that kind of line |
| 3 | `GOLD_INGOT` | The bank: Show only what went into the bank and came out of it. | shows that kind of line |
| 4 | `PLAYER_HEAD` | Members: Show who joined, left, changed role or was trusted. | shows that kind of line |
| 5 | `EXPERIENCE_BOTTLE` | Progress: Show upgrades, boosters, missions and warps. | shows that kind of line |
| 8 | `GOLD_BLOCK` | The bank by member: What each member put in and took out, under the same filters. | opens the bank by member |
| 10-16, 19-25, 28-34 | `PAPER` | One tile for each entry. On your island |  |
| 37 | `CLOCK` | All of it: Show everything the feed still keeps. | shows that period |
| 38 | `CLOCK` | The last day: Show what happened in the last 24 hours. | shows that period |
| 39 | `CLOCK` | The last week: Show what happened in the last 7 days. | shows that period |
| 40 | `CLOCK` | The last month: Show what happened in the last 30 days. | shows that period |
| 48 | `ARROW` | ← Previous page |  |
| 48 | `ARROW` | ← No page before this one |  |
| 50 | `ARROW` | → Next page |  |
| 50 | `ARROW` | → No page after this one |  |
| 49 | `FEATHER` | « Back | opens the [social window](#social) |

## Bank by member

The file is `menus/island-activity-totals.conf`. The window is titled "The bank by member" and has 5 rows.
What each member put in and took out of the island bank, the window the feed's totals button opens. It reads the same lines the feed shows: the member, the kinds and the stretch of time the player narrowed the feed to narrow these totals too.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 4 | `HOPPER` | Filters: These totals read the lines the feed shows. | opens the activity lines |
| 19-25, 28-34 | `%entry_material%` | One tile for each entry.  |  |
| 39 | `ARROW` | ← Previous page |  |
| 39 | `ARROW` | ← No page before this one |  |
| 41 | `ARROW` | → Next page |  |
| 41 | `ARROW` | → No page after this one |  |
| 40 | `FEATHER` | « Back | opens the activity lines |

## Trade

The file is `menus/player-trade.conf`. The window is titled "Trade with" and has 6 rows.
The window two players trade in, one each, opened when one answers the other's /is trade.

| Slot | Item | What it shows | Click |
|---|---|---|---|
| 0 | `RED_CONCRETE` | Click to agree to this trade. | agrees to the trade or takes it back |
| 0 | `LIME_CONCRETE` | You agree. Click to take it back. | agrees to the trade or takes it back |
| 4 | `BARRIER` | Call the trade off | calls the trade off |
| 8 | `RED_STAINED_GLASS_PANE` | The other player does not agree yet. |  |
| 8 | `LIME_STAINED_GLASS_PANE` | The other player agrees. |  |
