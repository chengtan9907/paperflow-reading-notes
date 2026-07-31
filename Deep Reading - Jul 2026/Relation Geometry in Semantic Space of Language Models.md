---
user_id: "cheng tan"
paper_id: 6190
arxiv_id: "2607.26762v1"
title: "Relation Geometry in Semantic Space of Language Models"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26762v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26762v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:28:57"
---
# Relation Geometry in Semantic Space of Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：semantic relations · language models · semantic space · relation geometry

## 一句话总结

本文系统性地研究了语言模型语义空间中语义关系的几何特性，从关系区域存在性、关系属性（对称性/传递性）的编码以及词汇与上下文信息的重要性三个角度进行了深入分析。

## 摘要

> When it comes to generating vector representations of words, current language models are achieving high-quality results. However, what is not known is the extent to which knowledge about semantic relations is represented in the geometry of the semantic spaces created in this way. In order to answer this question, we study the relation geometry of such semantic spaces from three perspectives. We first examine whether words standing in a particular relation to a target word~(called relata) occupy the same region in semantic space, and whether the regions corresponding to different relations are distinct from each other. We then verify to what extent semantic spaces reflect certain well-known properties of relations, such as symmetry, asymmetry, and transitivity. Finally, we consider which information about the target words and relata is more important for relation geometry: their surface forms, or their contexts. We conduct experiments on six semantic relations using causal, masked, and diffusion language models. The results show that relata in asymmetric relations relatively clearly occupy a distinct region in semantic space. Asymmetric relations' properties are only moderately well encoded in the semantic space, yet better than those of symmetric ones. Furthermore, when considering the question which information source has the strongest impact on results amongst the models we evaluated, we find that lexical information tends to be more important for the causal language model, whereas contextual information is more important for the masked and diffusion language models. Our results empirically show that relation geometry is not equally well-represented for all relations in semantic space, suggesting that there is a difference in how well semantic relations might be learned from distributional information alone.

Q1: 这篇论文试图解决什么问题？

语言模型产生的语义空间是否编码了语义关系的几何结构？具体而言，三个未充分解答的问题：(1) 具有特定语义关系的词（关系项）在语义空间中是否聚集在同一区域，且不同关系的区域是否可区分？(2) 语义空间是否反映了关系的数学性质（如对称性、不对称性、传递性）？(3) 目标词本身的词汇信息与上下文信息在塑造关系几何中哪个更关键？现有研究多集中于上下位关系，对其他关系类型、关系属性编码以及信息源缺乏系统性评估。

Q2: 有哪些相关研究？

分布语义学假设词汇语义可从上下文中习得，但 Harris 并未详细说明分布关系如何映射到语义关系（如上下位、部分整体等）。现有的关系几何研究主要针对超上位关系（如 word2vec 的类比方向），但存在以下局限：(1) 实证范围窄，只探讨了超上位，未涉及反义、同义、部分整体等关系；(2) 缺少对关系属性（如对称性）的定量检验；(3) 未区分不同模型架构（因果/掩码/扩散）对关系几何的影响；(4) 未系统比较词汇与上下文信息的作用。本文旨在填补这些空白。

Q3: 论文如何解决这个问题？

本文设计了一套系统性实验协议：采用六种语义关系（上义、下义、整体、部分、同义、反义），选取三种代表性语言模型（因果模型 LLaMA、掩码模型 ModernBERT、扩散模型 LLaDA），从三个层次分析关系几何。第一层次：通过线性分类器判断关系项区域是否存在且不同关系是否可分，计算区域中心距离或线性分离准确率。第二层次：检验空间是否编码对称性（同义/反义应为对称，上下位/整体部分应不对称）和传递性（如上位关系传递）——例如比较关系项对方向一致性、构建传递性测试三元组。第三层次：通过置换目标词表面形式（保留上下文）与置换上下文（保留表面形式）对比，评估词汇与上下文对关系表征的相对贡献。

Q4: 论文做了哪些实验？

