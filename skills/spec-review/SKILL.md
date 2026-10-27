---
name: spec-review
description:
  "Review the changes to a product spec since a fixed point (commit, branch,
  tag, or merge-base) or the uncommitted ones along two axes: Consistency (does
  the changed text hold with the rest of the spec?) and Writing (does the
  changed text follow the rules of spec-writing?). Runs both reviews in
  parallel sub-agents and reports them side by side. Use when the user wants
  to review a branch, a PR or uncommitted changes to the spec, or asks to
  \"review the spec since X\"."
---

Two-axis review of the spec diff between the working tree and a fixed point,
`HEAD` unless the user supplies one:

- **Consistency**: does the changed text hold with the rest of the spec?
- **Writing**: does the changed text follow the rules of spec-writing?

Both axes run as **parallel sub-agents** so they don't pollute each other's
context, then this skill aggregates their findings.

## Process

### 1. Pin the fixed point

Whatever the user said is the fixed point (a commit SHA, branch name, tag,
`main`, `HEAD~5`, etc.). If they didn't specify one, or asked for the
uncommitted changes, it is `HEAD`.

Capture the diff command once:
`git diff $(git merge-base <fixed-point> HEAD) -- docs/specs GLOSSARY.md`
(from the merge-base to the working tree, so the comparison is against the
point the branch left and uncommitted edits count, limited to the spec's
paths). Also note the list of commits via
`git log <fixed-point>..HEAD --oneline -- docs/specs GLOSSARY.md`.

Before going further, confirm the fixed point resolves
(`git rev-parse <fixed-point>`) and the diff is non-empty. A bad ref or empty
diff should fail here, not inside two parallel sub-agents.

### 2. The tests

Each axis carries its **tests** below, drawn from ISO/IEC/IEEE 29148 and the
INCOSE Guide for Writing Requirements. Each names its class of finding, says
whether its verdict is a hard violation or a judgement call, and reads _what
it is_ → _how to test it_.

The Consistency axis matches each test against each changed clause, and the
dangling test against each clause the diff removed or moved:

- **Contradicts** (hard): the changed clause and a clause elsewhere cannot both
  hold. → Quote both; no reading satisfies the two.
- **Restates** (hard): two clauses decide the same thing. → Remove either; the
  other still decides it. A decision belongs once, at the widest level it
  holds: a principle in `product.md`, else a shared area, else a platform file.
  A reference that names a decision and links to where it is made, as "as
  under [Repository](repository.md)", is not a match.
- **Dangles** (hard): a clause elsewhere refers, by name or by link, to a
  term, heading or decision the diff removed or renamed. → Quote the reference
  and the removed text.
- **Undecided** (hard): a case the changed clause admits, for which no clause
  gives the outcome. → State the case from the clause's own conditions, such
  as a set it ranges over being empty, and quote the clause; no clause in the
  spec decides the case.

The Writing axis matches each test against each changed clause:

- **Implied** (judgement call, worded "possible implied"): the changed clause
  follows from a decision already in the spec, so a builder gets two things to
  keep true. → Remove the clause; a builder reading the rest could build
  nothing a user would notice as different. A clause marked "for example",
  "such as", "unlike" or "but for" is an example or an exception, not implied.
  The signs:
  - a qualifier a general word already covers: "including gift card orders"
    after "every paid order";
  - a consequence of the rule before it: "a shipped order can't be cancelled"
    after "a customer can cancel an order until it ships";
  - what another area already decides: "a cancelled order is refunded" in
    Ordering when Payments decides refunds;
  - a tautology: "a paid invoice is one the customer has paid";
  - a detail no decision depends on;
  - what the product doesn't do, when no decision says it does.
- **Form** (hard): the changed text departs from spec-writing's Format. →
  Cite the Format rule it departs from.

### 3. Spawn both sub-agents in parallel

Issue both sub-agent calls together, in the foreground, and aggregate the
reports they return.

**Consistency sub-agent prompt** should include:

- The full diff command and commit list.
- The spec's paths: every file under `docs/specs`, and `GLOSSARY.md` if it
  exists, **plus the Consistency tests from step 2** pasted in full (the
  sub-agent has no other access to them).
- The brief: "Report, per file/clause where relevant, (a) every clause
  elsewhere that the diff contradicts; (b) every decision it restates, naming
  the widest level the decision holds at; (c) every clause left dangling by
  text it removed or moved; and (d) every case the changed text admits that
  no clause decides. Quote the changed or removed text and the clause
  elsewhere, with its file, for each finding. Under 400 words."

**Writing sub-agent prompt** should include:

- The diff command and commit list.
- The spec's paths, as above, **plus the Writing tests from step 2** pasted
  in full.
- The brief: "Call the Skill tool for 'spec-writing' for its Format. Report,
  per file/clause where relevant, (a) every place the diff departs from
  Format: cite the rule and quote the clause; and (b) any possibly implied
  clause you spot: name the sign, quote the clause and the decision it
  follows from. Distinguish hard violations from judgement calls: Format
  breaches can be hard, but implied is always a judgement call. Under 400
  words."

### 4. Aggregate

Present the two reports under `## Consistency` and `## Writing` headings,
verbatim or lightly cleaned. Do **not** merge or rerank findings, because the
two axes are deliberately separate (see _Why two axes_).

End with a one-line summary: total findings per axis, and the worst issue
_within each axis_ (if any). Don't pick a single winner across axes: that's
the reranking the separation exists to prevent.

## Why two axes

A change can pass one axis and fail the other:

- Text that follows every writing rule but contradicts a decision elsewhere →
  **Writing pass, Consistency fail.**
- Text that holds with the rest of the spec but adds a clause another
  decision already implies → **Consistency pass, Writing fail.**

Reporting them separately stops one axis from masking the other.
