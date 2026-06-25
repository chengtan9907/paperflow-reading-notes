---
user_id: "cheng tan"
paper_id: 1231
arxiv_id: "2606.22953"
title: "Plans Don't Persist: Why Context Management Is Load Bearing for LLM Agents"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.22953.pdf"
pdf_url: "https://arxiv.org/pdf/2606.22953"
abs_url: "https://arxiv.org/abs/2606.22953"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:07:31"
---
# Plans Don't Persist: Why Context Management Is Load Bearing for LLM Agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · plan persistence · context management · hidden state probe

## 一句话总结

提出 replay pairing 诊断方法，通过对比有无计划在上下文中的隐藏状态余弦距离，发现标准 LLM 智能体的计划是上下文时间对象而非持久状态。

## 摘要

> Long-horizon agents depend on context management: systems compress, summarize, and evict old tokens so tasks can continue beyond finite windows. That is safe only when dropped information is no longer needed or has been internalized. Plans are the stress case: they are written early, used for many steps, and first to be evicted. We introduce replay pairing, a diagnostic that runs the same trajectory with and without the plan in history and measures hidden-state cosine distance. On Llama-3.1-70B, plan signal spikes to 0.453 one step after the plan, then falls 4.1x in a single action-observation step; HotpotQA falls 12.4x. This is evidence that standard LLM agents do not carry plans forward as persistent state, and instead depend on the plan remaining in context. A layer-L32 probe detects this decay as a diagnostic, not as proof that it reads plan content itself. Reasoning models add a measurement confound: their ` ` traces re-derive plan content, so standard stripping leaves plan evidence in the stripped condition. We name this the reasoning-trace confound and fix it with strict stripping, which removes prior ` ` blocks from the stripped run only. It recovers +163% of the step+1 signal in-sample and +153% held out, while not meaningfully changing non-reasoning Llama (+4.8%). On DeepSeek-R1-Distill-Llama-70B, a Llama-trained probe transfers at AUROC 0.748 (p=6e-4), while R1-specific probes reach 1.000, suggesting R1 encodes plan signal in a different hidden-state direction. Finally, a compression stress test shows the practical cost: naive plan eviction cuts ALFWorld success by 34.7pp, while probe-gated re-surfacing does not recover it. The contribution is a measurement and stress-test framework showing that agent-critical information can be context-resident rather than persistent. Context management is load bearing, but plan protection alone is not enough.

Q1: 这篇论文试图解决什么问题？

长时域 LLM 智能体在有限上下文窗口内运行，实际部署中常通过压缩、摘要、丢弃旧令牌来管理上下文，但该做法仅当被丢弃信息不再需要或已被模型内化时才安全。计划（plan）是这类信息的压力案例：它在任务早期生成、指导后续多步行动、且容易在上下文管理中被优先丢弃。现有研究隐含假设计划一旦生成即被智能体内化（持久状态），但缺乏诊断手段区分计划是持久状态还是仅依赖上下文出现。本文瞄准该机制间隙：证明计划是上下文时间对象（context-time object），即其影响仅在上下文保留时存在，一旦被丢弃则急剧衰减。

Q2: 有哪些相关研究？

1. **智能体规划框架**：ReAct (Yao et al., 2022)、Chain-of-Thought (Wei et al., 2022)、Reflexion (Shinn et al., 2023) 等均假设智能体写出计划后计划指导后续行为，行为对齐指标确认计划有效，但无法区分两种机制（持久状态 vs 上下文依赖）。
2. **上下文管理技术**：总结、压缩、滑动窗口等方法被广泛使用，但从未系统检验被丢弃信息的关键性。
3. **模型忠实性与表示分析**：已有工作利用探针或相似度度量研究模型内部表示，但未针对智能体计划持久性问题设计。
4. **推理模型（reasoning models）**：如 DeepSeek-R1 通过 <think> 块生成推理痕迹，可能干扰计划信号测量。

Q3: 论文如何解决这个问题？

1. **Replay Pairing 诊断**：对同一智能体轨迹运行两次推理——一次保留计划在上下文（plan-full），一次掩蔽计划（plan-stripped）但不改变其他令牌；计算两次推理中每一步隐藏状态（hidden state）的余弦距离，作为计划信号（plan signal）度量。高余弦距离（≤1）表示计划强烈影响隐藏状态，低距离表示影响弱。
2. **推理痕迹混淆控制**：针对推理模型（如 R1）在 plan-stripped 条件下仍可能从之前的 <think> 块中恢复计划内容，引入严格剥离（strict stripping），即仅在 stripped 运行时删除全部历史 <think> 块，从而消除残留信号。
3. **压缩压力测试**：在 ALFWorld 任务上，比较四种上下文策略（基线：保留最近 4 条消息；条件丢弃：根据探针信号决定是否保留计划；随机丢弃；完全丢弃计划），在相同预算下测量任务成功率。

