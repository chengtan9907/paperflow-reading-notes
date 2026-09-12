---
user_id: "cheng tan"
paper_id: 10405
arxiv_id: "2609.02088v1"
title: "Rendering-in-the-Loop: An Execution-Driven Agent for Interactive Web Development"
publish_date: "2026-09-02"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Sept 2026/2609.02088v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.02088v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-09-05T01:30:42"
---
# Rendering-in-the-Loop: An Execution-Driven Agent for Interactive Web Development

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal web development · execution-driven agent · rendering in the loop · webpage interaction verification

## 一句话总结

RILA 是一个把浏览器真实渲染放进循环的"执行—批评—编辑"智能体：在真实浏览器中重放参考交互轨迹并逐动作验证生成网页，用同时衡量交互正确性与视觉保真度的 Execution-aware Rendering Score 指导迭代改码，并配套 execution-verified 数据合成流水线；在 IWR-Bench 上把 Qwen3.5-9B 从 40.40% 提升到 57.52%，超过 1T 参数 Kimi-K2.6 与专有 GPT-5.5 的单次生成结果。

## 摘要

> Multimodal large language models have achieved remarkable progress in front-end web development, generating interactive webpages from multimodal references such as screenshots and interaction videos. However, existing work largely emphasizes visual metrics such as aesthetics and layout similarity, while overlooking the more critical validation of interactive functionality. We present RILA, an execution-driven agent that puts browser rendering in the loop, iteratively editing generated code from runtime interaction feedback. RILA introduces an Action Interaction Verification (AIV) module that replays the reference interaction trajectory on the generated webpage to collect grounded execution-aware observations, and an Execution-aware Rendering Score (ERS) that jointly measures interaction correctness and visual fidelity to guide iterative optimization. We further build an execution-verified data synthesis pipeline that produces diverse, high-quality training data, offering gains complementary to inference-time optimization. On IWR-Bench, RILA consistently improves both interaction and visual fidelity across foundation models. Notably, with our training pipeline, RILA lifts the compact Qwen3.5-9B backbone from 40.40% to 57.52%, surpassing far larger one-shot generators, including the 1T-parameter Kimi-K2.6 (55.61%) and the proprietary GPT-5.5 (55.74%).

Q1: 这篇论文试图解决什么问题？

### 问题定位
这篇论文处理的是"多模态网页开发"中的可靠性问题：给定截图、线稿或交互视频等参考，生成包含 HTML/CSS/JavaScript/外部资源的可交互网页。论文把"一个网页可用"拆成两个相互独立但有张力的要求：视觉保真度（渲染结果是否贴近参考外观）与交互正确性（在用户动作下页面行为是否正确）。已有路线往往只优化其中一个，导致"看起来合理、用户一点就坏"或"行为恢复但视觉很糟"的结果。

### 已有路线各自漏掉什么
1. 一次生成式多模态网页生成方法：以截图/视频为条件直接生成代码，用 visual/layout 相似度训练或评估，缺少对执行行为的显式校验；2. 专门的前端修复方法：像 Yuan et al. 2025 那样对照设计规范审计渲染结果，规范本身不包含"用户动作后会发生什么"，因此仍无法覆盖运行时交互失败；3. 软件工程智能体：从编译器报错、单元测试或文本日志修代码，反馈中没有任何浏览器渲染或视觉信息，能修通功能却可能毁掉外观。论文认为决定性的失败往往发生在动作执行瞬间，所以只有把真实渲染和执行结果纳入反馈回路，才能同时照顾两个要求。

### 作为优化问题的难点
1. 生成网页的 DOM 结构与参考实现不同，参考轨迹中的选择器、坐标、元素状态不能直接照搬，需要在目标页面上可靠重放动作；2. 交互失败有多层原因——元素不存在、按钮未绑定事件、异步状态未更新、样式遮挡、滚动位置不对等，观测信号需要足够接地（grounded）；3. 交互正确性和视觉保真度是两类异构信号，需要一个统一分数同时驱动择优与收敛，避免顾此失彼；4. 迭代改码需要把"哪里不对"转成可执行的代码修改，而不是笼统批评；5. 训练数据若只按视觉标签生成，模型仍学不到"执行失败"的信号，需要先执行验证再保留样本。

