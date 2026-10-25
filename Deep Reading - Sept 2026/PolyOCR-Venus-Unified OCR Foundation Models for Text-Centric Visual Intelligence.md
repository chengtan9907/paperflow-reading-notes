---
user_id: "cheng tan"
paper_id: 14626
arxiv_id: "2609.37712v1"
title: "PolyOCR-Venus: Unified OCR Foundation Models for Text-Centric Visual Intelligence"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.37712v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.37712v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T13:07:07"
---
# PolyOCR-Venus: Unified OCR Foundation Models for Text-Centric Visual Intelligence

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：ocr foundation model · text-centric visual intelligence · instruction following · competence-guided policy optimization

## 一句话总结

本文提出 PolyOCR，一个覆盖 2B 到 9B 参数规模的统一 OCR 基础模型族，通过共享指令跟随框架、大规模数据引擎与 Competence-Guided Policy Optimization 训练方法，将 OCR 从纯文本转录推进到文本中心的通用视觉智能，并在 OCRBench v2.1、CC-OCR、KIE Benchmark、OmniDocBench v1.6 与 MDPBench 上取得领先或具有竞争力的性能。

## 摘要

> Optical Character Recognition (OCR) is evolving from plain-text transcription toward general visual intelligence, requiring models to recognize, localize, and reason over textual information in complex visual environments. However, existing OCR systems often excel at only some tasks and struggle to balance recognition, parsing, and reasoning across scenarios. In this report, we present PolyOCR, a family of unified OCR foundation models of varying scales. PolyOCR combines a shared instruction-following framework with a large-scale data engine that converts heterogeneous visual resources into quality-verified OCR supervision. We introduce Competence-Guided Policy Optimization, which combines verifier-based Group Relative Policy Optimization with on-policy distillation through sample-wise routing based on teacher reliability and the teacher--student competence gap. We also introduce OCRBench v2.1, our revision of OCRBench v2 with manually verified annotation corrections and task-aligned scoring metrics. Extensive experiments across OCRBench v2.1, CC-OCR, in-house KIE Benchmark, OmniDocBench v1.6 and MDPBench demonstrate that PolyOCR achieves state-of-the-art or highly competitive performance.

Q1: 这篇论文试图解决什么问题？

论文要解决的问题是：现有 OCR 系统在统一能力上存在结构性缺陷。具体拆解如下：1) 任务碎片化：传统 OCR 以模块化 pipeline 为主，先检测后识别，只能输出机器可读文本；新一代 OCR 需要在场景文本理解、空间定位、文档结构恢复、表格与公式解析、信息抽取和 OCR 推理等任务上同时工作，但现有系统通常是针对特定领域或任务族优化的专家模型，在单一任务内表现强、跨任务泛化弱。2) 能力不平衡：许多 MLLM 在识别、解析、推理三者之间难以兼顾，往往提升某项能力会牺牲其他能力，缺乏统一的建模范式。3) 数据瓶颈：异构视觉资源（自然图像、文档、表格、票据、证书、多语言文本等）的标注形态差异大，难以直接转化为一致、可验证的高质量 OCR 监督；已有数据引擎在质量控制上不足。4) 训练范式不匹配：常规 RL（如 GRPO）与蒸馏方法在异构 OCR 任务上存在样本难度不均、教师能力不可靠、师生能力差距动态变化的问题，可能造成灾难性遗忘或知识迁移效率低。5) 评测不一致：OCRBench v2 等基准存在标注错误和评分指标与任务不对齐的问题，导致模型对比不可靠。论文主张用统一指令跟随基础模型加能力导向数据构造与能力感知后训练来解决这些问题。

Q2: 有哪些相关研究？

