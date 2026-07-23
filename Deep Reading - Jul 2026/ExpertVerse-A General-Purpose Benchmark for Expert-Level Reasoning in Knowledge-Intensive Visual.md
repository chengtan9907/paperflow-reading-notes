---
user_id: "cheng tan"
paper_id: 4970
arxiv_id: "2607.19341"
title: "ExpertVerse: A General-Purpose Benchmark for Expert-Level Reasoning in Knowledge-Intensive Visual Synthesis"
publish_date: "2026-07-22"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19341.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19341"
abs_url: "https://arxiv.org/abs/2607.19341"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-22T13:54:54"
---
# ExpertVerse: A General-Purpose Benchmark for Expert-Level Reasoning in Knowledge-Intensive Visual Synthesis

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：expertverse · knowledge-intensive generation · visual reasoning · benchmark

## 一句话总结

ExpertVerse是一个面向知识密集型视觉合成中专家级推理的能力导向基准，涵盖9种认知能力×8个专家学科的58个子学科，并配备大规模推理数据集ExpertVerse-100K和基于强化学习的推理引擎KnowThinker。

## 摘要

> Recent advances in multimodal generative models have enabled instruction-based image generation to move beyond semantic manipulation to knowledge-driven visual reasoning. However, these methods focus on explicit commonsense reasoning, shallow causal understanding, and direct knowledge recall, failing at knowledge-intensive generation. We develop \textbf{ExpertVerse}, a capability-centric benchmark to evaluate generative models via knowledge-intensive lens. ExpertVerse stratifies reasoning generation across an orthogonal taxonomy of \textit{9 cognitive capabilities} and \textit{8 expert disciplines}, yielding \textit{58 sub-disciplines}. We curate 1,611 expert-annotated instances covering single-image editing, multi-image composition, and text-to-image generation. We further develop an automated workflow to produce \textbf{ExpertVerse-100K}, a large-scale dataset with reasoning traces and knowledge-anchored rationale annotations. Based on this, we train \textbf{KnowThinker} with RL fine-tuning, a VLM reasoning engine with world knowledge that jointly generates thinking processes and refined instructions. Towards the cross-modal credit misalignment and multi-objective gradient conflicts in multi-reward optimization, we propose a tailored Bootstrapped Pareto Policy Optimization (BPPO), which synergizes Bootstrapping Reward Rectification (BRR) and Conflict-Aware Pareto Advantage Fusion (CPAF). Extensive results of both open-source and proprietary models exposes critical reasoning deficits, highlighting imperative for knowledge-intensive benchmarks towards next-generation visual generation.

Q1: 这篇论文试图解决什么问题？

当前多模态生成模型在基于指令的图像编辑和生成方面取得了显著进展，但当任务需要专家级领域知识、结构化认知规划和深层因果推理时，模型往往生成违背物理常识或逻辑连贯性的内容。现有基准如RISEBench、KRISBench和UniREditBench主要评估显式常识、浅层因果和直接知识回忆，且局限于单图像操作，缺乏对知识密集型综合生成的全面考核。这导致模型在实际应用中难以应对需要跨学科知识整合和复杂推理的场景，如科学可视化、医学成像或工程制图。因此，需要一个能力导向的基准，系统性地评估生成模型在知识密集型任务中的推理能力。

Q2: 有哪些相关研究？

相关工作包括指令式图像编辑（如InstructPix2Pix）、多模态模型（如GPT-4V、Gemini）和推理基准。RISEBench引入假设推理维度，涵盖时间、空间、因果和逻辑推理；KRISBench提出基于知识的分类，评估事实、概念和程序知识；UniREditBench扩展到游戏世界和多对象交互。这些基准推动了推理评估，但主要依赖显式常识和单一图像操作，知识深度和广度有限。此外，多图像合成和文本到图像生成中的推理评估不足。ExpertVerse旨在填补这些空白，通过细粒度的认知-学科矩阵覆盖专家级推理。

Q3: 论文如何解决这个问题？

