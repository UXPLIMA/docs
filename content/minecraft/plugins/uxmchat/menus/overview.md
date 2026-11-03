---
title: Overview
order: 501
description: The ten window files, who opens them, the shared layout, sounds and Bedrock forms.
icon: layout-template
---

| Window | File | Opened by | Rows |
|---|---|---|---|
| [Settings](settings.md) | `menus/settings.conf` | `/chat`, `/chat settings` | 4 |
| [Recent](recent.md) | `menus/recent.conf` | `/recent` | 3 |
| [Friends](friends.md) | `menus/friends.conf` | `/friend` | 6 |
| [Emoji](emojis.md) | `menus/emojis.conf` | `/emoji` | 6 |
| [Colours](colours.md) | `menus/colours.conf` | `/chatcolor` | 6 |
| [Tag categories](tags.md) | `menus/tag-categories.conf` | `/tag` | 6 |
| [Tags](tags.md#a-category) | `menus/tags.conf` | a category | 6 |
| [Editor list](editor.md) | `menus/editor.conf` | `/chateditor ...` | 6 |
| [Editor lines](editor.md#lines) | `menus/editor-list.conf` | a list field | 6 |
| [Editor properties](editor.md#properties) | `menus/editor-properties.conf` | an object | 6 |

Every window has a grey pane behind every free square, the bottom row included. The six-row windows page their lists
with arrows at slots 48 and 50.

## Sounds

`menu.sounds` in [config.conf](../config/config-conf.md#menus-and-filtering-surfaces) is the sound of every window:
`click` (a button that does something), `page` (a page arrow) and `denied` (a button the player may not use). A button
whose own actions play a sound plays that sound alone.

## Bedrock

A Bedrock player gets each window as a form with the same choices. A snapshot reads as one line per stack.

## Changing a window

Every window is a file: move a slot, change a material, take a button out, and the plugin draws what you wrote. See
[Menu files](../config/menu-files.md). `/uxmchat reload` reads them again.

Related: [Menu files](../config/menu-files.md)
