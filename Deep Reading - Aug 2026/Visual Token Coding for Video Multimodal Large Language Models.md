---
user_id: "cheng tan"
paper_id: 9655
arxiv_id: "2608.28008"
title: "Visual Token Coding for Video Multimodal Large Language Models"
publish_date: "2026-08-31"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.28008.pdf"
pdf_url: "https://arxiv.org/pdf/2608.28008"
abs_url: "https://arxiv.org/abs/2608.28008"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-09-01T01:15:32"
---
# Visual Token Coding for Video Multimodal Large Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual token compression · video multimodal large language model · video coding · token pruning

## 一句话总结

本文提出一种受经典视频编码（如 HEVC）启发的视频多模态大模型视觉 token 压缩范式 Visual Token Coding (VTC)，通过预测 I/P 语义帧并利用帧间残差估计 token 冗余，并在其基础上引入动态分辨率、动态 token 分配与空间覆盖 Top-K 的 VTC_{Dy}，在无需微调的情况下以 50% token 预算保持 Qwen3-VL 平均性能 100.1%，以 25% token 预算保持 97.8%。

## 摘要

> In this paper, we propose a new token compression paradigm for video Multimodal Large Language Models (MLLMs), termed Visual Token Coding (VTC). Inspired by classical video coding principles, e.g., HEVC, VTC performs structured compression by predicting the I/P frames of a video and measuring their frame-wise residuals to estimate token redundancy. Based on this baseline framework, we also enhance VTC with a set of novel dynamic designs, such as Dynamic Resolution Input (DyRSO), Dynamic Token Allocation (DyTA), and Spatial Coverage Top-K (SC-TopK), and term this new approach $VTC_{Dy}$ . To validate VTC, we apply it to three MLLMs and conduct experiments on multiple video understanding benchmarks. The experimental results show that $VTC_{Dy}$ achieves an average performance retention of 100.1% with a 50% token budget for Qwen3-VL, while still retaining 97.8% of the average performance when the token budget is reduced to 25%. Moreover, as a plug-and-play design, VTC requires no additional tuning of MLLMs for token coding. Our code is available at https://github.com/Msr233/VTC.

Q1: 这篇论文试图解决什么问题？

视频多模态大模型（MLLMs）在视频语言理解方面取得显著进展，但输入视频对应的视觉 token 数量过大，严重制约实际部署。例如 Qwen3-VL 以 1 FPS 表示一小时视频就需要多达约 46 万视觉 token，带来高昂的计算和显存开销。现有 token 压缩方法大多通过 token-wise 重要性度量来压缩，例如视觉显著性、token 多样性、查询相关性，以及近年引入的帧间时序关系建模，但这些方法从根本上有两个缺陷：一是 token 级重要性打分缺乏对视频整体结构的利用，冗余信息分布在跨帧结构中，仅靠局部重要性难以准确估计；二是现有方法往往需要额外训练或与模型耦合，缺乏即插即用性。VTC 试图把视频编码中“参考帧+残差”的结构化预测思想引入 MLLM 的 token 压缩，把压缩问题重新定义为视频层级的冗余暴露问题，而不是逐 token 的重要性筛选问题。文中还指出，现有借用视频编码思想的工作（如 LLaVA-OneVision-2 中组装 P 帧的尝试）并未为 MLLM 提供合适的语义信息，说明这一方向的直接迁移存在语义适配缺口。

Q2: 有哪些相关研究？

相关工作可分为三条线。第一条是 MLLM 的 token 压缩，包括 token pruning（如 Chen et al. 2024；Huang, Zhou, and Han 2025）、token merging（如 Bolya et al. 2023；Yang et al. 2025c；Shao et al. 2025）及二者组合（Fu et al. 2025b）。这些方法通常用视觉显著性、token 多样性或 query 相关性等度量衡量 token 重要性，然后删除或合并低重要性 token。第二条是视频任务中的时序冗余建模，如 Shao et al. 2025 与 Ju et al. 2026 利用帧间时序关系估计 token 冗余，但它们仍停留在帧或 token 层面。第三条是视频编码思想在 MLLM 中的初步借用，例如 LLaVA-OneVision-2 (An et al. 2026) 借鉴视频编码来组装 P 帧，但据作者分析，它提供的压缩信息缺少适合 MLLM 直接使用的语义信息。此外论文引用了大量近期视频 MLLM 工作（Maaz et al. 2024；Jin et al. 2024；Lin et al. 2024；Cheng et al. 2024；Li et al. 2024；Zhang et al. 2025b；Bai et al. 2025a,b）作为背景。VTC 的定位是与这些工作互补：以视频编码的结构化预测为框架，在帧级别暴露冗余，而不是继续改进 token 级别的重要性度量。

Q3: 论文如何解决这个问题？

VTC 的核心思想是把视频编码中的 I 帧/P 帧结构迁移到 MLLM 的视觉 token 组织上：将视觉 token 组织成语义 I/P 帧，并通过时序残差分配 token 预算。具体地，系统先预测视频的 I/P 帧结构，再计算帧间残差，用残差大小度量帧间冗余：残差大的区域/帧保留更多 token，残差小的区域/帧分配更少 token。在此基础上，VTC_{Dy} 引入三项动态设计——Dynamic Resolution Input (DyRSO)：根据内容动态调整输入分辨率；Dynamic Token Allocation (DyTA)：根据时序残差动态分配不同帧的 token 预算；Spatial Coverage Top-K (SC-TopK)：在空间维度上保证 token 选择的空间覆盖，避免全部 token 集中在少数显著区域。整体上，VTC 是即插即用设计，不需要对 MLLM 执行任何额外调优，token 编码过程独立于模型训练。作者将其与传统视频编码对比，强调 VTC_{Dy} 在压缩率和性能保持上的优势。

