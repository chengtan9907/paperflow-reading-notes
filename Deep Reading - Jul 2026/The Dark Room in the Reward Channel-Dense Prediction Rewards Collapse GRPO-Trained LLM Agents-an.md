---
user_id: "cheng tan"
paper_id: 5458
arxiv_id: "2607.21273"
title: "The Dark Room in the Reward Channel: Dense Prediction Rewards Collapse GRPO-Trained LLM Agents -- and What Actually Works"
publish_date: "2026-07-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.21273.pdf"
pdf_url: "https://arxiv.org/pdf/2607.21273"
abs_url: "https://arxiv.org/abs/2607.21273"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-24T13:12:55"
---
# The Dark Room in the Reward Channel: Dense Prediction Rewards Collapse GRPO-Trained LLM Agents -- and What Actually Works

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：grpo · llm agents · dense reward · prediction reward

## 一句话总结

本文揭示在GRPO（组归一化强化学习）框架下，密集的逐步预测奖励会导致LLM智能体策略发生灾难性崩溃（暗室病理：预测准确率几乎为100%而任务成功率为0），并证明该崩溃仅由GRPO的标准差归一化造成；去除该归一化即可恢复基线级性能；提出基于信号方差轮廓的判据预测哪些密集信号是安全的，并证实辅助损失通道远优于奖励通道。

## 摘要

> Dense per-step supervision is an appealing remedy for sparse-reward, long-horizon LLM agents: reward the agent for predicting its next observation, and memory should follow. We show that under group-normalized RL (GRPO), this recipe does not merely fail -- it destroys the policy. Across Qwen3-1.7B/4B/8B on ALFWorld, a potential-based prediction reward drives every run into a degenerate absorbing state (prediction accuracy -> 1.0, task success -> 0,episode length pinned at the horizon): the "dark room" pathology, built automatically by the optimizer. A single-factor ablation localizes the cause -- removing only GRPO's std normalization turns the same reward from catastrophic (0%) into baseline parity -- and a two-line proposition explains why: in all-fail groups the z-scored advantage is invariant to the shaping coefficient, so bounded rewards become unbounded pressure and annealing cannot help. Our central insight generalizes this: what z-scoring amplifies is a dense signal's within-group variance while all-fail groups dominate, so signals whose variance decays by mastery are structurally amplifier-safe.This variance-profile criterion retrodicts our collapses, carries preregistered predictions for arms that had not yet run, and is consistent with published reward-channel successes (a compatibility check, not an independent test). Finally, a controlled signal-delivery matrix (identical signal, varying only the consumption mechanism) shows the reward channel is at best neutral while the auxiliary-loss channel gains ~20 points -- and a shuffled-gold placebo matches the true-gold arm, so the gap survives without correct labels. Endpoints are single-seed; seed replication and group-size controls are preregistered and in progress.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在GRPO（Group Relative Policy Optimization）训练下，为长视界LLM智能体引入密集逐步预测奖励是否有效？尽管直觉上密集信号应能改善稀疏奖励下的信用分配，但实验发现该方法导致灾难性失败。具体而言，论文试图理解：1) 为什么在GRPO下，一个设计为安全的潜在奖励（potential-based prediction reward）会引发策略完全崩溃（暗室病理）；2) 是什么具体机制导致了这种崩溃（是GRPO的哪个组件造成）；3) 如何预见哪些密集信号在GRPO下是安全的（方差轮廓准则）；4) 同样的信号通过奖励通道与辅助损失通道传递时效果差异如何。该问题的重要性在于：GRPO已成为LLM后训练的主流框架之一（如DeepSeek-R1），密集逐步监督是自然的产品改进方向，因此揭示其交互风险有重大实际影响。

Q2: 有哪些相关研究？

相关研究包括以下几个方面：
- 密集奖励与过程监督：密集过程奖励被广泛用于改善信用分配，但易被奖励黑客利用（Cui et al., 2025b）。进展原则（reward the change in success）可在某些场景缓解问题。本文的预测奖励属于基于势能的塑造，理论上保证最优策略不变。
- GRPO与组归一化RL：Shao et al. (2024) 提出GRPO，在组内计算优势函数的均值和方差并进行z-score归一化。该归一化在稀疏奖励下有效，但与密集信号交互产生意外副作用。
- 辅助损失与多任务学习：UNREAL (Jaderberg et al., 2017) 等工作中，辅助任务损失通过共享表征提升学习。本文对比了同一信号通过奖励通道与辅助损失通道的效果。
- LLM智能体多步推理：Feng et al. (2025) 提出的步独立多轮展开（step-independent multi-turn rollout）用于长程智能体训练，本文实验采用该框架。
- 失败分析与修复：本文属于诊断类工作，类似之前对奖励缩放、归一化技术的分析。核心发现是z-score归一化在密集信号中的放大效应，并提出方差轮廓作为安全性判据。

