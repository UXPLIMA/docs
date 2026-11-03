---
title: economy.conf
order: 603
description: The economy block, Treasury's wait, placeholder currencies and bridges.
icon: wallet
---

Everything sits in one `economy` block.

| Key | Default | Meaning |
|---|---|---|
| `treasury-wait-ms` | `250` | How long one Treasury call may take before the plugin gives up and takes nothing |
| `placeholder` | `{}` | Currencies made of a balance placeholder and a take command |
| `bridges` | `{}` | Economies reached through their Java API, described here |

The file's comments list every currency name a crate can write. The two blocks and every currency name are described
on [Economies](../integrations/economies.md).

Related: [Economies](../integrations/economies.md)
