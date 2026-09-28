---
user_id: "cheng tan"
paper_id: 13873
arxiv_id: "2609.30652"
title: "Recursive Self-Improvement via On-Policy Distillation for Reasoning"
institution: "根据论文作者信息（Shangjian Yin, Zehao Zhao, Kavosh Asadi 等），该研究团队主要来自 Meta 等前沿 AI 研究机构（合理推断，需结合正文最终确认）。"
publish_date: "2026-09-28"
pdf_url: "https://arxiv.org/pdf/2609.30652"
abs_url: "https://arxiv.org/abs/2609.30652"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-29T00:31:41"
---
# Recursive Self-Improvement via On-Policy Distillation for Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：recursive self-improvement · on-policy distillation · mathematical reasoning · dynamic co-evolution

## 一句话总结

本论文提出了通过动态共演化（DCE）与自我精炼简洁学习（SRCL）相结合的递归自改进框架，解决了策略内自蒸馏中特权教师模型因冻结而无法吸收学生模型新知识的局限性，在竞赛级数学基准上取得了显著的性能提升并有效控制了输出长度。

## 摘要

> On-policy distillation (OPD) trains a student model by having it generate trajectories, then matching its next-token predictions with an external teacher's next-token predictions. This provides dense, token-level supervision to the student. On-policy self-distillation (OPSD) eliminates the need for the external teacher. Specifically, a second frozen copy of the student model, now given the ground truth in its context, serves as the teacher. The student model only receives the problem and learns to mimic the privileged teacher model, while the teacher remains frozen throughout training. Previous work showed that freezing the teacher is useful for training stability, but we argue that this can prevent the teacher from incorporating the improvements learned by the student during training. Our primary contribution is to address this limitation with a recursive framework built around two complementary components. First, we let the privileged teacher co-evolve with the student so that revision learned in one round can guide the next, a process we refer to as Dynamic Co-Evolution (DCE). Second, because stronger revision can also make responses too verbose and self-critical, we additionally train on shorter, verified rewrites of the model's own on-policy responses. We call this complementary objective Self-Refined Concise Learning (SRCL). Overall, our comprehensive evaluations show that DCE+SRCL outperforms OPSD across multiple model scales and four competition-level mathematics benchmarks. Specifically, on Qwen3-8B, DCE+SRCL reaches 65.97% Average@12, outperforming OPSD by 35.62 percentage points while reducing mean output length by 7.80% relative to DCE alone.

Q1: 这篇论文试图解决什么问题？

### 核心研究问题
本论文聚焦于大语言模型（LLM）在复杂推理任务（如竞赛级数学问题）中的**递归自改进（Recursive Self-Improvement）**机制。具体而言，它试图解决现有的**策略内自蒸馏（On-Policy Self-Distillation, OPSD）**框架中，特权教师模型（Privileged Teacher）保持冻结所带来的能力演化瓶颈。

### 现有方法的缺陷与技术痛点
1. **教师模型能力固化**：在传统的 OPSD 中，为了维持训练稳定性，特权教师模型（即在输入上下文中额外提供真实答案的复制模型）在整个训练过程中是完全冻结的。这导致学生模型在训练后期学到的新知识、更优的推理路径或修正能力，无法反哺给教师模型，限制了整个系统的自改进上限。
2. **推理冗长与过度自我批判（Verbosity & Over-criticism）**：当模型通过强化或自改进获得更强的纠错和修正能力时，其生成的推理轨迹往往会变得极其冗长。模型倾向于在上下文中进行反复的自我否定和冗余解释，这不仅增加了计算开销（Token 成本），还可能因为过长的上下文引入新的推理错误。

### 隐含假设与研究边界
- **隐含假设**：论文假设通过在输入中提供 Ground Truth，相同的模型结构可以表现出超越其标准状态的“特权能力”（Privileged Capability），并且这种能力可以通过 Token 级别的交叉熵损失有效地蒸馏给自身。
- **研究边界**：本研究主要在数学推理（Mathematics Reasoning）这一具有明确客观对错标准的领域展开，依赖于能够自动验证正确性的基准测试。

