---
user_id: "cheng tan"
paper_id: 5712
arxiv_id: "2607.24720v1"
title: "The Physics of Multi-Turn Long-Horizon Planning: From Pre-training to Post-training via Single- and Multi-Teacher On-Policy Agentic Distillation"
institution: "中国科学院自动化研究所，复杂系统认知与决策智能重点实验室"
publish_date: "2026-07-27"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.24720v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.24720v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-29T11:49:59"
---
# The Physics of Multi-Turn Long-Horizon Planning: From Pre-training to Post-training via Single- and Multi-Teacher On-Policy Agentic Distillation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-turn planning · long-horizon planning · foundation model agents · chain-of-thought

## 一句话总结

本论文通过统一可控的多轮环境，系统研究了基础模型智能体在多轮长时规划能力在预训练、后训练和整合阶段的获取、塑造与汇聚机制。

## 摘要

> Multi-turn long-horizon planning is critical for foundation model agents, yet how to fundamentally improve it remains unclear. Existing models are trained on uncontrollable and opaque Internet data, making it difficult to identify how planning ability is acquired, shaped, and integrated. To address this challenge, we introduce a unified and controlled multi-turn environment that enables precise control. It allows systematically study long-horizon planning across three stages. (1) Planning ability acquisition during pre-training. We study data format, distribution, and quality. Explicit world model construction through CoT state transition modeling yields stronger long-horizon generalization. Atomic skills alone are insufficient for compositional generalization, whereas a litte long-horizon data works. Moreover, suboptimal trajectories severely impair performance because errors amplify over long horizons. (2) Planning ability shaping via GRPO and OPD post-training. Through mutual information, we distinguish general planning patterns from task-specific planning knowledge. For planning patterns, we identify three application regions of post-training: unnecessary, effective, and unsupported. OPD has a broader effective region than GRPO under low-quality and long-horizon settings, as it provides more consistent update directions. For planning knowledge, distilling unseen procedures from a teacher with different knowledge may impair student's prior world modeling without fully establishing new knowledge. (3) Planning ability integration through MOPD post-training. We show that multi-teacher on-policy distillation (MOPD) integrates capabilities by converging to shared planning-pattern across environments. Compatible patterns enable cross-environment generalization, partially shared patterns support continual learning, while completely conflicting patterns cause severe interference.

Q1: 这篇论文试图解决什么问题？

该文聚焦于基础模型智能体在多轮长时规划任务中的能力瓶颈。当前模型大多在不可控、不透明的互联网数据上训练，导致研究者难以区分规划能力是来自预训练数据还是后训练激活，也难以系统性地改进。具体问题包括：(1) 预训练数据格式（纯动作序列 vs 包含状态建模的CoT）、数据中长时轨迹的分布比例、数据质量（次优轨迹）如何影响规划能力的获取；(2) 后训练方法（GRPO和OPD）分别在何种条件下有效或失效，其作用于通用规划模式还是任务特定知识；(3) 当模型需要整合来自不同教师的规划能力时，如何实现跨环境泛化、持续学习并避免冲突。论文通过构建可控环境，将这些问题解耦并逐一实验回答。

Q2: 有哪些相关研究？

相关工作覆盖三大领域：(1) 长时智能体规划：当前研究分为基于符号规划的方法和基于学习的方法，但缺乏对预训练数据作用的分析。(2) 预训练数据策略：对数据格式（如是否包含思维链）、数据分布（长尾数据混合）、数据质量（错误传播）的研究已有探讨，但未专门针对多轮长时规划。(3) 后训练方法：GRPO（组相对策略优化）和OPD（在线策略蒸馏）被应用于对齐与强化，但它们在规划能力塑造上的差异和适用范围尚待厘清。本文在统一环境中对比这些方法，并引入互信息分析来区分规划模式与知识。此外，多教师蒸馏（MOPD）借鉴了知识集成研究，但侧重在线策略下的兼容性分析。

Q3: 论文如何解决这个问题？

论文通过三阶段实验设计系统分析规划能力。首先，构建一个统一可控的多轮环境（Section 3），环境具有可调节的视野长度、任务难度和奖励设计。然后：(1) 预训练阶段（Section 4），在固定模型中训练不同数据配置，包括：a) 数据格式：仅动作序列 vs 显式状态建模（CoT）；b) 长时数据比例：从0%到100%；c) 数据质量：专家轨迹 vs 次优轨迹（含噪声动作）。评估模型在不同规划长度上的泛化能力。(2) 后训练阶段（Section 5），基于预训练模型，采用GRPO和OPD进一步训练。通过互信息分解更新信号为通用规划模式与任务特定知识，识别三大应用区域：不必要的、有效的和无效的。特别在低质量或长时环境下比较两者。(3) 整合阶段（Section 6），提出MOPD：多个教师（不同环境训练）同时在线指导一个学生，学生通过策略蒸馏学习统一规划模式。实验分析教师之间的模式兼容性（完全兼容、部分共享、完全冲突）及其对泛化、持续学习和干扰的影响。

