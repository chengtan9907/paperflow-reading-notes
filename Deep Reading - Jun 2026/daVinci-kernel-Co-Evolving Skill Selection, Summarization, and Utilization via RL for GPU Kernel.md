---
user_id: "cheng tan"
paper_id: 418
arxiv_id: "2606.16497v1"
title: "daVinci-kernel: Co-Evolving Skill Selection, Summarization, and Utilization via RL for GPU Kernel Optimization"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.16497v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.16497v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:06:46"
---
# daVinci-kernel: Co-Evolving Skill Selection, Summarization, and Utilization via RL for GPU Kernel Optimization

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gpu kernel optimization · reinforcement learning · skill discovery · multi-agent rl

## 一句话总结

We present daVinci-kernel, a reinforcement learning framework for GPU kernel optimization that treats skill evolution as an integral part of the learning problem, coupling skill discovery with exploitation through a dynamically evolving skill library.

## 摘要

> daVinci-kernel is an RL framework for GPU kernel optimization that jointly trains three agents sharing one LLM backbone: a Skill Selection Agent (retrieves techniques via BM25 and LLM reranking), a Policy Agent (generates multi-turn CUDA/Triton kernels conditioned on skills), and a Skill Summary Agent (distills successful rollouts into reusable skills). Skills are added only after execution-based verification confirms reproducible speedups. The agents are initialized via structured SFT on diversity-filtered data and optimized jointly with multi-turn REINFORCE. On KernelBench, daVinci-kernel-14B achieves 37.2%/70.6%/32.2% on Level 1/2/3 under the Fast1 threshold, outperforming Dr. Kernel-14B. Gains are largest on hard tasks, indicating that evolving skill reuse becomes more important as optimization depth increases.

Q1: 这篇论文试图解决什么问题？

GPU kernel optimization starts from a correct but inefficient implementation and improves it through a sequence of transformations. As the policy improves, the source of further gains shifts: early improvements come from broad transformations correcting obvious inefficiencies, while later gains require subtler, tightly coupled decisions. This creates a fundamental difficulty: knowledge from early stages may become irrelevant or even detrimental later. Thus, a learning system must not only accumulate skills but also re-evaluate their usefulness as the policy evolves, and expose the most relevant skills to future decisions. Existing RL-based kernel optimizers (e.g., Dr. Kernel) lack a mechanism for dynamic skill evolution; they treat the skill library as static or monotonically growing, which fails to adapt to the changing capability frontier of the policy.

Q2: 有哪些相关研究？

Related work includes Dr. Kernel, a prior RL-trained model that uses REINFORCE for kernel optimization but does not incorporate dynamic skill evolution. CUDA-LLM improves kernels using compilation and runtime feedback. GPU Kernel Scientist formulates optimization as repeated proposal-and-verify. These methods either treat skills as static or do not explicitly manage a reusable skill library. Kernel-specific skill libraries have been explored manually, but daVinci-kernel is the first to automate skill co-evolution with policy training. Other lines of work on code generation with RL (e.g., CodeRL) focus on functional correctness rather than efficiency optimization. The proposed framework is also related to experience replay in RL, where past trajectories are reused, but here skills are distilled into reusable textual techniques rather than raw trajectories.

Q3: 论文如何解决这个问题？

daVinci-kernel consists of three agents sharing a single LLM backbone: (1) Skill Selection Agent: given a query kernel, retrieves candidate skills using BM25 for initial retrieval and an LLM-based reranker for final selection; (2) Policy Agent: generates multi-turn CUDA/Triton kernel code, optionally conditioned on selected skills; (3) Skill Summary Agent: after a successful rollout (execution-verified speedup), distills the key optimization steps into a new skill description. Candidate skills are added to the library only after execution-based verification confirms reproducible speedups on a holdout set. All agents are initialized via supervised fine-tuning (SFT) on a diverse, filtered dataset (to prevent skill mode collapse) and then jointly optimized end-to-end with multi-turn REINFORCE, where each agent has its own advantage estimate. The skill library is dynamically updated: new skills are added, and underperforming skills are deprecated based on usage statistics.

