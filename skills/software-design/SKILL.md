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

1. Frame the problem: the behavior the change must deliver, the quality
   attributes the spec states, and the language and platform it must run on. The
   libraries and components the code builds on today are part of the design,
   open to change like the rest.
2. Spawn one sub-agent per kind of prior art the problem has:
   - **Library**: the library's or framework's own recommended approach, from
     its documentation.
   - **Applications**: how other applications of the same kind as ours, and
     others facing the same problem, handle this behavior.

   Give each the framing and this brief: "Search the web for how this problem is
   solved. Report every principled design the sources take, each with its
   trade-offs, the evidence of its adoption, why its adopters chose it, and the
   sources it comes from. Evidence of adoption: the platform's SDKs providing or
   documenting it; applications like ours using it; and, for a library to adopt,
   its GitHub stars, package downloads and recent releases. Under 400 words."

### 2. Choose

Compare the reported designs and, for each decision, follow the consensus: the
design most sources converge on, and above all a solution the platform's SDKs
provide for this problem. Any other design, whether fewer sources take it or
none do, has to win against it.

Before choosing, write down the case against each design. The case against the
consensus first says why its adopters chose it, then why that reasoning doesn't
apply here. Strike out any reason that:

- counts how much code the design rewrites;
- leans on what the code already does;
- is circular: it assumes the choice it argues for.

Choose on the reasons that remain. Reshaping the current code to fit is the job
of the Plan step.

### 3. Plan

Make the change easy, then make the easy change. List every place the current
code departs from the chosen design: those parts are restructured first, so the
change itself only adds to the new structure.

## Output

End with the design, for the implementation that follows:

- the chosen design, where it follows or departs from the consensus and why,
  with its sources;
- the restructuring the change needs first, and then the change.
