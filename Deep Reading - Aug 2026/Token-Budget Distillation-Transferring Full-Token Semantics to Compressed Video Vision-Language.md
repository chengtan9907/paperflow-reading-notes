---
user_id: "cheng tan"
paper_id: 9650
arxiv_id: "2608.28138"
title: "Token-Budget Distillation: Transferring Full-Token Semantics to Compressed Video Vision-Language Models"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.28138.pdf"
pdf_url: "https://arxiv.org/pdf/2608.28138"
abs_url: "https://arxiv.org/abs/2608.28138"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:13:29"
---
# Token-Budget Distillation: Transferring Full-Token Semantics to Compressed Video Vision-Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video vision-language model · token compression · knowledge distillation · parameter-efficient fine-tuning

## 一句话总结

本文提出 Token-Budget Distillation（TBD）：在固定视觉 token 预算下，冻结视频 VLM 主干、仅更新 LoRA，并通过全 token 教师到压缩学生的双路蒸馏（任务损失 + 答案区 KL 蒸馏 + 基于 GT 的 margin 蒸馏 + 可靠性感知 KD 控制），让压缩后的学生模型恢复接近全 token 模型的语义与精度。

## 摘要

> Adapting video vision-language models (VLMs) is computationally expensive because video inputs produce a large number of visual tokens, making both fine-tuning and inference costly. Although visual token compression can reduce this overhead, direct adaptation on compressed inputs often causes semantic drift and noticeable performance degradation. We present Token-Budget Distillation (TBD), a parameter-efficient fine-tuning framework for adapting video VLMs under a fixed token budget. TBD freezes the pretrained backbone, updates only LoRA adapters, and integrates FlashVID-based visual token compression into the video pathway. To preserve full-token semantics under compression, TBD employs a dual-path teacher-student design, where a full-token teacher provides stable supervision and a compressed student is optimized with task loss, answer-region KL distillation, GT-anchored margin distillation, and reliability-aware KD control. This design enables the student to recover the semantic behavior of the full-token model while remaining efficient under aggressive token reduction. We evaluate TBD on three video VLM backbones, including LLaVA-Video, LLaVA-OneVision, and Qwen3-VL-8B-Instruct, across four video understanding benchmarks. TBD consistently outperforms compression-only baselines under both moderate and aggressive compression. On LLaVA-Video at retention ratio R = 10 percent, TBD preserves 97.0 percent of the Vanilla model's average accuracy; on LLaVA-OneVision at R = 10 percent, it achieves an average score of 58.4 and matches 100.0 percent relative accuracy.

Q1: 这篇论文试图解决什么问题？

1. 直接问题：视频 VLM 的输入是视频帧序列，转化为视觉 token 后数量巨大，导致全参数微调和推理都昂贵。
2. 已有缓解手段的缺陷：视觉 token 压缩可以减少开销，但直接在压缩后的输入上微调会产生语义漂移（semantic drift）和明显的性能下降。也就是说，压缩节省了计算，却破坏了模型对视频内容的语义理解。
3. 核心矛盾：在“固定 token 预算”这一硬约束下，既要参数高效（只更新 LoRA），又要让压缩学生模型的行为尽量接近全 token 模型。压缩率越高（如 R = 10%），可用的视觉信息越少，语义保持越难。
4. 深层难点（合理推断）：token 压缩会丢掉细粒度时空线索，造成训练与推理分布不一致；单纯让学生在压缩输入上拟合任务标签，容易让模型学会“压缩伪影”而非真正的语义；从全 token 教师做蒸馏时，教师信号本身也可能包含噪声或不可靠区域，需要可靠性感知的控制机制。
5. 论文要回答的问题：在固定 token 预算下，如何设计学生模型的监督信号，使得压缩后的视频 VLM 既能保持效率，又能恢复接近全 token 模型的语义行为？

Q2: 有哪些相关研究？

