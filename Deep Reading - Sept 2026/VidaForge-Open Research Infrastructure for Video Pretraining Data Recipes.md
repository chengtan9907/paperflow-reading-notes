---
user_id: "cheng tan"
paper_id: 10893
arxiv_id: "2609.06652v1"
title: "VidaForge: Open Research Infrastructure for Video Pretraining Data Recipes"
institution: "根据论文作者信息（Yan Ma, Jiadi Su, Pengfei Liu 等），该研究由上海交通大学/生成式人工智能研究组（GAIR Lab）或相关联合团队主导（合理推断，具体机构信息在检索片段中未完全显式列出，需回原文确认）。"
publish_date: "2026-09-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.06652v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.06652v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-12T13:22:53"
---
# VidaForge: Open Research Infrastructure for Video Pretraining Data Recipes

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：video pretraining · data recipe · open infrastructure · data pipeline

## 一句话总结

本研究推出了 VIDAFORGE，一个开源的视频预训练数据配方研究基础设施，它将从原始视频到训练数据集的端到端数据流水线抽象为包含五个阶段的可执行、可追溯工作流，并发布了包含 314 万剪辑的 VIDAFORGE-3M 数据集。

## 摘要

> Video foundation models increasingly rely on large-scale pretraining data, yet the end-to-end data pipelines behind them remain largely closed and difficult to inspect or reuse. Researchers seeking to understand how video data recipes affect model pretraining often need to build substantial infrastructure before testing even a focused hypothesis. We present VIDAFORGE, an open research infrastructure that represents a video data recipe as an executable five-stage workflow from raw videos to training datasets. A decision in this workflow can be varied to construct alternative datasets while preserving how every sample was produced. To demon strate this research workflow, we compare data recipes with different coverage and quality in early from-scratch pretraining of Wan 2.1 and V-JEPA 2.1. Across both learning objectives, the broader-coverage recipe achieves the highest downstream benchmark scores, while loss-based evaluation favors different recipes. This study demonstrates how VidaForge connects data-recipe choices to downstream model performance. We further release VIDAFORGE-3M, containing 3.14 million scene level clips totaling 6,475 hours, with fine-grained annotations and curation signals for video data-recipe research.

Q1: 这篇论文试图解决什么问题？

### 核心痛点与研究动机
1. **数据流水线的封闭性**：当前领先的视频基础模型（Video Foundation Models）极度依赖大规模预训练数据，但其背后的端到端数据处理流水线（Data Pipelines）通常是封闭且不透明的，外界无法对其进行审查、复现或重用。
2. **高昂的基础设施门槛**：研究人员如果想要探索不同的视频数据配方（Data Recipes）如何影响模型的预训练效果，通常需要先耗费大量精力构建庞大的底层基础设施，这极大地限制了对特定科学假设（例如数据质量与覆盖度的权衡）的快速验证。
3. **数据配方科学研究的缺失**：由于缺乏统一、可执行且可追溯的工具，视频预训练数据的科学研究目前主要局限于少数头部前沿实验室，社区缺乏标准化的实验协议来连接“数据配方选择”与“下游模型性能”。

### 隐含假设与边界条件
- **合理推断**：该研究隐含假设视频数据的处理流程可以被高度抽象为通用的多阶段决策链，且在不同学习目标（如生成式与自监督表示学习）之间，数据配方的调整对模型性能的影响具有可比性和可追踪性。同时，早期从头预训练（Early From-Scratch Pretraining）的趋势能够一定程度上反映大规模长期预训练的最终走向。

Q2: 有哪些相关研究？

### 相关研究背景与局限
1. **大规模视频数据集**：当前学术界和开源社区已经存在诸如 InternVid 等大规模视频数据集。然而，这些数据集通常只提供最终清洗和标注后的结果，并没有开源其生成这些数据的完整动态流水线代码和中间决策参数。
2. **视频基础模型的发展**：随着 Wan 2.1、V-JEPA 2.1 等视频生成与表示学习模型的快速演进，社区对高质量、定制化预训练数据的需求日益迫切。但现有研究多聚焦于模型架构和训练算法的改进，对数据侧的系统性、可控性研究相对滞后。
3. **VIDAFORGE 的定位**：与单纯发布静态数据集不同，VIDAFORGE 侧重于提供一个“可执行且可追溯”的路径。它不仅记录了样本级别的处理日志，还能将同一视频源池（Video Pool）通过不同的配方变体分别输送到生成式和自监督预训练实验中，填补了数据基础设施开源领域的空白。

Q3: 论文如何解决这个问题？

### VIDAFORGE 核心架构设计
VIDAFORGE 将复杂的视频数据配方抽象为一个包含**五个阶段的决策链（Five-Stage Decision Chain）**，实现了从原始视频到模型就绪（Model-Ready）训练数据集的全流程自动化与可追溯性：
1. **摄取（Ingestion）**：负责原始视频源的收集、格式标准化与元数据导入。
2. **分割（Segmentation）**：在场景级别（Scene-Level）对长视频进行切分，确保语义的连贯性与剪辑的独立性。
3. **筛选（Selection）**：基于多维度质量信号（如美学得分、运动幅度、分辨率等）对剪辑进行过滤或分级。
4. **标注（Annotation）**：生成细粒度的文本描述、标签或其他多模态控制信号。
5. **打包（Packaging）**：将处理后的视频和标注信息转化为适合分布式训练的高效数据格式。

