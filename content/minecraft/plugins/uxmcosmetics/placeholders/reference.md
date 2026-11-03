---
title: Reference
order: 801
description: Every %uxmcosmetics_...% placeholder and what it answers.
icon: list
---

PlaceholderAPI is required; the identifier is `uxmcosmetics`. `<family>` is `trail`, `halo`, `cloak`, `hat`, `suit`,
`emote`, `balloon`, `gadget`, `pet`, `mount`, `morph` or `banner`.

| Placeholder | Answer |
|---|---|
| `%uxmcosmetics_equipped_<family>%` | The name of what the player wears in the family, in their language, or empty |
| `%uxmcosmetics_equipped_id_<family>%` | Its id, or empty |
| `%uxmcosmetics_owned%` | How many pieces the player owns |
| `%uxmcosmetics_owned_<family>%` | How many they have in one family |
| `%uxmcosmetics_total%` | How many pieces the server has |
| `%uxmcosmetics_total_<family>%` | How many in one family |

`owned` and `owned_<family>` count pieces owned in the database; a piece held only by node is not counted. The
wardrobe is the copy kept for online players, so a player who is not online answers empty or 0.

An unknown placeholder is left as written, which shows a typo.

## Examples

```
Pet: %uxmcosmetics_equipped_pet%
Collection: %uxmcosmetics_owned%/%uxmcosmetics_total%
```

Related: [PlaceholderAPI](../integrations/placeholderapi.md)
