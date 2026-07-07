---
user_id: "cheng tan"
paper_id: 2861
arxiv_id: "2607.04293"
title: "CausalGame: Benchmarking Causal Thinking of LLM Agents in Games"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04293.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04293"
abs_url: "https://arxiv.org/abs/2607.04293"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:53:06"
---
# CausalGame: Benchmarking Causal Thinking of LLM Agents in Games

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：causal thinking · large language models · benchmark · scientific discovery

## 一句话总结

CausalGame是一个通过交互式游戏场景评估大语言模型代理因果思考能力的基准测试，揭示了当前模型在区分因果与关联、识别隐藏偏见方面的普遍不足。

## 摘要

> Building AI Scientist agents with Large Language Models (LLMs) has recently attracted growing attention. Since scientific discovery fundamentally relies on uncovering causal relationships from observations, the capability of causal thinking, i.e., distinguishing causation from correlation and recognizing hidden biases, is essential to LLM agents. Although a number of benchmarks exist for AI Scientists, none explicitly incorporate challenges from selection bias, measurement error, and hidden confounders that widely exist in real-world scientific discovery. To this end, we present CausalGame, a benchmark that evaluates the causal thinking capabilities of LLM agents through interactive games. CausalGame asks LLM agents to actively design experimental protocols, collect observation data, and derive a final solution with an explanation report. To emulate realistic scientific discovery challenges, we design 14 scenarios that incorporate selection bias, measurement error, and hidden confounders. Across 30 LLM agents, none demonstrates reliable causal thinking: the best model reaches only 68.0% survival against analytical optima of 78-85%, and merely 5-7% of sessions receive credits on the causal-reasoning rubrics. CausalGame provides a scalable and controlled testbed for evaluating the causal thinking of AI Scientist agents.

Q1: 这篇论文试图解决什么问题？

现有AI科学家基准测试（如Acharya et al., 2025; Verma et al., 2025）主要评估通用能力，但未明确包含现实科学发现中普遍存在的选择偏差、测量误差和隐藏混杂因素等因果推理挑战。这导致对LLM代理因果思考能力的评估不足，而这些能力是科学发现的核心。CausalGame旨在填补这一空白，提供一个受控但接近真实挑战的测试环境。

Q2: 有哪些相关研究？

相关研究包括AI科学家代理（Lu et al., 2024; Yamada et al., 2025）及其基准测试（Acharya et al., 2025; Verma et al., 2025）。这些基准通常评估假设生成、实验设计等综合能力，但缺乏对因果推理难点的针对性设计。其他相关领域包括因果关系发现、反事实推理等。CausalGame通过引入交互式游戏场景，明确测试代理在存在选择偏差、测量误差和隐藏混杂因素时的因果思考能力，与现有基准形成互补。

Q3: 论文如何解决这个问题？

CausalGame设计了14个互动游戏场景，每个场景包含一个隐含的因果结构（如选择偏差、测量误差或隐藏混杂因素）。代理需通过主动设计实验协议（如选择观察变量、执行干预）、收集观察数据，并最终推导出正确因果关系并撰写解释报告。游戏机制模拟了现实科学发现中的交互过程，允许代理主动探索并修正假设。游戏评分基于最终结论的正确性（存活率）以及对因果推理过程的细致评分（如是否识别出偏差来源）。

Q4: 论文做了哪些实验？

实验评估了30个前沿LLM代理，包括GPT-4、Claude、Gemini等系列模型。每个代理在14个场景中多次运行。主要指标：存活率（结论正确且可存活）和因果推理评分（基于预定义rubric）。结果显示，最佳模型（GPT-4o）平均存活率68.0%，而分析最优基于因果图计算为78-85%。在因果推理评分中，仅5-7%的会话获得学分（即正确识别因果机制）。此外，实验分析了代理在不同困难类型（选择偏差、测量误差、隐藏混杂因素）下的表现差异，发现所有模型在隐藏混杂因素场景下表现最差。

Q5: 发现了什么实验现象？

主要发现：1) 所有LLM代理均不能可靠地进行因果推理，最佳模型存活率远低于分析最优；2) 代理容易受到选择偏差影响，例如在只能观察幸存无人机的场景中，它们倾向于从幸存样本中学习错误关联；3) 代理获得的因果推理评分普遍极低，表明它们无法识别偏差来源或解释因果机制；4) 即使在成功场景中，代理也可能通过随机猜测或过度简化获得高存活率，但缺乏真正的因果理解；5) 代理在隐藏混杂因素场景下失败最严重，而选择偏差和测量误差场景也显著困难。

Q6: 有什么可以进一步探索的点？

1) 扩展更多真实场景，包括开放假设、多因果路径等；2) 改进代理的推理能力，如增强多步推理、长范围依赖处理；3) 研究多轮交互和自适应学习；4) 改进自动评估rubric以减少LLM评审者的系统性偏差；5) 探索与因果发现算法的结合；6) 测试更强LLM和专门因果推理模型；7) 将基准扩展到现实科学数据（如生物、社会科学）。

Q7: 总结一下论文的主要内容

论文提出CausalGame，一个评估LLM代理因果思考能力的基准测试。通过14个模拟选择偏差、测量误差和隐藏混杂因素的游戏场景，要求代理主动设计实验、观察数据并报告因果关系。对30个前沿LLM的评估显示，所有模型均表现出严重的因果推理缺陷：最佳存活率68.0%（分析最优78-85%），因果推理评分通过率仅5-7%。实验揭示了代理容易受选择偏差影响、无法识别隐藏混杂等关键问题。论文还提供了详细的失败模式分析，并讨论了未来方向。CausalGame为AI科学家系统的因果思考评估提供了可扩展的平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文直接涉及智能体（Agent）评估，与用户画像中智能体方向（权重0.10）高度相关。

## 基本信息

- 作者：Zhenhao Chen, Yongqiang Chen, Chenxi Liu, Junchi Yu, Xiangchen Song, Zijian Li, Jialin Li, Philip Torr, Bo Han, Kun Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.LG, stat.ML
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04293`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据，包括摘要、Related Work、Method、Conclusions等片段，以及field_evidence_map的字段映射。
