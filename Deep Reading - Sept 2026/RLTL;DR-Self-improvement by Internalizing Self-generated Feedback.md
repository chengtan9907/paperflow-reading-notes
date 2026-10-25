---
user_id: "cheng tan"
paper_id: 14332
arxiv_id: "2609.37633v1"
title: "RLTL;DR: Self-improvement by Internalizing Self-generated Feedback"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.37633v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37633v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-07T01:28:58"
---
# RLTL;DR: Self-improvement by Internalizing Self-generated Feedback

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：reinforcement learning with verifiable rewards · self-improvement · insight internalization · sequential rollout

## 一句话总结

本文提出 RLTL;DR：在强化学习训练中，当智能体任务失败时，让策略自己把验证器输出总结成一条 TL;DR 级 insight，并把这些 insight 依次放入后续 rollouts 的上下文以改善探索；同时额外对 (task, insight) 元组做监督微调，把 insight 直接内化进参数，从而在 Pass@128=0 的极难智能体与代码任务上打破 GRPO 的训练停滞，使 eval 时无需 insight 也能将 Pass@1 从 0–1% 提升到 12–13%。

## 摘要

> The common paradigm of reinforcement learning with verifiable rewards (RLVR) is to let agents make multiple attempts at a task, and optimize towards the successful ones. This becomes problematic in the realms of self-improvement, where tasks are so difficult that the agent has a low or even no chance of success, and where there are no teacher models or example solutions to distill from. In this paper, we introduce RLTL;DR. After each failed attempt, we show the policy the verifier outputs and let it write its own feedback, in the form of a single TL;DR insight. The next rollout is conditioned on all previous insights, and we sequentially sample rollouts until a solution is found. Moreover, we enable backpropagation on the in-context insights to internalize a direct task to insight mapping. On challenging tool-calling and coding datasets (filtered to Pass@128=0), standard GRPO training of a Qwen 3.5 9B Thinking policy stays flat at a Pass@1 of 0% to 1%. RLTL;DR breaks through this learning barrier, achieving a Pass@1 of 14-31% with insights in context during training and, crucially, 12-13% when no insight is in context at eval time. We identify that the key is the task to insight internalization. To study this further, we reduce our approach to SFTL;DR, training only on (task, insight) tuples, without showing or backpropagating on any rollouts. Training on only 4k of these tuples recovers almost the full performance of RLTL;DR and classical SFT on full rollouts. This demonstrates a promising compacted training paradigm of the form "on this sort of task, keep this sort of thing in mind", which we hope to inspire future research on.

Q1: 这篇论文试图解决什么问题？

本文要解决的核心问题是 RLVR（Lambert et al., 2024）在自改进场景中的“学习信号饥饿”问题。具体拆解如下：

1. 硬任务导致零正样本：在 agentic coding 等前沿任务中，策略本身可能对任务完全无能为力。作者用 Pass@128=0 来操作化“极难任务”——128 次并行 rollout 全部失败，GRPO 中成功轨迹奖励为 0，梯度没有可被放大的方向，训练曲线保持平坦（实验显示 Pass@1 长期停在 0–1%）。
2. 自改进没有外部教师：经典 RLVR 可以从教师模型或示例解蒸馏，但前沿场景里策略就是最强的模型，没有更强的 teacher、没有 golden solution。因此学习信号只能来自策略自身的尝试、验证器的反馈以及从失败中提炼的规律。
3. 并行独立 rollout 的探索效率不足：标准的 RLVR 让多个 rollout 平行且独立采样，失败尝试之间互不借鉴；错误模式无法在采样过程中被逐步纠正。论文认为需要把独立并行改为顺序条件化采样，让后续尝试能看到前序失败和验证器错误。
4. 验证器反馈没有被有效利用：简单的 SGE（self-generated experience / 基于先前尝试做条件生成）虽然能看到失败历史，但只是把失败尝试再喂回去，没有显式反思单元测试错误，反而可能误导后续 rollout（文中引用了 Cheng et al. (2026) 的类似观察）。
5. 推理时 insight 不可依赖：如果把 TL;DR 放在上下文中训练，eval 时也可能需要生成 insight 或依赖上下文；论文的目标是让 insight 只在训练时起“脚手架”作用，最终把 task→insight 的规律写进参数，使 eval 时即使不给 insight 也能隐式应用这些规律。
6. 更广的训练范式问题：经典 SFT 要在完整解决方案上学习，而极难任务根本没有成功解可供标注；论文追问：是否可以不学完整 rollout，只学压缩后的 (task, insight) 元组？这对应“压缩训练范式”（compacted training paradigm）的问题。

