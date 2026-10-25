---
user_id: "cheng tan"
paper_id: 16232
arxiv_id: "2610.18535v1"
title: "Meta-Video: Efficient Standardized Evaluation for Video Understanding"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18535v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18535v1"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:13:19"
---
# Meta-Video: Efficient Standardized Evaluation for Video Understanding

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：video understanding · benchmark samples · qwen2 vl-7b · standardized evaluation · claim improvements · benchmarks · across · meta-video

## 一句话总结

Oct Video understanding benchmarks are the primary yardstick for measuring 13 progressto claim improvements.in video-languageThemodeling,reported benchmarkand a…

## 摘要

> Oct Video understanding benchmarks are the primary yardstick for measuring
> 13 progressto claim improvements.in video-languageThemodeling,reported benchmarkand a growingscoresbodyin oftheseworkworks,relieshowever,on them
> are almost always produced by frameworks (e.g., lmms eval) used as black boxes:
> certain evaluation choices like frame sampling, preprocessing, and input format-
> ting are inherited from preset defaults and seldom inspected carefully. We show
> these unexamined choices are a major source of variance. Even for the same model
> checkpoint, evaluation configuration alone can shift accuracy by up to 19.5pp.
> Hence, it is unclear whether reported gains reflect genuine improvements in ca-[cs.CV] pability or differences in evaluation setup. Moreover, many benchmark samples
> offer little evidence of visual capability, since they can be answered without the
> video, and the full suites are expensive to evaluate. We address these issues in two
> parts. First, we curate existing suites for discriminability and efficiency, producing
> Meta-Video, a balanced 1,000-question benchmark over 791 videos spanning di-
> verse tasks and durations, which largely preserves model rankings, at 5× inference
> speedup for a full-frame count sweep experiment (4-128 frames). Second, we es-
> tablish a standardized evaluation protocol by sweeping the configuration space to
> isolate choices that measurably affect results, distill them into best practices and
> reporting standards, and release reproducible reference scores across five mod-
> els and seven benchmarks. We hope that the proposed evaluation framework and
> the Meta-Video benchmark will support video understanding research by provid-
> ing a reproducible and efficient foundation to measure progress in a reliable way.
> Project page: https://soumyasj.github.io/meta-video/arXiv:2610.18535v1 1 Introduction
> Benchmarks measure progress in video understanding, with researchers often using benchmark
> scores to claim improvements over prior methods or base models. Such claims assume that scores for
> the same checkpoint on the same benchmark are fairly comparable across evaluations and papers. We
> find that they are not. Table 1 compares scores reported in recent papers for two widely used check-
> points, Qwen2.5-VL-7B [5] and Qwen3-VL-8B-Instruct [4], across 11 video benchmarks. Several
> methods report gains over the Qwen2.5-VL-7B baseline that fall below our reproduction of the same
> baseline. When we evaluate these methods under identical settings, they don’t improve consistently
> over the baseline across varying frame counts. We see that reported performance varies substantially
> across papers by up to 19.5pp on VideoMME on the same checkpoint. These differences far ex-
> ceed the 2–5pp gains commonly showcased by these same papers over the base model. This makes
> it hard to tell whether an apparent gain claimed in these prior works, stems from a real capability
> improvement in the model, a difference in evaluation configuration, or both.
> Preprint.
> Table 1: Performance reported for the Qwen2.5-VL-7B and Qwen3-VL-8B backbone. across
> general video understanding and video reasoning benchmarks. Max ∆denotes the range of scores
> for each benchmark, differing by up to 19.5pp despite identical model weights. Ours (reproduced)
> are reported for 128 frames setting.
> General Video Understanding Video Reasoning
> Model VideoMME TempCompass MVBench LongVideoBench LVBench VideoMMMU MMVUmcq MMVUval VSIBench SciVideoBench VideoMathQA Minerva
> Base model: Qwen2.5-VL-7B
> Qwen2.5-VL-7B [5] 65.1 71.7 69.6 56.0 45.3 47.4 – 50.1 – – – –
> VideoRFT [59] 52.8-12.3 69.2-2.5 59.4-10.2 56.0 45.3 47.4 61.3 – 31.8 – – –
> Videochat-R1.5 [67] 64.4-0.7 – 67.4-2.2 – – 46.7-0.7 – – – – – –
> Open-o3-video [45] 62.4-2.7 – – – – 51.2+3.8 – – – – – –
> Video-R2 [42] 66.1+1.0 74.2+2.5 68.1-1.5 61.3+5.3 – 50.3+2.9 67.4 – 37.7 16.4 26.7 35.3
> Viss-R1 [17] 56.6-8.5 72.6+0.9 59.0-10.6 – – 48.1+0.7 60.0 – 30.1 – – –
> Rethinking CoT [77] 55.5-9.6 – 63.2-6.4 52.5-3.5 34.2-11.1 – 63.4 – – – – –
> STORM [34] 55.4-9.7 69.9-1.8 60.5-9.1 – – – – – – – – –
> CoVER [60] 63.1-2.0 – – 56.0 42.0-3.3 – – – – – – –
> Video-VER [39] 59.2-5.9 73.7+2.0 63.6-6.0 – – 47.3-0.1 64.2 – 32.3 – – –
> Video-TwG [7] 46.6-18.5 – – – – 44.4-3.0 – – – – – –
> SDRL [58] 56.5-8.6 72.5+0.8 63.3-6.3 – – 48.4+1.0 65.4 – 29.1 – – –
> Video-ToC [53] 59.6-5.5 72.9+1.2 59.2-10.4 – – 50.4+3.0 60.0 – 31.4 – – –
> VidBridge-R1 [9] 59.4-5.7 – 58.7-10.9 50.6-5.4 – – – – – – – –
> TW-GRPO [12] 56.5-8.6 72.5+0.8 63.3-6.3 – – – 65.4 – – – – –
> VideoAuto-R1 [35] 66.0+0.9 – 67.1-2.5 60.9+4.9 – 52.7+5.3 – – – – – –
> MOSS-ChatV [54] 59.7-5.4 72.2+0.5 67.1-2.5 – – 48.1+0.7 – – – – – –
> STRIVE [3] 57.8-7.3 72.8+1.1 – – – 48.6+1.2 – – 34.3 – – –
> RLER [71] 65.1 69.2-2.5 67.5-2.1 56.0 42.0-3.3 47.4 – – 37.4 – – –
> VideoKR [22] 65.1 – 66.3-3.3 60.9+4.9 – 51.1+3.7 – 55.7 – 28.1 – –
> AdaThinkV [55] 66.0+0.9 – 67.1-2.5 60.9+4.9 – 54.7+7.3 66.2 – – 27.9 29.8 –
> Max ∆ 19.5 5.0 10.9 10.7 11.1 10.3 7.4 5.6 8.6 11.7 3.1 0.0
> Ours (Reproduced) 65.6+0.5 73.4+1.7 68.0-1.6 60.2+4.2 42.1-3.2 38.8-8.6 64.6 51.9+1.8 38.3 27.9 26.9 35.3
> Base model: Qwen3-VL-8B-Instruct
> Qwen3-VL-8B-Instruct [4] 71.4 – 68.7 – 58.0 65.3 – 58.7 – – – –
> LLaVA-OneVision-2 [2] 71.4 74.3 69.0+0.3 68.0 58.0 – – – – – – –
> VSeek [24] 63.7-7.7 – – 54.5 38.7-19.3 – – – – – – –
> OneThinker [20] 63.3-8.1 – – 61.7 – 63.3-2.0 – – – – – –
> VideoKR [22] 68.2-3.2 – 67.9-0.8 61.6 – 61.8-3.5 – 59.6 – 33.4 – –
> InternVideo3 [68] 71.4 – – 62.4 58.0 – – – – – – –
> AdaThinkV [55] 72.5+1.1 – 69.4+0.7 67.6 – 61.0-4.3 69.9 – – 33.1 30.7 –
> Max ∆ 9.2 – 1.5 13.5 19.3 4.3 – 0.9 – 0.3 – –
> Ours (Reproduced) 69.2-2.2 74.2 69.2+0.5 63.6 46.5-11.5 56.6-8.7 69.1 58.7 59.7 34.5 31.6 36.0
> This shows that video evaluation can be especially sensitive to eval configuration. For example,
> Frame sampling determines which events are visible to a model. Which Video decoder is used,
> how Number of frames and Frame resolution are set determine the detail and temporal structure
> visible to the model. Further, the Input modality (input a video file or sequence of frames) and
> text Prompt can also affect performance. Yet, papers use evaluation frameworks like lmms eval
> as black boxes, leaving such details underspecified. Additionally, the quality of benchmark samples
> can affect the value and cost of evaluation. Some benchmark samples can be answered without even
> seeing the video. Further, because model rankings can differ across benchmarks, robust comparisons
> demand aggregating evaluations on multiple datasets, with very large inference costs. As such, a
> reliable evaluation supporting fast model development calls for 1 a clean set of benchmark samples,
> 2 low evaluation cost while preserving ranking, and 3 a standardized evaluation protocol.
> To address 1 and 2 , we introduce Meta-Video (Sec. 3), a compact video understanding bench-
> mark of 1,000 questions over 791 videos, balanced across source datasets and spanning diverse tasks
> and durations. We construct it by filtering out low-quality samples to further improve evaluation fi-
> delity and computational efficiency [28, 23]. Specifically, we remove questions solvable without
> the video, since these cannot measure video understanding. Meta-Video largely preserves model
> rankings over its source benchmarks, separates models more sharply, and requires 5× less infer-
> ence compute than the reference benchmarks for a full sweep over frame counts, which is the most
> expensive evaluation setting (Table 2(b)). To tackle 3 , we identify key parameters in video evalu-
> ation pipelines and systematically measure their effects on performance (Sec. 4). We find that input
> representation and inference backend substantially change scores. In particular, switching between
> vLLM and Hugging Face while keeping their respective default settings can change performance by
> more than 5pp, with gains decreasing once the defaults are fixed. Further, we show that extracted
> .jpg frames yield higher scores when few frames are sampled, while .mp4 input performs better
> with larger frame budgets. We observe that how many frames and which frames (frame sampling)
> the model sees is one of the largest, most consistent factors influencing performance. Our system-
> atic evaluation identifies the major impact factors and proposes a concise way to compile them in
> the form of evaluation cards to allow for a better, transparent, and reproducible practice in video
> understanding benchmarking.
> 2
> Together, our contributions are: (1) We introduce Meta-Video, a compact, balanced benchmark that
> preserves ranking trends across its source benchmarks (reliability), uses one-fifth of the inference
> compute for a full frame-count sweep (efficiency), and makes differences between models clearer
> (discriminability). (2) We examine the current state of video understanding evaluation and show that
> variation in often unspecified evaluation protocols can explain substantial discrepancies in reported
> scores. (3) We release a revised evaluation suite and an extensive set of reference configurations
> to improve reproducibility and support future comparisons. We hope our findings and standardized
> evaluation protocols offer a transparent and less costly way to evaluate video understanding, in a
> reliable manner.

