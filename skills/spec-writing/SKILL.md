---
name: spec-writing
description:
  Write or edit a product spec. Use when deciding or changing what the product
  does or how it looks, or an architecturally significant decision, even if
  the user only asks to build or change a feature and doesn't mention the spec.
---

# Spec Writing

The spec is the source of the product. People decide what the product does by
writing it in the spec, and the software is built from the spec and changed to
match it.

## File structure

```
/
├── GLOSSARY.md               ← the product's terms, if it keeps a glossary
├── docs/
│   └── specs/
│       ├── product.md        ← the product on one page, and the list of areas
│       ├── ordering.md       ← one file per area, with its screens
│       ├── billing.md
│       ├── payments.md
│       ├── shell.md          ← the regions every screen shares
│       ├── design_system.md  ← the design system the UI is drawn in
│       ├── components.md     ← the product's components that screens share
│       ├── architecture.md   ← architecturally significant decisions
│       └── visuals/          ← sketches of screens, as images or HTML
└── src/
```

That is a product with one UI. A product with more than one UI, such as a web
app and an iOS app, gives each UI a folder of its own
([More than one UI](#more-than-one-ui)):

```
docs/specs/
├── product.md
├── ordering.md               ← the area on every UI: its stories and rules
├── architecture.md
├── web/
│   ├── ordering.md           ← the area's screens on the web
│   ├── shell.md
│   ├── design_system.md
│   ├── components.md
│   └── visuals/
└── ios/
    └── ...
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

Before you recommend how the product behaves or how it is built, find out how
others have answered the same question: products of the same kind as this one
and others facing the same problem, and the platform's guidelines and standards,
such as Apple's Human Interface Guidelines, Material Design or RFC 3339. How
they answer it is a fact: look it up rather than ask the user or rely on memory,
which is often wrong in the details.

Frame the question from what the user and the spec have settled, and nothing
else: the kind of product it is for, the behaviour to decide, its quality
attributes, and the platform it runs on. Dispatch one sub-agent for the platform
the framing names and one for products of the kind it names, together, in the
foreground. Give each the framing and this brief, and nothing else: "Search the
web for how this question is answered. Report every principled option the
sources take, each with its trade-offs, the evidence of its adoption, why its
adopters chose it, and the sources it comes from. Evidence of adoption: the
platform providing or documenting it; products like ours using it, with each
product's users, downloads or GitHub stars; and, for a library to adopt, its
GitHub stars, package downloads and recent releases. Report them condensed and
complete: one entry per option, leaving out no option, evidence or source, and
nothing else. Where sources conflict, give both."

Once both have reported, list every option they give, and write the decision's
criteria: the quality attributes the framing states, and performance, wherever
the reports' trade-offs show the options can differ on them. Word each so that
more of it is better, one concern to each. Mark as a must-have any the user or
the spec states outright, and rank the rest High, Medium or Low by how much the
decision's success depends on them. Then:

1. Rule out any option that fails a must-have.
2. Of the rest, take the consensus as the reference: the most widely adopted,
   by the evidence of adoption the reports give, counting what the platform
   documents for this question as strong evidence.
3. Rate each other option against the reference on each criterion as better,
   same, worse or unknown, with the reason and the report it comes from.
4. Improve and combine options, the ones step 1 ruled out among them: where an
   option rates well but for one criterion, because of one part of it, change
   that part; where two options are each better on a different criterion, put
   the parts that make each better into one option. Rate each new option with
   the rest, from what the reports say of its parts.
5. Where a rating is unknown, say whether the choice would change if it went
   either way.
6. Follow the reference unless another option's better ratings outweigh its
   worse ones, weighed by each criterion's rank and by how far apart the
   options are on it, not by their count.

Write down the case against each option; the case against the consensus first
says why its adopters chose it, then why that doesn't apply here. Strike out
any reason, for or against an option, that counts the size of the change, leans
on what the code does today, or assumes the choice it argues for, and choose on
the reasons that remain.

Put the criteria, with their ranks, and the options to the user, each option
with its source in a few words, recommend the one you chose, and give the reason
against each of the others.

### Ask for the hard cases

A decision is ready when its edge cases are. Ask what happens when something is
missing, repeated, late, concurrent, refused or fails halfway, and write the
answers into the decision. Propose a concrete case when the user's statement is
abstract, and ask whether it's right.

### State the rule, not its cases

When a decision about what the product does lists cases, or you're about to add
a case to one, propose the rule the cases follow and ask whether it's right: "a
customer can cancel an order until it ships", not "a customer can cancel an
order that is placed, paid or packed". The rule can be a convention the product
follows: "amounts are written the way the customer's locale writes them". Make
it the most general rule from which a builder can still derive every case. A
rule is open–closed: it settles the next case without an edit, where a list
grows with every one. Write cases only as exceptions to the rule or as examples
of it.

### Challenge against existing decisions

When a new decision conflicts with one already in the spec, quote it with its
area: "billing.md says a sent invoice can't be changed, but this lets customers
correct the address on one. Which is right?" Don't let both stand.

### Put each decision where it belongs

Where a decision goes depends on what it decides:

- **What the product does** is decided once, so no other file repeats it. A
  choice that defines the product in broad strokes is a principle in
  `product.md` ([product.md](#productmd)). Every other decision goes in the
  area whose concern it is. A concern that several areas share is an area of
  its own, such as Currency, for how amounts are shown.
- **How one part of the UI looks and works** is decided where the part is: on
  its screen, or in its component ([components.md](#componentsmd)).
- **A rule over many parts of the UI**, such as that each text is in the
  active voice, goes in `design_system.md`, and only when the user asks for it
  ([design_system.md](#design_systemmd)).

With more than one UI, a decision that holds on every UI goes in the shared
files, and a UI's folder holds only what is specific to that UI. When a UI has
to differ, keep in the shared file only the part that holds on every UI, and
write each UI's version in its folder. A shared file decides nothing for one
UI alone, but it can link to the files of each UI that add to it.

### The spec leads

The spec says what the product should do, whether or not it does yet. Don't copy
behaviour from the existing software into the spec as if it were decided, and
don't weaken a decision because the software doesn't do it yet.

Once done, call the Skill tool with "spec-review" to review the text.

## Plain English

Write by the writing rules of ASD-STE100 Simplified Technical English.

## Document structure

### Overview, then details

`product.md`, each area and `architecture.md` open with an overview for someone
who wants to understand what it is about, then the details for someone who
builds from it.
The overview explains; it doesn't list, and it decides nothing that the details
don't. It may hold a diagram of the main path, such as the pipeline end to end.
In `product.md`, the details start with the principles under Solution
([product.md](#productmd)).

### What goes in the spec

The spec decides everything a user can observe: given the spec alone, a builder
could write all of the product's code again, deriving each case from its
decisions, without any change a user would notice. Beyond that, it holds only
the architecturally significant decisions.

What a user can observe includes each text that the product shows, such as a
label, a message or an error, and each icon. Write the text exactly, in quotes:
"No orders", not "a message that says that the list is empty". Name the icon by
its name in the icon set. The builder then chooses neither.

These never go in the spec:

- How the code does what the spec says: modules, functions, source file paths,
  data structures.
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

- **<A principle: a choice that defines the product in broad strokes>.** <One
  or two sentences.>

## <Area>

<The area, as in the area template below, with every heading one level lower.
Order areas the way a reader meets them.>

## Out of scope

- <Something a reader would expect the product to do, but it deliberately
  doesn't.>
```

A principle is a choice that defines the product in broad strokes: without it,
the product would be a different one. Every area holds to each principle, so no
area repeats it. A rule that holds in many areas but doesn't define the
product, such as how amounts are shown or the color of a button, goes where its
concern is
([Put each decision where it belongs](#put-each-decision-where-it-belongs)).

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
name it, such as Ordering or Billing. It holds both what the product does in
that part and the screens on which the user does it. Once areas have their own
files, a new area gets its file and its row in the Areas table in the same
edit.

```md
# <Area>

<Overview: why the area exists, for someone new to it. Open with the problem it
solves for its users, then what it lets them do about it. Then give an example:
a user with that problem, the question they bring, and the answer the area
gives them. Leave the terms the area gives its own meaning, such as
"backorder", to its topics.>

## User Stories

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a customer, I want to cancel an order until it ships, so that I don't pay for things I no longer need
</user-story-example>

## <Topic>

<The rule for the whole topic: "A customer can cancel an order until it
ships.">

<The rest of the topic, in the forms under Forms.>

## Screens

<The area's screens, as under Screens below.>

## Out of scope

- <Something a reader would expect this area to do, but it deliberately
  doesn't.>
```

Split what the product does in the area into topics, each a section named the
way a reader would look for it, such as Cancelling or Refunds. Order topics the
way a reader meets them. Most of a topic is the detail under its rule: what
cancelling does to the payment and the shipment, what the customer sees, and
so on, in the [forms](#forms).

### Forms

How a part of the UI looks takes the forms under
[How a part looks](#how-a-part-looks). The other details of the spec, such as a
topic of an area or an architectural decision, take whichever of these forms
fits. A section can use more than one form.

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

### architecture.md

How the product is built: the decisions costly to change later that no user
can observe, such as the languages and frameworks.

```md
# Architecture

<Overview: what the architecture is about at its core, explained to someone new
to it: "an order is a log of events; each service keeps its own view of it".>

## <Topic, such as State or Deployment>

<The rule for the whole topic: "A service reads only its own view of an
order, and updates it from the log.">

<The decisions under the rule, in the forms under Forms.>
```

### More than one UI

A UI is one way users reach the product, on one platform, such as its web app
or its iOS app. A product with more than one UI gives each a folder named for
its platform, such as `web/` or `ios/`.

Each file but `product.md` can be both shared, in `docs/specs/`, and in a UI's
folder. The shared file holds what holds on every UI, and the UI's file only
what is specific to that UI. Each takes the format above, titled with its UI,
such as `# Shell on iOS`.

An area's shared file holds its user stories and topics, and what its screens
show on every UI, such as their texts. `<platform>/<area>.md` holds the area's
screens on that UI, and what the UI does differently in the area's topics.

## The UI

The spec decides how each part of the UI looks as fully as the UI's code would.
Of the values that the code would set, the design system's components set
some, and the spec states all the others.

Write each part as a component: a component of the design system, one of those
with a change, or one of the product's own, built from the design system's
components and tokens. A component can have slots: parts that it leaves for the
product to fill, such as the headline of a list item. A screen is a component
too. Its layout holds its parts, and each part is a component, down to each
text and icon. So write a screen, and each part of it, as a tree: each
component, and what fills each of its slots.

### How a part looks

Wherever the spec says how a part of the UI looks, on a screen, in the shell or
in a component, say it in the first of these forms that fits:

1. **A component of the design system as it is.** Name the component, then give
   what fills each slot that the part uses. The design system decides the rest,
   such as the size, type and color of each slot and the space between them:

   ```md
   - **Member list item.** A Material 3 two-line list item:
     - Leading avatar: the member's profile photo.
     - Headline: the member's full name.
     - Supporting text: their role in the project.
   ```

2. **A component of the design system with a change.** Name the component, say
   only what differs from it, then give what fills its slots:

   ```md
   - **Member list item.** A Material 3 three-line list item, but with its
     headline on up to two lines and its supporting text on one:
     - Leading avatar: the member's profile photo.
     - Headline: the member's full name.
     - Supporting text: their role in the project.
   ```

3. **A component of the product's own.** Build it from the design system's
   components and tokens. Say how its parts are laid out, then give each part
   in these same forms:

   ```md
   - **Member card.** A Material 3 elevated card that holds two rows, 8dp
     apart:
     - First row: one run of text, on one line, cut with "…" where it doesn't
       fit: the member's full name, in `title-medium` and `on-surface`, then
       their username, in `body-medium` and `on-surface-variant`.
     - Second row: two columns, each half of the width, 16dp apart. Each holds
       text in `body-medium` and `on-surface-variant`, which wraps:
       - Left column: their role in the project.
       - Right column: their team.
   ```

A slot can hold another component. Where what fills a slot differs from one use
to the next, the component leaves the slot to its uses, and each use names the
component that fills it, such as a fact row in `components.md` and a use of it
on a screen:

```md
- **Fact row.** A row that holds the label at its start and the value at its
  end, at least 24dp apart:
  - Label: in `body-medium` and `on-surface-variant`.
  - Value: what the use gives: a text, or a component.
```

```md
- **Status.** A [fact row](components.md#fact-row):
  - Label: "Status".
  - Value: a Material 3 assist chip, with the status's icon as its leading
    icon and the status's name as its label.
```

Write a value as the design system's token for it: `surface-container-low`,
not `#f7f2fa`. Where the design system has no token for it, write a value on
the design system's grid, such as 8dp on a 4dp grid.

### Screens

Write an area's screens as a table, then a section for each screen:

```md
## Screens

| Screen  | Reached from          | Purpose                  |
| ------- | --------------------- | ------------------------ |
| Inbox   | opening the app       | find and open mail       |
| Message | choosing one in Inbox | read a message and reply |

### Inbox

- **Layout.** A Material 3 list-detail layout. The list pane holds the message
  list. In the expanded window size class, the detail pane holds the Message
  screen of the open message.
- **Message list.** A Material 3 list, with a three-line list item for each
  message, newest first:
  - Headline: the sender's name.
  - Supporting text: the subject on the first line, and the message's first
    line on the second.
  - Trailing supporting text: the time it arrived.
- **Compose.** A Material 3 extended FAB at the bottom right of the list pane,
  which opens New message:
  - Icon: `edit`.
  - Text label: "Compose".
```

The table has a row per screen: where the user reaches it from and what it is
for. A view that blocks what is behind it, such as a dialog, is a screen. A
menu, or a part of a screen that changes in place, is part of its screen. The
table is complete when every user story that the user carries out on a screen
has a screen that delivers it, and the user can reach every screen.

Each screen is then a section with its name. Give its layout, then each part it
holds, in order, with notes on its behaviour, each in a form of
[How a part looks](#how-a-part-looks). Where the layout names regions, describe
what each holds under its name.

A screen can link to sketches of it, as images or HTML files in `visuals/`,
such as one of a state or a window size class. Name each file for its screen,
state and window size class, such as `inbox-empty-compact.png`. A sketch is an
example of the text, and the text wins on conflict.

### shell.md

The regions that every screen shares, such as the navigation, the notices for
the whole product or its search. Every screen fills the space that the regions
leave. Write the shell in the form of a screen:

```md
# Shell

- **Layout.** <Where each region is, and what changes in each window size
  class.>
- **<Region>.** <What the region holds, in order, with notes on its
  behaviour.>
```

### design_system.md

The design system that the UI is drawn in. A design system sets the look of a
UI for any product drawn in it, such as Material Design 3 or Apple's Human
Interface Guidelines. It has foundations: named visual values, such as color,
type, shape, elevation, spacing, motion and icons, held as tokens. It has
components built from them, such as a button or a card, which hold no
product's content. Some design systems also give patterns and writing
guidance, which the product follows by following the design system.

`design_system.md` names the design system that the product follows, with a
link to its spec, and gives only what that spec doesn't decide:

- the choices that it leaves to the product, such as the product's colors or
  its icon set;
- what only its examples show, such as the size of an icon in each component,
  as a table with a row per component;
- the product's own rules over many parts.

A rule of the product's own covers a kind of part or text, wherever it is. It
says what each such part does as exactly as the part's own description would,
such as by naming the component that it uses. Write one only when the user
asks for it. For example:

- "Titles, headings, labels and buttons are in sentence case."
- "A title, a tooltip or a button's label ends with no period."
- "An action that removes something does so at once, then shows a Material 3
  snackbar whose action is 'Undo'."

These are not rules of the product's own:

- An aim that no builder could check, such as "messages are short and
  friendly".
- A rule with an exception, such as "buttons are filled, but outlined on the
  Settings screen". A rule names no screen and no area. Where a part has to
  differ from a rule, ask the user whether to change the rule.
- A decision about one part, such as the text of an empty inbox. It goes where
  the part is.
- What the product does, such as how long a deleted message is kept. It goes
  in its area.

The rest of the UI goes in other files: the product's screens, and what they
hold, say or do, in the areas, and the components that screens share in
`components.md`.

```md
# Design system

<One sentence: the design system the UI follows, with a link to its spec.>

## <Foundation, such as Color, Type, Shape, Layout or Icons>

- <A choice that the design system leaves to the product, and what the product
  chooses.>

## <The kind of part or text that its rules cover, such as Capitalization>

- <A rule of the product's own, which holds for every part of its kind.>
```

A window size class is a range of window widths in which a layout stays the
same, such as compact or expanded. A screen's layout says what changes from one
window size class to the next.

### components.md

The product's own components that more than one screen uses. Parts on more
than one screen that look alike are one of two things:

- **One component,** when the user wants a change to one of them to change
  them all. It is a section here with the component's name: how it looks, in
  a form of [How a part looks](#how-a-part-looks), then how it works, such as
  what closes a dialog.
- **Parts that only look alike.** Each is written where it is, with its own
  values, even where they are the same as another's. Each can then change
  alone.

A component looks and works the same wherever it is used, so its definition
names nothing that uses it: no screen, no component that holds it, and no case
that only some of its uses have. Each use names it and fills each of its
slots. Where uses need a component to differ, each version is a component of
its own, with a name of its own. A component that only one screen uses is
written on that screen.

```md
# Components

## <Component>

<How it looks, in a form of "How a part looks".>

<How it works.>
```
