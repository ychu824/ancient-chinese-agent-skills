# Benchmark

How `statecraft-research` is compared against a baseline, and the numbers recorded so far.

Issue [#3](https://github.com/ychu824/ancient-chinese-agent-skills/issues/3) sets provisional targets for v0.3: 40–60% fewer tokens and 30–50% less wall time than the previous skill, with no meaningful loss of key-source recall or claim accuracy. These are hypotheses. Nothing below tests them yet.

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

One comparison has been run: v0.1 against no skill, three tasks, one run per cell, on 2026-11-09. It used the Chinese text of v0.1.

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

## Not yet measured

- v0.2 and v0.3 have not been run against any baseline.
- No task has been repeated, so there is no estimate of variance.
- Known-work recall, label correctness and candidates per 1,000 tokens have not been scored for any version.
- Fast and Deep modes have not been run.
