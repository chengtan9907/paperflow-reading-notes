---
user_id: "cheng tan"
paper_id: 1392
arxiv_id: "2606.26091v1"
title: "On-Policy Self-Distillation with Sampled Demonstrations Reduces Output Diversity"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.26091v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.26091v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T16:12:26"
---
# On-Policy Self-Distillation with Sampled Demonstrations Reduces Output Diversity

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：self-distillation · on-policy · rollout diversity · pass@k

## 一句话总结

本文分析了on-policy self-distillation with sampled demonstrations (SDSD) 在提升 pass@1 准确率的同时，导致 rollout 多样性降低和 pass@k 曲线平坦的机制，并给出理论解释与实验验证。

## 摘要

> On-policy self-distillation achieves strong pass@1 accuracy by using a single model as both teacher and student, with the teacher conditioned on a correct demonstration to provide dense token-level feedback. We show that this could come at a hidden cost: rollout diversity decreases and pass@k curves flatten (i.e., generating more rollouts fails to improve accuracy). We trace this to compounding biases in the design of self-distillation with sampled demonstrations. The teacher scores each student rollout while conditioned on a sampled correct rollout, channeling its feedback through the model's own biases. We theoretically analyze the optimal self-distillation policy and show that it tilts the base distribution by a pointwise conditional mutual information score between the student's rollout and the correct rollout used as context. Unlike the ideal optimal on-policy reinforcement learning (RL), which preserves probability ratios among equally correct rollouts, self-distillation can amplify existing probability gaps, concentrating mass on already-dominant modes. On a controlled graph path-finding task and science question-answering benchmarks, self-distilled models match or exceed RL on average performance but exhibit substantially lower functional and semantic diversity, failing on out-of-distribution settings that require diverse strategies.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决 on-policy self-distillation with sampled demonstrations (SDSD) 在提高 pass@1 准确率时，可能悄悄损害输出多样性的问题。具体而言，尽管 SDSD 模型取得了良好的平均准确率，但其 pass@k 曲线往往平坦（生成更多 rollout 无法提升准确率），表明功能多样性坍塌。论文希望揭示这一现象背后的机制——即教师条件示范反馈如何引入系统性偏差，并理论地分析最优策略与理想 on-policy RL 的差异。

Q2: 有哪些相关研究？

相关研究涉及三个主要方向：
1. 自蒸馏 (Self-Distillation)：用同一模型同时作为教师和学生进行知识蒸馏，通常能提升单次准确率。
2. On-policy 强化学习 (RL)：通过环境奖励或可验证信号优化策略，理想情况下能等比例保留正确 rollout 间的概率比。
3. 生成多样化度量：常见方法如 token 级熵（论文 §4.4 指出其无法捕获功能/语义多样性），而本文强调 pass@k 曲线平坦是功能多样性坍塌的直接指标。
此外，[5_related_work] 片段提及一种方法：利用教师指导改变 RL 梯度幅度，但保持可验证奖励的方向——这可能是当前缓解多样性下降的对比基准。

Q3: 论文如何解决这个问题？

论文通过理论与实验双线解决该问题：
1. 理论分析：推导 SDSD 的最优政策形式，证明其等价于在教师条件正确示范下，通过点状条件互信息 (Pointwise Conditional Mutual Information, PCMI) 对 student rollout 进行加权。与理想 on-policy RL（等比例保留所有正确 rollout）不同，PCMI 加权倾向于放大已与示范及基础政策更兼容的 rollout，从而加剧模式集中（Fig. 1 示意）。
2. 实验验证：在受控图路径寻找任务（合成有向图，允许多种正确路径）与科学问答基准（Feng et al., 2024）上，比较 SDSD 与 on-policy RL 的 pass@1、pass@k 曲线以及功能/语义多样性指标。额外检验 token 级熵的失效性（§4.4）。

Q4: 论文做了哪些实验？

