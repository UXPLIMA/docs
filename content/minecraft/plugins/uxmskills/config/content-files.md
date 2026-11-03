---
title: Content files
order: 605
description: Skill, source, ability, stat and resource files.
icon: folder
---

Every content file is one thing; its file name is its id. Copy one to make another, delete one to remove it. A name,
description or badge is `@key`, a block per language, or one text.

## Skills

`skills/<id>.conf`: `enabled`, `display`, `maximum-level`, `curve`, `stat-points`, `abilities`, `sources`,
`earn-conditions`, `capstone`, `gates`, `loot`. See [Skills](../features/skills.md).

## Sources

`sources/<id>.conf`: `default` and `sources` blocks; each entry `type`, `blocks`, `states`, `after-states`, `support`,
`tool`, `trigger`, `scale-with-health`, `experience`, `money`, `multiplier`, `chance`, `cooldown-ms`. See
[Sources](../features/sources.md).

## Abilities

`abilities/<id>.conf`: `kind`, `unlock-level`, `level-interval`, `maximum-level`, `base-value`, `value-per-level`,
`level-formula`, `value-formula`, `conflicts`, `chance`, `cooldown-ms`, `cost`, `point-cost`, `point-cost-per-level`.
See [Abilities](../features/abilities.md).

## Stats

`stats/<id>.conf`: `enabled`, `display`, `value`, `traits`. See [Stats](../features/stats.md).

## Resources

`resources/<id>.conf`: `enabled`, `display`, `maximum`, `regeneration`, `period-ms`, `starts-full`.

Related: [The editor](../features/editor.md)