### 论文的核心主张
从检索到的 overview 片段看，RILA 明确把可靠网页开发形式化为"execution-driven incremental editing problem"，而非 one-shot generation 或"推倒重来式"的 regenerate-from-scratch repair。这个形式化基于两个观察：许多交互失败只在特定状态和动作序列下暴露；即使失败点已知，小范围增量修改也比整页重生成更可控、更容易保持已经正确的视觉部分。

Q2: 有哪些相关研究？

### 多模态网页生成
引言给出的语境是：多模态大模型已经能从文本指令、截图、草图（Li, Zhang, and Yang 2025 方向）与交互视频（Xiao et al. 2025; Chen et al. 2026 方向）生成完整网页；强代码模型（Hui et al. 2024; Guo et al. 2024）能够合成含 HTML/CSS/JS/外部资源的完整应用，视觉质量可观。相关工程与研究表明 UI-to-code 已从静态截图转向交互式、带反馈的设定，例如论文引用的 UI2Code^N: UI-to-Code Generation as Interactive（Yang et al. 2026 方向，片段只显示题目，推断为交互式 UI 转代码工作）。

### 前端渲染审计与修复
论文明确对立面是"dedicated front-end repair approaches"（Yuan et al. 2025 等）：它们基于设计规范审计静态渲染，能发现视觉/布局违规，但不检查页面在用户动作下的行为。这类方法可以看成"视觉在环但交互不在环"。

### 软件工程智能体与代码修复
另一个相关阵营是软件工程智能体：利用编译器报错、单元测试、文本日志修复代码。论文引用领域代表性基准 SWE-bench（Jimenez et al.，片段中可见 "SWE-bench: Can Language Models Resolve Real-World GitHub Issues?"，需回原文补充完整引用），强调这类方法的问题在于完全缺失渲染反馈——它听得到程序错误，但看不到页面坏了。RILA 的"用 SWE 工具定位和修复"继承了该路线，又把反馈源换成浏览器渲染与动作验证，形成交叉。

### 训练数据与合成网页数据
相关数据方向包括把网页截图转换成 HTML 的大规模数据/模型（Laurençon, Tronchon, Sanh 2024，标题为 "Unlocking the Conversion of Web Screenshots into HTML Code"，可合理推断与 WebSight 类工作有关）。RILA 的 execution-verified 数据合成流水线可视为这条线的延伸：不再只要求截图-代码配对，而是要求合成样本能被真实执行验证。

### 与 agent/RL/执行闭环的共性
摘要和引言没有穷举与网页 agents、GUI grounding、视觉语言模型浏览器 agent 的关系，但"在真实环境执行、收集执行反馈、迭代修正"的范式与网页操作 agent 的闭环训练存在方法论重叠。需要回原文确认 RILA 是否把 reference trajectory 的覆盖率和状态匹配等 GUI-grounding 技术纳入 AIV。

Q3: 论文如何解决这个问题？

### 总体框架
RILA 是一个迭代运行的 execution-driven agent，核心循环是"run in browser -> execute reference actions -> collect runtime observations -> score -> critique -> edit code"，论文总结为 execution–critique–edit loop。它把网页生成从"单次输出"改成"可验证的优化过程"，并保守保留历史最佳版本。

### 输入与参考演示（R）
从检索到的 overview 证据可知：除了参考截图/交互视频之外，系统会从交互视频进一步导出参考演示 R，R 包含参考截图与需要复现的交互轨迹 A={a1,…,aN}。R 同时充当两个角色：执行验证的目标和最终评估的目标。这是"reference trajectory replay"得以成立的基础；论文称 R serves as the target for both execution and evaluation。

### Action Interaction Verification（AIV）
AIV 模块负责把参考交互轨迹在生成网页上重放，并逐步（per-action）验证每个动作。它收集的是 grounded execution-aware observations：动作是否找到目标、是否真正生效、产生了什么 DOM/状态变化，而不是仅看 LLM 对截图的印象。结合 running in a real browser 的设定，可推断 AIV 要处理参考页面与生成页面结构不一致造成的动作映射困难，具体重放/容错机制在检索片段中未展开。

