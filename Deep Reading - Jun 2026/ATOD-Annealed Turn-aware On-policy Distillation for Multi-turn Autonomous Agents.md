---
user_id: "cheng tan"
paper_id: 512
arxiv_id: "2606.27814"
title: "ATOD: Annealed Turn-aware On-policy Distillation for Multi-turn Autonomous Agents"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27814.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27814"
abs_url: "https://arxiv.org/abs/2606.27814"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:11:32"
---
# ATOD: Annealed Turn-aware On-policy Distillation for Multi-turn Autonomous Agents

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：autonomous agents · on-policy distillation · reinforcement learning · multi-turn interaction

## 一句话总结

ATOD 是一种结合了退火策略的在线蒸馏与强化学习算法，通过回合级不确定性重加权（T-DUR）解决了长程智能体训练中的冷启动效率与教师性能天花板问题。

## 摘要

> 2026 Training small language-model agents for long-horizon interactive tasks requires both fast imita-
> tion and reward-driven improvement. On-policy distillation (OPD) provides dense teacher guidance
> and typically improves rapidly in the early stage, but its gains saturate once the student approachesJun
> the teacher, limiting the final performance ceiling. Reinforcement learning (RL) directly optimizes
> 26 environment rewards and encourages exploratory improvement toward a higher reward-defined ceil- ing, but sparse and delayed feedback makes early-stage learning much less efficient than OPD. In this
> paper, we propose ATOD (Annealed Turn-aware On-policy Distillation), a hybrid online distillation
> algorithm that explicitly exploits this complementarity. (1) ATOD uses an annealed OPD–RL
> schedule: OPD dominates early training to approach teacher-level behavior, while RL is gradually
> strengthened to drive reward-based exploration. (2) ATOD introduces Turn-level Disagreement-[cs.AI] Uncertainty Reweighting (T-DUR), which softly amplifies high-utility turns and improves dense
> supervision in long trajectories. Experiments on ALFWorld, WebShop, and Search-QA show that
> ATOD consistently outperforms competing post-training baselines: across the three student sizes,
> ATOD improves average success rate by 3.03 points over OPD and 23.62 points over GRPO, while
> surpassing the corresponding teacher models by 2.16 points.

Q1: 这篇论文试图解决什么问题？

### 核心科学问题
本研究聚焦于如何高效地训练小型语言模型（SLM）使其具备处理长程交互任务（Long-horizon interactive tasks）的能力。这类任务要求智能体在多轮对话中观察状态、选择动作、调用工具并修正计划。传统的训练路径面临以下三大核心矛盾：

1. **在线蒸馏（OPD）的“教师天花板”效应**：
 OPD 通过提供密集的 Token 级教师指导，能让学生模型在训练初期迅速提升。然而，一旦学生模型的分布接近教师，其性能增益就会进入平台期（Saturate）。由于学生只是在模仿教师，它很难纠正教师固有的偏差或在教师未覆盖的区域进行创新，导致最终性能受限于教师水平。

2. **强化学习（RL）的“冷启动”与稀疏奖励难题**：
 以 GRPO 为代表的 RL 方法直接优化环境奖励，理论上具有更高的性能上限。但在长程任务中，环境反馈往往是稀疏且延迟的（例如只有在完成整个购物流程后才有奖励）。这导致训练初期模型在海量动作空间中盲目探索，学习效率极低，难以在有限的计算资源内收敛。

3. **多回合轨迹中的“信号稀释”问题**：
 在长达数十个回合的交互中，并非所有步骤都具有同等的学习价值。一些回合属于常规操作（Routine），而另一些则是决定成败的关键决策点（Critical turns）。传统的 OPD 对所有回合进行均匀的 Token 级监督，这会导致关键信号被大量平庸的模仿样本所稀释，浪费了宝贵的训练步数。

### 技术权衡与隐含假设
论文假设教师模型虽然强大但并非完美，且学生模型通过适当的引导可以利用环境反馈发现比教师更优的策略。技术上，ATOD 试图在“模仿的稳定性”与“探索的灵活性”之间寻找动态平衡点。

Q2: 有哪些相关研究？

### 智能体蒸馏与模仿学习
早期的研究如 SOD（Standard On-policy Distillation）和 TCOD（Turn-level Contextualized On-policy Distillation）探索了如何将大型模型的智能转移到小模型。TCOD 引入了回合级的上下文感知，但它们本质上仍属于纯模仿范式，无法突破教师的性能上限。ATOD 通过引入 RL 信号，打破了这一限制。

