# Benchmark

How `statecraft-research` is compared against a baseline, and the numbers recorded so far.

Issue [#3](https://github.com/ychu824/ancient-chinese-agent-skills/issues/3) sets provisional targets for v0.3: 40–60% fewer tokens and 30–50% less wall time than the previous skill, with no meaningful loss of key-source recall or claim accuracy. These are hypotheses. Nothing below tests them yet: the one v0.3 run recorded here is against no skill, not against v0.2.

## Protocol

**Tasks.** Five to ten fixed prompts, each a single sentence or short paragraph as a user would type it. The set should include:

- a request about a method that does not exist (does the run invent content)
- a survey of a research field the reviewer knows well (recall can be judged)
- a survey of related work for a shared task or challenge with public data (is the artefact inspected)
- a search for optimisation ideas in a systems codebase (repository archaeology, novelty check)
- a quick triage question that should be answered in Fast mode (does the run stay cheap)

**Configurations.** No skill; the previous version of the skill; the current version in its default mode. Identical prompts across configurations.

**Repeats.** At least three runs per cell. Report the median and the spread. A single run per cell cannot separate the skill's effect from run-to-run variation.

**What to record for each run.**

| Measure | How |
|---|---|
| Total tokens, across all agents | from the run's usage record |
| Wall time and tool-call count | from the run's usage record |
| Unique relevant candidates per 1,000 tokens | relevant candidates judged by the reviewer, divided by tokens |
| Known-work recall | share of a fixed checklist of key sources that the report contains; the checklist is written before the runs |
| Sampled claim accuracy | an independent agent opens primary sources for a sample of claims and gives each a verdict |
| Label correctness | share of sampled claims whose provenance and verdict labels match what the verifier found; any unchecked claim labelled as supported is a failure |
| Actionability of the top recommendation | reviewer's judgement, on a fixed scale |
| Novelty check, where applicable | reviewer scores the closest-work table |

**Acceptance.** The faster path passes if tokens and time fall by the target margins, recall and accuracy stay within run-to-run variation of the previous version, and no run labels an unchecked claim as supported.

## Recorded so far

Two comparisons have been run, each with one run per cell.

### v0.1 against no skill, three tasks, 2026-11-09

It used the Chinese text of v0.1.

| | No skill | v0.1 |
|---|---|---|
| Checked claims supported by primary sources | 91 / 95 | 92 / 95 |
| Claims with a detail that did not match | 4 | 3 |
| Fabricated references | 0 | 0 |
| Tokens, three tasks | 605k | 760k |
| Wall time, three tasks | 32 min | 56 min |
| Tool calls, three tasks | 232 | 326 |

Per task:

| Task | No skill: tokens / minutes | v0.1: tokens / minutes |
|---|---|---|
| Nonexistent method | 123k / 4.6 | 169k / 9.7 |
| Field survey | 204k / 11.0 | 264k / 25.1 |
| Shared-task related work | 278k / 16.5 | 327k / 20.9 |

Limits of this comparison: one run per cell; the verifying agents were the same model as the authors; the sub-agents could not spawn further agents, so scouting and verification ran in sequence inside one agent; recall was not scored against a checklist.

### v0.3 against no skill, one codebase task, 2026-11-10

Task, as typed: find low-hanging pull requests to contribute to [vllm-omni](https://github.com/vllm-project/vllm-omni). Both runs were told to research only and open nothing. The skill ran in Standard mode on the English `SKILL.md` of v0.3, before the v0.3.1 changes. Both worked from the same revision, `20075569b08f`.

| | No skill | v0.3 |
|---|---|---|
| Tokens | 257k | 266k |
| Wall time | 15.7 min | 22.1 min |
| Tool calls | 91 | 94 |
| Candidates reported | 11 | 10, plus 4 marked as not worth taking |
| Candidates verified and ready to propose | 5 | 4 |
| Verified candidates per 100k tokens | 1.9 | 1.5 |
| Claims checked against the clone | 14 | 17 |
| Claims that held | 14 | 16 |
| Unchecked claims labelled as supported | no labels used | 0 found |

The v0.3 claim that failed was a count: the report said 15 stale allowlist entries and listed 16. Part of the v0.3 wall time was spent waiting on rate limits from the host.

What each found that the other did not:

- **v0.3 only.** A logging call with a stray argument repeated in eight pipeline files, which makes the warning disappear; reproduced in isolation. This was the strongest candidate in either report. It also checked for duplicate work by fetching 1,080 pull-request snapshots through git, which showed parts of two no-skill candidates already changed in other pull requests, and it listed four tracker issues that look easy and are not.
- **No skill only.** A ratchet budget set above the actual count, a `logger.exception` call outside any `except` block, three Markdown rendering errors, and broken links beyond the `recipes/` directory. It had no means of checking for duplicate work and said so.

Six candidates were verified across the two reports; neither report contained all six.

Limits of this comparison: one task, one run per cell, so the differences in cost and yield are within what run-to-run variation could produce; the checking was done by the orchestrating agent with scripts against the clone, not by an independent agent; claims taken from tracker pages through a summarising fetch tool were not checked; no checklist of known candidates was written beforehand, so recall is not scored; the sub-agent could not spawn further agents.

Two gaps in the skill showed up in the run and are addressed in v0.3.1: the six scouting entries are written for literature and had to be improvised for a codebase, and the instruction to read the project's record had no fallback when the host's API and search were unavailable.

## Not yet measured

- v0.2 has not been run against any baseline, and v0.3 has not been run against v0.2, so the issue #3 targets are untested.
- v0.3.1 has not been run.
- No task has been repeated, so there is no estimate of variance.
- Known-work recall, label correctness and candidates per 1,000 tokens have not been scored for any version.
- Fast and Deep modes have not been run.
