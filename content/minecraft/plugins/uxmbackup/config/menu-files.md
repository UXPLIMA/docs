---
title: Menu files
order: 503
description: The keys of menus/archives.conf, the tokens and the actions.
icon: layout-template
---

`menus/archives.conf` is the archives window. `/uxmbackup reload` reads it again.

| Key | Meaning |
|---|---|
| `title` | `@menu.title` |
| `rows` | `6` |
| `placeholders.recent` | How many runs the window reads, `112` |
| `placeholders.icon-archive`, `icon-running` | `CHEST` and `CLOCK` |
| `placeholders.role-done`, `role-partial`, `role-running`, `role-failed` | The title colours |

## The runs list

```hocon
runs {
  slots = ["10-16", "19-25", "28-34", "37-43"]
  list {
    source = "backup:runs"
    template {
      material = "%run_icon%"
      name = " "
      lore = ["tile:%run_role% @menu.%run_block% set size when %run_state% storages"]
      click {
        left {
          requirements = ["backup:may-verify"]
          click = ["backup:verify", "close", "sound:UI_BUTTON_CLICK 0.5 1.6"]
          deny  = ["backup:refuse", "sound:BLOCK_NOTE_BLOCK_BASS 0.6 0.9"]
        }
      }
    }
  }
}
```

| Name | Kind | Does |
|---|---|---|
| `backup:runs` | Source | The recent runs |
| `backup:may-verify` | Condition | The player holds `uxmbackup.verify` |
| `backup:verify` | Action | Verifies the run's archive |
| `backup:refuse` | Action | Says the player may not |
| `backup:nothing-yet` | View | Shows a tile only while no run exists |

Tokens: `%run_icon%`, `%run_role%`, `%run_block%`, `%run_state%`. The `ring` tile frames the window; `empty` shows
`Nothing yet`.

Related: [Archives](../menus/archives.md)
