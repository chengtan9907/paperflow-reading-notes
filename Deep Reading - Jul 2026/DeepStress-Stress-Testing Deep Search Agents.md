---
user_id: "cheng tan"
paper_id: 4074
arxiv_id: "2607.13920"
title: "DeepStress: Stress-Testing Deep Search Agents"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.13920.pdf"
pdf_url: "https://arxiv.org/pdf/2607.13920"
abs_url: "https://arxiv.org/abs/2607.13920"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-17T01:04:10"
---
# DeepStress: Stress-Testing Deep Search Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：search agents · stress testing · robustness · document reliability

## 一句话总结

本文提出 DeepStress，一个压力测试框架，通过替换搜索代理的检索模块为可控合成环境，评估其对不可靠证据的鲁棒性，并揭示代理间的显著行为差异。

## 摘要

> While search agents demonstrate impressive capabilities in multi-step question answering, their robustness to poor-quality evidence remains underexplored. This phenomenon occurs rarely in realistic benchmarks but can lead to dramatic failure in real life applications. Therefore in this study we propose DeepStress, a stress testing framework that controls the frequency of challenging evidence by replacing the retrieval module of search agents with a controlled synthetic environment. We use this framework to control three dimensions that can affect document reliability: trustworthiness, relevance, and factuality. Testing several search agents on HotpotQA and BrowseComp-Plus, we demonstrate that agents exhibit substantial differences in their ability to handle unreliable information and propose new metrics that better document systems outcomes as well as the interactions between conflicting parametric and retrieved knowledge.

Q1: 这篇论文试图解决什么问题？

搜索代理在多步问答任务中表现出色，但其对低质量证据（例如不可靠、不相关或不事实的文档）的鲁棒性尚未被系统研究。现有基准测试中此类情况发生频率低，掩盖了实际部署中的潜在失败风险。论文旨在填补这一空白，提出一种可控的压力测试方法来评估和揭示代理在恶劣文档条件下的行为差异。

Q2: 有哪些相关研究？

相关研究包括检索增强生成（RAG）在知识密集型任务中的应用，以及 LLM 处理检索来源冲突的能力。例如，TRACK 研究 LLM 如何传播新提供的事实，而其他工作则分类了 RAG 中的冲突情况。此外，传统压力测试方法主要关注模型对对抗性例子的鲁棒性，但针对搜索代理在动态检索环境中对证据质量退化的鲁棒性研究较少。DeepStress 填补了空白，提供了一个可控的合成环境用于系统评估。

Q3: 论文如何解决这个问题？

论文提出 DeepStress 框架，其核心是拦截搜索代理的检索调用，用合成文档生成器代替真实检索。该生成器可以根据实验设置产生具有不同信任度、相关性和事实性水平的文档。框架允许精确控制每个搜索返回的文档质量，从而模拟现实中可能遇到的低质量证据场景。通过这种方式，可以在受控条件下测试不同搜索代理的行为，并测量其依赖参数知识还是检索知识。

Q4: 论文做了哪些实验？

论文在 HotpotQA 和 BrowseComp-Plus 两个数据集上进行实验，测试了多种搜索代理（具体名称未在摘要中明确指出）。实验通过控制合成文档的信任度、相关性和事实性三个维度，生成低质量证据场景。评估指标包括最终答案准确率、模型表达不确定性的频率、工具调用次数以及对退化文档的反应行为。此外，还分析了代理在处理冲突的检索知识与参数知识时的表现。

Q5: 发现了什么实验现象？

实验观察到不同搜索代理在面临低质量证据时表现出显著的行为差异。例如，某些代理在文档不可靠时更容易产生错误答案，而另一些则会表达不确定性或增加工具调用次数以寻求更多信息。在信任度、相关性和事实性三个维度上，代理的鲁棒性各不相同。此外，代理在参数知识与检索知识冲突时表现出不同的偏好，部分代理更依赖自身参数知识，部分则更信任检索结果。

Q6: 有什么可以进一步探索的点？

未来工作可以扩展 DeepStress 框架以支持多文档检索场景，更贴近现实搜索环境。还可以研究更多维度的文档质量问题，例如文档的毒性、偏见或对抗性内容。此外，可以探索如何利用这些压力测试结果改进搜索代理的训练或提示策略，以增强其对低质量证据的鲁棒性。

Q7: 总结一下论文的主要内容

论文针对搜索代理在现实应用中可能遭遇低质量证据从而导致失败的问题，提出 DeepStress 框架，通过用合成文档生成器替换检索模块，在受控环境下系统评估搜索代理对不可靠信息的鲁棒性。框架控制文档的信任度、相关性和事实性三个维度。在 HotpotQA 和 BrowseComp-Plus 数据集上测试多种搜索代理，发现代理间存在显著行为差异，并提出新指标更好地记录系统输出以及冲突知识交互。实验揭示了代理处理退化文档的不同策略，如是否表达不确定性、是否增加检索等。论文还分析了参数知识与检索知识的交互作用。这项工作为评估和提升搜索代理的可靠性提供了重要工具和见解。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent）研究方向高度重合，关注搜索代理的可靠性和行为差异。

## 基本信息

- 作者：Ismael Rousseau, Geraldine Damnati, Frederic Bechet
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.13920`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 extracted evidence 和 PDF检索结果，结合 heuristic_draft 进行中文改写与补全。
