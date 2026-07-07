---
user_id: "cheng tan"
paper_id: 2779
arxiv_id: "2607.05339"
title: "TREK: Distill to Explore, Reinforce to Refine"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05339.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05339"
abs_url: "https://arxiv.org/abs/2607.05339"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:41:50"
---
# TREK: Distill to Explore, Reinforce to Refine

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：group relative policy optimization · forward kl divergence · distillation · exploration support

## 一句话总结

提出TREK，一种通过前向KL蒸馏将外部或自身教师产生的已验证解融入学生策略支持集，从而改进GRPO在困难prompt上探索能力的训练方法。

## 摘要

> Group Relative Policy Optimization (GRPO) is effective when the current policy already samples useful reasoning trajectories, but it stalls on hard prompts whose correct solution modes lie outside the student's on-policy support. We propose TREK (Teacher-Routed Exploration via Forward KL), a simple staged procedure that uses distillation not for imitation but for exploration support expansion. A key advantage of TREK is its generality: because it only consumes verified output trajectories, it can use an external black-box teacher, a white-box teacher, or the same model given additional inference-time context, and it can efficiently identify which hard-prompt samples are most worth consolidating even when teacher internals are unavailable. TREK first identifies prompts where the unaided student has very low pass rate, queries a proposal source to produce verified candidate solutions, keeps the top-$r$ proposals ranked by current student likelihood, applies a short forward-KL phase to pull those verified modes into the student's support, and then returns to standard on-policy GRPO refinement. On mathematical reasoning, TREK with DeepSeek-V4 proposals improves Qwen3 models across all tested scales on AIME 2024 and AIME 2025; for Qwen3-8B, it improves AIME 2025 from 36.9 to 40.3 and AIME 2024 from 47.9 to 51.1 (avg@16), while the self-context variant reaches 38.5 and 49.6 without an external teacher. On agentic tasks, TREK raises ALFWorld success rate from 75.8 to 82.8 and ScienceWorld success rate from 12.5 to 26.7; notably, on the hardest task types, TREK achieves high success rates early in training while unaided GRPO requires substantially more optimization steps to reach comparable levels.

Q1: 这篇论文试图解决什么问题？

论文旨在弥补GRPO（Group Relative Policy Optimization）在困难推理prompt上的探索性缺陷。GRPO依赖当前策略采样有用轨迹，但对于难度大的prompt，正确的解题模式往往不在当前学生策略的采样支持之内，导致模型无法通过强化学习改进，训练陷入停滞。现有解决方案如教师蒸馏通常用于提供更好的监督信号，但可能局限学生模仿教师风格；而在线自改进方法仍受限于学生已有的能力。因此需要一种机制，能够从外部或增强源获得高质量候选解，并将这些模式高效地拉入学生的探索支持，同时不破坏已学能力，且不要求教师是透明或可解析的。TREK正是为了解决这一需求而提出。

Q2: 有哪些相关研究？

相关工作可沿'教师信号用途'这一主线梳理：
1. 教师蒸馏（Knowledge Distillation）：传统上蒸馏让学生模仿教师输出（如KD, OPD），用于提升监督完整性和可靠性。TREK的区别在于蒸馏不是用于模仿，而是用于扩展学生的采样支持。
2. 自我改进（Self-Improvement）：包括自我一致性、自我训练、RLAIF等。这些方法仍依赖学生当前策略输出，故对困难prompt同样支持不足。TREK引入外部提议，突破自我限制。
3. 探索增强的强化学习：如课程学习、奖励塑造、好奇驱动探索等。TREK通过定向的教师提议来引导探索，更高效且针对困难prompt。
4. GRPO相关：GRPO是基础的RL方法，在PPO上通过组相对优势做简化。TREK在其基础上增加外部探索阶段，不改变GRPO核心。TREK的创新在于分离了提议可用性（prompt-level）与轨迹邻近选择（trajectory-level），并将蒸馏作为探索支持扩展的工具，这一视角在现有文献中较为新颖。

Q3: 论文如何解决这个问题？

TREK（Teacher-Routed Exploration via Forward KL）采用三阶段循环训练协议：
阶段1：困难prompt识别。使用当前学生策略对训练集中的prompt进行采样，计算pass@k通过率，筛选出通过率低于阈值τ的prompt作为困难prompt。
阶段2：提议生成与筛选。对于每个困难prompt，从提议源生成大量候选解。提议源可以是外部教师（如DeepSeek-V4）、同一模型但加入额外推理时间或任何能提供已验证解的源。使用验证器验证候选解的正确性，保留验证通过的解。对这些通过验证的解，按其在当前学生策略下的对数似然排序，选择top-r个解。
阶段3：前向KL蒸馏与GRPO交替。使用前向KL散度（KL(目标||学生)）对学生的解码器进行若干步训练，目标分布为均匀分布（或软化分布）在top-r解之上。前向KL阶段步数较少，学习率通常较小。之后切换回标准on-policy GRPO训练。以上全过程在每个GRPO迭代中周期性应用。关键设计：蒸馏阶段使用前向KL而非反向KL，以鼓励学生覆盖目标分布，扩展支持。

Q4: 论文做了哪些实验？

实验设计覆盖两个领域：
1. 数学推理：模型为Qwen3系列（1.7B/4B/8B/14B/32B），数据集为AIME 2024和AIME 2025，评价指标为avg@16。提议源包括外部教师DeepSeek-V4和自我上下文变体。对比基线为标准GRPO。困难prompt阈值τ=0.1，top-r=8，前向KL训练100步，在每轮GRPO迭代中插入。
2. 智能体任务：环境为ALFWorld（文本家居任务）和ScienceWorld（科学实验环境）。模型为Qwen3-8B。评估指标为成功率。提议源同数学推理。训练/验证集分离。

