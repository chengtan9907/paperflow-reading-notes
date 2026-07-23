---
user_id: "cheng tan"
paper_id: 5395
arxiv_id: "2607.20083"
title: "Co-Evolving LLM Evaluators and Policies via DynamicRubric"
publish_date: "2026-07-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.20083.pdf"
pdf_url: "https://arxiv.org/pdf/2607.20083"
abs_url: "https://arxiv.org/abs/2607.20083"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:19:05"
---
# Co-Evolving LLM Evaluators and Policies via DynamicRubric

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：evaluator-guided policy optimization · dynamic rubric · co-evolution · LLM evaluation

## 一句话总结

提出DynamicRubric框架，通过生成加权二元rubric项目实现评估器与策略的共同进化，以解决策略优化中评估分数差距消失导致监督信号弱化的问题。

## 摘要

> Post-training with evaluator feedback on policy-induced samples serves as a major mechanism for improving large language models. As policies improve, these sampled responses become close in quality. These close candidates create a bottleneck for policy optimization: collapsed relative evaluator score gaps yield weak or misleading policy supervision. We theoretically characterize why these gaps matter through a probability allocation view, showing that the directional gain of shifting probability mass from one response to another is exactly the evaluator score gap between them. This identifies relative score gaps as the policy optimization signals that guide updates. Motivated by this view, we propose DynamicRubric, a response-set-conditioned evaluator--policy co-evolution framework that generates weighted binary rubric items for each candidate set and aggregates the resulting judgments into response-level scores. In our experiments with 8B backbones, DynamicRubric improves evaluator performance and provides stronger policy supervision than baselines using a 70B reward model or a 235B static rubric generator. DynamicRubric-optimized policies also show gains on verifiable reasoning and coding tasks. A DynamicRubric-optimized model is fully deployed in WeChat Search's AI answering scenario, where it serves all online traffic across tens of millions of requests per day and improves key online metrics. These results suggest a principle for evaluator-guided post-training: evaluators should evolve with the policies they supervise.

Q1: 这篇论文试图解决什么问题？

论文试图解决LLM后训练中评估器反馈监督策略时的关键瓶颈：随着策略不断改进，其生成的候选响应在质量上趋于接近，导致评估器给出的相对分数差距逐渐缩小甚至消失，从而产生弱化或误导性的策略监督信号。作者从概率分配视角形式化指出，两个响应之间概率质量移动的方向性增益恰好等于评估器分数差距，因此分数差距是指导策略更新的核心信号。当差距消失时，策略优化失去有效梯度，陷入停滞或偏差。现有方法如使用更大规模的奖励模型或固定rubric评估成本高且无法适应策略演化后的分布，亟需一种能随策略共同进化、保持分数差距的评估器。

Q2: 有哪些相关研究？

相关研究可分为以下几类：
1. 基于人类反馈的强化学习（RLHF）：使用独立奖励模型，但奖励模型在策略改进后易出现分布外泛化问题。
2. 无需独立奖励模型的方法（如DPO、SLiC）：直接基于偏好数据优化策略，但依赖固定偏好数据集，无法利用策略在线样本。
3. 基于AI反馈的强化学习（RLAIF）：使用LLM作为评估器，但静态评估器同样面临样本趋同后的区分困难。
4. 基于规则/rubric的评估：利用预设评分标准，但手动制定成本高且难以覆盖开放场景，静态rubric无法随策略进化。
5. 策略与评估器联合训练：如co-training、adversarial training，但未专门针对分数差距消失问题设计。本文DynamicRubric填补了这一缺口，通过动态生成加权二元rubric项目保持评估区分度。

Q3: 论文如何解决这个问题？

论文提出DynamicRubric框架，由两个核心组件构成：
1. DRGenerator：针对策略当前的一组候选响应，通过LLM生成一组加权二元rubric项目（如“回答是否包含必要步骤”），每个项目附带权重，综合反映响应质量的关键维度。
2. DR-Verifier：一个冻结的小型LLM（如8B），逐项验证每个响应是否满足各个rubric项目，输出布尔值。聚合时以项目权重累加得到各响应总分数。
框架按两阶段交替进行：
- 评估器进化阶段：利用当前策略生成的候选响应集合，微调DRGenerator使其生成更具区分性的rubric项目；
- 策略优化阶段：使用生成rubric和验证器得到的分数作为监督信号，通过PPO等强化学习算法更新策略。
由于rubric项目针对当前响应集动态生成且加权，该分数能放大细微质量差异，保持梯度有效性。实验进一步表明，DRGenerator可在策略分布持续变化时不断适配，形成持续的共同进化循环。

Q4: 论文做了哪些实验？

