---
title: visitors.conf
order: 653
description: What visitors may do, and the visit fee.
icon: users
---

The file is `plugins/uxmSkyblock/modules/visitors.conf`. Every default below is the shipped value.

## `visitors`

Visitors

A player standing on an island they are no member of is a visitor there.

| Key | Default | What it does |
|---|---|---|
| `blocked-commands` | `sethome`, `setwarp` | The commands a visitor cannot run, each with the words after it that are blocked too: "home set" blocks /home set and leaves /home alone. A plugin's prefix, as /essentials:sethome, does not get past. |

## `visitors.fee`

The fee an island may charge a visitor who comes by /is visit. A member whose role may change the island's settings sets it with /is visitfee; the visitor is shown it, confirms, and the money goes to the island's bank. Members and staff who bypass the island protection pay nothing. Public warps stay free. Fees need an economy plugin.

| Key | Default | What it does |
|---|---|---|
| `most` | `10000.00` | The most an island may charge. 0 turns visit fees off. |
