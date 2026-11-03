---
title: Reference
order: 701
description: Every %uxmplots_...% placeholder.
icon: braces
---

Registered through uxmLib when PlaceholderAPI is installed. An unknown name is left as written, so a typo shows.

## The plot you stand in

Empty when the reader stands in no plot. The same words fill the windows and the plot sign.

| Placeholder | Gives |
|---|---|
| `%uxmplots_plot_id%` | Where the plot is, such as `creative:-3;5` |
| `%uxmplots_plot_world%` | Its world |
| `%uxmplots_plot_owner%` | Who owns it, empty on a free plot |
| `%uxmplots_plot_name%` | Its name, or empty |
| `%uxmplots_plot_members%` | How many are trusted on it |
| `%uxmplots_plot_banned%` | How many are banned |
| `%uxmplots_plot_rating%` | The average out of five, empty when unrated |
| `%uxmplots_plot_votes%` | How many have rated it |
| `%uxmplots_plot_visits%` | How many different people have been there |
| `%uxmplots_plot_tags%` | Its tags, separated by commas |
| `%uxmplots_plot_age_days%` | Days since it was claimed |
| `%uxmplots_plot_level%` | Its level |
| `%uxmplots_plot_level_points%` | The points behind it |
| `%uxmplots_plot_level_next%` | Points to the next level, empty at the top |
| `%uxmplots_plot_tips%` | What it has been tipped, in every currency, or empty |
| `%uxmplots_plot_honours%` | Contests it has placed in, or empty |
| `%uxmplots_plot_honours_best%` | Its best place, or empty |
| `%uxmplots_plot_note%` | The pinned guestbook note, or empty |
| `%uxmplots_plot_note_author%` | Who wrote it |

## Contests

The running contest is the one building, locked or being judged; of several, the first by id. Empty between contests.

| Placeholder | Gives |
|---|---|
| `%uxmplots_contest_name%` | Its name |
| `%uxmplots_contest_id%` | Its file name |
| `%uxmplots_contest_phase%` | The phase, as its id |
| `%uxmplots_contest_phase_word%` | The phase, in the reader's language |
| `%uxmplots_contest_left%` | Whole minutes until the phase ends |
| `%uxmplots_contest_time_left%` | The same, written out |
| `%uxmplots_contest_entered%` | `true` when the reader has entered it |
| `%uxmplots_contest_entries%` | How many entries it holds |
| `%uxmplots_contest_<id>_<field>%` | Any of those eight for the contest of that id |

## The reader

| Placeholder | Gives |
|---|---|
| `%uxmplots_owned%` | Plots held anywhere |
| `%uxmplots_owned_here%` | Plots held in the world they stand in |
| `%uxmplots_limit%` | How many they may hold anywhere, empty when uncapped |
| `%uxmplots_in_plot%` | `true` or `false` |
| `%uxmplots_is_owner%` | Whether they own the plot they stand in |

## Boards

| Placeholder | Gives |
|---|---|
| `%uxmplots_board_<board>_<place>_name%` | The plot's name, or its owner's |
| `%uxmplots_board_<board>_<place>_value%` | What it scored |
| `%uxmplots_board_<board>_<place>_plot%` | Where it is |

`<board>` is `rating`, `trending`, `visits`, `tips`, `level` or `honours`; `<place>` counts from one. A place past the
end reads as nothing, so a scoreboard written for ten lines on a server with three shows three. A board is read from
the snapshot built every `leaderboard.refresh-seconds`.

Related: [Boards and signs](../features/boards-and-signs.md)
