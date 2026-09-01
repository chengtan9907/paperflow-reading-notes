---
user_id: "cheng tan"
paper_id: 10011
arxiv_id: "2608.30163v1"
title: "Doc-REFRAG: Rethinking Multimodal Document Retrieval-Augmented Generation"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.30163v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.30163v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-02T01:45:21"
---
# Doc-REFRAG: Rethinking Multimodal Document Retrieval-Augmented Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal document retrieval-augmented generation · visual token compression · reinforcement learning selection · multi-image reasoning

## 一句话总结

本文提出 Doc-REFRAG，一个面向多图像文档检索增强生成的高效解码框架，搭配新构建的大规模数据集 DocLongRAG（343K 问答对、平均每问 37.4 张检索图），通过问题引导的视觉 token 压缩与基于强化学习的选择性展开，在六个基准上超越十一类强基线并显著降低推理延迟。

## 摘要

> Real-world knowledge resides in multimodal documents, necessitating retrieval-augmented generation (RAG) for accurate question answering. However, existing multimodal RAG models are primarily designed for single-image or closed-document settings and exhibit limited accuracy in realistic multi-image scenarios. Moreover, processing numerous retrieved images incurs substantial computational overhead from irrelevant visual tokens. To address these challenges, we introduce DocLongRAG, a large-scale dataset of 343K question--answer pairs, each associated with an average of 37.4 retrieved images to reflect authentic RAG workflows. Building on this dataset, we propose Doc-REFRAG, a question-guided framework that compresses visual tokens into coarse chunks and selectively expands question-relevant ones via a lightweight RL-based selector. Experiments on six benchmarks show that Doc-REFRAG outperforms eleven strong baselines, achieving state-of-the-art accuracy with significantly lower inference latency. Our resources are available at https://github.com/Collab-Gen/Doc-REFRAG.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：在真实世界的多模态文档 RAG 场景中，现有方法在（1）多图像设定下精度不足，以及（2）大量检索图像带来的高计算开销这两个方面存在明显的短板。具体来说：
1. 任务设定错位：已有多模态 RAG 模型大多在单图像或封闭文档（如单页 PDF）上训练和评估，而真实 RAG 流程往往返回数十张甚至更多的图像；模型在长序列、跨图像推理上未经过充分训练，导致准确率下降。
2. 计算效率瓶颈：检索返回的许多图像与问题无关或仅部分相关，但标准 MLLM 会把所有图像的视觉 token 全部送入解码器，造成大量无用计算。
3. 压缩与精度的矛盾：直接粗暴压缩视觉 token 会丢失细节，导致答案不准确；而保留全部 token 则效率低下。需要一种既能压缩又能在需要时恢复细节的机制。
4. 训练数据缺失：缺乏模拟真实多图像 RAG 流程的大规模监督数据，阻碍了模型在该场景下的训练与评测。
作者将问题定义为“如何在保留必要视觉细节的同时，把多图像文档 RAG 的输入压缩到可负担的序列长度，并最大化问答精度”。

Q2: 有哪些相关研究？

根据检索到的证据，相关工作可归纳为：
1. 多模态 RAG 中的检索器设计：现有研究集中于 retriever 设计（Yu et al., 2024; Yan et al., 2025; Zhang et al., 2025a; Hu et al., 2025; Yang et al., 2025; Hu et al., 2026），强调如何从文档中检索相关图像或段落，但多数未考虑下游 MLLM 的多图像长序列处理能力。
2. 多模态文档理解模型（Document Understanding Models in RAG）：背景部分提到 RAG 通过检索外部知识增强 MLLM，而文档理解模型在 RAG 流水线中承担解析版式、提取信息等角色；但这些模型通常在封闭文档上评测。
3. 视觉 token 压缩方法：方法对比部分提到两类范式——启发式压缩（选择视觉上复杂的块，假设这类区域对下游任务信息量更大）和训练感知方法（引入线性投影器，用少量粗粒度 token 结合细粒度视觉细节，如 Li et al., 2025; Hu et al., 2024b）。这两类方法都存在不足：启发式方法不依赖问题，无法区分与问题无关的复杂区域；训练感知方法受限于固定 token 预算，难以自适应。
4. 强化学习用于视觉 token 选择：本文的 RL 选择器属于该方向，但检索证据未明确点评其他 RL 选择工作。
需要说明：由于检索片段有限，对相关工作的完整覆盖可能不全，部分引用仅能基于片段确认。

