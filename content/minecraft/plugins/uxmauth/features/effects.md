---
title: Effects
order: 310
description: The lines run at every moment of the door, per way in, per server, and the countdown bar.
icon: sparkles
---

`effects` in [effects.conf](../config/effects-conf.md) is a list of lines per moment. A line is `[message]`,
`[actionbar]`, `[title]`, `[subtitle] <title> | <subtitle>`, `[bossbar]`, `[sound]`, `[particle]`, `[effect]`,
`[console]` or `[player]`. An `@name` is a line from the messages folder. `[]` is nothing.

| Moment | When | Shipped |
|---|---|---|
| `join-unregistered` | A player with no account arrives | A title to register |
| `join-registered` | A player with an account arrives | A title to log in |
| `join-code` | A session or premium player whose account asks the app code | A title for the code |
| `reminder-register`, `reminder-login`, `reminder-code` | Every `timeouts.reminder-seconds` while they wait | An action bar line |
| `wrong-password` | A wrong password | A low note |
| `registered` | A new account | A title and a chime |
| `logged-in` | A login | A title and a chime |
| `logged-in-password`, `-premium`, `-bedrock`, `-session`, `-forced` | After `logged-in`, by the way in | Nothing |
| `conflict` | A player's name went to its paid owner and their account moved | Nothing |
| `timeout`, `too-many-attempts`, `banned` | Just before those kicks | Nothing |
| `logged-out` | A logout | A note |
| `password-changed` | A password change | A message and a chime |

A login the proxy of a network vouched for runs only `logged-in`.

## Placeholders

`%player_name%` and `%player_uuid%` work in any line, and `%uxmauth_time_left%`, the seconds a waiting player has, for a
bar that counts down with each reminder:

```
"[bossbar] 5 red | <red>%uxmauth_time_left% seconds to log in"
```

A bar with `drain` after its colour empties over its own seconds, and a bar a waiting moment puts up comes down at the
login, so one bar on a join moment as long as the timeout shows the time left:

```
"[bossbar] 60 red drain | <red>Log in before the bar runs out"
```

Any other placeholder is answered by PlaceholderAPI when it is installed.

## Per server

`servers` holds lines for one server of a network, run after the lists above on the server whose `network.server-name`
is the name:

```hocon
servers {
  lobby { logged-in = ["[console] give %player_name% compass 1"] }
}
```

On the proxy, the `[console]` and `[player]` lines of `registered` and `logged-in` run as proxy commands, once per login
however many servers the player visits.

Related: [effects.conf](../config/effects-conf.md)
