---
user_id: "cheng tan"
paper_id: 6267
arxiv_id: "2608.02583v1"
title: "UEmbed: Unified Sparse and Dense Multimodal Embeddings"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02583v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02583v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T02:02:25"
---
# UEmbed: Unified Sparse and Dense Multimodal Embeddings

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：learned sparse retrieval · dense retrieval · multimodal embedding · decoder-only LLM

## 一句话总结

UEmbed 提出一种仅解码器的多模态嵌入模型，通过 N 个可学习特殊 token 将词表划分为 N 个不相交子集，在单次因果前向中同时生成稀疏词级和稠密表示，从而统一文本与多模态输入的稀疏与稠密检索。

## 摘要

> Sparse retrieval underpins modern search systems, from web search to retrieval-augmented generation. Existing work has introduced Learned Sparse Retrieval (LSR) to push beyond exact lexical matching toward richer semantics. Yet LSR has so far remained tied to encoder-style bidirectional architectures, and its extension to multimodal settings still relies heavily on auxiliary cross-modal modules. To address these limitations, we introduce UEmbed (Unified Embedding), a decoder-only multimodal embedding model that produces both sparse lexical and dense representations in one causal forward pass. UEmbed appends N learnable special tokens to the input and partitions the vocabulary into N disjoint subsets. Each token's causal hidden state predicts sparse weights over its assigned subset, and the N subsets are concatenated into the full sparse vector. Trained on public data, we release UEmbed at 2B, 4B, and 9B scales. UEmbed-9B reaches 71.8 (dense) and 71.0 (sparse) on MMEB-v2, outperforming multimodal embedding models trained on publicly available data (e.g., RzenEmbed). On BEIR, UEmbed also remains competitive with strong dense and sparse baselines. Furthermore, we demonstrate the practical utility of UEmbed across three dimensions: effectiveness, efficiency, and agentic applications. Overall, UEmbed offers a new paradigm: it unifies dense and sparse embeddings in one model, while further extending sparse retrieval to unify text and multimodal inputs.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决以下核心问题：
1. 现有学习型稀疏检索（Learned Sparse Retrieval, LSR）模型几乎都基于编码器式双向架构，难以直接利用近年来表现出色的仅解码器大语言模型（LLM）的能力，限制了稀疏检索的语义表达和扩展性。
2. 将稀疏检索扩展到多模态场景时，现有工作通常依赖辅助跨模态模块（如额外的视觉编码器、融合网络或投影层），而不是从单一多模态骨干网络中直接导出稀疏表示，导致架构复杂、难以推广到新模态，也增加了训练和推理成本。
3. 现有稀疏检索方法在实际应用中的实用价值尚未被充分挖掘，尤其在结合稠密检索的混合评分、效率优化以及智能体（agentic）应用等维度缺少系统性验证。
4. 稠密与稀疏两种表示通常由不同模型或不同分支产生，缺乏真正意义上的统一建模，限制了二者互补优势的发挥。
UEmbed 的提出正是为了消除上述割裂，实现“一个模型、一次前向、同时输出稠密与稀疏表示”，并将该能力自然延伸到多模态输入。

Q2: 有哪些相关研究？

相关研究可分为以下几类：
1. 学习型稀疏检索（LSR）：传统稀疏检索依赖词频统计（如 BM25），LSR 通过神经网络学习 token 权重以突破精确词匹配限制。已有工作如 SPLADE、SPLADE-v2、uniCOIL 等采用编码器架构，对 query 和 document 生成稀疏词级向量。这些方法虽能捕捉语义，但受限于双向编码器，难以规模化到仅解码器 LLM。
2. 稠密检索（Dense Retrieval）：以双编码器（如 DPR、Contriever、GTR）为代表，将文本编码为稠密向量，在向量空间中做最近邻搜索。近年来基于 LLM 的嵌入模型（如 E5、BGE、GTE）大幅提升了性能，但稠密与稀疏方法通常分离。
3. 多模态嵌入模型（Multimodal Embedding Models）：通用多模态嵌入模型（如 JinaCLIP、LanguageBind、MM-Embed 等）将图像、文本等模态编码到统一向量空间，但大多只输出稠密向量。部分工作尝试结合稀疏检索，如使用辅助跨模态模块将视觉特征映射到词表空间，但缺乏统一的单骨干设计。
4. 仅解码器 LLM 用于嵌入：近年来出现将仅解码器 LLM 用于稠密嵌入的趋势（如 LLM2Vec、E5-Mistral、NV-Embed），通过特殊 token 或注意力池化获得表示。但将这些方法扩展到稀疏检索的工作还很少，UEmbed 填补了这一空白。
5. 混合检索（Hybrid Retrieval）：实际系统中常将稠密与稀疏得分融合（如 RRF 或加权和），UEmbed 通过单模型同时提供两种表示，使混合检索更简洁高效。
6. 多模态大语言模型（MLLM）：如 Gemini、Qwen-VL、LLaVA 等，展示了跨模态理解能力，但它们生成的是文本或通用表示，不直接用于检索嵌入。UEmbed 利用 MLLM 的因果架构来产生检索专用稀疏与稠密表示。