Q3: 论文如何解决这个问题？

论文提出的 Doc-REFRAG 是一个问题引导的高效解码框架，整体思路是“先粗后细，按需展开”。
1. 问题引导的视觉 token 压缩：基于问题内容生成注意力引导的粗粒度视觉块（chunks），将原始图像 token 压缩成固定数量（k）的粗块。这里的关键是“问题引导”，即压缩不是纯视觉显著性驱动，而是结合问题语义，使保留的粗块更可能覆盖答案所需信息。
2. 轻量级 RL 选择器：训练一个小型强化学习选择器，负责判断哪些粗块与问题相关，并在解码时只展开这些被选为相关的块，恢复其细粒度视觉 token，而忽略无关块。RL 的奖励信号可设计为与最终答案准确性相关（局限部分提到“以答案准确率作为唯一奖励信号”，可推断）。
3. 三阶段训练范式：结论片段提到“a three-stage training paradigm”，具体阶段可能包括：先在 DocLongRAG 上训练压缩器与选择器，再联合微调生成器，最后用 RL 优化选择策略。由于摘要未给出各阶段细节，此处为合理推断。
4. 与 DocLongRAG 数据集的配合：数据集提供平均 37.4 张检索图像的长序列监督样本，使框架能在真实长距离多图像推理场景下训练。
整体上，Doc-REFRAG 属于“压缩-选择-展开”的级联机制，通过 RL 优化选择策略来平衡压缩率与信息保留。

Q4: 论文做了哪些实验？

论文在六个基准上评估，并与十一个强基线进行对比。实验包括：
1. 主实验：在六个多模态文档 RAG 基准上对比 Doc-REFRAG 与 11 个基线（包括启发式压缩方法、训练感知压缩方法、标准 MLLM RAG 等），报告准确率与推理延迟。
2. 效率评估：对比不同方法的推理延迟，Doc-REFRAG 实现了显著更低的延迟。
3. 消融研究：可推测对 chunk size k、RL 选择器、问题引导等组件进行了消融，以验证各部分贡献（具体消融指标未在摘要中列出，属于推测）。
4. 错误分析：局限部分提到在附录 L 中分析了剩余错误，64% 的错误集中在密集或复杂版面上，说明实验包含错误类型分布分析。
5. 数据规模验证：DocLongRAG 数据集包含 343,474 个问答对，平均每个问题 37.4 张图像，用于训练和评测长程推理。
需要注意：摘要中未列出具体的基准名称、基线的具体名称和数值，因此实验细节需回原文确认。

Q5: 发现了什么实验现象？

从摘要与检索证据可提取的实验现象包括：
1. 精度与效率的双重优势：Doc-REFRAG 在六个基准上超过 11 个强基线，达到 SOTA 精度，同时推理延迟显著更低，说明“压缩-选择-展开”机制确实能在不损失精度的前提下减少冗余计算。
2. 固定 chunk size 的适应性问题：k=3 被设为训练超参数，整体表现最好，但 k 无法在推理时动态调整，说明固定压缩率会限制模型在不同复杂度输入上的自适应能力。
3. 错误集中于密集/复杂版面：根据附录 L 的分析，64% 的剩余错误集中在密集或复杂版面上，提示视觉信息密度高的区域仍是主要失败模式，可能因为压缩或选择在这些区域丢失了关键细节。
4. 奖励信号单一导致的归因缺失：RL 选择器仅以答案准确率为奖励，无法细粒度判断哪些 chunk 真正贡献了正确答案，这会影响选择器的可解释性与优化信号质量。
5. 现有方法的对比倾向：论文指出启发式方法偏好视觉复杂 patch，这种策略可能被与问题无关的复杂背景误导；训练感知方法则受限于固定 token 预算。这些对比暗示 Doc-REFRAG 的相比优势来自“问题引导”而非纯视觉。
需要说明：具体的数值结果和消融趋势未在检索证据中给出，以上观察多数来自摘要与局限部分的定性描述，更细的量化结果待原文。