### Execution-aware Rendering Score（ERS）
ERS 是引导优化的目标函数，联合度量交互正确性与视觉保真度。它至少有两个作用：给每一轮迭代一个可比较的质量分；在迭代中保留下历史最好实现（论文 conclusion 明确提到 keeps the historically best implementation），防止改代码过程中"功能修好、视觉退化"或反之。ERS 的具体公式、视觉度量（如截图相似度、DOM 布局距离）和执行奖励权重在这批检索证据中缺失，需读原文 Method 部分。

### Execution-aware Critique 与代码编辑
论文不是只报分数，而是把 AIV/ERS 观测到的 discrepancy 转化为结构化修改建议（structured fix suggestions），再用软件工程（SWE）工具定位缺陷代码并修复。也就是说，LLM 不只"看到分数上升/下降"，还获得"哪个动作、哪个渲染区域、哪种代码模式出了问题"的细化解释。这是把执行信号落地成代码编辑的关键接口。

### 训练侧流水线
在推理时优化之外，论文还构建了 execution-verified data synthesis pipeline：合成多样、高质量的网页开发训练样本，并用真实执行验证过滤/标注，使模型在参数层面也能学到"可执行正确性"。摘要称其收益与 inference-time optimization 互补——合理推断为：推理时 RILA 能修错但成本高、单轮能力受限；训练数据则让更小的模型起点更高，从而在有限轮次内达到更强结果。

### 需要谨慎看待的部分
检索片段只可靠覆盖上述模块的名字、角色和循环结构；AIV 如何处理参考动作与生成页面元素不对齐、ERS 各分项如何加权、critique 是否依赖额外 VLM、SWE 工具具体调用了什么、训练合成数据用什么底模生成，都没有出现在已提供的证据里。这些内容在原文 Method 部分应有定义，不宜凭摘要脑补。

Q4: 论文做了哪些实验？

### 评测基准与任务
论文的实验统一落在 IWR-Bench 上。从名字与摘要语境推测，IWR 可能是 Interactive Webpage Rendering/Reconstruction 类基准，任务会同时要求生成网页的视觉相似度与对参考交互视频中动作序列的可复现性；但 IWR-Bench 的规模、任务划分、参考来源、动作类型与指标口径在检索证据中没有出现，需回原文确认。

### 被验证的主干/基座
1. 论文声称 RILA“across foundation models”一致提升交互与视觉保真度，说明实验覆盖了至少多个基础模型，但具体清单（开源/专有、规模、是否微调）缺失。
2. 明确写出的强基线是单次生成式网页生成器：Kimi-K2.6（约 1T 参数，开源）得到 55.61%，专有 GPT-5.5 得到 55.74%。两者被描述为 one-shot generators，即没有执行反馈闭环。
3. 受训练流水线加持的紧凑主干 Qwen3.5-9B 从 40.40% 提升到 57.52%。这里的数字应是 IWR-Bench 上的某个综合得分或主指标，论文 abstract 没有给出它包含哪几个分量。

### 实验设计的大致结构
由摘要和引言可拼出：实验需要回答三件事——（a）执行驱动闭环相比单次生成在交互正确性和视觉保真度上是否双升；（b）critique 质量与基础模型规模的关系（compact 9B 靠强 critique 反超更大模型是 Figure 1 的卖点）；（c）训练侧 synthesis pipeline 与推理时 RILA 的增益是否互补。推断原文应有：基础模型对比表、RILA 各轮次迭代曲线、AIV/ERS/critique 的消融、数据流水线的训练前后对比。

### 证据缺口
当前 retrieved_evidence 只给出 Abstract、Introduction、Conclusion 与少量 Overview 片段，method/results 正文没有进入检索结果。因此本字段无法写出完整实验协议、每个 baseline 的配置、消融表和失败案例；这些缺口必须通过原文补读解决，不能从摘要推断。

Q5: 发现了什么实验现象？

