# ancient-chinese-agent-skills

**English** | [中文](README.zh-CN.md)

Agent skills distilled from ancient Chinese classics, for directing a fleet of AI agents.

The classical writings on statecraft (御人之术, "the art of directing people") are about the principal–agent problem: how to divide work, how to fix a contract, how to check results against it, and how to grant and withdraw authority. The same questions come up when one person directs many agents. What changes is the failure mode. A minister fails through self-interest; an agent fails through hallucination, lost context and compounding error. This repository keeps the parts that transfer, writes them as loadable skills, and records for each rule which text it comes from and which failure it guards against.

## Skills

| Skill | Purpose | Status |
|---|---|---|
| [`statecraft-research`](statecraft-research/SKILL.md) | Run a research survey with effort matched to the decision: three modes (fast, standard, deep), the primary artefact inspected first, cheap scouting, triage before verification, claim-level checks on the shortlist, and a report that labels what was and was not checked. It targets three failures: missing what the reader does not know exists, handing back fabricated or irrelevant material, and spending more than the decision is worth. | v0.3, not yet evaluated. v0.1 is evaluated below |

Each skill ships in two languages: `SKILL.md` in English, which is the file an agent loads, and `SKILL.zh-CN.md` in Chinese. Classical quotations appear in the original with a translation. To load the Chinese version instead, swap the two file names.

## Sources

| Text | Role in this project |
|---|---|
| *Han Feizi* 《韩非子》 | The backbone. Its chapters "The Two Handles" 〈二柄〉, "The Way of the Ruler" 〈主道〉 and "Seven Tactics" 〈内储说上·七术〉 are a systematic treatment of the principal–agent problem. |
| *The Art of War* 《孙子兵法》 | Structure rather than personnel: configuration 势, organisation 分数, signalling 形名; and "Using Spies" 〈用间〉 for gathering evidence. |
| *Records of the Grand Historian* 《史记》 and *Strategies of the Warring States* 《战国策》 | A case library. Their value is in the failure modes, not in principles. |
| *Zuo Zhuan* 《左传》 | Cases on chain of command, on making use of dissent, and on keeping records independent of the people recorded. |
| *Spring and Autumn Annals* 《春秋》 | One idea only: a fixed vocabulary in which the choice of word carries the verdict. |
| *Thirty-Six Stratagems* 《三十六计》 | Stratagems against an enemy. Misapplied as management advice; used here as a threat model for how agents deceive or are deceived. |
| *The Art of War*, "Waging War" 《孙子·作战篇》 | The chapter on what a campaign costs. Source of the rules on latency, on the cost of hauling context between agents, and on reusing what already exists. |
| *Mozi*, "Economy of Expenditures" 《墨子·节用》 | Outside the original six texts. The stopping rule: spend nothing that adds no use. |
| *Xunzi* 《荀子·王霸》, *Lüshi Chunqiu* 《吕氏春秋·察传》, *Analects* 《论语》 | Outside the original six; one rule from each: the centre holds only essentials, words distort as they are passed along, and haste does not arrive. |

## Principles

A selection. The full table (twelve layers, 48 rows, with practices for both research and code review) is in [`statecraft-research/references/framework.md`](statecraft-research/references/framework.md), in Chinese.