Q6: 有什么可以进一步探索的点？

基于论文的局限和现有思路，可拓展的方向包括：
1. 动态自适应 chunk size：将训练超参数 k 改为推理时可根据问题复杂度或图像数量自动调整的机制，例如用学习到的控制器决定每个文档的压缩率。
2. 更丰富的 RL 奖励设计：在答案准确率之外，引入细粒度 chunk 级归因奖励或可解释性约束，使选择器获得更 dense 的监督信号。
3. 面向密集/复杂版面的鲁棒性提升：针对 64% 错误集中的密集或复杂版面，设计专门的版面感知压缩或选择策略，例如结合 OCR、布局分析或版面树。
4. 跨领域与科学应用迁移：数据集 DocLongRAG 若以通用文档为主，可构建医学报告、科学文献、法律卷宗等领域的多图像 RAG 变体，验证框架的迁移能力。
5. 训练效率优化：三阶段训练流程的每一阶段都可改进，例如用更轻量的选择器架构、蒸馏方法或课程学习来降低训练成本。
6. 扩展检索器与生成器的联合优化：当前工作主要聚焦解码侧，可进一步将检索器纳入端到端训练，实现“检索-压缩-选择-生成”的全局优化。
7. 真实系统部署：研究在超长文档集合上的可扩展性、缓存机制和流式处理，使框架能处理更大规模的检索结果。

Q7: 总结一下论文的主要内容

本文聚焦多模态文档上的检索增强生成（RAG），指出现有模型的两个关键缺陷：一是主要面向单图像或封闭文档，在真实多图像场景下精度有限；二是处理大量检索图像时，无关视觉 token 带来大量计算开销。为了推动这一方向的发展，作者首先构建了 DocLongRAG 数据集，包含 343,474 个问答对，平均每个问题关联 37.4 张检索图像，用于反映真实 RAG 流程中的长距离推理和多种格式覆盖。随后提出 Doc-REFRAG 框架，其核心是“问题引导的视觉 token 压缩”与“选择性展开”：先用问题引导机制把视觉 token 压缩为粗粒度 chunk（chunk size k 作为训练超参数，实验取 k=3 最佳），再训练一个轻量级 RL 选择器，判断哪些 chunk 与问题相关，仅在解码时展开这些相关 chunk 的细粒度视觉信息，从而降低无效计算。训练采用三阶段范式（具体阶段细节原文未在摘要中展开，推测包括压缩器/选择器的预训练、生成器微调与 RL 优化）。在六个基准上与 11 个强基线对比，Doc-REFRAG 取得 SOTA 精度并显著降低推理延迟。局限方面，作者指出 chunk size 固定不可动态调整，RL 选择器以答案准确率为唯一奖励导致缺乏 chunk 级归因，且附录错误分析显示 64% 的剩余错误集中于密集或复杂版面。整体而言，这项工作同时贡献了新数据集与新的高效解码框架，为多图像多模态文档 RAG 提供了一条兼顾精度与效率的路线。代码与资源已公开。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文属于多模态 RAG 与视觉 token 压缩的交叉方向，与生成（generation）研究高度相关，适合关注长上下文多模态生成的读者。

## 基本信息

- 作者：Ruofan Hu, Shengyang Xu, Minjie Hong, Xiaoda Yang, Sashuai Zhou, Ke Lei, Tao Jin, Zhou Zhao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.IR, cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.30163v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了检索到的摘要、Introduction、Conclusion、Related Work 与 Limitations 片段；字段中未覆盖的细节基于论文摘要与片段进行合理推断，并已在相应位置标注推测。
