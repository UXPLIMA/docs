---
title: Section files
order: 503
description: The keys of shops/<section>.conf.
icon: file-text
---

| Level | Keys |
|---|---|
| Section | `order`, `menu`, `permission`, `buy-permission`, `sell-permission`, `currency`, `icon`, `name`, `description`, `band`, `rotation` |
| `lines.<id>` | `order`, `type`, `item`, `icon`, `buy`, `sell`, `match`, `permission`, `conditions`, `actions`, `floor`, `ceiling`, `band`, `stock`, `currency`, `name`, `description`, and `section` on a shortcut |
| `band` | `enabled`, `move-per-trade`, `ceiling`, `floor`, `normalisation`, `normalisation-interval` |
| `rotation` | `enabled`, `size`, `every` |
| `stock.<buy\|sell>` | `server` (`enabled`, `capacity`, `refill`, `every`), `player` (`enabled`, `cap`, `every`) |

`blocks.conf` and `tools.conf` ship. What each key does is in [Sections and lines](../features/sections-and-lines.md),
[Prices](../features/prices.md) and [Stock and quotas](../features/stock-and-quotas.md).

Related: [The editor](../features/editor.md)