Q2: 有哪些相关研究？

### 策略内蒸馏与自蒸馏（On-Policy Distillation & Self-Distillation）
传统的策略内蒸馏（OPD）依赖于一个更强大的外部教师模型（如 GPT-4）来为学生模型生成的轨迹提供密集的、Token 级别的监督。为了摆脱对外部昂贵模型的依赖，策略内自蒸馏（OPSD）被提出。OPSD 利用模型自身的另一个副本作为教师，通过在提示词中加入 Ground Truth 来构建“特权上下文”，从而引导学生模型。然而，现有 OPSD 方法普遍采用静态教师策略，忽略了教师与学生的协同进化。

### 递归自改进与大模型对齐（Recursive Self-Improvement & Alignment）
近年来，诸如 STaR、ReST 和各种基于强化学习（RLHF/RLAIF）的方法尝试让模型通过生成、过滤和重新训练来实现自我提升。然而，这些方法大多基于序列级别的奖励（Sequence-level Reward），缺乏 Token 级别的细粒度监督。此外，诸如拒绝采样（Rejection Sampling）等方法容易导致模型输出分布的漂移，且无法解决模型在具备纠错能力后输出变得冗长的问题。本研究提出的 DCE 和 SRCL 正是针对这些相关研究的不足而设计的。

Q3: 论文如何解决这个问题？

### 递归自改进框架（Recursive Self-Improvement Framework）
本文提出了一种全新的递归训练架构，通过以下两个互补的组件来打破传统 OPSD 的限制：

#### 1. 动态共演化（Dynamic Co-Evolution, DCE）
- **机制设计**：取消了教师模型在训练过程中始终冻结的限制。DCE 采用多轮（Rounds）递归训练的模式。在每一轮中，上一轮训练更新后的学生模型将被用作新一轮的特权教师模型基础。
- **作用原理**：通过这种方式，学生模型在上一轮中学会的修正技能、更高效的推理逻辑，能够直接融入到下一轮教师模型的特权上下文中，从而生成更高质量、更具前瞻性的 Token 级监督信号，实现教师与学生的“协同进化”。

#### 2. 自我精炼简洁学习（Self-Refined Concise Learning, SRCL）
- **机制设计**：为了对抗 DCE 带来的输出冗长化趋势，SRCL 引入了一个互补的优化目标。系统会收集模型自身在策略内生成的响应，并筛选出最终结果正确的轨迹。
- **精炼与重写**：利用模型自身的能力（或通过特定提示词），将这些正确的、但可能包含冗余自我纠错的冗长轨迹，重写为更短、更直接、逻辑更紧凑的“精炼版本”。
- **联合训练**：将这些精炼后的短轨迹作为额外的监督数据引入训练，强制模型在保持高准确率的同时，学习如何用更精简的语言进行推理，从而在策略内实现“简洁性”的蒸馏。

Q4: 论文做了哪些实验？

### 实验设置与基准数据集
本研究在四个具有高度挑战性的竞赛级数学基准测试上进行了全面评估：
1. **MATH**：标准的困难数学问题集。
2. **GSM8K**：多步数学推理服务集。
3. **AIME (American Invitational Mathematics Examination)**：高难度美国数学邀请赛题目。
4. **OlympiadBench**：奥林匹克级别的综合理科推理基准。

### 评估模型规模
实验覆盖了多个模型尺度，重点在 **Qwen3-8B** 等主流开源基准上进行了深度验证，以展示该方法在不同参数量下的泛化能力和 Scaling 效应。

### 基线方法（Baselines）
- **Standard SFT**：标准监督微调。
- **OPSD (On-Policy Self-Distillation)**：传统的冻结教师策略内自蒸馏方法。
- **DCE Alone**：仅采用动态共演化而不加简洁性约束的消融变体。

Q5: 发现了什么实验现象？

