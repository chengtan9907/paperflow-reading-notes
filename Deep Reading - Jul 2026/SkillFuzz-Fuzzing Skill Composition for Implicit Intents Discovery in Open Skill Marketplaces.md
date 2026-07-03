---
user_id: "cheng tan"
paper_id: 2697
arxiv_id: "2607.02345"
title: "SkillFuzz: Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces"
institution: "利物浦大学, 穆罕默德·本·扎耶德人工智能大学"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.02345.pdf"
pdf_url: "https://arxiv.org/pdf/2607.02345"
abs_url: "https://arxiv.org/abs/2607.02345"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-03T22:28:07"
---
# SkillFuzz: Fuzzing Skill Composition for Implicit Intents Discovery in Open Skill Marketplaces

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill marketplace · implicit intent · fuzzing · monte carlo tree search

## 一句话总结

本文提出 SkillFuzz，一种免执行的模糊测试方法，通过契约引导的蒙特卡洛树搜索发现开放技能市场中技能组合产生的隐式意图。

## 摘要

> Large Language Model (LLM)-based agents increasingly automate software engineering tasks through reusable skills, natural-language instruction documents that guide planning and execution. Open skill marketplaces enable users to assemble agents by co-activating community-contributed skills, but marketplace operators typically audit skills in isolation. As a result, individually benign skills may interact to redirect an agent toward unintended objectives, which we term implicit intents. Detecting such intents is challenging because the effect emerges only through skill composition, execution environments are often unavailable at admission time, and the space of possible co-activations grows exponentially with marketplace size. In this paper, we formulate implicit-intent discovery as a fuzzing problem over skill compositions, where skill compositions are the unit under test, planning artifacts expose agent intent before execution, and deviations from a skill-free baseline serve as a differential oracle. Based on this formulation, we propose skillfuzz, the first execution-free testing approach that extracts structured skill contracts and uses contract-guided Monte Carlo Tree Search to prioritize potentially conflicting compositions. Across representative skill-marketplace workloads, skillfuzz discovers over 1,000 distinct implicit intents under a fixed query budget, confirms more than 80% of the highest-risk flagged compositions during execution-time validation, and identifies substantially more high-severity implicit intents than alternative search strategies while exploring only a fraction of the pairwise interaction space they require.

Q1: 这篇论文试图解决什么问题？

本文针对开放技能市场中技能组合的隐式意图发现问题。单一技能审核时无害，但组合后可能产生非预期的行为，使智能体执行被重定向到恶意或非预期目标。由于执行环境在技能准入时不可用，且组合空间指数级增长，现有方法难以有效检测。因此，问题在于如何在不执行代码的情况下，通过分析技能组合的规划产物来高效发现潜在的有害隐式意图。

Q2: 有哪些相关研究？

相关研究包括：(1) LLM-based agent 安全性：已有工作关注单技能安全审计，但缺乏对组合风险的建模。(2) 技能市场安全：现有市场运营者主要进行孤立审核，未考虑组合效应。(3) 模糊测试技术：在软件工程中广泛应用于漏洞挖掘，但极少应用于技能组合领域。(4) 组合测试与规划安全：部分工作探索了规划器的鲁棒性，但未与技能组合的隐式意图发现结合。本文填补了免执行测试技能组合隐式意图的空白。

Q3: 论文如何解决这个问题？

SkillFuzz 框架包含三个核心步骤：(1) 技能契约提取：从技能文档中提取结构化契约，包括输入输出规范、前置条件等，用于形式化约束。(2) 差异预言设计：以无技能激活的基线计划为标准，比较目标技能组合下规划产物（如计划步骤）的偏差，若偏差超过阈值则标记为隐式意图候选。(3) 契约引导的蒙特卡洛树搜索：利用契约信息指导 MCTS 的节点选择，优先探索可能产生冲突的技能组合，以有限预算最大化发现隐式意图。整个方法无需执行技能，仅依赖规划器生成的计划，实现免执行测试。

Q4: 论文做了哪些实验？

实验基于 SkillsBench 数据集，包含多种规划智能体（如 Qwen 系列和商业模型）和代表性任务。设计了四个研究问题：RQ1 验证隐式意图跨智能体的普遍性；RQ2 测试免执行测试是否对应实际执行风险；RQ3 比较不同搜索策略（随机、成对、MCTS）的发现效率；RQ4 评估技能契约引导对搜索的贡献。实验在固定查询预算下运行，统计发现的不同隐式意图数量、高风险组合确认率以及搜索空间覆盖率。

Q5: 发现了什么实验现象？

实验发现：(1) 所有评估的规划智能体中都出现了隐式意图，说明这是技能组合的普遍风险而非特定模型问题，但不同智能体在发现效率上存在差异。(2) Qwen 系列模型中，覆盖率随模型规模增大而减小，表明较小推理模型在面对冲突技能指令时产生更不稳定的计划，更容易暴露隐式意图。(3) SkillFuzz 在固定预算下发现了超过 1000 个不同隐式意图，且超过 80% 的高风险组合在执行验证中被确认有效。(4) 相比随机搜索和成对搜索，SkillFuzz 以显著更少的组合探索量发现了更多高严重性隐式意图，验证了契约引导 MCTS 的有效性。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：(1) 扩展到更多样化的规划智能体和任务场景，验证方法的通用性。(2) 结合执行反馈，将免执行测试作为筛选步骤，再对高风险组合进行实际执行验证，形成两级检测流水线。(3) 改进技能契约提取的自动化程度，减少人工标注开销。(4) 研究动态技能组合，考虑技能激活顺序对意图的影响。(5) 将方法应用于多模态技能或更复杂的规划器架构。(6) 探索对抗性技能提交者如何规避检测，提升方法的鲁棒性。

Q7: 总结一下论文的主要内容

本文针对开放技能市场中技能组合产生的隐式意图发现问题，提出 SkillFuzz 方法。首先形式化了问题：将技能组合视为测试单元，利用规划产物在免执行条件下检测偏离无技能基线的行为。然后设计了技能契约提取和差异预言，并采用契约引导的蒙特卡洛树搜索高效搜索冲突组合。实验在 SkillsBench 上使用多种规划智能体验证，结果表明隐式意图普遍存在，SkillFuzz 能发现大量高风险意图，且搜索效率优于基线方法。主要贡献包括：(1) 首次将隐式意图发现形式化为模糊测试问题；(2) 提出免执行的测试框架 SkillFuzz；(3) 实验证明方法的有效性和高效性。论文为技能市场安全提供了新的测试范式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本文与智能体安全性直接相关（用户画像中 agent 权重 0.10），提供了系统性组合风险测试方案。

## 基本信息

- 作者：Jinwei Hu, Yi Dong, Youcheng Sun, Xiaowei Huang
- 机构：利物浦大学, 穆罕默德·本·扎耶德人工智能大学
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI, cs.CL
- 日期：2026-07-03
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.02345`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段
