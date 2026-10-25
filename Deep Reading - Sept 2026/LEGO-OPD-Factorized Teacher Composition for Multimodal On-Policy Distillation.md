---
user_id: "cheng tan"
paper_id: 14507
arxiv_id: "2610.00333v1"
title: "LEGO-OPD: Factorized Teacher Composition for Multimodal On-Policy Distillation"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2610.00333v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.00333v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T13:04:50"
---
# LEGO-OPD: Factorized Teacher Composition for Multimodal On-Policy Distillation

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：on-policy distillation · multimodal reasoning · teacher composition · visual grounding

## 一句话总结

LEGO-OPD 提出一种因子化教师组合的多模态 on-policy 蒸馏方法：用 LLM 提供语言先验、VLM 提供视觉似然比，通过广义贝叶斯更新把二者组合成单一教师分布，并引入基于源参考的自适应校准系数，从而在提升视觉接地（visual grounding）的同时保持甚至改善纯文本推理能力。

## 摘要

> Multimodal on-policy distillation (OPD) aims to improve visual grounding while preserving the strong reasoning capabilities of language models. Recent multi-teacher approaches combine LLM and VLM teachers to provide complementary supervision. However, directly using a VLM's full predictive distribution entangles its visual grounding signal with its own language prior, preventing the grounding information from being transferred independently. Conversely, increasing the strength of visual supervision can improve perception but may overemphasize visual evidence and degrade language reasoning. To address this trade-off, we introduce LEGO-OPD, which selectively composes factors from a Language Expert and a Grounding expert into One teacher distribution for multimodal OPD. Under a generalized Bayesian formulation, the language expert provides a prior over candidate tokens, while the grounding expert contributes a visual likelihood that updates this prior, rather than transferring its complete predictive distribution. This factorized composition allows language reasoning and visual grounding to be controlled independently. We further introduce adaptive calibration to determine how strongly the visual likelihood should update the language prior at each decoding prefix. Specifically, LEGO-OPD uses the grounding expert's image-induced prediction shift as a prefix-dependent reference, preventing both insufficient and excessive visual supervision. Experiments with Qwen3 models show that LEGO-OPD consistently outperforms the evaluated single- and multi-teacher OPD baselines on both multimodal and text-only reasoning tasks. Moreover, it improves the initial student's visual perception while preserving text-only reasoning.

Q1: 这篇论文试图解决什么问题？

这篇论文要解决的问题可以拆成三层：

1. **多模态 OPD 的教师信号组合问题**。On-policy distillation 用教师在学生自生成前缀处的预测来训练学生 VLM，其有效性同时依赖两方面：可靠的视觉接地（visual grounding）和对已接地信息的强推理。现有两条路线各有缺陷：
 - 多教师路线（外部 LLM 做推理教师 + VLM 做多模态教师）直接组合二者的完整预测分布，但每个教师的预测分布都混合了它自己的语言先验与接地信号，二者纠缠后无法选择性地迁移互补优势；
 - 增强视觉分量路线（强化蒸馏信号中的视觉成分）虽然能提升感知，但过度强调视觉证据可能扭曲底层语言先验，使推理性能下降（论文将此描述为 grounding 与 reasoning 之间的 trade-off）。

2. **缺少对“语言先验”与“视觉证据”的独立控制机制**。理想方案应当能分别控制语言推理与视觉接地，再把二者组合成连贯的教师分布；现有基线都没有实现这种因子化分解。

3. **视觉监督强度缺少自适应调节**。即使有了因子化组合，每个解码前缀（prefix）上视觉证据应以多大权重影响预测并不是常数：某些前缀（如题目陈述、纯语言推理步骤）几乎不需要图像信息，另一些前缀（如图表读数、几何图形判断）高度依赖图像。固定权重要么在需要视觉处监督不足，要么在纯语言处过度扭曲语言先验。因此需要前缀相关的自适应校准，且该校准应有一个可靠的参考尺度。

4. **评估维度分离问题（隐含）**。多模态 OPD 的常见评估只关注多模态 benchmark 平均分，容易掩盖“视觉增强但语言退化”的负向迁移；本文强调要在多模态推理、纯文本推理、视觉感知三个维度分别评测，以验证蒸馏确实实现联合迁移而非零和交换。

核心矛盾一句话：**视觉证据与语言先验在教师分布中被纠缠，而任何试图增强前者的操作都可能破坏后者；需要一种能独立控制二者、并按前缀自适应调节强度的教师组合机制。**

Q2: 有哪些相关研究？