### 强化学习优化算法
GRPO（Group Relative Policy Optimization）是近年来受到关注的 RL 算法，它通过组内相对优势估计来取消单独的价值模型（Value Model），降低了显存开销。然而，GRPO 在处理复杂智能体任务时表现出明显的冷启动困难。ATOD 将 GRPO 作为其 RL 组成部分，但通过 OPD 提供了必要的引导。

### 混合学习范式
SDAR（Self-distillation Augmented RL）等方法尝试结合自蒸馏与 RL，但它们通常依赖于学生模型自身的历史版本作为参考，缺乏强有力的外部教师引导。ATOD 的不同之处在于它显式地利用了强力教师的密集信号，并通过退火机制管理两种信号的竞争与协作。此外，ATOD 针对多回合任务设计的 T-DUR 机制是其区别于通用混合学习算法的关键特征。

Q3: 论文如何解决这个问题？

### 1. 混合优势函数（Hybrid Advantage Construction）
ATOD 将两种互补的信号整合进一个统一的 Token 级优势函数中：
$A_t = \kappa(s)A_{OPD,t} + \rho(s)A_{GRPO,t}$
其中，$A_{OPD,t}$ 代表教师与学生在 Token 级别的对齐程度（通常基于 log 概率差），而 $A_{GRPO,t}$ 则是基于环境奖励的组内相对优势。这种设计允许模型在单次更新中同时吸收教师的经验和环境的反馈。

### 2. 动态系数退火调度（Dynamic Coefficient Annealing）
为了解决不同阶段的学习重心问题，ATOD 引入了随训练步数 $s$ 变化的系数：
- **早期阶段**：$\kappa(s)$ 较大，$\rho(s)$ 较小。此时模型主要进行“行为引导”，学习如何生成符合逻辑的动作和遵循交互协议。
- **后期阶段**：$\kappa(s)$ 逐渐减小，$\rho(s)$ 逐渐增大。此时模型进入“自主探索”阶段，利用环境奖励来微调策略，纠正教师的错误并寻找更高效的路径。

### 3. 回合级分歧-不确定性重加权（T-DUR）
这是 ATOD 的核心创新点，用于对 OPD 项进行细粒度调整。T-DUR 根据两个指标计算每个回合的权重 $w_{k(t)}$：
- **分歧度（Divergence）**：衡量学生动作分布与教师分布的差异。高分歧意味着学生尚未掌握教师的逻辑。
- **不确定性（Entropy）**：衡量学生对当前动作的信心。高熵意味着学生处于困惑状态。

**权重逻辑**：
- **关键回合（Critical）**：高分歧且高不确定性。学生完全不会，教师指导最重要，赋予最高权重。
- **自信错误（Confidently Wrong）**：高分歧但低不确定性。学生自以为对但其实错了，需要强力纠偏，赋予高权重。
- **不确定错过（Unsure Misses）**：低分歧但高不确定性。学生虽然碰巧对了但没把握，需要巩固，赋予中等权重。
- **常规回合（Routine）**：低分歧且低不确定性。学生已掌握，跳过或降低权重以节省计算资源。

Q4: 论文做了哪些实验？

### 实验设置
- **基准数据集**：
 1. **ALFWorld**：具身指令遵循任务，要求在文本环境中完成家务。测试长程规划能力。
 2. **WebShop**：网页导航与购物任务，涉及复杂的属性过滤和搜索。测试工具调用能力。
 3. **Search-QA**：涵盖 HotpotQA、TriviaQA 等 7 个子集的搜索增强问答。测试多跳推理与信息整合能力。
- **模型配置**：
 - **学生模型**：Qwen3-0.6B、1.7B、4B。
 - **教师模型**：针对 0.6B/1.7B 学生使用 Qwen3-4B (GRPO 训练版)；针对 4B 学生使用 Qwen3-30B-A3B。
- **对比基准**：Vanilla（原始模型）、GRPO（纯 RL）、OPD（纯蒸馏）、SDAR（自蒸馏+RL）、SOD、TCOD（先进蒸馏变体）。

### 训练协议
所有模型在相同的 150 步训练窗口内进行评估，使用组大小 $G$ 进行采样以计算 GRPO 优势。评估指标为最大验证成功率（Success Rate）和平均轨迹长度。

