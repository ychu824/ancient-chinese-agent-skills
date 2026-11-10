# Novelty collision check

Read this before ranking any idea as a candidate for a paper. A Chinese version is in `novelty-check.zh-CN.md`.

知彼知己者，百战不殆；不知彼而知己，一胜一负 ("Know the other side and know yourself, and a hundred battles bring no danger. Know yourself but not the other side, and you win one and lose one." *The Art of War*, "Attack by Stratagem" 《孙子·谋攻》). An idea examined only from the inside is the second case: it looks new because nobody looked at who else has done it.

Finding a plausible optimisation is not evidence that it is new. A broad label such as "streaming-aware scheduling" or "consistency regularisation" almost always has prior systems or papers behind it, and neighbouring projects often solve the same problem under another name.

## How to look

1. State the idea as a mechanism, not a label: what is decided, on what signal, to optimise what.
2. Search for that mechanism from at least three sides:
   - papers on the same problem, including under the other names in the terminology map
   - neighbouring systems and runtimes that serve the same kind of workload
   - the target project's own issues, pull requests and roadmap
3. Pick the closest two or three works. Closest means the largest overlap in mechanism, not the most famous.
4. Open each one and read the part that overlaps. An abstract is not enough to tell a difference in assumptions from a difference in wording.

## The closest-work table

One row per prior work, for each idea:

| Prior system or paper | Overlapping mechanism | Difference in assumptions, algorithm or objective | Evidence | Unresolved questions |
|---|---|---|---|---|
| name, link, year | what it already does that the idea also does | what the idea would do differently, stated concretely | where in the source this was read, and whether it was checked | what you could not determine |

## Reading the result

- If the difference column says only "applied to a different model" or "in a different codebase", the idea is an engineering contribution. It may still be worth doing; score it under engineering value.
- If a difference rests on something you could not read (closed source, a paper that would not open), say so in the last column and do not count it as a difference.
- If the search found nothing close, write "not found in this search" and list what was searched. Never write "no prior work exists".
- No idea is ranked as a paper candidate without at least one filled row. An idea with no row is a hypothesis awaiting this check.
