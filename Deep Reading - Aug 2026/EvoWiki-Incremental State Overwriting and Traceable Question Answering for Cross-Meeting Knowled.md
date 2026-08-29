---
user_id: "cheng tan"
paper_id: 9161
arxiv_id: "2608.23265v1"
title: "EvoWiki: Incremental State Overwriting and Traceable Question Answering for Cross-Meeting Knowledge Evolution"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23265v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23265v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:09:08"
---
# EvoWiki: Incremental State Overwriting and Traceable Question Answering for Cross-Meeting Knowledge Evolution

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：state overwriting · entity version chains · cross-meeting question answering · temporal reasoning

## 一句话总结

EvoWiki提出了一种面向跨会议知识演化的增量式问答架构，通过构建动态Wiki并采用实体版本链与细粒度状态覆盖协议来区分当前有效状态与被取代历史，在读取阶段以确定性寻址替代基于相关性的Top-k检索，从而在六个数据集和两个读者模型上显著提升了可追溯问答的准确性与事实忠实度。

## 摘要

> In long-term collaboration spanning multiple meetings, factual states such as decisions, risks, and ownership are continually revised, overturned, and replaced. Existing long-context methods typically stack the entire history, while many RAG, LLM-Wiki, and structured-memory methods organize knowledge as static or append-only facts and rely on semantic relevance at read time. Without explicit modeling of intrameeting decision processes and knowledge lifecycles, these approaches may retain conflicting old and new states simultaneously or discard history when updating snapshots, leading to stale retrieval and answers that are difficult to verify. We present EvoWiki (Evolving Wiki), an incremental question-answering architecture for dynamic long-form text. EvoWiki decouples offline incremental construction (BUILD) from online structured reading (READ). BUILD captures the intrameeting micro-evolution from proposal through discussion to decision and uses entity version chains and a fine-grained State-Overwrite Protocol to explicitly distinguish current valid states from superseded history while preserving meeting-level provenance anchors. READ bypasses relevance-based Top-k retrieval over raw meetings and performs deterministic entity addressing, temporal resolution, and cross-entity multi-hop aggregation over the complete Wiki to produce grounded and traceable answers. We further introduce CrossMeet, a high-fidelity bilingual benchmark derived from real-world business seeds and designed to simulate long-term state evolution, covering factual consistency, temporal reasoning, and cross-meeting multi-hop reasoning. Across six datasets and two reader models, EvoWiki improves macro-average Judge Accuracy over the strongest baselines by 9.72 and 10.00 percentage points, respectively. Further analyses and human evaluation show that EvoWiki is more robust and factually faithful under frequent state flips, validating valid-state-oriented evolutionary reading as a more reliable technical approach to cross-meeting knowledge evolution.

Q1: 这篇论文试图解决什么问题？

论文要解决的核心问题是：在长期协作中，跨会议问答必须识别查询时间点上仍然有效的事实状态，并追溯到其来源会议；然而现有方法无法有效处理事实状态的演化、版本替换和冲突。具体地，1）长上下文模型把所有会议历史放进一个窗口，但名义上的长上下文并不保证可靠用证或推理；2）传统RAG按语义相关性排序证据而非有效性，对于同一实体的过时和当前陈述可能赋予相似相关性；3）LLM-Wiki和结构化记忆方法改善了知识组织，但不建模跨会议决策的生命周期和替换关系，导致被取代状态与当前状态共存，引发陈旧检索。论文还指出现有任务不足以覆盖角色绑定、版本替换和跨会议多跳证据组合。因此，需要一个显式建模决策微观演化、支持状态覆盖和历史保留、并能支持可追溯问答的机制。

Q2: 有哪些相关研究？

相关工作可归纳为几类：1）长上下文建模：如Hsieh et al. 2024、Yen et al. 2025、Modarressi et al. 2025指出仅扩展上下文窗口并不能保证可靠证据使用和推理；2）传统RAG（Lewis et al. 2020）按语义相关性检索，但忽略时间有效性；3）时间感知和冲突感知方法（Vu et al. 2024; Wang et al. 2025; Zhang et al. 2024; Schumacher et al. 2025; Hou et al. 2025; Liska et al. 2022）处理时间敏感和到达知识，但可能仍无法区分同一实体的新旧状态；4）参数编辑研究（可能涉及事实定位和多跳一致性，如Li et al. 2026、Chen et al. 2026b）聚焦于模型参数中的事实更新，但不同于显式状态管理；5）LLM-Wiki和结构化记忆（Sarthi et al. 2024; Edge et al. 2024; Gutiérrez et al. 2025）改进了知识组织，但不建模生命周期和替换关系。此外，会议理解和跨会话知识更新也是相关领域（Prasad et al. 2023; Thonet, Besacier, and Rozen 2025; Wu et al. 2024）。论文在这些基础上强调了“写时消歧、读时确定”的核心思想。

Q3: 论文如何解决这个问题？

