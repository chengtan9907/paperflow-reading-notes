---
user_id: "cheng tan"
paper_id: 10495
arxiv_id: "2609.02998v1"
title: "Verify Before You Distill: Prompt-Level Teacher Gating for On-Policy Distillation"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02998v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02998v1"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:38:17"
---
# Verify Before You Distill: Prompt-Level Teacher Gating for On-Policy Distillation

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：on-policy distillation · each prompt · vanilla opd · teacher · reliability · tgopd · dense · supervision

## 一句话总结

We introduce Teacher-Gated On-Policy Distillation (TGOPD), built on the principle that teacher reliability should be verified at the prompt level before dense supervision is…

## 摘要

> On-policy distillation (OPD) accelerates post-training by providing dense token-level supervision from a frozen teacher on the student's own rollouts. Vanilla OPD applies this supervision uniformly across prompts, without checking whether the teacher is reliable for each prompt. Because reverse KL is mode-seeking, a confidently wrong teacher can induce a strong yet misleading update. Distributional proxies, such as entropy or teacher-student likelihood agreement, measure uncertainty or agreement but do not directly verify outcome correctness. We introduce Teacher-Gated On-Policy Distillation (TGOPD), built on the principle that teacher reliability should be verified at the prompt level before dense supervision is admitted. TGOPD estimates reliability from a small set of verifier-scored teacher probes and routes each prompt exclusively to dense OPD when the reliability check passes or to verifier-grounded GRPO otherwise. Across 4B and 35B students in mathematics, code, and instruction following, TGOPD outperforms Vanilla OPD in all six single-domain settings and achieves higher seven-benchmark averages at both scales under multi-domain training. By using otherwise-idle teacher capacity for reliability estimation, TGOPD also reduces teacher-side compute waste in asynchronous OPD, increasing teacher-node GPU utilization from 9.8% to 78.9% in the measured 4B single-domain run.

Q1: 这篇论文试图解决什么问题？

AllSpark Team On-policy distillation (OPD) accelerates post-training by providing dense token-level supervision from a frozen teacher on the student's own rollouts. Vanilla OPD applies this supervision uniformly across prompts, without checking whether the teacher is reliable for each prompt.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：every other distillation method causes negative transfer on LiveCodeBench: the student scores below the untrained base model (OPD -0.8, TrOPD -2.5, RG-OPD -3.5, RLSD-style -4.1).；Yet TGOPD is the only method that achieves positive transfer (+3.0 over base) and, in fact, surpasses the teacher itself (+1.3 LCB, +1.1 OJBench).；TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two supervision regimes.。

Q3: 论文如何解决这个问题？

every other distillation method causes negative transfer on LiveCodeBench: the student scores below the untrained base model (OPD -0.8, TrOPD -2.5, RG-OPD -3.5, RLSD-style -4.1). Yet TGOPD is the only method that achieves positive transfer (+3.0 over base) and, in fact, surpasses the teacher itself (+1.3 LCB, +1.1 OJBench).

Q4: 论文做了哪些实验？

TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two supervision regimes. A verifier audits the teacher on the current prompt.

Q5: 发现了什么实验现象？

TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two supervision regimes. A verifier audits the teacher on the current prompt.

Q6: 有什么可以进一步探索的点？

- It is therefore trajectory-level rather than token-specific, but remains verifier-grounded: its sign is determined by the verifier outcome relative to the group mean.
- Vanilla OPD provides dense token-level supervision without directly verifying teacher reliability, whereas GRPO provides verifier-grounded but coarse trajectory-level supervision.
- 优先核对语义命中的全文片段：3 2 Gate Conditioned Supervision Routing / B Training Hyperparameters And Infrastructure / D Per Configuration Gpu Utilization Traces。

Q7: 总结一下论文的主要内容

We introduce Teacher-Gated On-Policy Distillation (TGOPD), built on the principle that teacher reliability should be verified at the prompt level before dense supervision is…

AllSpark Team On-policy distillation (OPD) accelerates post-training by providing dense token-level supervision from a frozen teacher on the student's own rollouts. Vanilla OPD applies this supervision uniformly across prompts, without checking whether the teacher is reliable for each prompt.

every other distillation method causes negative transfer on LiveCodeBench: the student scores below the untrained base model (OPD -0.8, TrOPD -2.5, RG-OPD -3.5, RLSD-style -4.1). Yet TGOPD is the only method that achieves positive transfer (+3.0 over base) and, in fact, surpasses the teacher itself (+1.3 LCB, +1.1 OJBench).

TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two supervision regimes. A verifier audits the teacher on the current prompt.

主要贡献包括：every other distillation method causes negative transfer on LiveCodeBench: the student scores below the untrained base model (OPD -0.8, TrOPD -2.5, RG-OPD -3.5, RLSD-style -4.1).；Yet TGOPD is the only method that achieves positive transfer (+3.0 over base) and, in fact, surpasses the teacher itself (+1.3 LCB, +1.1 OJBench).；TGOPD represents teacher reliability as a per-prompt quantity and uses it to choose between two supervision regimes.。

需要注意的边界包括：It is therefore trajectory-level rather than token-specific, but remains verifier-grounded: its sign is determined by the verifier outcome relative to the group mean.；Vanilla OPD provides dense token-level supervision without directly verifying teacher reliability, whereas GRPO provides verifier-grounded but coarse trajectory-level supervision.。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 3 2 Gate Conditioned Supervision Routing / B Training Hyperparameters And Infrastructure / D Per Configuration Gpu Utilization Traces 部分。；它不一定和你当前最核心的 智能体 完全同题，但方法设计和评测组织值得借鉴。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 3 2 Gate Conditioned Supervision Routing / B Training Hyperparameters And Infrastructure / D Per Configuration Gpu Utilization Traces 部分。

## 基本信息

- 作者：Zhiwei Zhang, Zechen Sun, Fei Zhao, Kang Peng, Bin Liang, Huayu Deng, Yao Hu, Kam-Fai Wong, Mu Chuan
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2609.02998v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
