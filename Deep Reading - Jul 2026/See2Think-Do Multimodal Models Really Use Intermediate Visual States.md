---
user_id: "cheng tan"
paper_id: 5979
arxiv_id: "2607.26769v1"
title: "See2Think: Do Multimodal Models Really Use Intermediate Visual States?"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26769v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26769v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:12:59"
---
# See2Think: Do Multimodal Models Really Use Intermediate Visual States?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · intermediate visual states · visual reasoning evaluation · benchmark

## 一句话总结

提出了See2Think统一评估框架，包含See2ThinkBench基准和Visual Action-of-Thought (VAoT)评估协议，用于诊断多模态大模型是否真正依赖中间视觉状态进行推理。

## 摘要

> Multimodal large language models increasingly use sketches, annotations, tools, and intermediate images during reasoning, but it remains unclear whether they truly rely on these visual states. Existing benchmarks are limited both by task collections with narrow coverage or partially text-solvable samples and by evaluations that emphasize final answers without diagnosing how intermediate visual states are generated, rendered, and used. We introduce See2Think, a unified evaluation framework comprising See2ThinkBench and Visual Action-of-Thought (VAoT). See2ThinkBench contains 1,200 open-ended, visually dependent problems across 12 task categories spanning 2D structured, 3D scene, and real-world reasoning. VAoT records textual thoughts, visual actions, rendered states, and subsequent reasoning under four controlled inference settings. Evaluating representative proprietary and open-source multimodal models, we find that visual reasoning is strongly model- and environment-dependent, with no single setting consistently dominating across tasks. Process analysis further shows that models usually select relevant visual operations, while faithful rendering remains the clearest bottleneck and high feedback uptake does not necessarily translate into accuracy gains. Under task-relevant corrupted feedback, models exhibit behavioral dependence on visual states, with accuracy dropping by over 10 percentage points in 3D scenes. See2Think disentangles visual-state utility from behavioral dependence and provides a unified framework for evaluating whether multimodal models genuinely use intermediate visual states.
> Website: https://sgysy.github.io/seetothink/
> Repository: https://github.com/CSU-JPG/See2Think

Q1: 这篇论文试图解决什么问题？

多模态大模型在推理中生成或使用中间视觉状态（如草图、标注、工具输出），但现有评估方法无法诊断模型是否真正依赖这些视觉状态。主要问题包括：(1) 现有基准任务覆盖狭窄，部分样本可仅通过文本求解，无法区分视觉贡献；(2) 评估只关注最终答案，不分析中间视觉状态的生成、渲染和使用过程；(3) 缺乏可控的干预实验来测试视觉状态对推理的影响。因此，需要一套系统框架来分离视觉状态的效用和模型对其的行为依赖，从而判断“用图像思考”的真实性。

Q2: 有哪些相关研究？

相关工作包括：(1) 多模态思维链（Multimodal CoT）扩展文本CoT到多模态问题，评估推理质量、鲁棒性等。(2) 主动视觉推理：模型在推理中调用外部工具生成中间视觉信息。(3) 视觉状态忠实性评估：检查中间视觉信息是否忠实于输入或关键。(4) 视觉状态有用性诊断：通过移除或扰动视觉状态评估其贡献。然而，现有方法常使用oracle视觉线索或聚合分数，缺乏匹配干预和过程分析。See2Think通过统一基准和受控协议填补了空白。

Q3: 论文如何解决这个问题？

See2Think框架由两部分构成：(1) See2ThinkBench：包含1200个开放型、视觉依赖的推理问题，覆盖2D结构化（图表、电气原理图、地图）、3D场景（空间推理、计数）和真实世界（户外导航、视觉混音）。每个问题经人工筛选确保视觉不可替代，并提供统一答案生成接口（文本或选择）。(2) VAoT：一种可观察和可控的推理记录协议，在四种设置下运行：无视觉输入、仅初始视觉、初始视觉+操作但无中间渲染、完整过程（操作+渲染）。每种设置记录文本推理步骤、视觉操作指令、外部工具返回的渲染图像、以及后续推理。通过比较不同设置的最终答案和过程行为，诊断视觉状态是否被生成、渲染和使用。

Q4: 论文做了哪些实验？

论文评估了GPT-4o、Gemini、Claude、LLaVA、Qwen-VL等模型在See2ThinkBench上的表现。实验包括：(1) VAoT四种设置下的准确率比较；(2) 过程分析：模型选择的操作类型、渲染准确性、反馈采纳率；(3) 干扰实验：在任务相关和无关条件下破坏中间视觉反馈，观察模型行为。主要变量包括模型类型、任务类别、VAoT设置和反馈方式。

Q5: 发现了什么实验现象？

实验发现：(1) 视觉推理表现高度依赖于模型和推理环境，没有一种设置能在所有任务上一致最优。(2) 模型通常能选择相关视觉操作，但工具返回的渲染图像是否忠实是主要瓶颈。(3) 高反馈采纳率不直接带来高准确率，表明渲染质量或后续推理存在脱节。(4) 在任务相关破坏反馈下，模型表现出明显的视觉状态行为依赖，准确率在3D任务中下降超10个百分点；随机破坏影响较小。(5) 提供初始视觉输入在某些任务上提升显著，但更多操作不一定持续有益。

Q6: 有什么可以进一步探索的点？

未来方向包括：(1) 扩展See2ThinkBench到更多领域和复杂视觉状态类型。(2) 研究不同模型架构对视觉状态使用的影响。(3) 改进外部工具渲染忠实度。(4) 将VAoT扩展到交互式或在线学习场景。(5) 深入分析行为依赖与模型内部表示的关系。(6) 探索在训练中引入对忠实性的正则化或对抗训练。

Q7: 总结一下论文的主要内容

论文See2Think旨在系统评估多模态大模型在推理过程中是否真正使用中间视觉状态。现有方法让模型生成草图、使用工具得到中间图像，但评估限于最终答案，无法判断这些视觉状态是否被真正依赖。作者构建了See2Think框架，包含See2ThinkBench基准和VAoT推理协议。See2ThinkBench收录1200个人工筛选的视觉依赖问题，涵盖2D结构化、3D场景和真实世界场景，确保无法仅靠文本解答。VAoT协议设计了四种推理设置：无视觉、仅初始图像、初始+操作但无中间渲染、完整过程（操作+渲染），并记录每一步推理。评估GPT-4o、Gemini、Claude、LLaVA、Qwen-VL等模型，主要发现：(1) 视觉推理效果高度依赖于模型和设置，无单一最优；(2) 模型通常能选择正确操作，但渲染缺失或不准确是主要瓶颈；(3) 高反馈采纳率不保证高准确率；(4) 在任务相关破坏反馈下，模型表现出强烈的视觉状态行为依赖，准确率在3D任务中下降超10个百分点。研究分离了视觉状态的“效用”和“行为依赖”，为理解多模态推理的可信度和鲁棒性提供了关键见解。主要贡献：提出统一评估框架，构建高质量基准，设计过程级分析协议，揭示视觉状态使用的关键瓶颈。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：提出了评估多模态模型使用中间视觉状态的新范式，与多模态理解和可解释性研究密切相关。

## 基本信息

- 作者：Siyu Yan, Zhuoran Yan, Haiying Xu, Panhao Zhou, Jingyu Chen, Chenhao Ji, Shuo Cao, Yongheng Zhang, Haoze Liu, Siyu Zhang, Xiwen Gu, Yihao Liu, Alex Jinpeng Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26769v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了从PDF提取的检索证据。
