---
name: spec-writing
description:
  Write or edit a product spec. Use when deciding or changing what the product
  does or an architecturally significant decision, or when writing or editing a
  specification.
---

# Spec Writing

The spec is the source of the product. People decide what the product does by
writing it in the spec, and the software is built from the spec and changed to
match it.

## File structure

A small product:

```
/
├── docs/
│   └── specs/
│       ├── product.md
│       ├── design_system.md
│       └── architecture.md
└── src/
```

As it grows, its areas move out of `product.md` into a file each, and each
platform it runs on gets a folder for what holds only there:

```
/
├── docs/
│   └── specs/
│       ├── product.md           ← the product on one page, and the list of areas
│       ├── ordering.md          ← one file per area
│       ├── billing.md
│       ├── payments.md
│       ├── shipping.md
│       ├── design_system.md     ← the UX that holds across areas
│       ├── architecture.md      ← architecturally significant decisions
│       ├── macos/
│       │   ├── design_system.md ← the UX on this platform
│       │   └── architecture.md  ← architecture on this platform
│       └── web/
│           └── design_system.md
└── src/
```

Only `product.md` is required. Create the other files lazily: only when you
have something to write in them.

Use the format in the Format section below for every file under `docs/specs/`.

## While deciding

### Edit only what was decided

Change the spec when the user settles a behaviour, and only then. Don't write up
proposals, options or guesses: anything in the spec will be built.

### Leave out what isn't decided

The spec carries no open questions, TODOs or placeholders, because the builder
can't ask. Put the question to the user. If it stays open, leave the behaviour
out, or list it under Out of scope.

### Ask for the hard cases

A decision is ready when its edge cases are. Ask what happens when something is
missing, repeated, late, concurrent, refused or fails halfway, and write the
answers into the decision. Propose a concrete case when the user's statement is
abstract, and ask whether it's right.

### Challenge against existing decisions

When a new decision conflicts with one already in the spec, quote it with its
area: "billing.md says a sent invoice can't be changed, but this lets customers
correct the address on one. Which is right?" Don't let both stand.

### Keep other files' summaries true

After changing an area's details, check that its overview and `product.md` still
describe it correctly.

### Decide once, on the widest level it holds

A decision that holds across every area is a principle in `product.md`, such as
"all persisted state is stored in GitHub". One principle settles many
decisions, so the areas don't repeat it.

A decision that holds on every platform goes in the shared files, even if only
one platform has been built. A platform file holds only what is specific to
that platform: how it realises a shared decision (the color for each role, the
component used for navigation), and what the shared files leave open. The
shared files never name a platform, and a platform file never contradicts
them. When a platform has to differ, keep in the shared file only the part
that holds on every platform, and write each platform's version in its own
file.

### The spec leads

The spec says what the product should do, whether or not it does yet. Don't copy
behaviour from the existing software into the spec as if it were decided, and
don't weaken a decision because the software doesn't do it yet.

## Format

### Overview, then details

`product.md` and each area are read two ways: by someone who wants the picture,
and by someone who builds from it. So each has two parts:

1. **Overview**: the high-level picture in a few paragraphs, with the most
   important use cases as examples. A reader who wants a summary stops here.
2. **Details**: everything else the templates below show: user stories,
   decisions and what is out of scope. A builder reads these too.

The overview summarises the details and adds nothing they don't say. The
principles in `product.md` are the exception: each is stated there once and
holds in every area.

### What goes in the spec

The spec holds everything a user can observe: given the spec alone, all of the
product's code could be written again without any change a user would notice.
Beyond that, it holds only the architecturally significant decisions. These
never go in the spec:

- How the code does what the spec says: modules, functions, file paths, data
  structures, unless one is an architecturally significant decision.
- What other systems do on their own, except what the product relies on from
  them.
- The state of the work: what is built or not yet, plans, priorities, task
  lists.
- History: what changed, when, who asked or what it replaced. Words like "now",
  "new", "no longer", "previously" and "will" are signs of it.
- Open questions, TODOs and placeholders.

### product.md

The product on one page: what it is, the problem it solves, the principles
that hold across every area, and what it deliberately doesn't do. While the
product is small, its areas are sections of this file.

```md
# <Product> product spec

<One sentence: what the product is and what it does.>

## Vision

<One paragraph: where the product is heading, beyond what this spec covers.>

## Problem Statement

<The problem, from the user's point of view.>

## Solution

<One sentence: the shape of the solution.>

- **<A principle that holds across every area>.** <One or two sentences.>

## <Area>

<The area, as in the area template below, with every heading one level lower.
Order areas the way a reader meets them.>

## Out of scope

- <Something a reader would expect the product to do, but it deliberately
  doesn't.>
```

When `product.md` grows past about two pages, move every area into its own file
in one edit, raising its headings one level, and replace the area sections with:

```md
## Areas

| Area                | What it covers  |
| ------------------- | --------------- |
| [<Area>](<area>.md) | <One sentence.> |
```

### Areas

An area is one part of the product's behaviour, named the way its users would
name it, such as Ordering or Billing. It holds why the area exists, as user
stories each told from one user's point of view, and its decisions: what the
product does in the area, including UX that belongs to it alone. Once areas
have their own files, a new area gets its file and its row in the Areas table
in the same edit.

```md
# <Area>

<Overview: what this area covers and its most important use cases, in a few
paragraphs.>

## User Stories

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a customer, I want to cancel an order until it ships, so that I don't pay for things I no longer need
</user-story-example>

## Decisions

- <One decision per bullet.>

### <Subtopic, once there are more than about ten decisions>

- <One decision per bullet.>

<A state or flow diagram, if the decisions describe one.>

## Out of scope

- <Something a reader would expect this area to do, but it deliberately
  doesn't.>
```

### design_system.md

The UX that holds across areas: the colors, type and layout every area uses.

```md
# Design system

## <Foundation, such as Color, Type, Layout or Navigation>

- <A decision that holds across areas, one per bullet.>
```

Name what a foundation sets by role, not by value: an "Error" color, not
`#e01b24`. Each platform's design system gives the values.

### architecture.md

The architecturally significant decisions: the ones costly to change later,
such as the languages and frameworks.

```md
# Architecture

- **<An architecturally significant decision, as a statement>.** <What it means,
  and why it was chosen over the obvious alternative.>
```

### Platform folders

A platform is one environment the product runs on with its own conventions,
such as macOS, Linux or the web. Its folder holds only what holds on it alone,
in the files below.

#### <platform>/design_system.md

How the platform realises the shared design system: the platform's design
system it follows, such as Material Design 3 or Apple's Human Interface
Guidelines, the value for each color role, and the components that realise
navigation.

```md
# <Product> on <platform>: design system

<One sentence: the platform's design system it follows.>

## <Foundation>

- <How this platform realises the shared foundation, such as the color for
  each role or the component used for navigation.>
```

#### <platform>/architecture.md

The architecturally significant decisions that hold only on this platform, in
the `architecture.md` format, titled `# <Platform> architecture`.
