---
user_id: "cheng tan"
paper_id: 15991
arxiv_id: "2610.18829v1"
title: "OmniCompass: A Holistic Benchmark for Omni-Modal Video Grounding and Understanding"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18829v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18829v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:02:55"
---
# OmniCompass: A Holistic Benchmark for Omni-Modal Video Grounding and Understanding

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：omni-modal video understanding · video grounding benchmark · temporal grounding · spatial grounding

## 一句话总结

OmniCompass 是一个用于全模态视频接地与理解的全方位基准，通过双轨（VL/AV）、三任务（TG/SG/QA）、配对目标规格与一致性指标，系统评估 50 个多模态大语言模型在时空接地、音视频证据利用和跨规格一致性上的表现。

## 摘要

> With the rapid development of multimodal large language models (MLLMs), evaluating their video understanding capabilities has become increasingly important. Existing video understanding benchmarks cover only part of the evaluation dimensions, limiting systematic diagnosis of omni-modal video understanding. We introduce OmniCompass, a holistic benchmark for omni-modal video grounding and understanding. OmniCompass defines two evaluation tracks: OmniCompass-VL assesses vision-language models on tasks requiring visual evidence, while OmniCompass-AV assesses omni-modal models on tasks requiring joint audio-visual evidence. Each track includes three complementary tasks: temporal grounding (TG), spatial grounding (SG), and question answering (QA), assessing when a referenced target appears, where it is, and what happens. QA covers five perception and three reasoning categories in OmniCompass-VL, and three perception and two reasoning categories in OmniCompass-AV. OmniCompass-VL contains 820 videos and 2,460 instances across domains and three duration types: short, medium, and long. A narrative- and audio-rich subset forms OmniCompass-AV with 240 videos and 720 instances. Each instance is evaluated under two paired settings: text-only and text+image, specifying the same target through text and visual-language guidance. All instances are manually annotated with multi-round quality verification. Evaluations of 50 MLLMs show that open-source models still lag behind proprietary counterparts, particularly in audio-visual understanding. Precise temporal and spatial grounding remains challenging even for leading models, and their predictions still exhibit inconsistencies across the paired target specifications.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：现有视频理解基准在评测维度上覆盖不全，无法对全模态视频理解进行系统化诊断。具体而言，人类在真实场景中会通过语言或视觉线索识别目标，跨空间和时间注意视觉与听觉证据，并形成目标导向的判断；MLLM 也应具备类似能力。然而现有基准存在三方面局限：
1. 视频证据覆盖有限：大部分基准仅依赖视频帧的视觉内容，不考虑与事件同步的音频信息，无法评估音频在多模态理解中的作用；少数涉及音频的基准又往往忽略空间或时间接地。
2. 目标规格方式单一：有的只用文本查询，有的只提供参考图像；缺少在同一目标、同一任务语义和同一标注条件下，对文本指定与视觉指定进行配对比较的评测协议，因此难以判断模型是否真正理解了被引用目标，抑或只是对查询文本的表面关联做出反应。
3. 任务覆盖割裂：现有基准通常只评测时间接地、空间接地或问答中的某一类任务，没有把“何时、何地、发生了什么”三类基本问题统一到同一视频、同一目标之下，导致无法分析模型在这三类任务上的能力关联与短板。
此外，跨模态一致性问题未被充分关注：当同一目标分别用文本和参考图像指定时，模型是否给出一致的时空接地和问答结果？OmniCompass 正是为了填补这些空白而提出的，目标是建立一个能同时诊断目标识别、时空接地、音视频证据整合和跨规格一致性的评估框架。

Q2: 有哪些相关研究？

