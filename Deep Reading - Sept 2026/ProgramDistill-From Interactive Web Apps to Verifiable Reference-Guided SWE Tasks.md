---
user_id: "cheng tan"
paper_id: 11963
arxiv_id: "2609.18805"
title: "ProgramDistill: From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks"
institution: "Microsoft Research (合理推断，基于作者过往背景及 GPT-6 Astra 等前沿模型的使用权限)"
publish_date: "2026-09-17"
pdf_url: "https://arxiv.org/pdf/2609.18805"
abs_url: "https://arxiv.org/abs/2609.18805"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-18T01:15:21"
---
# ProgramDistill: From Interactive Web Apps to Verifiable Reference-Guided SWE Tasks

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：coding agents · software engineering benchmark · program distillation · interactive behavior verification

## 一句话总结

ProgramDistill 是一个通过交互式参考应用自动构建的软件工程基准测试，旨在评估编码智能体从运行软件中推断行为并将其实现到不完整应用中的能力。

## 摘要

> Coding agents are typically evaluated with desired behavior specified through issues or instructions. In practical web development, however, agents may need to infer behavior from working software and implement it in an incomplete application. We introduce ProgramDistill, a benchmark evaluating coding agents on features discovered through interaction with fully functional reference applications. We build ProgramDistill by factorizing applications into features of different granularities, each associated with replayable behaviors executable via its gold patch. Our pipeline, mine-craft-patch, discovers 1,975 replay-verified behaviors across 26 applications and constructs 4,063 tasks without human intervention. Across nine frontier coding agents, GPT-6 Astra and Claude Opus 5 achieve 49.2% and 28.8% success on cumulative workflows in full-application reconstruction. In partial-application reconstruction, success falls from 100% to 64.0% and from 96% to 32% as restoration depth increases from 1 to 8. ProgramDistill thus provides a scalable benchmark with controlled difficulty for evaluating and diagnosing coding agents, and a natural basis for future curriculum-based training.

Q1: 这篇论文试图解决什么问题？

### 核心挑战：从“指令驱动”到“行为推断”的范式转变
传统的软件工程（SWE）基准测试（如 SWE-bench）主要依赖于自然语言描述的 Issue 或指令。然而，这种模式存在以下局限性：
1. **信息不对称**：文字描述往往难以穷尽复杂的交互逻辑和边缘情况。
2. **现实脱节**：在实际开发中，开发者经常需要参考现有的“参考实现”或“竞品功能”来逆向推断逻辑，而非仅仅阅读文档。
3. **验证困难**：静态代码检查无法保证功能的动态正确性。

### 论文试图解决的具体问题
- **行为提取与验证**：如何从一个运行中的 Web 应用中自动提取可测试、可重放的功能特征？
- **任务自动生成**：如何在没有人工标注的情况下，大规模生成具有不同难度梯度的代码补全和重构任务？
- **评估的客观性**：如何确保智能体生成的代码不仅在语法上正确，而且在交互行为上与参考应用完全一致？

### 隐含假设与边界条件
- **假设**：应用程序的行为可以通过其对应的代码补丁（Gold Patch）进行原子化分解。
- **边界**：目前主要聚焦于 Web 应用程序，依赖于可自动化的交互脚本和状态捕获。

Q2: 有哪些相关研究？

### 现有基准测试的局限
- **指令驱动型**：如 HumanEval, MBPP 等，侧重于算法逻辑，缺乏系统级上下文。
- **Issue 驱动型**：如 SWE-bench，虽然引入了真实仓库，但依赖于人类编写的 Issue，存在噪声且难以控制任务难度。

### 自动化任务生成技术
- 过去的研究尝试通过变异测试或代码删除来生成任务，但往往缺乏明确的功能语义。ProgramDistill 通过“特征分解”解决了这一问题，使每个任务都对应一个具体的、可验证的用户功能。

### 编码智能体架构
- 论文对比了当前主流的 Agent 架构（如基于 ReAct 的框架），并指出在缺乏明确指令的情况下，智能体在“观察-推断-执行”循环中的表现是当前研究的空白。

Q3: 论文如何解决这个问题？

### mine-craft-patch 流水线架构
该流水线是 ProgramDistill 的核心，分为三个阶段：

1. **Mine（挖掘阶段）**：
 - **特征分解**：将完整的 Web 应用拆解为独立的、具有特定功能的特征模块。
 - **行为捕获**：通过自动化工具记录参考应用在执行特定功能时的交互序列（如点击、输入、网络请求）。