Q4: 论文做了哪些实验？

The main evaluation is on KernelBench, a benchmark with three difficulty levels (1, 2, 3) under the Fast1 threshold (single-turn speedup). daVinci-kernel-14B is compared against Dr. Kernel-14B (the strongest prior RL approach) and other baselines (e.g., direct LLM generation). Results: daVinci-kernel achieves 37.2% on Level 1, 70.6% on Level 2, and 32.2% on Level 3, outperforming Dr. Kernel-14B by up to 46% on Level 3. Comprehensive ablations study: (a) removing inference-time skill injection causes a large performance drop, especially on hard tasks; (b) training without joint selection and summarization weakens high-threshold speedups; (c) replacing LLM-based selection with BM25-only retrieval can still find shallow speedups but fails on demanding thresholds; (d) diversity-filtered SFT data mitigates skill mode collapse and improves final performance.

Q5: 发现了什么实验现象？

Key observations: (1) Skill injection at inference time is crucial; without it, performance drops significantly, especially on Level 3. (2) BM25-only retrieval helps for easy tasks but cannot reliably support high-threshold speedups; LLM-based reranking is necessary for difficult optimizations. (3) Joint training of all three agents is essential; removing either the selection or summary agent weakens high-threshold performance. (4) Diversity filtering of SFT data prevents the policy from collapsing to a narrow set of skills and improves generalization. (5) The skill library evolves such that early skills (e.g., constant-fill) are replaced by more sophisticated composite skills as training progresses. (6) On a case study (FlashAttention backward kernel), daVinci-kernel discovers a non-trivial tiling strategy that Dr. Kernel misses, achieving 2.0× speedup at Turn 1.

Q6: 有什么可以进一步探索的点？

Possible future work includes: (1) extending the framework to other hardware targets (e.g., NPU, TPU) and programming models; (2) scaling the LLM backbone to larger sizes to test if skill evolution benefits more from model capacity; (3) automatic management of skill library size (e.g., removal of obsolete skills); (4) transfer of skills across different kernel families; (5) incorporating multi-turn execution feedback beyond verification (e.g., profiling data); (6) applying the co-evolution paradigm to other code optimization tasks (e.g., compiler passes, query optimization).

Q7: 总结一下论文的主要内容

This paper introduces daVinci-kernel, a reinforcement learning framework for GPU kernel optimization that addresses the challenge of maintaining and evolving optimization knowledge as the model's capability improves. The key insight is that as the policy becomes more skilled, the nature of required optimizations shifts, making static skill libraries ineffective. daVinci-kernel co-evolves skill selection, summarization, and utilization through three jointly trained agents sharing an LLM backbone: a Skill Selection Agent that retrieves relevant techniques using BM25 and LLM reranking, a Policy Agent that generates multi-turn CUDA/Triton kernels conditioned on selected skills, and a Skill Summary Agent that distills successful rollouts into reusable skills. Skills are added only after execution-based verification ensures reproducible speedups. The agents are initialized via diverse SFT and optimized with multi-turn REINFORCE. On KernelBench, daVinci-kernel-14B achieves state-of-the-art results: 37.2% on Level 1, 70.6% on Level 2, and 32.2% on Level 3 under the Fast1 threshold, outperforming the prior best RL approach by up to 46% on Level 3. Ablation studies confirm the necessity of each component: inference-time skill injection, joint multi-agent training, LLM-based selection, and diversity-filtered SFT. The framework demonstrates that evolving skill reuse is crucial for deep kernel optimizations, and the closed-loop between skill discovery and exploitation enables continuous improvement.

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：This paper is highly relevant to the agent direction (weight 0.10) as it proposes a multi-agent RL framework for code optimization.

## 基本信息

- 作者：Dayuan Fu, Mohan Jiang, Tongyu Wang, Dian Yang, Jiarui Hu, Liming Liu, Jinlong Hou, Pengfei Li
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-06-15
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.16497v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的abstract、introduction、conclusion等片段，并补充了heuristic_draft中的信息。
