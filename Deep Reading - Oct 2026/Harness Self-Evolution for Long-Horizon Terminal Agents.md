---
user_id: "cheng tan"
paper_id: 17188
arxiv_id: "2610.27435"
title: "Harness Self-Evolution for Long-Horizon Terminal Agents"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.27435.pdf"
pdf_url: "https://arxiv.org/pdf/2610.27435"
abs_url: "https://arxiv.org/abs/2610.27435"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:47:00"
---
# Harness Self-Evolution for Long-Horizon Terminal Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：harness evolution · long-horizon terminal agent · execution reliability · flipping tasks

## 一句话总结

RiseHarness 提出一种在不更新模型权重的前提下，通过对比同一任务的成败轨迹、结合内部 harness traces 与 Harness Handbook、并借鉴外部 harness 设计来迭代改进 agent 运行时层（harness），从而同时提升长时程终端任务的平均成功率与跨重复运行一致性的框架。

## 摘要

> 论文围绕长时程终端 agent 的“执行可靠性”问题展开：同一模型、同一任务在多次运行中可能因为局部错误改变环境与后续上下文而出现不同结果，因此平均成功率不足以刻画执行一致性。作者提出 RiseHarness，一个迭代式改进 harness（模型与环境的运行时交互层）的框架：从“flipping tasks”（同一配置下有成功也有失败的任务）出发，配对对比成功与失败的 agent 轨迹，并结合内部 harness 执行痕迹与 Harness Handbook（记录交互行为与对应实现的连接），定位执行问题；随后检索其他 harness 框架的相关设计，结合当前执行流程、可用信号与模型行为生成改进提案；通过实现检查和重复运行评估决定改动是否保留，被接受的版本连同更新的 Handbook 与执行证据进入下一轮迭代。实验在 GPT-6-sol 与 DeepSeek-V4-Flash 两个模型上、以 Terminus-2 harness 为种子在 Terminal-Bench Hard (TBH) 上演化，并在未参与演化的 benchmark（如 TB2.1）上验证迁移性。结果显示相对种子 harness，RiseHarness 同时提升平均任务成功率与重复运行一致性；消融实验支持 harness 执行证据与外部设计研究两个组件的贡献。

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的核心问题是：长时程（long-horizon）终端（terminal）agent 在执行复杂任务时，平均任务成功率无法反映执行可靠性——同一模型在同一任务上多次运行时，由于局部错误会改变环境状态和后续上下文，结果可能在成功与失败之间波动。具体而言，论文针对以下子问题展开：
1. 执行一致性缺失：average success 只衡量“能否解决”，不衡量“是否总能在多次运行中一致解决”；τ-bench 用 pass^k 刻画一致性，但如何在 harness 层面系统性提升这一指标尚未解决。
2. 错误定位困难：长程轨迹中失败原因可能隐藏在 agent 推理、工具调用、观察构建、上下文管理等多层交互中，单看 agent 轨迹难以判断问题出在模型行为还是 harness 设计。
3. 运行时层（harness）缺乏系统性改进方法：已有工作（SWE-agent、OpenHands 等）证明了接口设计会影响固定模型的任务表现，但缺少一个闭环、可迭代的流程来持续改进 harness。
4. 失败信息未被有效利用：FIRE 等工作研究了不更新模型权重的运行时干预，但主要针对单次运行内的失败恢复；本文关注跨重复运行的“flipping tasks”（同一配置下有成功有失败的任务），将其作为改进信号源。
5. 评估协议问题：harness 改动是否真正有效，需要在同一模型、同一 benchmark 上的重复运行中验证，避免把一次成功误判为改进；论文为此设计了“先实现检查、后重复评估”的采纳机制。
总体而言，论文试图回答“能否通过迭代改进 agent 的运行支撑层，让同一个模型在长时程终端任务中既更成功、也更稳定”，即从“模型能不能做”转向“系统能不能稳定地做”。

Q2: 有哪些相关研究？

论文的相关工作可从三条线索梳理：
1. 长时程执行与一致性度量：Long-Horizon-Terminal-Bench 及其 2.0 扩展使用 intermediate grading 度量长工作流中的进度；τ-bench 提出 pass^k 作为一致性度量，要求任务在全部 k 次重复运行中都成功才算通过；更广泛的可靠性研究还涉及 robustness、predictability 与 safety。作者把这一定义作为“成功率之外还需衡量可靠性”的依据。
2. 失败诊断与恢复：AgentDebug 诊断 agent 错误并提供纠正性反馈以支持恢复；AgentRx 利用约束和 evidence-bearing validation logs 定位失败；FIRE 研究 failure-informed 的运行时干预，在不更新模型权重的情况下提升重复成功率。这些工作表明“不动权重、改运行时”是一条可行路线，但多聚焦于单次运行内的干预。
3. Harness 设计与框架：SWE-agent 证明在模型固定的情况下，界面设计（动作格式、观察构建等）显著影响任务表现；OpenHands 提供可组合工具、记忆管理、沙箱执行等运行时组件；Terminus-2 是本文的种子 harness。作者将 harness 定义为组织模型与环境交互的运行时层，包括命令执行、观察构建、上下文管理与完成判定。
此外，从元数据与摘要证据看，论文还涉及“flipping tasks”配对对比的思想（同一配置下成功/失败轨迹配对）作为演化信号，以及跨 harness 框架的外部设计检索（在设计提案阶段借鉴其他框架的模式）。整体上，本文处于“模型能力固定、系统层优化”这一研究范式内，与 FIRE、AgentDebug、AgentRx 形成最直接的方法论竞争。

