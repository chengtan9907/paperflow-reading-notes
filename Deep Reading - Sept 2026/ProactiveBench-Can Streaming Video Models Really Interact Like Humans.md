---
user_id: "cheng tan"
paper_id: 11391
arxiv_id: "2609.12658"
title: "ProactiveBench: Can Streaming Video Models Really Interact Like Humans?"
publish_date: "2026-09-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.12658.pdf"
pdf_url: "https://arxiv.org/pdf/2609.12658"
abs_url: "https://arxiv.org/abs/2609.12658"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-14T10:22:16"
---
# ProactiveBench: Can Streaming Video Models Really Interact Like Humans?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：streaming video understanding · proactive interaction · response timing · multimodal models

## 一句话总结

ProactiveBench 是一个针对流式视频多模态模型的基准测试，旨在评估模型在无显式提示的情况下，自主决定何时响应以及何时保持沉默的主动交互能力。

## 摘要

> Streaming video understanding requires models to process continuous multimodal input while maintaining temporal context. Existing evaluations are predominantly reactive: they query a model at a selected timestamp and therefore do not assess when it should respond. Proactive interaction instead requires monitoring a standing request, responding within an appropriate interval after the target event, and otherwise remaining silent. We introduce ProactiveBench, which evaluates models at one-second stream intervals without an explicit response cue. Its six subtasks vary trigger ambiguity and timing tolerance. Event Sensitivity geometrically combines response and silence rates on the same recording; four window-based subtasks distinguish early, in-window, and missed responses; and Duplicate Counting penalizes omissions and repetitions. Premature responses outnumber missed responses for four of the six evaluated systems, revealing a substantial gap in the temporal decision-making required for human-like interaction.
> Index Terms— streaming video understanding, proactive interaction, response timing, multimodal models, real-time decision making

Q1: 这篇论文试图解决什么问题？

### 核心挑战：从“反应式”到“主动式”的转变
1. **现有评测的局限性**：传统的视频问答（VideoQA）或视频理解任务通常是反应式的。评测者会在特定的时间戳向模型提问，这实际上是给了模型一个“现在请回答”的显式信号。但在真实场景中，智能体需要自主判断何时该说话。
2. **时间决策的缺失**：模型不仅需要理解视频内容，还需要具备时间感知能力，以决定在流式输入中哪个时刻触发响应是最优的。过早响应可能导致信息不全，过晚响应则失去交互意义。
3. **沉默的重要性**：在没有相关事件发生时，模型必须保持沉默。现有的评测往往忽略了对“持续沉默”能力的量化评估，导致模型可能通过高频响应来刷高召回率。

### 关键科学问题
- 模型能否在没有外部触发信号的情况下，根据常驻请求（Standing Request）自主识别目标事件？
- 模型在处理具有时间歧义性的触发条件时，能否在合理的窗口内做出反应？
- 模型是否能够区分“已处理事件”和“新发生事件”，从而避免重复啰嗦？

Q2: 有哪些相关研究？

### 相关研究领域
1. **流式视频理解 (Streaming Video Understanding)**：侧重于在线动作检测或视频流中的目标跟踪，但通常不涉及复杂的自然语言交互决策。
2. **主动交互系统 (Proactive Interaction Systems)**：早期的研究探讨了机器人或对话系统的主动性，但缺乏在大规模多模态长视频流上的系统性基准。
3. **多模态大模型 (LMMs)**：虽然 LMM 在静态图像和短视频剪辑上表现优异，但在处理长时序流式输入时的推理效率和时间决策能力仍是前沿难题。
4. **现有基准对比**：论文指出，虽然已有部分研究关注自发响应，但 ProactiveBench 通过引入“沉默率”和“早发响应惩罚”，提供了更严苛的时间决策评估维度。

Q3: 论文如何解决这个问题？

### ProactiveBench 设计框架
1. **流式输入协议**：模型以 1 秒为步长接收视频帧和音频流，必须在每个时间步输出“响应内容”或“[SILENCE]”标记。
2. **常驻请求 (Standing Request)**：在视频开始前给定一个任务指令（如“当有人进入房间时提醒我”），模型需在整个视频流中监控该请求的触发条件。

### 六大子任务体系
- **事件敏感度 (Event Sensitivity, ES)**：通过几何平均响应率（Response Rate）和沉默率（Silence Rate）来评估模型对事件触发的整体敏感度。
- **窗口化响应任务**：
 - **早发响应 (Early Response)**：在事件发生前或证据不足时触发。
 - **窗口内响应 (In-window Response)**：在预定义的合理时间区间内触发。
 - **漏报 (Missed Response)**：事件发生后未能在窗口内响应。