### 摘要/结论直接支持的观察
1. 执行驱动闭环能在不换骨干模型的前提下同时提高交互正确性与视觉保真度——不是“此消彼长”。
2. “尺度差距可以被执行反馈和强 critique 弥补”：9B 的 Qwen3.5 经训练和 RILA 闭环后，超过 1T 的 Kimi-K2.6 和更大规模的专有 GPT-5.5 的一次生成结果。这说明网页生成任务中，推理时的环境反馈+结构化修复比单纯堆参数更有效。
3. 训练侧 execution-verified 数据与推理时优化的收益互补：40.40%→57.52% 的提升明显超出单靠闭环或单靠训练可解释的范围（此句为基于摘要表述的合理推断，具体消融尚未看到）。
4. 一个明显的反直觉点：较小的开源模型不仅能逼近、还能超过“far larger one-shot generators”，包括专有模型。论文把这个结果归因于强 critique 引导的执行闭环能够补偿 code-model scale 的差距。

### 间接显现的行为规律
- 视觉指标好的页面并不等于交互可用，论文反复强调“look plausible but break under user actions”，可以视为一种失败模式画像：纯静态外观优化会产生假阳性页面。
- 反馈的种类决定修复路径：编译器/日志反馈可以把逻辑修通但丢视觉；渲染审计反馈可以保住视觉但漏交互；只有同时包含动作执行结果与渲染画面的反馈才能双向约束。不同反馈模态的缺失会产生不同形态的坏页面。
- critique 质量可能是闭环天花板的关键中介变量：如果 critique 弱，执行验证信号再强也难转成正确修改；因此“小模型+强批评者”可以胜过大模型单次生成，而“弱批评者+大模型”未必划算。

### 未在现有证据中出现的现象
论文没有提供单轮修复成功率、不同失败类型（选择器失效/异步未完成/事件未绑定/样式遮挡等）的分布、迭代收敛曲线、视觉分数与交互分数在中间轮次是否出现反向波动、训练数据规模的影响、以及各轮 token/时间成本。这些观察不能编造，必须查阅原文 Experiments 与可视化结果。

Q6: 有什么可以进一步探索的点？

### 原文逻辑直接延伸的方向
1. 把 RILA 的执行反馈从"参考轨迹重放"扩展到"自由形式用户探索"：不只验证给定轨迹，还让 agent 自选动作探测边界情况，可能覆盖原 demo 未展示的交互路径（推测）。
2. 把 execution-verified synthetic data 从网页开发推广到更多 UI 生成场景，如表单、仪表盘、移动端页面、复杂前端框架/组件库；当前只见到 IWR-Bench 一个基准的证据。
3. 研究 critique 模型本身的可扩展性：既然 RILA 的能力高度依赖强 critique，可以专门训练/蒸馏一个 execution-aware critic，或把 AIV 失败信号转成 critique 训练数据，形成"执行验证-批评-编辑"的自举回路。
4. 更细粒度地融合 ERS 与代码修改：例如用差分执行（修改前后分别重放轨迹）给出 per-action 根因定位，决定修复是 JS 行为、CSS 状态还是 DOM 结构问题。
5. 评估 pipeline 的工程化价值：RILA 每轮都要真实浏览器渲染和轨迹重放，在网页规模大、动作序列长、并发任务多时的延迟/成本控制是明显可做的工作；当前论文没有展示成本曲线。

### 更远的研究问题
1. 把同样的"渲染在环"思想用于网页操作 agent 的训练/评测，而不只是网页生成：生成与执行可以共享同一套可验证轨迹信号。
2. 结合代码级单元测试与视觉回归测试，把"功能正确"的定义从单条参考轨迹扩展到规范/意图形式化描述。
3. 视觉保真度与交互正确性之间可能存在帕累托前沿，ERS 的加权方式决定了收敛点；可研究多目标优化与用户偏好对齐。
4. 在 AI-for-science 这类需要定制科学 Web 工具/交互可视化平台的场景中，RILA 式"按演示轨迹生成并自动修到可用"的工作流可能具有迁移价值——若参考视频/轨迹可得，就能减少人工验收，但这属于推测，论文本身没有涉及。

### 需要原文素材才能落实的方向
训练数据合成流水线的具体 prompt/采样/验证策略、AIV 的容错机制和 ERS 的指标构成都是未来工作的前置知识；在补读 Method 前，以上扩展方向应视为 plausible 而非论文直接承诺的 Next Step。

Q7: 总结一下论文的主要内容