隐含假设（需要读者注意）：用 Pass@128=0 筛选任务意味着训练集只包含当前策略完全无法解决的任务；Pass@1 的绝对提升（12–13%）虽然显著，但绝对水平仍然较低，说明突破的是“从完全学不动到开始能学”的临界点，而不是“学到很高性能”。此外，作者引入 insight 作为人类可读的中介表征，隐含假设是这种高层反思比直接条件化原始错误信息更有利于学习和泛化。

Q2: 有哪些相关研究？

论文的关系网络大致可梳理为以下几支：

1. RLVR 与 GRPO 家族：Lambert et al. (2024) 提出 verifiable rewards 范式；Shao et al. (2024)、Liu et al. (2025b)、Yu et al. (2026) 发展了 GRPO 及其变体，用组内相对奖励替代 critic。本文的 baseline 和训练框架都建立在这条线上，指出其在 Pass@128=0 时失效。
2. 自改进 / self-improvement：这是论文定位的核心场景，特点是策略处于能力前沿、无教师无 golden solution。相关工作包括从自身采样中学习、self-training、拒绝采样蒸馏等；本文强调这些方法在“完全没有成功样本”时无效。
3. 基于验证器反馈的反思与纠错：SGE 类方法把先前失败尝试放进上下文再采样，论文实验显示它停留于 0–1% Pass@1，因为它看到失败历史却未反思单元测试，反而误导后续采样（复现了 Cheng et al. (2026) 的结论）。这形成“条件化尝试 vs. 条件化反思”的对比。
4. 与 insight/反思蒸馏平行的近期工作：Discussion 7.1 节列举三支相近现象——ECHO（Shrivastava et al., 2026）在 agentic terminal 交互中反向传播 terminal 输出从而提升关于 terminal 的通用知识，且这些 token 位于 user message 而非 agent message，与本文“只内化不显式生成”一致；Lu et al. (2026a) 在文本 embodied agent 和搜索任务上有类似发现；Cook et al. (2026) 先训模型读代码文档、再测试写代码，发现知识在格式间迁移，类似 task→insight 的反传跳跃。
5. 关于隐式/内化知识的工作：Discussion 7.1 提到 Nakkiran et al. (2026)，用于解释相似任务和输出格式如何通过海量相似任务训练与语义平滑被参数化更新——这为 insight 内化提供了理论类比。
6. 上下文学习与 insight 条件化：论文还涉及多轮 insight 顺序累积、最新 insight 与全部 insight 条件化的对比（检索证据显示“sequential insight rises faster and higher than just using the latest insight”），这关联 in-context learning、chain-of-thought 反思、以及 memory/scratchpad 类方法。
7. SFT baseline：经典 SFT 需要在完整 rollout（含成功或失败轨迹）上训练，本文的 SFTL;DR 与之对比，展示“只学 4k 条 (task, insight) 元组超过完整 rollout SFT”的压缩训练优势。

注意：由于论文为 arXiv 预印本且部分引用为 2026 年文献，无法从摘要和语义检索片段确认所有相关工作细节；以上第 4 条内容来自 Discussion 节证据，表述为论文自述的“相似现象”更稳妥。相关工作的完整对比（如是否与 RLTF-SD 的内部机制一致）需要阅读原文相关章节确认（合理推断：RLTF-SD 为某 reflection/self-distillation baseline，但原文在检索片段中未给出全称）。

Q3: 论文如何解决这个问题？

论文的技术方案由两个互补机制组成：

1. 顺序 rollout 与自生成 TL;DR insight（改进探索）：
 - 每轮尝试后运行验证器代码检查成功与否；失败时，把“前一次尝试 + 验证器错误输出”交给策略本身，让它产出一条高层、单句的 TL;DR insight，例如“记得对搜索结果分页”“用 cash 而不是 checking 作为账户类型”“需要真正阅读或滚动文章内容才能产生阅读历史记录”“用正确的字母序排序字段而不是标题”等（检索证据中出现的正是这类 agentic 任务反思）。
 - 下一条 rollout 以“任务 + 之前所有 TL;DR insight”为条件进行采样；顺序累积 insight，直到某个 rollout 通过验证器找到解。
 - 消融证据显示：顺序累积多个 insight 比只用最新一条 insight 更好；收益主要集中在前 5 条 insight；多个 hint 条件的边际收益随后递减（来自 method 相关检索片段）。
 - 探索从“并行 i.i.d. rollout”转为“顺序条件化采样”，让失败经验在采样过程中被逐步消化。

