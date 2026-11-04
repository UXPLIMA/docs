---
title: Reference
order: 801
description: Every %uxmbedrock_...% placeholder and what it renders.
icon: braces
---

PlaceholderAPI is required; the identifier is `uxmbedrock`, and `modules.placeholders` must be on.

| Placeholder | Renders | Offline |
|---|---|---|
| `%uxmbedrock_platform%` | `bedrock` or `java` | `java` |
| `%uxmbedrock_is_bedrock%` | `true` or `false` | `false` |
| `%uxmbedrock_device%` | The device family in lower case: `android`, `ios`, `windows`, `xbox`, `playstation`, `switch`, `macos`, `fire_os`, `vr`, `other`, `unknown`; `none` for Java | `none` |
| `%uxmbedrock_input%` | `keyboard_mouse`, `touch`, `controller`, `vr`, `unknown`; `none` for Java | `none` |
| `%uxmbedrock_ui%` | `classic` or `pocket`; `none` for Java | `none` |
| `%uxmbedrock_click%` | The reader's word for click: tap for touch, press for a controller, click otherwise | click |
| `%uxmbedrock_safe_<placeholder>%` | Another placeholder in colours Bedrock draws: `%uxmbedrock_safe_vault_prefix%` gives `%vault_prefix%` with hex colours turned into the nearest legacy colour. Needs `safe-placeholder` | empty |

`%rel_uxmbedrock_click%` is the relational form: the viewer's word, for text one player reads about another.
