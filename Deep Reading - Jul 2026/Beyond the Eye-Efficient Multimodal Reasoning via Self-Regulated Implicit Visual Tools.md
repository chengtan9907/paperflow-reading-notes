---
user_id: "cheng tan"
paper_id: 3639
arxiv_id: "2607.11106v1"
title: "Beyond the Eye: Efficient Multimodal Reasoning via Self-Regulated Implicit Visual Tools"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11106v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11106v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:41:27"
---
# Beyond the Eye: Efficient Multimodal Reasoning via Self-Regulated Implicit Visual Tools

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal reasoning · implicit visual tool · self-regulation · net tool gain

## 一句话总结

提出BEE，一种基于自我调节隐式视觉工具的多模态推理范式，通过两阶段训练和净工具增益度量，在降低推理延迟的同时实现细粒度感知SOTA。

## 摘要

> Recent multimodal large language models (MLLMs) have made remarkable progress on fine-grained perception tasks under the "Thinking with Images" (TwI) paradigm by iteratively performing various visual tool operations. However, this paradigm relies heavily on frequent external tool calls and repeated image re-encoding, which leads to substantial computational overhead and inference latency. To address these issues, we propose Beyond the Eye (BEE), a novel implicit visual tool paradigm centered on self-regulated capability. BEE directly incorporates visual tool invocation behaviors into the training objective and encourages the model to develop a self-regulated invocation mechanism. This design enables the model to adaptively balance internal knowledge and implicit tools, avoiding redundant tool usage while substantially reducing inference latency. Specifically, BEE involves a two-stage training process: (1) Formalized Chain-of-Thought (CoT) Supervised Fine-tuning (SFT). We construct CoT trajectories with structured tool slots and mixed invocation states. This stage activates the model's implicit tool representations and adaptive switching capability. (2) Self-regulated Reward-Driven Alignment. To address redundant tool usage caused by ambiguous cognitive boundaries, we first introduce the Net Tool Gain (NTG) metric to quantify this phenomenon. Based on this observation, we further propose a self-regulated reward mechanism. This mechanism penalizes ineffective tool dependency and encourages the model to perform knowledge routing, ensuring that implicit tools are invoked only when the model's internal knowledge is insufficient. BEE achieves state-of-the-art performance in fine-grained visual perception while remaining competitive in general reasoning tasks and achieving substantial gains in inference efficiency.

Q1: 这篇论文试图解决什么问题？

当前多模态大模型在细粒度视觉感知中广泛采用'Thinking with Images'范式，即通过迭代调用外部视觉工具（如物体检测、OCR、姿态估计等）并进行图像重编码来提取细节信息。这种方法虽然有效，但存在两个关键问题：(1) 频繁的外部工具调用和重复的图像编码带来巨大的计算开销和推理延迟，限制了实际应用；(2) 模型缺乏自适应性，无法区分何时依赖内部知识、何时需要调用外部工具，导致冗余工具使用和效率低下。本文旨在通过引入自我调节的隐式视觉工具来解决这些问题，使模型能够在保持甚至提升感知性能的同时大幅提高推理效率。

Q2: 有哪些相关研究？

相关研究大致分为两类：基于显式工具调用的多模态推理方法和隐式视觉工具方法。显式工具方法（如VisProg、ViperGPT、MM-ReAct等）通过程序合成或思维链调用外部视觉模型，实现了细粒度感知和推理的可解释性，但效率瓶颈明显。另一类方法（如Monet、EVA）尝试用连续隐式视觉表示替代显式工具调用，将外部模型的能力内化到LLM中，显著提升推理效率，但可能损失部分可解释性，且模型仍无法自适应调节工具调用。此外，认知边界学习、奖励对齐等方法也被用于优化工具使用。BEE属于隐式工具范式，但进一步通过自我调节机制和奖励对齐解决了冗余调用问题，同时保持了推理可解释性。

Q3: 论文如何解决这个问题？

BEE的整体框架是一种两阶段训练范式。第一阶段：形式化CoT监督微调。该阶段构造了结构化的CoT轨迹，每个推理步骤包含一个'工具槽'，用于指示是否调用隐式工具以及调用何种工具（例如检测、计数、OCR等）。轨迹混合了工具调用状态和直接知识回答状态，使模型学习隐式工具表征，并培养在内部知识充分时跳过调用的自适应切换能力。第二阶段：自我调节奖励驱动对齐。首先定义净工具增益（NTG）指标，衡量工具调用相对于基线（仅使用内部知识）的边际贡献。基于NTG设计奖励函数，当工具调用带来正面增益时给予正奖励，无效或负增益时给予惩罚。通过强化学习（如PPO）优化，鼓励模型在内部知识不足时才调用工具，否则依赖自身知识，从而形成自我调节的调用边界。训练完成后，推理时模型以端到端方式生成回答，隐式工具调用被内化在生成过程中，无需外部API调用。

