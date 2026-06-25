---
user_id: "cheng tan"
paper_id: 1245
arxiv_id: "2606.23112"
title: "Self-Evolution for Multi-Turn Tool-Calling Agents via Divergence-Point Preference Learning"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23112.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23112"
abs_url: "https://arxiv.org/abs/2606.23112"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:23:20"
---
# Self-Evolution for Multi-Turn Tool-Calling Agents via Divergence-Point Preference Learning

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multi-turn tool calling · self-evolution · tool graph · divergence point preference learning

## 一句话总结

提出 ToolGraph 图引导推理与发散点偏好学习（DPO）相结合的方法，在 tau2-bench 上通过自演进实现多轮工具调用智能体的性能提升，加权平均奖励从 0.304 提升至 0.355（+16.8%）。

## 摘要

> Multi-turn tool-using agents must coordinate long-horizon tool sequences while tracking dialogue state and policy constraints. Existing approaches often separate inference-time orchestration from parameter-level learning, leaving tool selection weakly structured and preference updates vulnerable to train--deployment prompt mismatch. For within-benchmark self-improvement, ToolGraph combines schema-derived topology, transition weights estimated from successful rollouts, and history-aware controls for write prerequisites and repeated-search loops. We then construct 161 preference pairs by locating divergence points via state-based matching and prefix-based alignment, filtered through action-correctness annotations, and train DPO under the same ToolGraph context used at inference. Across 375 tau2-bench tasks, ToolGraph raises the weighted average reward from 0.304 to 0.338 (+11.2% relative), while ToolGraph+DPO reaches 0.355 (+16.8% over the baseline), with the DPO gain concentrated in airline and retail. Fine-grained diagnostics further show that roughly half of telecom trajectories exhaust the step budget before action execution and that chosen reward positivity is the most useful checkpoint signal across our 16 evaluated DPO configurations.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决多轮工具调用智能体中两个核心问题：(1) 推理时编排（如图搜索、重排序）与参数级学习（如 DPO 微调）的分离，导致工具选择缺乏结构化引导，且偏好更新容易因训练和部署提示的不匹配而产生不一致；(2) 智能体如何从自身的执行轨迹中学习（自演进），同时避免将偶发的轨迹差异误判为偏好信号，尤其是参数级更新对噪声敏感。目标是在固定的基准环境（tau2-bench）中，利用智能体自己的历史 rollout 数据，通过改进推理策略和参数微调，持续提升多轮工具调用的可靠性和奖励。

Q2: 有哪些相关研究？

（合理推断）相关研究包括：(1) 工具调用智能体框架：如 ReAct、Toolformer、WebGPT，这些方法通常将工具使用视为上下文学习或简单的 API 调用，缺乏对工具间依赖和历史轨迹的结构化建模；(2) 推理时引导技术：如 Tree-of-Thought、Beam Search、Graph-of-Thought，将推理过程视为图搜索，但往往不结合参数学习；(3) 偏好学习与 DPO：在对话、摘要、代码生成等领域应用广泛，但直接用于工具调用场景的上下文对齐研究较少；(4) 自演进智能体：通过自我博弈、强化学习等方法自我改进，但需要大量交互数据且对提示变化敏感；(5) 与 tau2-bench 相关的工作：该基准专门评估多轮工具调用，已有一些基线如 GPT-4、Qwen 等，但本文是首个在自改进背景下系统结合图推理和 DPO 的工作。

Q3: 论文如何解决这个问题？

论文提出 ToolGraph 框架和 DPO 自改进方法，具体包括：
1. **ToolGraph 图引导推理**：
 - 为每个领域根据工具模式（schema）构建有向权重图，节点对应工具（如 API），边表示调用依赖关系（如写操作的前置条件）。
 - 边权重从成功的 rollout 轨迹中统计得出，反映工具转移的频率或可靠性。
 - 推理时使用历史感知的波束搜索（beam scoring）：不仅考虑当前状态，还考虑对话历史中的写操作等约束，加入领域特定的写前提控制和重复搜索循环检测。
2. **发散点偏好学习（Divergence-Point Preference Learning）**：
 - 在同一批 rollout 数据中，通过基于状态的匹配和前缀对齐找到两条轨迹的分歧点（divergence point）。
 - 在分歧点处，根据后续动作的正确性（如是否导致成功完成任务）标注 chosen 和 rejected 动作。
 - 过滤得到 161 个偏好对，使用与推理时相同的 ToolGraph 上下文作为提示条件，训练 DPO 模型（基于 Qwen3.5-9B）。
3. **训练与推理对齐**：DPO 的训练上下文（prompt 中包含 ToolGraph 信息）与部署时的推理上下文一致，以缓解 prompt 不匹配问题。

Q4: 论文做了哪些实验？

