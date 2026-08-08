---
user_id: "cheng tan"
paper_id: 6948
arxiv_id: "2608.06270v1"
title: "The Illusion of Visual Tool-Use: A Causal Audit of Thinking with Images"
publish_date: "2026-08-06"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.06270v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.06270v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-08T13:59:20"
---
# The Illusion of Visual Tool-Use: A Causal Audit of Thinking with Images

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual tool use · causal audit · multimodal llm · crop-and-zoom

## 一句话总结

本文用因果干预框架审计多模态大模型的视觉工具使用（crop-and-zoom），提出 Visual Evidence Gain 这一 step-level 估计量，并在六个模型、五个细粒度感知基准上发现：工具使用虽有总体准确率提升，但大量 rollout 中返回的视觉观测并不真正因果影响答案，即存在“视觉工具使用的幻觉”。

## 摘要

> The "thinking-with-images" paradigm equips multimodal LLMs with active visual operations such as crop-and-zoom. However, models using these operations often achieve only marginal or negative gains over direct inference at substantially higher token cost. They may also repeatedly crop irrelevant regions and fail on questions that direct inference answers correctly. We ask whether the returned visual evidence causally affects the answer. To answer this question, we formulate visual tool-use as a causal graph that separates observation-mediated paths from action-induced shortcuts. We then audit it through interventions at the three levels: policy (comparing tool-use with direct inference), trajectory (corrupting all observations during rollout), and step (counterfactually replacing one individual observation under a fixed prefix). Our step-level estimand, Visual Evidence Gain, isolates the contribution of each returned observation. Across six representative models and five fine-grained perception benchmarks, we uncover policy miscalibration with two failure modes. In Calling Without Looking, returned observations have no causal effect on the answer. In Looking Without Planning, observations are informative but the call schedule is incoherent. A trajectory-level diagnostic decomposes the policy-level accuracy gain and shows that the gain is concentrated in a Calibrated minority. We term this discrepancy the illusion of visual tool-use: despite aggregate accuracy gains, visual tool-use is not causally effective across a broad range of rollouts. The code is available at https://github.com/OpenCausaLab/CauAudit.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的问题是：视觉工具使用（visual tool-use）看似让多模态大模型“看得更清楚”，但其实际收益往往不可靠、不稳定，且代价高昂。已有工作常用“返回了更高分辨率证据”来解释工具使用的收益，但缺少对因果机制的检验。论文的核心问题是：模型在调用视觉工具后返回的观测，到底是否真正因果地影响了最终答案？

具体来说，论文关注三类现象：
1. 边际收益悖论：工具使用相比直接推理只有边际甚至负向的准确率提升，但 token 成本显著更高。
2. 轨迹级行为异常：模型可能反复裁剪无关区域，说明调用策略本身可能“乱看”。
3. 直接推理可答对的样本反而在工具使用后失败，说明工具调用可能引入干扰或错误路径。

论文进一步将问题拆解为两个层面：一是“调用策略与观测内容之间是否匹配”（即模型是“先看再答”还是“先答再看”）；二是“观测是否真正进入答案生成的因果路径”。前者是策略层面的校准问题，后者是因果层面的有效性问题。作者提出需要一种可操作的因果审计方法，把“看起来在用工具”和“实际上被工具帮助”区分开。

需要指出，摘要与检索片段未给出具体基准名称与模型列表，因此“哪些任务最容易出现幻觉”“哪些模型更严重”等细节仍需回原文确认。

Q2: 有哪些相关研究？

根据 retrieval 命中的 Related Work 片段，相关工作主要分两条线：

1. Thinking with images 与视觉工具使用：
 - 一批工作给多模态 LLM 配备 crop-and-zoom 等主动视觉操作（OpenAI, 2025b; Su et al., 2025; Zhang et al., 2025b; Zheng et al., 2025; Wang et al., 2025b; Lai et al. 等）。这些工作通常主张通过调用工具获得更高分辨率或局部细节，从而提升细粒度感知能力。
 - 这类范式的共同假设是“看得更细 = 答得更准”，但论文指出该假设缺少因果验证。

2. 对视觉工具使用机制的反思与诊断：
 - 有工作（如 2025a 的某篇）描述过“thinking with images 的幻觉”：视觉动作看起来像 rationale，但与答案的 grounding 很弱，并尝试用 process-aware training 改进。
 - 本文的“视觉工具使用的幻觉”与上述工作不同：本文不是提出训练方法，而是提出一个诊断框架，用于量化每条观测的因果贡献。