Q4: 论文做了哪些实验？

论文在多个基准上进行了全面评估。感知任务：在MME-Real等细粒度感知基准上与当前最优的显式工具方法（如Set-of-Mark、ViperGPT）和隐式方法（Monet、EVA）进行比较。推理任务：在MMMU等一般复杂推理基准上评估。效率测量：记录不同分辨率下的推理延迟。主要基线包括Qwen2-VL-7B、InternVL2-8B等基础MLLM以及显式/隐式工具增强模型。实验设置遵循标准协议。此外还进行了消融研究，验证SFT和对齐阶段的有效性，并分析了NTG度量的行为。

Q5: 发现了什么实验现象？

实验发现：(1) BEE在大多数细粒度感知基准上（如MME-Real、ColonCancer等）达到了SOTA，尤其在高分辨率设置下优势明显。(2) 在一般推理基准上，BEE与最强的基础MLLM保持持平或略有提升，表明隐式工具机制并未损害推理能力。(3) 效率方面，BEE对比显式工具方法在8K分辨率下延迟降低86.2%。(4) NTG度量分析显示，基础模型存在大量低效工具调用，而经过自我调节训练后，工具调用频次减少且每次调用的平均增益提高。(5) 消融表明，两阶段训练都不可或缺：SFT激活了工具表示，而对齐阶段进一步优化了调用策略。值得注意的是，在MME-Real基准上BEE未超越某个特定基线（如ZwZ），暗示在极端精细的感知任务中可能仍有提升空间。

Q6: 有什么可以进一步探索的点？

BEE展示了隐式工具范式的潜力，但仍有许多可探索的方向：(1) 增强复杂推理和泛化能力，特别是在需要多步推理和逻辑链的场景中，自我调节机制可能进一步优化。(2) 探索更细粒度的工具类型，将更多样化的视觉能力（如深度估计、运动检测）纳入隐式工具集。(3) 将BEE扩展到视频、3D等多模态任务。(4) 研究模型规模对自调节能力的影响，BEE是否在更大模型上更易收敛。(5) 结合本地部署约束，进一步压缩模型实现超低延迟推理。(6) 在安全性和偏好对齐方面，可能需要处理工具调用带来的偏见或幻觉风险。

Q7: 总结一下论文的主要内容

本文提出了一种通过自我调节隐式视觉工具实现高效多模态推理的新范式BEE。首先分析了当前“Thinking with Images”范式的不足：频繁调用外部视觉工具（如检测、OCR）和重复图像编码导致高延迟和计算开销。BEE的核心思想是将工具调用行为融入模型训练，使模型学会自适应地平衡内部知识和隐式工具调用。方法包括两阶段训练：第一阶段是形式化CoT监督微调（Formalized CoT SFT），通过构建带有结构化工具槽和混合调用（工具调用与直接知识回答）的CoT轨迹，激活模型的隐式工具表示，并学习何时调用工具、何时依靠内部知识；第二阶段是自我调节奖励驱动对齐（Self-regulated Reward-Driven Alignment），引入净工具增益（Net Tool Gain, NTG）指标来量化每次工具调用的边际贡献，并基于此设计奖励函数，惩罚无效工具依赖，鼓励模型在内部知识足够时抑制工具调用，实现知识路由。实验方面，BEE在多个细粒度视觉感知基准（如MME-Real等）上达到最优性能，同时与现有显式工具方法和隐式方法（Monet、EVA）相比，推理延迟显著降低，在8K分辨率设置下延迟减少高达86.2%。在一般复杂推理任务上，BEE也保持了竞争力。进一步的分析表明，NTG度量能够有效检测冗余工具使用，自我调节机制减少了不必要的调用。总体而言，BEE为高效多模态推理提供了一种新的隐式工具范式，兼顾性能和效率。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在Introduction、Method和实验部分；

## 基本信息

- 作者：Xiuwei Chen, Quanlin Chen, Wentao Hu, Zisheng Chen, Kun Xiang, Zehua Ma, Mingyang Zhang, Jianhua Han, Hanhui Li, Hang Xu, Xiaodan Liang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11106v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成以PDF语义检索证据为主要依据，结合摘要和启发式草稿进行综合分析和润色。
