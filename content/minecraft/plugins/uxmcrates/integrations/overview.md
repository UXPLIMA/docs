---
title: Overview
order: 901
description: Every plugin uxmCrates works with, what it adds, and what happens when it is missing.
icon: plug
---

uxmCrates loads on a bare Paper server. Every plugin below is reached only when it is installed.

| Plugin | Adds | Page |
|---|---|---|
| Vault, VaultUnlocked, PlayerPoints, EcoBits, Treasury | Costs and money rewards | [Economies](economies.md) |
| Any economy with a placeholder and a command, or a Java API | The same, described in `economy.conf` | [Economies](economies.md) |
| Nineteen claim plugins | The placing and opening rules | [Claims](../features/claims.md) |
| Oraxen, Nexo, ItemsAdder, CraftEngine | Custom items in content files | [Custom items](custom-items.md) |
| PlaceholderAPI | `%uxmcrates_...%`, and other placeholders in requirements | [PlaceholderAPI](placeholderapi.md) |
| mcMMO | A skill level in a requirement, read from mcMMO itself | [Crates](../features/crates.md#permission-requirements-and-cost) |
| Geyser, Floodgate | Forms for every window; a window reveal instead of a world one | [Reveals](../features/reveals.md) |
| uxmLang | One language choice for every UXM plugin | [Languages](../config/languages.md) |
| ExcellentCrates, CrazyCrates, SpecializedCrates | Their crate sets, read into files | [Migration](migration.md) |

`/uxmcrates doctor` reports the currencies, claims and placeholders it found.

Related: [Installation](../getting-started/installation.md)
