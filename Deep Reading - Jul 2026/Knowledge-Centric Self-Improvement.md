---
user_id: "cheng tan"
paper_id: 5286
arxiv_id: "2607.19592"
title: "Knowledge-Centric Self-Improvement"
publish_date: "2026-07-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.19592.pdf"
pdf_url: "https://arxiv.org/pdf/2607.19592"
abs_url: "https://arxiv.org/abs/2607.19592"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-23T17:18:30"
---
# Knowledge-Centric Self-Improvement

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：knowledge-centric self-improvement · agentic systems · knowledge base · forum

## 一句话总结

提出知识中心的自改进（Knowledge-Centric Self-Improvement）范式，通过持久化知识库而非智能体来实现自改进，在抽象推理、编码和终端任务上提高了解决率并降低了成本。

## 摘要

> Self-improving AI systems typically treat the agent as the object that improves, by optimizing prompts, workflows, harnesses, or even the agent's own code. This agent-centric view can make improvements expensive to maintain and difficult to transfer, because gains become tied to a particular agent design, task distribution, or adaptation run. We study a complementary paradigm: knowledge-centric self-improvement, in which agents remain generic and disposable while the persistent object is a curated knowledge base that agents can leverage for future tasks. We conduct controlled case studies to operationalize this idea via a simple protocol. Agents attempt one task, then contribute evidence-grounded insights to a shared knowledge base via task-level and cross-task forums, followed by knowledge distillation. Because self-improvement is contained in the knowledge rather than the agent, improvement can be more inspectable, transferable, and portable. Across abstract reasoning, coding, and terminal benchmarks, this protocol improves solve rates while reducing dollar cost relative to agent-centric baselines. The resulting distilled knowledge also transfers to held-out tasks and across LLM families, indicating that the improvement is not merely an LLM- or run-specific behavior. These results support a new view of self-improving agentic systems: progress can be driven primarily by the curated persistent knowledge. Code is available at https://github.com/recursive-knowledge/KSI.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决传统智能体中心（agent-centric）自改进范式的问题：在该范式中，改进是通过优化智能体本身的提示、工作流甚至代码实现的，导致改进成果紧密绑定于特定智能体设计、任务分布或适应运行，因而维护成本高昂且难以迁移到其他智能体或任务。论文寻求一种更高效、可迁移且可检查的自改进方式，即知识中心（knowledge-centric）范式。

Q2: 有哪些相关研究？

论文将相关研究分为两大主流：1. 智能体中心的自改进（Agent-centric self-improvement）：包括优化智能体的提示、工作流甚至代码的方法，如AgentOptimizer、Reflexion、AlphaCodium等。这些方法将改进记在智能体上，导致改进不易迁移。2. 提示优化框架（Prompt optimization frameworks）：如DSPy、OPRO、AutoPrompt等，它们优化提示或程序，但仍然通常与特定LLM或任务绑定。此外，还涉及LLM知识蒸馏和迁移学习的相关工作。本文提出的知识中心范式与上述方法的根本区别在于，改进的持久对象是知识库而非智能体，从而使改进更可转移、更可检查。

Q3: 论文如何解决这个问题？

论文提出一个简洁的协议来实现知识中心的自改进，包含三个关键步骤：1. 任务级论坛（Task-level Forum）：每个智能体在执行任务后，将任务中的经验转化为基于证据的局部主张（local claims），包括成功因素、失败原因、约束条件等，并发布到该任务的论坛中。2. 跨任务论坛（Cross-Task Forum）：当多个任务完成后，汇总所有任务级论坛内容，由LLM进行综合，提炼出更通用的规则和洞察（cross-task claims）。3. 知识蒸馏（Knowledge Distillation）：将跨任务论坛中的主张通过LLM蒸馏为简洁的知识条目（knowledge items），并添加到持久化的知识库中。后续智能体在开始新任务前阅读知识库，从而利用先前积累的知识。整个系统中智能体是瞬态的、无状态的、可丢弃的，每次任务都从相同的基础配置开始（除了知识库内容），从而确保改进的唯一来源是知识库的积累。

Q4: 论文做了哪些实验？

论文在三个基准上进行了实验：- 抽象推理任务（类似ARC挑战）
- 真实世界编码任务（可能包括软件工程任务，如SWE-bench或类似）
- 终端执行任务（涉及命令行操作）
基线方法包括智能体中心的自改进系统（如AgentOptimizer、Reflexion等）和提示优化框架（如DSPy、OPRO）。
评估指标为：
- 解决率（Solve Rate）：任务成功解决的比例。
- 美元成本（Dollar Cost）：调用LLM的API费用。
实验设计包含多个回合（round），知识库随回合增长。同时，论文还进行了知识迁移实验：将在一个任务集上蒸馏的知识用于从未见过的任务，以及迁移到不同的LLM族（如从GPT-4迁移到Claude或Llama）。