Q1: 这篇论文试图解决什么问题？

ier inputs each modality can attend to under causal attention, resulting in prompt patterns such as T-V-T or V-T-V. We evaluate these choices in Section 5.2.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4;；.jpg with vt);；The Qwen models gain most between 4 and 32 frames.。

Q3: 论文如何解决这个问题？

Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4; .jpg with vt);

Q4: 论文做了哪些实验？

The Qwen models gain most between 4 and 32 frames. InternVL3.5-8B gains only 0.2 points from 64 to 128 frames, while InternVL3.5-4B peaks at 45.5 with 32 frames and falls to 43.9 at 128.

Q5: 发现了什么实验现象？

The Qwen models gain most between 4 and 32 frames. InternVL3.5-8B gains only 0.2 points from 64 to 128 frames, while InternVL3.5-4B peaks at 45.5 with 32 frames and falls to 43.9 at 128.

Q6: 有什么可以进一步探索的点？

- Vishaal Udandarao was supported by a Google PhD Fellowship in Machine Intelligence.
- The authors gratefully acknowledge the Gauss Centre for Supercomputing e.V.
- We propose Meta-Video, a compact video understanding benchmark that is an efficient and reliable version of the source benchmarks.
- 优先核对语义命中的全文片段：Abstract / Method。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 再看 Method / Approach，把核心模块、训练目标和输入输出关系理清。
- 重点看 Experiments / Results，确认提升来自哪里，以及 baseline 是否公平。