2. 对 insight 进行反向传播实现内化（改进学习信号）：
 - 标准 RLVR 只对成功 rollout 反传，Pass@128=0 时无信号可用；本文允许对上下文中的 insight token 做反传，学习“给定任务直接预测 insight”的映射（task→insight internalization）。
 - 训练目标是 GRPO 与 insight 上的 SFT 损失的混合（消融中记作 LSFT，默认混合权重 λ=0.5）。作者特意验证：自我批判能力（self-critique）不如直接从 instruction 预测 insight 有效（Introduction 检索证据，Section F.1）；LSFT 被定位为内化手段而非显式生成手段。
 - 在 eval 时不给 insight，检验 insight 是否被隐式写进参数。

3. 极简化版本 SFTL;DR：
 - 不做 rollout 展示、不对 rollout 反传，只保留 (task, insight) 元组上的 SFT 损失，形式化为 πθ(f1,...,fI | g)（给定任务 g 生成 insight 序列 f1...fI）。
 - 只用 4k 条这样的元组训练即可恢复 RLTL;DR 几乎全部性能。

4. 训练数据与评测的细节设计：
 - 基准策略：Qwen 3.5 9B Thinking（Qwen Team, 2026）。
 - 数据集：SAPI、Appworld、Leetcode，筛选出 Pass@128=0 的任务子集作为训练集，确保训练时策略从未见过自己成功的样本。
 - 训练曲线按环境交互步数报告；SAPI 上每个任务约在 170k 环境交互后被看到一次，且报告性能是在对给定任务反传之前，因此 SAPI 在约 170k 之前的训练曲线可当作滚动 eval 曲线；Appworld 每 13k 步循环一次，Leetcode 每 9k 步循环一次，泛化判断依赖 heldout split。

5. 消融体系：
 - RLTL;DR 的四个消融 run 显示 LSFT 是驱动因素：λ 从 0.5 降到 0.01 或 0 会严重损害 train 和 eval 性能；λ 超过 0.5 不再提升。
 - 对照 SGE（只用先前失败尝试做条件，不反思验证器输出）。
 - 对照经典 SFT on full rollouts（rollout 含 insight 在上下文的样本，但不对 insight 特殊处理）。
 - 消融 insight 内容粒度：用完整详细反馈替代单句 TL;DR 的变体（结果字段检索证据）。

需要读者注意的边界：检索证据中 References 片段混入了具体任务反思文本，这说明部分 insight 示例来自数据/参考文献区域，不能当作方法的形式化定义；方法的形式化描述（如 GRPO+LSFT 的确切损失函数）在原文 Method 部分，本材料中 method 节为空，故损失函数细节需回原文确认（推测为标准 GRPO 项加 insight-token 交叉熵项）。

Q4: 论文做了哪些实验？

论文在三个 benchmark 上、以 Qwen 3.5 9B Thinking 为策略模型，系统比较了多种训练方法。实验设计要点如下：

1. 任务与数据筛选：
 - SAPI、Appworld、Leetcode 三个数据集；训练任务筛选到 Pass@128=0，即当前策略 128 次尝试全部失败，确保“无正样本、无教师、无示例解”的硬条件。
 - 训练中任务循环频率不同：SAPI 约 170k 环境交互后每个任务出现一次；Appworld 每 13k 步循环；Leetcode 每 9k 步循环。

2. 主要对比方法：
 - GRPO：标准组相对策略优化，作为 RLVR 基线；
 - SGE：以前序失败尝试做条件生成的自我经验基线（注意其条件中不包含对单元测试的反思）；
 - RLTF-SD：另一个能产生反思/自蒸馏信号的基线（全称在材料中未给出，需查原文）；
 - RLTL;DR：本文方法（顺序 insight 条件化 + insight 反传 + LSFT）；
 - SFTL;DR：仅用 (task, insight) 元组做 SFT，不使用也看不到 rollout；
 - SFT on full rollouts：经典 SFT，在 RLTL;DR 第一次运行收集的 rollout 上训练（多数 rollout 有 insight 在上下文中，但训练时不针对 insight token 特殊加权）。