相关工作按两条范式组织：1) 专用 OCR 系统：传统 OCR 通过任务特定 formulation 发展，包括文本检测与识别 pipeline（如 Chaudhuri 等经典工作）、场景文本识别、文档解析、表格理解、结构化信息抽取。近期代表如 Team et al. 2025、Cui et al. 2025b、Zhang et al. 2026b、Zhong et al. 2026、Wang et al. 2026a，这些系统在目标域内强，但跨场景泛化有限。2) 多模态大语言模型（MLLM）方向：Fu et al. 2025、Team et al. 2025、Li et al. 2026a、Qu et al. 2026c 等将 OCR 能力扩展到场景文本理解、空间定位、文档结构恢复、表格和公式解析、信息抽取、OCR 推理。论文还引用了 OCR 综述（Long et al. 2021; Zhang et al. 2024a）以及文本中心视觉智能的早期工作（Qu et al. 2023; Chen et al. 2024）。从检索证据可推测相关工作章节还涉及文本检测、grounding、位置感知 VQA、text spotting、文档结构理解等任务划分。需要说明：由于检索片段主要是 Introduction 残片，Related Work 的完整文献综述未被完整命中，具体 baseline 对比细节需回原文确认。

Q3: 论文如何解决这个问题？

PolyOCR 的解决方案由四层构成：1) 统一模型族：报告两个主变体 PolyOCR-2B 与 PolyOCR-9B，均采用 Qwen3.5 vision-language backbone，在统一 OCR 训练框架下训练；另有 PolyOCR-2.7B 面向多语言文档解析，由 1.2B ViT 视觉编码器和 1.5B Qwen2.5 文本解码器组成，针对多种书写系统和采集条件优化。2) 共享指令跟随框架：把所有 OCR 任务统一为指令跟随形式，使识别、grounding、位置感知 VQA、text spotting、文档结构理解、KIE、表格与公式解析、OCR 推理等任务共享同一建模范式。3) 大规模数据引擎：将真实世界资源、可控合成和模型辅助标注结合，把异构视觉资源转换成经过质量验证的 OCR 监督；从检索证据可见数据构造覆盖场景文本翻译、文档与表格合成、KIE 等能力维度，翻译数据用语言一致性、数字与符号保留、命名实体一致性、重复检测和长度约束过滤，每个样本包含原图与区域感知标注。4) Competence-Guided Policy Optimization（CGPO）：结合 verifier-based Group Relative Policy Optimization 与 on-policy distillation（OPD）。CGPO 的核心机制是根据教师可靠性和师生能力差距在样本级路由监督：缓存教师能力 cT(x) 和教师-学生能力差距 cT(x)-cS(x) 映射到 OPD 权重 α(x)，动态分配教师监督，从而在异构 OCR 任务上改善 RL 与知识迁移的效果，平衡异构任务学习并防止灾难性遗忘。5) OCRBench v2.1：对 OCRBench v2 进行修订，包含人工验证的标注修正和任务对齐的评分指标。

Q4: 论文做了哪些实验？

1) 评测协议：建立统一评估协议，所有对比模型在一致的基准版本、推理配置和评分流程下评估（相同 prompt 模板、图像预处理策略、视觉 token 预算、解码设置），保证可复现比较。2) 模型：PolyOCR-2B 与 PolyOCR-9B（Qwen3.5 backbone），另有 PolyOCR-2.7B（1.2B ViT + 1.5B Qwen2.5）仅用于 MDPBench 多语言文档解析评测。3) 基准：OCRBench v2.1（修订后主基准）、CC-OCR、自建 KIE Benchmark、OmniDocBench v1.6、MDPBench；能力覆盖文本感知、空间定位理解、结构化文档分析、信息抽取、OCR 推理。4) 消融：对提出的 CGPO 框架进行有效性验证。5) 文档类详细评测：分数在页面级别聚合再跨页平均，Table 8 报告证书、身份证、录取通知书、国际护照等整体结果；PolyOCR-2.7B 在两类采集条件下对比 OCR 专用模型，在全部五个汇总指标上取得更高分数（相对 MonkeyOCRv2-B-Parsing）。注意：检索证据只覆盖 Results 开头与部分表格残片，完整数值表、baseline 列表与消融细节需要回原文确认。

Q5: 发现了什么实验现象？

从可获取的检索证据与摘要可以归纳以下观察：1) PolyOCR-2.7B 在多语言文档解析上相对 OCR 专用模型（如 MonkeyOCRv2-B-Parsing）在两个采集类型下均表现更好，五个汇总指标全面领先，说明统一指令跟随训练在多语言文档场景中的优势。2) PolyOCR 在多个基准上达到 SOTA 或高度竞争性能，说明统一建模在跨任务覆盖上比专用系统更平衡；但检索证据中的表格数据是残片，无法复原具体数字。3) 从 CGPO 的设计动机看，论文隐含的核心观察是：异构 OCR 任务中 teacher 的可靠性因样本而异，固定权重的蒸馏和单一 RL 目标无法适配师生能力边界变化。4) 需要指出：评测设计强调统一协议与 OCRBench v2.1 的修正，隐含观察是原 OCRBench v2 的标注错误和评分不对齐会显著影响模型排名。由于检索证据不完整，无法报告具体消融趋势、负结果或指标间张力；这些都是信息缺口。

