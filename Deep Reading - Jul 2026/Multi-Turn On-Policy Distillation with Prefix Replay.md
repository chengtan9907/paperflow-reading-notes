---
user_id: "cheng tan"
paper_id: 2717
arxiv_id: "2607.04763"
title: "Multi-Turn On-Policy Distillation with Prefix Replay"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04763.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04763"
abs_url: "https://arxiv.org/abs/2607.04763"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:15:35"
---
# Multi-Turn On-Policy Distillation with Prefix Replay

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · multi-turn agent · prefix replay · prefix trap

## 一句话总结

提出 ReOPD，一种无需环境交互的多轮在线蒸馏方法，通过前缀重放和步骤衰减采样技术解决前缀陷阱，在保持准确率的同时实现至少4倍训练加速。

## 摘要

> We study on-policy distillation (OPD) for agentic tasks, where an LLM agent interacts with an environment over multiple turns and a student imitates a teacher over these multi-turn interaction histories. Fully online OPD is costly because each update requires fresh student rollouts through the environment and teacher queries at visited histories. We propose Replayed-Prefix On-Policy Distillation (ReOPD), an off-environment alternative that reuses pre-collected teacher trajectories as replayed prefixes: the student acts at selected steps, while the teacher provides dense per-step supervision without executing new environment interactions. We show that multi-turn OPD introduces a prefix trap: making histories more student-on-policy improves relevance to the student, but can query the teacher on histories where its target is unreliable. This creates a two-sided distribution shift between student occupancy and teacher reliability. ReOPD addresses this by treating multi-turn OPD as a reliability-aware prefix distribution design and implements it with a simple step-decaying sampling schedule that emphasizes early, lower-shift prefixes. Across mathematical reasoning with Python and search environments over multiple teacher and student model scales, ReOPD preserves or improves OPD-level accuracy, uses zero tool calls during student training, and is at least $4\times$ faster per training step than OPD. ReOPD therefore turns expensive agent-environment interaction into a reusable offline resource, enabling scalable distillation across tools, tasks, and environments.
> ![](images/c448e6bcaf54a57dd572eeeb597f0958387d2f114def5abe82902386ca8cc3f6.jpg)
> ![](images/0acd238a2a7b78bbe2efd01fd126c3379819c3ad67f402fb52fa3d0b8a308962.jpg)
> ![](images/7e109343e71270bf0f5389c8bf2a95340d2c07339b5c8361b9c5e144dba73953.jpg)
> Figure 1: ReOPD keeps the benefits of on-policy distillation while removing environment interaction. ReOPD matches or improves OPD accuracy, but trains much faster per step and eliminates tool calls during training by replaying teacher-recorded prefixes instead of executing fresh environment rollouts. The student/teacher models are Qwen3-4B-Instruct-2507 and Qwen3-8B, respectively.
> ![](images/aeb0908b68f690afb6483bf5176280375b22d04a8065061892bb989d5d4c51f0.jpg)
> Figure 2: Comparison between OPD and ReOPD for agentic tasks. Up: OPD with online environment. The environment is always alive during training. And all steps equally contribute to the loss. Down: ReOPD with offline environment. The environment is only alive for collecting the teacher's trajectories, which can happen during the training of the teacher agent by using RL, like GRPO. Afterwards, the environment is not needed for the training of the student agent. The earlier steps contribute more to the loss.

Q1: 这篇论文试图解决什么问题？

现有的多轮在线蒸馏（OPD）需要学生与环境频繁交互并查询教师，计算成本和环境依赖极高，难以扩展。此外，多轮 OPD 存在前缀陷阱（prefix trap）：使学生历史更接近在线分布（on-policy 采样）虽然与学生相关性强，但可能导致教师在这些历史上的目标不可靠，产生双向分布偏移。因此，需要一种既能利用在线蒸馏优势又能避免高成本和分布偏移的方法。

Q2: 有哪些相关研究？

合理推断。论文涉及的相关研究包括：(1) 在线蒸馏（On-Policy Distillation）在智能体任务中的应用；(2) 离线蒸馏或行为克隆（SFT）在模仿学习中的使用；(3) 多轮交互中的模仿学习与强化学习；(4) 教师学生训练中的分布偏移问题，如 DAgger 算法；(5) 前缀重放技术在离线强化学习或序列模型中的应用。文中引用了多个相关工作（如 Yao et al., Schick et al., Nakano et al., Jin et al.），但检索证据未提供具体细节，建议回看原文 'Related Work' 部分。

Q3: 论文如何解决这个问题？

