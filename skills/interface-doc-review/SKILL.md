---
name: interface-doc-review
description: Review the changes since a fixed point (commit, branch, tag, or merge-base) to find possible improvements to interface documentation. Use when the user wants to review a branch, a PR, work-in-progress changes, or asks to "review since X".
---

# Interface Documentation Review

Review a module's interface documentation.

## Process

### 1. Pin the fixed point

Whatever the user said is the fixed point — a commit SHA, branch name, tag, `main`, `HEAD~5`, etc. If they didn't specify one, ask for it.

Capture the diff command once: `git diff <fixed-point>...HEAD` (three-dot, so the comparison is against the merge-base).

Before going further, confirm the fixed point resolves (`git rev-parse <fixed-point>`) and the diff is non-empty. A bad ref or empty diff should fail here — not inside the sub-agent.

### 2. Spawn the sub-agent

Send one `Agent` tool call. Use the `general-purpose` subagent.

**Review sub-agent prompt** — include:

- The diff command from step 1.
- The **Review criteria**, pasted in full — the sub-agent has no other access to them.
- The brief: "Apply the rules to every comment in the diff, and report each one that fails: its file and line, which smell it fails (name it from the list), the current text, and the exact rewrite — or that it should be deleted."

### 3. Aggregate

Present the report, verbatim or lightly cleaned. Do not merge or rerank findings.

End with a one-line summary: total findings, and the worst issue.

## Review criteria

### Glossary

**Module** — anything with an interface and an implementation. Deliberately scale-agnostic: a function, class, package, or tier-spanning slice. Avoid: unit, component, service.

**Interface** — everything a caller must know to use the module correctly: the type signature, but also invariants, ordering constraints, error modes, required configuration, and performance characteristics. Avoid: API, signature (too narrow — they refer only to the type-level surface).

**Implementation** — what's inside a module, its body of code.

### The general principle

The interface documentation should describe succinctly the contract between the module and its client. The contract should say what the module does rather than how it does its job.

### The two-sided test

Liskov & Guttag frame spec quality as a balance between restrictiveness and generality (their ch. 9 has sections by those names). Two checks you can state as rules:

1. **Sufficiently general** — could you replace the module with a completely different algorithm and leave the documentation unchanged? If not, the documentation over-specifies.  
2. **Sufficiently restrictive** — could a hostile-but-conforming implementer satisfy every word of your documentation and produce something useless to a client? If so, it under-specifies.

### List of specific smells

Each smell reads what it is → how to fix; match it against the diff:

* **History** — mentioning previous versions of the code (i.e. how we got here) → cut it.  
* **Rejected alternatives** — defending the design against an approach you dropped → cut it (it belongs in e.g. commit messages and ADRs).  
* **Negative documentation** — listing what the code doesn't do → keep it only when the negation counters a default a stranger would actually assume; that's rare.  
* **Implementation leakage** — describe the contract; implementation notes go inline, next to the code.  
* **Caller-specific framing** — naming a function that calls this, or calling behavior "the X flow" → cut it.  
* **Coding agent session references** — "as you asked", "per our discussion". The subtler form has no tell-phrase: a fact reads as worth stating only because it just came up → cut it.
