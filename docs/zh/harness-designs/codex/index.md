# 拆解 Codex 的 harness 设计

OpenAI 的 [Codex](https://openai.com/index/harness-engineering/) 可能是四款产品里和"harness 原教旨"绑定最深的一个——那篇定义了整个领域名字的《Harness Engineering》文章，本身就是 OpenAI 团队用 Codex 写产品时的经验总结。所以拆 Codex 的 harness 设计，很大程度上就是拆那篇文章背后的工程实践。

Codex 的哲学可以浓缩成一句话：**仓库即事实来源（repository as the system of record），AGENTS.md 只是目录页，工程的价值在于设计环境、表达意图、构建反馈循环。**

## 一句话定位

OpenAI 团队用 Codex 在几周内交付了一个最终上百万行代码的产品，**每一行代码都是 Codex 写的**（原文见 [Harness Engineering](https://openai.com/index/harness-engineering/) 的 "Designing for growth" 一节）。他们的实践回答了一个问题：当工程师的角色从"写代码"变成"设计 harness"时，系统该怎么组织。Codex CLI 本身是开源的单体二进制（Rust 实现，[github.com/openai/codex](https://github.com/openai/codex)），但它对 harness 的贡献主要在**约定（convention）**和**上下文工程**上，而不是在花哨的扩展点上。

## 指令子系统：AGENTS.md 是目录页，不是百科全书

这是 Codex 对 harness 理论最有影响力的一条设计：

> 单个巨型指令文件不利于机械化检查（覆盖率、更新状态、所有权、交叉链接），偏离现实的情况无法避免。因此我们不再把 AGENTS.md 视为百科全书，而是将其视为**目录页**。代码库知识位于结构化的文档中，AGENTS.md 负责指向它们。

（以上是 [《Harness Engineering》原文](https://openai.com/index/harness-engineering/) 中 "AGENTS.md should be a directory page" 一节的直接转述。）

课程第四讲说"单个巨型指令文件会失败"，Codex 直接给出了正解：AGENTS.md 控制在 100 行左右（原文建议约 100 行，接近上限就拆到 `docs/`），放不下就拆分到 `docs/` 目录，让 agent 按需去读。这是"给地图，不给说明书"的权威出处。

配套的原则叫**执行不变量，不要微管实现**（原文："don't micromanage the implementation；focus on invariants"）：AGENTS.md 只写不可违反的硬约束和验证命令，具体怎么实现交给模型。这直接对应课程第二讲"约束而非微操"。

## 上下文子系统：Write-Select-Compress-Isolate

Codex 的上下文工程可以概括为四个策略，这是社区在 "context engineering" 成为独立学科后总结出来再映射回 Codex 的框架（框架出处见 [Context Engineering for Codex CLI](https://codex.danielvaughan.com/2026/06/10/context-engineering-codex-cli-write-select-compress-isolate-june-2026/)）：

- **Write（写出去）**：把上下文持久化到窗口之外——结论写进文档、状态写进文件，而不是留在对话里。对应"仓库即事实来源"。
- **Select（选进来）**：只把需要的 token 拉进窗口——AGENTS.md 指路、按需读文件，而不是全仓塞进去。
- **Compress（压缩）**：保留真正重要的——Codex 有自动压缩和手动 `/compact`，可以自定义 `compact_prompt`（见 [Context Engineering for Codex CLI](https://codex.danielvaughan.com/2026/06/10/context-engineering-codex-cli-write-select-compress-isolate-june-2026/)）。
- **Isolate（隔离）**：把上下文切到不同边界——用子智能体（subagent）隔离不同任务的上下文，一个前端子智能体永远看不到后端的数据库 schema。

Codex 还有一个很细的环境上下文设计：社区对 [codex-harness-internals](https://github.com/AlexKenbo/codex-harness-internals) 的源码分析显示，`build_environment_update_item` 只在环境变化时输出**变更字段**（CWD、git 分支、文件系统），而不是每轮都把完整系统上下文粘一遍。这是"上下文里不养重复 token"的工程细节。

## 工具与边界：worktree 隔离 + 子智能体

Codex 的两个核心 harness 机制：

**1. git worktree 隔离环境。** [《Harness Engineering》原文](https://openai.com/index/harness-engineering/) 的 "Environment" 一节写明：每个任务在独立的 git worktree 里运行，配合本地的可观测性栈（日志、指标、追踪），让每个变更都在独立环境中验证。这就是课程第七讲"给 agent 划清每次任务的边界"的物理实现——边界不是靠指令恳求，而是靠环境隔离强制。环境（environment）子系统在这里被做成了硬隔离。

**2. 内核级子智能体。** Codex 的 `spawn_agent` / `wait_agent` 是内核级工具：模型显式地创建子智能体、给它独立的会话历史与工具集、等待结果。子智能体继承父级的 AGENTS.md 指令，但运行在**自己的上下文**里。配置放在 `.codex/agents/*.toml`，可以指定不同模型和指令（细节见 [Context Engineering for Codex CLI](https://codex.danielvaughan.com/2026/06/10/context-engineering-codex-cli-write-select-compress-isolate-june-2026/) 的 Sub-agents 一节）。这是"上下文隔离"的直接实现——也是课程第十二讲"交接"精神的体现：每个子智能体是一个有清晰边界的工作单元。

## 反馈子系统：验证命令写进规范

OpenAI 实践里最强调的一点：在 AGENTS.md 里显式列出验证命令，把"怎么确认做对了"变成仓库的一部分。Codex 的工程流程里，测试、CI、文档、可观测性配置——全部由 Codex 生成，并且全部是"可执行的验证路径"。模型能力强但不可靠的解法，不是祈祷模型自觉，而是让**验证路径成为 harness 的默认组件**。

审批策略（approval policies）和计划模式（plan mode）则是反馈的另一个方向：在执行高风险操作前先出计划、先要审批，把"任务边界"和"人类决策权"做成运行时控制。

## 映射到课程框架

| 子系统 | Codex 的实现 | 评价 |
| --- | --- | --- |
| 指令 | AGENTS.md 目录页 + docs/ 拆分 + 执行不变量 | 教科书级，定义了"给地图不给说明书" |
| 工具 | worktree 隔离 + spawn_agent 子智能体 | 边界靠环境硬隔离，很强 |
| 环境 | 独立 worktree + 可观测性栈 | worktree 隔离是其招牌 |
| 状态 | Write 策略（状态写进文件/文档） | 依赖约定而非内建记忆 |
| 反馈 | 验证命令入规范 + 审批策略 + plan mode | 反馈路径默认化，值得抄 |

Codex 和 Claude Code 的对比很有意思：Claude Code 是"加法"——把记忆、权限、子智能体全做进内核；Codex 是"减法"——内核尽量克制，把更多责任放在仓库约定和上下文工程上。这也是为什么社区常说"Codex 的 harness 哲学比它的代码更值钱"。

## 值得借鉴的设计

1. **AGENTS.md 当目录页写**：控制在 100 行左右，指向 docs/ 里的细节，可机械化检查。
2. **只写不变量，不微管实现**：硬约束 + 验证命令，剩下的交给模型。
3. **用 worktree 做环境隔离**：任务边界靠环境强制，不靠指令恳求。
4. **环境上下文只传增量**：每轮只输出变更字段，别重复粘贴完整系统上下文。
5. **子智能体做上下文隔离**：拆任务的同时拆上下文，别让子任务污染主循环。

## 参考来源（原文 / 源码）

每条论断都能回溯到下面的原文或源码，避免凭印象转述：

- **OpenAI《Harness Engineering》**：AGENTS.md 目录页与约 100 行建议、executive invariants / don't micromanage、worktree 隔离 + 可观测性栈、验证命令入规范、上百万行产品案例、审批策略与 plan mode。本篇所有核心论断的主要出处。<br/>https://openai.com/index/harness-engineering/
- **OpenAI 官方《AGENTS.md》规范**（AGENTS.md 作为跨工具约定的标准）：<br/>https://openai.com/index/agents-md/
- **Codex CLI 开源仓库**（Rust 实现的单体二进制）：<br/>https://github.com/openai/codex
- **Context Engineering for Codex CLI**（社区）：Write-Select-Compress-Isolate 框架、`/compact` 与 `compact_prompt`、`spawn_agent` / `wait_agent` 子智能体与 `.codex/agents/*.toml` 配置。<br/>https://codex.danielvaughan.com/2026/06/10/context-engineering-codex-cli-write-select-compress-isolate-june-2026/
- **codex-harness-internals**（社区源码分析）：`build_environment_update_item` 增量环境上下文等实现细节。<br/>https://github.com/AlexKenbo/codex-harness-internals

相关讲义：[第三讲 · 让代码仓库成为唯一的事实来源](../lectures/lecture-03-why-the-repository-must-become-the-system-of-record/) ｜ [第四讲 · 把指令拆分到不同文件里](../lectures/lecture-04-why-one-giant-instruction-file-fails/) ｜ [第七讲 · 给 agent 划清每次任务的边界](../lectures/lecture-07-why-agents-overreach-and-under-finish/)