Q4: 论文做了哪些实验？

实验部分在多个维度展开：(1) 预训练数据实验：在三种视野长度（short, medium, long）上测试，使用GPT-2等模型。变量包括：是否使用CoT状态建模、长时数据比例（0%,5%,50%,100%）、轨迹质量（专家 vs 含10%错误动作的次优轨迹）。训练步数统一为9000步，评估准确率。(2) 后训练方法对比实验：基于预训练模型，分别用GRPO和OPD进行后训练，变化数据质量（专家/次优）和长度（medium/long）。测量规划准确率提升，并计算互信息分解更新方向。(3) 多教师整合实验：设计三个教师环境（兼容、部分共享、冲突），学生通过MOPD训练，评估跨环境泛化（零样本迁移）、持续学习（顺序引入新环境）和冲突消解（同时学习冲突模式）。主要评估指标包括任务成功率、规划路径长度等。

Q5: 发现了什么实验现象？

主要发现：(1) 预训练阶段：a) 显式世界模型（CoT状态建模）在长时规划上显著优于纯动作序列，尤其在组合泛化场景。b) 仅需少量长时数据（5%即可）即可产生组合泛化能力，但更多数据进一步提升。c) 次优轨迹中的错误在长时规划中放大，严重损害性能，且模型无法自动识别错误。(2) 后训练阶段：a) GRPO在专家轨迹和短时长下有效，但在低质量或长时设置下更新方向不一致，导致效果退化；OPD则提供更一致的梯度，因此有效区域更广。b) 互信息分析表明，GRPO更多更新任务特定知识，而OPD更多更新通用规划模式。c) 当预训练数据质量低时，后训练改善有限；次优数据下OPD强于GRPO。(3) 整合阶段：a) MOPD在兼容模式下实现正迁移，跨环境泛化提升。b) 部分共享模式支持持续学习，新知识不破坏旧技能。c) 完全冲突模式导致干扰，学生训练不稳定，最终模式无法收敛。d) 在线策略蒸馏（学生自身轨迹）比离线蒸馏更有效。

Q6: 有什么可以进一步探索的点？

论文为理解规划能力提供了基础，未来方向包括：(1) 将可控环境扩展到更真实和复杂的任务（如机器人操作、网页导航），验证结论的通用性。(2) 探索其他后训练算法（如PPO、DPO）在规划能力塑造上的作用。(3) 研究数据质量的自适应过滤或修正方法，以应对互联网噪声。(4) 多教师蒸馏中的兼容性度量与自动选择机制。(5) 将互信息分析方法应用于更大规模模型，检验其解释力。(6) 结合模型规模与数据规模的缩放律，研究规划能力的涌现条件。

Q7: 总结一下论文的主要内容

这篇论文题为《多轮长时规划的物理：从预训练到后训练的单/多教师在线策略蒸馏》，由中国科学院自动化研究所的研究人员完成。论文旨在系统性地理解基础模型智能体如何获取、塑造和整合多轮长时规划能力。主要贡献包括：(论证主线) 现有研究无法区分规划能力来自预训练还是后训练，作者因而构建统一可控的多轮环境，将问题解耦为三个阶段。(技术主线) 预训练阶段，通过控制数据格式（CoT vs 纯动作）、数据分布（长时比例）和数据质量，揭示显式世界模型构建、适量长时数据和高质量轨迹对组合泛化的关键作用，并发现次优轨迹的错误放大效应。后训练阶段，对比GRPO和在线策略蒸馏（OPD），利用互信息分解更新信号为通用规划模式与任务特定知识，确定两者各自的适用区域：OPD在低质量和长时设置下更有效，因为它提供一致的方向更新。整合阶段，提出多教师在线策略蒸馏（MOPD），实验证明兼容模式可跨环境泛化，部分共享支持持续学习，完全冲突导致干扰。(实验主线) 论文在T=200步的合成环境中进行大量消融实验，包括不同数据配置、后训练算法组合以及多教师设置，验证了各阶段的核心假设。总体而言，该工作为长时规划能力的形成机制提供了首个系统性实验分析，对预训练数据策略和后训练算法选择具有指导意义。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对智能体研究方向有直接贡献，特别是长时规划能力的数据需求与后训练选择。

## 基本信息

- 作者：Tianyi Men, Zhuoran Jin, Kang Liu, Jun Zhao
- 机构：中国科学院自动化研究所，复杂系统认知与决策智能重点实验室
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG
- 日期：2026-07-27
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.24720v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）及字段证据映射，综合启发式草稿完成。
