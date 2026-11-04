---
title: The preview page
order: 1309
description: A web page that shows the glyph pages and places menu pictures.
icon: globe
---

The preview page shows each glyph page of the last build, and each menu picture over the Bedrock chest panel, where you
move and size it and save. Saving writes the picture's place to gui.conf and builds the pack again.

## Turning it on

1. Set `preview-page = true` under `modules` in [config.conf](../config/config-conf.md).
2. Check `preview.host` and `preview.port`, then restart.
3. Run `/uxmbedrock preview` and open the link.

| Key | Default | Does |
|---|---|---|
| `preview.host` | `127.0.0.1` | The address it listens on. `127.0.0.1` keeps it on the machine; reach it through an SSH tunnel |
| `preview.port` | `8170` | The port; `0` for any free one |
| `preview.address` | empty | The address the link names, when the page is reached through another name |

The link carries a secret made at every start; a request without it gets nothing. Only the glyph pages and menu
pictures of the last build are served, and a save is taken only for a picture and a screen the build knows.