Q5: 发现了什么实验现象？

### 1. 性能突破与“超越教师”
ATOD 在所有三个基准上均取得了最优结果。最显著的发现是，ATOD 训练出的学生模型在 ALFWorld 和 WebShop 上**全面超越了它们的教师模型**。例如，Qwen3-4B 学生在 ALFWorld 上的成功率达到了 80.47%，而其 30B 教师仅为 80.47%（持平），但在 WebShop 上学生显著优于教师。这证明了 RL 信号确实帮助学生突破了模仿的局限。

### 2. 训练动力学分析
实验曲线显示，ATOD 成功结合了 OPD 的“快”和 GRPO 的“高”。在训练的前 40 步，ATOD 的增长曲线与 OPD 几乎重合，远超 GRPO 的缓慢起步；而在 80 步之后，当 OPD 进入平台期时，ATOD 凭借 RL 信号继续攀升，最终达到了 GRPO 都未能触及的高度。

### 3. T-DUR 的消融效应
消融实验表明，移除 T-DUR 会导致性能下降约 1.5-2 个百分点。通过可视化权重分布发现，T-DUR 确实将更多的注意力集中在了那些涉及复杂逻辑转换的回合上，而自动忽略了简单的观察描述或重复性动作。

### 4. 轨迹效率
在 Search-QA 任务中，ATOD 不仅提高了准确率，还缩短了平均轨迹长度。这意味着模型学会了更高效地使用搜索工具，减少了冗余的搜索步骤，这通常是纯 OPD 难以实现的（因为教师可能也存在冗余）。

### 5. 负结果与失败模式
在极少数情况下，如果退火速度过快（即过早切断教师信号），模型会出现性能回撤，这表明在复杂任务中，即使在后期，教师的“锚点”作用依然对维持策略稳定性有一定意义。

Q6: 有什么可以进一步探索的点？

### 1. 自适应退火策略
目前 ATOD 使用预设的线性或余弦退火调度。未来的研究可以探索基于学生模型实时表现（如验证集成功率或教师-学生 KL 散度的变化率）的自适应退火机制，以进一步优化训练效率。

### 2. 多教师协同蒸馏
考虑到不同教师模型在不同领域（如代码、逻辑、常识）各有长短，可以研究如何将 ATOD 扩展到多教师场景，利用 T-DUR 动态选择在特定回合表现最优秀的教师进行指导。

### 3. 跨模态智能体扩展
目前的实验集中在文本交互。将 ATOD 应用于视觉语言模型（VLM）驱动的具身智能体是一个极具前景的方向，因为视觉环境中的奖励更加稀疏，且动作空间更连续，对高效引导的需求更迫切。

### 4. 离线与在线的进一步融合
探索如何利用离线轨迹数据（Offline trajectories）为 ATOD 提供更好的初始分布，从而进一步缩短在线采样的耗时。

Q7: 总结一下论文的主要内容

本文提出了 ATOD 算法，旨在解决小型语言模型智能体在长程交互任务中训练效率低且性能受限的问题。研究者敏锐地观察到，在线蒸馏（OPD）虽能提供快速的早期引导，但存在“教师天花板”；而强化学习（RL）虽能突破上限，却面临严重的冷启动和稀疏奖励挑战。ATOD 通过一种创新的“先模仿、后超越”的混合架构解决了这一矛盾。其核心贡献在于：1) 提出了一个统一的混合优势函数，将 Token 级的教师指导与环境奖励相结合；2) 设计了动态退火调度，实现了从模仿到探索的平滑过渡；3) 引入了 T-DUR 机制，利用分歧度和不确定性对多回合任务中的关键决策点进行重加权，显著提升了监督信号的有效性。实验在具身智能、网页导航和复杂问答三大领域展开，结果表明 ATOD 不仅在性能上大幅领先于现有的蒸馏和 RL 基准，更证明了通过合理的训练策略，小参数量模型完全有潜力在特定任务中超越其大模型教师。这一发现为在资源受限环境下部署高性能、高效率的智能体提供了重要的理论支持和实践路径。

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：该研究直接关联智能体（Agent）和生成（Generation）方向，特别是如何优化小模型的决策能力。

## 基本信息

- 作者：Qitai Tan, Zefang Zong, Yang Li, Peng Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-29
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2606.27814`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了方法论中的混合优势函数公式、退火调度逻辑以及实验部分的对比数据和消融结果。
