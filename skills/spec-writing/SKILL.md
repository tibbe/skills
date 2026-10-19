---
name: spec-writing
description:
  Write or edit a product spec. Use when deciding or changing what the product
  does or an architecturally significant decision.
---

# Spec Writing

The spec is the source of the product. People decide what the product does by
writing it in the spec, and the software is built from the spec and changed to
match it.

## File structure

```
/
├── GLOSSARY.md                  ← the product's terms, if it keeps a glossary
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

Create each file lazily: only when you have something to write in it.

A product may keep a glossary, `GLOSSARY.md` at the repository root: its
ubiquitous language, in the sense of domain-driven design, with each term the
product gives its own meaning, such as "backorder", defined once. The spec uses
those terms unchanged, and a term the spec coins goes in the glossary.

## While deciding

### Write only what was decided

Change the spec when the user settles a behaviour, and only then: anything in
it will be built, and the builder can't ask. Put an open question to the user;
if it stays open, leave the behaviour out, or list it under Out of scope.

### Start from prior art

Before you recommend how the product behaves, find out how others have answered
the same question: products of the same kind as this one and others facing the
same problem, and the platform's guidelines and standards, such as Apple's Human
Interface Guidelines, Material Design or RFC 3339. How they answer it is a fact:
look it up rather than ask the user or rely on memory, which is often wrong in
the details.

Dispatch one sub-agent for the platform and one for products. Give each the
question and this brief: "Search the web for how this question is answered.
Report every principled design the sources take, each with its trade-offs, the
evidence of its adoption, why its adopters chose it, and the sources it comes
from. Evidence of adoption: the platform providing or documenting it, and
products like ours using it. Under 400 words."

Then follow the consensus: the design most sources converge on, counting what
the platform documents for this question as strong evidence. Any other design
has to win against it. Write down the case against each design; the case against
the consensus first says why its adopters chose it, then why that doesn't apply
here. Strike out any reason, for or against a design, that counts the size of
the change, leans on what the code does today, or assumes the choice it argues
for, and choose on the reasons that remain.

Put the designs to the user, each with its source in a few words, recommend the
one you chose, and give the reason against each of the others.

### Ask for the hard cases

A decision is ready when its edge cases are. Ask what happens when something is
missing, repeated, late, concurrent, refused or fails halfway, and write the
answers into the decision. Propose a concrete case when the user's statement is
abstract, and ask whether it's right.

### State the rule, not its cases

When a decision lists cases, or you're about to add a case to one, propose the
rule the cases follow and ask whether it's right: "a customer can cancel an
order until it ships", not "a customer can cancel an order that is placed, paid
or packed". The rule can be a convention the product follows: "amounts are
written the way the customer's locale writes them". Make it the most general
statement from which a builder can still derive every case. A rule is
open–closed: it settles the next case without an edit, where a list grows with
every one. Write cases only as exceptions to the rule or as examples of it.

### Write only what no decision already implies

A builder derives every case from the decisions, so a clause that follows from
a decision already in the spec tells them nothing. Instead it gives them two
things to keep true. After you write a decision, check each clause and ask
which decision it follows from, in this area or another. Keep it only if the
answer is none, or if it is an exception or an example. Signs of an implied
clause:

- A qualifier that a general word already covers: "including gift card
  orders" after "every paid order".
- A consequence of the rule before it: "a shipped order can't be cancelled"
  after "a customer can cancel an order until it ships".
- What another area already decides: "a cancelled order is refunded" in
  Ordering when Payments decides refunds.
- A tautology: "a paid invoice is one the customer has paid".
- A detail no decision depends on, such as a title format nobody reads.
- What the product doesn't do, when no decision says it does: "customers can't
  edit a sent invoice" when nothing lets them edit one. If a reader would
  expect the behaviour, list it under Out of scope.

### Challenge against existing decisions

When a new decision conflicts with one already in the spec, quote it with its
area: "billing.md says a sent invoice can't be changed, but this lets customers
correct the address on one. Which is right?" Don't let both stand.

### Keep overviews true

After changing an area's details, check that its overview and `product.md` still
hold. Change them only when the decision contradicts them, or changes what the
area is about.

### Decide once, on the widest level it holds

A decision that holds across every area is a principle in `product.md`, such as
"every amount is shown in the customer's currency". One principle settles many
decisions, so the areas don't repeat it.

A decision that holds on every platform goes in the shared files, even if only
one platform has been built. A platform file holds only what is specific to
that platform: how it realises a shared decision, and what the shared files
leave open. The shared files never name a platform. When a platform has to
differ, keep in the shared file only the part that holds on every platform, and
write each platform's version in its own file.

### The spec leads

The spec says what the product should do, whether or not it does yet. Don't copy
behaviour from the existing software into the spec as if it were decided, and
don't weaken a decision because the software doesn't do it yet.

## Format

### Overview, then details

`product.md` and each area open with an overview for someone who wants to
understand what it is about, then the details for someone who builds from it.
The overview explains; it doesn't list. It states no requirement the details
don't, except for the principles in `product.md`: each is stated there once and
holds in every area. An overview may hold a diagram of the main path, such as
the pipeline end to end; like the rest of the overview, it decides nothing.

### What goes in the spec

The spec decides everything a user can observe: given the spec alone, a builder
could write all of the product's code again, deriving each case from its
decisions, without any change a user would notice. Beyond that, it holds only
the architecturally significant decisions. These never go in the spec:

- How the code does what the spec says: modules, functions, file paths, data
  structures.
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

## <Area>

<The area, as in the area template below, with every heading one level lower.
Order areas the way a reader meets them.>

## Out of scope

- <Something a reader would expect the product to do, but it deliberately
  doesn't.>
```