根据论文引言、方法与结论中的线索，相关研究脉络可归纳如下（部分为合理推断，因为检索证据未覆盖完整 Related Work 章节）：

1. **On-policy distillation（OPD）框架**：Agarwal et al., 2024 与 Yin et al., 2026 等提出在 student 生成的 prefixes 上蒸馏教师预测，作为 VLM 多模态推理的 post-training 框架。这是本文方法所基于的训练范式（distilling teacher predictions at student-generated prefixes）。

2. **多教师互补监督路线**：Kwai Keye Team et al., 2026 与 Yin et al., 2026 等提出组合外部 LLM（负责推理）与 VLM 教师（负责多模态指导）来提供互补监督。其中 H-OPD（Yin et al., 2026）与本工作直接可比：本文实验数据就是 H-OPD 发布的 MMFineReason-123K 的 54,840 样本子集，并与之对比。论文指出这类方法直接组合完整预测分布，导致语言先验与接地信号纠缠。

3. **强化视觉监督路线**：Liu et al., 2026、Liang et al., 2026、Yoon et al., 2026 等通过加强蒸馏信号的视觉成分来提升 grounding；本文指出过度强调视觉证据可能扭曲语言先验、损害推理。

4. **多教师蒸馏与分布组合的一般方法（推测）**：多教师 KD 通常用加权平均或 product-of-experts 式组合；本文的广义贝叶斯组合（language prior + visual likelihood）可视为这些方法的一个特例或泛化，其中 α=0 退化为纯语言教师，α 增大则逼近 grounding 教师主导。文中提到的 top-16 OPD 设置来自 Li et al., 2026，说明 token-level 的 top-k 蒸馏也是相关技术背景。

5. **贝叶斯更新 / 先验-似然分解的蒸馏思想**：论文用 generalized Bayesian formulation（evidence score s 与 evidence scale α 的 KL 约束更新）推导教师分布，这一形式化与概率推理中先验加似然的结构有关（合理推断；证据片段中的 Eq. (1)-(2) 显示这是其理论核心）。

6. **VLM 视觉感知与推理解耦评估**：本文分别用多模态推理、纯文本推理与视觉感知 benchmark 评测，相关背景包括 MATH-Vision、MathVerse、MMMU-Pro、MathVista、RBench-M、OlympiadBench-MM、MATH-500、MMLU-Pro、RBench-T 等基准体系。

总体定位：本文处于“多教师 OPD”与“视觉增强 OPD”两条路线的交叉点，用因子化贝叶斯组合回应前者的纠缠问题，用自适应校准回应后者的过度视觉化问题。

Q3: 论文如何解决这个问题？

LEGO-OPD 的方法由三个紧密耦合的模块组成：

**1. 广义贝叶斯组合框架（理论基础）**
论文先给出一个一般化更新规则：给定语言先验 p0、有限证据分数 s(v) 与证据尺度 α≥0，更新分布为 q*_α = argmax_q { α E_{v∼q}[s(v)] − D_KL(q‖p0) }，其闭式解为 q*_α(v) = p0(v) exp{α s(v)} / Z(α)。这个形式把“先验 + 加权证据”统一为指数族更新，是后续因子化组合的依据。

**2. 因子化教师组合（Factorized Teacher Composition）**
核心符号定义：
- πL：冻结的 Language Expert（LLM）策略；qL,t(v) := πL(v | h_t)，h_t 包含系统提示、问题与学生生成前缀 y<t；
- πV：冻结的 Grounding Expert（VLM）策略；qIV,t(v) := πV(v | I, h_t)（有图像条件）；q0V,t(v) := πV(v | ∅, h_t)（无图像条件）；
- 三者共享词表 V，在相同文本上下文上评估。

组合的关键动作是**不用 VLM 的完整预测分布**，而是用其有图/无图对数概率差构造视觉残差：r_t(v) = zIV,t − z0V,t（logit 空间）。语言专家提供先验 qL,t，视觉残差作为证据项，组合分布为 qT,t = softmax(zL,t + α_t (zIV,t − z0V,t))。分支相关的 log-normalizer 在最终 softmax 中抵消，因此 logit 空间与概率空间组合等价；实现上出于数值稳定性使用 logit 空间（原文：For numerical stability, the implementation…，未完整给出，合理推断为 logit 空间计算）。

该式在概率空间等价于 qT,t(v) ∝ qL,t(v) exp{α_t r_t(v)}，与广义贝叶斯更新式 (1)-(2) 严格对应：语言先验由 πL 提供，视觉证据由 πV 的图像诱导偏移提供。α_t=0 时退化为纯语言教师；α_t 增大时视觉证据影响增强。

