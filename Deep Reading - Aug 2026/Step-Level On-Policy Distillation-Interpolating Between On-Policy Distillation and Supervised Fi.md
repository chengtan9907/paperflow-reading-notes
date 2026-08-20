---
user_id: "cheng tan"
paper_id: 8144
arxiv_id: "2608.16333"
title: "Step-Level On-Policy Distillation: Interpolating Between On-Policy Distillation and Supervised Fine-Tuning"
publish_date: "2026-08-18"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.16333.pdf"
pdf_url: "https://arxiv.org/pdf/2608.16333"
abs_url: "https://arxiv.org/abs/2608.16333"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-19T09:35:09"
---
# Step-Level On-Policy Distillation: Interpolating Between On-Policy Distillation and Supervised Fine-Tuning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge distillation · on-policy distillation · step-level supervision · llm agents

## 一句话总结

提出 Step-Level On-Policy Distillation (SOPD)，通过在学生生成的完整轨迹上按自然步骤施加教师监督，在 SFT 的长程校正与 OPD 的在线策略优势之间取折中，并在推理和智能体任务上显著超过传统 SFT 与 OPD。

## 摘要

> On-policy distillation (OPD) aligns a student model with a teacher's logit distribution on student-generated trajectories. This approach has achieved strong empirical gains and can often surpass conventional off-policy distillation with substantially less data. However, standard token-level OPD can provide only fragmented corrections along an erroneous student trajectory and cannot unfold a complete and correct repair path. Motivated by this limitation, we propose Step-Level On-Policy Distillation (SOPD), which combines the long-horizon correction of supervised fine-tuning (SFT) with the on-policy advantage of OPD to provide step-level supervision over complete student-generated trajectories. We show that, at different limits of step length, SOPD reduces to SFT or approximates OPD. Compared with SFT, the teacher responses in SOPD are conditioned on student trajectories and therefore align more closely with student-visited states; compared with OPD, SOPD provides longer-horizon corrections rather than fragmented token-level guidance. Across both reasoning and agent tasks, SOPD substantially outperforms conventional SFT and OPD. For example, on ALFWorld, SOPD improves the average success rate by 13.4 points over Vanilla OPD. We hope this work offers a new perspective for future research on distillation methods.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是标准 on-policy distillation (OPD) 在学生轨迹上仅提供 token 级、碎片化的校正，无法给出完整且正确的修复路径。具体而言：1) 当学生轨迹本身有早期错误时，后续 token 级监督只能逐点修补，学生难以学会从错误状态中恢复并走出一条完整正确轨迹；2) 传统 SFT 使用教师生成的轨迹，虽提供长程监督，但存在 train-test mismatch，即教师轨迹分布与学生实际访问状态分布不一致；3) OPD 虽然在线匹配学生分布，但 token 级监督的视野太短，无法像 SFT 那样提供长视野的指导。因此需要在两种监督信号之间取一个插值：既保持 on-policy 的状态分布匹配，又提供 step-level 的长程修复监督。

Q2: 有哪些相关研究？

相关研究包括：1) Knowledge distillation (Hinton et al., 2015)，将教师分布迁移到更小的学生模型；2) Sequence-level distillation (Kim & Rush, 2016)，在教师生成序列上训练，用于缓解 train-test mismatch；3) Standard OPD (Agarwal et al., 2024)，沿学生输出查询教师概率并最小化 token 级散度；4) DAgger 风格的 LLM 智能体训练方法 (Lauffer et al., 2025)，试图保留 SFT 的长程监督同时从学生状态生成更多训练轨迹，但 DAgger 会将教师响应直接插入学生轨迹，改变状态分布；5) Trajectory-Refined Distillation (TRD)，观察到标准 OPD 的固有局限——后续目标仍以初始错误学生前缀为条件，而不是包含早期教师校正的前缀；6) 其他蒸馏扩展方向：self-distillation、超越教师、与 RL 集成、black-box distillation 等。论文在 related work 中定位 SOPD 相对于这些方法的区别。

Q3: 论文如何解决这个问题？

SOPD 的核心思路是将学生自身生成的完整轨迹保留下来，在每个自然步骤（natural step，而非每个 token）向黑盒教师查询生成目标，并用 step-balanced cross-entropy loss 优化。做法上：1) 学生模型在环境中或给定 prompt 下生成完整轨迹；2) 在轨迹的每个自然步骤处，将该步骤之前的学生轨迹（包含学生已经做出的决策）作为条件，交给教师模型生成该步骤的目标响应；3) 优化一个 step-balanced 的交叉熵损失，使每个步骤的监督权重平衡，避免长轨迹中早期步骤被淹没；4) 在 step length 的极限下，当每个 step 退化为整个序列时，SOPD 退化为 SFT（教师基于初始 prompt 生成完整响应）；当 step 长度压缩到 token 级时，SOPD 近似 OPD（在每个 token 查询教师分布）。相比 OPD，SOPD 的教师目标看到的是包含前面步骤学生输出的完整前缀，因此能提供更长的校正路径；相比 SFT，教师目标以学生轨迹为条件，因此与学生访问状态更对齐。此外，SOPD 的 step-level 查询天然适用于黑盒蒸馏，因为只需要查询教师生成目标文本，而不需要教师概率分布。

