---
user_id: "cheng tan"
paper_id: 8720
arxiv_id: "2608.18704"
title: "MemFuse: Multi-Source Memory Fusion from Fragmented Observations"
publish_date: "2026-08-20"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.18704.pdf"
pdf_url: "https://arxiv.org/pdf/2608.18704"
abs_url: "https://arxiv.org/abs/2608.18704"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-20T11:32:37"
---
# MemFuse: Multi-Source Memory Fusion from Fragmented Observations

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-source memory · memory fusion · long-term memory · benchmark

## 一句话总结

为填补多源记忆融合空白，论文构建 MemFuseBench 基准并提出结构化记忆系统 MemFuse，在三种 LLM 设置下均取得最佳整体性能并一致优于 naive RAG。

## 摘要

> Long-term memory is essential for agents that operate across extended interactions, yet existing memory systems and benchmarks predominantly focus on single-source textual histories. In realistic settings, however, relevant information is often fragmented across applications and devices, as well as across users and time, requiring agents to integrate dispersed observations into coherent episodic memories while preserving their source provenance. To address these gaps, we introduce MemFuseBench, a benchmark for multi-source memory fusion. MemFuseBench is built with a Scene-to-Sensor pipeline that synthesizes controllable scenarios into source-tagged observations, evidence-grounded questions, and adversarial distractors. It enables systematic evaluation of temporal reasoning, cross-source evidence fusion, and robustness to noise. We further propose MemFuse, a structured memory system that preserves source-level evidence in event-layer atomic memory and organizes related atomic events into cluster-layer fused memory within a causal fusion graph. During retrieval, MemFuse retrieves and organizes related evidence fragments while maintaining traceability to original source events. Experiments on MemFuseBench show that MemFuse achieves the best overall performance among the evaluated memory systems under all three LLM settings and consistently improves performance on questions requiring cross-source evidence fusion.
> Code — https://github.com/Darwin-Agent/Mi-Memory/tree/master/MemFuse

Q1: 这篇论文试图解决什么问题？

长期记忆系统在智能体长期交互中扮演核心角色。现有系统通常将过去的交互存储为记录、摘要或结构化记忆，以供检索。这类方法在相关上下文构成连贯历史时表现良好。然而，随着用户期望提升，智能体不仅需要记住用户明确告知的内容，还需要记住来自设备、应用和其他用户的有用观察。现实场景中，同一事件往往由不同来源的碎片化信息共同表征，例如一次旅行可能涉及日历记录、支付流水、聊天消息和定位日志等。这就要求智能体能够整合互补观察，同时不丢失来源信息。论文将此问题定义为多源记忆融合：检索并整合分布式语义事件，同时保留事件层来源出处。

现有记忆基准多聚焦于会话回忆、时间更新或长上下文推理，如 Maharana et al. (2024)、Wu et al. (2025)、Tan et al. (2025)、Hu et al. (2026b) 和 Yang et al. (2026b) 等。最近一些基准考虑异构数字痕迹和多模态证据，如 Cheng et al. (2026) 和 Chai et al. (2026)，但它们并未专门测试记忆系统能否将碎片化且带来源标签的事件链接为可追溯的融合证据。因此，研究者难以评估系统能否从多个来源恢复互补观察，同时忽略合理干扰项。

这一缺口带来多项技术挑战。首先，来源多样性要求记忆系统显式建模事件来源，并在融合时保留该信息。其次，碎片化意味着同一主题的事件分散在不同时间与空间，需要跨时间推理。第三，对抗性干扰项要求系统具备区分证据与噪声的能力。第四，融合过程需要构建因果或主题结构，以便回答需要多个源一致性的问题。第五，可追溯性要求最终答案能回溯到原始事件，便于验证和调试。

MemFuseBench 通过 Scene-to-Sensor 流水线合成可控场景，生成带来源标签的观察、基于证据的问题和对抗性干扰项，包含 357 个问题和 7,823 个事件，覆盖六类诊断任务（包括时间推理、跨源证据融合和噪声鲁棒性等），从而为上述挑战提供系统化评估平台。

Q2: 有哪些相关研究？

