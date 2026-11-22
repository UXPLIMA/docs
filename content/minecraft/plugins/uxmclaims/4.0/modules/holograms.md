---
title: Holograms
order: 1309
description: Floating text above each claim block, and the hologram service other modules draw with.
icon: text
---

The holograms module shows a few lines of land information above every placed claim block: the land's name,
owner, chunks, members and creation date. Other modules, such as [Leaderboards](leaderboards.md), use the same
module to draw their own floating text. There is nothing to type: holograms appear and update by themselves.

Module `holograms` · enabled by default · `modules/holograms/config.yml` · requires [Lands](lands.md)

## Settings

| Key | Default | What it does |
|---|---|---|
| `display.backend` | `auto` | `text-display`, `armor-stand` or `auto`. `auto` uses text displays on 1.19.4 and newer, armor stands before. Asking for text displays on an older server falls back to armor stands with a warning. |
| `display.height` | `1.4` | Height above the claim block, in blocks. |
| `display.view-range` | `16` | Distance in blocks from which the hologram is seen. |
| `display.see-through` | `false` | Whether the text shows through blocks. |
| `display.alignment` | `center` | `left`, `center` or `right`. |
| `display.billboard` | `center` | How the text turns to the viewer: `fixed`, `horizontal`, `vertical` or `center`. |
| `display.line-width` | `200` | Width before a line wraps. |
| `display.scale` | `1.0` | Text size. |
| `display.text-opacity` | `0.98` | `0.0` to `1.0`. |
| `display.background.use-default` | `true` | `true` uses the client's default background; `false` uses the colour below. |
| `display.background.color` | `'#000000'` | `#RRGGBB` or `#RRGGBBAA`, used when `use-default` is off. |
| `display.shadow.enabled` | `true` | Shadow under the text. |
| `display.shadow.radius` | `0.2` | Shadow radius. |
| `display.shadow.strength` | `1.0` | Shadow strength. |
| `update.interval-seconds` | `5` | Seconds between checks: a hologram comes back after its chunk loads, and changed text is applied. Land changes (rename, members, owner) are applied at once anyway. |
| `update.only-when-viewed` | `true` | Only spawn and update holograms that have a player within view range. |
| `show-for.personal` | `true` | Personal lands get a hologram. |
| `show-for.town-home` | `true` | A town's home land gets a hologram. |
| `show-for.outpost` | `true` | Town outposts get a hologram. |
| `show-for.admin` | `false` | Admin lands get a hologram. |

The look settings (alignment, billboard, line width, scale, opacity, background and shadow) apply to text
displays. Armor stands show plain name tags.

## Hologram text

The lines are in the language file, `modules/holograms/lang/<language>/messages.yml`, under `lines`. They are
MiniMessage, and an empty list shows no hologram at all.

| Tag | Renders |
|---|---|
| `<land_name>` | The land's name. |
| `<owner_name>` | The owner: a player name, a town name, or `owner-server` for admin lands. |
| `<members>` | Number of members. |
| `<chunks>` | Number of claimed chunks. |
| `<created>` | Creation date, `yyyy-MM-dd`. |
| `<kind>` | `personal`, `town_home`, `outpost` or `admin`. |
| `<block>` | The claim block's material key. |

## Notes

- Claim block holograms only exist while the [Claim Block](claim-block.md) module runs. Without it, the module
  still runs for other modules' holograms.
- Hologram entities are never saved with the world. They are spawned again after a restart or chunk load, so
  disabling the module leaves nothing behind.
- Holograms are written in the server language, not each viewer's own language.
- `/uxm reload` restyles every hologram and rewrites every claim block's lines. Changing `display.backend`
  needs a restart.
- In network mode, a rename or claim block move on another server is not shown here until the next land event
  or restart.

Related: [Claim Block](claim-block.md), [Leaderboards](leaderboards.md), [Lands](lands.md)