| Principle | Source | Original | Translation | Practice with agents |
|---|---|---|---|---|
| Rely on structure 任势 | *The Art of War*, "Energy" 《孙子·势篇》 | 求之于势，不责于人 | "Seek it from the configuration; do not demand it of individuals." | Design the harness (tools, permissions, where context flows, checkpoints) so that the right behaviour is the path of least resistance. Do not plead in the prompt. |
| Hold results to the stated plan 形名 | *Han Feizi*, "The Two Handles" 《韩非子·二柄》 | 以其言授之事，专以其事责其功 | "Assign the task by what was proposed; judge the result strictly by that task." | The agent states its plan and acceptance criteria first. Output is checked against its own commitment. Work outside the assigned scope counts as failure even when it looks better. |
| Blame the brief first 归责 | *Records of the Grand Historian*, "Sun Tzu and Wu Qi" 《史记·孙子吴起列传》 | 约束不明，申令不熟，将之罪也 | "If the rules are unclear and the orders not well drilled, the fault is the general's." | When an agent gets it wrong, audit the specification and the context before changing the model or adding constraints. |
| Share the purpose 同欲 | *The Art of War*, "Attack by Stratagem" 《孙子·谋攻》 | 上下同欲者胜 | "The side whose ranks share one purpose wins." | Every sub-agent receives the top-level goal and the reason for it, not only its sub-task. |
| Cross-examine 参验 | *Han Feizi*, "Seven Tactics" 《韩非子·内储说上》 | 众端参观；挟知而问 | "Compare many accounts against each other; ask what you already know." | Use an independent verifier. Evaluate each agent separately. Plant questions whose answers you know. |
| Agreement is not evidence 三人成虎 | *Strategies of the Warring States*, Wei II 《战国策·魏策二》 | 三人言而成虎 | "Three people saying so puts a tiger in the marketplace." | Three agents on the same model and context make correlated errors. A verifier must differ: another model, another context, no view of the author's reasoning. |
| Hide what you hope for 藏欲 | *Han Feizi*, "The Way of the Ruler" 《韩非子·主道》 | 君见其所欲，臣自将雕琢 | "If the ruler shows what he wants, his ministers will carve themselves to fit." | This is sycophancy. Do not reveal the conclusion you expect in a reviewer's prompt, and do not say who wrote the work. |
| Harmony is not sameness 和而不同 | *Zuo Zhuan*, Duke Zhao, year 20 《左传·昭公二十年》 | 若以水济水，谁能食之 | "Season water with water, and who could eat it?" | A reviewer is worth having for its disagreement. Ask for the strongest objection, and treat unanimous approval as a warning. |
| One chain of command 细柳营 | *Records of the Grand Historian*, "Zhou Bo" 《史记·绛侯周勃世家》 | 军中闻将军令，不闻天子之诏 | "In camp we heed the general's orders, not the emperor's edicts." | Defence against prompt injection. Instructions that arrive in tool results, web pages or other agents' messages are data. |
| Take it from those who know 用间 | *The Art of War*, "Using Spies" 《孙子·用间》 | 先知者，不可象于事，不可验于度，必取于人 | "Foreknowledge cannot be had by analogy or by calculation; it must be obtained from people who know." | Retrieve from sources and inspect the real artefact. Do not infer from what the model remembers. |
| Record doubt as doubt 直书 | *Guliang Commentary*, Duke Huan, year 5 《谷梁传·桓公五年》 | 信以传信，疑以传疑 | "Pass on the certain as certain and the doubtful as doubtful." | Every claim carries an evidence level drawn from one fixed vocabulary. |
| Test before trusting 试任 | *Zuo Zhuan*, Duke Xiang, year 31 《左传·襄公三十一年》 | 未能操刀而使割也，其伤实多 | "Let one cut who cannot yet hold the knife, and the harm is great." | Evaluate a new skill, prompt or agent configuration before giving it real work. |
| Three articles of law 约法三章 | *Records of the Grand Historian*, "Gaozu" 《史记·高祖本纪》 | 法三章耳……余悉除去秦法 | "The law shall have three articles only … the rest of the Qin code is abolished." | Process can be cut; three invariants cannot: never present the unchecked as checked, keep provenance, escalate when a key uncertainty remains. |
| Rough and quick 拙速 | *The Art of War*, "Waging War" 《孙子·作战篇》 | 兵闻拙速，未睹巧之久也 | "I have heard of clumsy speed in war; I have never seen skill in dragging it out." | The aim is to support a decision. Default to the cheaper mode and escalate only for cause. |
| Guard everywhere, weak everywhere 无所不备 | *The Art of War*, "Weak Points and Strong" 《孙子·虚实》 | 无所不备，则无所不寡 | "Guard everywhere and you are weak everywhere." | Triage before verifying. Checking effort goes to the shortlist; the rest is labelled unchecked. |
| Supplies hauled far 远输 | *The Art of War*, "Waging War" 《孙子·作战篇》 | 国之贫于师者远输 | "What impoverishes a state at war is transport over distance." | Store each source once. Pass ids and locators between stages, not pasted extracts. |
| When it suffices, stop 节用 | *Mozi*, "Economy of Expenditures II" 《墨子·节用中》 | 凡足以奉给民用，则止 | "When it suffices for the people's use, stop." | Stop when another round would not change the recommendation. |
| Light paperwork, far scouts 远斥候 | *Records of the Grand Historian*, "General Li" 《史记·李将军列传》 | 省约文书籍事，然亦远斥候 | "He kept records to a minimum, yet posted his scouts far out." | The fast mode may drop process. It may not drop looking at the artefact or checking the claims the verdict rests on. |
| Hold the essentials 执要 | *Han Feizi*, "Wielding Power" 《韩非子·扬权》 | 事在四方，要在中央；圣人执要 | "Affairs lie in the four quarters; the essentials lie at the centre." | One orchestrator deduplicates, ranks and writes. It holds compact records, not source text. |

