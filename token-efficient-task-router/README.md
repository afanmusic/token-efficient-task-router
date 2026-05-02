# token-efficient-task-router

一个面向 WorkBuddy、iMA Copilot、OpenClaw、QClaw、CodeBuddy 及其他智能体系统的低消耗任务路由与执行控制 Skill。

English: A token-efficient control skill that classifies task clarity, complexity, risk, and expected cost before execution, then routes work into the safest and most efficient mode.

让智能体在真正开始消耗 token、读取大量上下文、修改文件或尝试高风险操作之前，先做判断，再决定如何行动。

English: It helps the agent decide before tokens, file edits, and broad execution start expanding.

## Current Version

- Current version: `v0.6.5`
- 技能主文件 / Skill file: [SKILL.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/SKILL.md)
- 对外说明 / Public guide: [GUIDE.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/GUIDE.md)
- 版本记录 / Change history: [CHANGELOG.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/CHANGELOG.md)
- 开源许可 / License: [LICENSE](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/LICENSE)

## 快速理解

这个 Skill 不是业务技能，而是一个“智能体工作方式控制层”。

它的核心作用是帮助智能体：

- 在执行前判断任务是否足够明确
- 在执行前判断复杂度、风险和 token 成本
- 在 Ask、Plan、Craft、Expert 之间进行路由
- 在高风险文件操作前建立确认闸门
- 在大任务里优先采用样本、分阶段、渐进式推进

English: This is not a domain skill. It is a control layer for agent behavior, focused on routing, safety, and token efficiency.

## 适合什么场景

当你面对以下情况时，这个 Skill 特别有用：

- 用户需求有些模糊，不适合直接执行
- 任务可能涉及文件、文件夹、项目结构或知识库
- 用户希望减少试错、降低 token 消耗
- 智能体需要先判断“应该诊断、先规划、直接做，还是建议专家介入”
- 任务可能存在大批量处理、长文输出、全量重构等高成本路径

English: Best for ambiguous, risky, multi-step, file-sensitive, or potentially expensive agent tasks.

## 不适合什么场景

这个 Skill 不应该拖慢本来就很简单的任务。

以下情形通常不需要重型路由流程：

- 一句话润色
- 简短翻译
- 单条命令解释
- 很短的模板生成
- 明确、低风险、一次即可完成的小任务

English: It should stay out of the way for small, clear, low-risk requests.

## 核心亮点

- 先判断，再执行，而不是默认直接展开
- 支持 `Ask`、`Plan`、`Craft`、`Expert` 四种模式
- 支持交互式任务路由确认，避免误判任务
- 支持渐进式交付，避免一上来就全量处理
- 支持文件安全闸门，减少误删、误改、误覆盖
- 支持 WorkBuddy、iMA Copilot、CodeBuddy 等不同平台的差异化行为

English: Key capabilities include mode routing, confirmation gates, progressive delivery, and platform-aware token control.

## 工作模式

### Ask Mode

用于诊断、解释、风险判断、可行性分析和下一步建议。

English: Use Ask Mode for diagnosis, explanation, and low-risk advisory work.

### Plan Mode

用于复杂任务、多步骤任务、文件整理、批量操作、知识库处理、重构、部署和其他中高风险执行。

English: Use Plan Mode when scope should be defined before execution.

### Craft Mode

用于明确、低风险、可以一次完成的小任务，强调快速交付，不做无谓铺陈。

English: Use Craft Mode for clear, low-risk, one-pass execution.

### Expert Mode

用于多次失败、专业性过高、继续试错不划算的情况，重点是引导进入更合适的专家视角。

English: Use Expert Mode when deeper specialization is more efficient than more trial-and-error.

## 这个 Skill 解决什么问题

很多智能体在真实工作里会出现同一类问题：

- 还没弄清需求，就先读很多材料
- 还没确定范围，就先大段输出
- 还没确认文件边界，就先动核心文件
- 还没证明“值得一次做完”，就先全量展开

这个 Skill 的目标不是让智能体更保守，而是让智能体更划算、更稳、更接近真实需求。

English: It solves over-reading, over-explaining, over-editing, and over-delivering before scope is validated.

## 风险与 Token 控制

Skill 内置了风险等级和 token 预算机制：

