---
user_id: "cheng tan"
paper_id: 2520
arxiv_id: "2607.01538"
title: "Can Language Models Actually Retrieve In-Context? Drowning in Documents at Million Token Scale"
publish_date: "2026-07-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.01538.pdf"
pdf_url: "https://arxiv.org/pdf/2607.01538"
abs_url: "https://arxiv.org/abs/2607.01538"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-06T12:51:58"
---
# Can Language Models Actually Retrieve In-Context? Drowning in Documents at Million Token Scale

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：in-context retrieval · long-context language models · attention dilution · block-sparse attention

## 一句话总结

本文首次系统研究语言模型在百万token语料库上的上下文检索能力，提出BlockSearch模型，揭示注意力稀释效应，并通过长度感知调整在多个基准上匹配甚至超越密集检索。

## 摘要

> Language models (LMs) raise an intriguing alternative to vector-based retrieval: conditioning on an in-context corpus and directly generating a relevant answer. However, prior work has largely focused on proprietary systems or the smaller-scale reranking task, leaving corpus-scale in-context retrieval largely unexplored. In this work, we present the first systematic study of in-context retrieval on two scales practical retrievers demand: million-token corpora and length-generalization far beyond training-time sizes. We first introduce BlockSearch, a 0.6B LM retriever whose architectural and training modifications improve over prior LM baselines and length-generalize up to 10 times beyond its training regime. Nevertheless, retrieval still collapses under more extreme extrapolation. We trace this failure to an attention dilution effect: as the corpus grows, irrelevant documents dominate the softmax denominator, reducing the normalized mass on the gold document even when its pre-softmax score stays high. Motivated by this analysis, we introduce length-aware adjustments to the attention softmax and document-level sparse attention. With these modifications, at the million-token scale, our model matches dense retrieval on widely studied benchmarks (e.g, MS MARCO and NQ), while outperforming the concurrent model MSA despite being 7 times smaller. Furthermore, it significantly outperforms dense retrieval on tasks requiring entirely different notions of similarity, such as LIMIT, achieving a 3 times higher score. Together, our results position in-context retrieval a promising alternative to classical retrieval while emphasizing attention control under extreme context growth as a new challenge.

Q1: 这篇论文试图解决什么问题？

传统向量检索依赖双编码器和近似最近邻搜索，而语言模型可以直接以条件生成方式进行检索（即上下文检索），但该方式在专有系统或小规模重排序之外尚未被充分探索。核心问题包括：（1）语言模型能否从百万token规模的上下文中可靠地检索出相关文档？（2）长度泛化到远超训练长度的范围时，性能如何衰退？（3）哪些机制导致了大规模上下文中的检索失败？本文旨在填补这一空白，提供系统性理解和方法改进。

Q2: 有哪些相关研究？

相关工作涵盖三个方面：（1）向量检索（Dense Retrieval），如Contriever、DPR等，是当前标准方法，但需要双编码器和索引。（2）上下文检索（In-Context Retrieval, ICR）的先驱工作，如使用块稀疏注意力的LM检索器，但多在千token规模或作为重排序器使用（例如MS MARCO重排序任务）。（3）长上下文语言模型与注意力机制研究，尤其是上下文扩展方法（如ALiBi、RoPE）和稀疏注意力。并发工作MSA（Multi-Scale Attention）使用Mamba架构，但其效果在本文实验中被超越。此外，LIMIT基准要求不同语义相似度概念（如主题匹配），为ICR提供了独特挑战。

Q3: 论文如何解决这个问题？

论文首先提出BlockSearch，一个0.6B参数的自回归LM检索器，其核心设计包括：（1）块稀疏注意力（block-sparse attention），将文档划分为块，仅允许块内和跨块的部分注意力，以扩展到百万token。（2）训练时随机化块大小和序列长度，促进长度泛化。（3）预训练和微调阶段使用ICR目标（给定上下文和查询，预测相关文档）。然后，通过机制分析发现注意力稀释效应：当语料库N增大时，无关文档的softmax分母权重增长，使金标文档的归一化注意力下降。为此提出两组改进：（a）长度感知softmax调整（length-aware softmax scaling），根据序列长度对logit进行缩放。（b）文档级稀疏注意力（document-level sparse attention），仅在少数候选文档上计算注意力，减少无用干扰。最终整合为完整的BlockSearch+模型。