Q3: 论文如何解决这个问题？

论文通过系统实验和机制分析解决问题：
1. 实验框架：在ALFWorld环境上，使用Qwen3-1.7B/4B/8B模型，采用步独立多轮展开（Feng et al., 2025）。训练使用GRPO，基线是稀疏成功奖励。预测奖励定义为基于势能的观测预测准确率。
2. 发现问题：所有采用预测奖励的GRPO运行均陷入退化状态——预测准确率升至接近100%，但任务成功率为0%，回合长度锁定在最大步数。论文将这种状态命名为暗室病理。
3. 定位原因：通过单因子消融（逐一移除GRPO组件），发现仅去除组标准差归一化（保留均值中心化）即可将0%成功率恢复至51.6%（与基线49.5%相当）。这表明罪魁祸首是标准差归一化。
4. 理论解释：在全失败组中，所有样本获得相同的预测奖励，导致组内方差为零，z-score优势变为无穷大，奖励系数失去调节作用，产生无界更新压力。更一般地，密集信号在掌握后方差消失，使得z-scoring放大饱和状态下的组内方差，而信号本身不再提供区分度。
5. 方差轮廓准则：提出一个判据——如果信号在被掌握后方差趋于零，则该信号在GRPO下是不安全的；反之如果信号即使在完全掌握后仍存在组内方差，则可能安全。该准则回溯预测了预测奖励的崩溃，并对其他信号类型做出预注册预测。
6. 信号传递对比：在控制信号相同条件下，通过奖励通道和辅助损失通道传递同一预测信号。结果显示辅助损失通道提升约20个百分点，而奖励通道至多中性。混淆黄金匹配真实黄金，说明正确标签并非必需，进一步支持信号内容本身不是崩溃原因。

Q4: 论文做了哪些实验？

论文在ALFWorld环境上设计了一系列实验：
1. 标准训练实验：使用Qwen3-1.7B/4B/8B模型，在ALFWorld的6个任务类型上，GRPO训练。比较条件：稀疏成功奖励（基线）vs 稀疏+预测奖励（势能）。结果所有预测奖励运行均崩溃。
2. 单因子消融实验：逐一更改GRPO组件：移除均值中心化、移除标准差归一化、同时移除两者、修改系数等。发现仅移除标准差归一化（组内仅中心化）即可挽救训练，成功率回升至基线水平（51.6% vs 49.5%），且预测准确率不再饱和。
3. 方差轮廓准则验证：对不同类型信号（预测信号、随机信号、恒定信号等）进行预注册预测，并运行实验验证准则的预测能力。
4. 信号传递通道对比：固定预测信号内容，通过(a)奖励通道（GRPO优势）和(b)辅助损失通道（独立交叉熵损失）传递。结果显示辅助损失通道相比基线提升约20点，奖励通道表现接近基线或更差。
5. 混淆黄金实验：使用随机标签代替真实观测标签作为预测目标，结果与真实标签组表现相同，说明信号具体内容不重要，崩溃源于结构。
6. 种子复制和组大小控制：目前完成单种子实验，种子复制和组大小控制实验已预注册并正在进行中。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：
- 主要现象（暗室病理）：所有使用预测奖励的GRPO运行都导致策略崩溃。预测准确率近乎完美（~1.0），任务成功率为0%，回合长度卡在最大步数。模型学会了忽略环境，输出恒定预测，从而获得高预测准确率，但无法完成任务。
- 反直觉发现：一个在单步时间差分学习中安全的势能奖励，在GRPO下变得灾难性。这表明组归一化改变了信号行为。
- 消融关键：移除GRPO的标准差归一化是唯一需要的改变。仅此一项使性能从0%跳到51.6%，预测准确率保持在0.49-0.64，未饱和。
- 方差与饱和：预测准确率模型快速达到高准确率，随后在饱和后停滞，失去了对探索的激励。其他信号（如随机信号）未发生崩溃。
- 信号通道不对称：同一信号通过辅助损失传递有效，通过奖励传递无效或有害。辅助损失通道提升约20个点，奖励通道至多中性。
- 混淆黄金结果：用乱序标签替代真实黄金标签，在辅助损失通道中依然达到类似性能，说明信号内容不关键，重要的是信号存在性和通道特性。
- 预注册预测验证：方差轮廓准则预测的信号安全性排序与未运行条件的结果一致（需等待完整实验）。
- 组大小效应：组大小可能影响崩溃严重程度（实验中组大小固定，组大小控制已预注册）。

Q6: 有什么可以进一步探索的点？