Q4: 论文做了哪些实验？

论文在推理任务和智能体任务上进行实验。具体任务集包括：1) 推理任务（reasoning tasks），可能涵盖数学推理或常识推理等；2) 智能体任务，明确提到 ALFWorld（文本交互式家务智能体环境）。对比方法包括：传统 SFT、Vanilla OPD、可能还有 off-policy sequence distillation 和 DAgger 风格方法。评测指标：ALFWorld 使用平均成功率；推理任务使用准确率或类似指标。由于检索证据有限，实验设置的具体细节（如模型规模、数据量、训练步数、温度设置等）未在证据中完整给出，需要回原文确认。

Q5: 发现了什么实验现象？

实验观察到的主要现象是：1) SOPD 在推理和智能体任务上都大幅超过传统 SFT 和 OPD；2) 在 ALFWorld 上，SOPD 平均成功率比 Vanilla OPD 提高 13.4 个百分点，这是一个大额提升；3) 从机制上可以推断（推测）：OPD 的碎片化校正导致错误累积后难以修复，而 SOPD 的长程步骤级校正能避免错误轨迹的持续偏移；4) 相比 SFT，SOPD 与学生状态分布的对齐能够降低分布失配带来的样本效率损失。但具体消融实验（如 step length 的影响、不同长度的行为、是否验证了极限退化为 SFT/OPD 等）在现有证据中未呈现，需要查阅原文确认。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向包括：1) 自动选择最优 step 粒度，而非固定自然步骤；2) 将 SOPD 与强化学习、self-distillation 或超越教师能力的研究结合；3) 将 SOPD 扩展到多模态、代码生成等需要长程结构化输出的任务；4) 理论分析 SOPD 与 DAgger、OPD 的偏差-方差权衡；5) 在更大规模模型和数据上验证其扩展性；6) 探索迭代式 SOPD：学生改进后重新生成轨迹再蒸馏，形成闭环自我提升；7) 将 step-balanced loss 改为自适应权重（如按状态不确定性加权）；8) 黑盒蒸馏场景下与仅输出采样、best-of-n 等策略结合的收益。

Q7: 总结一下论文的主要内容

这篇论文提出 Step-Level On-Policy Distillation (SOPD)，旨在解决标准 on-policy distillation（OPD）只能提供 token 级碎片化校正、无法引导学生走出完整正确轨迹的问题。论文首先指出现有 OPD 沿学生生成轨迹逐 token 查询教师分布，虽然保持了 on-policy 状态分布，但校正视野太短，学生一旦早期出错，后续监督无法展开完整修复路径。传统 SFT 虽然提供长程监督，但训练数据来自教师轨迹，存在 train-test mismatch。SOPD 的动机是将 SFT 的长程校正与 OPD 的 on-policy 优势结合：在学生生成的完整轨迹上，在每个自然步骤处向教师查询该步骤的目标输出，教师条件包含学生此前实际做出的所有决策，然后优化 step-balanced cross-entropy 损失。从理论上，SOPD 在不同 step length 极限下会退化为 SFT 或近似 OPD，表明它是两种方法的插值。相比 SFT，教师响应条件化于学生轨迹，与学生访问状态更一致；相比 OPD，它提供更长视野的步骤级校正而非碎片化 token 指导。同时，SOPD 天然适合黑盒蒸馏，因为只需教师生成目标文本，不要求概率访问。实验在推理任务和智能体任务上进行，结果显示 SOPD 显著优于传统 SFT 和 OPD，尤其是在 ALFWorld 上比 Vanilla OPD 提升 13.4 个百分点平均成功率。论文希望为蒸馏方法研究提供新视角。整体上，SOPD 的贡献在于提出一个新的监督插值框架，并展示了其在推理和智能体场景中的有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中 agent 方向直接相关（权重 0.10），可用于 LLM 智能体训练中的蒸馏和监督信号设计。

## 基本信息

- 作者：Changhui Sun, Lanbo Liu, Hang Lei, Tong Ling, Jiahang Xie, Zhiyong Zheng, Yujia Wang, Hao Liu, Feng Xiao, Lu Liu, Yanlong Du, Zifeng Cheng, Ziwei Jiang, Qing Gu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-08-18
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.16333`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要、Introduction 与 Related Work 的语义检索片段，并结合启发式草稿进行补全；由于全文未完整提供，部分实验细节以合理推断或推测标注，需回原文核对。
