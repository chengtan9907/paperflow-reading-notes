---
user_id: "cheng tan"
paper_id: 1233
arxiv_id: "2606.22737"
title: "GroundEval: A Deterministic Replacement for LLM-as-Judge in Stateful Agent Evaluation"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22737.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22737"
abs_url: "https://arxiv.org/abs/2606.22737"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:10:20"
---
# GroundEval: A Deterministic Replacement for LLM-as-Judge in Stateful Agent Evaluation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent evaluation · grounded evidence · tool use · trajectory analysis

## 一句话总结

提出 GROUNDEVAL，一种基于确定性证据的智能体评估框架，通过检查工具调用轨迹而非最终答案来判断智能体是否使用了正确的证据。

## 摘要

> Before letting an agent operate over real context, can you prove it used the right evidence? GROUNDEVAL turns that question into a deterministic test of what the agent searched, fetched, cited, and was permitted to access. In one case study, two frontier LLM judges scored a plausible agent response above 0.85. But the trace told a different story: the agent had never retrieved the artifact its answer depended on, yielding a GROUNDEVAL score of 0.000.
> We introduce GROUNDEVAL, a judge-free framework for evaluating agents against grounded, time-bounded, and access-controlled evidence. GROUNDEVAL uses a domain configuration to generate questions, lets the agent choose how to answer, and then scores both the final answer and the recorded trajectory that produced it. The benchmark targets three failures that LLM-as-judge evaluation struggles to detect: whether an agent checked before claiming absence, reasoned only from evidence available to the actor at the relevant time, and used the correct causal mechanism rather than a plausible one. These correspond to three tracks: Silence, Perspective, and Counterfactual. GROUNDEVAL exposes when plausible answers rest on invalid evidence paths, and produces structured per-question diagnostics that pair tool activity with the agent's turn-level narration, making each score inspectable rather than merely reported. What our case studies turned up is that this gap isn't some rare corner case. It's exactly the blind spot that final-answer and judge-based scoring were never built to catch.
> Availability. Code: github.com/tenurehq/groundeval.

Q1: 这篇论文试图解决什么问题？

LLM-as-judge 评估无法检测智能体是否使用了正确的证据路径，可能给高分但实际上证据无效。具体三个问题：1）智能体是否在声称信息缺失前进行了实际搜索？2）智能体是否只使用了在当时时间点可供角色访问的证据？3）智能体是否使用了正确的因果机制而非仅是合理的解释？

Q2: 有哪些相关研究？

相关研究包括 TRAJECT-Bench 等轨迹级诊断基准，它们评估工具选择、参数正确性和依赖顺序，但 GROUNDEVAL 进一步引入访问控制和因果机制检测。此外，LLM-as-judge 评估方法虽流行但存在盲点，无法检测证据路径的无效性。

Q3: 论文如何解决这个问题？

提出 GROUNDEVAL 框架，基于事件日志、工件语料库、访问策略和评估协议（Evaluation Contract）。通过领域配置自动生成问题，智能体自由选择工具或上下文回答问题。框架记录完整轨迹（包括工具调用、引用和叙述），然后从三个 track 评分：Silence Track（检查缺失前是否搜索）、Perspective Track（检查证据是否在时间点和权限范围内）、Counterfactual Track（检查答案是否依赖正确的因果机制）。支持 Context Mode（通过引用观察证据路径）和 Tool Mode（直接观察检索行为），生成每个问题的诊断信息。

Q4: 论文做了哪些实验？

在一个案例研究中，智能体被问及 Confluence 页面的活动，其回答被两个前沿 LLM judge（Kimi-K2.6 和 ChatGPT-5.5）评分，两者均给出 >0.85 的高分，且评论称赞推理清晰。但 GROUNDEVAL 检查轨迹发现智能体从未实际检索相关页面，因此 Silence Track 得分为 0.000，总体分数极低。该实验展示了 LLM-as-judge 的盲点。

Q5: 发现了什么实验现象？

1）LLM-as-judge 容易被看似合理但缺乏实际证据支持的推理所欺骗；2）GROUNDEVAL 能够通过轨迹分析暴露无效证据路径，产生可诊断的结构化评分；3）案例表明该问题并非罕见角落案例，而是最终答案和 judge 评分系统从未设计要捕获的盲点。

Q6: 有什么可以进一步探索的点？

1）将 GROUNDEVAL 扩展到更多领域（如代码库、医疗记录、金融文档）；2）支持更多工具类型和复杂的访问控制策略；3）集成到智能体训练流程中，作为训练信号；4）研究如何自动生成反事实问题以增强测试覆盖；5）探索与其他评估指标（如安全性、效率）的结合。

Q7: 总结一下论文的主要内容

论文提出 GROUNDEVAL，一个无评判的智能体评估框架，用于检测智能体是否在思考过程中使用了正确的证据。核心思想：通过记录智能体的工具调用、引用和叙述轨迹，结合事件日志、工件语料库和访问策略，生成三类测试（Silence、Perspective、Counterfactual），每个问题产生结构化诊断。案例研究显示，两个 LLM-as-judge 对同一个无效推理给出高分，而 GROUNDEVAL 正确识别了证据缺失。文章批评了仅依赖最终答案或 judge 评分的评估方式，并展示了 GROUNDEVAL 作为确定性替代方案的可行性。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与智能体评估方向直接相关（用户画像中 agent 权重 0.10）。

## 基本信息

- 作者：Jeffrey Flynt
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.SE
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22737`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据（groundeval 片段），结合 heuristic_draft 完成。
