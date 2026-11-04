---
title: Rules
order: 1080
description: How a rule is written, who it targets, and how rules inside one region and across regions are weighed.
icon: scale
---

Contents: [Writing a rule](#writing-a-rule) · [Targets](#targets) · [Inside one region](#inside-one-region) ·
[Across regions](#across-regions) · [Where rules come from](#where-rules-come-from) · [Asking why](#asking-why)

## Writing a rule

A rule is three words: an [action](actions.md), a target and an effect.

```text
/guard rule market break nonmember deny
/guard rule market use.door everyone allow
/guard rule arena damage.player everyone allow
/guard rule vault use.container perm:vault.open allow
/guard rule market break nonmember clear
```

`clear` removes the rule. A region keeps one rule per action and target, so setting `break nonmember allow`
replaces `break nonmember deny`.

## Targets

| Target | Who it matches |
|---|---|
| `everyone` | Every player |
| `nonmember` | A player with no role in this region |
| `role:<name>` | Players with that role here: `owner`, `manager`, `member`, `guest`, or a role from `roles.extra`. A role name on its own (`member`) means the same |
| `perm:<node>` | Players with that permission node, for example a LuckPerms group node `perm:group.vip` |
| `cond:<condition>` | Players for whom the condition is true, in the same syntax as menu requirements, for example `cond:%player_level% >= 30` |

A role rule names exactly that role: `role:member` does not match an owner. To let every role through, write the
rule for the people without one: `break nonmember deny`.

## Inside one region

For one action, guard starts at that action and walks up the tree (`use.door`, then `use`) until it reaches a
level where some rule matches the player. That level decides, and the levels above it are not looked at.

At that level, the most specific target wins:

1. `role:` and `nonmember` rules,
2. then `perm:` and `cond:` rules,
3. then `everyone` rules.

Between rules of the same kind, deny wins over allow.

| Rules in the region | Player | Result |
|---|---|---|
| `use everyone deny`, `use.door everyone allow` | anyone opening a door | allowed: `use.door` has a matching rule, so `use` is never asked |
| `use everyone deny`, `use.door everyone allow` | anyone opening a chest | refused by `use` |
| `break everyone deny`, `break role:member allow` | a member | allowed: a role rule beats an everyone rule |
| `break perm:group.vip allow`, `break everyone deny` | a VIP with no role | allowed: a perm rule beats an everyone rule |
| `break perm:group.vip allow`, `break nonmember deny` | a VIP with no role | refused: `nonmember` is more specific than a perm rule |
| `break role:member allow`, `break nonmember deny` | a guest | allowed by neither rule, since a guest is not a member and is not a nonmember either; the region gives no answer |

A region gives **no answer** when no level has a rule for the player. Then the next region is asked.

## Across regions

Where regions overlap, they are asked in this order, and the first one with an answer decides:

1. higher priority first;
2. with equal priority, the inner region (the one inside the other) first;
3. regions of equal priority where neither is inside the other are asked together, and a deny from any of them
   wins;
4. the world defaults last.

If nobody answers, the action is allowed.

| Regions | Result |
|---|---|
| `town` (priority 0, `break nonmember deny`) with `plaza` inside it (priority 0, no rules) | a stranger may not break in the plaza: `plaza` has no answer, `town` refuses |
| the same, with `plaza` given `break everyone allow` | anyone may break in the plaza: the inner region speaks first |
| `arena` (priority 10, `damage.player everyone allow`) over `spawn` (priority 0, `damage everyone deny`) | players may fight in the arena |
| two touching plots of priority 0, one allowing and one denying `use` where they overlap | refused: in a tie, deny wins |

## Where rules come from

A region's rules are merged down its [parent](concepts.md#parents-and-families) chain, starting at the top
parent. At each level the level's [template](templates.md) is laid down first and the level's own rules go over
it; the next level down then goes over both. For the same action and target, the nearer level wins, and within
a level the region's own rule wins over its template's. Behaviours and roles merge the same way.

So `/guard rule spawn use.container role:member allow` on a region linked to the `spawn` template adds to the
template rather than fighting it, and a child region needs only the lines that differ from its parent.

## Asking why

`/guard why <action>` is open to every player. It shows each region at their spot, in the order they were
asked, what each answered and the rule that decided, and whether that rule is the region's own, a parent's,
the template's or the world default. Staff can ask the same for someone else with
`/guard test <player> <action> [region]`, and [inspect mode](tools.md#inspect) shows it for a clicked block.
