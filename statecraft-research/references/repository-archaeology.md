# Repository archaeology

Read this when the survey is about a codebase or system: finding an optimisation, a research contribution, or a gap in an existing project. A Chinese version is in `repository-archaeology.zh-CN.md`.

夫地形者，兵之助也……知此而用战者必胜，不知此而用战者必败 ("Terrain is the soldier's ally. He who knows it and fights will win; he who does not know it and fights will lose." *The Art of War*, "Terrain" 《孙子·地形篇》). A proposal to change a system made without reading it is a march across ground nobody scouted.

不用乡导者，不能得地利 ("Without local guides one cannot gain the advantage of the ground." 《孙子·军争篇》). In a repository the local guides are the issues, pull requests and commit history: they say what has been tried, what was rejected and why.

## What to establish before recommending anything

1. **Pin the revision.** Record the repository, the commit hash and the date. A fast-moving project can invalidate a finding within weeks, and a finding without a revision cannot be rechecked.
2. **Find the execution path.** For the model or workload in question, identify the entry points and follow the request from input to output. Write down the files and functions on that path.
3. **Map what is already there.** For the area you might propose to change, list the mechanisms that exist: scheduling, batching, chunking, compilation, caching, transport between stages. An idea that duplicates an existing mechanism is not an idea.
4. **Read the project's own record.** Search open and merged pull requests, open and closed issues, benchmarks and recent commits for the area. Look for work in progress, rejected attempts and stated roadmap.
5. **Separate what was observed from what is supposed.** For each candidate bottleneck, say which of these it is:
   - measured: you ran it, or the project publishes a measurement, with the conditions
   - read: the code shows it, with a link to the lines
   - hypothesis: plausible from the design, not confirmed

## What to hand back

For each codebase-specific recommendation:

| Field | Content |
|---|---|
| Where | links to the files and lines on the implementation path, at the pinned revision |
| What exists now | the current mechanism, in a sentence or two |
| Related project history | issues and pull requests that touch it, with their state |
| Evidence | measured / read / hypothesis, and what has not been measured |
| Proposed change | what would be different |

A recommendation that cannot fill in "Where" is not ready to be ranked.

## Cautions

- Do not run downloaded code unless the user has asked for that and the environment allows it. Reading is usually enough to map the path.
- Sampling the README and a few issues is a pilot, and should be reported as one.
- An unmeasured opportunity is not a defect in the project. Describe it as a hypothesis, and do not open issues or pull requests on the user's behalf.