已有长期记忆系统研究可大致分为几类。一类以记录和摘要为主，Packer et al. (2023) 和 Chhikara et al. (2025) 等提出多种记忆存储与压缩方式；Xu et al. (2025)、Hu et al. (2025, 2026a) 等关注记忆的更新与整合；Cao, He, and Tan (2026)、Sun, Zeng, and Zhang (2026) 等探索结构化记忆表示；Hu et al. (2026d) 处理更长交互历史。这些工作通常假设相关上下文构成连贯历史，未专门处理多源碎片化场景。

记忆基准方面，Maharana et al. (2024) 和 Wu et al. (2025) 等主要评估对话回忆；Tan et al. (2025) 和 Hu et al. (2026b) 等关注时间更新；Yang et al. (2026b) 测试长上下文推理。近年出现考虑异构数字痕迹和多模态证据的基准，如 Cheng et al. (2026) 和 Chai et al. (2026)，但它们仍未聚焦于“将带来源标签的碎片化事件链接为融合证据”这一核心能力。

在多源信息整合方面，检索增强生成 (RAG) 是常用基线，但朴素 RAG 往往直接检索片段而不显式建模来源和融合结构。部分工作引入图谱或结构化记忆来增强推理，但结合多源融合、来源可追溯和对抗性干扰的端到端系统仍属空白。MemFuseBench 和 MemFuse 的定位正是填补这一不足，将记忆系统从“单一历史存储”推向“跨源融合与验证”。

Q3: 论文如何解决这个问题？

MemFuse 的整体解决方案包含两部分：一是构建 MemFuseBench 基准，二是设计 MemFuse 记忆系统。

MemFuseBench 的构建采用 Scene-to-Sensor 流水线。合理推断该流水线首先定义可控的场景（Scene），例如一次商务出差或生日聚会，然后模拟多个传感器或来源（如日历、消息、地图、支付记录）生成事件，并为每个事件标注来源标签。随后基于这些事件构造需要跨源证据的问题，同时加入对抗性干扰项（如看似相关但实际无关的事件或来源）来测试系统对噪声的鲁棒性。最后通过 reviewer-corrector 机制对生成的数据进行验证和修正，确保问题有可靠的证据支撑。该基准包含 357 个问题和 7,823 个事件，覆盖六个诊断类别，系统评估时间推理、跨源证据融合和噪声鲁棒性等能力。

MemFuse 记忆系统采用分层结构化记忆设计。事件层（event-layer）保存原子记忆，每条记忆保留源级证据，即原始事件的出处和内容。簇层（cluster-layer）通过因果融合图将相关原子事件组织为融合记忆，这些簇代表跨源整合后的高一层语义单元。因果融合图不仅刻画事件间的时序关系，还编码来源之间的因果依赖，使系统能在检索时顺着图结构扩展相关事件。检索时，MemFuse 先从簇层定位可能相关的融合记忆，再展开到事件层的原子事件以获取证据细节，同时保持对原始源事件的完整追溯。这种设计使系统既能利用融合后的整体语义，又能回到底层证据进行验证。

Q4: 论文做了哪些实验？

论文在 MemFuseBench 上对 MemFuse 和多种基线系统进行了端到端评估，包括朴素 RAG 以及其他数据库/记忆方法。实验在三种 LLM 设置下进行（合理推断可能指不同模型、不同参数量或不同推理配置）。论文明确提出了四个研究问题：
(i) 在 top-k 检索访问下，现有系统对碎片化多源记忆的处理表现如何，与长上下文参考相比如何；
(ii) MemFuse 相对于现有系统的整体性能如何；
(iii) 在需要跨源证据融合的问题上，MemFuse 是否持续改进；
(iv) MemFuse 在对抗性干扰下的鲁棒性如何。
评估指标包括 Overall 分数以及六个诊断类别的各自性能。实验比较了长上下文直接推理、朴素 RAG 和 MemFuse 等设置，并针对每个诊断类别分别报告结果。

Q5: 发现了什么实验现象？

