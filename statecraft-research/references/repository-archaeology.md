# Repository archaeology

Read this when the survey is about a codebase or system: finding an optimisation, a research contribution, or a gap in an existing project. A Chinese version is in `repository-archaeology.zh-CN.md`.

夫地形者，兵之助也……知此而用战者必胜，不知此而用战者必败 ("Terrain is the soldier's ally. He who knows it and fights will win; he who does not know it and fights will lose." *The Art of War*, "Terrain" 《孙子·地形篇》). A proposal to change a system made without reading it is a march across ground nobody scouted.

不用乡导者，不能得地利 ("Without local guides one cannot gain the advantage of the ground." 《孙子·军争篇》). In a repository the local guides are the issues, pull requests and commit history: they say what has been tried, what was rejected and why.

## What to establish before recommending anything

1. **Pin the revision.** Record the repository, the commit hash and the date. A fast-moving project can invalidate a finding within weeks, and a finding without a revision cannot be rechecked.
2. **Find the execution path.** For the model or workload in question, identify the entry points and follow the request from input to output. Write down the files and functions on that path.
3. **Map what is already there.** For the area you might propose to change, list the mechanisms that exist: scheduling, batching, chunking, compilation, caching, transport between stages. An idea that duplicates an existing mechanism is not an idea.
4. **Read the project's own record.** Search open and merged pull requests, open and closed issues, benchmarks and recent commits for the area. Look for work in progress, rejected attempts and stated roadmap. If the host's API and search are out of reach, use the fallback below.
5. **Separate what was observed from what is supposed.** For each candidate bottleneck, say which of these it is:
   - measured: you ran it, or the project publishes a measurement, with the conditions
   - read: the code shows it, with a link to the lines
   - hypothesis: plausible from the design, not confirmed

## Entries for a codebase

水因地而制流，兵因敌而制胜 ("Water shapes its course by the ground; an army shapes its victory by the enemy." 《孙子·虚实篇》). The six entries in `scouting.md` are aimed at literature. When the question is where to contribute, what to fix or what to optimise in a project, scout from these instead. The mode still sets how many.

| Entry | The question it asks |
|---|---|
| By tracker | What does the project say it wants (labels, roadmap, RFCs, tracking issues), and who has already claimed it |
| By the code's own debt | What does the project's own tooling say is unfinished: checker allowlists and ratchet budgets, hooks skipped in CI, tests no CI job selects, lint rules not yet enabled, documentation links and paths that no longer resolve |
| By the negative | Rejected or stalled pull requests, changes maintainers declined, issues the current code contradicts |
| By process | Contribution rules, the evidence a change must carry, how review and merging are queued and how long changes wait |
| By upstream and neighbours | What the parent project or sibling projects fixed or adopted that this one has not |
| By execution path | For a performance or correctness question: the path from input to output, items 2 and 3 above |

Two cross-checks pay for themselves. A label or an open issue is a claim: check it against the current code and its linked pull requests before counting it as available work. And a defect found in the code is a candidate only until the project's record has been checked for someone already fixing it.

## When the host's API and search are unavailable

管仲曰：老马之智可用也。乃放老马而随之，遂得道 ("Guan Zhong said: the wisdom of an old horse can be used. They let the old horses go and followed them, and so found the road." 《韩非子·说林上》). When the guide cannot be asked, follow the tracks the project left in git.

- **Open and recent pull requests.** On GitHub each pull request is published as git refs. List them with `git ls-remote <remote> 'refs/pull/*/merge'`, fetch them into a separate bare repository, and compare each snapshot's version of the files you would change with the pinned revision. A snapshot that already differs at your lines is someone else's work on the same thing. Other hosts publish equivalent refs under other names.
- **Merged history.** `git log --grep` and `git log -S<string>` on a clone deep enough to cover the period. A shallow clone cannot say when something was removed or why.
- **Fetch only.** None of this writes to the remote or needs credentials on a public repository.

What this does not see: issues and review discussion, pull requests with no merge ref (those that conflict with the base branch), and anything not yet pushed. Merge refs can also linger for pull requests that were later closed. Report the result as "not found in N pull-request snapshots", say which of these blind spots apply, and leave the tracker search as something for the user to do before acting.

## What to hand back

For each codebase-specific recommendation:

| Field | Content |
|---|---|
| Where | links to the files and lines on the implementation path, at the pinned revision |
| What exists now | the current mechanism, in a sentence or two |
| Related project history | issues and pull requests that touch it, with their state; if the tracker could not be searched, how duplication was checked and what that check cannot see |
| Evidence | measured / read / hypothesis, and what has not been measured |
| Proposed change | what would be different |

A recommendation that cannot fill in "Where" is not ready to be ranked.

## Cautions

- Do not run downloaded code unless the user has asked for that and the environment allows it. Reading is usually enough to map the path.
- Sampling the README and a few issues is a pilot, and should be reported as one.
- An unmeasured opportunity is not a defect in the project. Describe it as a hypothesis, and do not open issues or pull requests on the user's behalf.
