---
user_id: "cheng tan"
paper_id: 3842
arxiv_id: "2607.12786v1"
title: "CoRe: A Comprehensive Framework for Cross-Image Comparative Reasoning in Vision-Language Models"
publish_date: "2026-07-14"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.12786v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.12786v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-16T01:56:15"
---
# CoRe: A Comprehensive Framework for Cross-Image Comparative Reasoning in Vision-Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：cross-image comparative reasoning · vision-language model · dataset construction · reward framework

## 一句话总结

提出CoRe框架，包括CoRe-20K数据集、TriSR奖励结构和CoRe-Bench基准，显著提升视觉语言模型在跨图像比较推理任务上的性能。

## 摘要

> Cross-image comparative reasoning remains challenging for vision-language models (VLMs), especially when correct prediction requires fine-grained attribute grounding and globally consistent reasoning. We present CoRe, a unified framework for this problem. CoRe includes: (i) CoRe-20K, a large-scale triplet-based training set automatically constructed from structured visual metadata through a multi-expert collaborative pipeline, covering counting, depth, distance, and spatial relations; (ii) TriSR, a structured reward framework that jointly supervises attribute grounding, judgment alignment, and triplet consistency under GRPO optimization; and (iii) CoRe-Bench, the first benchmark dedicated to fine-grained cross-image comparative reasoning. Experiments show that CoRe substantially outperforms existing VLMs on CoRe-Bench while remaining competitive on standard multimodal benchmarks, achieving a 28.2-point gain in partial accuracy over the strongest baseline.

Q1: 这篇论文试图解决什么问题？

1. 跨图像比较推理是VLM的一项关键能力，但现有模型在该任务上表现不佳。具体表现为：当需要同时进行细粒度属性定位（如计数、深度、距离）和跨图像一致性比较时，模型往往给出错误答案。
2. 现有基准存在两个不足：一是主要评估单图像感知或通用多图像理解，缺乏专门针对跨图像比较推理的细粒度评测；二是问题设计往往偏向整体语义集成（如时序关系、图片风格对比），而非原子化的属性比较。
3. 这种系统性限制导致VLM在简单比较任务上也会失败，例如判断两图中哪个物体更多、哪个场景更远等。
4. 因此，迫切需要一种统一框架，既能提供专用训练数据，又能设计针对性奖励机制，从而提升VLM的跨图像比较推理能力。

Q2: 有哪些相关研究？

1. 单图像VLM基准：如MMBench、MMMU等，主要评估模型在单张图像上的视觉感知与知识推理，不涉及跨图像比较。
2. 多图像VLM基准：如MuirBench、MMRB、MT-Bench等，评估多图像理解能力，但侧重语义集成、时序理解或对话一致性，而非细粒度属性比较。
3. 比较推理数据集：现有一些数据集包含关系比较（如Visual Genome中的相对位置），但规模有限且覆盖面窄，缺乏自动构建方法。
4. 强化学习奖励设计：GRPO等方法用于对齐，但以往工作较少针对跨图像比较设计结构化奖励。CoRe通过多信号联合监督填补了这一空白。

Q3: 论文如何解决这个问题？

CoRe框架包含三个核心部分：
1. **CoRe-20K数据集**：利用结构化视觉元数据（如深度图、目标计数、空间坐标）通过多专家协作pipeline自动生成比较三元组。pipeline包括：属性提取、问题生成、答案校验，最终获得约20,000个高质量三元组，覆盖计数、深度、距离、空间关系四类比较。
2. **TriSR结构化奖励框架**：在GRPO优化下设计三个奖励信号：
 - 属性定位奖励：确保模型关注正确的比较属性（如数量、深度）。
 - 判断对齐奖励：确保比较判断（如A大于B）与属性定位一致。
 - 三元组一致性奖励：确保整体推理逻辑在三元组内自洽。
 通过联合优化，引导模型学习结构化比较推理。