Q7: 总结一下论文的主要内容

Oct Video understanding benchmarks are the primary yardstick for measuring 13 progressto claim improvements.in video-languageThemodeling,reported benchmarkand a…

ier inputs each modality can attend to under causal attention, resulting in prompt patterns such as T-V-T or V-T-V. We evaluate these choices in Section 5.2.

Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4; .jpg with vt);

The Qwen models gain most between 4 and 32 frames. InternVL3.5-8B gains only 0.2 points from 64 to 128 frames, while InternVL3.5-4B peaks at 45.5 with 32 frames and falls to 43.9 at 128.

主要贡献包括：Qwen2.5-VL-7B Datasets Meta-V (short / medium / long) RUNTIME Inference backend HF Batch size 1 (.mp4;；.jpg with vt);；The Qwen models gain most between 4 and 32 frames.。

需要注意的边界包括：Vishaal Udandarao was supported by a Google PhD Fellowship in Machine Intelligence.；The authors gratefully acknowledge the Gauss Centre for Supercomputing e.V.；We propose Meta-Video, a compact video understanding benchmark that is an efficient and reliable version of the source benchmarks.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Abstract / Method 部分。；它不一定和你当前最核心的 智能体 完全同题，但方法设计和评测组织值得借鉴。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Abstract / Method 部分。

## 基本信息

- 作者：Soumya Shamarao Jahagirdar, Anna Kukleva, Vishaal Udandarao, Samuel Thomas, Brian Kingsbury, Rogerio Feris, James R. Glass. Ameya Prabhu, Hilde Kuehne
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.18535v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
