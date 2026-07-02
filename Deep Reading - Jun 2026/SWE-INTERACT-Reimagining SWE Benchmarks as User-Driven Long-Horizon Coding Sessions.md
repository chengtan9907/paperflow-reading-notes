---
user_id: "cheng tan"
paper_id: 1930
arxiv_id: "2606.30573"
title: "SWE-INTERACT: Reimagining SWE Benchmarks as User-Driven Long-Horizon Coding Sessions"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.30573.pdf"
pdf_url: "https://arxiv.org/pdf/2606.30573"
abs_url: "https://arxiv.org/abs/2606.30573"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-30T16:30:55"
---
# SWE-INTERACT: Reimagining SWE Benchmarks as User-Driven Long-Horizon Coding Sessions

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：software engineering · multi-turn interaction · user simulation · agent evaluation

## 一句话总结

我们提出了SWE-INTERACT，一个用于评估编码智能体在多轮交互、用户驱动的软件工程任务上的新基准测试平台。

## 摘要

> We introduce SWE-Interact, a new testbed for evaluating coding agents on multi-turn, interactive, user-driven software engineering tasks. Existing frontier SWE benchmarks typically provide complete requirements upfront and evaluate agents on autonomous implementation. In contrast, SWE-Interact places agents in a realistic developer workflow: a carefully designed user simulator starts with vague or incomplete instructions, progressively reveals requirements, inspects the agent's workspace, and provides targeted feedback, revisions, and new constraints until the full task goal has been handed off. Grounded in large-scale studies of real coding-agent interactions, this setup tests whether agents can discover user intent, adapt to evolving requirements, and build on their own prior work. Across a suite of frontier and open-weight models, we find that strong performance on single-turn SWE tasks does not reliably transfer to multi-turn, user-driven workflows: the best-performing models solve roughly 50% of single-turn baseline tasks but only 25% of the corresponding SWE-Interact tasks. The strongest models in our evaluation, including Opus 4.8 and GPT 5.5, start strong even in the face of vague initial instructions, persevere until all the requirements are surfaced by the user, integrate them better and write clean code. However, they still suffer from over-agentic coding, forgetting requirements and technical mistakes. Weaker models start poorly under ambiguity, give up early, forget or ignore instructions and rework their code more. Overall, SWE-Interact measures an orthogonal, real-world capability axis for frontier model development: interactive goal discovery and iterative refinement with a user in the loop.

Q1: 这篇论文试图解决什么问题？

现有软件工程（SWE）基准通常提供完整且明确的需求，评估智能体在一次性实现上的自主能力，忽视了真实开发工作流中的多轮交互、需求渐进揭示和用户反馈。这导致这些基准无法衡量智能体在用户驱动的长期编码会话中的实际表现。

Q2: 有哪些相关研究？

相关研究包括SWE-bench Pro, SWE Atlas (Refactoring), DeepSWE等前沿基准。近期工作如SlopCodeBench, SWE-EVO, EvoCode-Bench, CodeClash引入了状态或多轮代码库任务，但它们的用户交互设置往往是确定性的、与智能体无关的工作流，剥离了真实世界的模糊性和杂乱。SWE-INTERACT填补了这一空白，通过一个可类比用户的模拟器来测试智能体在动态需求下的适应能力。

Q3: 论文如何解决这个问题？

SWE-INTERACT采用一个基于persona的用户模拟器，模拟器从真实编码会话数据中学习。从三个现有基准（SWE-bench Pro, SWE Atlas Refactoring, DeepSWE）中选取75个复杂任务，每个任务包含多层需求。智能体经历PLAN→APPROVAL→IMPLEMENT→REVISE→SUBMIT的迭代流程，用户模拟器在每个阶段提供反馈、补充需求或提出修改。任务难度不再取决于代码复杂度，而在于交互深度和意图发现。

Q4: 论文做了哪些实验？

论文评估了多个前沿和开源模型，包括Opus 4.8和GPT 5.5。在75个SWE-INTERACT任务上与相应的单轮基线进行对比。关键指标是任务完成成功率。实验还分析了模型的行为模式，如是否在模糊指令下坚持、是否过度编码、是否遗忘需求等。

Q5: 发现了什么实验现象？

最佳模型在单轮基线中解决了约50%的任务，但在SWE-INTERACT多轮任务中仅解决约25%，表明单轮表现不能可靠迁移到多轮交互设置。Opus 4.8和GPT 5.5在模糊初始指令下表现强劲，能坚持到需求完全揭露，整合需求并编写整洁代码，但仍存在过度自主编码、遗忘需求和技术错误。较弱模型在模糊性下表现差，容易过早放弃，忽略指令，更频繁地重写代码。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：改进用户模拟器的真实性和多样性；将SWE-INTERACT扩展到更多领域（如科学软件）；研究缓解过度编码和需求遗忘的机制；开发新的训练策略提升智能体的交互适应能力；探索多轮交互中任务难度与模型行为的关系。

Q7: 总结一下论文的主要内容

SWE-INTERACT提出了一个用于评估编码智能体在多轮用户驱动任务中的新基准。通过使用基于真实数据的用户模拟器，它更贴近实际开发工作流，揭示了现有单轮基准可能高估模型能力的现象。实验表明，即使在强模型上，单轮到多轮的迁移仍存在巨大落差（50%→25%），且模型在面对模糊性和反馈迭代时表现出不同的失败模式。论文强调了交互式目标发现和迭代细化作为前沿模型开发中一个正交且关键的能力维度。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：论文直接涉及智能体评估，与你的智能体方向（权重0.1）高度相关。

## 基本信息

- 作者：Mohit Raghavendra, Anisha Gunjal, Aakash Sabharwal, Yunzhong He
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-30
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.30573`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据片段。