### 追溯性与可复现性机制
- **全生命周期记录**：VIDAFORGE 能够完整保留每个阶段的中间输出、配方参数以及样本级别的处理记录。这意味着研究人员可以精确重建任何一个训练样本的加工历史，或者通过仅修改某一阶段的参数（如提高筛选门槛）来快速生成对比数据集（Data Variants）。
- **开源生态配套**：项目不仅提供了可运行的配方代码和详尽的文档，还附带了完整的视频操作指南（Walkthrough），涵盖从安装到实际部署的全过程，极大地降低了研究人员的使用门槛。

Q4: 论文做了哪些实验？

### 实验设计与协议
为了展示 VIDAFORGE 在实际数据配方研究中的威力，研究者设计了一项关于**数据覆盖度（Coverage）与数据质量（Quality）权衡**的系统性研究：
1. **对比配方构建**：利用 VIDAFORGE 基础设施，从相同的原始视频池中衍生出不同侧重点的数据配方变体（例如：一种侧重于广泛的视觉场景覆盖，另一种则通过严格的筛选机制侧重于高美学/高清晰度质量）。
2. **多预训练目标验证**：
 - **生成式预训练（Generative Pretraining）**：采用 **Wan 2.1** 模型架构进行早期从头预训练。
 - **表示学习预训练（Representation Pretraining）**：采用 **V-JEPA 2.1** 自监督架构进行早期从头预训练。
3. **评估指标**：
 - **下游基准测试（Downstream Benchmarks）**：评估模型在实际视觉任务和生成质量上的表现。
 - **基于损失的评估（Loss-based Evaluation）**：监控预训练过程中的验证集损失收敛情况。

### 开源数据集发布
- **VIDAFORGE-3M**：作为研究基础设施的一部分，团队释放了该数据集，包含 **314 万个场景级剪辑**，总时长达 **6,475 小时**。该数据集不仅体量庞大，还深度集成了细粒度的标注和丰富的策展信号（Curation Signals），直接服务于数据配方优化研究。

Q5: 发现了什么实验现象？

### 核心实验现象与发现
1. **覆盖度与质量的张力**：实验结果表明，在下游基准测试（Downstream Benchmarks）中，**拥有更广泛覆盖度（Broader-Coverage）的数据配方取得了最高的得分**。这表明在早期预训练阶段，视觉多样性和概念覆盖度对模型泛化能力的提升至关重要。
2. **评估指标间的冲突（指标张力）**：研究发现了一个反直觉或值得警惕的现象——**基于损失（Loss-based）的评估与下游基准测试的偏好并不一致**。具体而言，Loss 评估可能会青睐某些特定清洗规则下的高纯度/窄分布配方，但这并不能转化为更好的下游任务表现。这证明了单纯依赖预训练 Loss 来筛选数据配方存在潜在的误导性。
3. **跨学习目标的普适性**：无论是针对生成式的 Wan 2.1 还是针对自监督表示学习的 V-JEPA 2.1，VIDAFORGE 都能成功建立起“数据配方选择”与“最终模型性能”之间的清晰因果联系，展示了该基础设施在不同技术路线下的稳健性。

Q6: 有什么可以进一步探索的点？

### 可进一步探索的研究方向
1. **大规模 Scaling 规律验证**：当前实验主要集中在早期预训练阶段（合理推断），未来需要进一步验证这些数据配方在百亿参数级模型、数十万小时数据规模下的 Scaling Laws 是否依然成立。
2. **动态数据配方（Dynamic Recipes）**：探索在预训练的不同阶段（如前期、中期、微调期）动态调整覆盖度与质量的比例，利用 VIDAFORGE 的多阶段切换能力实现自适应数据流。
3. **更丰富的标注模态集成**：在标注（Annotation）阶段引入更先进的 3D 视觉信号、音频信号或多视角一致性标注，以支持下一代全模态视频基础模型的训练需求。
4. **自动化配方搜索（Auto-Data-Recipe）**：结合强化学习或贝叶斯优化，利用 VIDAFORGE 作为环境，自动搜索最优的数据筛选与打包参数组合。

Q7: 总结一下论文的主要内容

本论文针对视频基础模型预训练中“数据流水线封闭、研究门槛高”的行业痛点，推出了开源研究基础设施 VIDAFORGE。该系统将视频数据配方标准化为由摄取、分割、筛选、标注、打包组成的五阶段可执行工作流，实现了样本级加工历史的完全可追溯与参数化定制。利用 VIDAFORGE，研究团队系统探究了数据覆盖度与质量在 Wan 2.1（生成式）和 V-JEPA 2.1（表示学习）早期预训练中的影响。实验揭示了广泛覆盖度配方在下游基准测试中的优越性，并指出了预训练损失与下游实际性能之间的评估错位。此外，项目开源了包含 314 万剪辑（6,475 小时）且富含策展信号的 VIDAFORGE-3M 数据集。VIDAFORGE 为开源社区提供了一条从原始视频到预训练实验的标准化、科学化路径，有望加速视频数据科学的发展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作展示了如何系统性地组织大规模视频数据的清洗、标注与打包任务，对于关注系统性工作（Systematic Work）和数据基础设施建设的同学有极高的参考价值。

## 基本信息

- 作者：Yan Ma, Jiadi Su, Zhulin Hu, Ethan Chern, Linhao Zhang, Tiantian Mi, Pengfei Liu
- 机构：根据论文作者信息（Yan Ma, Jiadi Su, Pengfei Liu 等），该研究由上海交通大学/生成式人工智能研究组（GAIR Lab）或相关联合团队主导（合理推断，具体机构信息在检索片段中未完全显式列出，需回原文确认）。
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.06652v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成深度参考了 PDF 检索到的 Abstract、Introduction、Method、Conclusion 等核心章节的语义片段，并结合论文的系统性定位进行了详尽的中文精读报告组织。