Q3: 论文如何解决这个问题？

RiseHarness 的核心是一个闭环的 harness 演化流程，具体包括以下机制（合理推断与论文片段结合，部分细节需回原文确认）：
1. 种子与演化对象：从未修改的 Terminus-2 harness 出发，保持模型权重完全固定，在开发集 Terminal-Bench Hard (TBH) 上重复运行任务，获得成功/失败结果与双轨痕迹（agent 轨迹 + harness 内部 traces）。
2. Flipping tasks 筛选：定义任务 i 在第 t 轮 harness Ht 下的结果指示 yi,j(Ht)∈{0,1}（j 为重复序号），当某任务的重复结果既非全成功也非全失败时，将其选入 flipping task 集合 Ft = {i : 0 < Σ yi,j < rt}，即“同一配置下既有成功又有失败”的任务；对这些任务配对成功与失败运行。
3. 双轨证据：不仅分析 agent 轨迹，还利用 harness 内部执行痕迹（traces）与 Harness Handbook（把执行问题连接到具体交互行为及其实现的手册）来定位问题；消融实验表明 harness traces 能暴露 agent 轨迹单独看不到的内部处理过程。
4. 外部设计检索：在生成提案前，检索其他 harness 框架中与当前执行流程、可用信号和模型行为相适应的设计，生成候选提案。
5. 采纳机制：对候选改动先做实现检查（implementation checks），然后在开发集 D 上收集全新重复运行（Evaluate 阶段），根据任务结果与双轨痕迹决定是否采纳；被采纳的版本连同更新的 Handbook 和执行证据进入下一轮迭代（“每个版本保留先前改动并新增一个机制”）。
6. 评估协议：主实验报告平均成功率（Pass@1）与可靠性（重复运行全部成功的任务比例，以及 pass^k 类指标）；消融实验对比去掉 harness traces 与 Handbook、省略外部设计研究两种情况；迁移实验在未参与演化的 benchmark（如 TB2.1）上冻结演化后的 harness 验证泛化。
注意：heuristic_draft 中出现的“audit-fence”“self-consistency”“Claude-skill verification protocol”等内容与论文标题和正文证据不一致，属于检索噪声或无关元数据，应在阅读时忽略。

Q4: 论文做了哪些实验？

论文的实验围绕四个研究问题展开（RQ1–RQ4）：
1. 模型与基准：在 GPT-6-sol 和 DeepSeek-V4-Flash 两个模型上评估，模型权重在整个 harness 演化过程中固定；每个模型从未修改的 Terminus-2 harness 出发，在 Terminal-Bench Hard (TBH)（100 个有挑战性的终端任务）上演化；重复运行既提供行为分析的执行轨迹，也提供评估和选择 harness 版本的任务结果。
2. RQ1（主效果）：对比 RiseHarness 与种子 harness 及现有 baselines 的任务成功率与重复运行可靠性（Pass@1、Pass^4、全部成功任务比例）。
3. RQ2（消融）：分别去除 harness traces 与 Handbook、省略外部设计研究，考察对 harness 演化的影响。
4. RQ3（行为分析）：分析不同模型上保留的 harness 改动的行为目标与干预机制差异（例如 DeepSeek-V4-Flash 上关于 completion confirmation 的机制逐步累积）。
5. RQ4（迁移）：冻结模型与演化后的 harness，在未参与演化的 benchmark（如 TB2.1）上检验成功率与可靠性的提升是否迁移。
6. 演化过程细节：每一轮由 flipping tasks 触发，中间版本逐个机制累积，最终版本在 TBH 上表现超过中间版本；具体每轮任务数 rt、重复次数、迭代轮数等数值论文中未在检索证据中给出，需查原文实验设置部分。

Q5: 发现了什么实验现象？

