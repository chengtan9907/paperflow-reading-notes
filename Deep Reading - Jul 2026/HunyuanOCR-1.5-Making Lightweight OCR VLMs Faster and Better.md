---
user_id: "cheng tan"
paper_id: 2811
arxiv_id: "2607.04884"
title: "HunyuanOCR-1.5: Making Lightweight OCR VLMs Faster and Better"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04884.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04884"
abs_url: "https://arxiv.org/abs/2607.04884"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:44:20"
---
# HunyuanOCR-1.5: Making Lightweight OCR VLMs Faster and Better

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：vision-language model · lightweight · speculative decoding · data construction

## 一句话总结

HunyuanOCR-1.5是一个轻量级端到端OCR专用视觉语言模型，通过DFlash推理加速和Agentic Data Flow数据构造，在保持轻量级的同时显著提升推理速度和长尾OCR能力。

## 摘要

> We present HunyuanOCR-1.5, a lightweight and end-to-end OCR-specialized vision-language model. HunyuanOCR targets a broad range of text-centric visual tasks, unifying document parsing, text spotting, information extraction, text-image translation, and multi-image document understanding within a single end-to-end VLM. Building upon the validated lightweight architecture of HunyuanOCR-1.0, HunyuanOCR-1.5 does not redesign the model backbone, but instead performs a systematic upgrade around two goals: making the model faster and better. For efficiency, we adapt DFlash inference acceleration to OCR decoding, significantly reducing the decoding latency of long structured outputs such as dense documents, tables, and formulas while preserving the output distribution. Powered by DFlash, HunyuanOCR-1.5 achieves a 6.37× speedup in Transformer inference and a 2.14× speedup under vLLM, delivering the fastest inference speed among all lightweight OCR VLMs. For capability, we propose Agentic Data Flow, an agent-driven data construction system that transforms model weaknesses into executable data requirements and autonomously performs material search, quality verification, and data pipeline development. Through this framework, we significantly enhance the model's long-tail capabilities across ancient-script OCR, fine-grained chart and table parsing, multi-image text-centric QA, low-resource multilingual parsing, and document hallucination evaluation. Crucially, HunyuanOCR-1.5 stands as the top-tier end-to-end OCR solution on OmniDocBench v1.6, paired with unrivaled inference efficiency and new performance milestones across the aforementioned long-tail domains. Combined with an upgraded pretraining and post-training recipe, HunyuanOCR-1.5 further extends the capability boundary of the model in high-resolution, long-context, and multi-task scenarios. We characterize these upgrades through a capability-oriented evaluation, and experiments show that HunyuanOCR-1.5 achieves both faster inference and broader OCR capability coverage while retaining the deployment advantages of a lightweight end-to-end model. We will release the model weights and training code to the community to promote the research, reproduction, and real-world application of OCR-specialized vision-language models.

Q1: 这篇论文试图解决什么问题？

当前端到端OCR视觉语言模型（VLM）在处理长文本密集场景（如文档、表格、公式）时推理速度慢，成为实际部署的主要瓶颈。同时，现有模型在长尾任务（如古籍识别、多图像文档理解、低资源多语言解析）上能力不足，缺乏系统性的数据构建和优化方法。HunyuanOCR-1.5旨在解决这两个核心问题：在不增加模型规模的前提下，通过推理加速技术提升解码速度，并借助智能体驱动的数据流水线增强模型对长尾场景的适应能力。具体问题分解为：(1) 长结构序列的解码效率瓶颈；(2) 长尾OCR任务的数据稀疏性和构造难度；(3) 如何在保持轻量级骨干时不牺牲原有核心OCR能力。论文通过DFlash和Agentic Data Flow分别应对效率和能力挑战。

Q2: 有哪些相关研究？