此外，论文的 causal audit 方法显然受到因果推断（干预、反事实、路径分解）的启发，但在检索片段中没有详细列出相关因果方法文献。

总体看，论文站在“工具使用能力提升”与“可解释/可审计”两派工作的交叉点：既承认工具使用在某些样本上有效，也强调聚合指标掩盖了广泛 rollout 中的无效性。

Q3: 论文如何解决这个问题？

论文采用因果审计（causal audit）框架，核心思路是把视觉工具使用过程形式化为因果图，并区分两类路径：

- observation-mediated paths：观测真正作为证据进入答案生成，属于“看”的因果效应。
- action-induced shortcuts：调用动作本身或轨迹结构带来的伪相关，不经过观测内容即可影响答案。

在此基础上，论文设计三层干预：
1. Policy 层干预：比较工具使用策略与直接推理策略的准确率差异，得到聚合层面的增益。这是最常见的评估方式，但论文认为它只能反映平均效果，无法说明因果有效性。
2. Trajectory 层干预：在 rollout 过程中破坏（corrupt）所有观测，观察答案是否改变。如果破坏观测后答案几乎不变，说明观测没有进入因果路径。该层诊断可对 policy 层增益做分解。
3. Step 层干预：固定前缀，反事实地替换（counterfactually replacing）单个观测，估计该观测对答案的因果贡献。论文提出 Visual Evidence Gain（VEG）作为此层估计量，用于隔离每条返回观测的独立贡献。

轨迹层与步骤层的结合使得作者能将 policy 层准确率增益分解为“不同 rollout 的贡献”，从而识别出增益集中于少数已校准样本。

论文还定义了两个失败模式：
- Calling Without Looking（CWL）：调用了工具并返回了观测，但观测对答案无因果效应。
- Looking Without Planning（LWP）：观测本身有信息量，但调用时序不连贯、与问题需求不匹配。

整体方法强调“不改变模型参数，只做干预式审计”，因此适用于开源模型。

Q4: 论文做了哪些实验？

根据摘要与检索到的片段，论文的实验设计如下：

- 模型：六个代表性多模态大模型，且是开源的 thinking-with-images 模型（限制条件中提到所有实验都在开源模型上完成，因为干预需要访问内部状态）。
- 基准：五个细粒度感知 benchmark。摘要未给出具体名字，需要回原文确认。
- 干预协议：
 - policy 层：工具使用 vs 直接推理。
 - trajectory 层：破坏 rollout 中的所有观测。
 - step 层：固定前缀下替换单个观测，计算 Visual Evidence Gain。
- 分析维度：
 - 策略校准情况，识别 CWL 与 LWP 两类失败模式。
 - 用 trajectory 层诊断对 policy 层准确率增益做加性分解，观察增益是否集中在少数样本。
 - 额外观察：模型是否反复裁剪无关区域、是否在直接推理可答对的样本上失败，以及 token 成本差异。

受模型访问限制，论文无法对闭源模型做同样的干预实验。受限工具集：只研究 CROP-AND-ZOOM 操作，不包含图像分割、OCR、帧采样等其他视觉工具。

Q5: 发现了什么实验现象？

论文报告的主要实验现象：

1. 聚合增益的“幻觉”性：policy 层上，工具使用可能显示出总体准确率提升，但该提升并不能泛化为 rollout 层面的因果有效性；多数 rollout 中，visual tool-use 并不因果有效。
2. 边际/负收益与高 token 成本：工具使用相比直接推理往往只有边际甚至负向的准确率变化，但 token 成本显著更高。
3. 失败模式 CWL（Calling Without Looking）：模型确实调用了工具、返回了观测，但这些观测对最终答案没有因果作用，即“看了等于没看”。
4. 失败模式 LWP（Looking Without Planning）：观测本身包含信息量，但调用时序不连贯、与问题需求错位，导致信息没有被有效利用。
5. 增益集中性：trajectory 层诊断显示，policy 层准确率增益主要集中于少数已校准样本（Calibrated minority），绝大多数 rollout 的增益微弱或为负。作者由此提出“视觉工具使用的幻觉”：聚合指标会造成工具使用广泛有效的误导印象。
6. 行为异常：模型可能反复裁剪无关区域，并在直接推理能正确回答的问题上失败。

