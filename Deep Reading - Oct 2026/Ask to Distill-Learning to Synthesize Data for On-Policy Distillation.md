---
user_id: "cheng tan"
paper_id: 17006
arxiv_id: "2610.27450"
title: "Ask to Distill: Learning to Synthesize Data for On-Policy Distillation"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.27450.pdf"
pdf_url: "https://arxiv.org/pdf/2610.27450"
abs_url: "https://arxiv.org/abs/2610.27450"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:44:53"
---
# Ask to Distill: Learning to Synthesize Data for On-Policy Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · data synthesis · question generation · selective supervision

## 一句话总结

论文针对 on-policy distillation（OPD）提出一套问题合成与选择性监督框架：通过学习一个 synthesizer 生成问题，并用 teacher–student 分歧与学生熵构成的支持感知评分评估问题价值，从而在极少量问题时仍能高效完成大模型蒸馏。

## 摘要

> (a) Learn to ask where the teacher can help
> frozen scorer pair
> TRAINED
> On-policy distillation (OPD) can succeed with Synthesizer 𝑞 Student Teacher
> surprisingly few training questions, motivating S𝜃 𝜋𝑆 𝜋𝑇2026 a view of questions as carriers of distributional
> GRPO reward Σt st, st ≈ disagreement × compatibility knowledge transfer. How can we assess whether KL-filter keeps δ > τ →
> a newly generated question is valuable for distil- Teachable & reachableOct lation, and learn to generate better questions—

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是 on-policy distillation（OPD）中训练问题（question）的评估与生成问题：
1. OPD 的样本效率之谜：论文观察到 OPD 用出奇少的训练问题就能成功，这说明“问题”不仅是数据样本，更是分布知识的载体；问题质量而非数量决定蒸馏上限，因此需要一套评估问题价值的准则。
2. 缺少可判别的价值信号：现有的数据选择/选择性监督通常依赖 teacher–student 分歧、学生置信度等启发式，但缺少对问题本身分布性质的形式化刻画；论文希望回答“一个新生成的问题是否值得用于蒸馏”。
3. 问题生成未被学习化：已有 OPD 变体（GKD、MiniLLM、Speculative KD、Entropy-Aware OPD）大多在固定问题集上优化学生轨迹或教师目标，缺少一个能持续提出更有价值问题的 synthesizer。
4. 价值信号的多因素耦合：论文认为问题价值取决于两个互补条件——teachable（teacher 与学生存在足够分歧，δ>τ）与 reachable（学生在该候选支持集上具备相应的概率质量/熵），两者缺一不可，构成 KL-filter 的选择边界。
5. 理论与实验闭环：需要证明所提评分确实与可教区域覆盖相关（Proposition B.1 的 compatible-disagreement coverage），并在数学推理与代码生成任务上检验评分能否预测真实学习收益。

Q2: 有哪些相关研究？

相关研究主要分布在三个方向：
1. On-policy distillation：GKD（Agarwal et al., 2024）通过在 student 生成序列上训练，解决 fixed-response distillation 的分布不匹配；MiniLLM（Gu et al., 2024）基于 reverse KL 做 on-policy 优化。这是论文的直接基线。
2. 蒸馏轨迹与教师目标改进：Speculative Knowledge Distillation 通过交错 student 与 teacher 采样改进训练序列；Entropy-Aware OPD 在 reverse KL 之外引入 forward KL，利用学生熵信息；论文进一步把学生熵纳入问题价值评分，可视为该思路的扩展。
3. 数据效率与数据选择视角：论文引用了一篇关于 on-policy distillation 数据效率与数据选择的综述（CoRR abs/2609.05198），并将其作为“问题即知识载体”视角的学术背景。
4. 推理模型后训练与评测生态：背景涉及 DeepSeek-R1 等推理激励模型（ICLR 2026 相关文献），评测使用 MATH、AMC23、LiveCodeBench 等数学与代码基准。
5. RL 式奖励优化：synthesizer 的训练采用 GRPO 风格奖励，与 LLM 后训练中基于奖励的生成优化一脉相承。

Q3: 论文如何解决这个问题？

论文方法由评分函数、过滤机制与合成器训练三部分构成（部分公式细节需回原文确认）：
1. 支持感知（support-aware）价值评分：对 student 生成的推理轨迹逐状态计算 teacher 分布与 student 分布的分歧 δ，并结合 student 在该候选支持集上的熵/可及性 ω，形成综合评分 s。评分沿轨迹聚合（¯s），衡量一个问题能暴露多少 teacher 监督。
2. 理论保证：Proposition B.1（Compatible-disagreement coverage）指出，对任意非空候选集，评分与可教分歧之间存在 coverage 下界；如果某响应不在可教集内，则要么分歧 δ 低于阈值，要么可及性 ω 低于阈值，因此评分 s 不会超过对应上界。
3. KL-filter 选择性监督：只有同时满足 teachable（δ>τ）与 reachable（熵/可及性达标）的问题才进入蒸馏，避免把监督浪费在学生已掌握或无法企及的分布区域。
4. Synthesizer 训练：用 frozen scorer pair（教师+学生）提供奖励，以 GRPO 风格更新 synthesizer q；奖励即沿学生轨迹聚合的支持感知评分，使生成器学会提出那些能诱发高分歧且学生可及的问题。
5. 训练协议：数学推理任务使用分阶段模板（MATH/CODE SYSTEM 模板），共享 reward computation、discrepancy calibration、code OPD banks 与 LiveCodeBench 评测代码；训练超参数包括学习率 1e-6、熵系数 0.01、KL 系数 0.005、rollout temperature 1.0、共 60 步、每 5 步存 checkpoint。
6. 数据生成规模：生成 N=20,000 个原始问题，分 10 个 shard（每 shard 2000）处理，说明方法在中等规模合成数据上即可生效。

