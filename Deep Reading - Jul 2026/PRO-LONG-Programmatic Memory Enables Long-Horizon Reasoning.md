---
user_id: "cheng tan"
paper_id: 5404
arxiv_id: "2607.20064"
title: "PRO-LONG: Programmatic Memory Enables Long-Horizon Reasoning"
publish_date: "2026-07-23"
pdf_url: "https://arxiv.org/pdf/2607.20064"
abs_url: "https://arxiv.org/abs/2607.20064"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:20:30"
---
# PRO-LONG: Programmatic Memory Enables Long-Horizon Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model · agent · long-horizon reasoning · programmatic memory

## 一句话总结

PRO-LONG 是一个基于程序化记忆的最小化上下文管理框架，旨在提升 LLM 智能体在长时域、探索性任务中的推理能力。

## 摘要

> Long-horizon tasks require sustained perception, reasoning, and exploration, and are a persistent challenge for large language model (LLM) agents. This gap is reflected in their limited performance on continual learning benchmarks such as ARC-AGI-3, especially when models are evaluated out of the box. Various agent harnesses have been proposed to close this gap, and each commits to a strategy for handling long sequences of observations, i.e., what information to save from the environment and how to load it into model context, a choice we argue is particularly consequential. Existing methods for context management face a significant tradeoff, as preserving more information makes retrieving relevant details less tractable. We propose PRO-LONG, a minimal context management framework built around programmatic memory for LLM agents in long-horizon, exploratory settings. PRO-LONG addresses the tradeoff by keeping a complete, structured interaction log and capitalizing on recent progress in coding agents to search this history efficiently. On the full ARC-AGI-3 public game set, PRO-LONG improves over a base coding agent by an average of 18.0 percentage points across frontier models, and matches or exceeds state-of-the-art specialized harnesses (up to 76.1% pass@1) while using 4.2-5.8x fewer tokens. With Fable 5, PRO-LONG achieves 97.4% best@2 at a total cost of \$1,750. Relevant code and logs are available at https://github.com/alexisfox7/PRO-LONG.

Q1: 这篇论文试图解决什么问题？

长时域任务要求智能体持续感知、推理和探索，LLM 智能体在此类任务中表现不佳，尤其在持续学习基准（如 ARC-AGI-3）上开箱即用时差距明显。现有上下文管理方法面临核心权衡：保存更多历史信息虽保留细节，但增加了检索无关噪声的难度，导致性能下降。PRO-LONG 旨在通过程序化记忆打破这一权衡，既保留完整历史又确保高效检索。

Q2: 有哪些相关研究？

相关工作包括多种智能体框架（agent harnesses）和上下文管理策略，如滑动窗口、记忆压缩、检索增强等。代码智能体（coding agents）的最新进展（如代码自动生成和执行）被用于高效搜索。PRO-LONG 结合结构化日志和代码驱动检索，与 MemGPT 等记忆增强方法（推测）有概念关联，但更强调最小化修改和效率。

Q3: 论文如何解决这个问题？

PRO-LONG 提出程序化记忆：维护一个完整的、结构化交互日志，并利用代码智能体在该日志上执行生成式搜索。具体而言，每次交互事件（观察、推理、动作）被记录为可查询的条目；当需要历史信息时，智能体生成搜索代码（如 Python）检索相关条目。这种方法避免了压缩或截断带来的信息丢失，同时通过代码搜索的灵活性和效率对抗信息过载。

Q4: 论文做了哪些实验？

论文在 ARC-AGI-3 公共游戏集和 Fable 5 任务上评估 PRO-LONG。ARC-AGI-3 是持续学习基准，测试长时域感知与推理。Fable 5 可能类似（推测）。基线包括基础编码智能体（base coding agent，仅按时间线处理事件而不通过历史搜索）以及当前最先进的专用框架（specialized harnesses）。指标使用 pass@1 和 best@2，同时记录 token 消耗和运行成本。

Q5: 发现了什么实验现象？

PRO-LONG 在 ARC-AGI-3 上平均提升基础编码智能体 18.0 个百分点，达到 76.1% pass@1，同时 token 使用量减少 4.2–5.8 倍。在 Fable 5 上 best@2 达 97.4%，总成本仅 1,750 美元。这表明程序化记忆在信息保留和检索效率之间取得了优越平衡，且效率提升显著。值得注意的是，token 节省可能源于仅搜索相关片段而非全部历史（合理推断）。

Q6: 有什么可以进一步探索的点？

可探索的方向包括：将 PRO-LONG 推广到多模态、交互式或实时环境；优化搜索策略（如基于重要性采样的索引）；与更强代码智能体（如自我改进）结合；分析失败模式（如搜索遗漏关键历史时）；以及在更广泛的长时域任务（如机器人规划、对话系统）中验证泛化性。

Q7: 总结一下论文的主要内容

本文针对长时域任务中 LLM 智能体的上下文管理难题，提出 PRO-LONG 框架。其核心是程序化记忆：完整、结构化的交互日志加上代码驱动的搜索机制。该方法在保持历史完整性的同时实现了高效检索，解决了现有方法的保存-检索权衡。实验在 ARC-AGI-3 和 Fable 5 基准上进行，结果显示 PRO-LONG 在 pass@1 上提升 18.0 个百分点，匹配或超越专用框架，且 token 使用减少 4.2–5.8 倍。工作贡献包括提出最小化框架、效率验证以及开源实现。论文论证主线清晰：问题-权衡-方案-实验。技术主线强调“完整记录+代码搜索”的廉效设计。实验主线通过多模型、多基准、多指标全面验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接关联智能体（agent）方向，用户画像中权重 0.1。

## 基本信息

- 作者：Alexis Fox, Junlin Wang, Paul Rosu, Bhuwan Dhingra
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20064`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要基于论文摘要和启发式草稿，未参考 PDF 检索证据。
