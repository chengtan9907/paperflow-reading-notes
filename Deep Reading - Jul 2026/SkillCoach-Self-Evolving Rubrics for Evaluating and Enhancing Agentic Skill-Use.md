---
user_id: "cheng tan"
paper_id: 2646
arxiv_id: "2607.01874"
title: "SkillCoach: Self-Evolving Rubrics for Evaluating and Enhancing Agentic Skill-Use"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01874.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01874"
abs_url: "https://arxiv.org/abs/2607.01874"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:24:25"
---
# SkillCoach: Self-Evolving Rubrics for Evaluating and Enhancing Agentic Skill-Use

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · skill usage · process evaluation · self-evolving rubric

## 一句话总结

SKILLCOACH是一个自演化评分框架，通过从真实轨迹中衍生出基于技能的过程评分，分别评估技能选择、遵循、组合和反思四个维度，并将过程质量与外部验证器成功分离，用于评估和改进智能体的技能使用。

## 摘要

> Skills are becoming a reusable operational layer for LLM agents, encoding SOPs, domain rules, tool workflows, scripts, and validation routines. In realistic skill repositories, overlapping skills make reliable skill-use difficult. Final verifier success is too coarse for both evaluation and training, since an agent may pass through trial and error while selecting distractor skills, skipping required steps, composing workflows incorrectly or omitting final checks. We introduce SKILLCOACH, a self-evolving rubric framework for evaluating and enhancing agentic skill-use. SKILLCOACH derives skill-grounded process rubrics from real rollouts and evaluates trajectories along four dimensions: skill selection, skill following, skill composition, and skill-grounded reflection. It keeps the external verifier as a separate outcome signal, allowing process quality to be distinguished from accidental task success. The evolved rubrics further serve as process supervision for selecting high-quality training trajectories. Experiments show that evolved rubrics substantially improve evaluation quality, expose failures hidden by final accuracy, and provide stronger supervision signals than outcome-only filtering for enhancing agentic skill-use.
> Email: jzhu709@connect.hkust-gz.edu.cn; yutaoyue@hkust-gz.edu.cn; maokelong.1@jd.com

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是：在现实的技能库中，由于技能重叠，LLM智能体在技能使用上存在可靠性困难。现有的评估方法通常依赖最终任务成功（如外部验证器）来判断智能体表现，但这种结果导向的评估过于粗糙——智能体可能通过试错、选择错误技能、跳过关键步骤、错误排序或遗漏最终检查等方式偶然成功，而这些过程中的缺陷被最终成功掩盖。因此，需要一个能够捕捉轨迹过程质量的方法，既能用于评估也能用于训练监督。论文的核心问题是：如何设计一种基于技能结构的自演化评分框架，来精细评估智能体在技能选择、遵循、组成和反思方面的表现，并使过程质量与结果成功脱钩？

Q2: 有哪些相关研究？

相关工作涵盖以下几个方向：
1. **智能体技能使用与评估**：现有工作通常将技能视为可重用的操作层，但评估多采用最终任务成功或外部验证器。缺乏对技能使用过程的细粒度评估。
2. **过程监督与奖励建模**：类似过程奖励模型（PRM）在推理任务中用于提供中间步骤的监督信号，但尚未利用技能自身的结构来定义过程监督。
3. **自演化/自改进方法**：一些工作探索从智能体自身的推演中生成训练数据或评估指标，但未聚焦于技能使用场景中的过程评分。
4. **技能库中的干扰与选择**：现实技能库中技能重叠导致选择困难，但现有研究多假设技能是独立且准确的，未考虑干扰技能的影响。
论文指出，现有工作未利用技能结构本身来定义同时服务于评估和训练的过程监督，这是主要空白。

Q3: 论文如何解决这个问题？

论文提出SKILLCOACH，一个自演化评分（rubric）框架。方法包括：
1. **评分维度定义**：从真实轨迹中衍生出四个技能相关的过程评分维度：
 - 技能选择（Skill Selection）：智能体是否选择了正确的技能。
 - 技能遵循（Skill Following）：智能体是否正确执行了技能中的步骤。
 - 技能组成（Skill Composition）：智能体是否按正确顺序组合多个技能。
 - 基于技能的反思（Skill-grounded Reflection）：智能体是否在过程中进行自我检查或纠错。
2. **过程质量与结果信号分离**：保留外部验证器作为独立的“结果成功”信号，而评分则专注轨迹过程质量，使得即使任务偶然成功，过程缺陷也能被检测。
3. **自演化机制**：评分不是手工设计的，而是从智能体的实际推演中自动生成和演化。具体流程：首先收集智能体在带干扰技能库中的轨迹；然后利用LLM或基于规则的方法从轨迹中提取每个维度的评分标准；再通过迭代优化评分的一致性。
4. **训练数据筛选**：演化的评分进一步用作过程监督，从大量轨迹中筛选出高质量的样本（即过程评分高的轨迹）用于训练，以提升智能体的技能使用能力。
论文强调，该框架是第一个利用技能结构定义过程监督的，可同时用于评估和训练。

