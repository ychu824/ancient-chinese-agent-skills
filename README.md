# ancient-chinese-agent-skills

**English** | [中文](README.zh-CN.md)

Agent skills distilled from ancient Chinese classics, for directing a fleet of AI agents.

The classical writings on statecraft (御人之术, "the art of directing people") are about the principal–agent problem: how to divide work, how to fix a contract, how to check results against it, and how to grant and withdraw authority. The same questions come up when one person directs many agents. What changes is the failure mode. A minister fails through self-interest; an agent fails through hallucination, lost context and compounding error. This repository keeps the parts that transfer, writes them as loadable skills, and records for each rule which text it comes from and which failure it guards against.

## Skills

| Skill | Purpose | Status |
|---|---|---|
| [`statecraft-research`](statecraft-research/SKILL.md) | Run a research survey with a group of agents: independent scouting from several entry points, separate verification, and a report graded by evidence level. It targets two opposite failures: missing what the reader does not know exists, and handing back fabricated or irrelevant material. | v0.1, evaluated below |

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

## Principles

A selection. The full table (nine layers, 24 rows, with practices for both research and code review) is in [`statecraft-research/references/framework.md`](statecraft-research/references/framework.md), in Chinese.

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

## Evaluation of `statecraft-research` v0.1

The runs below used the Chinese text of the skill, now `SKILL.zh-CN.md`. The English `SKILL.md` is a translation of it and has not been evaluated separately.

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
├── SKILL.md                  # the workflow: scout → verify → judge → report, with brief templates
├── SKILL.zh-CN.md            # the same in Chinese
└── references/
    └── framework.md          # full table, the Thirty-Six Stratagems as threat model, limits of the analogy (Chinese)
```

## Install

A skill is a folder containing a `SKILL.md`. Copy the whole `statecraft-research/` folder to wherever your agent loads skills from, for example `~/.claude/skills/` for Claude Code, or `.claude/skills/` inside a project.