EvoWiki把离线增量构建（BUILD）与在线结构化读取（READ）解耦。BUILD负责将原始会议文本转换为动态Wiki：它捕获会议内部从提案、讨论到决策的微观演化过程，为每个实体维护版本链；通过细粒度的State-Overwrite Protocol显式区分当前有效状态与已被取代的历史状态，同时保留会议级别的来源锚点（provenance），使每次写入都对应到具体的会议和话轮。BUILD还在写时进行指代消解，确保实体引用正确合并。READ阶段不再访问原始会议，而是基于构建好的完整Wiki进行确定性操作：实体寻址（entity addressing）直接定位到目标实体，时间解析（temporal resolution）根据查询时间确定有效版本，跨实体多跳聚合（cross-entity multi-hop aggregation）将分散在不同实体或版本中的信息组合起来，从而生成完整、可追踪的答案。论文强调这种不对称设计将歧义消解和冲突解决移到写时，使读时保持高效和可解释。

Q4: 论文做了哪些实验？

论文评估了EvoWiki在六个数据集和两个读者模型上的性能。主要数据来自新提出的CrossMeet基准：该基准源于真实业务种子，包含10个高保真模拟会议、2000个QA对，平均上下文长度超过26000个token，并带有问题类型、推理跳数和跨会议证据链标注；从检索证据来看，CrossMeet采用英语和中文双语。评估指标为Judge Accuracy（宏观平均）。实验还设计了状态翻转（state-flip）、证据位置（evidence-position）等分析场景，并进行了人类评估：三位具有NLP背景的研究者独立对200个CrossMeet-EN问题进行盲评，将EvoWiki的回答与匿名基线回答配对比较。论文报告，在六个数据集和两个读者设置上，EvoWiki的宏观平均Judge Accuracy分别达到60.09和63.02，比最强基线高出9.72和10.00个百分点。

Q5: 发现了什么实验现象？

实验观察表明：1）在频繁状态翻转的场景下，EvoWiki比基线更加稳健和事实忠实，说明显式状态覆盖和版本链管理能有效避免陈旧与当前状态混杂；2）证据位置分析显示EvoWiki的全Wiki确定性寻址策略能够应对远程证据组合，而基线在依赖长距离证据时可能失效；3）人类评估中，三位NLP背景的研究者盲评200个CrossMeet-EN问题，结果显示EvoWiki生成的答案在可接受性和可追踪性上优于基线（具体偏好比例未在给出的信息中量化）；4）EvoWiki的Wiki-only READ受限于BUILD阶段提取的完整性，在ASR噪声和非正式口语条件下，信息丢失会传导到答案质量上，这是一个需要正视的失败模式。

Q6: 有什么可以进一步探索的点？

论文在结论中提出了未来工作方向：不确定性感知的写入（uncertainty-aware writing），即当提取状态不确定时如何表达；选择性源验证（selective source verification），用于在发现冲突或低置信度时主动回查原始会议；以及扩展到更广泛的语言、领域和时间范围。基于此可以进一步探索：1）将不确定性估计引入状态覆盖协议，在写时为低置信度实体保留候选版本；2）利用大模型验证机制在READ阶段对关键事实进行交叉验证；3）将EvoWiki与参数编辑方法结合，使LLM的隐式知识也随Wiki同步更新；4）支持流式会议输入，使BUILD能够在会议进行中实时更新；5）扩展至多模态会议（含语音、幻灯片），以及非英语语言；6）研究更长时间跨度（如跨季度）的状态演化，探索遗忘机制与重要性加权。

Q7: 总结一下论文的主要内容

EvoWiki旨在解决跨会议问答中知识状态随时间演化导致的陈旧检索和不可追溯问题。论文指出现有长上下文、RAG、LLM-Wiki等方法要么堆叠全部历史导致证据冲突，要么将知识视为静态事实忽视生命周期；因此需要显式建模“提案-讨论-决策”的微观演化。为此，EvoWiki采用双阶段架构：离线BUILD阶段从原始会议中抽取实体、决策和关系，利用实体版本链和State-Overwrite Protocol区分当前有效状态与历史状态，同时记录会议级来源；写时进行指代消解，将冲突解决放在写入端。在线READ阶段则完全基于构建好的Wiki进行确定性实体寻址、时间解析和跨实体多跳聚合，避免了相关性检索的模糊性。论文还构建了双语基准CrossMeet，包含10个高保真会议、2000个QA对、平均26K+ tokens，覆盖事实一致性、时间推理和跨会议多跳问答。在六个数据集和两个LLM reader上，EvoWiki的Judge Accuracy（宏观平均）达到60.09和63.02，比最强基线高9.72和10.00个百分点。状态翻转、证据位置和人类评估进一步证明其稳健性和可追溯性。主要局限是依赖BUILD提取质量，在ASR噪声和非正式口语下可能丢失信息；未来工作包括不确定性感知写入、选择性源验证和多语言、多领域、长时间跨度扩展。整体上，EvoWiki通过“写时区分、读时确定”的设计实现了更可靠的知识演化和问答。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：EvoWiki提供了一种可演进的结构化记忆范式，对于构建长期运行且需要追踪事实变化的智能体（agent）具有直接借鉴价值，尤其是读写分离和状态覆盖协议。

## 基本信息

- 作者：Dongsheng Chen, Tianyu Wang, Wenhui Que
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23265v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文摘要、Introduction和Conclusion片段，以及检索到的证据块；检索证据补充了CrossMeet数据规模（10个会议、2000 QA对、26K+ tokens）和人类评估细节，但未提供完整实验表格和消融量化结果。
