---
user_id: "cheng tan"
paper_id: 2255
arxiv_id: "2606.32025v1"
title: "Generative Skill Composition for LLM Agents"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.32025v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.32025v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:07:16"
---
# Generative Skill Composition for LLM Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · skill composition · autoregressive decoding · constrained decoding

## 一句话总结

提出SkillComposer，将结构化技能组合形式化为任务条件技能序列预测，使用约束自回归解码器联合确定子集、数量和顺序。

## 摘要

> Recent LLM agents benefit from skills for solving complex tasks. Skills encapsulate modular packages of procedural knowledge and instructions for performing specialized tasks, such as setting up a sandboxed environment, running a test suite, or refactoring a function across multiple files. As skill libraries grow and become reusable across tasks and domains, selecting an appropriate skill composition has emerged as a central bottleneck. Existing approaches fall into two categories. One exposes the agent's reasoning to the entire skill collection; the other performs skill retrieval via embeddings or LLM-based rerankers. Both provide useful insights; however, they miss the structural nature of skill composition, which is a joint decision over which skills, how many, and in what order—three dimensions that cannot be decoupled. We formalize this as structured skill composition: given a task and a skill library, predict an executable skill plan that jointly specifies the activated subset, count, and execution order. We propose SkillComposer, which instantiates structured skill composition as task-conditioned skill sequence prediction. SkillComposer uses a constrained autoregressive decoder over skill identifiers, so subset, count, and order emerge jointly from a single decoding pass, and dependencies between successive skills are captured naturally. We build a training set of task-composition pairs from a real, human-curated skill library. We then evaluate SkillComposer along two axes: composition quality on a held-out test set, and downstream task success on SkillsBench across two production-grade coding agents. On {GPT-5.2-Codex, Gemini-3-Pro-Preview}, SkillComposer raises the pass rate by $\{+23.1,+18.2\}$ pp over the no-skill baseline, surpassing top-3 retrieval and matching the gold-skill retrieval upper bound at lower prompt-token cost.
> Project Page: https://skill-composer.github.io/

Q1: 这篇论文试图解决什么问题？

技能组合问题：给定任务和技能库，需要预测可执行的技能计划，同时确定激活哪些技能、使用多少个技能以及执行顺序。现有方法要么暴露所有技能给智能体推理，要么通过检索（嵌入或LLM重排序）选择技能。但技能组合是结构化的联合决策，三个维度相互依赖，不能解耦。检索方法独立选择技能，忽略了技能间的依赖关系（如先运行测试才能重构函数），而暴露全部技能则浪费上下文窗口。因此，需要一种能够捕获技能之间结构依赖的方法。

Q2: 有哪些相关研究？

相关研究分为两类：1) 暴露全部技能给智能体，如ReAct、AutoGPT等；2) 检索式方法，如使用嵌入或LLM重排序选出top-k技能（Wang et al., 2023; Yuan et al., 2023; Qian et al., 2023; Ma et al., 2025）。这些方法均未显式建模技能组合的结构性。此外，工具学习领域也有研究，但技能通常被视为独立工具，顺序不重要。本文首次形式化结构化技能组合，并提出端到端序列预测方法。

Q3: 论文如何解决这个问题？

SkillComposer将技能组合视为任务条件有序技能序列预测。具体地：1) 构建训练数据：从真实人工策划的技能库中获取任务-组合种子，构建技能依赖图，通过分层合成和质量过滤生成监督数据（包括单技能和多技能组合）。2) 模型架构：使用冻结的检索调优编码器（frozen retrieval-tuned encoder）对任务进行编码，然后使用约束自回归解码器（constrained autoregressive decoder）在技能标识符上进行预测。解码器在每一步只能输出有效的技能标识符（在技能库中），并且通过因果掩码自然捕获技能之间的后继依赖关系。3) 推理时：给定任务，模型自回归生成技能序列，即同时决定了子集、数量和顺序。无需显式执行顺序输入。

Q4: 论文做了哪些实验？

实验沿两个轴线进行：1) 组合质量评估：在合成数据和保留的真实数据上测试预测计划与目标技能子集、数量和顺序的匹配程度。2) 下游任务成功评估：在SkillsBench上测试两个生产级编码代理（GPT-5.2-Codex和Gemini-3-Pro-Preview）的pass率。对比基线包括无技能基线、top-3检索（基于嵌入和LLM重排序）、gold skill上界（使用真实技能顺序）。

Q5: 发现了什么实验现象？

1) 在组合质量上，SkillComposer在子集、数量和顺序三个维度均显著优于检索基线，尤其是顺序匹配度。2) 下游任务成功：在GPT-5.2-Codex上，pass率从无技能基线提升23.1个百分点；在Gemini-3-Pro-Preview上提升18.2个百分点。3) 超越top-3检索（基于嵌入或LLM重排序），且与gold skill上界（即使用真实技能计划）相当，但提示token成本更低。4) 消融实验（推测，但未提供具体数值）：约束解码比非约束解码更好，分层合成数据比仅用种子数据更好。

Q6: 有什么可以进一步探索的点？

1) 更强大的backbone和更大的策划技能库有望进一步提升组合准确性和排序。2) 预训练语料的先验和任务覆盖会影响泛化，需要更丰富的训练分布。3) 扩展到开放域技能库（动态增加技能）。4) 技能组合的可解释性和失败模式分析。5) 跨领域迁移，如从编码任务推广到机器人或对话任务。

Q7: 总结一下论文的主要内容

本文针对LLM智能体技能组合的结构化决策问题，提出了SkillComposer。首先形式化问题：给定任务和封闭技能库，预测一个有序技能计划，同时决定子集、数量和顺序。现有方法（暴露全部或检索）忽略了三者的联合依赖。SkillComposer采用任务条件技能序列预测：使用冻结的检索调优编码器编码任务，约束自回归解码器生成技能标识符序列。训练数据通过分层合成和过滤从真实人工种子生成。实验在两个编码智能体（GPT-5.2-Codex, Gemini-3-Pro-Preview）上进行，在SkillsBench上pass率分别提升23.1和18.2个百分点，超过top-3检索，匹配gold skill上界，且提示成本更低。本文还贡献了技能依赖图构建方法和合成数据管道。局限性包括依赖预训练语料先验和技能库覆盖范围。未来方向包括更强backbone、更大库、动态技能库和跨领域扩展。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与智能体（agent）方向直接相关，权重0.10，尤其关注技能模块化和组合。

## 基本信息

- 作者：Xinyu Zhao, Zhen Tan, Vaishnav Tadiparthi, Nakul Agarwal, Kwonjoon Lee, Ehsan Moradi Pari, Hossein Nourkhiz Mahjoub, Tianlong Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.32025v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括abstract、introduction、related works、limitations等片段，结合heuristic_draft补充细节，未编造具体数值但基于证据推断。