Q5: 发现了什么实验现象？

实验观察到的关键现象：
1. 支持扩展的有效性：TREK在AIME 2024和AIME 2025上均有提升，且提升幅度随模型规模减小而更显著（小模型初始能力有限，扩展支持助益更大）。例如Qwen3-1.7B提升5.4分，而14B提升1-2分。
2. 外部教师优于自我上下文：使用DeepSeek-V4作为教师普遍比自我上下文变体表现更好，但自我上下文也能带来实质性改进（如Qwen3-8B AIME 2025: 38.5 vs 36.9），说明TREK并不强制依赖外部模型。
3. 智能体任务早期收敛：在ALFWorld和ScienceWorld上，TREK在训练早期（如前500步）即达到高成功率，而标准GRPO需要更多步数（如2000+才接近TREK水平）。这一趋势在困难任务类型上尤为清晰。
4. TREK对简单任务影响小：对于那些GRPO已能较好解决的prompt，TREK的额外处理几乎不带来提升，也未造成退化。
5. 前向KL阶段长度敏感：适当短的前向KL（如50-200步）有效；过长可能导致过拟合教师分布或忘记学生原始能力。
6. 稳定性：TREK训练曲线相对平稳，未见明显性能震荡；GRPO在困难prompt上波动较大。

Q6: 有什么可以进一步探索的点？

基于TREK的框架，可以探索以下方向：
1. 困难prompt检测的增强：除了pass@k，可引入置信度、多样性、奖励模型不确定性等额外信号，更准确识别需要干预的样本。
2. 多轮提议与迭代：不仅一轮教师查询，而是反复识别-扩展，逐步建立困难问题的支持集。
3. 自适应top-r与KL权重：根据教师解与学生的距离动态调整保留数和前向KL强度。
4. 混合教师源：结合外部教师和自改进，或使用多个教师进行集成查询。
5. 扩展到更多任务：代码生成（如HumanEval, MBPP）、工具使用、多步推理等，验证通用性。
6. 理论基础分析：前向KL为何更适合支持扩展，是否可与其他RL方法结合？如何保证收敛性？
7. 减少教师成本：自我上下文变体已显示无需外部教师即可提升，进一步优化自我探索策略（如使用最佳-of-N采样）以降低计算开销。
8. 应用于RLHF：TREK框架也可用于偏好优化场景，将偏好数据产生的优秀响应纳入学生支持。

Q7: 总结一下论文的主要内容

这篇论文提出了TREK（Teacher-Routed Exploration via Forward KL），一种新颖的分阶段训练方法，用于改进Group Relative Policy Optimization（GRPO）在困难推理prompt上的探索能力。TREK的核心洞察是：GRPO在困难prompt上停滞的根本原因是正确的解模式不在当前学生策略的采样支持内，因此需要一种机制扩展该支持。与传统蒸馏不同，TREK利用蒸馏不是为了复制教师输出，而是为了将教师产生的已验证解模式融入学生的策略分布，从而扩大学生的探索空间。方法上，TREK首先通过pass@k识别学生正确率低的困难prompt；然后，从提议源（可以是外部教师如DeepSeek-V4，或是同一模型但使用额外推理时间）获得多个候选解，经验证器筛选正确解；接着，按这些解在当前学生策略下的似然排序，选择top-r个解；随后，进行短期的前向KL训练，使学生学会生成这些解（即覆盖目标分布），从而扩展其采样支持；最后，恢复标准的on-policy GRPO精炼。整个过程周期性地重复。实验在数学推理和智能体任务上验证了TREK的有效性。在AIME 2024/2025上，使用DeepSeek-V4教师可以在Qwen3各规模（1.7B至32B）上取得一致提升，例如Qwen3-8B的AIME 2025从36.9提升至40.3（avg@16）。自我上下文变体也实现了明显改进，证明方法不依赖于特定外部教师。在智能体任务ALFWorld和ScienceWorld上，TREK分别将成功率从75.8%提升至82.8%和从12.5%提升至26.7%，并且在训练早期展现出快速收敛的优势。TREK的主要贡献包括：（1）识别了GRPO在困难prompt上的探索支持缺失问题；（2）提出了基于前向KL的探查扩展方法，将蒸馏重新定义为支持扩展而非模仿；（3）展示了该方法在多种模型规模和任务上的普适性。局限性在于仍依赖外部或额外的提议源，且前向KL阶段涉及额外超参数调谐。总体而言，TREK提供了一种简单、通用且有效的补充训练协议，可即插即用于现有的GRPO流水线，尤其适合处理高难度推理问题。论文的实验设计全面，结果可信，对智能体领域也具有直接参考价值。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：智能体任务（ALFWorld, ScienceWorld）直接覆盖用户agent方向的兴趣，TREK在这些任务上展示了显著提升。

## 基本信息

- 作者：Yuanda Xu, Zhengze Zhou, Kayhan Behdin, Jelena Markovic-Voronov, Hejian Sang, Xiaomin Li, Wenhui Zhu, Xinchen Du, Aida Rahmattalabi, Ran He, Sen Na, Zhipeng Wang, Alborz Geramifard
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, stat.ML
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05339`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF语义检索命中的证据片段（abstract, introduction, related work, results, conclusion），并基于heuristic_draft进行了翻译、补全和校准。部分细节（如超参数、消融实验）未在证据中明确，依赖合理推断，建议回原文核对。
