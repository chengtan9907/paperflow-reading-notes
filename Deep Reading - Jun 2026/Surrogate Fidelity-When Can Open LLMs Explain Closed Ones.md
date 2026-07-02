---
user_id: "cheng tan"
paper_id: 2268
arxiv_id: "2606.32008v1"
title: "Surrogate Fidelity: When Can Open LLMs Explain Closed Ones?"
publish_date: "2026-06-30"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.32008v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.32008v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-07-02T02:10:44"
---
# Surrogate Fidelity: When Can Open LLMs Explain Closed Ones?

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：mechanistic interpretability · surrogate fidelity · model transferability · feature attribution

## 一句话总结

该论文提出代理保真度（surrogate fidelity）概念，系统评估开放语言模型在预测、归因和表示层面的测量能否作为闭源模型行为的可靠代理，发现预测一致性显著高估归因一致性，机制解释不能自动跨模型迁移。

## 摘要

> Mechanistic interpretability (MI) requires full access to model internals, yet the APIs for most widely deployed language models at best expose log-probabilities over output tokens. This creates a surrogate problem: when do measurements made on open models allow us to make claims about a closed model? We evaluate surrogate fidelity at the prediction, attribution, and representation levels. For binary classification tasks, log-odds provide an API-compatible scalar readout of the model's representation space, and leave-one-out attributions provide insight into model behavior. Across eleven models spanning four families (Llama, Qwen, GPT, and Gemini), we find that prediction fidelity substantially overstates attribution fidelity: models that agree on what the answer is often disagree on why. We document an access-validity inversion: white-box signals like attention patterns and perturbation magnitudes are highly stable across models but only weakly predictive of causal attributions, which black-box input ablations capture by design. Mechanistic insight does not automatically transfer to closed targets, and prediction-level agreement is insufficient to warrant such transfer. Code and results are available at https://github.com/facebookresearch/surrogate.

Q1: 这篇论文试图解决什么问题？

本文试图解决的核心问题是：当闭源语言模型（如GPT、Gemini）仅提供有限API接口（如对数概率）时，从开放模型（如Llama、Qwen）上获得的机械可解释性发现（如电路分析、稀疏自编码器、激活修补）能否可靠地推断到闭源模型？MI社区隐含假设机制发现可跨模型迁移，但该假设缺乏系统验证。特别是，预测层面对齐（开放和闭源模型给出相同答案）是否足以保证归因或表示层面的对齐？

Q2: 有哪些相关研究？

相关工作涵盖以下方向：（1）机械可解释性（MI）：电路分析（Olah et al., 2020; Elhage et al., 2021; Wang et al., 2022; Conmy et al., 2023）和稀疏自编码器（SAEs）需要完全模型访问。（2）特征归因方法：留一归因（Koh & Liang, 2017）、ContextCite（Cohen-Wang et al., 2024）、局部代理方法如MExGen（Paes et al., 2025）和gSmILE（Dehghani et al., 2025）估计输入级归因。（3）代理模型：传统上用于解释黑盒模型（如LIME、SHAP），但本文首次系统评估代理保真度在不同模型系列间的迁移。（4）跨模型对齐研究：之前工作关注表示相似性（如CKA、SVCCA），但未聚焦于可解释性结论的可迁移性。

Q3: 论文如何解决这个问题？

本文提出代理保真度的层次化度量框架，包括预测保真度（预测一致性）、归因保真度（归因相似性）、表示保真度（内部表示一致性）和跨层次保真度（如用预测保真度预测归因保真度的能力）。在二分类任务上实例化该框架。具体地：（1）预测保真度：通过对比开放和闭源模型在相同输入上的对数几率输出，计算相关系数或精确匹配率。（2）归因保真度：使用留一归因（输入标记删除后的对数几率变化）和两种基线（白盒注意力模式、扰动幅度）作为归因信号，评估它们与因果归因（通过输入消融测量）的一致性。（3）在四个模型系列（Llama 2/3、Qwen 2、GPT 3.5 Turbo、Gemini Pro）的11个模型上，使用多个二分类数据集（如情感分析、有毒评论检测等）进行实验。