RILA（Rendering-in-the-Loop Agent）是一篇面向"交互式网页自动开发"的 arXiv 论文（2609.02088v1，作者 Yilong Guo、Hanqi Chen、Zixiao Ye、Guanzhong Wang、Chen Yu、Zeyu Chen）。论文的靶子是当前多模态网页生成的两难：一个可用的网页同时要求视觉保真和交互正确，但现有系统通常只在单条反馈通道内优化。

论文的论证主线开始于对现状的分类：纯一次生成式 MLLM 网页系统把渲染质量当作主要目标，依赖截图/设计稿与生成页面的视觉相似度；前端修复方法（如 Yuan et al. 2025 一类工作）对照设计规范审计静态渲染，规范覆盖不了真实用户操作；软件工程智能体则从编译错误、单元测试、日志修复代码，拥有执行世界的信息，却对页面视觉一无所知。三种路线彼此隔离的结果是：生成的页面要么视觉达标但动作一执行就坏，要么功能被修通但页面视觉被改坏。论文由此提出关键判断：交互失败通常要到真实浏览器中执行动作、看到 DOM 与画面变化时才会暴露，所以可靠网页开发不能做成 one-shot generation，也不能做成"只看渲染/只看日志"的 repair，而应把浏览器渲染放进优化环路，用执行反馈驱动增量编辑。

技术主线围绕一个执行—批评—编辑循环展开。RILA 从参考交互视频导出参考演示 R，R 包含参考截图和动作轨迹 A={a1,...,aN}，并同时作为执行目标与评估目标。每轮迭代中，RILA 在真实浏览器里运行当前生成版本，用 Action Interaction Verification（AIV）模块重放参考轨迹并逐步验证每次动作，得到接地气的执行感知观测；随后用 Execution-aware Rendering Score（ERS）把交互正确性与视觉保真度压成一个统一分数，既指导本轮修复方向，也保留历史最优实现，防止迭代回退。得到观测与分数后，Execution-aware Critique 把与参考演示的差异转成结构化修复建议，RILA 再借助软件工程（SWE）工具定位故障代码并实施修改。这个"先验证、后打分、再结构化批评、最后工具化编辑"的组合，是论文相对于"让 LLM 直接改代码"或"整页重新生成"的核心设计。

第二条技术线是执行验证的数据合成流水线。系统先合成多样化、高质量的网页开发训练样本，再用真实执行验证样本是否成立，而不是只看截图配对。摘要强调该训练侧增益与推理时优化互补：推理时 RILA 解决单次生成漏掉的问题，训练数据则提高模型起点的可执行质量，两者不是重复收益。

实验主线集中在 IWR-Bench。RILA 在多个基础模型上都一致地同时改进交互正确性和视觉保真度。最醒目的定量结果是：配合 execution-verified 训练流水线后，紧凑的 Qwen3.5-9B 主干得分从 40.40% 提升到 57.52%，越过两个更大的单次生成模型——1T 参数的开源 Kimi-K2.6（55.61%）和专有 GPT-5.5（55.74%）。论文把这个结果解读为：在强 critique 引导下，执行驱动细修可以弥补巨大的 code-model scale 差距。结论进一步把它抽象为方法论主张："keeping rendering in the loop"能把运行期交互变成可验证的优化信号，从而可靠地提升网页质量。

需要注意的是，本次使用的 PDF 语义检索只覆盖 Abstract、Introduction、Conclusion 和少量 Overview 片段，Method/Results 的公式、具体消融和完整表没有进入证据，因此 summary 中关于 AIV/ERS 工作方式的部分主要来自结论段的概括性描述；精确的指标构成、训练数据规模、每轮迭代次数、失败模式分析等仍需要回到原文确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 agent 方向直接相关：RILA 是"用真实环境执行信号驱动智能体迭代"的代表，展示如何把浏览器 runtime 交互变成可验证的优化目标，而不是让 LLM 自己想象页面效果。

## 基本信息

- 作者：Yilong Guo, Hanqi Chen, Zixiao Ye, Guanzhong Wang, Chen Yu, Zeyu Chen
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-09-02
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.02088v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 arXiv 元数据以及 Abstract/Introduction/Conclusion/Overview 的 PDF 语义检索命中片段，并修正了 heuristic_draft 中把参考文献误当 limitations 的问题；Method/Results 正文未见检索命中，相关细节已标注为信息缺口并建议回原文核验。