Q4: 论文做了哪些实验？

实验在三个基准上进行：MS MARCO和NQ（来自BEIR）用于标准检索评估，LIMIT用于需要不同语义相似度概念的任务。对比基线包括：密集检索（Contriever-MS MARCO，约0.3B）、并发模型MSA（约4.2B参数）、以及BlockSearch的消融变体。主要评估指标为Recall@1和Recall@5（MAP平均精度）。控制语料库大小N从1K到1M token，并测试长度泛化（训练时最大长度128K，测试时至1M）。消融实验验证了随机化训练、稀疏注意力、长度感知调整的贡献。机制实验分析了预softmax分数与后softmax质量随N的变化，以及对数期望的预测。

Q5: 发现了什么实验现象？

（1）BlockSearch在中等规模（N≤100K）时表现良好，但随N增至1M，Recall急剧下降至接近随机，而密集检索保持稳定。（2）注意力稀释分析：金标文档的预softmax分数随N增加仍保持较高，但其在softmax后的归一化权重被大量无关文档削弱，导致检索崩溃。（3）引入长度感知softmax和文档级稀疏注意力后，在1M token尺度上，BlockSearch+在MS MARCO和NQ上匹配密集检索（Recall@1≈60%），且在LIMIT上Recall@1约0.6，是密集检索（约0.2）的3倍。（4）相比MSA（4.2B），BlockSearch（0.6B）在更大参数效率下达到更好或相当的性能。（5）消融显示，随机化训练对长度泛化至关重要，而不加稀疏注意力时1M性能下降约15%。

Q6: 有什么可以进一步探索的点？

（1）缩小大N（如超过百万）下的剩余性能差距，这是实际部署的关键挑战。（2）更高效的注意力控制机制，例如动态稀疏模式或混合检索（ICR+向量检索）。（3）将ICR扩展到更大模型（>0.6B）以验证scaling规律。（4）在更多领域任务（如开放域QA、事实验证）中评估ICR的实际效果。（5）改进长度泛化的理论理解，特别是注意力稀释与softmax scaling的关系。（6）将块稀疏注意力与更先进的序列模型（如State Space Model）融合。

Q7: 总结一下论文的主要内容

本论文系统研究了语言模型在上下文检索（ICR）任务中扩展到百万token规模的能力与局限性。作者首先指出，ICR通常被限制于小规模或重排序场景，而实际检索系统需要处理大规模语料库并泛化到超长输入。为填补这一空白，论文提出了BlockSearch，一个0.6B参数的自回归LM检索器，使用块稀疏注意力降低计算量，并通过训练时随机化块大小和序列长度来促进长度泛化（训练128K，测试1M）。然而，在极端长度外推下（如语料库包含大量无关文档时），BlockSearch的检索性能急剧下降。通过机制分析，论文揭示了“注意力稀释”效应：当上下文包含许多文档时，所有文档的注意力分数总和恒为1，无关文档大量挤占了softmax分母，导致金标文档的归一化权重降低，即使其原始logit仍较高。基于此诊断，作者提出了两种长度感知的改进：一是对softmax进行缩放（根据序列长度调整logit温度），二是引入文档级稀疏注意力（仅允许少量候选文档参与交互）。最终模型BlockSearch+在MS MARCO和NQ上匹配了强密集检索基线（Contriever），在LIMIT上取得3倍于密集检索的性能，同时比并发模型MSA小7倍。论文贡献包括：首次系统评估ICR的大规模能力；揭示注意力稀释机制；提出有效缓解方案；并在多个基准上验证了ICR作为检索范式的潜力。实验还指出当前长上下文基准（如Needle in a Haystack）过于合成，ICR任务能提供更语义化的挑战。局限在于大N下仍有明显退化，且未探索更大模型。整体上，该工作为长上下文检索和注意力控制提供了重要的实证与理论基础。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对长上下文语言模型的研究有直接启示，尤其是注意力机制在极端长度下的行为。

## 基本信息

- 作者：Siddharth Gollapudi, Nilesh Gupta, Prasann Singhal, Sewon Min
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.01538`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据片段。
