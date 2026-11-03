---
title: rating.conf
order: 503
description: Standing time, rating changes, trending and the confidence weighting.
icon: star
---

| Key | Default |
|---|---|
| `must-have-stood-for-seconds` | 30 |
| `wait-before-changing-seconds` | 3600 |
| `trending-half-life-days` | 7 |
| `trending-visit-weight` | 0.5 |
| `confidence-votes` | 5 |
| `confidence-average` | 3.0 |
| `categories` | `[]` |

What each does is in [Gallery](../features/gallery.md#ratings).

Related: [Gallery](../features/gallery.md)
