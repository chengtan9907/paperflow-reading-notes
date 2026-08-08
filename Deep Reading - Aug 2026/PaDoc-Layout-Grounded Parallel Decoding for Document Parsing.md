---
user_id: "cheng tan"
paper_id: 6944
arxiv_id: "2608.06146v1"
title: "PaDoc: Layout-Grounded Parallel Decoding for Document Parsing"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06146v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06146v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T13:58:14"
---
# PaDoc: Layout-Grounded Parallel Decoding for Document Parsing

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：document parsing · layout-aware decoding · parallel decoding · multimodal large language model

## 一句话总结

PaDoc 提出一种布局锚定的文档解析并行解码框架，将预测布局视为共享页面表征上的分支结构，在单个 MLLM 中通过 packed variable-length ancestor attention 训练和 masked parallel decoding 推理，在保持全页上下文的同时把解码深度压缩到最长布局-内容路径，并在 OmniDocBench 上取得了领先的解析质量与显著更高的单 GPU 服务吞吐。

## 摘要

> End-to-end document parsers provide a unified interface, but serialize page layouts and regional contents into one autoregressive sequence. This formulation forces independent regions onto a decoding path whose length grows with the total content, whereas crop-based two-stage parsers expose region-level parallelism at the cost of repeated visual prefills and fragmented page context. To retain full-page context while removing dependencies, we propose PaDoc, a layout-grounded parser that treats the predicted layout as a branching structure over a shared page representation. Under a region-sufficiency assumption, we derive a prefix-conditioned factorization in which the layout stream and regional content branches advance concurrently, reducing the decoding depth to the longest layout-content path. We realize this factorization within a single MLLM: packed variable-length ancestor attention preserves the visibility under standard next-token training, while masked parallel decoding creates branches that the evaluated vLLM backend serves as concurrent requests with cache-resident shared-prefix reuse. On OmniDocBench Full, PaDoc attains an Overall layout F1 of 91.1 and, among end-to-end parsers, a top-tier Overall score of 94.24 together with the best Text Edit (0.038) and Formula CDM (95.59). On a 384-page subset and one A800 GPU, it is the fastest end-to-end parser at five concurrency levels, improving valid-page throughput by 67.4-118% and reducing P95 latency by 39.2-54.9% relative to a same-backbone Sequential SFT baseline. Code is available at https://github.com/Longin-Yu/Padoc

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决端到端文档解析器在自回归解码上的根本效率问题。当前主流端到端解析器（如各类 document VLM）将页面布局和区域内容统一序列化为类似 (b1,y1,b2,y2,...,bN,yN) 的扁平序列，因果解码器必须严格等 yk 生成完后才能开始 bk+1。然而文档中的不同区域通常在空间上分离、语义上独立，这种序列化强加了不存在的依赖关系，导致解码的关键路径长度等于所有区域内容长度之和。随着单页内容增多，尤其是商业文档、论文、表格和公式密集页面，解码成本急剧上升。与此同时，基于裁剪的两阶段系统（layout-first）虽然可以并行处理每个区域，但每个裁剪块需要重复的视觉编码，而且裁剪后的区域失去了完整页面上下文，影响复杂版面（如跨栏、表格、嵌套区域）的识别。核心矛盾在于：既要保持全页上下文，又要消除区域间不必要的序列依赖。PaDoc 的起点是观察到文档布局天然提供了条件独立结构：只要给定共享页面图像和区域位置/类别，不同区域的内容生成是可以并行的。因此问题转化为：(1) 如何设计一种分解方式，在不破坏标准自回归训练的前提下让区域分支并行； (2) 如何在真实推理引擎（如 vLLM）中把这种并行变成可服务的并发请求； (3) 如何在解码质量和显式布局定位上都不劣于甚至超过现有基线。

Q2: 有哪些相关研究？

