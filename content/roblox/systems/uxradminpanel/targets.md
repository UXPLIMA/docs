---
title: Choosing targets
order: 4
description: Keywords, partial names, teams, ranks, lists and exclusions.
icon: crosshair
---

Any argument of type `Players` accepts a target expression. It resolves to a list, and the
command runs once per player in it.

## Keywords

| Expression | Resolves to |
|---|---|
| `me` | You |
| `all` | Everybody in the server |
| `other` or `others` | Everybody except you |
| `random` | One player at random, possibly you |
| `friends` | Everybody in the server you are friends with |
| `nearest` | The closest other player |
| `furthest` | The furthest other player |

`nearest` and `furthest` measure from your character to theirs. A player without a
character counts as infinitely far away, so `nearest` skips them and `furthest` may well
pick them.

Three of the keywords are renameable:

```lua
Localization = { Self = "me", All = "all", Other = "other" },
```

The English words keep working alongside whatever you set, so translating them adds a name
rather than replacing one.

## Prefixed selectors

| Expression | Resolves to |
|---|---|
| `team-Police` | Everybody on the Police team |
| `rank-Mod` | Everybody currently holding the Mod rank |
| `userid:12345678` | That user, if they are in the server |

Team and rank names are matched case-insensitively. A team name with a space in it needs
quoting: `"team-Red Team"`.

<Callout type="tip" title="rank- is how you address your staff">

`u!bring rank-Mod` pulls every moderator to you, and `u!servermessage` plus `rank-Admin`
reaches exactly the people who can act on it.

It reads the live rank, so somebody promoted a minute ago is included.

</Callout>

## Partial names

Anything that is not a keyword or a prefixed selector is matched against usernames and
display names, case-insensitively, as a substring.

| Typed | Matches |
|---|---|
| `bui` | `Builderman`, and anybody else containing those letters |
| `Player` | Every default-named test account at once |

A partial name that matches several players resolves to all of them for a normal argument.
Commands that need exactly one target refuse an ambiguous match and tell you how many it hit.

## Lists and exclusions

Commas combine, and a leading minus excludes.

```
u!kill all,-me
u!bring team-Police,team-Medic
u!ff rank-Mod,-Player1
u!heal nearest,furthest
```

| Rule | |
|---|---|
| Include order is preserved | The first mention wins |
| Duplicates collapse | A player named twice is hit once |
| Exclusions are applied last | Order in the expression does not matter |
| An empty result is an error | The command is refused with a message |

`all,-me` is the everyday one: it is what `other` means, written out.

## Defaults

Most commands default their target argument to `me`, so `u!fly` is `u!fly me`. A few default
to `all`. The default is in the command's own definition, listed on the Commands page in the
panel.

<Callout type="warning" title="An expression that matches nobody is an error, not a no-op">

`u!kill team-Police` on a server with no police tells you nothing matched and does not run.

That is the safe behaviour, and it means a typo in a team name is visible rather than
silent. It also means a scheduled command that relies on a target list will report an error
on an empty server.

</Callout>

## Everything is checked twice

The expression is resolved on the server from the actor's point of view, so `nearest` means
nearest to the actor's real character position and `friends` is checked against the real
friend list.

Immunity is then checked per target as the moderation commands run, so a rank-wide
expression that sweeps up a protected player is refused for that one and carries on with
the rest. See [Ranks and permissions](ranks.md).