模型：LLaMA（因果）、ModernBERT（掩码）、LLaDA（扩散），并加入简单的词嵌入基线。数据集：基于 WordNet 和 ConceptNet 等资源构建六类语义关系的词对，以及无关系对照对。任务1：关系区域可分性——训练线性 SVM 区分某一关系的关系项 vs 无关词，计算准确率。任务2：关系属性编码——测试对称性（如同义词对在空间中距离是否双向一致）和传递性（如 A is-a B、B is-a C 是否预测 A is-a C）。任务3：信息源重要性比较——设计置换实验，保持上下文或词汇不变，测量关系几何变化。指标包括分类准确率、相似性评分等。消融实验包括：不同长度的句子上下文、不同层表征等。

Q5: 发现了什么实验现象？

1. 关系区域存在性：非对称关系（上下位、整体部分）的关系项区域相对独立且与无关词区域可分，但与同义/反义关系重叠较多；无关三元组与相关三元组线性可分性明显优于关系间区分。2. 关系属性编码：非对称关系的属性（不对称性、传递性）在语义空间中有中等程度体现（传递性短距离显著，长距离差），对称关系（同义、反义）的属性编码更弱。3. 模型对比：整体上 LLaMA 表现优于 ModernBERT 和 LLaDA；但在反义关系上，简单词嵌入基线反而超过所有 LM，提示 LM 的上下文建模可能干扰反义表征。4. 信息源：因果模型（LLaMA）中词汇信息重要性高于上下文；掩码和扩散模型则相反，上下文信息更重要。5. 负结果：关系区域之间存在大量重叠，尤其是同义和反义区域几乎不可分；传递性编码仅限一跳传递，多跳失败。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 将分析扩展到更多语言和领域，验证关系几何的跨语言一致性；(2) 考虑更复杂的几何结构（如双曲空间、非欧几何）来捕获关系层次性；(3) 研究动态上下文（如对话、长文档）对关系几何的影响；(4) 结合显式知识图谱约束关系表征；(5) 探索语义关系几何在下游任务（如推理、信息抽取）中的实际效用；(6) 分析不同训练目标（因果 vs 双向）对关系几何的根本影响原因。

Q7: 总结一下论文的主要内容

本文以语言模型的语义空间为研究对象，系统考察了语义关系在该空间中的几何表征。作者从三个核心问题展开：(1) 关系项（与目标词具有特定关系的词汇）是否在语义空间中占据特定的区域，且不同关系的区域是否彼此分离？(2) 语义空间是否体现了关系的代数性质——对称性、不对称性和传递性？(3) 在关系表征中，目标词的词汇信息与上下文信息哪个贡献更大？为回答这些问题，论文覆盖了六种基础语义关系：上下位（hypernymy/hyponymy）、整体部分（holonymy/meronymy）、同义（synonymy）和反义（antonymy），并选取三类现代语言模型：因果模型 LLaMA、掩码模型 ModernBERT、扩散模型 LLaDA。实验方法论：第一层用线性分类器检验区域可分性，发现非对称关系的关系项区域相对清晰，而同义/反义区域高度重叠；第二层通过构造对称 / 传递性测试三元组检验属性编码，结果表明非对称关系的属性编码中等，长距离传递性普遍缺失；第三层通过词汇/上下文置换实验揭示模型架构对信息源的依赖差异：因果语言模型更依赖词汇信息，而掩码/扩散模型更依赖上下文。此外，简单词嵌入基线在反义任务上反而胜过所有神经网络模型，暗示当代 LM 的上下文机制可能削弱了反义关系的纯净表征。主要贡献：(1) 首次在统一框架下系统评估六种语义关系在因果、掩码、扩散三类 LM 中的关系几何；(2) 揭示了关系特性（对称 vs 非对称）与几何可表征性之间的关联；(3) 发现了模型架构对信息源偏好的系统差异。论文最后指出，语义空间的关系几何并非对所有关系均匀成立，对分布假设学习语义关系的能力提出了质疑。讨论中承认了局限：仅测试三种模型、六种关系，且分析限于欧几里得空间的线性分类，未探索非线性结构。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对评估语言模型语义能力特别有意义，提供了可复用的评估协议（关系区域、属性、信息源）。

## 基本信息

- 作者：Zhihan Cao, Hiroaki Yamada, Simone Teufel, Tatsuya Hiraoka, Kentaro Inui, Hitomi Yanaka, Takenobu Tokunaga
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26762v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，并优先使用了field_evidence_map中对应字段的chunk内容。