相关工作可以划分成几个脉络。第一代是早期的图像到序列模型，如 Kim et al. 2022、Lee et al. 2023、Blecher et al. 2024，它们建立了从页面图像直接输出结构化序列的范式。随后 document VLM 将其扩展到全页结构化解析，代表性工作包括 Nassar et al. 2025、Poznanski et al. 2025、Niu et al. 2026、Dong et al. 2026，这些方法往往输出 Markdown、HTML、JSON 或布局-内容记录。另一条线是 layout-first 两阶段系统，它们先检测版面区域，再对每个区域裁剪并独立识别，例如 Cui et al. 2026、Niu et al. 2026 以及 Feng 等人的工作，这类系统能暴露区域级并行，但裁剪导致重复视觉编码和上下文碎片化。PaDoc 试图结合两者优点：保留共享页面表示和完整上下文，同时通过预测布局得到并行分支。它还与并行解码技术（如 masked decoding、skeleton decoding 等）相关，但更强调由版面结构驱动的并行，而不是通用的投机解码。此外，它与多模态大模型中的 KV cache 复用、vLLM 的 continuous batching 和 prefix caching 有工程层面的联系。

Q3: 论文如何解决这个问题？

PaDoc 的方法核心是把布局视为共享页面表示上的分支结构，并通过一个区域充分性假设进行概率分解。假设区域内容 yk 在给定共享页面图像 x、布局前缀和其自身布局信息（区域框、类别等）的条件下，与其他兄弟区域的内容相互独立，则整体序列概率可以分解为布局流和区域内容分支的前缀条件因子。这样，每个区域内容分支只需要关注图像和合适的布局祖先，而不需要依赖兄弟区域的完整内容。训练时，PaDoc 采用 packed variable-length ancestor attention：将多个区域分支打包到一个训练序列中，通过逻辑祖先注意力掩码，让每个分支只能看到图像和对应布局祖先，同时屏蔽兄弟区域内容；这种打包方式避免了密集注意力矩阵，使得可以用标准 next-token 预测目标进行训练，而不改变 MLLM 的架构或损失函数。推理时，PaDoc 使用 masked parallel decoding：把区域分支作为独立的解码分支，在 vLLM 后端作为并发请求提交；由于所有分支共享同页图像和布局前缀，缓存中的共享前缀可以复用，从而避免了重复的视觉预填充。这样，解码深度从所有区域内容总长度减少到最长布局-内容路径长度，即页面中单个最长区域内容的长度。PaDoc 不修改 MLLM 架构，只是在训练和推理阶段重新组织注意力与解码调度。

Q4: 论文做了哪些实验？

实验沿两个互补维度展开。第一是布局定位与端到端解析质量，在 OmniDocBench Full 上评测 Overall layout F1、端到端 Overall 分数、Text Edit 距离以及 Formula CDM（图表/公式匹配分数）。第二是服务效率，在一个 384 页的子集上，使用单张 A800 GPU，在五种并发级别下测量有效页吞吐（valid-page throughput）和 P95 延迟。对比基线包括同骨干的 Sequential SFT 基线（即把区域按扁平序列串行解码的微调模型），以及其他端到端解析器（包括另一个 1.0B 的 HunyuanOCR-1.5 模型）。论文报告了 PaDoc 在目标指标上的绝对值以及相对 Sequential SFT 基线的吞吐提升和延迟降低百分比。

Q5: 发现了什么实验现象？