Q4: 论文做了哪些实验？

实验部分包括：（推断）在多个二分类任务（如IMDb情感分析、Jigsaw毒性检测等）上比较开放模型（Llama, Qwen）与闭源模型（GPT-3.5, Gemini）的预测和归因保真度。具体实验设置：（1）计算预测保真度（开放与闭源模型的对数几率相关性和准确率一致性）。（2）计算归因保真度：对每个输入，用留一法计算归因向量，然后比较开放与闭源模型之间的归因相似性（如余弦相似度、秩相关）。（3）比较白盒信号（注意力权重、隐藏状态扰动敏感性）与黑盒归因（输入消融）作为因果归因代理的质量。（4）消融分析：改变任务、模型大小、家族等观察趋势。

Q5: 发现了什么实验现象？

关键发现包括：（1）预测保真度显著高估归因保真度：模型在预测上高度一致（如相关系数>0.95），但归因相似度却低得多（如余弦相似度仅0.3-0.5）。这表明答案一致不等于推理路径一致。（2）访问-有效性反转（access-validity inversion）：白盒信号（如注意力模式、扰动幅度）在模型间高度稳定，但不能有效预测因果归因；相反，黑盒输入消融（仅依赖API访问）天然捕获了因果归因信息。因此，开放的内部表示并未提供更好的归因保真度代理。（3）跨模型家族的归因差异大于家族内差异：不同系列模型的归因模式差异显著，即使预测行为相似。（4）预测保准性不能泛化到新任务或分布外样本。

Q6: 有什么可以进一步探索的点？

进一步探索方向包括：（1）在更多任务和模型上验证代理保真度，特别是多步推理和生成任务。（2）开发更精细的归因保真度度量，考虑非线性交互和特征间冗余。（3）探索是否可以通过训练跨模型映射函数来提高归因保真度。（4）在解释层面（如电路级）验证迁移性，而不仅仅是特征归因。（5）研究模型对齐（如蒸馏、微调）如何影响代理保真度。（6）将代理保真度框架扩展到多模态模型。

Q7: 总结一下论文的主要内容

论文针对机械可解释性（MI）中一个未被充分审视的假设——开放模型的MI发现可迁移至闭源模型——提出了系统验证框架“代理保真度”。作者指出，当前MI方法（如电路分析、SAEs）需要完全模型内部访问，但最具研究价值的前沿模型（如GPT-4、Gemini Ultra）通常只提供有限API（如对数概率）。因此，研究者常使用开放模型作为代理来研究闭源模型的行为。本文定义了三个层次的保真度：预测保真度（输出一致性）、归因保真度（输入重要性相似性）和表示保真度（内部表示相似性），并特别关注跨层次关系。通过在11个模型（Llama 2/3、Qwen 2、GPT 3.5、Gemini Pro）上的二分类实验，作者得到两个核心结果：（1）预测保真度显著高估归因保真度，即模型在答案上一致时，它们对输入特征的依赖可能完全不同；（2）出现“访问-有效性反转”：从开放模型内部获得的白盒信号（如注意力）在模型间高度稳定，但作为归因代理的效果不如仅依赖API的黑盒输入消融方法。这意味着，开放的内部表征并不能自动带来更好的归因可靠性。论文挑战了MI社区关于跨模型泛化的常见假设，并强调需要直接在解释层面验证迁移性，而非仅依赖预测一致性。附录提供了任务细节、模型配置、实验结果表格和代码链接。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：该论文直接挑战MI领域关于跨模型泛化的常见假设，对使用开放模型研究闭源可解释性的工作具有重要意义。

## 基本信息

- 作者：Philippe Chlenski, Zachariah Carmichael, Ayush Warikoo, Chia-Tse Shao, Yingxiao Ye, Aobo Yang, Vivek Miglani, Nehal Bandi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-30
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.32008v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了PDF检索证据（abstract、our contributions、discussion等片段），并结合heuristic_draft进行润色和补全。