3. **CoRe-Bench基准**：包含多样化比较推理问题，每个问题提供两幅图像和一个比较问题，要求模型输出答案。基准覆盖四种关系类型，难度分级，用于标准化评测。
此外，训练中使用ms-swift库提供的重复惩罚等辅助损失，但非本文核心贡献。

Q4: 论文做了哪些实验？

实验从三个互补视角评估：
1. **Benchmark级性能**：在CoRe-Bench上对比多个开源VLM基线（如LLaVA、Qwen-VL、InternVL等），报告Partial Accuracy指标。
2. **诊断分析**：对TriSR各奖励组件进行消融，验证属性定位、判断对齐、三元组一致性的贡献；分析模型在不同比较类型（计数、深度等）上的表现差异。
3. **泛化测试**：在标准多模态基准（MMBench、MMMU、MME等）上评估CoRe，检查是否保持通用能力。
实验设置包括使用固定训练epoch、学习率等，基于ms-swift框架。

Q5: 发现了什么实验现象？

1. **现有VLM的系统性失败**：在CoRe-Bench上，基线模型普遍表现不佳，尤其在计数和深度比较任务上，表明跨图像比较推理确实是VLM的薄弱环节。
2. **CoRe的显著提升**：CoRe在Partial Accuracy上超过最强基线28.2个百分点，表明结构化奖励训练有效。
3. **奖励组件贡献**：消融显示每个奖励信号均不可或缺，其中属性定位奖励贡献最大，判断对齐次之。
4. **泛化保持**：在MMBench、MMMU等通用基准上，CoRe与基线相当或略有提升，说明未遗忘通用能力。
5. **数据规模效应**：实验显示CoRe-20K规模越大，性能提升越明显，但达到一定规模后收益递减。

Q6: 有什么可以进一步探索的点？

1. **扩展比较属性**：将CoRe-20K扩展至颜色、材质、形状等更多属性，提升覆盖范围。
2. **改进数据生成**：引入更鲁棒的多专家pipeline，减少自动标注噪声。
3. **任务泛化**：将CoRe框架应用于其他多图像任务，如视觉对话、视频理解。
4. **结合复杂推理**：探索将比较推理与多步推理、常识推理结合。
5. **鲁棒性研究**：测试CoRe在对抗性样本、分布漂移下的表现。
6. **奖励设计优化**：研究更复杂的奖励结构，如自适应权重、多任务学习。

Q7: 总结一下论文的主要内容

本文针对视觉语言模型（VLM）在跨图像比较推理任务上的不足，提出CoRe（Comprehensive Comparative Reasoning）统一框架。首先，通过多专家协作流水线从结构化视觉元数据自动构建了大规模比较三元组数据集CoRe-20K，覆盖计数、深度、距离和空间关系四类比较任务。其次，设计了TriSR（Triplet Structured Reward）框架，在GRPO优化下联合监督属性定位、判断对齐和三重一致性，使模型学会进行结构化比较推理。最后，构建了首个专用基准CoRe-Bench，用于细粒度评测。实验部分，CoRe在CoRe-Bench上以Partial Accuracy指标超过最强基线28.2个百分点，诊断分析验证了各奖励组件的有效性，通用基准测试表明方法保持了通用能力。主要贡献包括：提出统一框架CoRe，系统性解决跨图像比较推理问题；构建CoRe-20K数据集，实现自动化、覆盖多种比较类型；设计TriSR结构化奖励，通过多信号联合监督提升推理质量；提出CoRe-Bench基准，填补评测空白。局限性主要体现在数据集覆盖属性有限、自动生成可能引入噪声、基准规模较小以及训练计算成本较高。未来工作可从属性扩展、鲁棒性、任务泛化等方向展开。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本文提出的结构化奖励框架可推广至其他需要多信号联合监督的推理任务

## 基本信息

- 作者：Lin Peng, Cong Wan, Zeyu Guo, SongLin Dong, Yihong Gong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-07-14
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.12786v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本摘要基于PDF语义检索证据和启发式草稿生成，部分细节可能需进一步查阅全文确认。
