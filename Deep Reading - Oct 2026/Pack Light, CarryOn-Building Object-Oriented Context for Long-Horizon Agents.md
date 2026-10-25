---
user_id: "cheng tan"
paper_id: 16401
arxiv_id: "2610.18784v2"
title: "Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.18784v2.pdf"
pdf_url: "https://arxiv.org/pdf/2610.18784v2"
generation_provider: "heuristic"
generation_model: "PaperFlow template"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:15:24"
---
# Pack Light, CarryOn: Building Object-Oriented Context for Long-Horizon Agents

> ★★★★☆ 推荐阅读 · 模型 heuristic/PaperFlow template

🏷 关键词：around reusable · execution context · long-horizon agents · object-oriented context · accounts progress · carryon · basis · records

## 一句话总结

Long-horizon agents accumulate progress in persistent task state, yet repeatedly process interaction histories describing how that progress was achieved.

## 摘要

> Long-horizon agents accumulate progress in persistent task state, yet repeatedly process interaction histories describing how that progress was achieved. This raises a natural question: can long-horizon context instead be organized around reusable progress rather than the history that produced it? In this paper, we introduce CarryOn, an object-oriented context runtime that organizes continuation around reusable task objects. Rather than treating compressed histories or reconstructed accounts of progress as the primary basis for continuation, CarryOn builds a context view from continuation carriers, complementary execution context, and a live tail of recent interactions. A core challenge is keeping this reusable basis valid after its surrounding history is retired: how can CarryOn update the basis without reconstructing the full representation at every fold? CarryOn maintains carrier records across folds, revising affected records against bound evidence and applicable environment checks while carrying forward unaffected records and jointly updating execution context. Experiments on OSWorld 2.0 and BrowseComp-Plus show improved gains of 5.8% and 7.0% while reducing total token usage by 51.9% and 22.9%, respectively. Project page: https://ethannju.github.io/carryon

Q1: 这篇论文试图解决什么问题？

xt around reusable progress improves over generic compression, while the full CarryOn runtime further improves over the same principles implemented through repeated summarization. Continuation carriers and execution context play complementary roles, and reusable progress is typically maintained across folds rather than repeatedly reconstructed.

Q2: 有哪些相关研究？

当前自动解析没有稳定提取出 Related Work 的完整脉络。可先从论文引言、相关工作章节和引用线索核对它主要对比了哪些方法。

从当前证据可见，论文的定位至少包括：identifies progress that may become reusable in subsequent execution.；The Tracker maintains candidates, each representing a potential new carrier or an update to an existing one.；Those internal rules create no new host agent duty to re-read or re-verify.。

Q3: 论文如何解决这个问题？

identifies progress that may become reusable in subsequent execution. The Tracker maintains candidates, each representing a potential new carrier or an update to an existing one.

Q4: 论文做了哪些实验？

Those internal rules create no new host agent duty to re-read or re-verify. Preserve targeted checks for actual conflicts, missing required information or unknown outcomes;

Q5: 发现了什么实验现象？

Those internal rules create no new host agent duty to re-read or re-verify. Preserve targeted checks for actual conflicts, missing required information or unknown outcomes;

Q6: 有什么可以进一步探索的点？

- Carriers and the execution context are complementary.
- We conduct an ablation study on the shared 40-task OSWorld 2.0 subset by hiding either continuation carriers or execution context from the host agent.
- As the task progresses, CarryOn jointly updates continuation carriers and execution context to reflect evolving task state and remaining requirements, progressively replacing…
- 优先核对语义命中的全文片段：Method。
- 先看 Introduction，确认论文到底在补哪一块空白。
- 再看 Method / Approach，把核心模块、训练目标和输入输出关系理清。
- 最后看 Conclusion / Discussion，判断作者自己如何定义边界与下一步。

Q7: 总结一下论文的主要内容

Long-horizon agents accumulate progress in persistent task state, yet repeatedly process interaction histories describing how that progress was achieved.

xt around reusable progress improves over generic compression, while the full CarryOn runtime further improves over the same principles implemented through repeated summarization. Continuation carriers and execution context play complementary roles, and reusable progress is typically maintained across folds rather than repeatedly reconstructed.

identifies progress that may become reusable in subsequent execution. The Tracker maintains candidates, each representing a potential new carrier or an update to an existing one.

Those internal rules create no new host agent duty to re-read or re-verify. Preserve targeted checks for actual conflicts, missing required information or unknown outcomes;

主要贡献包括：identifies progress that may become reusable in subsequent execution.；The Tracker maintains candidates, each representing a potential new carrier or an update to an existing one.；Those internal rules create no new host agent duty to re-read or re-verify.。

需要注意的边界包括：Carriers and the execution context are complementary.；We conduct an ablation study on the shared 40-task OSWorld 2.0 subset by hiding either continuation carriers or execution context from the host agent.；As the task progresses, CarryOn jointly updates continuation carriers and execution context to reflect evolving task state and remaining requirements, progressively replacing…。

与用户画像的关系：从全文语义检索命中的片段看，相关信息主要落在 Method 部分。；这篇论文和你当前画像里的方向有直接重合：智能体（权重 0.10）。；如果你更看重系统性工作，可以重点看它如何组织任务设定、实验协议和整体框架。。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：从全文语义检索命中的片段看，相关信息主要落在 Method 部分。

## 基本信息

- 作者：Yuchen Lu, Julian McAuley, Lei Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：heuristic / PaperFlow template
- arXiv ID：`2610.18784v2`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 生成式精读补充本次未返回，当前内容仍按精读模板基于已拿到的摘要、元数据和可用 PDF 片段生成。
