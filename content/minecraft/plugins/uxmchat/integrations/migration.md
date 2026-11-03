---
title: Migration
order: 903
description: Moving from uxm-tags and uxm-chat-color, and from an older uxmChat folder.
icon: arrow-right-left
---

## From uxm-tags and uxm-chat-color

uxm-tags and uxm-chat-color are retired: uxmChat carries tags and chat colours.

1. Install uxmChat with `uxmTags` and `uxmChatColor` still in `plugins/`.
2. Start once. The first start reads their tags, presets, worn tags, grants and colour choices, and copies the language
   lines their tags name into `messages/`. Their files are only read. `import.only` in `tags.conf` limits the tags
   brought over to the ids it lists.
3. Remove the old jars.
4. Show `%rel_uxmchat_tag_mini%` wherever `%rel_uxmtags_tag%` was.

Each import runs once and leaves a marker in the uxmChat folder: `.imported-uxmtags`, `.imported-uxmtags-words` and
`.imported-uxmchatcolor`. Delete a marker to run that import again.

| Old | New |
|---|---|
| `/tags` | `/tag` (`/tags` is an alias) |
| `%rel_uxmtags_tag%` | `%rel_uxmchat_tag_mini%` |

## From an older uxmChat folder

A folder from a release before the module folders moves on the first start: each file goes to its module folder,
`words_<language>.txt` to `modules/filters/words/`, the words and trivia of `games.conf` into their language files
(the old file stays as `games.conf.bak`), and the SQLite file into `data/`. The console names everything it moved.

Related: [Tags](../features/tags.md), [Chat colours](../features/colours.md)
