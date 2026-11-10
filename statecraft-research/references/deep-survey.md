# Deep survey

Read this only in Deep mode. A Chinese version is in `deep-survey.zh-CN.md`.

Cheng Bushi ran his army by the book: formations kept, watches beaten, every record in order, the men never at rest. Li Guang ran his with almost no paperwork. Neither was ever caught out, and Cheng's own verdict was that Li's army was easy and would be helpless against a sudden attack, while his own was burdensome and could not be surprised (李广军极简易，然虏卒犯之，无以禁也……我军虽烦扰，然虏亦不得犯我, *Records of the Grand Historian*, "General Li" 《史记·李将军列传》). Deep mode is Cheng Bushi's way. It costs what he said it costs, so use it when a surprise would be expensive: the survey will be published or relied on by others, the decision is hard to reverse, or the user asks for completeness.

## What Deep adds

- All six scouting entries, and a considered decision on a second pass.
- For a codebase: related issues, pull requests and recent changes reviewed, with the revision recorded.
- Every material claim checked by an independent verifier, with source, locator, provenance and verdict.
- A closest-work comparison (`novelty-check.md`) and a falsifiable plan (`experiment-design.md`) for every ranked idea.
- Coverage diagnostics in the report.

## Gates

A budget says when to stop; it does not say the evidence is sufficient. Claiming more than was done is the failure here (言大而功小者则罚, "he whose words are large and whose results are small is punished").

| Gate | Fast | Standard | Deep |
|---|---|---|---|
| Artefact inspected, where one exists | sampled | yes | yes, with revision recorded |
| Entries covered | one or two | three | all six, with a second pass considered |
| Issues, pull requests and recent changes reviewed (codebases) | no | sampled | yes |
| Closest prior work compared | no | for the top idea | for every ranked idea |
| Material claims checked | those the verdict rests on | shortlist | all, independently |
| Falsifiable experiment plan | no | for the top idea | for every ranked idea |

If a Deep run ends with a gate unmet, report it as the mode it actually reached and name the gate in the Status line.

## Coverage diagnostics

Report these with their caveats, in place of a single coverage figure:

- overlap between entries, and how many candidates each entry found that no other did
- whether the last round was still turning up new, decision-relevant candidates
- clusters you know of and did not cover

Do not turn overlap into an estimate of the size of the field. The usual formula assumes each entry samples the same population independently and evenly; search entries do neither, and the error can go either way (疑以传疑, "pass on the doubtful as doubtful").

## Report structure

```
# Title
## Status                       (mode reached; escalations; gates not met; constraint assumptions)
## Conclusions first
## What you probably don't know (ranked by new × usable, a sentence or two each, pointing into the body)
## Measured                     (data, code or repository inspected; revision; what was and was not measured)
## Map of the field             (by category, including the terminology map)
## Ideas                        (engineering value and novelty scored separately; closest-work table; falsifiable plan)
## Not recommended
## Verification log             (claims checked, count per verdict, each partly supported and unsupported claim listed)
## Coverage diagnostics and what was not found
## Sources
```

Write each section once. The verification log lists verdicts by claim id; it does not repeat the claims.