论文提出 ReOPD，无需环境交互的多轮在线蒸馏方法。核心思路：从固定池中重放预收集的教师轨迹作为前缀（teacher-forced prefix），在特定步骤让学生行动，教师提供目标。具体实现：(1) 前缀分布设计：将多轮 OPD 简化为选择有效前缀分布ρ_t，该分布需平衡学生相关性（on-policy）与教师可靠性（前缀偏移小）；(2) 步骤衰减采样调度：采用对步骤号单调递减的采样概率，优先采样早期步骤的前缀，因为早期前缀较短、分布偏移较小，教师目标更可靠；(3) 训练过程：从教师轨迹池中按衰减概率选择一个教师轨迹和步骤t，学生在该步骤进行单步预测，教师提供监督。整个过程不调用环境，因此训练成本由离线轨迹池决定，比在线 OPD 快得多。

Q4: 论文做了哪些实验？

论文在两类智能体任务上进行了实验：(1) 数学推理任务，使用 Python 执行环境（如计算、代码验证）；(2) 搜索任务，可能需要调用搜索引擎。实验中比较了 ReOPD 与完全在线 OPD、离线蒸馏（SFT）等基线，并在不同教师和学生模型规模（如小模型 7B 到大模型 70B）下评估。实验测量了准确率（即任务成功率）和训练吞吐量（每步时间）。具体数据集和任务细节未在检索证据中明确，但大致包括多轮交互直至任务完成。论文还进行了消融实验，验证步骤衰减采样的效果，以及不同采样策略（均匀、反衰减等）的比较。

Q5: 发现了什么实验现象？

实验现象：(1) ReOPD 在准确率上匹配或超过完全在线 OPD，且显著优于离线 SFT；(2) 训练效率：ReOPD 每步训练速度至少比 OPD 快4倍，因为避免了环境交互和教师频繁查询，学生训练过程中零工具调用；(3) 消融趋势：步骤衰减采样策略优于均匀采样或增采样，表明早期前缀（low shift）对训练效果好；(4) 前缀陷阱验证：随着步骤增加，教师目标可靠性下降，验证了分布偏移的存在；(5) 模型规模扩展：ReOPD 在不同规模模型下均有效，大教师模型收益更大；(6) 失败案例/边界条件：可能当任务需要长期规划时，仅靠早期前缀可能不足，需要结合更长时间的前缀（但仍需权衡可靠性）。这些详细信息建议查看原文实验部分。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向：(1) 自适应采样策略：根据学生当前能力动态调整前缀分布，而非固定衰减；(2) 结合强化学习奖励：不仅模仿教师动作，还利用任务奖励进行优化，可能进一步提升性能；(3) 扩展到更复杂的环境和工具集，如多步骤数据库查询、API 调用链等；(4) 教师池更新机制：在离线轨迹池中引入新鲜轨迹以保持数据多样性；(5) 理论分析：为前缀陷阱和可靠性权衡提供更严格的理论界；(6) 跨领域应用：应用于生物实验、化学合成等科学智能体任务。

Q7: 总结一下论文的主要内容

论文研究多轮在线蒸馏（OPD）在智能体任务中的高效实现。完全在线的 OPD 虽然效果好，但需要大量环境交互，成本高。作者发现多轮 OPD 存在前缀陷阱：使学生历史更 on-policy 虽然能提高相关性，但可能降低教师在这些历史上的目标可靠性。为解决这一问题，提出 ReOPD（Replayed-Prefix On-Policy Distillation），一种无需环境交互的离线蒸馏方法。ReOPD 利用预先收集的教师轨迹作为重放前缀，在选定步骤让学生行动并由教师提供监督。它通过步骤衰减采样策略优先使用早期前缀，从而在保持学生相关性同时确保教师可靠性。实验在数学推理和搜索任务上验证了 ReOPD 的有效性：准确率匹配或超越在线 OPD，训练速度至少快4倍，且无需在训练中调用工具。该方法将昂贵的智能体交互转化为可复用的离线资源，为大规模蒸馏提供了可扩展的范本。主要贡献在于：(1) 提出 ReOPD 算法；(2) 揭示前缀陷阱；(3) 将多轮 OPD 视为可靠性感知的前缀分布设计；(4) 实现高效训练，零环境交互。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心问题是多轮智能体任务中的蒸馏，与用户关注的 agent 方向直接重合（权重0.10）

## 基本信息

- 作者：Baohao Liao, Hanze Dong, Christof Monz, Xinxing Xu, Li Dong, Furu Wei
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, stat.ML
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04763`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 检索到的证据片段（abstract、method、results部分），并结合 heuristic_draft 进行了润色和补全。