实验观察到的关键现象如下。第一，PaDoc 在 OmniDocBench Full 上的 Overall layout F1 达到 91.1，表明引入并行分解和分支训练没有损害布局定位能力。第二，在端到端解析质量上，PaDoc 获得 Overall 94.24，属于端到端解析器中的顶级水平，同时 Text Edit 达到 0.038（越低越好）和 Formula CDM 95.59，均是最优结果，这说明并行分解不仅没有降低文本和公式识别精度，反而在质量上具有竞争力。第三，在服务效率上，PaDoc 在所有五个并发级别下都是最快的端到端解析器，相比 Sequential SFT 基线，有效页吞吐提升 67.4%–118%，P95 延迟降低 39.2%–54.9%。第四，值得注意的是，PaDoc 虽然只有 2.1B 参数，却超越了 1.0B 的 HunyuanOCR-1.5，这个对照表明布局锚定的并行化显著降低了服务成本，而不必牺牲或甚至能提升质量。这些现象支持了论文的核心主张：区域级并行可以主要从页面布局结构中获取，而不需要重复的视觉编码或引入与版面无对应的分支。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：1) 更复杂版面的扩展，例如跨页区域、表格跨栏、嵌套区域或注释/脚注，这些场景可能削弱区域充分性假设，需要检验分解的有效性并在必要时引入跨区域注意力。2) 训练与推理的联合优化，例如在训练中模拟推理时的 masked parallel decoding 与并发调度，缩小训练-推理不一致；也可以探索预测布局不确定性对并行分支质量的影响。3) 探索非自回归或半自回归的增强，例如在布局分支内部也引入并行 token 生成，进一步压缩单分支延迟。4) 将 PaDoc 的分解原理推广到其他结构化生成任务，如文档级信息抽取、长文档理解、网页结构重建、甚至代码或程序生成中的独立函数块并行。5) 系统级优化：当前实验只在单张 A800 上做并发评测，可扩展到多 GPU、分布式前缀缓存、更细粒度的请求调度策略，以及不同批次大小的行为。6) 改进区域充分性假设，比如让分支能通过可学习的门控机制动态决定是否需要看到兄弟区域的全局摘要或局部特征。7) 在更大规模模型和更复杂文档语料上验证，并引入更多的质量指标（如表格结构识别准确率、阅读顺序正确率）。8) 探索如何在训练中引入布局预测误差的鲁棒性，因为在预测布局不准确时并行分支会生成错误内容。

Q7: 总结一下论文的主要内容

PaDoc 论文针对端到端文档解析中的序列化解码效率问题，提出一种布局锚定的并行解码方法。文档解析任务需要从页面图像中提取布局区域、阅读顺序、文本、公式、表格和图形等结构化表示。现有端到端解析器将布局和内容序列化成扁平 token 序列，强制独立区域沿一条随总内容长度增长的解码路径顺序执行；两阶段裁剪方法虽然可以并行，却因重复视觉编码和上下文碎片化损失效率与全局信息。PaDoc 的核心洞察是，文档布局本身就提供了并行结构：在给定页面图像和区域布局条件下，不同区域的内容生成是近似独立的。为此，论文提出区域充分性假设，并推导出前缀条件分解，将联合概率分解为布局流和并行区域内容分支。训练阶段，PaDoc 在单个多模态大语言模型中引入打包变长祖先注意力，通过逻辑祖先掩码让每个区域分支只可见图像与对应布局祖先，而屏蔽兄弟区域内容，从而在标准 next-token 训练目标下实现并行分支训练，并且避免了密集注意力矩阵。推理阶段，PaDoc 使用掩码并行解码，将区域分支映射为 vLLM 后端上的并发请求，并利用缓存中的共享前缀复用，避免重复视觉预填充，将解码深度压缩到最长布局-内容路径。实验在 OmniDocBench Full 上进行质量评估，PaDoc 获得总体布局 F1 91.1，端到端总体 94.24，文本编辑距离 0.038，公式 CDM 95.59，均达到顶级水平；在 384 页子集与单张 A800 GPU 上，PaDoc 在五种并发级别下都是最快的端到端解析器，相比同骨干 Sequential SFT 基线吞吐提升 67.4%–118%，P95 延迟降低 39.2%–54.9%。此外，尽管只有 2.1B 参数，PaDoc 超过了 1.0B 的 HunyuanOCR-1.5，验证了布局锚定并行能显著降低服务成本。论文贡献可概括为：提出了基于布局分支的并行解码公式化分解；设计了一种无架构修改的单 MLLM 训练与推理方案；并通过全面的实验证实其在质量与速度上的优势。PaDoc 的主要局限可能在区域充分性假设对复杂跨区域版面的限制，以及当前评测集中度（单 GPU、单数据集子集）对泛化性的影响。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向（权重 0.10）直接相关：PaDoc 是自回归生成中通过结构化解码实现并行的典型案例，对生成效率优化有参考价值。

## 基本信息

- 作者：Hao Yu, Jiabo Zhan, Kang Liu, Linnan Zhao, Dongxu Yue, Rui Chen, Jinglin Wang, Chong Sun, Chen Li, Jing Lyu, Chun Yuan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06146v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（abstract、introduction、experiments、conclusion 片段），并结合论文元数据与摘要信息进行了补全。
