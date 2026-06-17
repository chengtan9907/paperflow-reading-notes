---
user_id: "cheng tan"
paper_id: 367
arxiv_id: "2606.16774v1"
title: "OpenClaw-Skill: Collective Skill Tree Search for Agentic Large Language Models"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16774v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16774v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:07:00"
---
# OpenClaw-Skill: Collective Skill Tree Search for Agentic Large Language Models

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language model agents · skill tree search · collective intelligence · reinforcement learning

## 一句话总结

提出Collective Skill Tree Search (CSTS)和Collective Skill Reinforcement Learning (CSRL)框架，通过集体智能自动构建结构化、多样化且可迁移的技能树，显著提升LLM智能体在复杂OpenClaw环境中的工具使用、多步推理和泛化能力。

## 摘要

> Equipping Large Language Model (LLM) agents with effective skills is crucial for solving complex tasks in real-world systems like OpenClaw. In this work, we aim to develop a framework that automatically constructs such reusable skills to enhance LLMs in tool use, multi-step reasoning, and dynamic environment interaction. To this end, we propose Collective Skill Tree Search (CSTS), a novel tree-search-based skill construction framework that constructs structured, diverse and generalizable tree of skills. The core idea of CSTS is to leverage collective intelligence to jointly search, identify and compose effective skills via two iterative phases: Collective Skill Node Generation (CSN-Gen) and Collective Skill Node Assessment (CSN-Assess). CSN-Gen exploits collective knowledge from multiple models to explore diverse candidate skills for each subtask, enabling comprehensive skill exploration. CSN-Assess employs multiple models as judges to evaluate and select skill nodes with two scoring mechanisms: (1) collective quality scoring that aggregates independent evaluations to produce a robust estimate of skill effectiveness, and (2) collective transferability scoring that explicitly verifies whether a skill generalizes well across different models. With CSTS, we construct a set of comprehensive tree of skills along with skill-augmented training data, enabling models to effectively learn and utilize skills. Besides, we introduce Collective Skill Reinforcement Learning, which actively selects multiple relevant skills from the tree to broaden solution-space exploration, avoid being trapped by a single skill and its resulting homogeneous or suboptimal solutions. As a result, our trained model, OpenClaw-Skill, exhibits outstanding agentic capabilities in long-horizon planning, tool use and generalization over challenging benchmarks.

Q1: 这篇论文试图解决什么问题？

现有LLM智能体的技能构建方法面临三个主要局限：1) 技能碎片化（Skill Fragmentation），仅捕获孤立子任务的局部过程，缺乏整体关联；2) 多样性有限（Limited Diversity），单一模型固有偏见导致技能同质；3) 可迁移性差（Poor Transferability），技能在不同LLM骨干间性能下降明显。这些限制了智能体在OpenClaw等现实系统上处理复杂多步任务的能力。因此，需要一种自动化构建结构化、多样化且跨模型可迁移的技能框架。

Q2: 有哪些相关研究？

相关工作包括：1) LLM智能体在交互环境中的应用（如ReAct、Reflexion、Voyager），2) 基于技能的方法（如skill-based agents、程序化技能库），3) 树搜索方法（如MCTS用于推理或机器人技能学习）。现有技能构建多依赖单一模型或手工设计，缺乏系统性的自动构造和多样性保证。本文提出的CSTS首次将树搜索与集体智能结合，解决上述方法的碎片化、低多样性和差迁移性问题。

Q3: 论文如何解决这个问题？

核心方法分为两部分：1) Collective Skill Tree Search (CSTS)：首先将复杂任务分解为有序子任务序列，定义技能树深度；然后通过两个迭代阶段构建技能树：CSN-Gen利用多个LLM（如Qwen3系列）生成每个子任务的多样化候选技能节点，CSN-Assess引入集体质量评分（独立评估聚合）和集体可迁移性评分（验证技能跨模型泛化能力）筛选节点。最终形成结构化技能树，每层对应子任务，节点为技能，路径为组合技能序列。基于该树构造技能增强的SFT训练数据。2) Collective Skill Reinforcement Learning (CSRL)：在相同子任务下，从技能树中主动选择多个相关技能生成轨迹组，通过比较（如DPO风格）优化模型，使其倾向于更有效的技能策略，避免被单一技能导致的同质化或次优解束缚。

Q4: 论文做了哪些实验？

实验基于Qwen3-4B、Qwen3-8B、Qwen3.5-4B、Qwen3.5-9B四个骨干模型。使用CSTS收集2K高质量SFT示例，在8块H100上训练2个epoch，学习率5e-6。评估两个基准：1) QWENCLAWBENCH，报告类别级和总体得分；2) PINCHBENCH，报告最佳成功率和平均成功率。对比基线包括闭源模型（Claude-Opus-4.6, GPT-5.4等）和开源模型（MiniMax-M2.7, Llama 4 Maverick等）。此外进行消融研究（如CSRL有效性、技能多样性分析）。

Q5: 发现了什么实验现象？

1) OpenClaw-Skill在所有骨干上一致提升：总体得分在Qwen3-4B提升5.8点，Qwen3-8B提升4.3点，Qwen3.5-4B提升9.7点，Qwen3.5-9B提升10.4点。2) 提升在长程工具使用和反馈类别最明显，例如SVM指标从33.2提升至70.9。3) 在PINCHBENCH上，OpenClaw-Skill 9B在23任务版本中最佳成功率与闭源Claude-Opus-4.6相当（93.3 vs 93.3），平均成功率81.6超过多数开源模型。4) 消融表明CSRL带来额外增益，技能树结构化比平铺技能集更优。5) 集体可迁移性评分有效筛选出跨模型泛化能力强的技能。

Q6: 有什么可以进一步探索的点？

1) 将CSTS扩展到更多复杂环境（如机器人、软件工程）。2) 探索更高效的集体评估机制，减少多模型推理开销。3) 结合在线学习，使技能树随新任务动态演化。4) 研究技能树的可解释性和层级结构自动优化。5) 将CSRL与更先进的RL算法（如GRPO）结合。6) 降低对初始任务分解的依赖性，支持自动子任务发现。

Q7: 总结一下论文的主要内容

本文针对LLM智能体在OpenClaw等现实系统中的技能构建难题，提出Collective Skill Tree Search (CSTS)框架。CSTS通过集体智能（多模型协作）自动构建结构化技能树：先分解任务为子任务，再迭代生成多样化候选技能（CSN-Gen）并评估质量与可迁移性（CSN-Assess）。基于该树构造SFT数据训练模型，进一步提出Collective Skill Reinforcement Learning (CSRL)，通过比较不同技能下的轨迹来拓宽解空间。实验在QWENCLAWBENCH和PINCHBENCH上表明，基于Qwen3.5-4B和9B的OpenClaw-Skill模型取得一致显著提升，在长程规划、工具使用和泛化上达到或超越强闭源模型。论文贡献包括：首次将树搜索引入技能构建，集体智能保证多样性与可迁移性，CSRL增强探索。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：与智能体方向高度相关（用户方向权重0.10），直接解决技能自动构建问题。

## 基本信息

- 作者：Tianyi Lin, Chuanyu Sun, Jingyi Zhang, Changxu Wei, Huanjin Yao, Shunyu Liu, Xikun Zhang, Liu Liu, Jiaxing Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-15
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16774v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段，主要来自Introduction、Method和Results部分。