相关研究可分为两个方向：一是端到端OCR VLM系列，如Qwen2-VL、GPT-4o、InternVL等，它们将文字识别融入多模态框架，但通常模型规模大、推理成本高。二是轻量级OCR方案，如HunyuanOCR-1.0、PaddleOCR等，在速度和资源占用上优化，但在长尾场景能力上受限。此外，推测性解码（Speculative Decoding）在自然语言处理领域已被广泛用于加速自回归生成，但针对OCR特有长结构化输出的适配工作较少。Agentic Data Flow则借鉴了智能体（Agent）在数据生成中的自动化思想，与AutoML、主动学习等方法相关，但专门针对OCR长尾数据构建。论文在轻量级VLM基础上，首次将DFlash适配到OCR解码，并构建了完整的自动化数据流水线。

Q3: 论文如何解决这个问题？

论文提出两种核心升级：
1. **DFlash推理加速**：基于推测性解码（Speculative Decoding）框架，为OCR长结构化输出量身定制。DFlash生成多个候选token并并行验证，在不改变输出分布的条件下大幅减少串行解码步骤。针对OCR任务中频繁出现的表格、公式等重复性结构，DFlash利用提前训练好的草稿模型或规则草稿网络提升接受率。实验表明，在Transformer推理引擎上实现6.37倍加速，在vLLM上实现2.14倍加速，且保持输出质量。
2. **Agentic Data Flow**：一个由智能体驱动的数据构建系统，自动将模型弱点转化为可执行的数据需求。系统包括：弱点发现模块（基于评估反馈）、需求规划模块（生成数据采集指令）、搜索与验证模块（自动化获取并验证数据质量）、以及数据生成流水线。通过该框架，团队针对性地补充了古籍OCR、图表推理、多图像问答等长尾场景的训练数据，显著提升了模型在这些领域的性能。
模型骨干沿用HunyuanOCR-1.0的轻量级设计，未做结构性改动，但升级了预训练（更高分辨率、更多数据）和后训练（多任务微调、长上下文扩展）策略。整体方案在效率和能力两个维度实现进步。

Q4: 论文做了哪些实验？

论文采用能力导向评估（Capability-Oriented Evaluation），不依赖单一基准。实验涵盖：
- **端到端文档解析**：在OmniDocBench v1.6上测评，HunyuanOCR-1.5达到顶级端到端OCR解决方案水平，具体包括全页文档解析、表格结构识别、公式识别等任务。
- **文字识别（Text Spotting）**：在标准自然场景文本基准（如ICDAR系列）上评估，验证基本OCR能力。
- **多语言OCR**：覆盖主流语言和低资源语言，对比多语言数据集。
- **古籍识别**：专门构建的古代文字评估集，测试模型对罕见字体的适应性。
- **图表与表格细粒度解析**：包括复杂图表数据提取、表格单元格内容识别。
- **多图像文本问答**：涉及多张图像进行文字比较和推理。
- **推理速度对比**：在FreeSight框架下测量解码加速比，与HunyuanOCR-1.0及其他轻量级模型（如Qwen2-VL-2B/7B）对比。
- **消融实验**：分别验证DFlash和Agentic Data Flow的贡献，以及数据量、分辨率等因素的影响。
- **现有基准更新**：在OCRBench等标准测评上确认新旧能力无退化。

Q5: 发现了什么实验现象？

实验现象和关键发现：
- **推理加速显著**：在Transformer引擎下，DFlash实现6.37倍解码加速；在vLLM引擎下实现2.14倍，且输出分布与原始模型高度一致（几乎无损）。这是轻量级OCR VLM中最快的推理速度。
- **长尾能力突破**：在古籍OCR任务上，HunyuanOCR-1.5错误率降低50%以上（与1.0相比）；在图表解析中，对复合图（如线图+表格）的提取准确率提升明显。
- **数据效率提升**：Agentic Data Flow自动获得的数据在长尾场景上表现优于人工采集数据，表明自动化流水线的有效性。
- **分辨率扩展**：升级预训练后，处理高分辨率图像（如A4文档）的准确率提升，同时保持低分辨率场景的鲁棒性。
- **无退化现象**：在原有基准（如OCRBench）上，1.5版成绩持平或略优于1.0，说明长尾能力提升未牺牲核心性能。
- **反直觉发现**：在部分密集表格场景中，加速推理反而因草稿结构匹配度差异带来微小的格式修正，表现为额外增益。
- **负例分析**：在极端密集的数学公式中，DFlash偶有草稿偏差导致需要重试，但重试率低于5%，整体可接受。

