---
title: "Migration"
order: 40
description: "Reading the crates of ExcellentCrates, CrazyCrates or SpecializedCrates into uxmCrates files."
---

`/uxmcrates migrate <plugin>` reads another plugin's crate set and writes uxmCrates files. The source is read
from its folder under `plugins/`, so it does not have to be installed or enabled. The sources are
`excellentcrates`, `crazycrates` and `specializedcrates`.

The answer says how many crates and keys came across and how many settings did not. A report in
`plugins/uxmCrates/migrations/` lists every value the reader saw and did not use, by path and by value. Read it
before going live: a setting with no equivalent is named there rather than dropped quietly.

The command needs `uxmcrates.admin.migrate`. Run `/uxmcrates reload` afterwards to load what was written.