实验在 tau2-bench 基准上进行，包含四个领域：航空（50 任务）、电信（114 任务）、零售（114 任务）、银行（97 任务），共 375 个任务。基础模型为 Qwen/Qwen3.5-9B，通过 vLLM 部署在 HPC2 A800 GPU 上。比较三种方法：基线（无引导）、ToolGraph（仅推理时图引导）、ToolGraph+DPO（推理时引导 + 参数微调）。评估指标为加权平均奖励（reward），并报告各领域的详细结果。此外，进行了 16 种 DPO 超参数配置的消融实验，分析不同选择信号（如 chosen reward positivity、margin 等）和检查点策略的影响。

Q5: 发现了什么实验现象？

主要实验现象包括：
- **整体性能提升**：ToolGraph 将加权平均奖励从 0.304 提升至 0.338（+11.2%相对提升）；ToolGraph+DPO 进一步达到 0.355，相比基线提升 16.8%。
- **领域特异性增益**：DPO 的额外提升主要集中于航空（airline）和零售（retail）领域，在电信和银行领域改善不明显。
- **电信领域预算耗尽问题**：细粒度诊断显示，约一半的电信任务轨迹在成功执行动作之前就已经耗尽了最大步骤预算，说明电信任务对工具调用效率和策略规划要求更高，可能限制了 DPO 的有效性。
- **DPO 配置影响**：在所评估的 16 种 DPO 配置中，使用“所选奖励正性”（chosen reward positivity）作为检查点信号的配置表现最佳，表明偏好对中选中的动作本身的正向信号比相对差距更重要。
- **自改进性质**：所有边权重和 DPO 偏好对均来自智能体自身的先前 rollout，因此结果是基准内自改进的体现，而非依赖外部数据或更大模型。

Q6: 有什么可以进一步探索的点？

（合理推断）可进一步探索的方向包括：(1) 跨域泛化：当前方法仅在同一基准内自改进，未来可研究如何将在一个域中学到的 ToolGraph 或偏好模型迁移到新域；(2) 解决电信领域步骤预算耗尽问题：可能需要对电信任务设计更高效的图搜索策略或早停机制；(3) 扩展到更大模型：当前使用 9B 模型，在更大规模模型上的表现和训练效率有待验证；(4) 在线学习与持续演进：探索在部署过程中持续收集新数据并更新 DPO 策略，实现终身学习；(5) 更复杂的偏好构造：除了当前的分歧点方法，可尝试更细粒度的奖励建模或使用模型自我评估来扩展偏好对数量；(6) 与强化学习（如 PPO）结合：比较 DPO 与基于 RL 的方法在工具调用自演进中的优劣；(7) 对提示复杂度的鲁棒性：研究当用户指令或 API 模式发生变化时，图结构和 DPO 策略如何自适应。

Q7: 总结一下论文的主要内容

本文提出了一种结合图引导推理（ToolGraph）和发散点偏好学习（DPO）的方法，用于多轮工具调用智能体的基准内自改进。论文首先指出现有多轮工具调用方法将推理时编排与参数级学习分离，导致工具选择缺乏结构且偏好更新受提示不匹配影响。为解决该问题，论文构建了 ToolGraph：基于领域工具模式构建有向图，边权重从成功 rollout 中统计，并在推理时使用历史感知波束搜索，处理写前提和循环检测。然后，在相同 ToolGraph 上下文中，通过状态匹配和前缀对齐定位轨迹发散点，根据后续动作正确性构建 161 个偏好对，训练 DPO 模型（基于 Qwen3.5-9B）。实验在 tau2-bench 的四个领域（航空、电信、零售、银行）共 375 个任务上进行。结果表明：ToolGraph 单独使用将加权平均奖励从基线 0.304 提升至 0.338（+11.2%），而 ToolGraph+DPO 进一步达到 0.355（+16.8%）。DPO 增益主要集中于航空和零售领域。细粒度诊断显示电信领域约一半轨迹在动作执行前耗尽预算，且 chosen reward positivity 是最有效的 DPO 检查点信号。论文的工作量较大，系统性结合了推理时引导与参数微调，且验证了自改进的可行性。主要贡献包括：提出 ToolGraph 图引导推理框架、设计发散点偏好构造方法、在 tau2-bench 上实证了自改进效果。局限在于 DPO 增益领域不均、电信预算问题未解决、仅评估单一 9B 模型。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：论文主题与智能体方向直接相关（用户画像中 agent 权重 0.10），特别是多轮工具调用和自改进。

## 基本信息

- 作者：Jiaqiang Tang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.AI, cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23112`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 检索证据（abstract 和 introduction 的语义匹配片段），并结合 heuristic_draft 进行结构化补充；部分字段（如 related_work、future_directions）因缺乏直接证据，基于全文上下文做了合理推断。
