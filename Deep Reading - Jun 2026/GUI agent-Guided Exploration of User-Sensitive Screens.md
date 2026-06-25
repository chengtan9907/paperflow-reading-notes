---
user_id: "cheng tan"
paper_id: 1466
arxiv_id: "2606.25705v1"
title: "GUI agent: Guided Exploration of User-Sensitive Screens"
publish_date: "2026-06-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.25705v1.pdf"
pdf_url: "https://arxiv.org/pdf/2606.25705v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-25T18:20:22"
---
# GUI agent: Guided Exploration of User-Sensitive Screens

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agent · gui automation · user-sensitive screens · exploration

## 一句话总结

提出一个探索器代理，从示范任务出发系统探索GUI应用查询空间，自动识别可能导致用户敏感屏幕的查询，并训练代理在关键时刻请求用户接管。

## 摘要

> LLM agents are increasingly being used to automate tasks for users within an open GUI environment. They inevitably encounter screens containing user-sensitive information, for which takeover of task execution by the user is highly desirable or even necessary. State-of-the-art LLM-driven agents are usually fine-tuned to complete tasks regardless of the safety implications of their actions. This makes their real-world deployment difficult and adversely affects the reliability. Therefore, it is crucial to identify and categorize user-sensitive states and define user-sensitive queries. This dataset would be to engineers to recognize and request handover to the user in critical scenarios. This short paper develops an explorer agent that systematically explores the query space starting from one demonstrated task to identify queries that, if executed, would lead to user-sensitive states in a GUI environment.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决LLM驱动的GUI代理在执行任务时可能遇到包含用户敏感信息的屏幕，但现有代理被训练为无论如何都要完成任务，缺乏识别敏感状态并请求用户接管的能力，导致安全风险和部署困难。需要一种方法自动探索和识别这些敏感查询，以便在必要时将控制权交还给用户。

Q2: 有哪些相关研究？

相关研究包括：（1）GUI代理泛化工作，如GUI-Xplore（Sun et al., 2025）使用预录视频构建转移图实现跨应用泛化，GUI-Explorer（Xie et al., 2025）构建无监督探索；（2）基于探索的先验方法，利用动作-状态图或树搜索（如Koh et al., 2024的树搜索语言模型代理）；（3）GUI自动化工具如UI-TARS（Wang et al., 2025）等。但这些工作主要关注任务完成效率，未专门针对用户敏感状态识别与接管。

Q3: 论文如何解决这个问题？

论文提出一个框架，包含两个模型：一个基础语言/视觉语言模型（LM）用于决定动作，另一个探索器LM（主要贡献）用于系统探索查询空间。探索器从用户示范的一个任务出发，通过迭代训练构建动作-状态图，沿可能导致用户敏感屏幕的路径进行探索，并学习识别哪些查询会引发敏感状态。框架在探索过程中不断更新探索器LM，使其能更高效地探测敏感区域，最终输出一个敏感查询集合。

Q4: 论文做了哪些实验？

论文在选定的GUI环境（如电子邮件或银行应用）中进行了实验，从用户示范任务开始，让探索器迭代探索查询空间。实验测量了以下指标：每个训练轮次探索的查询数量、用户敏感查询的比例、探索器识别敏感屏幕的精度与召回（推测），以及用户敏感查询空间随训练轮次的变化。具体实验设置（如应用类型、示范任务数量、LM型号等）未在检索证据中明确，但结论部分提到用户敏感查询空间在后续训练轮次后缩小。

Q5: 发现了什么实验现象？

实验观察到：（1）随着训练轮次增加，用户敏感查询空间逐渐缩小，表明探索器有效识别并避免敏感区域；（2）探索器能够从单一示范任务推广到未见过但相关的敏感查询；（3）基于动作-状态图的探索方法比随机探索更高效地发现敏感状态（合理推断）。但缺乏具体数值和基线对比，实验结果部分描述较为笼统。

Q6: 有什么可以进一步探索的点？

论文在结论中提及若干未来方向：（1）将方法扩展到更多类型的GUI应用（如社交媒体、文件管理）；（2）优化探索器的训练效率，减少所需示范任务数量；（3）处理动态GUI环境（如屏幕内容实时变化）；（4）对用户敏感状态进行更细粒度的分类（如隐私、安全、财务等）；（5）探索不同LM架构（如纯文本 vs 视觉语言模型）对敏感状态识别的影响。

Q7: 总结一下论文的主要内容

本文针对LLM驱动的GUI代理在开放环境中面临用户敏感屏幕时缺乏接管机制的问题，提出了一种系统探索查询空间的方法。主要贡献包括：开发一个探索器代理，从用户示范任务出发，通过迭代训练构建动作-状态图，自动识别会导致用户敏感状态的查询路径；在选定的GUI应用上验证表明，用户敏感查询空间随训练轮次增加而缩小，证明探索器有效。相关工作涵盖了GUI代理泛化与探索先验。实验展示了方法的可行性，但缺乏定量结果和严格基线对比。未来工作可扩展应用场景和优化效率。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文直接涉及智能体（agent）的安全与接管机制，与用户画像中智能体方向高度相关。

## 基本信息

- 作者：Aradhana Nayak, Mussadiq Nazeer, Wang Peng, Feng Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-06-24
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.25705v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，但证据覆盖有限（仅摘录了Abstract、Introduction、Related Work、Conclusion部分片段），部分细节为合理推断。