论文的 Introduction 和检索证据显示，相关研究主要围绕以下几个方向：
1. 视频理解基准：已有基准通常仅基于视频帧（Chen et al., 2025a）解决任务，或需要同步音视频内容（Hong et al., 2026），但很少同时涵盖两种证据类型。
2. 目标规格方式：目标呈现可以只通过文本查询（Fu et al., 2025a），也可以配合参考图像（Ma et al., 2026）；不同规格方式对模型的要求不同，但缺乏配对对比。
3. 时空接地任务：时间接地（TG）和空间接地（SG）分别预测目标特定的时间区间和空间区域，相关工作包括 Lei et al. (2021)、Cheng et al. (2026) 等；近年 MLLM 也开始支持回看相关时间片段（Yang et al., 2026a; Zeng et al., 2026b）、检查任务相关的空间证据（Wang et al., 2026c; Meng et al., 2026），以及选择性获取音视频证据进行推理（Xing et al., 2026; Chen et al., 2026）。
4. 全模态/音视频模型：包括 Gemini、Qwen-Omni、Seed 系列、VideoLLaMA2、VITA 等，这些模型原生支持音频输入，但它们的音视频联合理解能力缺乏系统评测。
5. 一致性评测：现有基准较少在相同任务和相同目标下改变目标规格来检验预测一致性；OmniCompass 引入的配对设置和 CS（Consistency Score）指标填补了这一缺口。
总之，OmniCompass 相较于已有工作的核心差异在于：在同一个基准中统一 TG/SG/QA 三类任务、设计 VL/AV 双轨、使用配对目标规格，并同时评估正确性与一致性。

Q3: 论文如何解决这个问题？

OmniCompass 的解决方法可以概括为“两轨、三任务、配对规格、统一标注、一致性指标”的基准设计：
1. 双轨设计：OmniCompass-VL 面向视觉-语言模型，只提供视觉证据相关的任务；OmniCompass-AV 面向全模态模型，要求模型联合音频和视觉证据才能正确回答。AV 子集从 VL 中筛选叙事丰富和音频丰富的视频，保证音频对理解事件有实际贡献。
2. 三任务统一：对每个被引用目标，同时标注时间接地（目标出现的起止时间）、空间接地（目标所在区域，通常以框/掩码形式）和问答（描述目标相关事件）。这样可以在同一目标上对比模型在“何时、何地、是什么”三类能力上的表现。
3. 配对目标规格：每个实例都有 text-only 和 text+image 两种规格，二者指定相同目标、相同任务语义和相同答案标注。通过对比模型在两种规格下的输出，可以计算预测的跨规格一致性，避免模型仅从文本线索猜测。
4. QA 分类体系：VL 轨的 QA 覆盖 5 个感知类别和 3 个推理类别，AV 轨覆盖 3 个感知类别和 2 个推理类别，用于细粒度诊断模型对不同认知层次任务的差异。
5. 人工标注与多轮质量验证：全部视频和实例由人工标注，并经过多轮质量核查，以保证目标标注、接地框/区间和答案的可靠性。
6. 评测协议：帧采样策略根据模型类型调整（如专有模型和大规模开源模型采样 64 帧，小模型 32 帧，GPT 系列 49 帧以适配文本+图像参考，VITA-1.5-7B 按官方 16 帧限制）；每帧前附加时间戳，帮助模型对齐时间位置。
7. 一致性指标（CS）：除传统正确性指标（如 TG 的 mtIoU、SG 的 msIoU、QA 的准确率）外，新增 CS 指标衡量同一实例在 text-only 与 text+image 两种规格下预测的一致性。
该设计使得评测不仅能报告整体正确性，还能揭示目标规格变化带来的行为差异。

Q4: 论文做了哪些实验？