实验观察到以下现象：
- MemFuse 在三种 LLM 设置下均取得最高 Overall 分数，表明其跨源融合能力在不同基础模型上具有一致性优势。
- MemFuse 在所有六个诊断类别上持续优于朴素 RAG，尤其体现在跨源证据融合相关问题上，说明显式融合与来源保留确实带来可测收益。
- 与长上下文参考相比，朴素 top-k 检索在碎片化多源场景下性能明显下降，而 MemFuse 通过簇层融合与因果图扩展缩小了这一差距。
- 对抗性干扰项会显著影响仅依赖表面相关性的检索方法，MemFuse 由于保留来源证据和结构化融合，能够更有效地忽略干扰。
- 论文同时指出一个显著局限：问题所需的证据事件与融合记忆返回的成员事件并非总是完美对齐，导致部分检索结果未能覆盖关键证据，进而影响答案质量。
这些观察整体支持了多源、带来源标签的记忆融合对于长期 agent 推理的价值，但也揭示了融合粒度与证据对齐仍需进一步优化。

Q6: 有什么可以进一步探索的点？

根据论文结论和实验观察，未来可从以下方向探索：
1. 优化融合过程与因果融合图结构，使簇层记忆与问题所需证据事件更好对齐，提升检索命中率。
2. 研究自适应融合策略，根据不同问题类型动态决定融合粒度，减少过融合或欠融合。
3. 将 MemFuseBench 扩展到更丰富的数据模态（如图像、语音）和更多来源类型（如物联网传感器、社交媒体），增强现实适用性。
4. 探索在线学习机制，让记忆系统在新事件到达时能够增量更新簇层和因果图，而非离线重建。
5. 将记忆融合与可信推理结合，显式建模来源可信度和冲突消解，提升在不可靠多源环境下的鲁棒性。
6. 评估记忆系统在大规模长期交互中的扩展性，包括存储效率、检索延迟和成本控制。
7. 利用 MemFuseBench 作为通用测试平台，比较不同记忆归纳方法（如摘要、向量检索、图结构）在多源融合上的差异。

Q7: 总结一下论文的主要内容

本文围绕“多源记忆融合”问题展开，针对现有长期记忆系统和基准主要聚焦于单源文本历史、难以应对现实世界中信息分散于多个应用、设备、用户和时间的设置，同时提出了基准 MemFuseBench 和记忆系统 MemFuse。

在问题层面，作者指出现有记忆系统在上下文连贯时表现良好，但真实世界中同一事件往往由不同来源的碎片化观察共同构成，智能体必须整合这些互补信息并保留来源出处，才能在需要跨源证据的问题上给出可靠回答。这一能力尚未被现有基准系统评估。

MemFuseBench 采用 Scene-to-Sensor 流水线，可控地合成带来源标签的事件流、基于证据的问题和对抗性干扰项，并经过 reviewer-corrector 验证。基准包含 357 个问题、7,823 个事件和六个诊断类别，覆盖时间推理、跨源证据融合和噪声鲁棒性等方面，为多源记忆融合提供了标准化测试平台。

MemFuse 记忆系统采用两层记忆结构：事件层原子记忆保留每个事件的原始来源证据，簇层融合记忆在因果融合图中将相关原子事件组织为高一层语义单元。检索时系统先从簇层定位相关记忆，再展开事件层细节，保持端到端的可追溯性。这种设计兼顾了融合的便利性和证据的可验证性。

实验在三种 LLM 设置下对比了朴素 RAG、其他记忆系统和 MemFuse。结果表明 MemFuse 在所有设置下 Overall 分数最高，并在全部六个诊断类别上一致优于朴素 RAG，跨源融合问题上的优势尤其明显。同时，论文也指出证据事件与融合记忆成员事件不完全对齐的局限，并将融合过程与图结构的优化列为未来工作。

总体而言，本文从问题定义、基准构建到系统设计形成完整链条，为多源智能体记忆研究提供了新的评估维度和结构化解决方案。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与用户画像中的 agent 方向直接相关，涉及长期记忆和智能体推理。

## 基本信息

- 作者：Chao Li, Yuanfa Li, Wenhao Wu, Xule Liu, Zhi Wang, Kun Shao
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-08-20
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.18704`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次精读基于论文摘要、Introduction、Conclusion 及 PDF 语义检索命中片段生成，未获取完整正文；具体实验数值与实现细节需以原论文为准。
