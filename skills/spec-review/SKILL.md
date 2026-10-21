---
name: spec-review
description:
  "Review the changes to a product spec since a fixed point (commit, branch,
  tag, or merge-base): does the changed text hold with the rest of the spec
  and follow the rules of spec-writing? Runs the review in a sub-agent. Use
  when the user wants to review changes to the spec, or asks to \"review the
  spec since X\"."
---

Review of the spec diff between the working tree and a fixed point the user
supplies: does the changed text hold with the rest of the spec and follow the
rules of spec-writing?

The review runs as a **sub-agent** so this session's context doesn't colour it.

## Process

### 1. Pin the fixed point

Whatever the user said is the fixed point (a commit SHA, branch name, tag,
`main`, `HEAD~5`, etc.). If they didn't specify one, ask for it.

Capture the diff command once:
`git diff <fixed-point> -- docs/specs GLOSSARY.md` (two-dot against the
working tree, so uncommitted edits count, and limited to the spec's paths).
Also note the list of commits via
`git log <fixed-point>..HEAD --oneline -- docs/specs GLOSSARY.md`.

Before going further, confirm the fixed point resolves
(`git rev-parse <fixed-point>`) and the diff is non-empty. A bad ref or empty
diff should fail here, not inside the sub-agent.

### 2. Spawn the sub-agent

**Sub-agent prompt** should include:

- The diff command and commit list.
- The spec's paths: every file under `docs/specs`, and `GLOSSARY.md` if it
  exists.
- The brief: "Call the Skill tool for 'spec-writing', then read the whole
  spec, then the diff. Check each changed clause against the rest of the spec
  and report every place it breaks a rule under spec-writing's 'While
  deciding' or departs from its 'Format'. For each finding quote the changed
  line, the spec or skill text it fails against with its file, and the fix as
  spec text. Under 400 words."

### 3. Report

Present the report verbatim or lightly cleaned. End with a one-line summary:
the total findings, and the worst one (if any).