1. 视觉 token 压缩/剪枝：摘要和实验部分明确提到的 baseline 包括 FlashVID、FastVID、VisionZip、FastV。这些方法属于 compression-only 路线：它们压缩视觉 token 以降低计算，但不对压缩带来的语义损失做显式补偿。FastV 通常对应 token pruning 思路，VisionZip 偏语义 token 选择，FastVID/FlashVID 可看作更进一步的视频 token 压缩方案；具体机制在检索到的片段中未展开，属合理推断。
2. 参数高效微调（PEFT）：TBD 使用 LoRA adapters，只更新少量参数，属于 PEFT 家族。这与全参数微调相比成本更低，也是本文“固定 token 预算”之外的另一层效率设计。
3. 知识蒸馏（KD）：TBD 采用双路 teacher-student 设计，涉及 KL 蒸馏、margin 蒸馏和可靠性感知 KD 控制。这连接了经典 KD 与视频 VLM 压缩两个方向，但把蒸馏对象从“大模型教小模型”扩展为“全 token 模型教压缩 token 模型”。
4. 视频 VLM 主干：实验覆盖 LLaVA-Video、LLaVA-OneVision、Qwen3-VL-8B-Instruct，说明该项工作面向当前主流开源视频理解模型。
5. 视频理解基准：摘要说用了四个视频理解基准，introduction 片段中可见 LongVideoBench [38]；其余三个基准的名称未在检索证据中完整出现。
6. 现有工作的缺口：compression-only 方法不能解决语义漂移；TBD 的定位是在压缩之上叠加蒸馏式语义迁移，且与 PEFT 结合。

Q3: 论文如何解决这个问题？

TBD 的整体框架是“固定 token 预算 + 参数高效微调 + 双路师生蒸馏”三者的组合。
1. 固定 token 预算：实验采用保留比例 R = 20%（中等压缩）和 R = 10%（激进压缩），即只保留原视觉 token 的 10% 或 20%。
2. 参数高效微调：冻结预训练视频 VLM 主干，只更新 LoRA 适配器，避免全参数微调的显存和存储开销。
3. 压缩模块：在视频通路中集成基于 FlashVID 的视觉 token 压缩。检索证据未说明该模块是否可训练、是否与 LoRA 联合更新，需回原文确认（推测为固定或轻量可学习的压缩前置模块）。
4. 双路师生设计：
 - 全 token 教师（full-token teacher）接收完整视觉 token，提供稳定的语义监督；
 - 压缩学生（compressed student）接收压缩后的 token，在固定预算下执行任务；
 - 学生通过多种损失逼近教师的行为。
5. 损失函数四部分（名称来自摘要，具体公式未在检索证据中给出）：
 - 任务损失：标准的下游任务监督，让模型学会正确回答；
 - 答案区域 KL 蒸馏：只在 answer 区域计算学生与教师的 KL 散度，重点对齐生成答案的分布，而不是全序列无差别对齐（合理推断）；
 - GT 锚定的 margin 蒸馏：以 ground truth 为锚，要求学生模型在正确答案与错误答案之间保持足够 margin，可能同时借鉴教师输出的 margin 信息（合理推断）；
 - 可靠性感知 KD 控制：根据教师或学生预测的可靠性动态调整蒸馏权重，避免不可靠的教师信号误导学生（推测）。
6. 设计意图：任务损失保障任务正确性，KL 蒸馏对齐语义分布，margin 蒸馏强化判别边界，可靠性控制降低噪声监督。四者共同缓解激进压缩下的语义漂移。

Q4: 论文做了哪些实验？

1. 模型骨干：LLaVA-Video、LLaVA-OneVision、Qwen3-VL-8B-Instruct 三个视频 VLM。
2. 基准：四个视频理解 benchmark；检索证据中只明确看到 LongVideoBench [38]，其余三个名称未在片段中给出。
3. 压缩设置：中等压缩 R = 20% 和激进压缩 R = 10%。
4. 对比方法：Vanilla（全 token，不压缩）以及 compression-only baselines：FlashVID、FastVID、VisionZip、FastV；TBD 为本文方法。
5. 主要数值（来自检索命中的结果片段）：
 - LLaVA-Video 上，TBD 在 R = 20% 和 R = 10% 下都是压缩变体中平均分最高的；
 - LLaVA-Video R = 20%：TBD 平均分 59.7，FlashVID 59.3，FastVID 58.7，VisionZip 59.0，FastV 数值在片段中被截断；
 - LLaVA-Video R = 10%：TBD 保留 Vanilla 平均精度的 97.0%；
 - LLaVA-OneVision R = 10%：TBD 平均分 58.4，FlashVID 57.9，FastVID 57.1，VisionZip 54.0，FastV 53.6，相对 Vanilla 的精度保持率达 100.0%；
 - Qwen3-VL-8B-Instruct 也有实验，但检索证据未包含其具体结果。
6. 证据缺口：缺少逐 benchmark 分数、消融实验、训练开销、推理延迟、压缩后 token 实际数量等细节，需回原文核对。

Q5: 发现了什么实验现象？

