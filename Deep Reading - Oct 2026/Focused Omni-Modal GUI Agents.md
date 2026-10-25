---
user_id: "cheng tan"
paper_id: 16583
arxiv_id: "2610.26203v1"
title: "Focused Omni-Modal GUI Agents"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26203v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26203v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:09:14"
---
# Focused Omni-Modal GUI Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agent · omni-modal interaction · focused perception · sequential distraction

## 一句话总结

本文针对流式全模态 GUI 代理中“完整上下文均匀暴露、相关性瞬态稀疏、动作误差反馈复合”导致的 Sequential Distraction 问题，提出 Focused Perception 概念并实现 FocusGUI——一个零初始化残差注入的轻量适配器，通过逐步动态优先多模态通道、将最相关证据压缩为动作充分隐表示并注入冻结解码器嵌入，仅靠动作监督即在 Qwen2.5-Omni、VITA-1.5、MiniCPM-o 4.5、Baichuan-Omni-1.5 四个骨干上平均将 Type Match 提升 +33.2、Exact Match 提升 3.0 倍，并比最强现有增强方法多提升 +30.1 Type Match。

## 摘要

> Graphical User Interface (GUI) agents increasingly act over streaming omni-modal observations spanning screenshots, audio, video, interaction history, and device states. However, exposing this complete context uniformly at every step ignores that relevance is transient and sparse: the signal determining one action can become clutter at the next. Because executed actions feed back into the context, these errors compound into Sequential Distraction, driving the agent into irreversible trajectory drift. We formulate the underlying requirement as Focused Perception: a representation that is step-adaptive, action-sufficient, and non-intrusive. We realize this in FocusGUI, a lightweight adapter that dynamically prioritizes multimodal channels against a step-specific decision reference. Instead of uniformly processing all inputs, it compresses the most relevant evidence into a compact latent and subtly injects it into the frozen decoder's embeddings, guiding the model's attention without altering its native parameter space. FocusGUI trains from action supervision alone, requiring neither modality-relevance labels nor backbone fine-tuning. Across four omni-modal backbones, it raises overall Type Match by +33.2 and Exact Match by 3.0 times over the frozen backbone on average, and Type Match by +30.1 over the strongest existing enhancement method.

Q1: 这篇论文试图解决什么问题？

问题背景：GUI 代理的输入正在从静态截图转向动态、流式的多模态观察。真实计算环境中，决定下一步动作的证据不一定在当前视觉帧内：一次意外的通知提示音可能要求立即切换上下文，一段实时视频中的特定转场可能决定何时点击，口语指令需要被持续映射到屏幕变化。当代全模态模型可以在单一网络内摄入截图、视频流、音频、交互历史和设备状态，于是自然形成了一个默认范式——在每一步把完整的多模态观察暴露给模型，由解码器在生成动作时隐式决定什么重要。

隐性假设与困难：这种默认范式假设解码器能在每一步从完整观察中可靠地重新发现关键信息。本文分析说明这很难成立。首先，相关性是步骤特有的：在多步交互中逐模态移除的测试显示，主导模态在相邻步骤之间切换的频率接近一半，也就是说不存在一个全局固定的重要通道；其次，相关性是稀疏的：在单个步骤内，只有很小的观察子集支撑决策，其余观察即使不是噪声，也可能分散注意力。由于动作执行后会反馈回观察与历史，任何一步的注意力错误都会成为下一步的输入污染，误差随步骤复合。作者称这种复合失败为 Sequential Distraction，它会把智能体推入不可逆的轨迹漂移。

现有方案为什么不够：最直观的补救是“减法”——对观察做某种形式的裁剪或空间精化，例如只保留屏幕上的相关区域后再交给模型。这类方法有三个结构性缺陷。第一，它们大多只作用于视觉/屏幕通道，无法触及音频、视频或设备状态；一旦决策依赖这些非视觉通道，无论屏幕如何精化都不可恢复。第二，它们需要同时满足“动作选择”和“参数接地”两个目标，但空间精化往往只对后者有效，难以同时改进前者；作者明确指出空间精化无法同时满足选择动作与接地动作参数。第三，它们在输入空间上做修改，等于让下游解码器依赖一个被截断的观察，可能丢失全局上下文。