Q6: 有什么可以进一步探索的点？

1) 强化基础 OCR 能力：继续提升细粒度文本感知、定位和识别，并在语义层面推进 OCR 理解。2) CGPO 的精细化：教师可靠性估计可以从样本级扩展到 token 级或片段级，探索多教师路由、动态能力边界估计。3) 评测方法：把 OCRBench v2.1 的人工验证与任务对齐评分思路推广到更多基准。4) 数据引擎扩展：覆盖更多书写系统、低资源语言、复杂版式与文档类型，改进自动质量验证器。5) 模型架构探索：扩大参数规模、引入 MoE 或优化视觉编码器与文本解码器的配比；PolyOCR-2.7B 的 1.2B ViT + 1.5B decoder 架构表明编码器-解码器配比是影响多语言 OCR 的关键设计维度。6) 应用方向：OCR 作为视觉文档与智能系统的接口，可进一步与 agent、文档工作流、RAG 结合。7) 跨模态与多语言翻译：场景文本翻译的数据合成与过滤规则可以扩展到更多语言对与端到端翻译模型。

Q7: 总结一下论文的主要内容

论文的论证主线：OCR 从传统文本转录走向文本中心视觉智能，现有专用 OCR 系统和 MLLM 都难以同时平衡识别、解析与推理，因此需要一个统一的 OCR 基础模型范式。技术主线：1) 数据引擎把真实资源、可控合成、模型辅助标注整合为质量验证的大规模 OCR 监督；2) 共享指令跟随框架统一建模场景文本理解、文档解析、信息抽取、结构化内容恢复、多语言转换与 OCR 推理；3) CGPO 将 verifier 基 GRPO 与 on-policy distillation 结合，用教师可靠性 cT(x) 和师生能力差距 cT(x)-cS(x) 计算样本级蒸馏权重 α(x)，动态分配教师监督，改善异构 OCR 任务上的 RL 与知识迁移，防止灾难性遗忘。实验主线：在 OCRBench v2.1、CC-OCR、自建 KIE Benchmark、OmniDocBench v1.6、MDPBench 上以统一评估协议评测 PolyOCR-2B/9B 与多语言变体 2.7B，覆盖文本感知、空间定位理解、结构化文档分析、信息抽取、OCR 推理；PolyOCR 达到 SOTA 或高度竞争性能，2.7B 在 MDPBench 五个汇总指标上超越 OCR 专用模型。论文同时贡献 OCRBench v2.1 修订基准。整体结论是 PolyOCR 建立了统一建模范式，下一目标是继续强化细粒度文本感知、定位与识别并推进语义理解。信息缺口：检索证据未包含完整数值表、消融实验细节和完整 baseline 列表；作者机构未在元数据中提供。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：CGPO 的核心思想（根据教师可靠性与师生能力差距做样本级监督路由、动态调整蒸馏权重）对任何涉及 RL+蒸馏混合训练的智能体或多模态模型训练都有方法论借鉴价值

## 基本信息

- 作者：GuangJian Team, Kaili Huang, Yongshuo Zhang, Bingtao Fu, Changjiang Jiang, Chenfan Qu, Chenfeng Zhang, Fangming Cui, Gaoyang Zhang, Jiangwei Xie, Jianshu Li, Jing Huang, Jingwen Bai, Mingqi Fang, Tao Fang, Weihong Zhang, Wenbo Du, Xiongfei Bai, Xuekang Zhu, Yinan Xia, Zhenming Wang, Jian Liu, Jingjing Liu, Xiang Qi, Weiqiang Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.37712v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 arXiv 元数据与 130 个 PDF 语义检索片段（Introduction/Results/Conclusion 命中为主），但 Method 章节与完整数值表未被命中，相关机制描述部分属于基于摘要与结论的合理推断，并已在字段内标注。