**3. 源参考自适应校准（Source-Referenced Adaptive Calibration）**
- 关键设计：视觉似然对语言先验的更新强度 α_t 不能是常数，而应随解码前缀变化；
- 参考尺度：用 grounding expert 自身由图像引起的预测偏移（image-induced prediction shift，即有图与无图条件下的预测差异，如 qIV 与 q0V 的差异）作为前缀相关参考；
- 校准目标：既防止视觉监督不足（α 过小，grounding 信号传不进去），也防止视觉监督过度（α 过大，扭曲语言先验损害推理）；
- 闭式系数：通过局部 KL 近似（local KL approximation）推导出 α_t 的闭式形式（Eq. (13)），即校准系数可从专家分布/对数的局部结构直接算出，无需额外训练；论文还用数值 KL-matching 参考评估了该近似的误差（结论中明确提到）。

**4. 学生训练目标**
组合后的教师分布 qT,t 作为 reverse-KL OPD 目标（Eq. (3)）：学生 πθ 在同一个前缀 h_t（含学生自生成前缀）上匹配教师分布，采样前缀与教师目标固定（relevance 证据片段：We use this composed teacher distribution as the target for the reverse-KL OPD objective in Eq. (3), holding the sampled prefixes and teacher targets fixed）。训练中所有学生参数更新，前缀来自学生 rollout（temperature 1.0，无 top-p/top-k 截断）。

**5. 方法解释（Interpretation）**
论文进一步把学生预测写成与教师组合类似的因子化形式，用 p0θ,t(v) := πθ(v | ∅, h_t) 表示学生的语言（无图）预测，以对比学生内部的先验-似然结构与教师组合结构，从而解释两个因子各自的贡献（证据片段 score=0.867）。

一句话概括技术路线：**用 LLM 的分布做先验、用 VLM 的有图-无图 logit 差做视觉似然、用 VLM 自身的图像诱导偏移做前缀级自适应权重，然后合成单一教师分布做 reverse-KL on-policy 蒸馏。**

Q4: 论文做了哪些实验？

实验设计可分为训练设置、评估体系、对比基线与消融四部分：

**1. 模型与数据**
- 学生：Qwen3-VL-2B-Instruct；
- Language Expert：Qwen3-4B-Instruct-2507（LLM）；
- Grounding Expert：Qwen3-VL-4B-Instruct（VLM）；
- 训练数据：H-OPD 发布的 MMFineReason-123K 中的 54,840 样本子集（该子集支持与 H-OPD 对比，且包含其语言教师所需的图像描述）。

**2. 训练配置**
- 所有实验（含消融）共用训练实现、学生初始化和优化设置，仅方法相关部分不同（保证公平对比）；
- 200 步训练，AdamW，lr=1e-6，无 warmup，global batch size 128；
- prompt 与 response 长度上限 12,384 token；
- rollouts 用 temperature 1.0，无 top-p/top-k 截断；
- 采用 Li et al., 2026 的 top-16 OPD 设置（合理推断：每步蒸馏 top-16 token 的分布而非全词表）。

**3. 评估体系（三维分离）**
- 多模态推理：MATH-Vision、MathVerse、MMMU-Pro、MathVista、RBench-M、OlympiadBench-MM；
- 纯文本推理：MATH-500、MathVerse-Text Only、MMLU-Pro、RBench-T 等；
- 视觉感知：独立评测（Table 1 中另有视觉感知 benchmark，检索片段未给出具体名称，需回原文确认）；
- 答案语义解释使用生成式 AI 作为评估管线的一部分（Appendix C.3，结论声明中提及）。

**4. 对比基线**
- 单教师 OPD 基线（具体名字未在检索片段中出现，合理推断包括 LLM-only 教师、VLM-only 教师等变体）；
- 多教师 OPD 基线（包括 H-OPD 这一直接可比方法）；
- 固定权重（non-adaptive）版本作为自适应校准的对照。

**5. 消融与验证实验**
- 语言先验与视觉残差因子的互补性消融（结论中明确提到：language prior and visual residual provide complementary benefits）；
- 自适应校准 vs 固定权重：比较两种模式下多模态与纯文本推理平均分；
- 更强调视觉（stronger visual emphasis）条件下自适应校准是否仍不损害推理；
- 校准系数闭式近似的误差评估：以数值 KL-matching 为参考；
- 完整结果在 Table 4 中展开（证据片段提及），说明 Table 4 给出了各因素效应的细化结果。

具体数值细节（如各 benchmark 的完整分数表）在检索证据中未完整覆盖，需要回原文 Table 1-4 核对。