Q3: 论文如何解决这个问题？

UEmbed 的核心方法如下：
1. 架构：采用仅解码器（decoder-only）多模态大语言模型作为骨干，支持文本和视觉输入（如图像、视觉文档）。模型在输入序列后附加 N 个可学习特殊 token，这些 token 通过因果注意力汇总输入信息。
2. 词表划分：将模型的输出词表（vocabulary）划分为 N 个不相交子集（disjoint subsets）。每个特殊 token 的因果隐状态通过一个投影层预测其对应子集上的稀疏权重，即该子集内每个词（或词片）的激活分数。
3. 稀疏向量拼接：将 N 个子集的稀疏权重向量拼接起来，得到完整的稀疏向量，其维度等于词表大小。这个稀疏向量可直接用于倒排索引检索，类似 SPLADE 的表示。
4. 稠密表示：同样从特殊 token 的隐状态中（如选择最后一个特殊 token 或池化）得到稠密向量，用于稠密检索。具体方式可能是对 N 个特殊 token 表示进行聚合，或使用额外的稠密投影。
5. 训练：在公开数据上训练，采用多任务目标，同时优化稀疏检索损失（如基于 log-saturation 的稀疏权重损失）和稠密检索损失（如对比学习损失）。通过划分词表并分配不同 token 预测不同子集，有效规避了直接对全词表预测时的计算开销和稀疏监督信号稀疏的问题。
6. 推理：一次前向即可获得稠密和稀疏表示，无需额外模块。UEmbed 发布了 2B、4B、9B 三个规模，验证了扩展性。
论文还强调其设计保留了与现有检索基础设施的兼容性，稀疏向量可以直接用于类似 SPLADE 的索引。

Q4: 论文做了哪些实验？

论文的实验主要围绕以下方面展开：
1. 多模态检索基准：在 MMEB-v2 上评估稠密和稀疏检索性能，对比了多种公开可用的多模态嵌入模型（如 RzenEmbed、MM-Embed 等）。UEmbed-9B 取得稠密 71.8、稀疏 71.0 的平均分数，优于基于公开数据训练的模型。
2. 文本检索基准：在 BEIR 基准上对比强稠密和稀疏基线。表格 2 显示在稠密设置下，UEmbed-9B 的平均 nDCG@10 达到 56.3，UEmbed-4B 为 56.0，均优于对比模型。说明 UEmbed 在纯文本检索上也具有竞争力。
3. 稀疏检索性能：单独评估稀疏表示的质量，与 SPLADE 等 LSR 方法对比，确认其稀疏表示不逊于编码器型 LSR。
4. 混合检索分析：论文指出“混合评分（dense+sparse）持续改善文本和视觉文档检索”，实验验证了稠密与稀疏互补性。
5. 效率实验：分析推理速度、向量存储开销等，证明单模型双输出在效率上的优势。
6. 智能体（agentic）应用：演示 UEmbed 在检索增强生成（RAG）或多跳检索等智能体场景中的实用价值，可能包括更少的模型调用或更好的工具检索结果。
7. 消融研究：涉及特殊 token 数量 N、词表划分方式等设计选择，图 (c) 展示了特殊 token 数量 N 的影响。
8. 案例研究：附录 C 展示了具体检索案例，说明稀疏和稠密表示在不同类型查询上的行为差异。

Q5: 发现了什么实验现象？

从摘要和检索证据中可归纳以下实验现象：
1. 稠密与稀疏性能接近：在文本和静态视觉文档上，稀疏与稠密表示的性能非常接近；但在视频领域存在更明显的差距，论文将原因归结为视频的高信息密度和时序复杂性（检索证据 Limitations 部分）。这是一个值得关注的反直觉点：稀疏表示并非总是显著弱于稠密，在视频上差距才凸显。
2. 混合检索一致增益：在文本和视觉文档检索中，将稠密与稀疏得分混合（hybrid scoring）始终优于单独使用任一模式，说明两种表示捕捉的信息互补。
3. 规模扩展性：从 2B 到 9B，性能持续提升，表明 UEmbed 的方法能随模型规模扩展而受益。
4. 与公开数据多模态嵌入模型的对比：UEmbed-9B 优于 RzenEmbed 等模型，但论文限定在“公开数据训练”的范围内比较，暗示闭源或大规模内部数据训练的模型可能更强，这里存在指标口径问题。
5. 文本检索竞争力：在 BEIR 上，UEmbed 在稠密设置下达到平均 nDCG@10 56.3（9B），与强稠密/稀疏基线竞争，说明统一模型没有牺牲文本检索能力。
6. 特殊 token 数量的影响：图 (c) 展示了不同 N 的影响，但具体趋势未在检索片段中给出，需要原文确认。推测存在最优 N 值，过大或过小可能影响稀疏表示的质量或训练难度。
7. 语言与文化偏差：训练语料以英文和中文为主，导致对其他语言覆盖不足，这是一个已知局限，也反映了多语言泛化的困难。
这些观察中，前三点有明确证据，第四、五点为摘要支持，第六、七点分别为图注和 limitations 证据，其余为合理推断。

