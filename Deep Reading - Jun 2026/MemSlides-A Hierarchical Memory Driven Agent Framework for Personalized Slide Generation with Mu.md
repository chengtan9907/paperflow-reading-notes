---
user_id: "cheng tan"
paper_id: 416
arxiv_id: "2606.17162v1"
title: "MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.17162v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17162v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:06:20"
---
# MemSlides: A Hierarchical Memory Driven Agent Framework for Personalized Slide Generation with Multi-turn Local Revision

> ★★★☆☆ 值得快速浏览 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：hierarchical memory · personalized presentation generation · multi-turn revision · agent framework

## 一句话总结

MemSlides proposes a hierarchical memory framework separating user profile memory, working memory, and tool memory to achieve round-0 persona alignment and reliable multi-turn localized revision for personalized slide generation.

## 摘要

> Personalized presentation generation requires more than conditioning on a current prompt or template: agents must preserve stable user preferences across tasks, retain newly introduced preferences and constraints during multi-turn revision, and carry out local edits reliably. We propose MemSlides, a hierarchical memory framework for personalized presentation agents that separates long-term memory from working memory and further divides long-term memory into user profile memory and tool memory. User profile memory stores intent-conditioned profiles for round-0 personalization, working memory carries active preferences and session constraints across revision rounds, and tool memory stores reusable execution experience for reliable localized editing. MemSlides pairs this memory design with scoped slide-local revision, so targeted updates act on the smallest affected region instead of repeatedly regenerating the full deck. In controlled experiments, user profile memory improves persona-alignment judgments on a multi-persona, multi-intent profile bank, tool-memory injection improves closed-loop modify behavior in diagnostic matched-pair settings, and qualitative cases illustrate working memory's ability to carryover preferences. Taken together, these results suggest that effective personalization in presentation authoring depends on separating persistent user profiles, session-level working memory, and reusable execution experience across generation and localized revision.

Q1: 这篇论文试图解决什么问题？

Existing slide generation systems produce complete decks but lack persistent personalization—they cannot preserve stable user preferences across tasks, retain newly introduced preferences during multi-turn revision, or carry out localized edits without regenerating the whole deck. Current approaches treat personalization as an implicit byproduct of prompting rather than a direct service enabled by memory design.

Q2: 有哪些相关研究？

Prior work on automatic presentation generation (e.g., SlideTailor, DeepPresenter) conditions on reference slides or templates but does not accumulate user profiles. Agent memory research (e.g., MemGPT, Generative Agents) inspires separating long-term and working memory. This work adapts such memory concepts to the domain of slide authoring, additionally introducing tool memory for reliable localized editing.

Q3: 论文如何解决这个问题？

MemSlides comprises three memory components: (1) user profile memory stores round-0 personalization profiles conditioned on user intent; (2) working memory carries active preferences and session constraints across revision rounds; (3) tool memory stores reusable execution experience for reliable localized edits. A scoped slide-local revision mechanism updates the smallest affected region instead of regenerating the full deck, reducing token usage and preserving consistent style.

Q4: 论文做了哪些实验？

Experiments are conducted on a controlled multi-persona, multi-intent profile bank. Personalization is evaluated via persona-alignment judgments (GLM-5, Gemini 3.1 Pro, GPT-5) against SlideTailor and DeepPresenter baselines. Tool-memory effectiveness is tested on nine diagnostic matched-pair modify examples. Qualitative cases illustrate working memory's ability to carry over preferences across rounds. DeepPresenter-style quality metrics check compatibility with general presentation quality.

Q5: 发现了什么实验现象？

User profile memory achieves all-column wins over both baselines on GLM-5 and Gemini 3.1 Pro, and leads on most dimensions with GPT-5 (Content, Visual, Specificity) while DeepPresenter has slightly higher Structure. Tool-memory injection improves closed-loop modify success and verification rates in diagnostic settings. Qualitative cases show working memory retains user-preferred templates and visual styles across multiple revision rounds. Personalization gains do not sacrifice general presentation quality.

Q6: 有什么可以进一步探索的点？

Future work should include broader human studies, randomized edit sets, and stronger safeguards for memory consent, deletion, and sensitive preferences. Extending to real-user deployment and more diverse presentation domains (e.g., academic vs. business) would strengthen generality.

Q7: 总结一下论文的主要内容

This paper identifies the gap that current slide generation systems lack persistent personalization for multi-turn revision. MemSlides addresses this with a hierarchical memory framework: user profile memory (intent-conditioned profiles for round-0 alignment), working memory (active preferences and session constraints across turns), and tool memory (reusable execution knowledge for reliable local edits). The local revision mechanism modifies only the smallest affected slide region, avoiding full regeneration. Experiments on a controlled multi-persona profile bank show that user profile memory improves persona alignment across multiple LLM backends (GLM-5, Gemini 3.1 Pro, GPT-5) compared to SlideTailor and DeepPresenter baselines. Tool memory ablation on diagnostic modify pairs demonstrates higher completion and verification rates. Qualitative cases confirm working memory preserves user preferences across rounds. The results show that separating memory types and enabling localized revision are effective for personalized slide generation. Limitations include controlled rather than real-user settings and proxy edit requests.

## 推荐指数

★★★☆☆（3/5）
- 推荐理由：Directly addresses agent-based personalization and generation, matching user's top directions (agent, generation).

## 基本信息

- 作者：Ye Jin, Yangyang Xu, Jun Zhu, Yibo Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.HC, cs.MA
- 日期：2026-06-15
- 推荐级别：**值得快速浏览**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.17162v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要依赖PDF检索证据（Introduction、Results、Conclusion片段）并结合heuristic_draft润色补全。
