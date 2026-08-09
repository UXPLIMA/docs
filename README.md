# UXPLIMA Documentation

The content behind [docs.uxplima.com](https://docs.uxplima.com). Every page here
is Markdown with a small front matter block; the documentation platform compiles
this repository, validates it, and publishes a snapshot.

## A page

```markdown
---
title: Claim Flags
order: 320
description: What each flag protects, and which ones to leave off.
---

Prose, headings, tables and code fences behave the way they do anywhere else.
```

- `title` names the page in the sidebar and as its heading.
- `order` decides where the page sits among its neighbours; groups follow the
  order of the pages inside them.
- `description` is the lead paragraph under the title and the description search
  engines show. A page without one still publishes, and the validation report
  lists it.

Directories become sidebar groups, named after the directory. `index.md` stands
for the directory that contains it.

## Beyond Markdown

A page may use the components the platform renders. The ones this content
already relies on:

```markdown
<Callout type="warning" title="Read this first">

Anything inside is ordinary Markdown.

</Callout>

<Tabs>
<Tab label="Maven">

...

</Tab>
<Tab label="Gradle">

...

</Tab>
</Tabs>
```

`Callout` accepts `note`, `info`, `tip`, `success`, `warning` and `danger`.

Raw HTML is refused, and so is any component outside the published set.

## Links

Link to another page by its file, the way the repository is laid out:
`[Basic concepts](../getting-started/concepts.md)`. The compiler resolves it to
the page it names, so the link keeps working under every version, locale and
domain the site is served at.

## Publishing

Pushing to `main` does not publish on its own. The platform pulls this
repository, compiles it, and reports what it found; publishing a snapshot is a
deliberate step taken in the administration.