3. 评测口径：
 - 主指标为 Pass@1：Figure 2 报告训练全程中、上下文不含 insight 的 rollout 的 Pass@1，这样 insight 只间接改善之前 batch 的学习信号，可与基线直接比较。
 - SAPI 在 170k 前可用滚动 eval 近似；Appworld 与 Leetcode 以 heldout split 判断泛化。
 - Table 1 汇总训练后的 heldout eval Pass@1：SAPI 上 GRPO 57.8%、SGE 57.7%、RLTF-SD 53.0%、RLTL;DR 91.1%；Appworld 上分别为 33.7%、33.5%、38.9%、61.7%；Leetcode 上分别为 55.1%、56.4%、42.2%、49.1%。注意这里 SAPI 与 Appworld 上 RLTL;DR 明显领先，但 Leetcode 上 RLTL;DR 低于 GRPO 与 SGE。

4. 消融实验：
 - LSFT 权重：λ=0.5 对比 λ=0.01、λ=0，前四个消融 run 显示 LSFT 是驱动因素；λ>0.5 无进一步提升。
 - insight 顺序条件化：全部历史 insight 条件 vs. 仅最新 insight 条件；顺序累积提升更快更高。
 - 累积条数：收益主要来自前 5 条；多个 hint 的边际收益递减。
 - 自我批判 vs. 直接预测：让策略基于尝试做自我批判，弱于直接从 instruction 预测 insight（Section F.1）。
 - insight 内容粒度：单句 TL;DR vs. 完整详细反馈（结果字段检索证据）。

5. SFTL;DR 数据规模实验：用约 4k 条 (task, insight) 元组训练，结果几乎恢复 RLTL;DR 全部性能，并超过完整 rollout 上的经典 SFT。

6. 论文额外承认的失败模式：Leetcode 上 RLTL;DR 和 RLTF-SD 在训练中能发现解（GRPO 不能），但过程出现过拟合，最终 heldout 性能不如 GRPO（Section 7 讨论）。

注意：Table 1 的 SAPI 数值（91.1%）与摘要中“eval 无 insight 12–13%”之间存在明显口径差异，合理推断是 Table 1 的 heldout eval 在训练完成且可能包含 insight 条件或不同采样设置下测得，而 12–13% 特指“训练中、无 insight 上下文、按环境交互逐步报告”的 Pass@1；这一矛盾点需要在原文 Results 完整段落中核对（不应简单混用两组数字）。

Q5: 发现了什么实验现象？

实验现象与反直觉发现可归纳如下：

1. GRPO 确实在 Pass@128=0 任务上完全停滞：Pass@1 长期保持 0%–1%，印证“无成功 rollout 即无学习信号”的机制诊断。
2. SGE 同样停滞（0%–1%）：尽管它条件化前序失败尝试、且明确标注这些尝试失败，但不反思单元测试错误会误导后续 rollout。这个现象复现了 Cheng et al. (2026) 的观察，说明“看到失败历史”与“消化失败原因”之间有本质区别。
3. RLTL;DR 打破学习屏障：训练中 insight 在上下文时 Pass@1 达 14–31%；关键的是，eval 时上下文不含 insight 仍有 12–13%，说明提升不是简单依赖上下文提示，而是 insight 被内化进了参数。
4. 内化具有任务泛化性：SAPI 上 170k 步之前的曲线（每个任务只被看到一次、且评测在反传之前）已经上升，说明 task→insight 的映射能泛化到新任务；但 Leetcode 上没有观察到这种泛化，反而出现过拟合。
5. 最反直觉的结果——只学 (task, insight) 元组就够了：SFTL;DR 不显示 rollout、不对 rollout 反传，只用 4k 条 (task, insight) 元组训练，几乎恢复 RLTL;DR 全部性能，且优于经典 SFT on full rollouts。insight token 在 eval 时从不被生成，但训练它们依然能把高层结论写进参数并隐式应用。作者将之称为 “secret sauce”，并用近期工作（ECHO、Lu et al. (2026a)、Cook et al. (2026)）佐证“user 消息中的 token 也能被内化”这一现象。
6. 消融中的梯度趋势：LSFT 混合权重 λ 是决定性因素——λ 从 0.5 降到 0.01 或 0 严重损害训练和 eval 性能；λ>0.5 无额外收益。前四个消融 run 一致指向 insight SFT 是驱动因素。
7. 顺序 insight 的边际收益：顺序累积 insight 比只条件化最新 insight 更快更高；最大增益出现在前 5 条 insight；多个 hint 条件的优势随后递减。
8. 反直觉的对比：在 Leetcode heldout 上，RLTL;DR（49.1%）和 RLTF-SD（42.2%）反而低于不做反思的 GRPO（55.1%）和 SGE（56.4%）；训练中能找到解不等于 heldout 泛化好，insight 内化在部分任务上可能造成过拟合。
9. insight 内容粒度的张力：单句 TL;DR 已经足够且是默认配置，替换为完整详细反馈的变体并未带来一致优势（依据 Results 检索证据，具体数值缺失）。
10. 自我批判路径不敌直接预测：让策略自我批判给定尝试，弱于直接从 instruction 预测 insight，说明 insight 的“可内化性”比“反思真实性”更重要。