Q4: 论文做了哪些实验？

论文将 VTC 应用到三个视频 MLLM 上，并在多个视频理解基准上开展实验。核心实验设置是 token 预算控制：在 50% 和 25% 两种 token 预算下比较 VTC 与 baseline 的性能保持率。主要评测对象是 Qwen3-VL，结果包括：VTC_{Dy} 在 50% token 预算下平均性能保持 100.1%，在 25% token 预算下平均性能保持 97.8%。论文还在另外两个 MLLM 上验证了方法的通用性，并进行了 VTC 与 VTC_{Dy} 的对比实验（即验证三项动态设计的贡献）。由于检索证据中未见逐基准数值表、baseline 方法列表和消融细节，具体数据集名称、对比方法和每项动态设计的独立消融数值需要回原文确认。

Q5: 发现了什么实验现象？

最核心的实验观察是：50% token 预算下 Qwen3-VL 平均性能保持率达 100.1%，意味着压缩一半 token 不仅没有损失，还出现轻微平均性能提升——这是一个反直觉现象，说明 token 冗余中确实包含干扰信息，结构化压缩可以起到隐式去噪/去冗余的作用。在更激进的 25% token 预算下仍有 97.8% 的性能保持，显示残差驱动的预算分配在极低预算下依然稳健。文中将 VTC 与 VTC_{Dy} 对比，说明动态分辨率、动态 token 分配和空间覆盖 Top-K 三项设计对压缩率和性能保持均有增益。由于检索证据有限，以下内容属于合理推断：VTC_{Dy} 相对 VTC 的增益在低预算设置下可能更明显，因为动态设计主要针对极端压缩下的信息保留。另外，不同视频类型（如静态场景 vs 剧烈运动场景）上的表现差异、以及不同 MLLM 上的增益幅度差异未见明确数据，属于信息缺口。

Q6: 有什么可以进一步探索的点？

可从以下方向延伸：1) 将 VTC 的 I/P 帧预测与残差度量从手工设计升级为可学习模块，使其自适应不同视频分布；2) 把残差信息用于指导 token merging 或量化，与 pruning 结合形成多级压缩管线；3) 将 VTC 扩展到流式/在线视频理解场景，测试实时 token 预算分配；4) 探索 VTC 与长视频记忆机制（如 memory bank、KV cache 压缩）的协同；5) 在不同视频 MLLM 架构（不同视觉编码器、不同 token 化方式）上系统验证 VTC 的通用性，并分析视觉编码器对 I/P 帧预测质量的影响；6) 将视频编码中更丰富的工具（如双向预测 B 帧、分层编码、码率控制）引入 token 分配，建立与经典编码理论的更完整类比；7) 分析性能保持率与视频内容属性（运动强度、场景切换频率、语义密度）之间的关系，从而理解 VTC 的失效边界；8) 将 token 残差作为可解释性信号，用于分析 MLLM 对视频哪些内容敏感、哪些内容冗余。

Q7: 总结一下论文的主要内容

该论文解决的核心问题是视频 MLLM 输入视觉 token 数量过大带来的计算与内存开销。作者观察到 Qwen3-VL 等先进模型处理 1 FPS 的一小时视频约需 46 万视觉 token，现有 token 压缩方法多为 token 级重要性度量，缺乏对视频时间结构的利用。作者从经典视频编码（HEVC）中得到启发，提出 Visual Token Coding (VTC) 范式：将视觉 token 组织成语义 I/P 帧，通过预测 I 帧和 P 帧并计算帧间残差来度量 token 冗余，再据此进行结构化压缩。这一设计将压缩问题从逐 token 重要性排序转换为视频层级的冗余暴露。在此基础上，作者提出 VTC_{Dy}，加入 Dynamic Resolution Input (DyRSO)、Dynamic Token Allocation (DyTA) 和 Spatial Coverage Top-K (SC-TopK) 三种动态机制，分别处理输入分辨率、不同帧的 token 预算分配和空间覆盖问题。实验方面，VTC 被应用于三个视频 MLLM，在多个视频理解基准上验证。Qwen3-VL 上 VTC_{Dy} 在 50% token 预算下平均性能保持 100.1%，在 25% 预算下保持 97.8%。方法为即插即用设计，无需对 MLLM 进行额外调优。结论部分作者指出，视频编码的结构化预测与残差编码可以补充帧级重要性分数的不足，在视频层面暴露冗余，为长视频 MLLM 的 token 预算受限场景提供了实用基础。代码已开源。总的来说，该工作的论证主线是“视频编码思想 → 结构化压缩范式 → 动态增强 → 多模型多基准验证”，技术主线是“I/P 帧预测 → 帧间残差度量 → token 预算分配 → 空间覆盖约束”，实验主线是“token 预算比例扫描 × 多 MLLM 通用性 × 动态设计消融”。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对从事高效多模态大模型研究的用户：VTC 提供了一个区别于 token 级重要性度量的结构化压缩范式，值得作为 baseline 或灵感来源

## 基本信息

- 作者：Chenxin Fang, Tao Chen, JunChao You, Jun Peng, Yiyi Zhou, Rongrong Ji
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-08-31
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.28008`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索命中的摘要、引言、方法与结论片段，并在此基础上结合论文元数据进行了信息补全与合理推断。