论文在两大轨上进行了系统实验：
1. 评测模型：共 50 个 MLLM。OmniCompass-VL 评测的视觉-语言模型包括专有模型 Gemini 系列、GPT 系列、Seed 系列和 Qwen 闭源系列；开源模型包括 Kimi-K3、GLM-5.3-Flash、DeepSeek-V4.1-Flash、MiMo-V2.5、Qwen 开源系列、InternVL3.5-38B、Keye-VL-2.0-30B-A3B、TimeLens 系列和 LLaVA 系列。OmniCompass-AV 评测原生支持音频的全模态模型，包括 Gemini 系列、Qwen-Omni 系列、Seed-2.0-Lite/Mini、MiMo-V2.5、video-SALMONN 2+、MiniCPM-o-4.5-8B、Ola-7B、VideoLLaMA2-7B 和 VITA-1.5-7B。
2. 实现细节：帧采样方面，专有模型和超 100B 参数的开源模型统一采样 64 帧，较小模型采样 32 帧；GPT 系列使用 49 帧以适配 50 图像上限内的 Text+Image 参考；VITA-1.5-7B 遵循官方 16 帧限制。按照 TimeLens 的做法，每帧前直接附加以秒为单位的时间戳。
3. 评测设置：每个实例在 text-only 和 text+image 两种设置下分别评测，报告 TG 的 mtIoU、SG 的 msIoU、QA 准确率以及跨规格一致性分数（CS）。
4. 消融/对比分析：对比专有与开源模型在 VL/AV 轨上的整体表现；分析参考图像对 TG/SG/QA 的影响；分析时间戳对模型性能的影响；比较不同模型规模的效果；分析 QA 子类别（如 Recognition、Understanding、Comparative Judgment、Action Recognition、Causal Reasoning 等）的表现。检索片段还显示，某些开源模型（如 MiMo-V2.5）在 AV 轨上的表现与专有模型差距较大，而 Qwen3.8-27B 是少数在加入视觉线索后有所提升的百亿参数以下开源模型。

Q5: 发现了什么实验现象？

实验揭示了以下主要现象：
1. 开源与专有模型的整体差距：开源模型整体落后于专有模型，且在音视频联合理解（AV 轨）上的差距尤为明显；例如检索片段显示，AV 轨上专有模型 Gemini-3.8-Flash 的某个聚合指标远高于开源领头模型 MiMo-V2.5（如 CSAvg 上专有 57.4% 对开源 24.6%）。
2. 强 QA 与弱接地并存：即使模型在 QA 上表现出较高的准确率，其精确的时间接地（TG）和空间接地（SG）依然薄弱；聚合准确率相近的模型，细粒度接地能力可能差异很大。这说明“能回答”不等于“知道何时何地发生”。
3. 跨规格不一致：模型在 text-only 与 text+image 两种目标规格下的预测不一致，即使两种规格指定的是同一目标、同一任务和同一答案。参考图像并不总是带来改善，视觉目标规格的帮助不稳定。
4. 参考图像的任务依赖效应：加入参考图像的效果因任务而异，并不是对所有任务都有益。例如开源模型中只有 Qwen3.8-27B 在百亿参数以下规模获得提升：TG mtIoU 提升 1.9%、SG msIoU 提升 1.5%，但 QA 准确率下降 2.2%；检索片段提示“视觉线索可能有助于目标匹配但会忽略有用信息”。
5. 时间戳的积极作用：在 VL 轨，加入时间戳对 TG/SG/QA 均有提升，例如某些模型 TG 提升显著（如 GLM-5.x 和 Qwen3.5 各自 TG 提升 20% 以上），SG 提升 16.9%–22.8%，QA 提升 18.6%–20.0%；在 AV 轨提升相对更小且更依赖任务，例如 SG 提升 18.3%（Gemini-3.8-Flash）和 22.3%（另一模型），但 QA 在所有对比中均提升 0.7%–5.9%。
6. 类别级差异：检索片段显示不同 QA 子类别的表现差异显著，如 Recognition、Understanding、Comparative Judgment、Action Recognition、Causal Reasoning 等类别在专有与开源模型间的差距不同，说明模型在感知与推理维度上的能力分布不均。
7. 模型规模效应：在 AV 轨上，更小规模的开源模型普遍难以利用音频证据，只有少数模型在视觉线索辅助下取得接地提升，且可能以牺牲 QA 为代价。

Q6: 有什么可以进一步探索的点？