Q5: 发现了什么实验现象？

根据结论与检索到的 results 片段，实验现象与规律如下：

1. **主结果：全面优于基线**。在 Qwen3 模型族上，LEGO-OPD 在评估过的单教师与多教师 OPD 基线中取得多模态与纯文本推理的平均最佳成绩（结论明确陈述）。

2. **多模态增益与文本几乎不变的模式**。检索到的 results 片段给出一个代表性数据点“56.25 (+0.24)”，并描述为“multimodal gains with little change in text-only performance”（多模态提升而纯文本几乎不变）。这一 pattern 被论文解读为语言推理与视觉接地被联合迁移到学生（joint transfer）的证据，而不是视觉增强换语言退化的零和结果。

3. **因子互补性**。消融显示语言先验与视觉残差两个因子提供互补收益：二者同时存在优于各自单独使用。

4. **自适应校准优于固定权重**。自适应（source-referenced）校准在两类推理平均分上都超过固定权重；并且自适应方案允许更强的视觉强调（stronger visual emphasis）而不导致推理退化。这说明固定权重下“增强视觉→损害推理”的 trade-off 在自适应机制下被部分打破——这是最值得注意的结果：问题不在于视觉监督的绝对强度，而在于强度是否与前缀的视觉需求匹配。

5. **初始学生感知能力提升**。视觉感知评估显示 LEGO-OPD 提升了初始学生（Qwen3-VL-2B）的视觉感知，同时保持纯文本推理——即“感知增强”不是以“语言能力下降”为代价。

6. **近似误差可控（隐含）**。局部 KL 近似得到的闭式校准系数与数值 KL-matching 参考的误差被评估过，说明闭式系数虽为近似但经验上足够可靠（具体误差数值未在检索片段中给出）。

7. **指标间张力与对应解释**。论文刻意把多模态推理、纯文本推理、视觉感知分开报告，暗示平均分提升可能掩盖内部维度差异；其核心经验规律是：多模态收益 + 文本平稳 + 感知提升三者同时成立，才是“联合迁移”而非“权衡转移”。

需要注意的是：Table 4 的完整消融数字、单个 benchmark 的涨跌细节、以及是否存在某些 benchmark 上下降的情况，在检索证据中没有覆盖，不能断言所有子任务都提升。

Q6: 有什么可以进一步探索的点？

基于论文框架与已知局限（Appendix F 中讨论，内容未在检索片段中给出），可进一步探索的方向包括：

1. **跨模型与跨规模泛化**：本文只用 Qwen3 模型族（2B 学生 + 4B 教师）。值得验证 LEGO-OPD 在更大/更小规模、不同架构（如 Llama、Gemini 系 VLM）、不同词表对齐条件下是否保持优势；特别是语言专家与接地专家词表不一致时的组合策略。

2. **校准机制的更细粒度设计**：
 - 当前 α_t 是前缀级（per-prefix）标量；可探索 per-token、per-head 或基于注意力/视觉特征的自适应权重；
 - 图像诱导预测偏移目前来自 VLM 自身的有图/无图对比；可研究更丰富的参考量（如不确定性、视觉 token 的置信度、OCR 文本存在性）作为校准信号；
 - 闭式局部 KL 近似与数值 KL-matching 的误差边界可进一步理论刻画，并探索更高阶修正。

3. **更广的教师因子分解**：除“语言先验 + 视觉似然”外，可加入其他因子（如外部工具/代码执行证据、领域知识库、安全/格式约束），推广到多因子贝叶斯组合；并辨析哪些因子应进先验、哪些应进似然、哪些应做证据缩放。

4. **训练动态与在线校准**：OPD 的 rollout 前缀随学生训练而变化，图像诱导偏移参考也随之漂移；可以研究 α_t 在训练过程中的演化规律、是否收敛，以及校准信号自身是否需要课程式调整。

5. **推理时应用**：因子化组合当前用于训练期蒸馏；同样机制可直接用于 inference-time 的专家组合（类似 test-time scaling），或在偏好优化（如 RLHF/DPO 变体）中作为参考分布。

6. **评估体系扩展**：目前视觉感知评估的具体 benchmark 未在检索片段中明确；可补充细粒度接地任务（如指代表达、视觉问答定位）、对抗性视觉干扰、以及多图/视频输入下的表现。

7. **理论深化**：广义贝叶斯更新中的证据分数 s(v) 与尺度 α 的语义可进一步联系到 calibrated likelihood、temperature scaling 与 LLM-as-Bayesian 的既有理论；也可分析 reverse-KL 训练下学生先验/似然因子如何随训练解耦。