这些现象共同支持论文的中心论点：insight 的价值主要不在于作为上下文提示词，而在于作为可被参数吸收的压缩学习信号。

Q6: 有什么可以进一步探索的点？

基于论文的机制发现、局限与近期平行工作，可以拓展的方向包括：

1. 理解 (task, insight) 内化的机制：为什么不对 rollout 反传也能学到可迁移规律？作者借用 Nakkiran et al. (2026) 的语义平滑解释，但这仍是类比，需要机制层面的实验：如 probing 参数中 insight 信息的存储位置、分析 task→insight 映射的隐式表征、比较 insight 与完整 rollout 的梯度信息含量。
2. 压缩训练范式的规模化：4k 条元组就接近完整性能，那么 insight 数量、任务多样性、模型规模与内化效率之间是什么 scaling 关系？是否可以构造 insight 库的课程学习或主动筛选（只保留高信息量 insight）？
3. 缓解 Leetcode 式过拟合：论文承认 Leetcode 上 RLTL;DR 与 RLTF-SD 训练中能找到解但 heldout 泛化差。可探索的方向包括 insight 多样性正则、任务级 early stopping、在 insight 生成中注入更抽象的策略类知识、或把 Pass@128 阈值换成更细粒度难度分层。
4. insight 表征形式的优化：单句 TL;DR 是手工定义的压缩格式；能否让模型自适应选择压缩粒度、用结构化的 insight（如规则列表、伪代码、更抽象的启发式）替代自然语言单句？当前发现“完整详细反馈”不优于 TL;DR，说明格式与内化效率存在非平凡关系。
5. 与 self-critique 的对照：论文发现自我批判不如直接预测 insight，这与大量反思/自我纠错工作形成张力。值得进一步研究：什么条件下批判性反思优于直接归纳？验证器信号的粒度（单条错误信息 vs. 多轮错误累积）如何影响两者差距？
6. 将 insight 内化扩展到更多模态与任务族：ECHO（terminal 交互）、Lu et al. (2026a)（embodied agent、搜索）、Cook et al. (2026)（代码文档→代码生成）表明该现象跨任务存在；系统验证它在科学推理、AI-for-science 工具调用、长程 agent 规划中的适用性，以及 insight 对多步依赖任务的记忆能力。
7. 把 insight 从“训练脚手架”升级为“终身记忆”：当前 insight 只在训练 batch 内顺序累积；能否跨任务、跨 session 累积 insight，形成持续自我改进的模型？这涉及 insight 去重、冲突消解、陈旧 insight 遗忘等问题。
8. 理论化 RLVR 的学习信号饥饿：Pass@128=0 只是一个操作化阈值；能否给出“无正样本 RL 可学习性”的理论刻画，预测 insight 内化在何种 reward 结构、验证器信息量和策略能力组合下有效？
9. 与在线探索方法的结合：顺序 insight 条件化是一种简单探索增强；可以结合树搜索、best-of-n 动态分配、验证器置信度等，进一步提高极端低成功率任务的采样效率。
10. 更公平的跨方法评测协议：Table 1 与摘要数值口径不一致（SAPI 91.1% vs. 12–13%），后续工作需要统一训练中无 insight eval、训练后 heldout eval、以及 insight 是否在上下文的报告标准，避免方法比较失真。

Q7: 总结一下论文的主要内容