1. TBD 在两个压缩率下都取得压缩变体中的最佳平均分，说明“压缩 + 蒸馏”的组合优于单纯压缩。
2. 激进压缩下语义恢复效果显著：LLaVA-OneVision 在 R = 10% 时达到 100.0% 相对精度，LLaVA-Video 在 R = 10% 时保留 97.0% 的 Vanilla 平均精度，表明蒸馏可以几乎完全抵消 token 预算骤减带来的语义损失。
3. Compression-only baseline 在激进压缩下退化明显：LLaVA-OneVision R = 10% 时 VisionZip（54.0）和 FastV（53.6）明显低于 TBD（58.4），说明仅靠压缩策略无法保持语义。
4. 不同压缩方法对压缩率的敏感度不同：在 LLaVA-Video R = 20% 时 VisionZip 为 59.0，尚接近 TBD 的 59.7；但在 LLaVA-OneVision R = 10% 时 VisionZip 降至 54.0，与 TBD 差距拉大。当然这涉及不同骨干，不能直接跨模型比较，但至少提示激进压缩会放大方法间差异。
5. 检索证据中未出现消融趋势、失败案例、指标间张力或负结果；这些信息需从原文 Experiments/Discussion 部分获取。

Q6: 有什么可以进一步探索的点？

1. 更极端的 token 预算：探索 R = 5% 甚至更低时的语义保持极限，以及 TBD 的损失设计是否需要随预算变化。
2. 压缩模块与蒸馏的联合学习：当前是 FlashVID-based 压缩 + 蒸馏，若把压缩器也设计为可学习并与 LoRA 联合优化，可能进一步提升精度-效率权衡。
3. 可靠性感知机制的具体化：研究如何定义教师/学生的可靠性（如置信度、熵、token 重要性），动态调整 KD 权重，并给出理论或经验分析。
4. 扩展到更长视频和流式输入：LongVideoBench 已覆盖长视频，可进一步研究 TBD 在在线/流式视频理解中的表现。
5. 与其他 PEFT 结合：LoRA + 量化、prefix tuning、adapter 等，进一步降低部署成本。
6. 系统级评估：补充 FLOPs、推理延迟、显存占用、吞吐量等实际效率指标，而不仅是 token 保留率。
7. 跨领域迁移：将“压缩 + 蒸馏 + 可靠性控制”范式用于多模态 agent 的视觉上下文管理，或用于科学数据（如视频观测、时序影像）的轻量化理解。
8. 理论分析：对 margin distillation 为何能恢复全 token 语义、语义漂移如何被量化给出更形式化的解释。

Q7: 总结一下论文的主要内容

1. 问题背景：视频 VLM 的视觉 token 数量大，微调和推理成本高；token 压缩能降本，但直接适配压缩输入会导致语义漂移和性能下降。
2. 方法核心：TBD 是一个参数高效微调框架，在固定 token 预算下冻结主干、只更新 LoRA，并集成 FlashVID 视觉 token 压缩。它采用双路师生结构：全 token 教师提供稳定监督，压缩学生通过任务损失、答案区域 KL 蒸馏、GT 锚定的 margin 蒸馏和可靠性感知 KD 控制来逼近教师语义。
3. 实验设置：三个视频 VLM 骨干（LLaVA-Video、LLaVA-OneVision、Qwen3-VL-8B-Instruct）、四个视频理解基准（可见 LongVideoBench）、两种压缩率（R = 20% 和 R = 10%），对比 Vanilla 与 FlashVID/FastVID/VisionZip/FastV 等 compression-only baseline。
4. 主要结果：TBD 在所有设置下都是压缩变体中的最优；LLaVA-Video R = 10% 保留 97.0% 的 Vanilla 平均精度；LLaVA-OneVision R = 10% 平均分 58.4，相对精度 100.0%；在 LLaVA-Video R = 20% 平均分 59.7，高于 FlashVID 59.3、VisionZip 59.0、FastVID 58.7 等。
5. 结论：通过 token 压缩、LoRA 和可靠蒸馏的协同，压缩学生可以恢复全 token 模型的语义行为，同时保持效率。
6. 证据说明：本次精读主要基于摘要、introduction 和 main results 的检索片段；方法细节、完整消融、Qwen3-VL 具体结果、逐基准分数和 limitations 原文尚未获得，相关判断已标注为合理推断或推测。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：核心问题是视频 VLM 的 token 压缩与语义保持，与多模态 agent 中视频输入成本高、上下文受限的问题直接相关。

## 基本信息

- 作者：Xiaoyang Guo, Guoping Luo, Jusheng Zhang, Keze Wang, Wenhao Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.28138`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、introduction、conclusion、main results 片段），并对未检索到的细节做了明确标注的合理推断或推测。
