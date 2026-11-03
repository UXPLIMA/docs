---
title: Gallery and boards
order: 503
description: The gallery with its pinned and sponsored rows, sorting, search and filters, and the boards window.
icon: images
---

## Gallery

`/plot browse`.

| Slots | Holds |
|---|---|
| 0 to 8 | The pinned row, in the order staff added |
| 9 to 17 | The sponsored row, shuffled on every draw |
| 18 to 44 | Every visible build, in the order chosen |
| 47 | Sort: left for the next order, right for the previous |
| 48 | Search: left asks for a word (in chat, by `text-input.conf`), right clears it |
| 49 | Unrated: left shows only builds nobody has rated, right clears |
| 50 | Tag `medieval`: left filters by it, right clears |
| 51 | Random: goes to a build you have not seen |

Left click on a build goes there; right click opens its panel without going. No sort, filter or search reaches the
pinned and sponsored rows.

| Token | Means |
|---|---|
| `%plot_id%`, `%plot_world%`, `%plot_owner%`, `%plot_name%` | Where it is and whose |
| `%plot_rating%`, `%plot_votes%`, `%plot_visits%`, `%plot_tags%`, `%plot_age_days%` | What people made of it |
| `%gallery_found%` | How many builds match |
| `%gallery_sort%`, `%gallery_sort_word%` | The order, as its id and in the reader's language |

Filters a button may set with `list-filter:plots:gallery:<key>=<value>`: `world=<name>`, `tag=<a,b,c>` (every one of
them), `search=<words>` (a part of the name or the owner's) and `unrated=true`. Copy the tag button for other tags.

## Boards

`/plot leaderboard`. Lines 0 to 44, the top of one board; a click on a line goes to that build. The bottom row picks
the board:

| Slot | Board |
|---|---|
| 45 | `rating` |
| 46 | `trending` |
| 47 | `visits` |
| 48 | `tips` |
| 50 | `level` |
| 51 | `honours` |

Tokens: `%board_place%`, `%board_name%` (the plot's name, or its owner's), `%board_value%`, `%board_plot%`,
`%board_showing%`.

Related: [Gallery](../features/gallery.md), [Boards and signs](../features/boards-and-signs.md)