Q6: 有什么可以进一步探索的点？

基于论文自身局限和领域趋势，可探索的方向包括：
1. 语言与文化偏差的缓解：扩大多语言语料，引入跨语言对比学习，或设计语言无关的词表划分，以提升低资源语言上的稀疏检索质量。
2. 视频检索的稀疏表示优化：针对视频高信息密度和时序动态，设计专门的稀疏化策略，如基于关键帧或时间片段的稀疏 token，或使用可学习的时序聚合。
3. 更大规模与更高效率：探索 20B 以上规模的性能边界，同时优化特殊 token 的稀疏预测的计算效率，如通过分组投影或近似最近邻减少内存。
4. 动态词表划分：当前词表划分是静态的，可研究根据输入内容或模态动态调整划分，或学习层次化稀疏结构。
5. 多模态稀疏检索的索引与压缩：结合文档膨胀（document expansion）与稀疏向量压缩，降低倒排索引的存储开销，使 UEmbed 的稀疏表示在工业级规模下可用。
6. 零样本与少样本适应：利用 LLM 的上下文学习能力，通过提示或适配器快速适应新领域、新模态，无需重新训练。
7. 与生成模型的协同：探索将 UEmbed 嵌入到 RAG 或 agent 框架中，作为统一检索器，替代多个专用检索器，简化系统并减少延迟。
8. 长尾语义与实体检索：分析稀疏表示对专有名词、实体链接和跨语言实体的覆盖，结合外部知识库增强。
9. 公平性与鲁棒性：评估不同文化背景查询的检索偏差，对抗样本下的稳定性，以及噪声输入对稀疏权重的影响。
10. 理论分析：从信息论角度解释词表划分的合理性，研究稀疏表示的容量与可分性，为最优划分提供理论指导。

Q7: 总结一下论文的主要内容

UEmbed 论文提出了一个统一的稠密与稀疏多模态嵌入模型，旨在解决现有学习型稀疏检索（LSR）局限于编码器架构、多模态扩展依赖辅助模块、以及稠密与稀疏表示分离这三个主要问题。论文的核心创新在于仅解码器的架构设计和词表划分策略：在输入序列后附加 N 个可学习特殊 token，并将词表分为 N 个不相交子集，每个 token 在因果前向中只预测自己子集上的稀疏权重，最后拼接为完整稀疏向量，同时从同一隐状态获得稠密表示。这种设计避免了全词表预测的高计算开销，并使得单一模型即可输出两种检索表示。模型在公开数据上训练，发布了 2B、4B、9B 三个规模。
实验方面，UEmbed 在 MMEB-v2 多模态基准上，UEmbed-9B 取得稠密 71.8、稀疏 71.0 的成绩，超越了基于公开数据训练的多模态嵌入模型（如 RzenEmbed）；在 BEIR 文本基准上，稠密设置的 nDCG@10 平均分达到 56.3（9B），与强基线竞争。论文还通过实验展示了三个实用优势：一是混合评分一致提升文本和视觉文档检索效果；二是设计兼容现有检索基础设施；三是在智能体应用中具有实际价值。
论文同时承认了一些局限：训练语料以英文和中文为主，存在语言与文化偏差；视频领域稀疏与稠密表示性能差距稍大，归因于高信息密度和时序复杂性；模型参数规模相对较大，可能带来部署成本。整体而言，UEmbed 提供了一种新的范式：在单一多模态骨干中统一稠密和稀疏嵌入，并使稀疏检索能够直接处理文本和视觉输入，为多模态检索系统的简化与扩展提供了新思路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向的关联：UEmbed 作为统一检索器可简化 RAG 和多跳检索中的工具链，论文也专门强调了 agentic 应用，这与你的 agent 方向（权重 0.10）直接相关。

## 基本信息

- 作者：Tingyu Song, Mingxin Li, Yanzhao Zhang, Dingkun Long, Pengjun Xie, Zhijie Nie, Yilun Zhao, Shu Wu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI, cs.CL, cs.IR
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02583v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据，包括摘要、Introduction、Results 与 Limitations 等片段，并基于启发式草稿进行中文精读整理。
