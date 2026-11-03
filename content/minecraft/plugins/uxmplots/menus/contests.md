---
title: Contests, scorecard and hall of fame
order: 406
description: The contest list, the voting scorecard and the hall of fame.
icon: trophy
---

## Contests

`/contest` as a player. Every contest, 45 to a page; a click says how it stands (`plots:contest-scores`). The book at 49
closes the window and says how to enter with the build you stand on; the gold block at 45 opens the
[hall of fame](#hall-of-fame).

| Token | Means |
|---|---|
| `%contest_name%`, `%contest_id%` | What it is called, and its file name |
| `%contest_phase%`, `%contest_phase_word%` | `announced`, `building`, `locked`, `judging` or `closed`, and the same in the reader's language |
| `%contest_theme%` | What people build, once it may be read |
| `%contest_fee%`, `%contest_room%` | The fee and the free places, each empty when there is none |
| `%contest_left%`, `%contest_time_left%` | Minutes left in the phase, and the same in the reader's units |
| `%contest_standing%` | The phase and the time left |

Conditions: `plots:contest-accepting`, `plots:contest-judging`, `plots:contest-none`.

## Scorecard

`/contest vote` alone takes you to the next entry and opens its card. Each category is a line in slots 0 to 8: left
click raises it, right click lowers it, between one and five stars. Slot 13 explains the card when a contest scores in
categories (`plots:card-by-category`); 21 sends the whole card as one vote; 23 closes it and says how to reach the next
build. Tokens: `%card_category%`, `%card_stars%`, `%card_contest%`.

## Hall of fame

`/contest hall`. Every kept build. Left click puts it up on a viewing plot and walks you in (`plots:hall-view`); right
click goes where it stood (`plots:hall-visit`). 47 sorts, 48 searches (`search=` part of the contest's name or the
winning team's), 49 shows winners alone (`winners=true`); `contest=<id>` filters one contest.

| Token | Means |
|---|---|
| `%hall_contest%`, `%hall_contest_id%` | The contest, as named on the day it closed |
| `%hall_place%`, `%hall_score%` | The place, 1 for the winner, and the score |
| `%hall_winner%`, `%hall_builders%` | The team or builder, and everybody behind it |
| `%hall_plot%`, `%hall_world%`, `%hall_plot_held%` | Where it stood, and whether that ground is still claimed |
| `%hall_age_days%`, `%hall_found%`, `%hall_sort%`, `%hall_sort_word%` | Age, matches and order |

Related: [Contests](../features/contests.md)