聚焦感知的提出：基于上述分析，本文把核心需求形式化为 Focused Perception，并要求任何实现满足三个性质：step-adaptive（焦点表示必须随步骤变化，因为相关性是步骤的函数而不是全局常量）；action-sufficient（表示只需包含生成下一步动作所需的证据，不要求重建完整观察，因为原始观察路径仍然保留）；non-intrusive（引导解码器注意力时不应改动骨干的原生参数空间，以避免昂贵的微调并保持预训练能力）。这三个性质直接决定了机制形式：不是在输入上做减法，而是在解码器读取方式上做加法——用轻量适配器计算一个步骤特定的焦点信号，注入解码器的嵌入表示。

问题定位：本文因此不是提出又一个 GUI 解析器，而是挑战“完整上下文每步均匀暴露”这一默认设计，主张相关性是一种瞬态、稀疏且随步骤演化的属性，需要显式的、逐步的感知组织。这也是与 DRS-GUI、OmniParser、RegionFocus 等视觉精化方法拉开距离的关键点。

Q2: 有哪些相关研究？

1. 流式与全模态 GUI 代理：Henry 等人的 OmniGUI 基准把截图与同步音频、视频、历史配对，是本文的评估载体；Jang、Yao 等研究 GUI 代理在动态观察下决策；ByteDance Seed、Li、Yuan、Chen 等关注通知打断、视频转场、语音到屏幕变化映射等具体场景。这些工作展示了全模态决策的必要性，但普遍沿用每步输入完整上下文的做法，没有显式处理逐步相关性问题。
2. 全模态基础模型：Qwen2.5-Omni、VITA-1.5、MiniCPM-o 4.5、Baichuan-Omni-1.5 均支持文本、视觉、听觉和时序输入，架构与多模态表示各异。本文把它们作为冻结骨干，验证 FocusGUI 跨架构的通用性。
3. 视觉 UI 解析与空间精化：OmniParser 将截图解析为结构化元素，DRS-GUI 和 RegionFocus 精化屏幕区域以改善 grounding。作者认为这些方法本质上做输入“减法”，覆盖不了非视觉通道，且难以同时满足动作选择与参数接地，因而不能解决 Sequential Distraction。
4. 动作预测与 GUI actor 增强：结果表中出现 GUI-Actor 作为对比增强方法，合理推断它代表在骨干之上增加可训练动作头或策略网络的常见做法；FocusGUI 与之相比的关键差异是不改动骨干且显式建模通道焦点。
5. 参数高效适配与旁路注入：FocusGUI 使用零初始化残差注入，与 LoRA、Adapter 等参数高效微调思想相近但作用位置不同——它不修改任何权重，只在解码器嵌入序列上加残差，引导注意力；这更接近前向过程中的“焦点控制”而非参数微调。
6. 上下文选择与记忆压缩：相关工作常通过检索历史或摘要来压缩上下文，而 FocusGUI 压缩的是当前多模态观察中与步骤相关的证据，且保留原始路径，两者互补而非替代。
7. 注意力可解释性：Figure 4 的通道权重 α(m)t 分析属于用注意力权重诊断模型行为的一类做法，作者用它展示跨交互阶段的相关性漂移，这类分析与可解释性研究关系密切。

Q3: 论文如何解决这个问题？

