---
title: Bedrock
order: 405
description: Floodgate players in without a password, linked accounts, and native forms.
icon: smartphone
---

With `bedrock.auto-login = true` (the default), a Bedrock player Floodgate vouched for through Xbox Live enters without
a password: into their own Bedrock account, made at their first visit, or into the Java account Floodgate linked them
to. An unlinked Bedrock player under a Java account's name is still asked for that account's password.

Off, every Bedrock player uses a password through a native form.

Bedrock players never meet the captcha, are not counted by the accounts-per-address limit, and with
`second-factor.premium-skips` skip the app code. Every dialog has a Bedrock form with the same fields, and the "remember
me" box is a toggle.

Related: [Premium names](premium.md), [Protection](protection.md)
