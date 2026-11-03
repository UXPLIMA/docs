---
title: PlaceholderAPI
order: 904
description: The uxmcrates placeholders, and other plugins' placeholders in a requirement line.
icon: braces
---

With PlaceholderAPI installed:

- uxmCrates publishes the [`%uxmcrates_...%` placeholders](../placeholders/reference.md), and `/uxmcrates doctor`
  says `published`.
- A requirement can compare any placeholder: `"placeholder %vault_eco_balance% >= 1000"`. A placeholder nobody
  answers is compared as its own text, which refuses.
- A [placeholder currency](economies.md#placeholder-currencies) reads its balance.

A requirement that reads an mcMMO skill, `%mcmmo_level_<skill>%`, is answered from mcMMO itself and works without
PlaceholderAPI.

Without PlaceholderAPI the rest of the plugin works; `/uxmcrates doctor` warns that the placeholders are not published.

Related: [Placeholders](../placeholders/reference.md)