1. 问题形式化：在交互步骤 t，智能体收到全模态观察 Ot = {g, It, Vt, At, Ht, St}，依次为用户目标、当前界面、时序视觉观察、声学观察、交互历史、设备状态。它预测可执行动作 yt = (yt,1, ..., yt,Lt)，表示为既包含动作原语也包含参数的动作 token 序列。令 FΘ0 为预训练全模态骨干，参数 Θ0 完全冻结；令 Et 为送入其解码器的嵌入序列。
2. 核心设计原则：作者承诺在“解码器如何读”而非“给它看什么”上实现焦点，这决定了机制形式。FocusGUI 既不替换也不剪枝观察的任何部分；它从同一观察构造一个紧凑焦点表示 zt = fϕ(Ot)，映射为残差 Rt = Aϕ(zt)，并把残差加到一小部分现有嵌入位置上。最终解码器运行在 eEt = Et + SJt(γRt) 上，其中 SJt 把残差放在位置集 Jt，其余位置为零；预测分布写为 pΘ0,ϕ(yt | Ot) = pΘ0(yt | eEt)。这样“观察保留”与“决策引导”被分离：原始通路保留完整多模态上下文，FocusGUI 通路学习如何为该步动作强调上下文。
3. 焦点表示的性质：由于每个像素、坐标和转录仍由骨干直接可见，zt 不必重建观察，也不必编码任何执行细节；它只需要承载与下一步动作相关的证据，即 action-sufficient。这允许把焦点表示压缩到很小的维度，降低适配器开销。
4. 逐步自适应证据组织：优先级不能在融合表示内部定义，因此观察先被暴露为可分离的感知通道 Xt = {x(m)t}_{m∈M}，其中 M 索引来源，包括骨干的集成隐藏状态和一个空间屏幕通道等。FocusGUI 根据一个步骤特定的决策参考动态优先这些通道，压缩最相关的证据而不是统一处理所有输入。这就是 step-adaptive 的具体实现。
5. 注入与初始化：残差使用零初始化，训练开始时注入量为零，避免扰动预训练骨干的初始行为；注入点被安排在动作边界，使动作 token 生成时能够利用焦点信号。由于残差只作用于嵌入，骨干权重完全冻结，不需要为每个任务微调。
6. 训练监督：只使用动作标注。焦点适配器和动作预测器共同训练，不需要模态相关性标注（标注哪些通道重要），也不需要骨干微调。作者强调这是实践上很重要的性质，因为全模态相关性标注非常昂贵。
7. 容量控制：默认参数 K=4、dz=64（推测为焦点头数与压缩维度）在四个骨干上固定，不按基准调参；消融覆盖 16 倍范围。这个设置使方法易于移植到不同骨干。

Q4: 论文做了哪些实验？

1. 基准与指标：论文在 OmniGUI 基准上评估，覆盖 29 个应用，共 708 episodes、2572 个动作步；每一步把截图与同步音频、视频、动作历史配对，因此决定性信号可以来自四条流中的任意一条。指标包括 Type Match（TM，动作原语是否匹配）、Exact Match（EM，在 TM 基础上要求坐标落入目标元素 bounding box 且生成文本匹配）和 Goal Progress（GP，每 episode 正确执行步数的比例再跨 episode 平均）。
2. 骨干与训练协议：选用四个代表性开源全模态骨干——Qwen2.5-Omni、VITA-1.5、MiniCPM-o 4.5、Baichuan-Omni-1.5。四个骨干均支持文本、视觉、听觉和时序输入，但架构与多模态表示不同，用于检验 FocusGUI 的跨家族泛化性。对每个骨干，基础模型保持冻结，训练只更新轻量 FocusGUI 头与动作预测器；四个模型使用相同的 episode-disjoint 训练/测试划分。
3. 基线方法：对比了 DRS-GUI、OmniParser、RegionFocus 三种现有增强方法，它们都只精化视觉/屏幕且不修改骨干权重；结果表中还出现 GUI-Actor 作为对比增强方法（合理推断是一种可训练的 GUI 动作头或 actor 增强）。所有方法分别应用到四个骨干上，不改变骨干权重。
4. 主要对比实验：摘要报告平均结果——相对冻结骨干，FocusGUI 将整体 Type Match 提升 +33.2，Exact Match 提升 3.0 倍；相对最强现有增强方法，Type Match 多提升 +30.1。检索到的结果表碎片显示逐骨干数值有明显增长（例如某些单元格从二十几提升到五十几），但表头缺失，具体列对齐需查原文确认。
5. 通道动态分析：Figure 4 在测试 episodes 上按交互阶段（每 25 步一组）平均通道权重 α(m)t，显示 history、screen、audio 等通道的权重在不同阶段发生变化，部分通道权重变化为负值（检索碎片可见如 audio 在某阶段 +3.6、screen/history 在另一阶段 -1.2/-1.7 等），说明不存在固定优先级，逐步自适应是必要的。
6. 容量消融：在 Qwen2.5-Omni 上对紧凑焦点维度 dz 做 16 倍范围扫描，并固定另一个参数；默认 (K=4, dz=64) 在全部骨干上固定，未针对基准调参。
7. 深层交互稳定性：论文报告 FocusGUI 在较深交互步骤仍保持较高准确率，而基线方法在历史上下文累积处通常退化；这与 Sequential Distraction 的预测一致，说明焦点注入能部分抵消误差复合。

