---
name: product-prior-art
description:
  Find how others have answered how a product behaves or how it is built, and
  recommend an answer. Use while writing a spec, before you recommend how the
  product behaves or how it is built.
---

# Product prior art

## Process

### 1. Research

Before you recommend how the product behaves or how it is built, assume the
question has been answered before, and research how in parallel sub-agents, so
each search stays out of the others' context and yours. How others answer it is
a fact: look it up rather than ask the user or rely on memory, which is often
wrong in the details.

1. Frame the question from what the user and the spec have settled, and
   nothing else: the kind of product it is for, the behaviour to decide, its
   quality attributes, and the platform it runs on.
2. Spawn one sub-agent per kind of prior art the question has, together, in
   the foreground:
   - **Platform**: the guidelines and standards of the platform the framing
     names, such as Apple's Human Interface Guidelines, Material Design or
     RFC 3339.
   - **Products**: how other products of the kind the framing names, and
     others facing the same problem, answer the question.

   Give each the framing and this brief, and nothing else: "Search the web for
   how this question is answered. Report every principled option the sources
   take, each with its trade-offs, the evidence of its adoption, why its
   adopters chose it, and the sources it comes from. Evidence of adoption: the
   platform providing or documenting it; products like ours using it, with each
   product's users, downloads or GitHub stars; and, for a library to adopt, its
   GitHub stars, package downloads and recent releases. Report them condensed
   and complete: one entry per option, leaving out no option, evidence or
   source, and nothing else. Where sources conflict, give both."

### 2. Criteria

Once every research sub-agent has returned its report, list every option the
reports give. Then, for each decision, write its criteria: the quality
attributes the framing states, and performance, wherever the reports' trade-offs
show the options can differ on them. Word each so that more of it is better, one
concern to each. Mark as a must-have any the user or the spec states outright,
and rank the rest High, Medium or Low by how much the decision's success depends
on them.

### 3. Choose

For each decision:

1. Rule out any option that fails a must-have.
2. Of the rest, take the consensus as the reference: the most widely adopted,
   by the evidence of adoption the reports give, counting what the platform
   documents for this question as strong evidence.
3. Rate each other option against the reference on each criterion as better,
   same, worse or unknown, with the reason and the report it comes from.
4. Once every option is rated, improve and combine options, the ones step 1
   ruled out among them:
   - **Improve:** where an option rates well but for one criterion, because of
     one part of it, change that part.
   - **Combine:** where two options are each better on a different criterion,
     put the parts that make each better into one option.

   Rate each new option with the rest, from what the reports say of its parts.
5. Where a rating is unknown, say whether the choice would change if it went
   either way.
6. Follow the reference unless another option's better ratings outweigh its
   worse ones, weighed by each criterion's rank and by how far apart the
   options are on it, not by their count.

Before choosing, write down the case against each option. The case against the
consensus first says why its adopters chose it, then why that reasoning doesn't
apply here. Strike out any reason, for or against an option, that:

- counts the size of the change;
- leans on what the code does today;
- is circular: it assumes the choice it argues for.

Choose on the reasons that remain.

## Output

For each decision, put its criteria, with their ranks, and its options to the
user, each option with its source in a few words, recommend the one you chose,
and give the reason against each of the others.
