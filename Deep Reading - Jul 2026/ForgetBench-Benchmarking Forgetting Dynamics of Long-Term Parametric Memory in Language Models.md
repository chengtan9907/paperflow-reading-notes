---
user_id: "cheng tan"
paper_id: 6121
arxiv_id: "2607.26455v1"
title: "ForgetBench: Benchmarking Forgetting Dynamics of Long-Term Parametric Memory in Language Models"
publish_date: "2026-07-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.26455v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.26455v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-31T01:25:56"
---
# ForgetBench: Benchmarking Forgetting Dynamics of Long-Term Parametric Memory in Language Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：large language models · knowledge editing · forgetting dynamics · long-term memory

## 一句话总结

ForgetBench是一个用于系统评估大语言模型在持续知识编辑下长期参数遗忘动态的基准，包含概念QA和场景QA两种互补范式，通过顺序编辑和统一评估框架揭示现有编辑方法在事实保留与泛化间的显著失衡。

## 摘要

> Large language models (LLMs) have demonstrated strong capabilities in knowledge acquisition and reasoning, yet their ability to retain previously acquired knowledge under repeated updates remains insufficiently understood. Existing evaluation paradigms primarily focus on single-step reasoning or static knowledge editing, which fail to capture the temporal dynamics of knowledge retention and degradation during continual model modification. In this work, we propose ForgetBench, a benchmark designed to systematically characterize forgetting behavior in LLMs under continual knowledge editing. ForgetBench introduces two complementary evaluation paradigms, namely concept-based QA and scenario-based QA, to disentangle isolated factual retention from structured relational knowledge preservation. Building upon a sequential editing framework, we construct temporally ordered knowledge streams and evaluate model behavior across multiple editing stages. To quantitatively analyze long-term retention dynamics, we further introduce a unified evaluation framework that models knowledge evolution over time, enabling the measurement of temporal decay, retention strength, and cross-instance stability. Extensive experiments across diverse models and editing methods demonstrate that existing approaches fail to strike a balance between long-term retention and generalization quality. Our findings highlight the need for more robust memory mechanisms that can effectively acquire, update, and preserve knowledge over time in future LLMs. Code will be released upon acceptance.

Q1: 这篇论文试图解决什么问题？

大语言模型（LLM）在知识获取和推理方面表现出色，但其在重复参数更新下保留先前知识的能力仍未充分理解。现有知识编辑评估标准主要集中于单步编辑的成功性，通过编辑准确率、泛化性和局部性等指标衡量单个编辑的效果。然而，真实应用中模型需不断接收新知识，形成连续编辑序列，早期知识可能因参数更新而遗忘（灾难性遗忘）。当前基准缺乏对这种长期动态的系统评估，具体问题包括：（1）缺乏对时间维度上知识保留和退化的建模；（2）现有评估将编辑视为独立事件，忽略编辑顺序和上下文干扰；（3）未能区分孤立事实和关系知识的遗忘模式；（4）缺少统一量化框架比较不同方法在长期保留上的表现。ForgetBench填补这一空白，通过系统基准和评估协议揭示LLM在持续编辑下的遗忘动态。

Q2: 有哪些相关研究？

相关研究集中在知识编辑基准和持续学习领域。现有基准如zsRE和UnKE主要评估单次编辑是否成功，使用准确率、泛化性和局部性指标，但忽视编辑序列效果。持续学习领域研究任务序列上的遗忘，但通常针对分类而非知识编辑。LLM记忆评估如MQuAKE和Counterfact同样局限于单步或少量编辑。本文的ForgetBench专门聚焦长期参数遗忘的时间动态，通过顺序编辑和定量框架弥补缺口。与其他基准不同，ForgetBench强调时间维度和知识类型区分，提供新的评估维度和见解。

Q3: 论文如何解决这个问题？

ForgetBench包含三个核心组件。评估范式：概念QA（围绕单一实体事实，便于控制参数干扰）和场景QA（基于多智能体交互图，测试结构化关系知识）。知识流构建：基于时间有序的编辑流，每步编辑改变一个事实，记忆年龄定义为原始编辑到当前测试间的编辑次数，可追踪遗忘曲线。统一评估框架：提出三个指标——时间衰减（记忆质量下降速率）、保留强度（给定年龄下正确率）和跨实例稳定性（不同顺序或实例间一致性）。三者共同多维度量化遗忘。

