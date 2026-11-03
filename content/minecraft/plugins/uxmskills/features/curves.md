---
title: Curves and formulas
order: 402
description: The shared curve, presets, formulas, lists, and the formula grammar.
icon: trending-up
---

## A curve

```hocon
curve { preset = "slow-late", base = 90 }                               # config.conf ships this
curve { formula = "base * level ^ steepness", base = 60, steepness = 1.6 }
curve = [100, 250, 500, 900]
```

A preset is `linear`, `quadratic` or `slow-late`, with a `base`; it expands into a formula you can then edit.
`/uxmskills curves` prints what each preset costs. A formula may use its own variables declared beside it. A list
repeats its last value: `[100, 200, 400]` costs 400 for every level after the third.

A skill that writes no curve takes `curve` from `config.conf`. After changing a curve, `/uxmskills recalculate`
rewrites every stored level.

## Formulas

Every number can be a formula: `100 * level ^ 1.6`. The grammar is `+ - * / %`, `^` (right associative, so `2 ^ 3 ^ 2`
is 512), brackets, unary minus, named variables and the functions `min`, `max`, `floor`, `ceil`, `round`, `abs`,
`sqrt`, `pow`, `ln`, `log`, `exp` and `clamp`. There is no assignment, condition or loop. A broken formula is refused at
load with the position of the character that broke it, and an answer that is not a finite number is refused when
evaluated.

Related: [Skills](skills.md)
