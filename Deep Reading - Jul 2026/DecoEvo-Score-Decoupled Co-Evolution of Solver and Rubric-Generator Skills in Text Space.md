---
user_id: "cheng tan"
paper_id: 5750
arxiv_id: "2607.25675v1"
title: "DecoEvo: Score-Decoupled Co-Evolution of Solver and Rubric-Generator Skills in Text Space"
publish_date: "2026-07-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.25675v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.25675v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:56:04"
---
# DecoEvo: Score-Decoupled Co-Evolution of Solver and Rubric-Generator Skills in Text Space

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：text-space optimization · co-evolution · large language models · rubric generation

## 一句话总结

DecoEvo通过解耦求解器和评分标准生成器的演化目标，在文本空间中实现协同演化，并在所有15个骨干-基准组合上取得最优平均性能。

## 摘要

> Text-space optimization adapts large language models (LLMs) by editing external natural-language artifacts rather than model weights, so the optimized artifacts remain inspectable and the model can be treated as a black box. However, most existing text-space methods keep evaluation fixed. On open-ended tasks, this can become a bottleneck: once the solver improves on the criteria a rubric measures, omitted dimensions remain invisible to the optimization signal. Simply evolving the rubric is also unreliable when updates are selected by the current solver's score, because apparent progress can come from making the rubric easier to satisfy. We introduce DecoEvo (Decoupled Co-Evolution), which co-evolves a solver skill and a rubric-generator skill under decoupled objectives without using gold rubrics during optimization. The solver skill is updated using criterion-level feedback, while the rubric-generator skill is revised through complementary audits of requirement coverage and response discrimination that are independent of aggregate solver score. This separation focuses generator updates on newly exposed solver weaknesses, reducing repeated emphasis on criteria the solver already satisfies. Under each benchmark's official evaluation, DecoEvo outperforms all compared methods across five benchmarks and three LLM backbones, yielding 2.8--5.0\% relative gains over SkillOpt in the five-benchmark average.

Q1: 这篇论文试图解决什么问题？

这篇论文旨在解决文本空间优化中评估标准固定或不稳健的问题。具体而言：1）当求解器技能在评分标准衡量的维度上优化后，其他未衡量的维度无法获得优化信号，导致评估瓶颈；2）若演化评分标准，但更新基于当前求解器的分数，评分标准可能变得更易满足而非更准确地反映任务质量，导致虚假进步。DecoEvo通过解耦求解器和评分标准生成器的演化目标，使得评分标准能自适应地暴露求解器弱点，同时避免被求解器利用。

Q2: 有哪些相关研究？

相关工作包括文本空间优化（如SkillOpt、EvoLM）、协同演化方法（如古典协同演化遗传算法）、对比评分标准生成（如Rezaei et al., 2026; Liu et al., 2026d）、配对自适应评分标准（如OpenRS, Jia et al., 2026），以及使用外部评判模型调整优化信号的方法。DecoEvo与这些方法的关键区别在于生成器更新完全独立于聚合求解器分数，而是通过结构审计基于要求覆盖和响应区分性进行更新。

Q3: 论文如何解决这个问题？

DecoEvo采用解耦协同演化框架，维护两个持久技能：求解器技能（生成解决方案）和评分标准生成器技能（生成评估评分标准）。两个技能都在文本空间中以自然语言形式表示，LLM黑箱保持冻结。在每次迭代中，求解器技能基于当前评分标准提供的标准级反馈更新（每个标准的得分指示该维度的表现）；评分标准生成器技能通过两种互补的审计机制更新：1）要求覆盖审计：检查当前评分标准是否覆盖任务的所有关键要求，若遗漏则生成新的标准；2）响应区分审计：比较不同质量的响应，确保评分标准能区分优劣。这两种审计均独立于求解器的聚合分数，从而避免生成器迎合求解器当前表现。生成器的修订被提炼为可重用的技能更新。整个过程不依赖黄金评分标准。

Q4: 论文做了哪些实验？

论文在五个开放任务基准（具体名称未在检索片段中给出，但通常涉及推理、数学、代码等）和三个LLM骨干（GPT-4o、Qwen3-4B、Qwen3-8B）上进行实验。对比方法包括至少SkillOpt等文本空间优化基线。每个骨干-基准组合为一个设置，共15个设置。实验采用各基准的官方评估指标。论文报告平均得分和相对增益。

Q5: 发现了什么实验现象？

DecoEvo在所有15个骨干-基准组合上取得最高平均分。相对于SkillOpt，在GPT-4o上平均增益5.0%，Qwen3-4B上2.9%，Qwen3-8B上2.8%，对应绝对分数分别提升3.10、1.74、1.78分。这表明解耦演化能持续提升性能，且增益对不同骨干和任务鲁棒。片段还提到诊断可被蒸馏为可重用的生成器技能修订，暗示该方法具有迁移和积累能力。

Q6: 有什么可以进一步探索的点？

1）将DecoEvo扩展到更多样化的任务和骨干，验证泛化性；2）探索更复杂的评分标准结构（如层级标准、权重动态调整）；3）结合其他自演化方法（如权重微调）形成混合系统；4）研究生成器技能更新的可迁移性，形成通用评分标准库；5）分析潜在风险：如过度审计导致计算开销增加，或评分标准演化与任务真实目标偏离；6）应用于安全、对齐等敏感领域，确保演化方向符合人类意图。

Q7: 总结一下论文的主要内容

DecoEvo提出了一种解耦协同演化框架，用于在文本空间中优化LLM的求解器和评分标准生成器技能。现有文本空间优化方法通常固定评估标准，导致优化信号遗漏未覆盖维度；或者演化评分标准但易因依赖求解器分数而产生虚假进步。DecoEvo通过设计独立的更新目标来解决这一问题：求解器基于标准级反馈更新，评分标准生成器通过要求覆盖审计和响应区分审计更新，两者均不依赖求解器的总体得分。这使得评分标准能自适应地暴露求解器弱点，同时避免被求解器操纵。实验在五个开放任务基准和三个LLM骨干（GPT-4o、Qwen3-4B、Qwen3-8B）上进行，共15个设置。DecoEvo在所有设置中取得平均最优性能，相对SkillOpt增益2.8-5.0%，表明该方法有效且鲁棒。此外，生成器产生的诊断可被精炼为可重用的技能修订，具有实际应用价值。论文还讨论了与对比方法（如SkilOpt、EvoLM、OpenRS等）的关系，并指出解耦演化在避免共同适应陷阱方面的优势。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该方法核心是文本空间中的自演化，与智能体技能自动改进高度相关，可直接作为智能体系统持续优化模块。

## 基本信息

- 作者：Jiangwang Chen, Zixin Song, Junlin Liu, Shuaiyu Zhou, Haiyan Wu, Haihan Shi, Chenxi Zhou, Hanqing Li, Xiao Yang, Da Zhu, Guanjun Jiang, Hai Wan, Xibin Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.25675v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了检索到的结论和方法片段，并依据field_evidence_map分配字段证据，部分细节（如具体基准名称、消融实验）因证据不足而未详细展开。