Q4: 论文做了哪些实验？

实验覆盖数学推理与代码生成两类任务（部分细节因证据缺失需回原文确认）：
1. 分歧预测学习价值实验：在 1,500 个自然问题上检验支持感知评分的分歧分量能否预测真实学习收益，报告 Spearman 相关系数（ρ(¯s, ¯δ)=0.90）。
2. 评分分桶准确率实验：将问题按评分分桶，对比六个基准上的平均准确率；在三个随机种子下报告均值±标准差，并给出 95% CI；结果显示高评分桶准确率显著更高（提示存在约 2.15× 的桶间差异，具体数值需核对原文）。
3. Score quintile 分组实验：按评分五分位把 1,500 个问题分组，比较各组蒸馏学习收益，检验评分的排序能力。
4. 数据效率实验：验证 OPD 用极少量问题即可成功的核心观察，并对比合成问题与随机/自然问题在监督密度上的差异。
5. 评测配置说明：AMC23 与 LiveCodeBench 被用作 checkpoint 选择基准，其报告分数不是 held-out 估计，解读结果时需注意这一边界。

Q5: 发现了什么实验现象？

实验现象与发现（部分数值需回原文核对）：
1. 分歧是评分的强预测因子：在自然问题上支持感知评分与分歧分量高度相关（Spearman ρ(¯s, ¯δ)=0.90），说明 teacher–student 分歧是问题价值的主要驱动。
2. 评分分桶与准确率强相关：六个基准平均上，高评分桶的准确率显著高于低评分桶（如 2.15× 的提升量级，具体数值需核对原文），支持评分作为问题选择依据的有效性。
3. 评分能预测学习收益：按五分位分组后学习收益随评分升高而增加，说明评分不仅反映当前分歧，还能预示蒸馏后的真实增益。
4. 少量问题即可驱动蒸馏：OPD 用出人意料地少的训练问题便能成功，这一反直觉现象支撑了“问题即分布知识载体”的观点，也说明数据质量比数量更重要。
5. 合成问题暴露更密集的监督：优化后的问题能暴露更密集、更丰富的 support-aware 监督信号，且不依赖更长的 student 轨迹，说明问题设计可以替代轨迹长度带来的覆盖收益。
6. 评测边界：AMC23 与 LiveCodeBench 仅作为 checkpoint 选择基准，其分数不能视为 held-out 泛化估计，这是结果解读中的重要张力点。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：
1. 把支持感知评分推广到更多模态（代码、工具调用、多轮对话、agent 轨迹），验证问题合成范式的通用性。
2. 研究 synthesizer 与 student 联合训练时的动态稳定性：当 student 能力提升后，评分分布会发生漂移，需要重新校准 synthesizer 或阈值 τ。
3. 将评分函数迁移到 agent 场景：在 agent 轨迹中状态空间更大、动作分支更多，问题价值评估可能需要结合环境反馈与长期收益。
4. 探索更高效的问题合成：例如用更少 shard、更小规模 synthesizer，或采用 online 自适应生成，降低合成成本。
5. 建立从问题分布到蒸馏收益的更完整理论：目前 Proposition B.1 只给出 coverage 下界，尚缺对不同问题分布之间优劣的定量刻画。
6. 失败案例分析：研究哪些问题会被 KL-filter 误弃（如学生熵过高但仍有学习空间）或误留（如分歧高但不可学习），以及评分在哪些分布区域失效。
7. 把“问题合成”与“课程学习”结合：按学生当前能力动态调整问题难度顺序，可能进一步提升数据效率。

Q7: 总结一下论文的主要内容

论文围绕 on-policy distillation 的数据效率问题提出“学会提问”的框架。核心动机是一个反直觉观察：OPD 用极少量训练问题即可成功，说明问题本身是分布知识转移的载体，问题质量而非数量决定蒸馏上限。论文由此提出两个问题：如何评估问题价值，以及如何学习生成更好问题。方法层面，论文定义支持感知评分 s，综合 teacher–student 分歧 δ 与学生熵/可及性 ω，并以 KL-filter（δ>τ）保证被选问题同时满足 teachable 与 reachable；同时用 GRPO 风格奖励训练 synthesizer q，使其学会生成能最大化有效监督的问题。理论部分给出 Proposition B.1 的 compatible-disagreement coverage 保证，说明评分与可教分歧之间的下界关系。实验在数学推理与代码任务上进行：验证分歧对学习价值的预测力（Spearman ρ=0.90）、评分分桶准确率差异、五分位学习收益以及六个基准的平均表现。结果显示优化后的合成问题能暴露更密集的监督信号，无需更长轨迹。训练协议细节包括学习率 1e-6、熵/KL 系数 0.01/0.005、rollout temperature 1.0、60 步训练、20,000 原始问题分 10 shard 生成。论文也明确 AMC23 与 LiveCodeBench 作为 checkpoint 选择基准而非 held-out 评估的边界。总体而言，该工作把蒸馏中的数据选择从启发式过滤升级为可学习的合成与评分闭环，为 OPD 的数据效率研究提供了一个系统性框架。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文把数据选择从启发式过滤升级为可学习的问题合成，对任何依赖合成数据做蒸馏或后训练的工作都有参考价值。

## 基本信息

- 作者：Jiashuo Sun, Hao Zhang, Xueqiang Xu, Heng Wang, Yixuan Xie, Pengcheng Jiang, Zhenrui Yue, Jiajun Fan, Peiran Li, Ge Liu, Shaowen Wang, Jiawei Han
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.27450`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要与 Introduction 片段，并结合启发式草稿完成；部分实验数值与机构信息因证据缺失未完全核实。