论文设计了两个主要实验场景：
1. 图路径寻找任务：一个受控的有向图环境，节点间有多条等价正确路径。模型需从起点生成到终点的路径 token 序列。测量 pass@1 准确率、pass@k 曲线、以及基于路径集合的功能多样性（不同路径的数量）。
2. 科学问答基准 (Feng et al., 2024)：包含需要多步推理的科学问题，答案可能对应多种合理的推理链。同样评估 pass@1、pass@k 以及基于语义内容的多样性（如蕴含不同解题思路的答案频率）。
对比方法包括：
- On-policy RL：使用可验证奖励信号进行策略梯度优化。
- SDSD：使用同一模型作为教师和学生，教师上下文包含采样的正确示范。
- 其他基线（如无蒸馏、固定教师蒸馏等）。
实验报告了 pass@1 与 pass@k 的数值，以及功能/语义多样性的定量结果，并进行了消融研究（§4.4 验证 token 级熵无法反映实际多样性差异）。

Q5: 发现了什么实验现象？

1. 性能与多样性的权衡：SDSD 在 pass@1 准确率上匹配甚至超过 on-policy RL，但 pass@k 曲线明显更平坦——即生成更多 rollout（k 增大）带来的准确率提升微乎其微，表明模型产生的正确输出模式高度重复。
2. 功能/语义多样性坍塌：在图路径任务中，SDSD 倾向于只探索少数几条路径，而 RL 能保留多种正确路径；在科学问答中，SDSD 的答案语义集聚度高（多为相同思路），RL 则覆盖更多解题策略。
3. token 级熵的失效：论文在 §4.4 中直接展示，尽管 SDSD 模型在 token 级条件熵上与 RL 接近甚至更高，但其功能/语义多样性却更低，说明 token 级熵无法区分不同层次多样化。
4. 分布外泛化失败：在测试时引入需要新策略的变种任务上，SDSD 准确率急剧下降，而 RL 因为保留了多样化策略而更具鲁棒性。

Q6: 有什么可以进一步探索的点？

1. 缓解多样性下降的方法：论文在 [5_related_work] 片段提及一种已有思路（使用教师指导改变 RL 梯度幅度，但保持可验证奖励的方向），可进一步探索将其与 SDSD 结合以恢复多样性。
2. 更合理的多样性度量：鉴于 token 级熵失效，需要开发能捕捉功能或语义多样性的新指标（例如基于推理结构或语义等价类的多样性度量）。
3. 多示范与去偏策略：当前 SDSD 仅使用一个正确示范作为教师条件，尝试多个示范或对示范进行反事实去偏可能减轻 PCMI 导致的模式集中。
4. 扩展到更多应用：论文仅在路径寻找和科学问答上验证，未来可在数学推理、代码生成等需要多解的任务中评估影响，并设计针对性干预。
5. 对 SDSD 理论的深化：当前 PCMI 分析基于离散 rollout，可推广到连续动作或更复杂的序列生成场景。

Q7: 总结一下论文的主要内容

本文系统研究了 on-policy self-distillation with sampled demonstrations (SDSD) 对输出多样性的影响。首先，通过理论分析揭示 SDSD 的最优策略会通过点状条件互信息 (PCMI) 加权，使得模型过度增强与已采样示范兼容的 rollout，从而造成模式坍塌——这是与理想 on-policy RL（保留正确解概率比）的关键区别。其次，在图路径寻找和科学问答基准上实验证实：SDSD 虽能取得高 pass@1 准确率，但 pass@k 曲线平坦，功能/语义多样性显著低于 RL，且在分布外泛化中失败。论文还指出 token 级熵不能有效反映这种多样性损失。最后，讨论了缓解策略（如修改梯度方向）并指出未来方向。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：本文与你兴趣方向中的「AI for Science」直接相关：科学 QA 任务正是知识推理与多解探索的典型场景。

## 基本信息

- 作者：Andrei Liviu Nicolicioiu, Mohammad Pezeshki, Aaron Courville
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI
- 日期：2026-06-24
- 推荐级别：**值得快速浏览**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.26091v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了检索到的 PDF 证据（abstract、introduction、conclusion 及 related_work 片段），并依据 heuristic_draft 进行补充和润色。
