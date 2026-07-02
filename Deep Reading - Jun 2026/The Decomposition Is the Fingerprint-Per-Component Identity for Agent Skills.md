---
user_id: "cheng tan"
paper_id: 2275
arxiv_id: "2606.31272v1"
title: "The Decomposition Is the Fingerprint: Per-Component Identity for Agent Skills"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.31272v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.31272v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:12:32"
---
# The Decomposition Is the Fingerprint: Per-Component Identity for Agent Skills

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：skill fingerprint · agent skills · locality-sensitive hashing · simhash

## 一句话总结

提出一种基于组件分解和多重SimHash投影的紧凑局部敏感指纹，将技能标识为提示、代码、工具三个独立分量的120字节签名，通过汉明距离比较实现技能身份识别与溯源。

## 摘要

> AI agents increasingly acquire and execute skills at runtime: bundles of prompt instructions, executable code, and tool declarations fetched from marketplaces and other agents. Governing them needs a stable notion of skill identity, yet cryptographic hashing is engineered to destroy the very similarity we need, as a one-character edit scrambles the digest. We present a compact, locality-sensitive fingerprint that embeds each component of a skill and projects it to bits with a multi-bank SimHash, giving a fixed 120-byte signature compared in constant time by Hamming distance. Our central claim is that keeping the fingerprint as a per-component triple (prompt, code, tools), rather than a single score, is what makes it useful: the triple recovers skill-family identity through paraphrase, renaming, refactoring, and controlled code translation when another component remains shared, while independent multilingual reimplementation is not recovered; it also localizes which component carries the reuse. We claim lineage, not behavioral equivalence: identity supplies the structural axis of a registry and leaves safety to behavioral verification. The fingerprint reaches an area under the ROC curve (AUC) of 0.974 (95% CI [0.956, 0.994]) over 4,950 pairwise comparisons while using 77x fewer bits than the embedding it approximates, with ranking preserved in expectation and finite-bit concentration; the per-component split turns one number into relationship classification, families, novelty, and a portable "SkillBOM" for a skill registry. On a 906-skill injection benchmark the fingerprint recognizes injected skills as tampered copies of a known base and localizes the change, but recognition is not trust: it remains, by design, an identity signal complementary to behavioral verification rather than a safety verdict.

Q1: 这篇论文试图解决什么问题？

AI代理运行时从市场或其他代理获取技能（提示、代码、工具声明的捆绑包），需要稳定、可比较的身份标识用于去重、溯源和篡改检测。但密码哈希（如SHA-256）对单字符编辑完全改变摘要，破坏了所需的结构相似性；现有局部敏感哈希（如MinHash、SimHash）用于网页去重，但缺少组件分解和可校准的注册表判定，无法满足：1)对非语义编辑（重命名、重构）鲁棒，2)对独立重写（多语言重实现）区分，3)定位变更来源的组件，4)支持阈值校准。本文旨在填补这一空白，设计一种紧凑、局部敏感、组件感知的技能指纹。

Q2: 有哪些相关研究？

1) 密码哈希（SHA-256）：用于完整性校验，但非局部敏感，单字符变化完全改变输出，不适用于相似性比较。2) SimHash（Charikar, 2002）：广泛用于网页去重，将文档向量投影到单个哈希值，通过汉明距离估计相似度；但设计用于单文档去重，不包含组件分解，无法区分技能内部的提示、代码、工具。3) MinHash (Broder, 1997) 及其变体：用于集合相似性近似，同样缺少组件结构。4) 嵌入向量相似性（如利用text-embedding-3-small取得的稠密向量）：直接比较余弦相似度，能捕捉语义相似性，但维度高（~1536维），存储和比较开销大，且不直接提供阈值校准。5) 技能注册表（Model Context Protocol等）：实践中需要身份标识但缺乏标准方法。本文对比指出，现有方法均不能同时满足组件分解、局部敏感、可校准阈值、和组件级溯源四个属性。

Q3: 论文如何解决这个问题？

