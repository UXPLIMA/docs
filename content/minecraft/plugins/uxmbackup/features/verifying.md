---
title: Verifying
order: 406
description: Reading an archive back against its manifest.
icon: shield-check
---

Every archive carries a manifest of every file and its checksum. `/uxmbackup verify <id>`, or a click on an archive in
the [archives window](../menus/archives.md), fetches the archive from the first storage that holds it and reads every
file back against the manifest.

| Answer | Means |
|---|---|
| `verify.whole` | Every file is there and matches, with the count checked |
| `verify.broken` | How many files are missing and how many are damaged |

An in-place restore verifies too: an archive that is not whole is never put over live data.

`/uxmbackup info <id>` lists what one archive holds and how big it is without reading every file.

Related: [Restoring](restoring.md)