- `Low Risk`：小文本生成、轻量建议、明确的小修改
- `Medium Risk`：新建文件、单文件修改、文档整理、轻量脚本调整
- `High Risk`：删除、覆盖、批量重命名、批量移动、核心文件修改、部署或数据库变更
- `Critical Risk`：密钥、Cookie、未知远程执行、不可逆数据处理

同时支持四种 token 预算模式：

- `Micro`
- `Standard`
- `Deep`
- `Artifact`

English: The skill combines a built-in risk model with output-depth control so the agent can spend tokens more intentionally.

## 交互式路由与渐进式推进

这是这个 Skill 很重要的一层能力。

当任务不够明确、可能涉及文件、可能是大范围处理，或者智能体不确定该进入哪种模式时，它不会盲目替用户决定，而是先发出一个极简确认提示，让用户确认：

- 任务复杂度
- 处理模式
- 输出详细度

同时，大任务默认遵循：

`判断 -> 样本 -> 分阶段 -> 全量`

而不是一上来就“全部做完”。

English: The skill prefers interactive routing and progressive delivery over blind full-batch execution.

## 平台适配

### WorkBuddy

更偏向保守文件处理，优先只读分析、范围确认和样本批次。

English: WorkBuddy defaults should remain conservative around folders and file operations.

### iMA Copilot

更偏向缩小知识范围，优先最相关材料、短结论和分组输出，而不是默认长篇报告。

English: iMA Copilot should start narrow, evidence-first, and expansion-on-demand.

### CodeBuddy

更偏向限制读代码范围、修改范围和测试范围，避免扫全仓和大面积重构。

English: CodeBuddy should optimize read budget, edit budget, and test budget.

## 示例提示词

```text
请判断这个任务应该进入 Ask、Plan、Craft 还是 Expert 模式。
```

```text
请先用 Plan Mode 规划，不要直接修改文件。
```

```text
这是一个明确的小任务，请直接用 Craft Mode 给结果。
```

```text
这个报错先用 Ask Mode 诊断，不要改代码。
```

```text
如果继续修复会浪费 token，请告诉我是否应该进入 Expert Mode。
```

```text
请用最省 token 的方式回答，只给我下一步。
```

English: See [GUIDE.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/GUIDE.md) for more public usage examples.

## 安装方式

### 文件夹方式

将 `token-efficient-task-router/` 目录放入支持 Skill 的本地目录、工作目录或上传目录即可。

### 压缩包方式

当前可发布产物：

- `token-efficient-task-router-v0.6.5.zip`
- `token-efficient-task-router-v0.6.5.skill`

本地测试仍保留最新别名：

- `token-efficient-task-router.zip`
- `token-efficient-task-router.skill`

### Codex 本地安装

如需在 Codex 本地长期复用，可放入：

```text
~/.codex/skills/token-efficient-task-router
```

安装后建议重启一次应用，使新 Skill 稳定生效。

English: For Codex local use, place the folder under `~/.codex/skills/token-efficient-task-router`.

## 公开仓库结构

```text
token-efficient-task-router/
├── .gitignore
├── SKILL.md
├── LICENSE
├── README.md
├── GUIDE.md
├── VERSION
├── CHANGELOG.md
├── references/
├── templates/
├── examples/
├── tests/
└── scripts/
```

## 公开文档

- [README.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/README.md)：公开仓库首页与平台展示说明
- [GUIDE.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/GUIDE.md)：面向外部读者的独立行为说明
- [CHANGELOG.md](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/CHANGELOG.md)：版本演进记录
- [LICENSE](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/LICENSE)：公开授权说明

## 文件安全原则

未经确认，这个 Skill 不鼓励：

- 覆盖核心文件
- 删除文件
- 批量重命名
- 批量移动
- 改动部署配置
- 改动数据库
- 大面积未确认重写

它的设计目标不是“凡事变慢”，而是“只在确实更安全或更划算时才让执行变慢”。

English: The skill slows things down only when slowing down is safer or cheaper.

## 打包与校验

- 打包脚本 / Packaging: [scripts/package_skill.py](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/scripts/package_skill.py)
- 校验脚本 / Validation: [scripts/validate_skill.py](/Volumes/AlfredLi-1T/GPT_CodeX/token-efficient-task-router/scripts/validate_skill.py)
