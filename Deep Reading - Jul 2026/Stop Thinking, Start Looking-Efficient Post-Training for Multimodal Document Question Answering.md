---
user_id: "cheng tan"
paper_id: 4410
arxiv_id: "2607.14682v1"
title: "Stop Thinking, Start Looking: Efficient Post-Training for Multimodal Document Question Answering via Reasoning-Free Alignment"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14682v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14682v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:59:05"
---
# Stop Thinking, Start Looking: Efficient Post-Training for Multimodal Document Question Answering via Reasoning-Free Alignment

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：document visual grounding · reinforcement learning · GRPO · multimodal QA

## 一句话总结

本文提出Perception-RFT框架，利用组相对策略优化(GRPO)进行多模态文档视觉定位训练，直接对齐视觉特征与结构化输出，无需中间推理，在4B参数规模下减少推理token超过60%，并发现推理增强的强化学习性能反而不如直接感知训练。

## 摘要

> Efficient multimodal document question answering with explicit visual grounding, locating the precise document region that supports each answer remains an open challenge. Current approaches bifurcate into Supervised Fine-Tuning (SFT), which requires large annotated datasets and reaches optimization plateaus, and reasoning-centric Reinforcement Learning (RL), which depends on verbose intermediate traces that inflate inference token cost without clear benefit. We introduce Perception-RFT, a training framework that applies Group Relative Policy Optimization (GRPO) to multimodal document QA, bypassing intermediate reasoning tokens to directly align visual features with structured grounding outputs. To rigorously evaluate the necessity of reasoning, we construct a reasoning variant under identical reward settings. We find that reasoning-enabled models suppress their reasoning traces during training, converging to direct perception-based policies at the 4B parameter scale, reducing per-query inference token length by more than 60%, while reasoning-enabled RL underperforms perception-only training. Through a fine-grained analysis of Qwen3-VL-4B optimization dynamics, we confirm that SFT saturation and cold-start RL instability established in text-domain post-training extend to multimodal, and identify a previously uncharacterized Grounding Divergence: a selective trade-off between semantic robustness and geometric precision on two out of distribution (OOD) benchmarks (4,828 samples) under joint RL optimization. We further show that an early SFT$\rightarrow$RL transition achieves comparable precision with 65% less training data.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决多模态文档问答中显式视觉定位（Document Visual Grounding, DVG）的训练效率与必要性争论问题。具体而言：(1) 现有基于SFT的方法需要大量人工标注的定位数据，且训练容易达到性能饱和，无法进一步提升；(2) 基于推理的RL方法（如引入Chain-of-Thought中间推理）虽然直观，但会产生大量额外推理token，在推理时显著增加计算成本，且中间推理是否真正有助于定位精度尚无定论。因此，论文的核心研究问题是：在多模态文档定位的RL训练框架下，显式推理是否必要？能否通过直接对齐视觉特征与定位输出（推理自由）实现更高效且性能不降甚至更优的训练？

Q2: 有哪些相关研究？

相关工作涵盖三个主要方向：(1) 文档视觉问答与定位：DocVQA、DUDE、DOGR-Bench等数据集和基线方法推动了文档理解，但大多数方法依赖SFT，未系统探索后训练阶段的RL优化。(2) 后训练方法：文本域中的RLHF、GRPO以及SFT+RL两阶段训练已被广泛研究，Guo等人（2025）发现SFT饱和和冷启动RL不稳定性等动力学现象。本文将这些动力学验证扩展到多模态视觉场景。(3) 推理增强视觉模型：如Vision-R1、R1等多模态变体尝试将推理链引入视觉问答，但本文首次通过控制实验隔离推理的作用，并得出在文档定位中推理非但无益反而有害的结论。

Q3: 论文如何解决这个问题？

论文提出Perception-RFT框架，其核心是使用Group Relative Policy Optimization (GRPO) 进行面向文档视觉定位的后训练。训练过程中，模型直接由输入文档图像和问题输出结构化的定位坐标（如边界框），不产生任何中间推理token。为了评估推理的必要性，论文在同一reward设置下构建了两个变体：RFTb（无推理，直接输出坐标）和Reasoning-RFTb（先输出推理链再输出坐标）。两个变体共享相同的奖励函数，该函数基于定位准确度（如IoU）和格式正确性。训练基座模型为Qwen3-VL-4B，在内部金融文档数据集（In-Domain, ID）和两个公开分布外(OOD)基准（DOGR-Bench、MMDocBench）上进行冷启动RL训练（即从预训练模型直接开始RL，无中间SFT）。通过这种可控比较，系统隔离了推理对优化动态和最终性能的影响。

Q4: 论文做了哪些实验？

