---
title: Guestbook and sessions
order: 504
description: A plot's guestbook and the list of open building sessions.
icon: book-open
---

## Guestbook

`/plot guestbook` on a plot. The notes, newest first, 45 to a page; a pinned note sits at the top. The owner left clicks
a note to pin it (`plots:note-pin`) and right clicks to remove it (`plots:note-remove`). The book at 49 closes the window
and says how to sign (`/plot sign <note>`).

Tokens: `%note_author%`, `%note_when%`, `%note_text%`, `%book_notes%`, and every `%plot_...%` word. Conditions:
`plots:book-empty`, `plots:note-pinned`.

## Sessions

`/plot sessions`. Every open session on the server; a click joins it and goes there (`plots:session-join`).

| Token | Means |
|---|---|
| `%session_plot%` | Which plot is open |
| `%session_owner%` | Whose it is |
| `%session_left%` | Whole minutes left |
| `%session_guests%` | How many are building there now |
| `%sessions_open%` | How many sessions are open |

Related: [Community](../features/community.md), [Members and visitors](../features/members.md#sessions)