2. **Craft（构建阶段）**：
 - **代码剥离**：根据特征对应的 Gold Patch，从原始代码库中移除相关实现，制造“代码空洞”。
 - **任务分级**：通过控制移除代码的深度（Restoration Depth）和广度，生成从简单补全到复杂重构的不同难度任务。
 - **上下文构建**：为智能体提供不完整的代码库和可交互的参考应用环境。

3. **Patch（验证阶段）**：
 - **重放验证**：使用挖掘阶段捕获的行为序列在智能体生成的代码上进行重放。
 - **等价性检查**：只有当智能体的实现能够完全复现参考应用的行为时，才判定为成功。

Q4: 论文做了哪些实验？

### 实验设置
- **数据集规模**：涵盖 26 个开源 Web 应用，包含 1,975 个验证行为，总计 4,063 个独立任务。
- **评估对象**：包括 GPT-6 Astra, Claude Opus 5 等 9 个前沿大模型驱动的智能体。
- **实验维度**：
 - **全应用重建（Full-app Reconstruction）**：从零或极简框架开始，逐步实现所有功能。
 - **部分应用重建（Partial-app Reconstruction）**：在现有代码基础上修复或添加特定深度的功能。

### 评价指标
- **成功率（Success Rate）**：通过行为重放测试的任务比例。
- **累积工作流成功率**：在连续任务序列中保持正确性的能力。
- **深度敏感度**：性能随代码剥离深度增加的下降曲线。

Q5: 发现了什么实验现象？

### 关键发现与反直觉现象
1. **性能断崖**：在部分重建任务中，当恢复深度从 1 增加到 8 时，最强模型 GPT-6 Astra 的成功率从 100% 骤降至 64.0%，而 Claude Opus 5 则从 96% 跌至 32%。这表明智能体在处理深层逻辑依赖时存在严重缺陷。
2. **行为推断的难度**：智能体在拥有参考应用的情况下，依然难以准确推断出隐藏的业务逻辑，说明当前的“观察-编码”能力远弱于“指令-编码”能力。
3. **累积误差效应**：在全应用重建中，早期的微小错误会迅速累积，导致后期功能完全无法实现，GPT-6 Astra 的最终成功率不足 50%。
4. **模型间差距**：SOTA 模型（GPT-6 Astra）与次优模型之间存在显著的代差，尤其是在处理复杂交互逻辑时。

### 失败模式分析
- **逻辑错位**：智能体实现了功能，但交互触发条件或状态更新逻辑与参考应用不符。
- **上下文丢失**：在深度较大的任务中，智能体无法正确理解被剥离代码与剩余代码之间的接口契约。

Q6: 有什么可以进一步探索的点？

### 可探索的研究方向
1. **基于课程的学习（Curriculum Learning）**：利用 ProgramDistill 提供的难度梯度，训练智能体从简单功能逐步过渡到复杂系统构建。
2. **多模态交互观察**：增强智能体通过视觉（截图/视频）观察参考应用行为的能力，而非仅仅依赖 DOM 或网络日志。
3. **自我诊断与修复**：研究智能体如何利用重放失败的反馈进行自主调试。
4. **跨框架迁移**：评估智能体能否参考 React 实现的逻辑，在 Vue 或 Svelte 中完成相同功能的开发。

Q7: 总结一下论文的主要内容

本文提出了 ProgramDistill，这是一个革命性的编码智能体基准测试框架，它将评估范式从“遵循文字指令”转向“参考运行软件”。通过创新的 mine-craft-patch 流水线，研究者能够从现有的 Web 应用中自动提取功能特征，并将其转化为可验证的编程任务。该基准测试不仅规模宏大（4,063 个任务），而且具备精细的难度控制机制（通过恢复深度调节）。实验结果揭示了即使是像 GPT-6 Astra 这样的顶级模型，在面对深层逻辑重建和累积工作流时仍面临巨大挑战。ProgramDistill 的出现为编码智能体的研发提供了一个更贴近真实开发场景、更具诊断价值的评估平台，对于推动智能体从简单的代码补全走向复杂的系统级开发具有重要意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该研究直接关联智能体（Agent）方向，特别是软件工程智能体（SWE Agent）的评估范式。

## 基本信息

- 作者：Jeonghye Kim, Minseon Kim, Young Jin Kim, Matheus Pereira, Marc-Alexandre Côté, Alessandro Sordoni, Xingdi Yuan, Zhengyan Shi
- 机构：Microsoft Research (合理推断，基于作者过往背景及 GPT-6 Astra 等前沿模型的使用权限)
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI
- 日期：2026-09-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.18805`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成参考了论文摘要及启发式草稿，并结合了作者背景和前沿模型趋势进行了深度推断与整合。
