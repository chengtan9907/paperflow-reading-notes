---
user_id: "cheng tan"
paper_id: 4428
arxiv_id: "2607.14614v1"
title: "Beyond Entropy: Correctness-Aware Advantage Shaping via Contrastive Policy Optimization"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14614v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14614v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:59:39"
---
# Beyond Entropy: Correctness-Aware Advantage Shaping via Contrastive Policy Optimization

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：contrastive policy optimization · reinforcement learning with verifiable rewards · correctness-aware advantage shaping · token-level contrastive disagreement

## 一句话总结

提出对比策略优化（Contrastive Policy Optimization, CPO），利用参考引导分布与普通生成分布间的标记级对比分歧作为正确性感知的优势信号，解决了基于熵的优势信号无法区分有用不确定性和有害混淆的问题，在可验证奖励任务上显著优于基线方法。

## 摘要

> Reinforcement learning with verifiable rewards (RLVR) commonly uses entropy for advantage shaping. However, entropy cannot distinguish useful uncertainty from detrimental confusion, limiting its effectiveness as a correctness signal. We propose Contrastive Policy Optimization (CPO), which uses token-level contrastive disagreement between reference-guided and vanilla generation distributions for correctness-aware advantage shaping. Both theoretical and empirical results show that this disagreement reliably indicates token-level correctness. We further show that On-policy Distillation is a special case of CPO, where the posterior distribution is instantiated by an external teacher model. CPO also resolves the zero-advantage problem. Experiments on in-domain and out-of-domain benchmarks demonstrate that CPO substantially outperforms entropy-based RLVR methods while maintaining strong generalization. Further analysis shows that correct and incorrect responses naturally support exploration and exploitation respectively, and balancing both leads to the best performance.

Q1: 这篇论文试图解决什么问题？

在RLVR中，策略优化常使用熵作为优势塑造信号以鼓励探索，但熵是全局不确定性度量，无法区分高熵源于探索性正确推理（有用不确定性）还是错误路径（有害混淆），导致优势信号噪声大、学习不稳定。此外，GRPO等方法会驱动熵趋近于零，严重约束模型探索能力，并存在零优势问题（策略确定时优势消失）。因此，需要一种正确性感知的token级优势信号，既能识别正确token以加强，又能识别错误token以抑制，并维持适度探索。

Q2: 有哪些相关研究？

相关研究主要包括：基于熵的RLVR方法（如PPO、GRPO）用熵奖励控制探索，但熵的局限性已被广泛讨论；对比训练方法在RL中的尝试（如对比反事实学习）；在线蒸馏方法通过最小化策略与教师分布的KL散度来学习；GRPO使用群体相对优势但熵仍会崩溃；GSPO等探索轨迹级优势。CPO借鉴对比概率变化思想，直接以覆盖分歧作为优势，并与蒸馏建立联系。此外，在数学推理和代码生成任务中探索与利用的平衡也与之相关。

Q3: 论文如何解决这个问题？

CPO的核心是使用参考引导分布与当前策略分布之间的token级对比分歧作为优势信号。首先理论证明，对于可验证任务，该对比分歧（对数概率差）能可靠指示token正确性：正确token倾向于概率增加，错误token降低。在此基础上，CPO将对比分歧直接作为优势函数进行策略更新：对每个token，从参考策略（如初始策略或教师模型）获取参考分布，计算当前策略下该token的对数概率与参考对数概率之差，作为优势。该优势为正值时鼓励token，为负时抑制。CPO还通过稳定化处理避免极端值。进一步证明，在线蒸馏（最小化两个分布KL散度）是CPO在特定优势权重下的特例。CPO天然解决零优势问题：当策略完全确定时，参考分布仍可能提供非零对比信号。

Q4: 论文做了哪些实验？

实验在多个推理基准（GSM8K、MathQA、MATH、HumanEval等）上进行，基包括GRPO、PPO、DPO变体等。实验设置：域内性能比较、域外泛化测试、消融研究（对比分歧 vs 熵）、零优势设置对比、探索程度分析。具体细节见论文。

Q5: 发现了什么实验现象？

1) CPO在所有基准上显著优于基于熵的基线，域内外均保持优势；2) GRPO训练中熵快速趋近零，而CPO保持适度熵，持续支持探索；3) 正确token获得正优势促进探索，错误token获得负优势抑制错误，实现自动平衡；4) 零优势设置下，CPO比专门方法RL-ZVP平均高3.2%，说明其优势信号更有效；5) 消融表明对比分歧贡献最大，熵信号的作用较小。

Q6: 有什么可以进一步探索的点？

1) 将CPO扩展到连续控制或更复杂的任务；2) 探索其他形式的对比分歧（如对称KL散度、JS散度）；3) 结合课程学习或自适应参考策略；4) 理论分析CPO的收敛性和最优性；5) 在更大规模模型和更多任务上验证；6) 减少对参考分布的计算依赖。

Q7: 总结一下论文的主要内容

本文提出了对比策略优化（CPO），一种针对RLVR中熵信号局限性的正确性感知优势塑造方法。通过理论和实验验证，参考引导与当前策略分布之间的token级对比分歧能可靠指示token正确性。基于此，CPO直接以该分歧作为优势更新策略，并自然解决了零优势问题，同时揭示在线蒸馏是特例。实验在多个推理基准上展示CPO显著优于熵方法，且支持有效的探索-利用平衡。贡献包括新信号、新框架、理论联系和实验验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接针对生成任务中可验证奖励的策略优化

## 基本信息

- 作者：Weiwen Xu, Jia Liu, Hou Pong Chan, Long Li, Deng Cai, Min Chen, Hao Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14614v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本回答参考了论文Abstract及检索到的证据片段（方法、结果、背景等）进行生成。