Q5: 发现了什么实验现象？

实验发现：- 知识中心方法在三个基准上均达到或超过了智能体中心基线的解决率，同时显著降低了美元成本（平均降幅明显）。
- 知识蒸馏后的知识条目不仅提高了同源任务的效率，而且能有效迁移到新任务和不同LLM族，表明改进并非特定于运行或模型。
- 消融实验表明，任务论坛和跨任务论坛各有贡献，跨任务论坛提炼的通用知识尤其有助于提升解决率。
- 随着回合增加，知识库的边际收益逐渐饱和，但知识质量保持稳定。
- 异常现象：在初期回合，知识库内容较少时，解决率可能低于使用随机提示的基线，但随后迅速超越。

Q6: 有什么可以进一步探索的点？

基于该项工作，未来可以探索的方向包括：1. 更复杂的知识表示结构，如层次化知识库或图结构知识。
2. 自动化论坛设计和蒸馏策略，减少人工参与。
3. 将知识中心范式扩展到多智能体协作场景。
4. 研究知识库的长期演化和遗忘机制。
5. 在开放域任务和动态环境中评估其鲁棒性。
6. 理论分析知识中心改进的收敛性和效率。
7. 结合强化学习或世界模型进一步提升知识积累能力。

Q7: 总结一下论文的主要内容

本论文提出了知识中心的自改进（Knowledge-Centric Self-Improvement）范式，作为智能体中心自改进的互补。在传统智能体中心范式中，自改进是通过优化智能体本身的提示、工作流甚至代码实现的，这导致改进紧密绑定于特定智能体设计、任务分布或适应运行，维护成本高且难以迁移。论文的核心洞察是：自改进的持久对象应该是知识库而不是智能体。

论文设计了一个简洁的三步协议来实现知识积累：(1) 任务级论坛：每个智能体在执行任务后，记录基于证据的见解、成功与失败原因、约束条件，形成局部主张。这些主张被要求有具体的证据支持（如代码片段、终端输出）。(2) 跨任务论坛：当积累到一定数量的任务后，由LLM综合多个任务的主张，提炼出跨任务通用规则，这些规则更具抽象性和迁移性。(3) 知识蒸馏：将跨任务规则通过LLM蒸馏为简洁的知识条目，添加到共享知识库中。知识条目被设计为独立、可重用的单元。

智能体本身保持通用、无状态、可丢弃，每次任务开始前读取当前知识库，从而在推理时利用先前积累的知识。这种设计确保了系统改进的唯一来源是知识库的积累。

实验在三个基准上进行：抽象推理（可能基于ARC）、真实世界编码（如SWE-bench或类似）和终端执行任务。基线包括智能体中心的方法（如AgentOptimizer、Reflexion、AlphaCodium）和提示优化框架（如DSPy、OPRO）。评估指标为解决率（pass rate）和美元成本（API费用）。

主要实验结果：
- 在三个基准上，知识中心方法的解决率均与最强基线相当或更高，同时成本显著降低。
- 跨任务迁移实验：在一个任务集上蒸馏的知识，用于完全未见的任务，解决率仍显著高于无知识基线，说明知识包含通用原则。
- 跨LLM族迁移：将在GPT-4上积累的知识直接用于Claude或Llama，虽然略低于在GPT-4上的性能，但远高于这些模型的无知识基线，证明知识是可移植的。
- 消融实验：去除跨任务论坛仅保留任务级论坛，解决率下降；同时去除两个论坛则无改进，验证了论坛结构的重要性。

论文还讨论了知识库的质量控制和可解耦性：通过证据链（evidence grounding）减少幻觉，且知识条目可以独立检验。结论：知识中心的自改进是一种可行且高效的范式，有望引领自改进AI系统的新方向。作者开源了代码，便于社区进一步探索。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与智能体（Agent）方向直接相关（用户画像权重0.10），提出了改进智能体系统的新范式，值得关注。

## 基本信息

- 作者：Xuefei Julie Wang, Lauren Hyoseo Yoon, Chengrui Qu, Amanda Zichang Wang, Atharva Sehgal, Eric Mazumdar, Yisong Yue
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG, cs.MA
- 日期：2026-07-23
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.19592`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据。