这篇 arXiv 预印本（Kirchhof 等人，编号 2609.37633v1，2026-09-29）研究一个具体且尖锐的问题：当任务难到当前策略在 128 次尝试内一次都成功不了（Pass@128=0）、又没有教师模型或示例解时，RLVR/GRPO 完全学不动，自改进循环无法启动。作者把这一现象归因于“学习信号饥饿”——没有成功 rollout 就没有可被正向放大的奖励信号。

论文的解法是 RLTL;DR，由两个相互配合的技术组成。第一个技术针对探索：把传统的并行 i.i.d. rollout 改成顺序 rollout，每次失败后把验证器的输出交给策略，让它自己写一条高层 TL;DR insight（如“记得给搜索结果分页”），下一条尝试以任务加全部历史 insight 为条件；顺序累积 insight 直到找到解。消融显示顺序累积优于只用最新一条 insight，且收益集中在前 5 条。第二个技术针对学习信号：允许对上下文中的 insight token 反向传播，把 task→insight 的映射直接内化进参数；作者特意对比发现，让策略基于尝试做自我批判不如直接从 instruction 预测 insight 有效。训练目标为 GRPO 与 insight 上的 SFT（LSFT）的混合，默认权重 λ=0.5；前四个消融 run 显示 LSFT 是性能的驱动因素。

实验在 SAPI、Appworld、Leetcode 三个 benchmark 上、用 Qwen 3.5 9B Thinking 策略、训练任务按 Pass@128=0 筛选后进行。结果呈现清晰的层级：标准 GRPO 与 SGE 都停滞在 0–1% Pass@1；RLTL;DR 在训练中 insight 在上下文时达到 14–31%，关键的是 eval 不给 insight 仍有 12–13%，说明内化确实发生；SAPI 的滚动 eval（每个任务被看到一次、评测在反传前）显示内化能泛化到新任务。训练完成后的 heldout eval（Table 1）中，RLTL;DR 在 SAPI 上达 91.1%（GRPO 57.8%）、Appworld 上 61.7%（GRPO 33.7%），但 Leetcode 上只有 49.1%，低于 GRPO 的 55.1% 和 SGE 的 56.4%，作者承认在此数据集上训练中能找到解却过拟合。

论文最引人注目的发现是简化版 SFTL;DR：不用展示 rollout、不对 rollout 反传，只对 (task, insight) 元组做 SFT，4k 条数据即可恢复 RLTL;DR 几乎全部性能，并超过在完整 rollout 上做经典 SFT。Discussion 中作者将此称为 “secret sauce”，并列举三个平行现象（ECHO、Lu et al. (2026a)、Cook et al. (2026)）说明“在 user 消息中的 token 不显式生成也能被内化”并非孤例。结合 Nakkiran et al. (2026) 的语义平滑观点，作者提出一种压缩训练范式：“在这类任务上，记住这类要点”（on this sort of task, keep this sort of thing in mind），希望启发后续研究。

论文的主要局限包括：Leetcode 上的过拟合与泛化失败；摘要指标（12–13%）与 Table 1 指标（如 SAPI 91.1%）之间的口径差异需要在原文中仔细核对；Method 部分在本材料的检索证据中缺失，损失函数与 insight 生成的完整形式化需要回原文确认；部分对比方法（RLTF-SD 等）的全称与实现细节未在检索片段中出现；实验绝对性能仍偏低，说明方法突破的是“从完全学不动到开始能学”的临界点，而非直接达到高可靠性。整体上，论文的核心贡献不是“更强的 RL”，而是一个关于学习信号形式的反直觉发现——压缩后的 (task, insight) 监督可以替代完整 rollout 监督，这可能对自改进、数据效率和 agent 训练范式产生更广泛影响。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向（权重 0.10）直接重合：论文核心是 agentic coding 与工具调用任务上的训练范式，涉及顺序 rollout、验证器反馈、自生成经验。

## 基本信息

- 作者：Michael Kirchhof, Eleonora Gualdoni, Andrew Szot, Khashayar Gatmiry, Aryo Lotfi, Abbas Kazerouni, Omar Attia, Sanjoy Chowdhury, Alexander Toshev
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL, stat.ML
- 日期：2026-09-29
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37633v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Results、Discussion、References 证据片段，并结合 heuristic_draft 与用户画像进行校准；对缺失的 Method 细节与指标口径矛盾已做明确标注。
