---
name: stabilize-spec
description:
  Find why a product spec keeps needing clarification, and propose the rules
  and redesigns that would make it stable.
disable-model-invocation: true
---

# Stabilize spec

A stable spec settles the next case without an edit. When the same areas keep
needing clarification, the cause is usually in how the spec is written or how
the feature is conceived, not in the cases themselves. Find those causes from
the spec's history, and propose what would remove them.

## Process

### 1. Gather the evidence

The evidence is the spec's history of clarification: every issue, open and
closed, that asked what the spec means or reported a gap in it, and the git
history of the spec files. When the user names where clarifications are kept,
or an area to look at, start there. Otherwise find them: look
for how the project marks a question for a person, issues and pull requests
that quote or link the spec, and the spec edits that answered a question. Read
every one, and the spec text it touches as it stands in the working tree.

For each clarification, note the area it touches and its outcome: the spec
changed, it closed without a change, or it fixed text that had gone stale.

Set aside the clarifications caused by how they were filed rather than by the
spec, such as a run of issues closed without a change that stops when the
process that files them changed. Without this, an area can look fragile when it
isn't.

Done when every clarification has an area and an outcome, and the noise is set
aside with its reason.

### 2. Find the causes

For each area with clarifications, and each cluster of them about one concept,
look for these causes:

- **A list where a rule belongs.** A decision enumerates cases, or grew a case
  with each clarification. Propose the rule the cases follow, as spec-writing's
  "State the rule, not its cases" describes, and check that every listed case
  and the next one follow from it. The rule can be a convention, such as "every
  duration a command takes is written the way the settings write it".
- **A restatement.** A fact decided in one area is repeated in another and has
  drifted from it, or will. Propose stating it once and referring to it.
- **A rule with no home.** Pretend the whole spec is one document: where would
  the decision sit? When the answer is a chapter that doesn't exist, propose it
  as a new area, and test it: map every decision that would move into it, and
  report the overlaps, the gaps and the terms used in two senses that the
  mapping turns up.
- **A poorly conceived feature.** Several clarifications circle one concept,
  and each fix patched where the trouble showed instead of its cause, or broke
  one of the spec's principles. Ask what really drives the need: list every
  decision that relies on the concept, and find which of them need the property
  that causes the trouble. Then use the prior-art skill to find a design
  without it.
- **Hidden state.** The product keeps state outside the facts its platform
  designed to hold it, and parses it back, such as text in comments or names.
  Propose deriving it from facts the platform keeps, or a change in the process
  that makes it unneeded.

Done when every clarification is explained by a cause, or marked as a one-off
decision the spec rightly left to be asked.

### 3. Walk the flows

Write out one or two whole journeys through the product as the spec would read
with your proposals: each step, who acts, and the state after it. Note every
contradiction or gap the walk exposes.

## Output

Report, without editing the spec:

- the clarifications by area and outcome, with the noise set aside and why;
- each cause, with the clarifications that show it, the proposal written as
  the decision text, and what it settles;
- what the walk through the flows exposed;
- the order you'd decide them in.

When the user picks a proposal, decide and write it with the spec-writing
skill.