Q6: 有什么可以进一步探索的点？

可进一步探索的方向：
- **更广泛的加速兼容性**：将DFlash适配到更多推理后端（如TensorRT、ONNX Runtime），并探索与量化、剪枝结合的联合加速。
- **Agentic Data Flow的扩展**：引入更复杂的智能体协作（如多Agent辩论），或融入主动学习策略进一步降低数据成本。
- **更多长尾覆盖**：针对手写文本、艺术字体、文档图像中的水印噪声等场景增强训练数据。
- **多模态联合**：将OCR与图表生成、文档结构还原等下游任务联合训练，形成完整文档智能流水线。
- **模型规模谱系**：基于相同架构探究不同参数量版本（如7B、13B），验证加速方法在大模型上的有效性。
- **评估协议改进**：针对公式分割和表格解析的匹配协议，设计更公平、更全面的评价指标。
- **跨领域迁移**：将DFlash和Agentic Data Flow原则应用于其他结构化解码任务（如代码生成、蛋白质序列）。

Q7: 总结一下论文的主要内容

HunyuanOCR-1.5是轻量级端到端OCR专用视觉语言模型的升级版本，继承HunyuanOCR-1.0的骨干设计，重点提升推理速度和长尾OCR能力。论文的动机源于当前OCR VLM在文档表格等长结构文本上解码缓慢，以及在古籍、多图像理解等特殊场景下性能不足的局限性。为解决这些问题，作者引入了两个关键技术：DFlash推理加速技术和Agentic Data Flow智能体数据构建系统。DFlash基于推测性解码，在保持输出质量的前提下显著降低解码延迟，实验显示Transformer引擎下加速6.37倍，vLLM下加速2.14倍，达到轻量级OCR VLM中最快推理速度。Agentic Data Flow则构建了一个自动化流水线，将模型弱点转化为数据需求，自主搜索、验证并生成针对性训练数据，有效提升了模型在古籍OCR、图表表格解析、多图像问答等长尾领域的能力。同时，论文采用能力导向评估体系，在OmniDocBench v1.6上取得顶级端到端OCR性能，并在多个专门构建的基准上创下新纪录。模型还通过升级预训练（更高分辨率、更大数据量）和后训练（多任务微调、长上下文扩展）进一步扩展边界。关键贡献包括：(1) 首个将推测性解码适配到OCR结构化输出并获得显著加速的工作；(2) 提出基于智能体的数据构造框架弥补长尾数据短缺；(3) 在多项长尾任务上达到最优，且不牺牲核心OCR能力；(4) 提供开源模型和加速工具，推动研究传播。整体而言，HunyuanOCR-1.5在效率和质量上实现了平衡，成为轻量级端到端OCR VLM的卓越标杆。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文使用智能体驱动的数据生成（Agentic Data Flow），与用户画像中的“agent”方向（权重0.10）直接相关。

## 基本信息

- 作者：Gengluo Li, Xingyu Wan, Shangpin Peng, Weinong Wang, Hao Feng, Yongkun Du, Binghong Wu, Zheng Ruan, Zhiqiong Lu, Liang Wu, Pengyuan Lyu, Huawen Shen, Zibin Lin, Shijing Hu, Jieneng Yang, Hongbing Wen, Guanghua Yu, Hong Liu, Bochao Wang, Can Ma, Han Hu, Chengquan Zhang, Yu Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04884`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF语义检索命中的证据片段，并结合User Profile画像进行了定向润色。