Q5: 发现了什么实验现象？

1. 模态主导权快速切换：逐模态移除测试显示，在多步交互中主导模态在相邻步骤间切换接近一半；这直接反驳了“存在全局固定重要通道”的直觉，也解释了固定通道加权方案为何失效。
2. 单步相关性稀疏：单个步骤内只有一小部分观察支持决策，其余可能成为分散注意力的干扰；说明“完整上下文”并非越多越好。
3. 视觉精化基线存在结构性失败模式：DRS-GUI、OmniParser、RegionFocus 都只处理屏幕/视觉通道，无法触达音频、视频或设备状态。论文明确指出，一旦决策依赖非视觉通道，无论屏幕如何精化都是不可恢复的；这是它们提升有限的原因，也是 FocusGUI 能拉开差距的机制性解释。
4. 通道优先级随交互阶段漂移：Figure 4 显示通道权重 α(m)t 按交互阶段变化，且变化方向不一致（一些通道在某些阶段权重上升、另一些阶段下降）。这支持“相关性是步骤的函数”这一核心假设，并说明 step-adaptive 不是可有可无的装饰。
5. FocusGUI 的增益分布：摘要中的平均提升 +33.2 TM、3.0 倍 EM 是稳健的；检索到的结果表碎片也显示 +FocusGUI 相对基线和 +GUI-Actor 有显著增长，但具体单元格对齐和每个骨干的细致分解需要原文核对。
6. 深层交互的鲁棒性：当交互步数加深时，历史上下文通常导致模型退化，但 FocusGUI 仍保持较高准确率；这是“抵抗 Sequential Distraction”的直接证据，暗示焦点信号不只是提升单步 accuracy，还改变了误差在轨迹上的累积方式。
7. 容量消融的稳定性：跨 16 倍范围的焦点维度扫描中，默认配置表现稳定（细节需原文确认），说明性能不是靠某个过拟合的超参数取得的；默认 K=4、dz=64 跨四个骨干固定而不调参，也支持方法的通用性。
8. 指标间张力：EM 的相对提升（3.0 倍）远大于 TM 的绝对提升（+33.2），存在基数效应；Goal Progress 从表格碎片看仍然偏低（如个位数到十几），说明即便有焦点注入，完整的多步任务依然困难，Sequential Distraction 只是被缓解而未被消除。

Q6: 有什么可以进一步探索的点？

1. 作者明确指出的方向：当前通道优先化依赖“当前观察”，未来可以扩展到预测或预估下一步的通道相关性（anticipate channel relevance），这有望在交互序列的更早阶段抑制轨迹漂移。
2. 从单步焦点到多步规划：把 Focus 从 step-level 扩展到 action-sequence 级或 plan-level，让焦点表示携带未来几步的证据需求，从而更好协调长程目标与当前动作。
3. 记忆与焦点的结合：将历史观察压缩成持久焦点状态或可检索的焦点记忆，与当前观察的焦点表示融合，处理更长 horizon 的任务，减少 Sequential Distraction 的累积。
4. 弱监督与自监督相关性学习：目前只靠动作监督隐式学习通道相关性；未来可探索用弱监督（如任务描述、演示）或自监督（如模态间一致性）来提供相关性信号，降低对大量动作标注的依赖。
5. 可解释性与审计：通道权重 α(m)t 本身提供了每步“模型在听哪条流”的线索；可进一步开发面向人的焦点可视化与审计接口，用于诊断智能体的错误原因。
6. 跨域与跨设备泛化：当前实验集中在 OmniGUI 智能手机场景；可扩展到桌面 GUI、Web 界面、多屏或跨设备交互，那里通道数量和干扰模式更复杂。
7. 对抗性与嘈杂环境：真实环境有突发通知、多任务竞争和对抗性 UI 变化；可评估焦点机制在这种噪声下的鲁棒性，并研究如何区分“真实相关信号”与“刻意干扰”。
8. 与强化学习/在线学习结合：FocusGUI 的残差注入可视为策略网络的一部分，未来可用奖励信号直接优化焦点策略，或者在线调整通道优先级以适应个体用户习惯。
9. 更细粒度的注入位置：目前残差加在少量嵌入位置；可探索在注意力层内部、跨层或不同注意力头之间注入焦点信号，可能带来更可控的引导。
10. 理论分析：可研究焦点残差如何从机制上防止历史错误进入后续步骤的 key/query 计算，以及注入容量与轨迹稳定性之间的理论关系。

