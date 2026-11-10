# A falsifiable experiment plan

Read this when turning an idea into its smallest experiment. A Chinese version is in `experiment-design.zh-CN.md`.

胜兵先胜而后求战，败兵先战而后求胜 ("The winning army secures victory first and then seeks battle; the losing army fights first and then looks for victory." *The Art of War*, "Dispositions" 《孙子·形篇》). Decide what counts as winning before running anything. A plan that fixes its threshold after seeing the numbers can only succeed.

多算胜，少算不胜 ("Much calculation wins; little calculation does not." 《孙子·计篇》).

## What the plan must contain

| Part | What to write |
|---|---|
| Hypothesis | one sentence that the experiment could show to be false |
| Baselines | what the idea is compared against, including the strongest simple alternative |
| Controlled conditions | workload, hardware, model, data and configuration, fixed and written down so the run can be repeated |
| Metrics | the quantities measured, including at least one that could get worse. For a serving system, for example: time to first output, tail latency, throughput, utilisation, and output quality |
| Ablations | which parts are switched off to show where the effect comes from |
| Expected trade-offs | what the idea is likely to cost, stated before the run |
| Success threshold | the size of effect that would count, and the variation between runs it has to exceed |
| Stop/go criteria | the result at which you stop, and the result at which you invest more |
| If it fails | what you would try or conclude next |

## Rules of thumb

- **Do not assume the knob is valid everywhere.** A setting that works for one model may be meaningless or harmful for another. Say which models or datasets the plan covers.
- **Compare like with like.** Vary one policy at a time under matched load. For an adaptive policy, the baselines include each fixed setting it chooses between.
- **Include a quality metric whenever speed is the goal.** A faster result that is worse is a different trade-off, not a win.
- **Estimate noise first.** Run the baseline more than once before interpreting a difference.
- **Mark the cost.** Give the order of magnitude in compute and time, and label it as inferred unless it was measured.
- **Start with the cheapest version that could fail.** If the smallest experiment cannot refute the hypothesis, it is a demonstration and should be called one.