`product.md` holds its areas as sections until it grows past about two pages.
Then move every area into its own file in one edit, raising its headings one
level, and replace the area sections with:

```md
## Areas

| Area                | What it covers  |
| ------------------- | --------------- |
| [<Area>](<area>.md) | <One sentence.> |
```

### Areas

An area is one part of the product's behaviour, named the way its users would
name it, such as Ordering or Billing, and holds the UX that belongs to it
alone. Once areas have their own files, a new area gets its file and its row
in the Areas table in the same edit.

```md
# <Area>

<Overview: what this area is about at its core, explained to someone new to it,
with its most important use cases as examples.>

## User Stories

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a customer, I want to cancel an order until it ships, so that I don't pay for things I no longer need
</user-story-example>

## <Topic>

<The rule for the whole topic, in one sentence: "A customer can cancel an
order until it ships.">

<The rest of the topic, in the forms below.>

## Out of scope

- <Something a reader would expect this area to do, but it deliberately
  doesn't.>
```

Split what the product does in the area into topics, each a section named the
way a reader would look for it, such as Cancelling or Refunds. Order topics the
way a reader meets them. Most of a topic is the detail under its rule: what
cancelling does to the payment and the shipment, what the customer sees, and
so on, in whichever of these forms fits. A topic can use more than one form.

- Write rules as bullets, each holding one rule and opening with a bold label
  that names what it settles:

  ```md
  - **Cancelling.** A customer can cancel an order until it ships.
  - **Refunds.** A cancelled order is refunded to the card that paid for it.
  ```

- Write a sequence as numbered steps along the usual path, then what happens
  when a step goes another way, keyed to that step:

  ```md
  1. The customer pays.
  2. The warehouse packs the order.
  3. The order ships.

  - **1a. The card is declined.** The order waits a day for payment, then is
    cancelled.
  - **2a. An item is out of stock.** The customer chooses to wait for it or to
    have the rest shipped now.
  ```

- Write a set of things that share attributes, such as roles, labels, settings
  or outcomes, as a table with a row per thing:

  ```md
  | Role   | Can see       | Can change          |
  | ------ | ------------- | ------------------- |
  | Viewer | every order   | nothing             |
  | Clerk  | every order   | orders not yet paid |
  ```

- Write a lifecycle as a table with a row per transition, so the builder can
  check every state against every event:

  ```md
  | State  | Event                | Next      | Effect                   |
  | ------ | -------------------- | --------- | ------------------------ |
  | placed | customer pays        | paid      | the warehouse is told    |
  | paid   | customer cancels     | cancelled | the payment is refunded  |
  | paid   | warehouse ships      | shipped   | the customer is emailed  |
  ```

### design_system.md

The UX that holds across areas.

```md
# Design system

## <Foundation, such as Color, Type, Layout or Navigation>

- <A decision that holds across areas, one per bullet.>
```

Name what a foundation sets by role, not by value: an "Error" color, not
`#e01b24`.

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
such as macOS, Linux or the web.

#### <platform>/design_system.md

How the platform realises the shared design system: the platform's design
system it follows, such as Material Design 3 or Apple's Human Interface
Guidelines, the value for each color role, and the components that realise
navigation.

```md
# <Product> on <platform>: design system

<One sentence: the platform's design system it follows.>

## <Foundation>

- <How this platform realises the shared foundation.>
```

#### <platform>/architecture.md

The architecturally significant decisions that hold only on this platform, in
the `architecture.md` format, titled `# <Platform> architecture`.
