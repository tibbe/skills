---
name: product-prior-art
description:
  Find how others have answered how a product behaves or how it is built, and
  recommend an answer. Use while writing a spec, before you recommend how the
  product behaves or how it is built.
---

# Product prior art

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