从检索到的 results 与 conclusion 证据可归纳的观测包括：
1. 总体效果：相对种子 Terminus-2 harness，RiseHarness 在两个模型上都提升平均任务表现（Pass@1 提高）和跨重复运行一致性（全部成功的任务比例提高）；表 1 汇总了 RiseHarness 与对比 baselines 的性能。
2. 可靠性提升独立报告：说明“可靠性是单独报告的”，即成功率的提升与一致性的提升作为两个维度分开呈现。
3. 演化过程中的波动：在 TBH 上，中间版本之间存在性能波动（“despite fluctuations among intermediate versions”），但最终版本仍优于中间版本——这提示 harness 演化不是单调递增的，需要依赖重复评估与采纳门控来筛选。
4. 机制累积模式：DeepSeek-V4-Flash 上针对 completion confirmation 的机制随版本逐步累积（每个版本保留先前改动并新增一个机制），体现出一种“增量补丁”式的演化路径。
5. 消融支持：去除 harness 执行证据（traces/Handbook）和外部设计研究都会削弱演化效果，说明双轨证据与外部设计检索都是有效组件。
6. 反直觉/张力点：平均成功率提升与中间版本波动并存，意味着若不进行重复运行评估，单次运行的假阳性可能误导版本选择；这正好呼应论文“用 flipping tasks + 重复评估”的方法论动机。
注：GPT-6-sol 在 TBH 与 TB2.1 上的具体数值（如 5.25%、3.09%）出现在 heuristic_draft 中，但检索证据未直接支撑，需回原文 Table 1 核对。

Q6: 有什么可以进一步探索的点？

论文自身提出的未来方向（来自 conclusion 检索证据）包括：
1. 在更多模型、更多任务分布和更多 agent 框架上验证 RiseHarness 的普适性。
2. 探索降低 harness 演化评估成本的方法（当前需要反复多次重复运行，开销较大）。
基于论文的问题设定与方法，还可进一步探索的方向包括（推测）：
3. 将 flipping tasks 的筛选阈值 rt 与重复次数 k 联合优化，研究“最少重复多少次才能可靠识别 harness 改进”。
4. 将演化出的 harness 机制（如 completion confirmation、重复读取的观测去重等）抽象为可复用的设计模式库，跨模型迁移时直接匹配而不重新演化。
5. 把 harness 演化与模型微调、prompt 优化结合，研究系统层与模型层改进的互补性与顺序效应。
6. 将一致性指标从 pass^k 扩展到更细粒度（如部分成功、错误严重度、恢复时延），避免全有或全无的二值化丢失信息。
7. 研究 harness 演化的“过拟合”风险：在开发集 TBH 上演化多轮后，改进是否可能在未见过的任务上反而有害——论文的 RQ4 迁移实验是第一步，但还缺少对负迁移案例的机制分析。
8. 将方法论迁移到图形界面、浏览器、移动设备等其他终端形态，检验“双轨痕迹 + Handbook + 外部检索”是否依赖终端任务的特殊性。

Q7: 总结一下论文的主要内容

论文提出 RiseHarness，目标是在不更新模型权重的前提下，通过迭代改进 agent 的运行时层（harness），同时提升长时程终端任务的平均成功率与重复运行一致性。论证主线是：平均成功率高不代表执行可靠，因为同一模型同一任务多次运行可能因局部错误改变环境与上下文而产生不同结果；因此需要把“系统能否稳定支持成功执行”作为独立的研究对象。技术主线是一个闭环演化框架：从 Terminus-2 种子 harness 开始，在 TBH 开发集上重复运行任务，筛选“flipping tasks”（同一配置下既有成功又有失败的任务），配对对比成功与失败轨迹，同时利用 harness 内部 traces 与 Harness Handbook 将执行问题定位到具体交互行为与实现；随后检索其他 harness 框架的相关设计，结合当前执行流程、可用信号与模型行为生成提案；通过实现检查与开发集上的全新重复评估决定是否采纳，被采纳的版本连同更新 Handbook 与执行证据进入下一轮。实验主线覆盖四个研究问题：主效果（RQ1）显示相对种子 harness 与 baselines，RiseHarness 在两个模型上都提升了任务成功率与重复可靠性；消融（RQ2）支持 harness 执行证据与外部设计研究的贡献；行为分析（RQ3）显示不同模型保留的机制有不同行为目标与干预机制（如 DeepSeek-V4-Flash 上 completion confirmation 机制的逐步累积）；迁移实验（RQ4）检验冻结模型与 harness 后提升能否推广到未参与演化的 benchmark。实验还观察到中间版本性能有波动，说明重复评估与采纳门控是必要的。局限与未来方向包括：模型、任务分布与框架覆盖有限，演化评估成本高，需要进一步降低。整体上，论文代表“模型能力固定、系统层持续优化”的研究范式，与 FIRE、AgentDebug、AgentRx 等失败感知运行时干预工作形成直接对话。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文与你画像中的 agent 方向（权重 0.10）直接相关，属于“模型权重固定、系统层优化”这一与模型训练正交的研究路线。

## 基本信息

- 作者：Ruhan Wang, Yucheng Shi, Zongxia Li, Junyao Yang, Zhongzhi Li, Yue Yu, Haitao Mi, Dongruo Zhou, Leoweiliang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.27435`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索证据（introduction/results/conclusion 片段），对 heuristic_draft 中与论文不一致的噪声内容（audit-fence、self-consistency 等）做了剔除与标注；部分具体数值与实验细节因证据不足已标明需回原文核对。