Q7: 总结一下论文的主要内容

本文围绕一个具体而普遍的失败模式展开：全模态 GUI 代理在多步交互中把完整上下文均匀暴露给解码器，但相关性随步骤快速变化且高度稀疏，注意力错误又会通过动作执行反馈回上下文，形成复合误差，导致不可逆的轨迹漂移。作者把这一现象命名为 Sequential Distraction，并通过对 OmniGUI 行为的分析（单模态移除、通道权重变化）论证了它的存在与成因。

针对这一诊断，作者提出 Focused Perception 作为需求定义：任何解决方案都必须满足 step-adaptive（随步骤变化）、action-sufficient（只需动作所需证据）和 non-intrusive（不改变预训练骨干参数空间）。这一定义排除了此前主流的输入空间“减法”路线——DRS-GUI、OmniParser、RegionFocus 等视觉精化方法只能处理屏幕通道，既无法覆盖音频、视频、设备状态，也难以同时完成动作选择和参数接地。

FocusGUI 是作者给出的实现：一个轻量适配器。在每一步，它先从完整观察中构造一组可分离的感知通道，包括骨干的集成隐藏状态和空间屏幕通道等；然后根据步骤特定的决策参考为这些通道动态分配优先级，把最相关的证据压缩成紧凑的焦点表示 zt；再将 zt 通过一个映射产生零初始化残差，注入到冻结解码器的少量嵌入位置上。最终解码器看到的嵌入是原始嵌入加一个稀疏掩码的残差，骨干权重完全不动。焦点残差从零初始化，因此训练初期不影响预训练行为；注入点位于动作边界，便于动作 token 生成时利用焦点信号。训练只需要动作标注，不需要任何模态相关性标注，也不需要微调骨干。默认容量参数 K=4、dz=64 跨四个骨干固定，不按基准调参。

实验在 OmniGUI 基准上进行，覆盖 29 个应用、708 episodes、2572 个动作步，每一步都配合同步音频、视频与历史，因此决定性信号可以来自任一流。论文报告 Type Match、Exact Match、Goal Progress 三个指标，并使用 Qwen2.5-Omni、VITA-1.5、MiniCPM-o 4.5、Baichuan-Omni-1.5 四个骨干检验泛化性。结果显示，FocusGUI 相对冻结骨干平均提升 Type Match +33.2、Exact Match 3.0 倍，相对最强现有增强方法平均多提升 Type Match +30.1；在较深交互步骤中仍保持较高准确率，而基线通常因历史累积而退化。通道权重分析表明焦点优先级确实随交互阶段变化，消融显示默认容量配置在 16 倍范围内表现稳定。

论文的局限也很明确：焦点选择只依赖当前观察，不具备前瞻性；实验覆盖的骨干和基准仍有限；Goal Progress 的绝对值不高说明多步误差尚未被完全消除。未来方向包括预测通道相关性以更早抑制轨迹漂移、把焦点扩展到多步规划、引入更丰富的监督信号，以及把方法推广到更多设备和场景。总体而言，本文的价值在于把“每步均匀暴露完整上下文”这一默认设计问题化，给出了一个约束明确、实现轻量、训练经济的替代方案，并用多骨干实验系统验证了其有效性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent 方向直接重合（权重 0.10）：本文属于 GUI agent 的多模态感知与决策问题，并提出了新的范式。

## 基本信息

- 作者：Fuyao Zhang, Wenwen He, Jiaming Zhang, Yurong Hao, Xuan Wang, Guowei Guan, Che Wang, Longtao Huang, Wei Yang Bryan Lim
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26203v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 66 个 PDF 语义检索片段（覆盖背景、方法、结果、结论），并结合论文摘要与章节信息完成；部分表格数字和公式仅能从碎片中局部复原，相关处已在正文中做保守处理并提示回原文核对。
