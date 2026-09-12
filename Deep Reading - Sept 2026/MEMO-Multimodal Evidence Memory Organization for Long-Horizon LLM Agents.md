---
user_id: "cheng tan"
paper_id: 10899
arxiv_id: "2609.07471v1"
title: "MEMO: Multimodal Evidence Memory Organization for Long-Horizon LLM Agents"
institution: "哈尔滨工业大学 (Harbin Institute of Technology), 上海交通大学 (Shanghai Jiao Tong University)"
publish_date: "2026-09-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.07471v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.07471v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-12T13:23:48"
---
# MEMO: Multimodal Evidence Memory Organization for Long-Horizon LLM Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：llm agents · external memory · multimodal memory · memory organization

## 一句话总结

MEMO 提出了一种面向长程 LLM 智能体的多模态证据内存组织方法，通过训练证据提取器和内存管理器，在有限的上下文预算内动态选择文本或视觉模态来优化内存读取效率。

## 摘要

> Long-running LLM agents rely on external memory to store and reuse information beyond a single context window, yet there is a fundamental tension between the continuous accumulation of interaction trajectories and the limited context capacity. The key challenge in agent memory is therefore not only to retrieve relevant records, but also to select necessary evidence under a given budget and organize it in an appropriate modality. Existing memory readout methods mainly use textual or visual forms. Text preserves high fidelity, but its linear token representation makes contents with different importance compete for the limited context at nearly uniform unit cost. Visual readout renders text into document-like images, which can use two-dimensional layouts to expose structure and emphasize key information, but it may lose fine-grained details during rendering and compression. To address this issue, we propose MEMO, a Multimodal Evidence Memory Organization method for LLM agents. MEMO first uses a trained evidence extractor to select relevant memory blocks and form evidence units with source information and presentation requirements. A trained query-conditioned memory manager assigns each unit to a textual, visual, or dual-channel carrier and selects a layout that matches the evidence structure. A deterministic memory construction module then generates the textual package and visual pages. The memory manager is trained with feedback from an offline reader that measures the utility of the guided memory plan, so that retention and presentation decisions align with downstream usage. We evaluate MEMO on four benchmarks, HotpotQA, 2WikiMultiHopQA, LoCoMo, and ALFWorld, with multiple reader backends. The results show that MEMO presents memory more efficiently with fewer memory tokens, improves downstream task performance, and builds more effective working memory under constrained budgets. For example, under a fixed 128-token budget with Qwen3-VL-32B as the reader, MEMO achieves an F1 score of 73.91 on 2Wiki, outperforming text-only memory with 56.26 and visual-only memory with 35.89.

Q1: 这篇论文试图解决什么问题？

### 1. 核心矛盾：内存膨胀与上下文瓶颈
在长程交互任务中，LLM 智能体产生的交互轨迹（Trajectories）会随时间持续累积。然而，LLM 的上下文窗口（Context Window）是有限且昂贵的。直接将全部历史记录输入模型会导致：
- **信息竞争**：真正有用的证据与大量冗余或弱相关内容在有限空间内竞争，导致模型注意力分散。
- **成本高昂**：长上下文显著增加了推理的计算开销和延迟。

### 2. 现有读取模态的权衡（Trade-offs）
论文指出，现有的内存读取（Readout）方式存在明显的优缺点：
- **文本模态（Textual Readout）**：
 - **优势**：具备极高的保真度，能够精确保留原始记录的每一个字符。
 - **劣势**：其线性 Token 表示意味着所有内容（无论重要程度）都以几乎统一的单位成本消耗上下文。对于具有复杂结构的信息，文本难以高效展示其层级或关联。
- **视觉模态（Visual Readout）**：
 - **优势**：通过二维布局（如表格、列表、加粗强调）展示结构化信息，能够利用视觉先验引导模型关注重点。
 - **劣势**：在渲染和压缩过程中可能丢失细粒度的文本细节，且对于非结构化叙述的表达效率可能不如纯文本。