ExpertVerse基准构建基于一个正交分类矩阵：9种认知能力（包括假设推理、因果分析、类比迁移、反事实推理、抽象归纳、推理链、约束满足、空间推理和时序推理）和8个专家学科（涵盖物理、化学、生物、地理、历史、艺术、工程和数学），共产生58个有效结合的子学科。数据收集采用层级扩展策略：首先由专家定义种子参考提示（包含图像描述、用户指令和领域解释），然后利用专有VLM自动扩增，并经两阶段质量保证。共构建1,611个专家标注实例，覆盖单图像编辑、多图像合成和文本生成。进一步开发自动化工作流生成ExpertVerse-100K数据集，包含推理轨迹和知识锚定理由。KnowThinker模型采用‘思考-执行’架构：一个VLM推理引擎（思考器）生成知识锚定的步骤计划和精化指令，然后传递给冻结的视觉编辑器（执行器）生成图像。训练使用强化学习，提出BPPO算法，包含两个新颖组件：自举奖励校正（BRR）校准跨模态信用分配，冲突感知帕累托优势融合（CPAF）处理多目标梯度冲突。BPPO通过迭代优化最大化图像奖励和推理链奖励。

Q4: 论文做了哪些实验？

实验评估包括三部分：（1）在ExpertVerse基准上评测开源和专有模型，包括Stable Diffusion系列、Midjourney、DALL-E、Gemini等，采用四维评估协议：视觉质量（FID等）、输入一致性（CLIP score）、知识推理（专家判断+自动评估）和逻辑连贯性（新引入指标）。（2）在现有基准（如RISEBench、KRISBench、UniREditBench）上对比KnowThinker与直接生成方法。（3）消融研究分析BPPO组件、奖励设计、思考器精度等。实验设置详细说明数据划分、评估协议和基线。

Q5: 发现了什么实验现象？

关键发现：（1）现有模型在知识推理和逻辑连贯性维度表现显著低于视觉质量维度，揭示能力偏差。（2）跨模态信用误对齐问题严重：即使图像视觉质量高，推理链却错误（如概念混淆、物理矛盾）。（3）KnowThinker在所有基准上取得SOTA，尤其在知识推理指标上大幅领先，证明思考-执行架构的有效性。（4）BPPO组件消融显示BRR和CPAF均有贡献，CPAF对多目标平衡至关重要。（5）一个反直觉现象是：更大模型在简单常识任务上表现更好，但在专家级别推理上未必，显示scaling law对知识推理不直接适用。（6）失败案例包括模型在涉及化学结构或历史年代细节的任务上生成错误，表明当前数据覆盖可能仍不足。

Q6: 有什么可以进一步探索的点？

未来工作可沿以下方向扩展：（1）扩充ExpertVerse学科覆盖范围，增加医学、法律等专业领域。（2）将评估扩展到视频生成或3D合成。（3）探索更有效的奖励设计，利用过程奖励模型进行中间步骤监督。（4）结合外部知识库（如维基百科）增强推理基础。（5）将KnowThinker应用于下游任务如教育可视化、科学图表生成。（6）研究推理链的可解释性和错误校正机制。（7）探索更高效的训练策略减少对大规模推理数据的依赖。

Q7: 总结一下论文的主要内容

本文提出ExpertVerse基准，旨在全面评估视觉生成模型在知识密集型任务中的推理能力。作者首先分析现有基准的局限，设计了一个包含9种认知能力和8个专家学科的58子学科分类法，并构建了1,611个专家标注实例和100K规模数据集。针对模型在知识推理中的不足，提出了KnowThinker模型，采用解耦的思考-执行架构，将推理规划与视觉生成分离。为训练推理引擎，开发了BPPO算法，创新地解决跨模态信用对齐和多目标优化冲突。大量实验表明，当前主流模型在知识推理上存在显著缺陷，而KnowThinker取得了领先性能。该工作为知识感知视觉合成提供了系统性的评估方法和解决思路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接涉及生成任务（用户画像中生成权重0.10），提供了评估和提升生成模型推理能力的系统框架。

## 基本信息

- 作者：Yuan Wang, Yongchao Du, Mengting Chen, Jinsong Lan, Xuetao Feng, Xiaoyong Zhu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-22
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19341`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要、引言、结论和评估指标片段。
