---
title: Discord webhooks
order: 265
description: Posting to a Discord channel with no bot, no token and no JDA.
icon: send
---

An incoming webhook URL is all you need to post to a channel. No bot account, no gateway connection,
no library.

```java
DiscordWebhook webhook = new DiscordWebhook(url);

webhook.sendContent("Server started");
webhook.sendContent("Line one", "Line two");
webhook.sendEmbed(DiscordEmbed.colored("Alert", "Disk is 90% full", 0xFF0000));
webhook.sendEmbeds(List.of(first, second));
```

Every send is non-blocking and returns a `CompletableFuture<Integer>` of the HTTP status, so a server
thread never waits on network I/O.

## Detailed results

```java
webhook.sendContentDetailed(text).thenAccept(status -> {
    if (status.delivered()) return;
    if (status.rateLimited()) logger.warning("Discord rate limited us");
    if (status.authError()) logger.severe("The webhook URL is wrong or revoked");
    logger.warning(status.message());
});
```

`WebhookStatus` classifies the response: `delivered`, `rateLimited`, `authError`, `clientError`,
`serverError`, plus a human `message`.

Use the detailed form for anything an operator configured, because a revoked webhook should produce a
clear log line rather than silence.

## Embeds

```java
DiscordEmbed embed = DiscordEmbed.builder()
        .title("Player banned")
        .description("Notch was banned by an admin.")
        .color(0xFF5555)
        .url("https://example.com/bans/1")
        .author(a -> a.name("MyPlugin").iconUrl(iconUrl))
        .footer(f -> f.text("Server 1"))
        .thumbnail(thumbUrl)
        .image(imageUrl)
        .timestamp(Instant.now())
        .field("Reason", "Griefing", true)
        .field(f -> f.name("Duration").value("7 days").inline(true))
        .build();
```

`color` also takes an Adventure `RGBLike` or three channel values. `description` takes several lines
or a list.

`DiscordEmbed.of(title, description)` and `DiscordEmbed.colored(title, description, color)` are the
short forms.

## Full messages

```java
WebhookMessage message = WebhookMessage.builder()
        .content("Deployment finished")
        .embed(embed)
        .username("Deploy Bot")
        .avatarUrl(avatarUrl)
        .threadName("deploys")
        .build();

webhook.send(message);
webhook.sendDetailed(message);
```

`username` and `avatarUrl` override the webhook's configured identity per message, which is how one
webhook serves several logical senders.

## What it does for you

**Mentions are suppressed by default.** Every request carries an empty allowed-mentions parse list,
so a message body can never mass-ping everyone, here, or a role, even if a player's chat message ends
up inside it.

**One shared HTTP client.** Constructing a webhook per config reload does not leak a thread pool.

**One retry on a rate limit.** A transient 429 does not drop the message, and one retry is not a
storm.

**Embed limits are enforced** before the request goes out, so an over-long field is caught locally
rather than as a 400 from Discord.

<Callout type="warning" title="A webhook URL is a credential">

Anyone holding it can post to that channel. Keep it in config, not in code, and treat it like a
password when handling logs and bug reports.

</Callout>