### 3. 内存读取的决策挑战
智能体内存系统不仅需要“检索”相关记录，更需要解决“如何呈现”的问题：
- **证据筛选**：在给定 Token 预算下，哪些证据是解决当前查询所必需的？
- **模态分配**：哪些证据适合用文本表达，哪些适合渲染成图像？
- **布局优化**：如何排列这些证据以最大化阅读器的理解效率？

Q2: 有哪些相关研究？

### 1. 文本外部内存系统
这是目前最主流的智能体内存接口。研究重点在于何时存储、检索、修改和压缩记录：
- **压缩与检索**：如 SimpleMem 和 LightMem，侧重于通过轻量化手段管理历史记录。
- **强化学习优化**：如 MemRL、Agentic Memory 和 Mem-α，通过学习记录的效用或内存操作指令来提升性能。
- **生命周期管理**：如 EverMemOS 和 MemoryOS，进一步引入了内存层级和复杂的生命周期管理机制。然而，这些方法最终都输出线性文本，未能解决 Token 竞争问题。

### 2. 视觉增强与多模态智能体
随着多模态大模型（MLLMs）的发展，利用视觉信号增强智能体能力成为新趋势：
- **AgentOCR**：尝试将智能体历史重新想象为图像，通过光学自压缩（Optical Self-Compression）来减少 Token 消耗。
- **布局感知模型**：许多研究关注如何让模型更好地理解文档布局（Layout Analysis），但将其应用于动态生成的智能体内存组织仍是一个较新的领域。

### 3. 证据提取与摘要
传统的摘要方法（Summarization）虽然能压缩信息，但往往会丢失溯源性（Traceability）。MEMO 借鉴了证据提取的思想，强调保留原始证据的片段（Spans），以确保决策的可靠性。

Q3: 论文如何解决这个问题？

### 1. 证据提取器（Evidence Extractor）
MEMO 首先对原始内存记录进行细粒度处理。提取器会根据当前查询（Query）识别相关的源块，并预测包含关键事实的字符级跨度（Spans）。这些跨度与原始记录共同构成“可溯源证据单元”（Traceable Evidence Units）。每个单元都保留了其来源信息，并明确了哪些细节是关键的。

### 2. 查询驱动的内存管理器（Memory Manager）
这是 MEMO 的核心决策组件，负责在 Token 预算约束下进行多维调度：
- **模态分配**：为每个证据单元决定是使用文本通道、视觉通道还是双通道（Dual-channel）。
- **布局选择**：根据证据的结构特征（如列表、对比、因果关系）选择最合适的视觉呈现模板。
- **预算分配**：在有限的 Token 总量内，动态调整各单元的权重。

### 3. 确定性内存构建模块（Deterministic Construction）
根据管理器的决策，该模块执行具体的生成工作：
- **文本打包**：将分配给文本通道的证据串联成紧凑的上下文。
- **视觉页面渲染**：将分配给视觉通道的证据按照选定布局渲染成图像页面。视觉呈现可以利用字体大小、颜色或位置来强调提取器识别出的关键跨度。

### 4. 基于反馈的训练策略
内存管理器通过离线阅读器（Offline Reader）的反馈进行训练。系统会评估不同内存计划（Plan）下阅读器回答问题的准确性，利用这种效用度量来优化管理器的决策逻辑，使其生成的内存组织形式最符合下游 LLM 的阅读偏好。

Q4: 论文做了哪些实验？

### 1. 实验基准与数据集
研究团队在四个具有代表性的基准上进行了测试，涵盖了不同的内存需求场景：
- **HotpotQA & 2WikiMultiHopQA**：测试多跳推理和从大量文档中提取证据的能力。
- **LoCoMo**：长上下文内存基准，考察模型处理极长历史记录的能力。
- **ALFWorld**：具身智能体任务，测试在连续动作空间中的决策与记忆调用。

### 2. 阅读器后端（Reader Backends）
为了验证通用性，实验采用了多种多模态大模型作为阅读器：
- **Qwen3-VL-32B**（主要测试对象）
- 其他主流开源或闭源多模态模型，以评估 MEMO 的跨模型迁移能力。

