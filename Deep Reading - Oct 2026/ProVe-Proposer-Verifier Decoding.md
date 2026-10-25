---
user_id: "cheng tan"
paper_id: 17250
arxiv_id: "2610.27815"
title: "ProVe: Proposer-Verifier Decoding"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.27815.pdf"
pdf_url: "https://arxiv.org/pdf/2610.27815"
abs_url: "https://arxiv.org/abs/2610.27815"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:47:57"
---
# ProVe: Proposer-Verifier Decoding

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：tokens pass · pass verify · speculative decoding · verify tokens · average throughput · block · proposer · one

## 一句话总结

Speculative decoding accelerates autoregressive (AR) language models by letting a small model draft tokens serially and a large model verify them in parallel.

## 摘要

> Speculative decoding accelerates autoregressive (AR) language models by letting a small model draft
> tokens serially and a large model verify them in parallel. We reverse these roles. Starting from a
> standard AR checkpoint, we fine-tune the large model into a proposer that predicts a block of future
> tokens in a single forward pass, then use one additional forward pass to verify the proposed block.
> This gives ProVe, proposer-verifier decoding. The verifier can be a smaller AR model or the proposer
> itself; the latter gives ProVe-lossless, which reproduces the proposer’s greedy AR decoding. Block
> proposals already provide substantial room for acceleration: with a block size of 16, the proposed2026 block has an average AR-matching prefix of 6.6 tokens on GSM8K. Across six benchmarks with a 32B
> proposer, ProVe-lossless reaches 2.5× average AR throughput without training a separate draft head.
> With a small 3B verifier, ProVe reaches 2.7× average throughput at 2.6 accuracy points below AR onOct
> the six-task average, providing a controllable tradeoff between speed and accuracy.
> 20
> [cs.CL] (a) Speculative decoding (b) ProVe (c) ProVe-lossless = greedy AR
> draft k tokens in k passes →verify k tokens in 1 pass propose k tokens in 1 pass →verify k tokens in 1 pass propose k tokens in 1 pass →verify k tokens in 1 pass
> Figure 1. Three ways to spend two model calls per decoding cycle. (a) Speculative decoding: a small model drafts one
> token per pass and the large model verifies the chain in one pass. (b) ProVe: the large proposer writes a proposed block
> of 𝐵tokens in one pass, and a small verifier reads it in one more and commits the accepted prefix. (c) ProVe-lossless: the
> proposer verifies its own proposed block; the committed prefix plus its correction token equals greedy AR decoding,
> token for token.

Q1: 这篇论文试图解决什么问题？

rule commits only 3.0. Lowering the confidence threshold commits more tokens, but also increases the rate of committing tokens that diverge from greedy AR decoding (Section 3.1).

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：ollable speed–accuracy tradeoff: a 7B verifier reaches 2.3× average throughput with mean accuracy within 0.3 points of AR, while a 3B verifier reaches 2.7× at 2.6 points below AR.；Contributions • Building on the block-proposing capability of MARS, we reverse the usual speculative-decoding roles: the large model proposes 𝐵tokens in one pass, and a verifier…；We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard…。

Q3: 论文如何解决这个问题？

ollable speed–accuracy tradeoff: a 7B verifier reaches 2.3× average throughput with mean accuracy within 0.3 points of AR, while a 3B verifier reaches 2.7× at 2.6 points below AR. Contributions • Building on the block-proposing capability of MARS, we reverse the usual speculative-decoding roles: the large model proposes 𝐵tokens in one pass, and a verifier…

Q4: 论文做了哪些实验？

We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard… nt is a control rather than a requirement of ProVe;

Q5: 发现了什么实验现象？

We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard… nt is a control rather than a requirement of ProVe;

Q6: 有什么可以进一步探索的点？

- In this work, we show that block proposals contain substantially more usable content than the proposer’s own confidence suggests: on GSM8K, the proposed block has an average…
- The gap arises because proposer confidence is computed from placeholders and therefore does not see the realized proposed prefix.
- 优先核对语义命中的全文片段：Results / Introduction。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 重点看 Experiments / Results，确认提升来自哪里，以及 baseline 是否公平。
- 最后看 Conclusion / Discussion，判断作者自己如何定义边界与下一步。

Q7: 总结一下论文的主要内容

Speculative decoding accelerates autoregressive (AR) language models by letting a small model draft tokens serially and a large model verify them in parallel.

rule commits only 3.0. Lowering the confidence threshold commits more tokens, but also increases the rate of committing tokens that diverge from greedy AR decoding (Section 3.1).

ollable speed–accuracy tradeoff: a 7B verifier reaches 2.3× average throughput with mean accuracy within 0.3 points of AR, while a 3B verifier reaches 2.7× at 2.6 points below AR. Contributions • Building on the block-proposing capability of MARS, we reverse the usual speculative-decoding roles: the large model proposes 𝐵tokens in one pass, and a verifier…

We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard… nt is a control rather than a requirement of ProVe;

主要贡献包括：ollable speed–accuracy tradeoff: a 7B verifier reaches 2.3× average throughput with mean accuracy within 0.3 points of AR, while a 3B verifier reaches 2.7× at 2.6 points below AR.；Contributions • Building on the block-proposing capability of MARS, we reverse the usual speculative-decoding roles: the large model proposes 𝐵tokens in one pass, and a verifier…；We compare against five decoding baselines: standard autoregressive (AR) decoding, ProVelossless, the original MARS confidence rule, EAGLE-3 (Li et al., 2025a), and standard…。

需要注意的边界包括：In this work, we show that block proposals contain substantially more usable content than the proposer’s own confidence suggests: on GSM8K, the proposed block has an average…；The gap arises because proposer confidence is computed from placeholders and therefore does not see the realized proposed prefix.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Results / Introduction 部分。；这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Results / Introduction 部分。

## 基本信息

- 作者：Ziqi Jin, Weiwen Xu, Jiaxi Li, Yujie Zhong, Qi She, Aixin Sun
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.LG
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.27815`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