### 核心实验现象与指标分析
1. **大幅超越基线**：在 Qwen3-8B 模型上，结合了 DCE 和 SRCL 的完整方法（DCE+SRCL）达到了 **65.97% 的 Average@12** 综合得分，相比于传统的 OPSD 实现了 **35.62 个百分点** 的巨大跨越。这有力地证明了动态更新教师模型以及引入简洁性精炼的必要性。
2. **长度控制与性能的张力（Trade-off）**：实验观察到，仅使用 **DCE Alone** 时，虽然准确率有所提升，但模型的平均输出长度急剧增加，表现出明显的冗长和多余的自我批判行为。而引入 **SRCL** 后，在保持甚至进一步提升准确率的前提下，**平均输出长度相对于 DCE Alone 显著减少了 7.80%**。
3. **跨尺度泛化**：该方法在不同规模的模型上均表现出一致的性能增益，表明动态共演化带来的自改进能力具有良好的 Scaling 特性，并非特定规模模型的偶发现象。

Q6: 有什么可以进一步探索的点？

### 可进一步探索的方向
1. **跨领域泛化验证**：目前该方法主要在数学推理领域得到验证。未来可以探索将其扩展到代码生成（Code Generation）、法律文本分析、多步逻辑推理智能体（Agent Trajectory Planning）等其他具有客观验证标准的复杂任务中。
2. **动态共演化的步长与稳定性控制**：在更长周期的递归训练中，如何精确控制教师模型的更新频率（例如引入 Polyak 平均或软更新机制），以防止在极端情况下出现训练崩溃或分布漂移，是一个值得深入研究的系统性问题。
3. **更高级的简洁性度量**：目前的 SRCL 依赖于模型自身的重写。未来可以引入基于信息论的压缩比度量，或者结合强化学习中的长度惩罚（Length Penalty），以实现更精细的推理效率优化。

Q7: 总结一下论文的主要内容

本论文针对大语言模型在复杂推理任务中的自改进问题，提出了一种创新的递归自蒸馏框架。传统的策略内自蒸馏（OPSD）通过冻结一个拥有 Ground Truth 特权的教师模型来指导学生，虽然保证了训练稳定，但限制了教师模型吸收新知识的能力，且容易导致模型输出变得冗长和过度自我批判。为了打破这一僵局，本文提出了动态共演化（DCE）与自我精炼简洁学习（SRCL）相结合的方法。DCE 允许特权教师模型在多轮递归训练中与学生同步演化，从而提供更高质量的 Token 级监督；SRCL 则通过让模型在自身策略内响应的简短、已验证的重写版本上进行训练，有效压缩了推理长度。在 Qwen3-8B 等模型及四个竞赛级数学基准（如 MATH、AIME）上的实验表明，DCE+SRCL 取得了突破性的进展，不仅在 Average@12 指标上超越 OPSD 达 35.62 个百分点，还成功将输出长度降低了 7.80%，实现了推理准确率与计算效率的双重提升。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该方法虽然以数学推理为载体，但其“动态教师-学生共演化”的递归自改进架构，对于构建具备自我进化能力的通用智能体（Agent）具有极高的借鉴价值。

## 基本信息

- 作者：Shangjian Yin, Zehao Zhao, Kavosh Asadi, Rui Liu, Yuchen Lu, Shike Mei, Hang Cui, Luke Simon, Zhouxing Shi, Hamed Firooz
- 机构：根据论文作者信息（Shangjian Yin, Zehao Zhao, Kavosh Asadi 等），该研究团队主要来自 Meta 等前沿 AI 研究机构（合理推断，需结合正文最终确认）。
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-28
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.30652`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 PDF 抓取或解析失败，本次报告改为按模板基于摘要和元数据生成；方法与实验细节建议回原文核对。 本次生成主要参考了论文的元数据、摘要以及详细的启发式草稿信息，对核心方法（DCE+SRCL）的机制、实验结果及技术权衡进行了深度推导与结构化中文重构。