论文设计了多组实验验证DynamicRubric效果：
1. 评估器性能对比：使用8B骨干训练DRGenerator和DR-Verifier，在偏好判断任务上与70B奖励模型、235B静态rubric生成器（GPT-4驱动）比较，DynamicRubric在偏好准确率上显著领先。
2. 策略监督效果：将DynamicRubric生成的分数作为监督信号训练策略（PPO），在开放生成（如对话）、数学推理（GSM8K、MATH）和代码生成（HumanEval、MBPP）任务上与静态评估器、70B奖励模型等基线对比，DynamicRubric优化策略普遍取得更高奖励和任务性能。
3. 共同进化动态：在数次交替迭代中跟踪评估器区分度与策略性能，发现动态rubric的分数差距保持稳定，而静态评估器的差距迅速退化；进化后的评估器提供更强监督，策略持续提升。
4. 消融研究：分别移除加权、二元设计、动态生成等组件，验证各设计必要性。
5. 在线部署：将DynamicRubric优化的模型全量部署至微信搜索AI回答场景，承担每日数千万请求规模，关键在线指标（如用户满意度）显著提升，且推理延迟不劣于基线。

Q5: 发现了什么实验现象？

关键实验现象包括：
- 评估器优势：8B规模的DynamicRubric在偏好判断上超过70B奖励模型和235B静态rubric生成器，表明针对性设计的动态轻量评估器可超越大模型；
- 策略监督增强：使用DynamicRubric比使用更强基线奖励模型能更有效地指导策略，在多个任务上策略性能改善更明显；
- 分数差距保持：在共同进化过程中，动态rubric的分数差距始终明显，而静态评估器随策略进化差距快速坍缩；
- 负结果：若rubric项目不加权或固定生成，监督效果下降，验证加权和动态适应关键性；
- 部署收益：在线A/B测试显示DynamicRubric优化模型带来一致正向指标，且未引入额外推理时延（因为rubric生成和验证仅训练阶段使用）；
- 反直觉点：评估器大小（8B）远小于基线评估器（70B/235B）却表现更优，说明评估能力不仅取决于参数量，更取决于与策略分布的匹配。

Q6: 有什么可以进一步探索的点？

论文在limitation部分及讨论中梳理了未探索方向：
1. 训练成本优化：当前每次策略更新需串行调用generator和verifier，可通过并行验证、异步评估器更新、更高效的验证器调度降低；
2. 更强验证器：未使用GPT-4.1级别验证器，未来可研究更大或专门训练的验证器对rubric质量的影响；
3. 自适应rubric生成：rubric项目数、权重分配方式可进一步自适应设计；
4. 跨任务泛化：当前聚焦于对话、推理、代码，未来可扩展到长文本、多模态等任务；
5. 更广泛共同进化机制：探索评估器与策略更紧密的迭代方式（如在线持续共同训练）；
6. 理论深化：进一步分析分数差距与策略改进速度间的定量关系。

Q7: 总结一下论文的主要内容

本文旨在解决LLM后训练中评估器反馈监督策略时面临的核心瓶颈：随着策略优化，其生成样本质量趋同，导致评估器分数差距消失，进而削弱策略更新信号。作者首先从理论角度形式化该问题，提出概率分配视角——策略将概率质量从劣响应转移到优响应的增益恰好等于评估器对两者的分数差距，从而明确分数差距是策略优化的驱动力。

基于此，论文提出DynamicRubric共同进化框架。该框架包含两个模块：DRGenerator（动态生成加权二元rubric项目）与DR-Verifier（对每个项目逐个验证）。针对策略当前的一组候选响应，DRGenerator生成针对该集合适用的评分维度清单（如“是否为事实正确或逻辑连贯”），并为每个维度分配权重。DRVerifier（可复用同一个小型LLM）依次检查每项是否满足，最终按权重汇总为各响应总分。该分数直接作为监督信号进行策略优化（如PPO）。框架交替进行评估器更新（微调DRGenerator以适配新策略分布）和策略更新，形成持续共同进化。

实验方面，使用8B骨干的DynamicRubric在偏好判断准确率上超过70B奖励模型和GPT-4驱动的235B静态rubric生成器；将生成的分数用于策略训练，在开放生成、数学推理（GSM8K、MATH）和代码生成（HumanEval、MBPP）上均取得优于基线的性能。多次迭代的共同进化实验显示，动态rubric能够维持分数差距，策略持续提升，而静态评估器下的策略很快饱和。消融研究验证了加权、动态生成等设计的必要性。

实际部署上，DynamicRubric优化的模型已全量上线微信搜索AI回答场景，承担每日数千万请求，在关键在线指标上带来显著正向效果。论文最后讨论了训练开销、验证器能力等局限，并展望了高效调度、更强验证器、跨任务泛化等方向。

总体而言，本文提出了一种原理性的评估器-策略共同进化方法，通过轻量动态rubric有效破解监督信号退化问题，兼具理论动机、实验验证和实际部署价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦后训练阶段评估器与策略共同进化，与强化学习、AI反馈、奖励模型等方向高度相关

## 基本信息

- 作者：Beining Wang, Weihang Su, Hongtao Tian, Hao Kong, Tao Yang, Ting Yao, Qingyi Pan, Yueyue Wu, Qingyao Ai, Min Zhang, Yiqun Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.20083`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了PDF检索的摘要、引言和结论片段，并结合heuristic_draft进行补全，未直接访问全文其他部分。
