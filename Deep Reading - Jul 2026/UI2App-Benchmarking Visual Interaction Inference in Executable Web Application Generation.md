---
user_id: "cheng tan"
paper_id: 2946
arxiv_id: "2607.06306"
title: "UI2App: Benchmarking Visual Interaction Inference in Executable Web Application Generation"
publish_date: "2026-07-08"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.06306.pdf"
pdf_url: "https://arxiv.org/pdf/2607.06306"
abs_url: "https://arxiv.org/abs/2607.06306"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-09T00:53:55"
---
# UI2App: Benchmarking Visual Interaction Inference in Executable Web Application Generation

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：interaction inference · visual fidelity · ability recover · address gap · align closely · screenshots · iis · ui2app

## 一句话总结

Large language models (LLMs) have demonstrated growing competence in web page generation.

## 摘要

> Large language models (LLMs) have demonstrated growing competence in web page generation. However, existing text-driven approaches rely on complex prompts that impose substantial demands on users and offer limited expressivity for page layout and cross-page visual coherence. Image-driven paradigms, which take UI screenshots as input, align more closely with real development workflows. However, current benchmarks focus primarily on visual fidelity and lack a systematic evaluation of the interaction capabilities in generated artifacts. To address this gap, we introduce UI2App, the first benchmark targeting interaction inference, the ability to recover application behavior from screenshots alone, without any textual or behavioral guidance. UI2App comprises 327 screenshots grouped into 45 state-coherent screenshot sets for runnable multi-route web applications. We design an end-to-end pipeline that evaluates each artifact along four dimensions: executability, navigation reachability, visual fidelity, and interaction inference. The interaction metric (IIS) assesses inferred interactions by functional correctness and state-management complexity, crediting any valid implementation rather than matching a single reference. Experiments on six frontier vision-language models reveal a marked capability mismatch between visual reconstruction and interaction realization: the visual-fidelity leader scores only 7.5 on IIS, ranking fourth and trailing the IIS leader by 5.2x. High-complexity interactions such as cross-page state remain a pervasive bottleneck, with half of the evaluated models scoring exactly zero on this dimension. Overall, the results indicate that inferring complete interaction behavior from static screenshots remains a key challenge for models.

Q1: 这篇论文试图解决什么问题？

Large language models (LLMs) have demonstrated growing competence in web page generation. However, existing text-driven approaches rely on complex prompts that impose substantial demands on users and offer limited expressivity for page layout and cross-page visual…

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：In this paper, we introduced UI2App, the first benchmark to measure interaction inference: recovering application behavior from image-only multi-page screenshots, with no textual…；Each artifact is scored along four dimensions (executability, navigation reachability, visual fidelity, and IIS), with IIS grounded in the interaction taxonomy that tiers…；UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input.。

Q3: 论文如何解决这个问题？

In this paper, we introduced UI2App, the first benchmark to measure interaction inference: recovering application behavior from image-only multi-page screenshots, with no textual… Each artifact is scored along four dimensions (executability, navigation reachability, visual fidelity, and IIS), with IIS grounded in the interaction taxonomy that tiers…

Q4: 论文做了哪些实验？

UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input. \- End-to-end evaluation protocol and IIS taxonomy.

Q5: 发现了什么实验现象？

UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input. \- End-to-end evaluation protocol and IIS taxonomy.

Q6: 有什么可以进一步探索的点？

- A UI2App task supplies only a set of M screenshots (M = 4–14, mean 7.3), each capturing a distinct route or visual state of the target application.
- No action trace or interaction description is provided.
- 优先核对语义命中的全文片段：Abstract。

Q7: 总结一下论文的主要内容

Large language models (LLMs) have demonstrated growing competence in web page generation.

Large language models (LLMs) have demonstrated growing competence in web page generation. However, existing text-driven approaches rely on complex prompts that impose substantial demands on users and offer limited expressivity for page layout and cross-page visual…

In this paper, we introduced UI2App, the first benchmark to measure interaction inference: recovering application behavior from image-only multi-page screenshots, with no textual… Each artifact is scored along four dimensions (executability, navigation reachability, visual fidelity, and IIS), with IIS grounded in the interaction taxonomy that tiers…

UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input. \- End-to-end evaluation protocol and IIS taxonomy.

主要贡献包括：In this paper, we introduced UI2App, the first benchmark to measure interaction inference: recovering application behavior from image-only multi-page screenshots, with no textual…；Each artifact is scored along four dimensions (executability, navigation reachability, visual fidelity, and IIS), with IIS grounded in the interaction taxonomy that tiers…；UI2App is the first benchmark targeting interaction inference rather than specification-following from image-only input.。

需要注意的边界包括：A UI2App task supplies only a set of M screenshots (M = 4–14, mean 7.3), each capturing a distinct route or visual state of the target application.；No action trace or interaction description is provided.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。；这篇论文和你当前画像里的方向有直接重合：生成（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Abstract 部分。

## 基本信息

- 作者：Grace Man Chen, Litao Guo, Yifan Wu, Yiyu Chen, Yenchi Tseng, Sicheng Liu, Yuyu Luo, Ying-Cong Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SE, cs.AI, cs.CV
- 日期：2026-07-08
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2607.06306`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
