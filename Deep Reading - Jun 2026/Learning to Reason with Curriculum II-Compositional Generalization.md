---
user_id: "cheng tan"
paper_id: 589
arxiv_id: "2606.27721"
title: "Learning to Reason with Curriculum II: Compositional Generalization"
publish_date: "2026-06-29"
pdf_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.27721.pdf"
pdf_url: "https://arxiv.org/pdf/2606.27721"
abs_url: "https://arxiv.org/abs/2606.27721"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-29T15:25:34"
---
# Learning to Reason with Curriculum II: Compositional Generalization

> ★★☆☆☆ 按需阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：compositional generalization · sequence length · setting inspired · where learner · ability solve · learning · curriculum · computation

## 一句话总结

We study this question through the canonical problem of learning to simulate semiautomata (predicting the outcome of $T$ steps of sequential computation), a model that captures…

## 摘要

> Compositional generalization, the ability to solve complex problems by combining solutions to simpler sub-problems, is a fundamental capability of both natural and artificial intelligence, and a key mechanism underlying chain-of-thought reasoning. However, the theoretical underpinnings of compositional generalization remain poorly understood: when and why does decomposing a problem into parts yield more efficient learning than solving it directly? We study this question through the canonical problem of learning to simulate semiautomata (predicting the outcome of $T$ steps of sequential computation), a model that captures state tracking, regular language recognition, and modular arithmetic. We show that an autocurriculum-based approach building on Part I of this series, recursively decomposing longer sequences into shorter sub-problems, learning to solve them, and composing the solutions, achieves dramatically better statistical complexity than direct methods. (i) For a setting inspired by supervised fine-tuning (SFT) where the learner receives interactive feedback on intermediate states of the computation, curriculum facilitates learning from only $2^{\mathcal{O}(\sqrt{\log T})}$ tokens of supervision; i.e., subpolynomial in the sequence length $T$, overcoming the $Ω(T)$ token barrier required by direct simulation. (ii) For a setting inspired by reinforcement learning with verifiable rewards (RLVR), where the learner improves a pre-trained reference model using an outcome verifier, we show that curriculum reduces the requirement on the reference model from coverage at the full sequence length $T$ to coverage at a shorter block length $B \ll T$, an exponentially weaker condition.

Q1: 这篇论文试图解决什么问题？

Jun Compositional generalization—the ability to solve complex problems by combining solutions to simpler sub-problems—is a fundamental capability of both natural and artificial… However, the theoretical underpinnings of compositional generalization remain poorly understood: when and why does decomposing a problem into parts yield more efficient learning…

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：This work asks how curriculum and composition let a learner solve problems far beyond the reach of its base capabilities—a mechanism widely credited for the success of modern…；Adopting semiautomaton simulation as a testbed, our results show that composition combined with curriculum can achieve superpolynomial reductions in supervision and computational…；There, we show that one can use a weak reference model and outcome verifier to “simulate” iCoT, allowing the curriculum and composition machinery we develop here to carry over…。

Q3: 论文如何解决这个问题？

This work asks how curriculum and composition let a learner solve problems far beyond the reach of its base capabilities—a mechanism widely credited for the success of modern… Adopting semiautomaton simulation as a testbed, our results show that composition combined with curriculum can achieve superpolynomial reductions in supervision and computational…

Q4: 论文做了哪些实验？

There, we show that one can use a weak reference model and outcome verifier to “simulate” iCoT, allowing the curriculum and composition machinery we develop here to carry over… hought takes Ω(𝑡) time.

Q5: 发现了什么实验现象？

There, we show that one can use a weak reference model and outcome verifier to “simulate” iCoT, allowing the curriculum and composition machinery we develop here to carry over… hought takes Ω(𝑡) time.

Q6: 有什么可以进一步探索的点？

- However, our results also require that the model we train is Markovian, which rules out classes like Transformers that can attend to the full history.
- However, in many natural settings, including language model reasoning, it is natural to consider stochastic transition functions 𝜋: 𝑆× Σ →Δ𝑆.
- We close by discussing the limitations of our results, followed by several concrete technical questions they leave open.
- 优先核对语义命中的全文片段：Introduction。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 最后看 Conclusion / Discussion，判断作者自己如何定义边界与下一步。

Q7: 总结一下论文的主要内容

We study this question through the canonical problem of learning to simulate semiautomata (predicting the outcome of $T$ steps of sequential computation), a model that captures…

Jun Compositional generalization—the ability to solve complex problems by combining solutions to simpler sub-problems—is a fundamental capability of both natural and artificial… However, the theoretical underpinnings of compositional generalization remain poorly understood: when and why does decomposing a problem into parts yield more efficient learning…

This work asks how curriculum and composition let a learner solve problems far beyond the reach of its base capabilities—a mechanism widely credited for the success of modern… Adopting semiautomaton simulation as a testbed, our results show that composition combined with curriculum can achieve superpolynomial reductions in supervision and computational…

There, we show that one can use a weak reference model and outcome verifier to “simulate” iCoT, allowing the curriculum and composition machinery we develop here to carry over… hought takes Ω(𝑡) time.

主要贡献包括：This work asks how curriculum and composition let a learner solve problems far beyond the reach of its base capabilities—a mechanism widely credited for the success of modern…；Adopting semiautomaton simulation as a testbed, our results show that composition combined with curriculum can achieve superpolynomial reductions in supervision and computational…；There, we show that one can use a weak reference model and outcome verifier to “simulate” iCoT, allowing the curriculum and composition machinery we develop here to carry over…。

需要注意的边界包括：However, our results also require that the model we train is Markovian, which rules out classes like Transformers that can attend to the full history.；However, in many natural settings, including language model reasoning, it is natural to consider stochastic transition functions 𝜋: 𝑆× Σ →Δ𝑆.；We close by discussing the limitations of our results, followed by several concrete technical questions they leave open.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Introduction 部分。；它不一定和你当前最核心的 智能体 完全同题，但方法设计和评测组织值得借鉴。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Introduction 部分。

## 基本信息

- 作者：Nived Rajaraman, Audrey Huang, Miroslav Dudik, Robert Schapire, Dylan Foster, Akshay Krishnamurthy
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG
- 日期：2026-06-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2606.27721`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
