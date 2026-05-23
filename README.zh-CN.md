# Co-Scientist Agent Suite

中文说明 | [English](README.md)

这是一个**模型无关、跨平台可移植的科研 subagent 套件**，灵感来自 Nature 论文：

Gottweis 等，"Accelerating scientific discovery with Co-Scientist"，Nature，2026。

论文原文：<https://www.nature.com/articles/s41586-026-10644-y>

本仓库不是 Google、DeepMind、Gemini 或 Nature 的官方实现。它把论文公开描述的 Co-Scientist 架构，转写成可在 Codex、Claude Code、Cursor、Antigravity、TRAE、GitHub Copilot 等 AI coding 平台中复用的 agent 指令。

## 简明介绍

Co-Scientist Agent Suite 是一套用于科研构思的可移植 agent 模板。它把一个科研任务拆成多个协作角色：生成假设、批判假设、聚类相似想法、排序候选方向、进化更强版本、总结剩余弱点，并设计验证路线。

它的目标不是替代科学家，而是让科研假设生成过程更结构化、更可审计、更可证伪。

## 设计目标

Nature 论文中的 Co-Scientist 是一个结构化科研思维引擎，包含：

- 科学家参与的自然语言交互界面。
- Supervisor 负责调度的任务框架。
- 专门化 agents：Generation、Reflection、Ranking、Evolution、Proximity、Meta-review。
- 用于长程推理的上下文记忆。
- 用于文献 grounding 和专业科学模型调用的工具使用机制。
- 默认评价标准：alignment、plausibility、novelty、testability、safety。
- 对生成假设进行真实生物医学验证的工作流。

本套件在 prompt / instruction 层面保留这些架构思想，但不依赖 Gemini 2.0，也不复刻原文的内部工程系统。

## 与原始系统的差异

这是一个可移植的 instruction-level 重构，不是原始 Co-Scientist 系统的源码复现。

主要简化包括：

- **不包含 Gemini 内部基础设施**：原文系统使用 Gemini-based agents 和内部工程框架；本套件保持模型无关。
- **不保证真实异步运行时**：支持原生 subagents 的平台可以并行或异步运行；不支持的平台使用顺序执行降级方案。
- **默认没有真实持久化 memory**：本套件定义了可移植的 `Context Ledger` 格式，但是否持久化取决于宿主平台或仓库文件。
- **默认没有自动 Elo 引擎**：Ranking agent 定义了 Elo-style tournament 协议，但分数存储和更新需要平台支持或显式文件。
- **不内置数据库访问**：文献、预印本、临床试验、GEO、药物、蛋白结构等工具都是可选集成。
- **不自动执行湿实验验证**：Validation agent 只负责生成验证计划。

其中 `co-scientist-validation` 是一个工程化扩展。Nature 论文报告了真实世界验证流程，但没有把 Validation 列为核心 specialized agent。

## 仓库结构

```text
co-scientist-agent-suite/
  README.md
  README.zh-CN.md
  AGENTS.md
  CLAUDE.md
  GEMINI.md
  subagents/
    co-scientist-supervisor.md
    co-scientist-generation.md
    co-scientist-reflection.md
    co-scientist-ranking.md
    co-scientist-evolution.md
    co-scientist-proximity.md
    co-scientist-meta-review.md
    co-scientist-validation.md
  workflows/
    co-scientist-discovery-loop.md
  docs/
    nature-architecture-map.md
    platform-support.md
    tool-contract.md
    context-ledger-template.md
    local-codex-skill-bindings.example.md
    build-with-plugin-creator.md
  .cursor/rules/
    co-scientist.mdc
  .github/
    copilot-instructions.md
```

## Subagents

| 论文角色 | 文件 | 作用 |
| --- | --- | --- |
| Supervisor | `subagents/co-scientist-supervisor.md` | 解析研究目标、分配阶段、维护停止规则 |
| Generation | `subagents/co-scientist-generation.md` | 生成多样化科研假设和研究方案 |
| Reflection | `subagents/co-scientist-reflection.md` | 批判正确性、新颖性、假设前提和安全性 |
| Ranking | `subagents/co-scientist-ranking.md` | 执行 pairwise / tournament-style 排序 |
| Evolution | `subagents/co-scientist-evolution.md` | 在不覆盖父假设的前提下改进强候选 |
| Proximity | `subagents/co-scientist-proximity.md` | 聚类、去重、映射相似假设 |
| Meta-review | `subagents/co-scientist-meta-review.md` | 总结评审模式并指导下一轮迭代 |
| Validation | `subagents/co-scientist-validation.md` | 工程化扩展：把 top 假设转成分阶段验证计划 |

## 快速使用

### 通用方式

把 `AGENTS.md` 保留在仓库根目录。大多数现代 coding agents 都可以读取或被引导读取仓库级说明。

### Claude Code

如果希望使用 Claude Code 的原生 subagent 路由，可将 `subagents/` 中的文件复制到：

```text
.claude/agents/
```

### Codex

保留 `AGENTS.md` 和 `subagents/`。使用时让 Codex 参考根目录 `AGENTS.md` 和对应 subagent 文件。

如果想把这套 agent suite 打包成 Codex 插件，请看 `docs/build-with-plugin-creator.md`。在 Codex 里可以这样说：

```text
调用 $plugin-creator，帮我把 https://github.com/VivalavidaLu/co-scientist-agent-suite 构建成一个 Codex 插件。
```

### Cursor

使用：

```text
.cursor/rules/co-scientist.mdc
```

### GitHub Copilot

使用：

```text
.github/copilot-instructions.md
AGENTS.md
```

### Antigravity

使用：

```text
AGENTS.md
GEMINI.md
```

### TRAE

使用 `AGENTS.md` 作为可移植项目级说明；如果你的 TRAE 环境支持自定义 agents，可导入 `subagents/` 中的角色提示词。

## 科学完整性原则

本套件强制遵守以下规则：

- 不把假设写成已验证结论。
- 未做文献检索前，不声称“新颖性”。
- 区分 verified facts、data-supported inferences、literature-grounded hypotheses 和 exploratory ideas。
- 关键输入缺失时暂停，而不是自行脑补。
- 优先设计可证伪的验证路线，而不是宏大的空泛课题。
- 不从体外实验或 in silico 结果推断临床疗效。

## 可选工具集成

生物医学任务中，如果宿主平台能提供数据库或搜索工具，本套件效果会更好。推荐的可选能力包括：

- PubMed / MEDLINE：同行评议生物医学文献。
- bioRxiv / medRxiv：近期预印本。
- ClinicalTrials.gov：临床转化和试验状态。
- GEO 或其他 omics 数据库：公开表达数据集。
- 药物、靶点、通路和蛋白结构数据库。

公开仓库中不要写死本机 skill 路径。应使用抽象能力描述，并在工具缺失时记录为 `unmet_evidence_requirements`。详见 `docs/tool-contract.md`。

### 推荐 Skills 来源

如果需要现成的科学数据库和分析 skills，可以参考：

- <https://github.com/K-Dense-AI/scientific-agent-skills>

该仓库提供科学数据库、科研写作、数据分析等 Agent Skills，可作为 PubMed、bioRxiv、ClinicalTrials.gov、GEO、药物数据库、通路数据库、omics 分析等能力的可选来源。

安全提示：只安装你需要的 skills，安装前阅读对应 `SKILL.md`，不要把第三方 skills 当作自动可信依赖。

## 范围限制

本套件不提供：

- 原始 Co-Scientist 源码。
- Gemini 内部基础设施。
- 自动文献访问或湿实验执行能力。
- 在没有用户数据或可验证来源时验证科学结论的能力。
