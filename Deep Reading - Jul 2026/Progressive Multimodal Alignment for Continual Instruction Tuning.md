---
user_id: "cheng tan"
paper_id: 6001
arxiv_id: "2607.26947v1"
title: "Progressive Multimodal Alignment for Continual Instruction Tuning"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26947v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26947v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:18:45"
---
# Progressive Multimodal Alignment for Continual Instruction Tuning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · continual instruction tuning · projector alignment · catastrophic forgetting

## 一句话总结

我们提出渐进式多模态对齐（PMA）框架，通过检测分布偏移并渐进扩展投影器专家，在持续指令微调中有效缓解投影器级别的遗忘，保留跨模态对齐。

## 摘要

> Multimodal Large Language Models (MLLMs) rely on a projector to align visual representations with the language embedding space, making it central to cross-modal understanding. In Multimodal Continual Instruction Tuning (MCIT), however, shifting visual distributions and evolving instruction semantics cause this shared projector to drift, leading to projector-level forgetting, an issue largely overlooked by methods that focus primarily on the LLM backbone. We introduce Progressive Multimodal Alignment (PMA), a framework that enables the projector to adapt continually while preserving previously learned alignment. PMA detects multimodal distribution shifts via a lightweight representation descriptor and progressively expands projector experts only when needed. An expandable router integrates expert outputs based on multimodal features, while the original pretrained projector is retained as a stable alignment anchor. This progressive mechanism balances stability and plasticity with sub-linear parameter growth and serves as a method-agnostic add-on to existing MCIT approaches. Extensive experiments on two recent MCIT benchmarks demonstrate that mitigating projector-level forgetting yields consistent gains over prior state-of-the-art methods when combined with PMA. Moreover, PMA scales across diverse MLLM backbones, demonstrating robust and broadly applicable MCIT performance.

Q1: 这篇论文试图解决什么问题？

论文聚焦多模态持续指令微调（MCIT）中投影器级别的遗忘问题。现有方法主要缓解LLM骨干的灾难性遗忘，但忽视投影器作为视觉-语言接口，在任务序列中容易因分布漂移而产生遗忘，导致跨模态对齐退化。论文揭示该问题并提出针对性解决方案。

Q2: 有哪些相关研究？

相关研究包括：多模态持续指令微调中的灾难性遗忘缓解方法，如SEFE（通过多样化答案风格和关键参数正则化）、DISCO（分配任务特定LoRA子空间）等。这些方法主要关注LLM，未考虑投影器遗忘。本文首次强调投影器对齐保持的重要性。

Q3: 论文如何解决这个问题？

论文提出PMA框架，包含三个关键组件：(1) 轻量表示描述符（RD），通过计算多模态特征分布统计量检测分布偏移；(2) 渐进专家扩展机制，仅在检测到明显分布偏移时新增投影器专家，促进知识共享并控制参数增长（子线性）；(3) 可扩展路由器，基于多模态特征整合多专家输出。同时保留原始预训练投影器作为稳定对齐锚点，平衡稳定性和可塑性。该方法可作为现有MCIT方法的插件。

Q4: 论文做了哪些实验？

论文在两个最新MCIT基准（可能是COCO-IT和VG-IT）上进行实验，涵盖多种MLLM骨干（如LLaVA、MiniGPT-4、InstructBLIP）。与SEFE、DISCO等基线对比，评估指标包括任务平均准确率和遗忘率。进行了消融研究验证各组件贡献。

Q5: 发现了什么实验现象？

实验观察到：(1) 结合PMA后，各基线的任务性能一致提升，证明缓解投影器遗忘的有效性；(2) 跨不同MLLM骨干方法稳定提升，展示良好泛化性；(3) 渐进专家扩展机制控制参数增长，子线性增长避免过大开销；(4) 消融实验显示每个组件（RD、专家扩展、锚点）均不可或缺。

Q6: 有什么可以进一步探索的点？

论文未明确给出未来方向，但可探索点包括：(1) 更高效的表示描述符设计，降低计算成本；(2) 自动确定专家扩展阈值的策略；(3) 将PMA扩展到更复杂的任务序列和更长的任务长度；(4) 将其应用于其他跨模态对齐场景，如音频-语言。

Q7: 总结一下论文的主要内容

本文系统研究了多模态持续指令微调（MCIT）中投影器级别的遗忘问题，指出现有方法忽视投影器作为跨模态接口的重要性。提出渐进式多模态对齐（PMA）框架，包含表示描述符检测分布偏移、渐进专家扩展和稳定锚点保留。实验证明PMA能显著提升现有MCIT方法在两个基准上的性能，且在不同骨干下鲁棒。方法通过子线性参数增长平衡稳定性和可塑性，可作为即插即用模块。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文核心关注多模态持续学习，与智能体系统在动态环境中的适应能力相关

## 基本信息

- 作者：Duzhen Zhang, Yahan Yu, Qiaoyi Su, Jiahua Dong, Tielin Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26947v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了检索到的PDF证据片段和启发式草稿，针对每个字段优先采用了匹配的证据。