这些现象说明，问题的主要瓶颈不是“工具能力不足”，而是策略失校准（policy miscalibration）——即在何时调用、调用哪里、如何利用观测这些决策上，模型行为与“有效证据利用”不一致。

Q6: 有什么可以进一步探索的点？

论文点出的开放问题与可扩展方向：

1. 扩展工具集：当前只研究 CROP-AND-ZOOM，未来可覆盖图像分割、OCR、帧采样等更多视觉工具，检验因果审计结论是否跨工具成立。
2. 克服模型访问限制：当前干预只能在开源模型上实现，如何对闭源 API 模型做近似因果审计（例如通过输出扰动、黑盒反事实）是开放问题。
3. 从诊断到训练：已有工作提出 process-aware training 以缓解“thinking with images 的幻觉”，本文的诊断框架可作为训练信号或评估指标，推动策略校准训练。
4. 策略层面的规划机制：针对 LWP（看但不会规划）问题，可探索更好的调度、记忆或注意力机制，使观测在正确时间进入因果路径。
5. 因果方法精细化：Visual Evidence Gain 目前是 step 层估计量，可进一步做路径分解、中介分析，或考虑观测之间的时序依赖。
6. 跨任务与跨模态迁移：从细粒度感知 benchmark 扩展到更复杂的视觉问答、智能体任务，检验“增益集中于少数样本”的现象是否普遍。
7. 对 token 成本与信息效率的联合优化：既然大量观测不贡献因果，研究如何减少无效调用、降低推理成本。

Q7: 总结一下论文的主要内容

论文围绕“thinking-with-images”这一范式展开因果审计。所谓 thinking-with-images，是指让多模态大模型在推理时主动调用 crop-and-zoom 等视觉操作，以获取局部高分辨率证据。该范式在直觉上能提升细粒度感知，但已有观察显示：工具使用往往只带来边际甚至负向的准确率变化，却显著增加 token 消耗；模型可能反复裁剪无关区域，并在直接推理本可答对的样本上失败。论文由此提出核心质疑：返回的视觉证据是否真的因果地影响了答案？

为了回答这个问题，作者将视觉工具使用建模为因果图，把每条轨迹的路径分为“观测中介路径”（observation-mediated paths）与“动作诱导捷径”（action-induced shortcuts），并设计了三个层级的干预：policy 层比较工具使用与直接推理；trajectory 层在 rollout 中破坏所有观测；step 层在固定前缀下反事实替换单个观测。step 层对应的估计量 Visual Evidence Gain（VEG）用于隔离每条观测的因果贡献。

在六个代表性模型和五个细粒度感知基准上，论文发现策略失校准是核心瓶颈，具体表现为两种失败模式：Calling Without Looking（观测无因果效应）与 Looking Without Planning（观测有信息但调用规划不连贯）。trajectory 层诊断进一步揭示，policy 层的总体准确率增益是真实的，但集中于少数已校准样本；在更广泛的 rollout 中，视觉工具使用并不因果有效。作者将这种聚合增益与 rollout 级无效性之间的错位称为“视觉工具使用的幻觉”（the illusion of visual tool-use）。

论文的贡献包括：首次将视觉工具使用置于因果审计框架下；提出三层干预协议与 VEG 估计量；识别出两类可操作的失败模式；给出一个 trajectory 级诊断来分解 policy 级增益；并开源代码（https://github.com/OpenCausaLab/CauAudit）。局限性方面，实验限于开源模型与单一工具 crop-and-zoom，未覆盖分割、OCR、帧采样等操作，也无法对闭源模型做内部干预。总体而言，论文的价值在于把“工具使用是否有效”从聚合指标问题转化为可检验的因果问题，并为后续的策略校准训练与评测提供了新的诊断工具。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对评估“模型是否真的在使用工具/信息”这一命题有直接方法论价值，其因果审计思路可迁移到智能体工具调用评测。

## 基本信息

- 作者：Zhiheng Wang, Bo Peng, Lai Wei, Chaochao Lu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-06
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.06270v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 49 个片段，其中有效信息来自 Abstract、Related Work、Discussion、Conclusion 与 Limitations；正文实验细节缺失，因此具体模型名、基准名与数值均未臆造，需回原文确认。
