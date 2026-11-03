---
title: The claim itself
order: 204
description: Name, words, looks, spawn, flags and time.
icon: flag
---

| Command | Turkish | Does |
|---|---|---|
| `/claim rename <name>` | `adlandir` | Renames the claim, `claims.name-min` to `name-max` letters |
| `/claim describe [text]` | `aciklama` | Describes it; no text clears it |
| `/claim greeting [text]` | `karsilama` | What visitors read when they arrive |
| `/claim farewell [text]` | `veda` | What visitors read when they leave |
| `/claim icon` | `simge` | Makes the item you hold its icon |
| `/claim colour <colour>` | `renk` | Its map colour, as `#rrggbb` |
| `/claim category <category>` | `kategori` | One of `claims.categories` |
| `/claim setspawn` | `noktakoy` | Where teleports to it land |
| `/claim flag <flag> [value]` | `ayar` | Changes a flag; needs `uxmsimpleclaims.flag.<flag>` |
| `/claim extend <days>` | `uzat` | Pays for more days of a time limited claim |

Texts are at most `claims.text-max` letters. Every change needs the role permission `edit-info`, `set-spawn` or
`manage-flags`.

Related: [Claim window](../menus/claim.md)
