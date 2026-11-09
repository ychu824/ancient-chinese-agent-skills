# ancient-chinese-agent-skills

Agent skills distilled from ancient Chinese classics（孙子兵法、韩非子、左传、战国策、史记、三十六计）for directing a fleet of AI agents.

御人之术讲的是委托-代理问题：怎么分工、怎么立约、怎么验功、怎么授权和收权。这些问题在管一组 agent 时原样出现，只是失败模式换了：agent 的问题不是私心，而是幻觉、context 丢失和误差累积。这个仓库把能迁移的部分写成可以直接加载的 skill，并保留每条规则的出处和它防的是哪种失败。

## Skills

| Skill | 用途 | 状态 |
|---|---|---|
| [`statecraft-research`](statecraft-research/SKILL.md) | 组织一组 agent 做调研：多入口并行探查、独立核查、按证据等级交付。针对两种失败：漏掉你不知道存在的东西，和交回编造或不相关的东西。 | v0.1，评估中 |

完整的古籍 → 机制 → agent 做法对照表（九层 24 条，含 code review 场景）在 [`statecraft-research/references/framework.md`](statecraft-research/references/framework.md)。

## 结构

```
statecraft-research/
├── SKILL.md                  # 流程：探 → 验 → 判 → 报，brief 模板
└── references/
    └── framework.md          # 完整对照表、三十六计 threat model、类比失效处
```

## 安装

Skill 是一个带 `SKILL.md` 的文件夹。把 `statecraft-research/` 整个目录放到你的 agent 读取 skill 的位置即可，例如 Claude Code 的 `~/.claude/skills/` 或项目里的 `.claude/skills/`。

## 评估方法

每个 skill 都和"不带 skill"的 baseline 用完全相同的 prompt 对照，量三件事：

- 报告里的条目经独立核查后有多少被一手来源支持
- 两组各自找到了哪些对方没找到的东西
- token 和用时

Skill 只有在某一项上明显好于 baseline 时才算成立。评估结果会随版本更新在这里。

## 关于类比的边界

古籍讲的是管人。下面几点是 agent 和人不同、类比不成立的地方，使用时要记得：

- **赏罚没有对象**：激励要换成筛选，"罚"只能是拒收、重试、收权。
- **换将零成本**：纠正一个跑偏的 agent 通常不如丢掉 context 重开。
- **稀缺资源是 context，不是忠诚**：防结党、权谋制衡这类术基本用不上。
- **放权程度取决于验证成本**，而不是通信距离。
