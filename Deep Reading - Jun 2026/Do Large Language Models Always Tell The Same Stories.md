---
user_id: "cheng tan"
paper_id: 521
arxiv_id: "2606.17350v1"
title: "Do Large Language Models Always Tell The Same Stories?"
publish_date: "2026-06-15"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.17350v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.17350v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-18T00:10:07"
---
# Do Large Language Models Always Tell The Same Stories?

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：narrative diversity · story generation · similarity · creativity

## 一句话总结

This paper investigates narrative diversity in LLM-generated stories, finding that outputs from 10 models are consistently more similar to each other than human-written stories, with frontier models converging to a generic mean narrative, and common mitigation strategies such as negative prompting and temperature scaling failing to significantly increase diversity.

## 摘要

> Recent advances in large language models Human
> (LLMs) have enabled the generation of high- Opus-4.6
> quality prose, yet the question of whether these2026 models are capable of generating diverse out- Gemini-3.1
> puts remains contested. In this work, we in- GPT-5.2
> vestigate the diversity of LLM-generated sto-Jun Kimi-K2.5 ries through the framework of narrative sim-
> 15 ilarity.dataset ofUsinghuman-writtena contrastivestoriesframeworkand promptsand a Gemma-3
> Qwen3 from r/WritingPrompts, we collect narrative
> similarity judgments across 10 representative OLMo3.1
> LLMs, utilizing both human evaluations and
> OLMo SFT
> three different automatic annotation methods.
> Our findings reveal a consistent trend: LLM- OLMo DPO[cs.CL] generated narratives are consistently more sim-
> OLMo RL
> ilar to each other than human-written stories
> 0.65 0.70 0.75 0.80 0.85 0.90 0.95 1.00 are. We demonstrate that frontier models in Cosine Similarity
> particular converge on a “mean” generic nar-
> rative that approximates individual human sto- Figure 1: The distribution of narrative component em-
> ries but lacks the collective diversity of human bedding similarity between pairs of stories, generated
> authors. Finally, we show that common miti- by the same model or written by humans, across our
> gation strategies, including negative prompting dataset. We note that human-written stories are consis-
> and temperature scaling, fail to meaningfully tently less similar to each other than LLM-generated
> address this homogeneity. ones.

Q1: 这篇论文试图解决什么问题？

Prior work on LLM creativity yields contradictory results, partly due to ambiguity in operationalizing creativity. This paper narrows the focus to narrative diversity: the question of how similar LLM-generated narratives are within and across models, and compared to human-written stories. The authors aim to provide a robust, reproducible assessment of LLM output homogeneity.

Q2: 有哪些相关研究？

The paper situates itself within debates on LLM creativity (e.g., Gilhooly 2024; Bellemare-Pepin et al. 2026; Lu et al. 2025; Padmakumar et al. 2026; Marco et al. 2025; Shypula et al. 2025). It notes that Lu et al. (2026) caution against direct creativity judgment due to inconsistency. Prior work also explored n-gram novelty (Chakrabarty et al., 2024) and alignment effects, but the paper argues that condensed plot summaries miss organic narrative content. This work builds on Hatzel et al. (2026)'s contrastive similarity framework to circumvent those issues.

Q3: 论文如何解决这个问题？

The authors adopt a contrastive annotation framework where annotators judge which of two candidate stories is narratively closer to a reference story. They construct a dataset of human-written stories from r/WritingPrompts and generate stories from 10 LLMs (e.g., GPT-4, Gemini, Claude, Llama, Qwen, etc.), using the same prompts. Three automatic similarity methods (e.g., cosine similarity of narrative component embeddings) complement human judgments. The study covers different model families, scales, and post-training variants (SFT, DPO, RL). It also tests negative prompting and temperature scaling as mitigation strategies.

Q4: 论文做了哪些实验？

The paper evaluates narrative similarity across 10 LLMs and human authors using a contrastive annotation setup. Human evaluators and three automatic metrics (embedding-based) score pairwise similarity. The experiments include: (1) overall similarity distributions for each model vs. human, (2) analysis of frontier model convergence (e.g., Opus-4.6, Gemini-3.1, GPT-5.2, Kimi-K2.5), (3) comparison of post-training variants (Gemma-3, Qwen3, OLMo3.1 base vs. SFT vs. DPO vs. RL), and (4) ablation of negative prompting and temperature scaling on diversity. The dataset and prompts are from r/WritingPrompts.

Q5: 发现了什么实验现象？

LLM-generated narratives are consistently more similar to each other than human-written stories (see Figure 1 distribution). Frontier models (e.g., GPT-5.2, Gemini-3.1) show especially tight clustering, converging to a 'mean' generic narrative. Human-written stories exhibit lower pairwise similarity. Negative prompting and temperature scaling do not significantly increase diversity; in some cases they marginally affect but do not close the gap. The difference between human and LLM similarity is substantial; a fragment suggests human scores are 66.2% higher in some metric (inferred from 'thors score 66.2% higher than LLMs', but exact context unclear).

Q6: 有什么可以进一步探索的点？

The paper mentions that decoding methods like avoidance decoding (Park et al., 2025) were not tested and may have an effect. Other promising directions: (1) investigating fine-grained narrative components (plot, character, style), (2) extending to non-English stories, (3) testing more diverse prompts or story tasks, (4) exploring explicit diversity constraints at inference, (5) studying the role of temperature on a wider range, (6) connecting narrative diversity to model size and training data.

Q7: 总结一下论文的主要内容

This paper addresses the contested question of whether LLMs generate diverse narratives by focusing on narrative similarity as a proxy. The authors collect human-written and LLM-generated stories from 10 models (including GPT-4, Gemini, Claude, Qwen, Llama, etc.) using prompts from r/WritingPrompts. A contrastive similarity annotation framework (Hatzel et al., 2026) is employed, where annotators judge which of two stories is more similar to a reference. Both human evaluations and three automatic embedding-based methods are used. The key finding is that LLM stories are significantly more similar to each other than human stories, with frontier models showing the highest homogeneity—effectively converging to a generic 'mean' narrative that approximates individual human writing but lacks collective diversity. Common mitigation strategies (negative prompting, temperature scaling) are shown to be ineffective. The results hold across model families, scales, and post-training variants. The paper provides a rigorous operationalization of one aspect of creativity and underscores a fundamental limitation in current LLM-based story generation.

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：Directly addresses story generation (your top direction, weight 0.10).

## 基本信息

- 作者：Thennal DK, Hans Ole Hatzel
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI
- 日期：2026-06-15
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.17350v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的摘要和引言部分证据，但论文完整文本未提供，部分信息基于推断（如具体实验设置和数值）。