- **重复计数 (Duplicate Counting, DC)**：评估模型是否会对同一个已识别事件进行多次重复提醒，惩罚冗余输出。
- **异常警报 (Anomaly Alert, AA)**：针对突发性、非预期的视觉/听觉事件的即时反应能力。
- **持续沉默 (Sustained Silence, SS)**：在长达数分钟的无相关事件视频中，评估模型保持沉默而不产生幻觉响应的能力。

### 数据与标注
- 每个示例包含源录像、精确的时间定位标签、常驻请求以及参考响应文本。子任务间采用不相交的录像数据，防止模型通过记忆特定视频来作弊。

Q4: 论文做了哪些实验？

### 实验设置
- **评估对象**：选取了 6 个具有代表性的流式多模态模型（涵盖了基于 Transformer 和循环机制的不同架构）。
- **输入格式**：模型持续接收 1fps 的视频流，不提供任何关于事件何时开始的元数据。
- **评估指标**：除了传统的文本相似度外，核心指标是时间精度指标（如响应延迟、早发率、漏报率）。

### 实验对比维度
- **触发歧义性**：对比模型在“明确触发条件”与“模糊触发条件”下的表现差异。
- **时间容忍度**：测试模型在不同长度的响应窗口（从 0 秒容忍到随事件时长变化的动态窗口）下的鲁棒性。

Q5: 发现了什么实验现象？

### 关键实验发现
1. **早发响应倾向 (Premature Response Bias)**：在 6 个受测系统中，有 4 个系统的早发响应数量显著多于漏报数量。这表明当前模型非常“急躁”，往往在事件刚刚露出苗头甚至尚未发生时就急于做出判断。
2. **时间决策鸿沟**：模型在“决定何时说话”上的表现远逊于其在“理解发生了什么”上的表现。即使模型能正确识别事件，其触发时机也往往偏离人类预期的最佳窗口。
3. **指标间的张力**：提高响应率往往会以牺牲沉默率为代价。模型难以在保持高召回率的同时，在无关片段中维持完美的沉默。
4. **歧义性挑战**：对于起始点模糊的事件（如“当气氛变得紧张时”），模型的响应时机表现出极大的不确定性，容易在窗口外徘徊。
5. **重复响应问题**：部分模型缺乏对已生成内容的记忆，导致在长事件持续期间不断重复相同的提醒，DC 指标表现较差。

Q6: 有什么可以进一步探索的点？

### 可探索的研究方向
1. **延迟与精度的帕累托优化**：如何设计新的损失函数，平衡模型的推理延迟与时间触发的准确性。
2. **长时上下文的显式建模**：引入更高效的记忆机制（如状态空间模型 SSM 或长效缓存），以维持对常驻请求的长期关注。
3. **强化学习时机对齐**：利用强化学习（RLHF 的变体）来训练模型学习人类偏好的响应时机，而不仅仅是预测下一个 token。
4. **多模态流式架构改进**：探索能够原生处理异步多模态流的架构，减少因特征对齐导致的时间感知偏差。
5. **交互式反馈学习**：研究模型在被用户纠正响应时机后，如何在线调整其后续的主动交互策略。

Q7: 总结一下论文的主要内容

本文提出了 ProactiveBench，这是一个旨在填补流式视频理解中“主动交互”评估空白的基准测试。研究的核心逻辑在于：真正的智能体不应只是被动地回答问题，而应学会在流式环境中自主决策响应时机。论文通过设计六个维度的子任务，系统地考察了模型在处理常驻请求时的响应精度、沉默质量以及对重复信息的抑制能力。实验结果揭示了一个重要的技术现状：当前的多模态模型普遍存在“早发响应”的缺陷，即在证据不足时过度触发，这反映了模型在时间因果性和决策耐心方面的不足。ProactiveBench 为未来开发更具类人交互特征的流式 AI 助手提供了标准化的评测框架和数据支持，强调了时间决策在多模态智能体研究中的核心地位。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：为智能体（Agent）在长时监控、智能家居或辅助驾驶等场景下的自主交互提供了评测框架。

## 基本信息

- 作者：Kaixuan Du, Xin Wan, YuKun Wang, Hang Zhang, Meng Cao, Dai Guan, Ming Chen, Ni Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-09-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2609.12658`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，重点提取了 Abstract、Conclusion 以及关于子任务设计和实验观察的核心结论。