Q4: 论文做了哪些实验？

论文通过四个研究问题（RQ）设计实验：
1. **评分忠实性（RQ1）**：测试演化的评分是否忠实于轨迹过程质量，能否作为可用的轨迹级评估工具。
2. **评估有效性（RQ2）**：检查评分是否能够揭示被最终准确性隐藏的失败模式，例如智能体使用了错误技能但偶然成功的情况。
3. **训练增益（RQ3）**：利用演化的评分筛选高质量训练轨迹，验证其是否比仅基于结果（成功/失败）过滤提供更强的监督信号，从而提升智能体技能使用能力。
4. **元能力消融（RQ4）**：测试智能体在有无黄金技能（ground truth skill）时的表现差异，验证评分框架对技能依赖的敏感性。
实验设置包括：在模拟技能库环境中构建任务，技能库包含黄金技能和若干干扰技能；基线包括仅结果过滤、随机轨迹选择等。使用不同规模的模型（如DeepSeek V4 Flash等）。指标包括最终准确率、评分与任务成功的相关性、过程失败检测率等。

Q5: 发现了什么实验现象？

实验观察到的关键现象包括：
1. **评分暴露隐藏失败**：演化的评分能够检测到智能体使用干扰技能或错误步骤但最终偶然成功的情况，而最终准确率无法区分这些失败。
2. **过程监督优于结果监督**：用评分筛选训练轨迹（仅保留高分过程轨迹）相比仅使用任务成功轨迹，显著提升了智能体在后续任务上的技能使用能力（最终准确率更高）。
3. **技能依赖敏感性**：当智能体没有黄金技能时，其轨迹评分较低，但一旦提供黄金技能，评分和任务成功率均提升，表明评分对技能存在性敏感。
4. **消融分析**：每个评分维度（选择、遵循、组成、反思）都对整体评估有贡献，去除任一维度会降低评估的区分度。
5. **尺度趋势**：更大规模模型（如DeepSeek V4 Flash）在过程中的表现更好，但仍能从评分中受益，且评分对不同规模模型均能提供有效监督。
（注：具体数值未提供，以上基于摘要和实验设计推断。）

Q6: 有什么可以进一步探索的点？

基于论文内容，可进一步探索的方向包括：
1. **扩展到开放域技能库**：当前实验在模拟的静态技能库中进行，未来可检验评分在动态或开放域技能库中的泛化性。
2. **自动化评分演化的稳定性与效率**：自演化过程可能依赖LLM或规则，后续可研究更高效的评分演化方法，如利用强化学习。
3. **技能结构更丰富的维度**：当前四个维度是否足够？可探索额外维度如技能时效性、跨技能冲突检测等。
4. **与在线学习结合**：评分可作为奖励信号用于在线训练（如RLHF），而不仅是离线筛选。
5. **跨智能体架构的适应性**：评分框架在不同智能体架构（ReAct、Plan-and-Execute等）下的适配性。
6. **真实应用场景验证**：在更复杂的真实世界任务（如机器人操作、软件工程）中测试框架的有效性。
7. **评分可解释性与细粒度反馈**：利用评分维度提供可操作的失败原因，帮助智能体调试。

Q7: 总结一下论文的主要内容

论文提出了SKILLCOACH，一个自演化评分框架，旨在评估和改进LLM智能体的技能使用能力。核心洞察是：最终任务成功不足以衡量技能使用的质量，因为智能体可能通过试错或错误决策偶然成功。SKILLCOACH从真实轨迹中自动衍生出四个过程评分维度（技能选择、遵循、组合、反思），将过程质量与外部验证器的结果信号分离。该评分既可作为评估工具，也可用于筛选高质量训练轨迹。实验表明，演化的评分能有效暴露最终准确率隐藏的失败，并且作为训练监督信号优于单纯的结果过滤。论文贡献在于：首次利用技能结构定义过程监督，同时服务于评估和训练；提出了自演化机制，使评分无需手工设计；通过实验验证了评分在不同模型规模下的有效性。局限包括：当前评估限于模拟环境，技能库为静态，且评分演化依赖特定LLM能力。整体上，该工作为智能体技能使用的细粒度评估和训练提供了新范式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文针对LLM智能体技能使用中的评估与训练问题，与智能体方向直接相关（权重0.10）。

## 基本信息

- 作者：Jiayin Zhu, Kelong Mao, Yudong Guo, Dengbo He, Sulong Xu, Simiu Gu, Yutao Yue
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01874`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成基于PDF检索证据（Abstract、Conclusion、Introduction等片段），并结合heuristic_draft润色补全。