8. **应用方向（结合用户画像）**：若用于 AI-for-Science 场景（如科学图表理解、显微图像、分子结构图的视觉推理），可探索将领域 VLM 作为 grounding expert、将推理强 LLM 作为 language expert 的组合，并针对科学图像的前缀级视觉依赖模式调校自适应校准。

Q7: 总结一下论文的主要内容

**问题主线**：多模态 on-policy distillation（OPD）需要在学生自生成前缀上蒸馏教师预测，以同时提升视觉接地与语言推理。但现有两类方法都有结构性缺陷：多教师方法（LLM+VLM）直接组合完整预测分布，把每个教师的语言先验与接地信号纠缠在一起，无法独立迁移互补优势；增强视觉监督的方法虽改善感知，却可能过度强调视觉证据、扭曲语言先验并损害推理。由此引出核心需求：一个能分别控制语言推理与视觉接地、并能按解码前缀自适应调节视觉贡献的蒸馏框架。

**技术主线**：LEGO-OPD 给出三段式方案。
- 第一，广义贝叶斯形式化：给定先验分布 p0、有限证据分数 s 与证据尺度 α，最优更新为 q*_α = argmax{αE[s] − D_KL(q‖p0)}，闭式解 q*_α(v) ∝ p0(v) exp{αs(v)}，把“先验+加权证据”统一进指数族框架。
- 第二，因子化教师组合：语言专家 πL（Qwen3-4B-2507）提供语言先验 qL,t；接地专家 πV（Qwen3-VL-4B）不提供完整预测分布，而是提供其有图/无图 logit 差 zIV,t − z0V,t 作为视觉似然残差；组合教师为 qT,t = softmax(zL,t + α_t(zIV,t − z0V,t))（logit 空间与概率空间等价，因分支 log-normalizer 在 softmax 中抵消）。α=0 时退化为纯语言教师，从而语言推理与视觉接地被解耦控制。
- 第三，源参考自适应校准：α_t 由 grounding expert 自身的图像诱导预测偏移（有图 vs 无图分布差异）作为前缀相关参考确定，通过局部 KL 近似得到闭式系数（Eq. (13)），以防止视觉监督不足或过度；近似的可靠性用数值 KL-matching 参考评估。
- 学生训练：冻结教师，在固定学生采样前缀上最小化 reverse-KL OPD 目标 Eq. (3)；并给出学生预测的因子化解释（p0θ,t = πθ(·|∅,h_t)）以揭示两个因子的贡献。

**实验主线**：学生 Qwen3-VL-2B-Instruct，语言教师 Qwen3-4B-Instruct-2507，接地教师 Qwen3-VL-4B-Instruct；数据为 MMFineReason-123K 的 54,840 子集；200 步 AdamW（lr=1e-6，batch 128，无 warmup），rollout temperature 1.0，top-16 OPD 设置。评估分三维：多模态推理（MATH-Vision、MathVerse、MMMU-Pro、MathVista、RBench-M、OlympiadBench-MM）、纯文本推理（MATH-500、MathVerse-Text Only、MMLU-Pro、RBench-T 等）与视觉感知。主要发现：
- LEGO-OPD 在单/多教师 OPD 基线中取得多模态与纯文本推理的平均最佳；
- 代表性结果是多模态增益伴随纯文本几乎不变（如某任务 56.25 (+0.24)），被解读为语言推理与视觉接地的联合迁移；
- 消融显示语言先验与视觉残差互补，自适应校准优于固定权重，且在更强的视觉强调下仍不损害推理；
- 初始学生的视觉感知得到提升而纯文本推理保持。

**结论与局限**：LEGO-OPD 提供了一种平衡多模态蒸馏中语言推理与视觉接地的有效框架；局限在 Appendix F 中讨论（具体内容未在检索证据中给出）。复现细节由 Section 4.1 与附录 B、C.1、C.3 提供；论文也声明了 AI 工具用于稿件修改与评估管线中的语义答案判分。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 on-policy distillation / LLM-VLM 多教师蒸馏研究直接相关：本文是 H-OPD（Yin et al., 2026）之后在同一数据子集（MMFineReason-123K 54,840 子集）上的直接推进，可作对比阅读。

## 基本信息

- 作者：Jaeyun Shin, Hangeol Chang, Jong Chul Ye
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.00333v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索证据（method、introduction、results、conclusion 片段）与字段-证据映射；部分数值、表格与附录 F 细节在证据中缺失，已在相应字段明确标注需回原文核对。
