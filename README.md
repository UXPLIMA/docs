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

## Writing standard

Pages are reference material, not essays. A reader arrives with a question and leaves with an answer.

1. **One page per thing.** A module is documented on its own page and nowhere else. No second page repeats its
   commands, and no overview page restates what its page already says.
2. **Fixed order on a module page:** the lead paragraph, `## Commands`, `## Permissions`, `## Settings`,
   `## Placeholders`, `## Notes`, then a single `Related:` line. A section with nothing to say is left out
   rather than filled.
3. **600 words of hand-written prose per module page**, 900 on any other page. Generated tables do not count.
   A reference page that enumerates an API or a catalogue is bounded per entry instead: three sentences each,
   and no page-level budget.
4. **The description is one line.** It renders as the lead under the title, so the page's own first paragraph
   says something else.
5. **Tables before sentences.** Anything enumerable is a table. A sentence explains a rule a table cannot.
6. **One `Callout` per page at most**, and only where the reader would otherwise make a costly mistake.
7. **No decoration.** No `---` between sections, no emoji, no marketing adjectives ("powerful", "seamless",
   "comprehensive"), no analogies, no em dash. Second person, present tense.
8. **Generated blocks are not edited by hand.** The text between `{/* generated:x */}` and
   `{/* /generated */}` comes from the plugin's own catalogues, through `tools/docs/generate.py` in the plugin
   repository. Editing it there is editing something that gets overwritten.

A module page looks like this:

```markdown
---
title: Homes
order: 9070
description: Private teleport points each player saves in a slot grid.
---

Two or three sentences: what the module does and what a player types to reach it.

Module `homes` · enabled by default · `modules/homes/config.conf`

## Commands

{/* generated:commands */}
{/* /generated */}

## Notes

- Three to six bullets: quotas, traps, integrations.

Related: [Teleport](teleport.md), [Warps](warps.md)
```

## Links

Link to another page by its file, the way the repository is laid out:
`[Basic concepts](../getting-started/concepts.md)`. The compiler resolves it to
the page it names, so the link keeps working under every version, locale and
domain the site is served at.

## Publishing

Pushing to `main` does not publish on its own. The platform pulls this
repository, compiles it, and reports what it found; publishing a snapshot is a
deliberate step taken in the administration.
