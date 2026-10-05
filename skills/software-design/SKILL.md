---
name: software-design
description:
  Design the code before changing it. Use before implementing a feature or
  fixing a bug.
---

# Software design

Decide how the code should look so it reads as if it had been designed for what
it now does from the start.

## Process

### 1. Prior art

Assume the problem has been solved before, and research how in parallel
sub-agents, so each search stays out of the others' context and yours.

1. Frame the problem: the behavior the change must deliver, and the parts of the
   codebase's stack that shape the solution, such as its language and main
   frameworks.
2. Spawn one sub-agent per kind of prior art the problem has:
   - **Language**: the language's idiomatic patterns for this problem.
   - **Library**: the library's or framework's own recommended approach, from
     its documentation.
   - **Applications**: how other applications handle this behavior.

   Give each the framing and this brief: "Search the web for how this problem is
   solved. Report every principled design the sources take, each with its
   trade-offs and the sources it comes from. Under 400 words."

### 2. Choose

Compare the reported designs and choose one. Prefer the design that makes
illegal states unrepresentable: precise types and data structures, and a smart
constructor where the type system can't express an invariant. For a bug fix,
find the design under which the bug cannot occur. Choose the design that best
solves the problem, even if it differs from how the code works today. Reshaping
the current code to fit is the job of the Plan step.

### 3. Plan

Make the change easy, then make the easy change. List every place the current
code departs from the chosen design: those parts are restructured first, so the
change itself only adds to the new structure.

## Output

End with the design, for the implementation that follows:

- the chosen design and why, with its sources;
- the restructuring the change needs first, and then the change.