论文进行了以下主要实验：(1) 主实验：在ID (Finance) 和两个OOD基准（DOGR-Bench、MMDocBench）上，比较基座模型、SFT、无推理RL (RFTb) 和有推理RL (Reasoning-RFTb) 的定位精度和推理token长度。(2) 优化动态分析：跟踪训练过程中奖励和定位指标的变化，验证SFT饱和现象（SFT前期快速提升后停滞）和冷启动RL不稳定性（RL初期波动后稳定提升）在视觉领域的复现。(3) 推理作用消融：比较RFTb和Reasoning-RFTb的最终性能，以及训练过程中推理token长度的演化。(4) 定位发散分析：在OOD基准上，按任务语义（如表格结构理解、标题识别）和几何精度（如边界框对齐）拆分子指标，发现两者在联合RL优化下存在反向波动。(5) 数据效率实验：对比不同SFT→RL转换时机（训练数据比例），发现早期SFT（仅使用35%数据）后接RL可达到与完整训练相当的精度，减少65%数据。实验全部使用Qwen3-VL-4B模型，评估指标包括定位准确率和推理token数。

Q5: 发现了什么实验现象？

实验观察到的关键现象如下：(1) 推理抑制：Reasoning-RFTb在训练过程中自发生成的推理token数量逐渐减少，最终接近RFTb的无推理输出模式，表明模型在优化压力下主动抛弃冗余推理。(2) 性能悖论：在ID数据上，Reasoning-RFTb与RFTb最终精度接近，但前者token长度是后者的2.5倍以上；在OOD数据上，Reasoning-RFTb的精度显著低于RFTb，且训练早期达到峰值后便退化，而RFTb持续提升至稳定平台。(3) SFT饱和与RL持续进步：SFT在有限数据下快速饱和，而RFTb在更长时间训练中能持续提升精度，证实文本域的动力学在多模态中成立。(4) 冷启动RL不稳定：从预训练模型直接启动RL时，初期指标波动大，但最终收敛到优于SFT的水平。(5) 定位发散：在OOD评估中，语义任务（如文档分类）和几何任务（如边界框回归）的指标出现相互冲突的趋势，例如随着训练增加，语义F1上升但几何IoU下降，或反之，表明共享表征在联合优化下难以同时满足两种目标。(6) 数据效率优势：仅使用35%的SFT数据后转入RL，即可达到使用全部数据训练的定位精度，证实了早期过渡策略的高效性。

Q6: 有什么可以进一步探索的点？

基于当前发现，未来可以探索以下方向：(1) 定位发散机制：深入分析语义与几何表征冲突的根源，设计多任务奖励或解耦训练来缓解这种权衡。(2) 推理的边界条件：在需要多步隐含推理的任务（如“盖了章的文件是哪一份？”）上重新评估推理是否必要，探索推理在不同视觉推理难度下的作用。(3) 更大规模扩展：在7B、14B甚至更大多模态模型上验证本文结论的普遍性，以及Perception-RFT的扩展性。(4) 更丰富的奖励设计：引入任务难度自适应奖励，或结合视觉语言模型的其他目标（如OCR辅助）以进一步提升泛化。(5) 跨任务迁移：将Perception-RFT应用于其他文档智能任务（如表格结构识别、票据解析）甚至自然图像定位任务。(6) 理论解释：从信息论或表征学习角度解释为何无推理策略在文。档定位中表现更优。

Q7: 总结一下论文的主要内容

本文聚焦于多模态文档问答中的显式视觉定位（DVG）问题，目标是在不牺牲精度的情况下实现高效的训练。当前主流方法包括监督微调（SFT）和基于推理的强化学习（RL），但前者容易饱和且依赖大量标注，后者则因冗长的推理链导致高昂的推理成本，且其必要性存疑。针对这一矛盾，作者提出Perception-RFT，一种基于Group Relative Policy Optimization (GRPO) 的推理自由训练框架。其核心思路是直接优化视觉特征到结构化定位输出（如边界框）的映射，完全不产生中间推理token。为了系统评估推理的作用，论文在同一奖励设置下构建了两个对比训练流程：无推理的RFTb和有推理的Reasoning-RFTb。实验采用Qwen3-VL-4B作为基座模型，在内部金融文档数据集（ID）和两个公开分布外基准（DOGR-Bench、MMDocBench）上进行冷启动RL训练。主实验表明，无推理的RFTb在定位精度上与推理变体相当甚至更优，而推理token长度减少超过60%。进一步的分析揭示了几个关键发现：(1) 推理抑制：Reasoning-RFTb在训练过程中自发减少推理链长度，显示优化驱动模型转向直接感知。(2) 动力学迁移：在文本域中观察到的SFT饱和和冷启动RL不稳定性在多模态视觉定位中同样成立。(3) 定位发散：在OOD数据上，语义理解与几何精度两个维度的指标在联合RL优化下表现出选择性权衡，即提升一方会损害另一方，形成一种此前未表征的权衡模式。(4) 数据效率：早期SFT（仅使用约35%的数据）后过渡到RL训练，可以达到与完整训练相当的精度，显著降低了标注需求。总体而言，本文通过严谨的控制实验系统论证了在多模态文档定位中显式推理的非必要性，并提出了一种高效、低成本的训练范式。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接相关领域：多模态文档问答、视觉定位、后训练方法，本文为文档智能提供了一种高效训练范式。

## 基本信息

- 作者：Harikrishnan P M, Goutham Vignesh, Ganesh Parab, Saisubramaniam Gopalakrishnan, Vishal Vaddina, Varun V, Rohit Agrawal
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14682v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要基于PDF语义检索到的摘要、方法、结果等证据片段，并结合heuristic_draft推断完成，部分细节可能需回溯原文确认。