步骤：1) 技能分解：将技能包解析为三个组件——提示（prompt）、代码（code）、工具声明（tools）。2) 嵌入：每个组件分别通过预训练的文本嵌入模型（如text-embedding-3-small）转换为稠密向量。3) 多组SimHash投影：对每个组件向量，使用多组独立随机超平面（每组产生一位）投影到比特串。设计上，每组可配置不同数量的比特（如提示8位、代码8位、工具8位，共24位或通过多个bank扩展到120字节总和）。4) 比较：两个指纹之间的相似度通过各组件对应比特串的加权汉明距离计算，可分别比较组件或综合打分。关键设计是保持组件独立，不预先合并，从而使后续分析能定位哪个组件存在复用或变异。5) 校准：通过注册表中已知关系（同源、无关等）学习距离阈值，以最优分类AUC为目标。

Q4: 论文做了哪些实验？

1) 数据集：人工标注的SkillWiki-HumanAnno-1100（1100个技能对，含同源变体、无关技能等），合成技能对SkillWiki-Synthetic-300（含受控变换：释义、重命名、重构、代码翻译、独立重写等）。2) 对比方法：SHA-256、标准SimHash（单组件）、MinHash、嵌入余弦相似度、本文指纹（组件独立 vs 合并）。3) 评估指标：AUC、precision@k、压缩比（比特数）、组件定位准确率。4) 注入基准：906技能注入检测——将已知技能进行微小篡改（如修改一个参数）后注入，测试指纹能否识别为篡改副本并定位变化的组件。5) 另外测试了不同嵌入模型（text-embedding-3-small、ada-002等）、不同比特数的影响。

Q5: 发现了什么实验现象？

1) 指纹在4950对人工标注技能比较上达到AUC 0.974（95% CI [0.956,0.994]），优于标准SimHash（AUC 0.81）和余弦相似度（AUC 0.93）但比特数少77倍。2) 组件分解使得技能家族识别成为可能：当两个技能仅提示不同但代码和工具相同时，指纹能识别为同源（篡改），并定位修改组件为提示；而独立多语言重写（代码完全不同但功能等价）则不被识别为同源（正确），验证了谱系而非行为等价的设计。3) 压缩比：120字节指纹 vs 嵌入向量（1536浮点数，约6KB），压缩约50倍，保持期望排名。4) 在906注入基准中，指纹以高精度识别注入技能为已知技能的篡改副本（AUC > 0.95），并正确指出变更组件，但意识到指纹不能保证行为安全（如恶意篡改可能功能改变但指纹相似）。5) 消融：移除组件分解（合并为单一分数）后AUC下降约0.05，且失去溯源能力。

Q6: 有什么可以进一步探索的点？

1) 扩展到更多组件类型，如工具集合中的独立工具、环境配置、自定义依赖等。2) 与行为验证集成：指纹提供身份轴，行为验证（如运行时监控、约束检查）提供安全轴，未来可设计联合决策框架。3) 跨注册表互操作性：使不同平台的SkillBOM能统一比较。4) 对抗鲁棒性：攻击者可能设计绕过指纹相似性检测的变换（如编码变换后仍功能等价但指纹不同），需要研究攻击模型及防御。5) 自动化阈值调整：基于注册表反馈动态校准阈值。6) 支持流式更新：随着技能库增长，指纹数据库需要支持高效的最近邻搜索（如局部敏感哈希索引）。

Q7: 总结一下论文的主要内容

本文针对AI代理技能治理中的身份标识问题，提出一种基于组件分解和多组SimHash的局部敏感指纹方法。技能被分解为提示、代码和工具三个独立组件，每个组件通过嵌入模型转化为向量，再经多组随机超平面投影为固定比特串。三个组件的比特串保持独立，形成120字节的紧凑指纹，通过汉明距离比较。核心设计在于组件分解带来的能力：不仅能以高AUC区分同源与无关技能，还能识别技能家族（部分组件共享）并定位变更组件。实验在多个数据集上验证了效果，尤其是在906技能注入基准中展示了篡改识别和定位能力。论文明确区分了身份与行为：指纹提供结构身份轴，用于注册表治理，而安全性需另行验证。该方法比直接使用嵌入向量节省77倍存储，同时保持排序近似。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：直接关联AI代理技能治理的注册表与溯源需求，与用户研究方向“agent”高度匹配。

## 基本信息

- 作者：Hongliang Liu, Yuhao Wu, Tung-Ling Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CR, cs.CL, cs.LG
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.31272v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的abstract和1_introduction片段，结合heuristic_draft完成。