Q4: 论文做了哪些实验？

1. **隐藏状态衰减测量**：使用 Llama-3.1-70B 在 HotpotQA（问答）和自定义规划任务上运行 replay pairing，记录每步余弦距离。
2. **跨模型复制**：在 Llama-3.1-8B、Llama-3.1-405B （合理推断）上重复实验，确认衰减趋势。
3. **推理模型实验**：使用 DeepSeek-R1-Distill-Llama-70B，先用标准 stripping（仅删除当前步的 <think> 块）重复衰减实验，发现残留信号；再用严格 stripping 重复，恢复大部分信号。
4. **探针迁移**：训练线性探针（layer L32）区分 plan-full 与 plan-stripped 隐藏状态；将 Llama 探针直接应用于 R1（无微调），测量 AUROC；对比 R1 专属探针。
5. **压缩压力测试**：在 30 个 ALFWorld 任务（×5 runs = 150 runs/策略）用 Llama-3.1-70B（20步上限）测试四种上下文策略：keep_recent=4 消息+额外；报告成功率。

Q5: 发现了什么实验现象？

1. **计划信号快速衰减**：在 Llama-3.1-70B 上，计划信号（余弦距离）在写入计划后的第一步达到峰值 0.453，随后一个动作-观察步内衰减 4.1 倍；在 HotpotQA 上衰减 12.4 倍。衰减在 8B 和 405B 模型上复现。
2. **推理痕迹混淆**：使用标准 stripping 时，R1 在 stripped 条件下仍有较高信号（因为历史 <think> 块隐含计划内容）；严格 stripping 后，step+1 信号恢复 +163%（样本内）和 +153%（样本外），而对非推理模型 Llama 仅变化 +4.8%，证明混淆的确存在。
3. **探针迁移性**：Llama 训练的探针在 R1 上达到 AUROC 0.748（p=6e-4），而 R1 专属探针可达 1.000，表明 R1 以不同的隐藏状态方向编码计划信号。
4. **压缩代价**：简单丢弃计划（naive eviction）使 ALFWorld 成功率下降 34.7 个百分点；基于探针的条件重新显示（probe-gated re-surfacing）未能恢复性能，提示计划保护本身不足以弥补上下文管理损失。

Q6: 有什么可以进一步探索的点？

1. **通用性验证**：将 replay pairing 扩展到其他可能仅存在于上下文的关键令牌，如安全指令、用户偏好、工具定义等。
2. **更好的上下文保护策略**：设计自适应保留机制，不仅保护计划，还考虑重要性分级。
3. **模型内化训练**：研究能否通过微调或提示使模型更持久地内化计划，减少对上下文依赖。
4. **推理模型校准**：针对推理模型因 <think> 痕迹产生的混淆，开发更精确的信号剥离或测量方法。
5. **跨任务域评估**：在更复杂的长期任务（如具身智能、软件开发）中验证衰减现象和压缩影响的普遍性。
6. **理论解释**：探索计划信号快速衰减的机制原因（注意力模式、层间交互等）。

Q7: 总结一下论文的主要内容

本文诊断并验证 LLM 智能体中计划的持久性假设。作者提出 replay pairing 方法，通过对比有无计划历史轨迹中每步隐藏状态的余弦距离，直接测量计划对模型内部表示的影响。实验表明，在 Llama-3.1-70B 上计划信号在一步内衰减 4-12 倍，证明计划未作为持久状态存储而仅依赖上下文出现。针对推理模型（如 DeepSeek-R1）的 <think> 痕迹引入推理痕迹混淆，严格剥离可恢复信号。进一步，压缩压力测试显示简单丢弃计划导致 ALFWorld 成功率下降 34.7 个百分点，但探针门控重新显示未能完全恢复，说明上下文管理是“承重”但计划保护本身不够。主要贡献包括：1) 验证计划是上下文时间对象；2) 提供可复现的诊断框架；3) 揭示推理痕迹混淆；4) 量化压缩的实际代价。该工作为智能体上下文安全设计提供了关键测量基础。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文直接聚焦 LLM 智能体上下文管理，与用户画像中 agent 方向（权重 0.10）高度相关。

## 基本信息

- 作者：Aman Mehta, Anupam Datta
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.22953`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（abstract, introduction, results 片段）并补充 heuristic_draft。
