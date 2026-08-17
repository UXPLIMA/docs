---
title: PlaceholderAPI
order: 805
description: The uxmclaims expansion, and where its placeholders can be used.
icon: braces
---

```yaml
generalSettings:
  placeholderSupport: true
```

uxmClaims registers an expansion with the identifier `uxmclaims`. It is bundled: there is nothing to
download from the eCloud.

The complete syntax, the path rules and every modifier are in
[Placeholders → Reference](../placeholders/reference.md). This page is about where they work.

## In other plugins

Any plugin that resolves PlaceholderAPI placeholders can use them: scoreboards, tab lists, holograms,
chat formats, NPC dialogue.

```
%uxmclaims_claim_name%
%uxmclaims_claim_owner_name%
%uxmclaims_count:claim_chunks%
%uxmclaims_time:claim_remainTime%
%uxmclaims_is_claimable%
```

Which claim they describe is the one selected with `/claim select`, or failing that the one the player
is standing in.

## In uxmClaims' own files

`messages.yml`, `webhooks.yml`, the hologram lines and every menu use the same engine **without** the
`uxmclaims_` prefix, and with more objects in context:

```yaml
'<gray>Owner:</gray> <white>%claim.owner.name%</white>'
'<gray>Expires:</gray> <gold>%time:claim.remainTime%</gold>'
```

This part works with `placeholderSupport: false` and without PlaceholderAPI installed: it is the
plugin's own parser. The switch and the dependency only govern *external* placeholders.

## Third-party placeholders inside uxmClaims

With PlaceholderAPI installed, another plugin's placeholders work inside uxmClaims' messages and
menus. A hologram line can carry `%vault_eco_balance%`; a menu lore line can carry
`%player_world%`.

## Defaults outside a claim

```yaml
placeholderDefaults:
  uxmclaims_claim_name: 'No claim'
  uxmclaims_claim_owner_name: 'No claim'
```

Two are defined. Every other path renders empty outside a claim, so add an entry for anything you put
on a permanent scoreboard.

`%uxmclaims_is_claimable%` is the exception: it works anywhere, because it is answered before the
claim is resolved.