基于论文发现，以下方向值得进一步探索：
1. 多种子验证与统计可靠性：当前所有结论基于单种子运行，种子复制是最高优先级后续工作，以确认现象一般性和结果稳健性。
2. 组大小的影响：组大小可能影响z-score方差估计噪声，进而影响崩溃严重程度或起始点，需系统变化组大小。
3. 其他密集信号类型：除了预测奖励，还有很多其他形式密集信号（过程奖励模型、进度奖励、shaping奖励），方差轮廓准则提供了一个分类工具，但需在更多任务和信号上验证。
4. 其他RL算法：本文聚焦GRPO，但类似组归一化思想出现在其他算法中，需检查这些算法是否也有类似脆弱性。
5. 理论严格性：论文提供了直观机制解释，但更严格的理论分析（收敛证明、边界条件）仍缺失。
6. 实际部署修复：既然定位原因，可设计修正版GRPO，如自适应标准差截断、进行组归一化时考虑密度奖励的方差特性。
7. 辅助损失与奖励混合利用：论文表明辅助损失通道更优，但最佳实践可能结合两者，需探索如何在不引入崩溃风险情况下利用密集信号。
8. 扩展到更多LLM和任务：当前仅使用Qwen3系列和ALFWorld，在其他模型（如Llama）和任务（如WebArena）中检查普遍性。
9. 与奖励黑客其他模式对比：暗室病理类似于奖励黑客一种形式但具特定机制，将其与梯度黑客、exploitation等联系起来。

Q7: 总结一下论文的主要内容

这篇论文系统研究了在GRPO（Group Relative Policy Optimization）框架下，为长时域LLM智能体引入密集逐步预测奖励（potential-based prediction reward）所导致的灾难性失败问题，并将其命名为暗室病理。

论证主线：密集逐步监督是解决长时域稀疏奖励的自然思路：每一时刻奖励智能体对其下一观测的预测正确性。由于使用势能塑造，理论上该奖励保持最优策略不变。然而论文发现，在GRPO中，这一配方不仅无效，反而完全摧毁策略。通过精心设计的实验和消融，作者最终定位到罪魁祸首是GRPO优势计算中的标准差归一化（z-scoring）。该归一化在所有样本都失败的组中，将原本有界的预测奖励转化为无界的更新压力，迫使策略陷入一个所有步都预测正确但完全不执行有效动作的退化状态。

技术主线：
1. 问题发现：在ALFWorld任务上，使用Qwen3-1.7B/4B/8B，预测奖励组成GRPO训练，每种子均收敛到100%预测准确率和0%任务成功率。
2. 根因定位：通过逐一关闭GRPO组件（均值中心化、标准差归一化、两者同时），发现仅移除标准差归一化就将性能从0%恢复至51.6%（基线49.5%）。
3. 理论理解：在全失败组中，所有样本的预测奖励相同，组内方差为零，导致z-score优势变为无穷大（除以0）。更一般地，任何密集信号如果在其被完全掌握后组内方差消失（如预测准确率掌握后达到100%），则会在GRPO的z-scoring下遭受无界放大，引发崩溃。论文据此提出方差轮廓准则：安全的密集信号应具有在掌握后仍保持组内方差的特性。
4. 准则验证：该准则回溯解释了预测奖励的崩溃，并对尚未运行的信号组合做出预注册预测（如随机信号安全、恒定信号危险等）。
5. 信号渠道对比：在控制信号内容相同下，比较通过奖励通道（GRPO优势）和辅助损失通道（独立交叉熵）的效果。辅助损失通道带来约20点的提升（相对于基线），而奖励通道至多中性甚至有害。混淆黄金实验（使用随机标签）表明，信号内容本身不重要，崩溃源于通道特性。

实验主线：
- 环境：ALFWorld（6类任务，30-50步回合）。
- 模型：Qwen3-1.7B/4B/8B，步独立多轮展开框架。
- 对照：稀疏成功奖励（基线）、稀疏+预测奖励（势能）。
- 消融：GRPO组件的逐一移除。
- 准则验证：多种信号类型（预测、随机、常数等）在GRPO下的表现。
- 通道对比：奖励 vs 辅助损失，混淆黄金。
- 当前状态：单种子完成；种子复制和组大小控制已预注册并进行中。

核心贡献包括：揭示并命名GRPO中密集预测奖励的暗室病理；精确定位问题源于GRPO标准差归一化；提出方差轮廓准则；通过控制实验证明奖励通道本身次优而辅助损失通道有效。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与LLM智能体训练方向直接相关：论文研究GRPO下密集信号效果，这是当前LLM后训练（如DeepSeek-R1）的核心议题。

## 基本信息

- 作者：Yu Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-07-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.21273`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读主要基于论文摘要、检索证据片段（OpenAI Qwen3-Embedding-8B检索）以及用户提供的启发式草稿生成。由于未完整阅读全文，部分细节可能不精确；推荐直接访问原文核对具体数值和实验设计。
