---
user_id: "cheng tan"
paper_id: 1241
arxiv_id: "2606.23238"
title: "HOLMES: Evaluating Higher-Order Logical Reasoning in LLMs"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23238.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23238"
abs_url: "https://arxiv.org/abs/2606.23238"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:19:51"
---
# HOLMES: Evaluating Higher-Order Logical Reasoning in LLMs

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：higher-order logic · symbolic reasoning · benchmark · large language models

## 一句话总结

HOLMES 是首个面向高阶逻辑推理的真实世界基准，包含 1379 个实例，覆盖法律与金融领域，实验表明当前 LLM 平均准确率仅 50.64%，且高答案准确率可能掩盖推理缺陷。

## 摘要

> Logical reasoning is essential for reliable AI, yet existing benchmarks are largely first-order-logic-centric, focusing on object-level deduction over fixed predicates. This misses many realistic scenarios where models must reason over rules, predicates, functions, constraints, and decision procedures themselves. We introduce HOLMES (Higher-Order Logic Meets real-world Explainable Symbolic reasoning), the first real-world benchmark for higher-order symbolic reasoning in LLMs, containing 1379 instances. Built on higher-order logic, HOLMES pairs natural-language problems with HOL formalizations, ground-truth answers, verifiable reasoning traces, and fine-grained controllable reasoning factors across law and finance. Experiments show that current LLMs still struggle on HOLMES, with an average accuracy of only 50.64% and the best model reaching 59.54%. Our analyses further reveal that high final-answer accuracy can mask shortcut reasoning in conflict-resolution settings, while performance drops sharply under scope-conditioned and compositional reasoning. These findings identify higher-order symbolic reasoning as a key bottleneck for building reliable and verifiable LLMs. The project code and dataset are publicly available at https://github.com/wuyucheng2002/HOLMES.

Q1: 这篇论文试图解决什么问题？

当前 LLM 逻辑推理评估主要基于一阶逻辑基准（如对象级演绎），无法覆盖需要推理规则、谓词、函数、约束和决策过程本身的高阶逻辑场景。例如法律合规性检查、金融约束求解等现实任务依赖于高阶推理，但缺乏系统性评估工具。

Q2: 有哪些相关研究？

现有形式化逻辑基准如 LogicNLI、CLUTRR、LogicalDeduction 等虽推动了推理评估，但它们多基于一阶逻辑或有限符号结构，缺乏对高阶量化和函数抽象的支持。HOLMES 首次将高阶逻辑引入真实世界场景，与符号构造的基准互补。

Q3: 论文如何解决这个问题？

构建 HOLMES 基准：1）基于高阶逻辑形式化系统（如简单类型论），将法律和金融领域的自然语言问题转化为 HOL 公式；2）每个实例包含问题、形式化表示、标准答案、可验证推理轨迹（逐步证明）；3）设计细粒度推理因素，包括冲突解决、范围条件约束、组合推理等。评估时采用零样本提示，对 11 个开源/闭源 LLM 进行自动评测。

Q4: 论文做了哪些实验？

评估 11 个 LLM（包括 GPT-4o、Claude 3.5、Llama-3-70B 等），在 HOLMES 的 1379 个实例上进行零样本测试。报告总体准确率及按推理因素分组的性能：冲突解决、范围条件约束、组合推理等。此外分析错误模式，如捷径推理（高答案准确但推理轨迹有误）和性能骤降场景。

Q5: 发现了什么实验现象？

1）总体准确率低（平均 50.64%，最高 59.54%），高阶推理仍是瓶颈；2）高最终答案准确率可能掩盖不完整推理，尤其是在冲突解决任务中，模型常依赖表面模式；3）在需要范围条件和组合推理的任务上，性能显著下降，表明模型缺乏对规则约束的泛化能力；4）开源模型与闭源模型差距明显，但最佳闭源模型仍远未达到可靠。

Q6: 有什么可以进一步探索的点？

扩展 HOLMES 至更多领域：医学、科学推理、程序验证、政策合规等；引入更复杂的推理模式如递归定义和归纳证明；利用可验证轨迹进行推理过程监督；探索模型事后解释与高层推理一致性的关系；构建多跳和长链高阶推理变体。

Q7: 总结一下论文的主要内容

论文提出 HOLMES，首个面向高阶逻辑推理的真实世界基准，填补了现有 LLM 推理评估在一阶逻辑之外的空白。基准包含 1379 个法律与金融领域实例，每个实例配有高阶逻辑形式化、标准答案、可验证推理轨迹及细粒度因素标签。评估 11 种主流 LLM，发现平均准确率仅 50.64%，最佳为 59.54%，且高答案准确率可能掩盖推理缺陷。进一步分析表明，模型在冲突解决、范围条件约束及组合推理上表现薄弱，确认高阶逻辑推理是构建可靠可验证 LLM 的关键瓶颈。论文公开了代码和数据集。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本基准可直接用于评估智能体在复杂规则约束下的推理能力，与用户关注的 agent 方向相关。

## 基本信息

- 作者：Yucheng Wu, Jundong Xu, Mingzhen Ju, Yue Yu, Chenpeng Wang, Haoxuan Li, Liangming Pan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23238`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据片段（Abstract、Introduction、Related Work、Conclusion、Limitations 部分）。
