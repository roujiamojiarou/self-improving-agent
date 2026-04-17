# self-improving-agent# Self-Improving Agent

一个让 AI Agent 持续自我进化的轻量机制。无需复杂架构，只需要三个文件和一个人类审批节点。

## 核心理念

AI 在执行任务时会犯错、会被纠正、会发现更好的做法——这些经验通常随 session 结束就丢了。这个机制让 AI **在错误发生时就记录**，并通过人类审批把重要经验**合并进工作区配置**，实现真正的累积进化。

## 文件结构
workspace/
├── .learnings/
│ ├── ERRORS.md # 每次工具执行失败时立即追加
│ ├── LEARNINGS.md # 用户纠正我、或我发现更好做法时追加
│ └── FEATURE_REQUESTS.md # 缺失能力记录
└── [skill目录]/
├── SKILL.md # 主文件（正文）
└── PATCH.md # 待合并的改进建议

Copy

## 循环机制
每次 session:
发现错误 → 立即追加到 .learnings/ERRORS.md
被纠正 → 立即追加到 .learnings/LEARNINGS.md
发现更好做法 → 追加到 .learnings/LEARNINGS.md

人类说"批准" → 我把对应内容合并到 SKILL.md 正文
人类说"不用改" → 我删除对应内容

Copy

## 触发条件

| 触发 | 立即执行 | 记录位置 |
|------|---------|---------|
| 命令/工具执行失败 | ✅ | `.learnings/ERRORS.md` |
| 用户纠正（"No..." "Actually..."） | ✅ | `.learnings/LEARNINGS.md` |
| 发现知识过时或有误 | ✅ | `.learnings/LEARNINGS.md` |
| 发现更好的做法 | ✅ | `.learnings/LEARNINGS.md` |
| 用户要求不存在的能力 | ✅ | `.learnings/FEATURE_REQUESTS.md` |

## 合并规则

**不要做的事：**
- ❌ "等会话结束再记录"
- ❌ 只在脑子里想"下次记住"
- ❌ 用户指出了才想到记录

**自我检查：**
> 如果我刚执行了一个工具调用并得到了错误，先检查是否已将该错误记录到 `.learnings/ERRORS.md`，再继续其他工作。

## 晋升规则

当一个 pattern 被验证有效时，把它从行为变成规则：

- 行为模式 → `SOUL.md`
- 工作流改进 → `AGENTS.md`
- 工具坑点 → `TOOLS.md`

## 动机

大多数 Agent 框架关注的是"怎么让 AI 做更多事"，而不是"怎么让 AI 记住自己做错过什么、改进过什么"。这个机制填补了后者。

受 [OpenClaw Self-Improving Agent skill](https://github.com/openclaw/openclaw) 启发并重构。

---

*欢迎 fork 和改进。如果你用了这个机制，欢迎在 Issues 里分享你的 `.learnings/` 案例。*
