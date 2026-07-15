---
user_id: "cheng tan"
paper_id: 3816
arxiv_id: "2607.11357v1"
title: "OpsMem: Dual-Memory Reasoning with Cross-Memory Resonance for Failure Diagnosis"
publish_date: "2026-07-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.11357v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.11357v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-15T00:49:25"
---
# OpsMem: Dual-Memory Reasoning with Cross-Memory Resonance for Failure Diagnosis

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：failure diagnosis · large language model · multi-agent system · dual-memory

## 一句话总结

OpsMem提出一种双记忆框架，通过跨记忆共鸣协调故障诊断中的短期状态与长期经验，在华为微服务数据集上将诊断匹配率提升最多46.88%。

## 摘要

> Failure diagnosis in modern software systems requires iterative evidence acquisition and hypothesis reasoning guided by operational experience. Existing LLM-based methods improve diagnosis through agentic reasoning or knowledge augmentation, but they often lack a mechanism to coordinate the evolving diagnostic state with operational experience during iterative diagnosis. We propose OpsMem, a dual-memory framework that maintains a short-term memory for the current diagnostic state and a long-term memory for reusable operational experience. OpsMem uses cross-memory resonance to activate state-relevant long-term memory, conditions multi-agent diagnosis on the short-term and activated long-term memories, and consolidates reusable experience from solved incidents back into long-term memory. Experiments on a real-world Huawei microservice failure diagnosis dataset show that OpsMem outperforms representative agentic-reasoning and knowledge-augmented baselines, improving Match and Relevant by up to 46.88% and 18.39% over the strongest baseline, respectively.

Q1: 这篇论文试图解决什么问题？

现代软件系统故障诊断面临两大挑战：一是诊断过程需要不断迭代，每次迭代产生的证据和假设需要与已有的操作经验对齐；二是现有的基于LLM的方法要么采用智能体推理（如ReAct、Reflexion）但经验复用能力弱，要么采用知识增强（如知识图谱、知识库）但难以动态适应诊断状态的变化。具体而言，当诊断状态变化时，如何动态激活最相关的操作经验成为一个关键瓶颈。OpsMem旨在解决这一协调问题，设计一个能同时管理瞬态诊断状态和持久操作记忆的框架。

Q2: 有哪些相关研究？

相关研究主要分为两类：智能体推理方法和知识增强方法。智能体推理方法如ReAct（Yao et al., 2023）和Reflexion（Shinn et al., 2023）让LLM通过行动-观察循环进行诊断，但缺乏对历史经验的系统复用。知识增强方法如FlowXpert（构造混合知识库）和数据库知识图谱方法（Shu et al., 2025），它们将领域知识编码为图谱等形式，但无法在诊断过程中根据当前状态动态调整。此外，RAG方法从外部知识库检索信息，但检索粒度固定，不能精细匹配诊断阶段。OpsMem的双记忆框架同时结合了智能体的灵活性和知识的结构化，并通过跨记忆共鸣实现动态激活。

Q3: 论文如何解决这个问题？

OpsMem的核心是双记忆架构和跨记忆共鸣机制。短期记忆（STM）采用图数据结构，表示当前故障的诊断状态，包括已收集的证据、生成的假设和推理路径。长期记忆（LTM）以知识图谱形式组织，每个节点代表一个操作经验模式（如某种故障的根因和修复步骤），边表示模式间的关联。跨记忆共鸣（CMR）包含三个步骤：(1) 信号耦合：计算STM中每个节点与LTM中节点之间的关联强度，形成耦合信号；(2) 模式激活：根据耦合信号选择LTM中激活值最高的子图；(3) 记忆传播：将激活的LTM子图传递给后续诊断模块。然后，多智能体系统（包括证据收集智能体、假设生成智能体和验证智能体）在STM和激活LTM的引导下协作生成诊断结论。诊断完成后，经验巩固模块通过另一个多智能体过程，将成功诊断案例转化为新的操作经验模式并更新LTM，实现持续学习。

Q4: 论文做了哪些实验？

实验基于华为真实微服务故障数据集，包含多种微服务故障类型。对比基线包括：(1) 纯LLM问答；(2) ReAct智能体；(3) Reflexion智能体；(4) RAG增强；(5) 知识图谱增强方法。评估指标为Match（诊断结果与真实故障的匹配度）和Relevant（诊断报告的相关性）。设计了三个研究问题：RQ1整体有效性对比；RQ2组件贡献（消融STM、LTM、CMR、多智能体、巩固模块等）；RQ3随时间改进能力（模拟LTM随诊断案例增加的性能变化）。实验设置中，LTM初始构建通过访谈、问卷和文档，由GPT协助提取模式。

Q5: 发现了什么实验现象？

RQ1结果显示OpsMem在Match和Relevant上均优于所有基线，Match最高提升46.88%（相比最强基线），Relevant提升18.39%。消融实验（RQ2）表明：去除跨记忆共鸣后性能下降最大，证实了动态激活的重要性；去除短期记忆或长期记忆也导致显著下降，表明双记忆互补；去除多智能体协调后，诊断一致性变差。RQ3显示随着LTM中经验积累，OpsMem的性能呈上升趋势，表明其持续学习能力有效。未观察到明显的负结果或反直觉现象。需要注意的是，实验仅针对微服务故障领域，其他领域的泛化性未知。

Q6: 有什么可以进一步探索的点？

可以进一步探索的方向包括：(1) 将OpsMem扩展到更广泛的运维领域（如云基础设施、网络故障）；(2) 减少对人工构建LTM的依赖，研究自动经验抽取方法；(3) 优化跨记忆共鸣的计算效率，支持更大规模记忆；(4) 结合更先进的LLM推理策略（如思维链、树搜索）来增强多智能体协作；(5) 探索记忆遗忘机制，避免LTM膨胀；(6) 在更多样化的故障数据集上验证鲁棒性。

Q7: 总结一下论文的主要内容

本文提出OpsMem，一种双记忆推理框架用于故障诊断。论文首先指出现有基于LLM的诊断方法在经验复用和状态协调上的不足，然后设计了一个包含短期记忆（STM）和长期记忆（LTM）的解决方案。STM追踪当前诊断的进展（证据、假设等），LTM以知识图谱形式存储可复用的操作经验。通过跨记忆共鸣（CMR），在诊断过程中动态激活与当前状态最相关的LTM子图，从而引导多智能体系统进行有效诊断。诊断结束后，经验巩固模块将成功的案例转化为新的模式并更新LTM。实验使用华为真实微服务故障数据集，对比了多种基线，结果显示OpsMem在诊断匹配度和相关性上大幅领先，消融实验验证了各组件的必要性，时间改进实验证明了其持续学习能力。论文的主要贡献在于提出了一种新颖的双记忆视角，将瞬态诊断状态与持久经验分离并动态耦合，为运维智能化提供了新思路。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：本论文与智能体（Agent）研究方向直接相关，提出了多智能体协作诊断框架。

## 基本信息

- 作者：Yongqian Sun, Rongchen Gao, Yu Luo, Wenwei Gu, Shenglin Zhang, Qingyi Guo, Qiuai Fu, Yaoliang Wu, Dan Pei
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.SE
- 日期：2026-07-13
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.11357v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索证据，包括背景、方法、结果和局限相关片段，并结合启发式草稿进行补充。
