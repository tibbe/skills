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
├── GLOSSARY.md
├── docs/
│   └── specs/
│       ├── product.md
│       └── architecture.md
└── src/
```

As it grows, its areas move out of `product.md` into a file each:

```
/
├── GLOSSARY.md                  ← the vocabulary: each term defined once
├── docs/
│   └── specs/
│       ├── product.md           ← the product on one page, and the list of areas
│       ├── ordering.md          ← one file per area
│       ├── billing.md
│       ├── payments.md
│       ├── shipping.md
│       └── architecture.md      ← architecturally significant decisions
└── src/
```

Create files lazily: only when you have something to write.

- If no `docs/specs/` exists, create `product.md` and `architecture.md` together
  when the first decision is made.
- While the product is small, each area is a section of `product.md`. When
  `product.md` grows past about two pages, move every area into its own file in
  one edit, and replace the sections with the Areas table.
- Once areas have their own files, a new area gets a file and a row in the Areas
  table in the same edit.
- If no `GLOSSARY.md` exists, create it when the first term is settled.

Use the format in the Format section below for every file under `docs/specs/`.
`GLOSSARY.md` has its own format; if a domain-modeling skill is available, use
it for glossary edits.

## Terms

- **Area**: one part of the product's behaviour, named the way its users would
  name it, such as Ordering or Billing. It is a section of `product.md` in a
  small product, and its own file in a larger one.
- **Decision**: one statement of what the product does or requires, settled by
  the person who owns the product. Decisions are the part of the spec that is
  built.
- **User story**: why an area exists, told from one user's point of view.

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

### Use the glossary's words

Use terms exactly as `GLOSSARY.md` defines them. When the user uses a word the
glossary lists under _Avoid_, or a new term the spec needs, settle the term and
update the glossary in the same edit.

### Keep other files' summaries true

After changing an area's decisions, check that `product.md` still describes the
area correctly.

### The spec leads

The spec says what the product should do, whether or not it does yet. Don't copy
behaviour from the existing software into the spec as if it were decided, and
don't weaken a decision because the software doesn't do it yet.

## Format

### Normative and informative

The spec has two kinds of content:

- **Normative**: the Solution principles in `product.md`, and the Decisions in
  each area and in `architecture.md`. These are what gets built. Every fact
  about the product is stated normatively exactly once.
- **Informative**: everything else: the Vision, the Problem Statement, each
  area's opening paragraph, user stories and diagrams. These help a reader
  understand the normative parts. They summarise and explain, and add no
  requirement of their own.

A reader who stops at the informative parts gets a correct picture. A builder
who reads the normative parts gets every requirement.

### What stays out of the spec

The templates below show what goes in each file. Terms are defined only in
`GLOSSARY.md`, never in the spec files. These never go in the spec:

- How the code does what the spec says: modules, functions, file paths, data
  structures, unless one is an architecturally significant decision, which goes
  in `architecture.md`.
- What other systems do on their own, except what the product relies on from
  them.
- The state of the work: what is built or not yet, plans, priorities, task
  lists.
- History: what changed, when, who asked or what it replaced. Words like "now",
  "new", "no longer", "previously" and "will" are signs of it.
- Open questions, TODOs and placeholders.

### product.md

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

<A diagram of the main flow, if there is one.>

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

```md
# <Area>

<One or two sentences: what this area covers.>

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

### architecture.md

```md
# Architecture

<One sentence: what kind of decisions this file holds.>

- **<An architecturally significant decision, as a statement>.** <What it means,
  and why it was chosen over the obvious alternative.>
```