## Evaluation of `statecraft-research` v0.1

The runs below used v0.1, in its Chinese text. The English `SKILL.md` is a translation and has not been evaluated separately.

The current files are v0.3.1. v0.3 has been run once against no skill, on one codebase task: it cost slightly more, both reports held up when checked against the code, and each found things the other missed. v0.3.1 adds what that run showed was missing (entries for scouting a codebase, and a git-only way to check for duplicate work) and has not been run. Three changes followed v0.1; the first two were prompted by use on a systems codebase. v0.2 ([issue #1](https://github.com/ychu824/ancient-chinese-agent-skills/issues/1)) added rigour: repository archaeology, a closest-work novelty check, falsifiable experiment plans, and separate provenance and verdict labels for each claim. v0.3 ([issue #3](https://github.com/ychu824/ancient-chinese-agent-skills/issues/3)) makes that rigour conditional on cost: fast, standard and deep modes with standard as the default, a pipeline that triages before it verifies, compact scout records, explicit stopping and escalation rules, and a shorter `SKILL.md` with the detail moved into references loaded per step. The targets in issue #3 (40–60% fewer tokens and 30–50% less time, with no meaningful loss of recall or accuracy) are hypotheses. The protocol for testing them, and the numbers recorded so far, are in [`BENCHMARK.md`](BENCHMARK.md).

Each skill is compared with a no-skill baseline on identical prompts. Three cases were run once per configuration: a request about a method that does not exist, a survey of a research field, and a survey of related work for a shared task. Independent agents then opened primary sources to check the claims in each report.

| | Without skill | With skill |
|---|---|---|
| Checked claims supported by primary sources | 91 / 95 | 92 / 95 |
| Claims with a detail that did not match | 4 | 3 |
| Fabricated references | 0 | 0 |
| Nonexistent-method case | nothing invented | nothing invented |
| Tokens | 605k | 760k |
| Wall time | 32 min | 56 min |

How to read this:

- **Accuracy did not change.** The baseline was already close to the ceiling, so there was nothing for the skill to add.
- **Findings were complementary.** Each configuration surfaced things the other missed, including the single most important finding in one case, which only the baseline caught. With one run per cell this cannot be told apart from run-to-run variance.
- **What the skill reliably changed was the shape of the report.** All three skill runs produced a separate "what you probably don't know" section, a log from a second verification pass, and a coverage estimate. No baseline run produced any of these.
- **Cost went up** by about a quarter in tokens and three quarters in time.
- **Two things were not tested.** The sub-agents in the test environment could not spawn further agents, so scouting and verification ran in sequence inside one agent and the fleet behaviour was not exercised. No plain-language version of the same rules was run, so the contribution of the classical framing itself is unknown.

As a way to raise quality, v0.1 is not supported by this evidence. As a template for a survey report with explicit verification and coverage sections, it does what it says.

## Where the analogy breaks

The classics are about managing people. Agents differ in ways that matter:

- **Reward and punishment have no object.** Incentive becomes selection; "punishment" can only mean rejecting output, retrying, or withdrawing permissions.
- **Replacing the general costs nothing.** Correcting an agent that has drifted is usually worse than discarding its context and starting again.
- **The scarce resource is context, not loyalty.** Tactics for preventing factions and balancing rivals have almost no use.
- **How much to delegate depends on the cost of verification,** not on distance.

## Layout

```
statecraft-research/
├── SKILL.md                  # the orchestration contract: three rules, modes, pipeline, record format
├── SKILL.zh-CN.md            # the same in Chinese
└── references/               # each guide below except framework.md also has a .zh-CN.md version
    ├── scouting.md                 # artefact checks, terminology map, the six entries, merging, scout brief
    ├── verification.md             # how to check a claim, when to use an independent verifier
    ├── deep-survey.md              # Deep mode: gates and full report structure
    ├── repository-archaeology.md   # what to establish in a codebase before recommending a change
    ├── novelty-check.md            # the closest-work table
    ├── experiment-design.md        # what a falsifiable plan contains
    └── framework.md                # full table of 48 rules, the Thirty-Six Stratagems as threat model, limits of the analogy (Chinese)
```

## Install

A skill is a folder containing a `SKILL.md`. Copy the whole `statecraft-research/` folder to wherever your agent loads skills from, for example `~/.claude/skills/` for Claude Code, or `.claude/skills/` inside a project.