Q4: 论文做了哪些实验？

实验选取四个模型（Llama-3 8B、Llama-3.1 8B、Qwen-2.5 7B、DeepSeek-R1 7B）和多种编辑方法（FT、MEND、ROME、IKE等）。评估协议：构建包含100个编辑步的知识流，每步后测试所有先前知识的表现（概念QA和场景QA）。数据来源：概念QA基于Wikidata，场景QA基于人工交互图。评估指标按时间衰减、保留强度和跨实例稳定性计算。对比分析包括：不同编辑方法的遗忘曲线、模型差异、知识类型差异，以及消融实验如编辑顺序影响。

Q5: 发现了什么实验现象？

实验揭示关键现象：遗忘随编辑步数显著增加，呈指数衰减；场景QA关系知识遗忘快于概念QA孤立事实；编辑方法间存在权衡，如ROME初始准确率高但长期保留差，FT泛化好但编辑准确率低；跨实例稳定性不足，编辑顺序影响遗忘模式；DeepSeek-R1 7B在某些场景下表现出更好长期保留；时间衰减速率非恒定，早期快后期平缓。这些发现显示现有方法在长期保留与泛化间难以平衡。

Q6: 有什么可以进一步探索的点？

未来方向包括：（1）设计鲁棒记忆机制（弹性权重、记忆重放等）；（2）扩展评估至更大模型和多模态；（3）结合外部记忆（RAG）减轻参数遗忘；（4）优化连续编辑策略（顺序、频率）；（5）动态编辑场景（知识冲突、过期更新）；（6）因果分析参数更新致忘机制；（7）跨领域推广至持续学习任务；（8）评估遗忘对社会影响（安全性、公平性）。ForgetBench可扩展支持这些探索。

Q7: 总结一下论文的主要内容

本文提出ForgetBench，一个系统评估大语言模型（LLM）在持续知识编辑下长期参数遗忘动态的基准。现有知识编辑评估主要聚焦单步编辑的成功性（如编辑准确率、泛化性、局部性），忽视编辑序列中知识随时间退化的动态过程。ForgetBench通过两个互补评估范式填补此空缺：概念QA（Concept-based QA）测试模型对孤立事实的保留能力，通过顺序更新控制参数干扰；场景QA（Scenario-based QA）利用多智能体交互图引入结构化关系知识，评估模型对复杂关系知识的长期记忆。两者共同覆盖从单一事实到关系网络的记忆评估需求。基于顺序编辑框架，ForgetBench构建时间有序的知识流，定义记忆年龄（Memory Age）为后续编辑操作次数，模型在不同编辑阶段被周期性测试，追踪知识演化全过程。统一评估框架建模知识演化，量化时间衰减（Temporal Decay）、保留强度（Retention Strength）和跨实例稳定性（Cross-instance Stability）。实验在四个主流LLM（Llama-3 8B、Llama-3.1 8B、Qwen-2.5 7B、DeepSeek-R1 7B）上结合多种编辑方法（FT、MEND、ROME、IKE等）进行广泛评估。结果显示：所有方法随编辑次数增加准确率急剧下降；场景QA关系知识遗忘快于概念QA；不同编辑方法在保留和泛化间存在明显权衡；跨实例稳定性不足；模型架构影响遗忘模式。主要贡献包括：（1）首次提出系统评估长期参数遗忘的基准；（2）设计两种互补范式覆盖不同知识类型；（3）引入统一评估框架；（4）揭示现有编辑方法的时序局限。局限性包括仅考察参数记忆、知识流人工构建、模型规模有限等。ForgetBench为理解LLM记忆机制和开发鲁棒编辑方法提供重要实验平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该工作直接关联持续学习与知识编辑领域的遗忘评估，对理解模型长期记忆有重要意义。

## 基本信息

- 作者：Ruxi Gu, Zhenliang Zhang, Wei Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-07-29
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.26455v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成参考了PDF检索证据中的Abstract和Introduction片段，并结合启发式草稿进行补全和润色。
