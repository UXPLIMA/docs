---
title: PlaceholderAPI
order: 805
description: The uxmclaims expansion, which claim it describes, and its fallbacks.
icon: braces
---

uxmClaims registers a PlaceholderAPI expansion with the identifier `uxmclaims`. It is built in, so
there is nothing to download from the eCloud. The syntax and every path are on
[Placeholders reference](../placeholders/reference.md); this page covers the integration.

## Settings

```yaml
generalSettings:
  placeholderSupport: true
```

With `true` and PlaceholderAPI installed, the expansion is registered at startup. Changing the switch
needs a restart.

## In other plugins

Scoreboards, tab lists, holograms, chat formats and any other plugin that resolves PlaceholderAPI
placeholders can use them:

```
%uxmclaims_claim_name%
%uxmclaims_claim_owner_name%
%uxmclaims_count:claim_chunks%
%uxmclaims_time:claim_remainTime%
%uxmclaims_is_claimable%
```

They describe one claim, picked in this order:

| Order | Claim |
|---|---|
| 1 | The claim the player selected with `/claim select` |
| 2 | The claim the player is standing in |
| 3 | The player's only claim, when they own exactly one |

`%uxmclaims_is_claimable%` ignores that order: it is `true` when no claim covers the player's
position. It does not check WorldGuard regions or `disabledWorlds`.

## When there is no claim

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'Wilderness'
  uxmclaims_claim_owner_name: 'None'
```

When no claim is found, or a path has no value, the expansion answers with the entry for that
placeholder from `placeholderDefaults` in `messages.yml`. A placeholder without an entry is left as
typed, so add one for anything you put on a permanent scoreboard.

## Notes

- **uxmClaims' own files do not need PlaceholderAPI.** Messages, menus, holograms, webhooks and map
  tooltips use the plugin's own parser, with `%claim.name%` style paths and no `uxmclaims_` prefix.
  They work with `placeholderSupport: false`.
- **Other plugins' placeholders do not work inside uxmClaims' files.** A `%vault_eco_balance%` in a
  menu or hologram stays as typed.

Related: [Overview](overview.md), [Placeholders reference](../placeholders/reference.md), [messages.yml](../config/messages-yml.md)
