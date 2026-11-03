---
title: hologram.conf
order: 512
description: The sign over a plot, its lines, themes and the full appearance block.
icon: type
---

| Key | Default |
|---|---|
| `enabled` | true |
| `height` | 2.5 |
| `lines` | Name, owner, rating, visits, tags, honours, tips, pinned note |
| `themes` | `billboard`, `plain`, `far` |
| `appearance.billboard` | `CENTER` (or `FIXED`, `VERTICAL`, `HORIZONTAL`) |
| `appearance.see-through` | false |
| `appearance.text-shadow` | true |
| `appearance.line-width` | 200 |
| `appearance.view-range` | 1.0 |
| `appearance.scale` | 1.0, and `scale-x`, `scale-y`, `scale-z` |

Off until written: `glow`, `background`, `text-opacity`, `brightness` (`block`, `sky`), `alignment`, `shadow-radius`,
`shadow-strength`, `translate-x`/`y`/`z`, `yaw`, `pitch`. A theme writes only what it changes and reads the rest from
`appearance`; a plot whose theme you delete goes back to `appearance`.

A line may use `%plot_id%`, `%plot_world%`, `%plot_owner%`, `%plot_name%`, `%plot_members%`, `%plot_banned%`,
`%plot_rating%`, `%plot_votes%`, `%plot_visits%`, `%plot_tags%`, `%plot_age_days%`, `%plot_level%`,
`%plot_level_points%`, `%plot_level_next%`, `%plot_tips%`, `%plot_honours%`, `%plot_honours_best%`, `%plot_note%` and
`%plot_note_author%`, or be `@key` for a line of the language files.

Related: [Boards and signs](../features/boards-and-signs.md#the-plot-sign)