基于论文的结论和评测中发现的问题，可以进一步探索的方向包括：
1. 更强的多模态对齐：论文发现专有模型在音视频理解上领先，说明跨模态对齐仍是瓶颈，可研究更有效的音频-视觉-文本对齐训练方法。
2. 音频感知接地：目前模型即使能理解音频内容，也难以将音频事件精确绑定到时间和空间位置；可探索专门的音频感知时空接地模型或训练目标。
3. 跨参考一致性改善：配对规格下预测不一致表明模型对目标引用的鲁棒性不足；可研究统一目标表征、对比学习或一致性正则化来提升跨规格稳定性。
4. 长视频与更细粒度接地：基准已覆盖短、中、长三种时长，但长视频中的精确接地仍可能是挑战，可进一步扩大时长范围或增加多事件场景。
5. 更丰富的模态扩展：当前“omni”主要指视觉+音频，未来可纳入更深度的交互（如触觉、深度、场景图）或多视角视频，形成真正的全模态评测。
6. 评测指标细化：CS 指标可进一步分解为不同任务、不同规格差异方向上的一致性；还可联合校准正确性与一致性。
7. 训练与评测闭环：基于 OmniCompass 的诊断结果，设计针对薄弱能力的微调或提示策略，并验证其是否在基准上带来可泛化的改进。
8. 动态视频理解：结合目标引导的主动采样、回看机制和音频事件触发，研究模型如何像人类一样按需获取证据。
9. 跨领域泛化：七个领域之外的更多场景（如医学视频、科学实验录像、工业巡检）可验证基准的领域迁移性。

Q7: 总结一下论文的主要内容

OmniCompass 是一篇以“系统诊断全模态视频理解”为核心目标的基准论文。它的出发点是人类感知的多模态本质：人通过语言或视觉线索锁定目标，在时间和空间中整合视觉与听觉证据，最终形成判断。MLLM 的快速发展使得这类能力可以被模型化，但现有基准要么只覆盖视频帧视觉内容，要么只考虑文本查询，要么孤立地评测时间接地、空间接地或问答中的某一种任务，无法回答“模型到底能不能在真实复杂场景中完成全模态接地与理解”这一根本问题。

为此，论文设计了 OmniCompass 基准。它包含两条评测轨：VL 轨面向视觉-语言模型，AV 轨面向原生支持音频的全模态模型。每条轨都统一了三个互补任务：时间接地（TG）、空间接地（SG）和问答（QA），对应目标“何时出现、在哪里、发生了什么”。VL 轨的 QA 进一步细分为 5 个感知类别和 3 个推理类别，AV 轨为 3 个感知类别和 2 个推理类别，以支持认知层面的细粒度诊断。数据集方面，VL 轨包含 820 个视频、2,460 个实例，覆盖七个领域和短/中/长三种时长；其中叙事与音频信息丰富的 240 个视频、720 个实例构成 AV 轨。全部实例由人工标注并经过多轮质量验证。每个实例都有两种配对的目标规格：纯文本指定与文本+参考图像指定，二者对应相同目标、相同任务语义和相同标注。

评测协议上，论文采用了适配不同模型的帧采样策略（专有与大模型 64 帧、小模型 32 帧、GPT 系列 49 帧、VITA 16 帧），并在帧前附加秒级时间戳。除传统正确性指标外，论文提出一致性评分（CS），用于衡量同一实例在换用视觉指定后预测是否保持一致。基于 50 个 MLLM 的大规模实验得出若干关键结论：开源模型总体上仍落后于专有模型，音视频联合理解上的差距尤为突出；即使是领先模型，精确的时间与空间接地也仍然困难；模型的 QA 准确率较高时，其接地能力可能很差；模型在两种目标规格上的预测不一致；参考图像的影响是任务依赖的，并不总是正面；时间戳在两条轨上都能带来性能提升。

论文的贡献在于：提供了一个可以同时覆盖 QA、精确接地和跨引用一致性的统一评测框架，能够帮助研究者定位模型在目标识别、时空定位、音视频证据使用和跨规格泛化上的具体短板，从而为更强的多模态对齐和音频感知接地研究提供方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该基准的系统性任务组织（TG/SG/QA 统一、paired 规格、CS 指标）对设计 agent 评测协议有直接借鉴价值，尤其是需要评估目标引用鲁棒性的场景。

## 基本信息

- 作者：Xuyang Liu, Shihao Wang, Xinyu Wei, Minghan Li, Zeqing Wang, Zuhao Yang, Zichen Wen, Lei Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.CL, cs.MM
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.18829v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 retrieved_evidence 中的 Introduction/Results/Conclusion 片段及 field_evidence_map 的字段映射；部分具体数字来自检索片段，其余内容基于摘要、结论和合理推断组织，未参考外部资料。