### 3. 实验设置与基准线
- **预算约束**：重点对比了在极低预算（如 128 Tokens）和标准窗口（如 4096 Tokens）下的表现。
- **对比方法**：包括纯文本检索（Text-only Retrieval）、纯视觉渲染（Visual-only Rendering）以及现有的内存压缩 SOTA 方法。
- **评估指标**：Exact Match (EM)、F1 Score 以及 Token 消耗量。

Q5: 发现了什么实验现象？

### 1. 极低预算下的显著优势
在 128 Token 的严苛预算下，使用 Qwen3-VL-32B 作为阅读器时，MEMO 在所有基准测试中均取得了最佳的 EM 和 F1 分数。这证明了多模态组织在信息密度极高时的优越性。

### 2. 高效的 Token 利用率
在 4096 Token 的大窗口设置下，MEMO 平均仅使用 83.93 个 Token 就能达到甚至超过其他方法使用数千 Token 的效果。这表明 MEMO 能够极大地压缩冗余，同时保留关键证据。

### 3. 消融实验发现
- **证据提取的贡献**：消融研究显示，证据提取器是准确率提升和压缩的主要来源，因为它在呈现之前就过滤掉了大量无关信息。
- **模态决策的价值**：内存管理器通过动态选择模态，进一步提升了复杂任务（如多跳问答）的性能，因为某些结构化证据在视觉下更易被模型捕获。

### 4. 任务间的张力与失败模式
- 在某些纯叙述性极强的任务中，视觉模态的收益会收窄，文本模态表现出更强的鲁棒性。
- 当 Token 预算极度匮乏时，如果管理器错误地选择了复杂的视觉布局，可能会导致关键细节被过度压缩而模糊，这是未来需要优化的边界情况。

Q6: 有什么可以进一步探索的点？

### 1. 动态预算分配算法
目前 MEMO 在固定预算下表现优异，未来可以探索如何根据任务难度动态调整 Token 预算，实现更灵活的资源管理。

### 2. 更多模态的整合
除了文本和视觉，智能体内存还可以包含音频、结构化数据流或代码执行轨迹。将 MEMO 扩展到支持更多模态的统一组织是一个重要方向。

### 3. 在线学习与实时进化
目前的管理器主要通过离线反馈训练。开发一种能够根据智能体实时交互反馈进行在线更新的内存管理机制，将增强其在未知环境中的适应性。

### 4. 视觉渲染的端到端优化
目前视觉页面是确定性渲染的，未来可以尝试将渲染过程也纳入可学习框架，让模型自主学习最有利于自身理解的“视觉语言”布局。

Q7: 总结一下论文的主要内容

这篇论文介绍了 MEMO，一种为长程 LLM 智能体设计的创新内存组织框架。针对智能体在长期运行中面临的内存爆炸与上下文窗口限制之间的矛盾，MEMO 抛弃了传统的单一文本或单一视觉读取模式，转而采用一种查询驱动的多模态证据组织策略。其核心流程包括：首先通过证据提取器从海量历史中识别关键事实跨度，形成证据单元；随后由内存管理器根据当前任务需求，为每个单元智能分配最适合的呈现模态（文本或视觉）及布局；最后生成最终的内存包供阅读器模型使用。实验结果表明，MEMO 在 HotpotQA、ALFWorld 等多个基准测试中表现卓越，尤其是在 Token 预算受限的情况下，它能以极低的成本（平均约 84 tokens）实现高精度的任务执行。该研究不仅提升了智能体的内存效率，也为多模态大模型如何更有效地消费外部知识提供了新的范式，即通过“证据级”的模态调度来平衡保真度与结构化表达。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对于关注智能体（Agent）长程记忆管理的研究者具有高度参考价值。

## 基本信息

- 作者：Xian Gao, Jinpeng Wang, Jiacheng Ruan, Guangyu Cao, Ting Liu, Yuzhuo Fu
- 机构：哈尔滨工业大学 (Harbin Institute of Technology), 上海交通大学 (Shanghai Jiao Tong University)
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-09-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.07471v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 MEMO 的三阶段架构、实验预算设置（128 vs 4096 tokens）以及消融实验的结论。
