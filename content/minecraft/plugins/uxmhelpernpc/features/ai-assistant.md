---
title: AI Assistant
order: 36
description: "The Gemini-backed assistant: setup, prompts, limits and what it costs."
icon: message-circle-question
---

The NPC can take a question in chat and answer it from a knowledge base you write. It is off by
default.

```yaml
artificial-intelligence:
  enabled: false
  gemini-key: ""
  gemini-endpoint: "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent"
  chat-timeout: 15
  prompt:
    - You are a server helper. Only use given info.
    - Be nice, keep replies brief and clean.
    - If the info isn't there, reply that you don't know.
    - 'Knowledge Base:'
    - '%s'
    - 'Question:'
    - '%s'
```

## How a question flows

1. A menu item runs `[ask_question]`, or an admin runs `/uxmhelpernpc ask`.
2. The player's next chat message is captured instead of being sent to chat.
3. The prompt is assembled: your lines, then the knowledge base in place of the first `%s`, then
   the question in place of the second.
4. The reply is sent back to that player, formatted with MiniMessage.

The player has `chat-timeout` seconds to type. Keep `cooldown.ask-question-cooldown` equal to it —
the shipped config does, and a shorter cooldown lets a player queue questions faster than they can
be answered.

## The two `%s`

Both are positional and both are required, in that order: knowledge base first, question second.
Everything else in `prompt` is yours to write. The shipped prompt tells the model to answer only
from the knowledge base and to say it does not know otherwise — keep that instruction, or the NPC
will confidently invent server rules.

<Callout type="warning" title="The key is a credential">

`gemini-key` sits in `config.yml` in plain text and pays for every question asked. Rate-limit it
with `cooldown.ask-question-cooldown`, keep `uxmhelpernpc.askai` off the default player group
unless you mean it, and treat the file as a secret in backups.

</Callout>

<Callout type="note" title="Consider not turning this on">

The shipped config says it plainly: an assistant costs water and power on every question, and a
helper on staff answers better. Turn it on because you have measured that nobody is around to
answer, not because it is in the config.

</Callout>
