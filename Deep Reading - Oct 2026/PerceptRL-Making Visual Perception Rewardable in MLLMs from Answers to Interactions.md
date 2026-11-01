---
user_id: "cheng tan"
paper_id: 18878
arxiv_id: "2610.37471"
title: "PerceptRL: Making Visual Perception Rewardable in MLLMs from Answers to Interactions"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Oct 2026/2610.37471.pdf"
pdf_url: "https://arxiv.org/pdf/2610.37471v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-11-02T01:19:31"
---
# PerceptRL: Making Visual Perception Rewardable in MLLMs from Answers to Interactions

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multimodal large language models · visual perception · reinforcement learning · agentic reinforcement learning

## 一句话总结

PerceptRL 将 MLLM 后训练中的细粒度视觉感知转化为可验证奖励问题：先从密集图像描述中抽取并验证感知错误，构造成可用规则判分的 QA，再用多轮视觉工具智能体 RL、Coordinate Injection 与 grounding-aware 过程奖励，强化单轮难以解决的密集感知案例。

## 摘要

> —Fine-grained visual perception remains a bottleneck during post-training of multimodal large language models (MLLMs),
> where models first perceive images and then reason over the extracted information, so perceptual errors can limit the gains from
> reasoning-focused optimization. Although extensive image-caption pre-training improves perception, the influence of this supervision
> can be diluted as later training stages change the data mixture and objectives. Continuing to optimize captions during post-training is a
> natural response, but their subjective, information-dense nature makes verifiable reward design difficult. To address this, we present
> PerceptRL to strengthen perception during post-training at two complementary levels. Perception RL uses a multi-agent pipeline to
> extract verified error items from dense image captions and compose the corresponding visual facts into rewardable QA pairs: chain QA
> links correlated errors, while multiple-choice QA combines unrelated errors. A specialized route targets high-error domains with tailored
> verification, while a general route addresses perceptual errors beyond these domains through a domain-agnostic pipeline. This2026 error-driven construction provides fixed answer labels and rule-based rewards, making caption-derived perceptual supervision
> verifiable. Perception Agentic RL targets fine-grained and dense cases that single-turn Perception RL still fails to resolve, enabling
> multi-turn inspection with visual tools. We observed coordinate drift even in models trained to invoke tools: incorrect crops orOct annotations can distort later evidence and propagate errors. Coordinate Injection corrects spatial operations in cold-start trajectories,
> while a grounding-aware process reward supervises intermediate spatial fidelity alongside final-answer correctness. Training proceeds
> through shared cold-start SFT, single-turn Perception RL, and multi-turn Perception Agentic RL. Across seven benchmarks under a27
> shared tool-use protocol, the full pipeline achieves 71.6% average accuracy, exceeding the base model with the same tools and the
> strongest evaluated frontier VLM by 15.7 and 5.2 percentage points, respectively.

Q1: 这篇论文试图解决什么问题？

（1）核心问题是后训练阶段的感知监督既不可验证，也不够细粒度。MLLM 的能力链是先感知图像，再在提取出的信息上推理；如果模型数错物体、混淆颜色或误判空间关系，后续推理即使逻辑正确也可能得到错误答案。论文引用 CodePercept 的观察：在其 STEM 视觉推理分析中，扩展感知比扩展推理更有效。这说明感知不是可以完全交给推理阶段自行修复的前置模块，而是一个需要单独优化的瓶颈。

（2）图像–caption 预训练提供了重要视觉监督，但后训练阶段的数据混合和目标变化会稀释这种监督。一个自然反应是继续在后训练优化 caption，但 caption 有两个结构性困难。其一是主观性：事实正确的 caption 可以在粒度、重点、风格和长度上不同，模型很难知道哪个方向是期望目标。其二是不可奖励性：caption 信息密集、开放，一段描述捆绑大量视觉断言，要构造可验证奖励，必须决定哪些断言被验证、如何归因、如何给部分正确性打分。这使 caption 难以像数学题最终答案那样直接做 RLVR。

（3）由此产生的技术缺口是：如何把感知错误从开放 caption 转成有固定答案标签、可用规则奖励的监督信号，同时不丢失细粒度空间信息。论文的答案不是简单用闭集 VQA 替换 caption，而是从 dense caption 中挖掘错误项，再用 chain QA 和 multiple-choice QA 两种形式重组。这里隐含假设是：caption 中的错误可以被多智能体验证器可靠识别，且 QA 化不会把感知任务过度简化。这个假设是全文方法成立的关键，但仅凭摘要和检索片段无法完全验证。

（4）第二个缺口是单轮感知 RL 对细粒度、密集案例仍不够。密集场景需要反复裁剪、放大、检查局部区域；一旦允许工具调用，就出现新的失败模式：坐标漂移。即使模型学会调用工具，它也可能生成错误裁剪框或错误标注，后续证据被污染，错误沿着多轮轨迹传播。这个问题把感知从单轮答案正确性推进到多轮空间操作保真度，要求过程奖励和坐标校正。

（5）评价设计上的难点是区分感知增益与推理增益。论文在七个 benchmark、共享工具接口下比较，试图让工具使用条件一致；但不同 benchmark 对计数、颜色、空间关系、状态识别和综合推理的负载不同。片段显示模型表现具有任务依赖性，某些基准由 Gemini-3 Flash 领先，这说明 PerceptRL 的收益并非全面 SOTA，而是有选择地补感知短板。若只看平均分，可能掩盖任务间不均衡；若只看计数，又可能高估泛化。

（6）需要直接指出的证据薄弱处：启发式草稿中混入了公式、表格和断行残片，例如坐标缩放、clip 范围、目标记录等句子，不能据此还原完整问题定义。论文真正的 problem formulation 应以 Introduction、Method 的干净段落为准。现有证据足以确认问题主线，但不足以确认作者是否讨论了所有反例，例如验证器假阳性、QA 化信息损失、跨域迁移失败等。

Q2: 有哪些相关研究？

（1）MLLM 视觉感知瓶颈与评测。论文把细粒度感知视为当前 MLLM 的重要短板，涉及计数、颜色、空间关系、状态判断等任务族。片段提到的评测包括 RealWorldQA、MMStar、BLINK、ERQA，以及两个 counting benchmark；还出现 RealWorldCounting、Color、Specialized Spatial、State 等条目及样本量残片。相关研究通常用多模态 benchmark 诊断感知错误，或分析感知与推理的耦合。论文引用 CodePercept 的结论——扩展感知比扩展推理更有效——作为动机。

（2）图像–caption 预训练与后训练监督。图像–caption 数据是视觉监督的重要来源，但论文指出其影响会在后训练阶段被数据混合和目标变化稀释。与本文相关的一条线是继续在 post-training 优化 caption，或用 dense caption 做视觉指令微调；另一条线是用闭集 VQA、检测、grounding 等任务提供更明确的监督。本文的差异在于不放弃 caption 的信息密度，而是把 caption 中的错误转成可验证 QA。

（3）可验证奖励 RL 与推理后训练。数学和代码领域的 RLVR 通常用最终答案对照参考解，给生成解提供客观反馈，而不需要标注每一步推理。论文明确把这一范式作为类比，并指出 caption 优化缺少这种最终答案检查。片段显示两个 RL 阶段都使用 DAPO，且涉及 decoupled clipping ranges、token-level credit 等机制。相关研究还包括 reasoning-focused RL 如何提升推理能力，以及这种优化为何可能不自动修复感知。

（4）多智能体验证与数据构造。Perception RL 用多智能体流水线从 dense caption 中提取 verified error items。相关研究包括用 LLM/VLM 做 claim 级事实验证、多智能体辩论、生成–验证–修订循环，以及从模型失败中主动构造训练数据。片段提到“agent-based verification：VLM 逐条审计 caption claim，并做全局检查”，说明验证不是单次打分，而是有分解的审计过程。

（5）Agentic RL 与视觉工具使用。第二层方法让模型多轮调用视觉工具进行细粒度检查。相关工作包括工具增强的视觉问答、代码执行式图像操作、裁剪/放大/标注、以及多轮 agentic RL。论文的独特观察是 coordinate drift：工具调用本身不足以保证空间操作正确，错误坐标会传播。相关研究涉及视觉 grounding、坐标回归、工具执行反馈和过程奖励。

（6）过程奖励与 credit assignment。论文用 grounding-aware process reward 监督中间空间保真，并与最终答案正确性结合。相关研究包括 process reward model、step-level credit assignment、token-level credit，以及把最终奖励回传到中间动作的 RL 设计。片段出现“3.3.3 Token-Level Credit”与轨迹状态、执行动作、clip 目标等公式残片，说明方法可能在多轮轨迹上做细粒度信用分配；但具体公式因 PDF 噪声无法确认。

（7）与本文最接近的空白。现有工作要么用 caption 做监督但难以验证，要么用闭集 VQA 做 RL 但覆盖窄，要么做 agentic 工具使用但缺少空间保真的过程监督。PerceptRL 试图把三者接起来：caption 错误挖掘、可验证 QA 奖励、多轮工具与坐标校正。其相关工作定位是否完整，需要回原文核对引用列表；当前证据只能确认这条大致谱系。

Q3: 论文如何解决这个问题？

（1）总体框架是两层次、三阶段。两层次指单轮 Perception RL 与多轮 Perception Agentic RL；三阶段指共享 cold-start SFT、单轮 Perception RL、多轮 Perception Agentic RL。共享 cold-start SFT 先让模型学会工具调用和输出格式，为后续 RL 提供可执行轨迹；单轮 Perception RL 用 caption 派生 QA 的规则奖励优化感知答案；多轮 Perception Agentic RL 再针对单轮失败的密集案例，训练模型用视觉工具反复检查。这个课程设计的合理推断是：先稳定格式，再学单轮感知，最后学多轮空间操作，避免直接在复杂工具环境里探索导致奖励稀疏。

（2）Perception RL 的数据构造是核心。论文用多智能体流水线从 dense image captions 中提取经过验证的错误项，并把对应视觉事实组织成可奖励 QA。这里的关键不是生成更多 caption，而是 error-driven：先发现模型在哪些感知事实上出错，再围绕失败构造监督。片段把数据构造分解为两个子问题：可靠地检测 caption 中的错误，以及把错误组合成 QA。专用 route 针对高错误领域做定制验证，通用 route 用域无关流水线处理这些领域之外的感知错误。两条 route 的分工体现覆盖—精度的权衡：专用 route 对已知高错误类型更精确，通用 route 扩大覆盖但可能更粗。

（3）QA 形式有两种。chain QA 连接相关错误，适合有依赖关系的感知链，例如一个错误会引出或掩盖另一个错误；它保留多步结构，使奖励不仅看最终选项，也看链式推理是否一致。multiple-choice QA 组合不相关错误，把多个独立感知断言放进一个单选题，利用干扰项做规则判分。两种形式都产生固定答案标签，从而支持 rule-based reward。论文声称这使 caption 派生监督可验证；其代价是 QA 化会压缩开放 caption 的信息，可能只保留可被选项覆盖的感知维度。

（4）奖励设计。单轮 Perception RL 主要用规则奖励：答案标签固定，模型输出与标签匹配即可给分。片段显示两个 RL 阶段都使用 DAPO，并有 decoupled lower/upper clipping 等实现细节；这说明训练不是简单 PPO，而是针对 RL 稳定性做了裁剪范围解耦。片段还出现 token-level credit 与轨迹状态、执行动作、clip 目标等公式残片，合理推断方法在多轮轨迹上做比最终答案更细的信用分配；但具体公式和适用范围需回原文确认。

（5）Perception Agentic RL 针对单轮仍失败的细粒度、密集案例。模型被允许多轮 inspection with visual tools，例如裁剪、放大、标注或执行图像操作代码。论文观察到 coordinate drift：即使模型训练过调用工具，仍会生成错误坐标；错误 crop 或 annotation 会改变后续可见证据，使错误沿轨迹传播。这不是简单的答案错误，而是中间空间操作保真度问题。

（6）Coordinate Injection 是针对性修复。摘要说它在 cold-start trajectories 中修正空间操作；片段还显示推理时对未校正坐标应用 Coordinate-Injection correction。合理推断是：cold-start 轨迹中的坐标被注入正确值或校正值，使模型在 SFT 阶段看到空间一致的操作；推理时再做一次校正，减少坐标漂移。具体是训练时替换坐标、推理时缩放坐标，还是两者结合，片段不足以完全确认。

（7）grounding-aware process reward 监督中间空间保真与最终答案正确性。与只奖励最终答案不同，过程奖励要求中间 grounding 操作（例如框、点、区域）与真实空间目标一致，同时最终答案也要正确。这样可以抑制“答案蒙对但证据错误”的轨迹，也可以缓解多轮错误传播。代价是需要中间 grounding 监督，标注成本和奖励设计复杂度更高。

（8）整体技术权衡。PerceptRL 的优点是保留 caption 的信息密度，同时把监督转成可验证形式；用多轮工具补单轮感知的细粒度不足；用坐标校正和过程奖励处理 agentic 轨迹的空间错误。代价是流水线复杂：多智能体验证、两套 QA 构造、两阶段 RL、多轮工具执行和过程奖励都增加工程与计算成本。论文需要证明这些组件各自有增益，而不是靠更大数据或更长训练取得效果；当前证据只能确认设计存在，完整消融需查原文。

Q4: 论文做了哪些实验？

（1）评测范围。论文在七个 benchmark 上评估，并强调共享 tool-use 设定，即不同方法在相同工具接口下比较，以减少工具可用性差异带来的混淆。片段明确出现 RealWorldQA、MMStar、BLINK、ERQA，以及两个 counting benchmark。还出现 RealWorldCounting、Color、Specialized Spatial、State 等条目及样本量数字，例如 RealWorldCounting 14,109/9,783、Color 3,710/377、Specialized Spatial 3,965/1,150、State 3,331/2,483；这些可能是数据集统计或子集规模，但表格残片无法确认列义，不能当作实验结论。

（2）对比对象。片段提到 Gemini-3 Flash 在 MMStar、BLINK、ERQA 上领先，也提到 Claude-Opus-4.6/4.7/4.8 的预测被纳入比较。这说明论文既与强闭源模型比较，也可能与同工具接口下的基线比较。具体 baseline 列表、模型规模和训练数据条件需回原文核对；当前证据不足以判断比较是否完全公平。

（3）主要训练设置。两个 RL 阶段使用 DAPO；训练经过 shared cold-start SFT、single-turn Perception RL、multi-turn Perception Agentic RL。片段出现 code execution、状态转移、clip 目标、token-level credit 等公式残片，说明实验涉及多轮工具执行轨迹和细粒度信用分配。具体超参数、数据规模、GPU 预算未在证据中体现。

（4）消融与变体。合理推断论文会做以下消融：是否有 chain QA 与 multiple-choice QA 两条构造；specialized route 与 general route 的单独/联合效果；单轮 Perception RL 与多轮 Agentic RL 的叠加；Coordinate Injection 的训练/推理校正；grounding-aware process reward 的有无；cold-start SFT 的必要性。片段中“Both routes (Ours)”行以及“with Python acts on uncorrected coordinates; Ours applies Coordinate-Injection correction at inference”提示至少存在 route 组合和坐标校正的对比。但这些是推断，证据中没有完整消融表。

（5）评价指标与呈现。主要是各 benchmark 准确率，可能取平均或分类汇总。片段中“Both routes (Ours)”行出现 85.2、82.6、75.1、64.7、62.8、73.8、71.9、42.1 八个数字，可能是七个 benchmark 加平均，也可能是其他列组合；不能强行映射。片段还报告两个 counting benchmark 上 6.2 个百分点的增益，以及 RealWorldQA、MMStar 上的增益，说明实验既看专项感知也看综合基准。

（6）实验设计的关键问题。第一，共享工具接口是否对所有 baseline 都真正一致，尤其是坐标校正和工具调用次数是否受限；第二，感知增益是否与推理增益分离，是否有仅推理训练的控制组；第三，多轮 agentic RL 的收益是否来自更多工具调用、更多训练 token 或过程奖励，需消融区分；第四，七个 benchmark 是否覆盖足够多的细粒度感知维度，是否存在训练集与评测集的领域重叠。当前证据不足以回答这些，需要回原文实验节核对。

Q5: 发现了什么实验现象？

（1）计数基准上的增益最明确。片段报告两个 counting benchmark 上 6.2 个百分点的提升，并称 RealWorldQA 与 MMStar 的增益说明改进不限于计数。这说明 PerceptRL 的感知奖励至少在一类可客观计数的任务上有效，并且可能迁移到综合视觉问答。但 6.2 是单基准、平均还是合计，片段有断行，需核对表格。

（2）增益具有任务依赖性。片段明确说 Gemini-3 Flash 在 MMStar、BLINK、ERQA 上领先，说明 PerceptRL 并非所有基准都超过强闭源模型。更合理的解读是：它在某些感知短板上缩小差距或取得增益，但在综合推理或分布不同的基准上仍有强基线领先。这种任务依赖性本身是重要现象：感知优化不等于全面能力提升。

（3）表格行显示明显的任务间不均衡。片段中“Both routes (Ours)”行出现 85.2 到 42.1 的数字跨度；若这些数字对应不同 benchmark，则说明模型在部分任务上很强、在某一任务上明显较弱。最低分 42.1 可能对应高难度空间、状态或多轮工具任务，但列义无法确认。这个不均衡提示：平均分不能代表所有感知维度，论文应报告分项诊断。

（4）coordinate drift 是核心反直觉现象。即使模型被训练去调用工具，它仍可能生成错误坐标；错误 crop 或 annotation 不是无害的中间步骤，而会改变后续证据，导致错误传播。这意味着 agentic 视觉感知的瓶颈不只是“会不会调用工具”，而是“空间操作是否保真”。这一观察支撑了 Coordinate Injection 与 grounding-aware process reward 的必要性。

（5）单轮 Perception RL 仍有失败案例。论文明确说 Perception Agentic RL 针对单轮 Perception RL 仍无法解决的细粒度、密集案例；这等于承认单轮 QA 奖励有覆盖上限。密集场景需要多轮局部检查，单轮答案监督无法告诉模型“应该看哪里”和“如何修正裁剪”。这是方法从单轮走向多轮的直接经验依据。

（6）两条 route 与两种 QA 形式可能互补。片段中“Both routes”作为完整方法出现，暗示 specialized 与 general route 联合使用优于单一路径；chain QA 与 multiple-choice QA 分别处理相关错误和不相关错误，理论上互补。但证据没有给出各自消融数值，不能断言每一部分都独立显著。

（7）坐标校正可能带来推理时收益。片段对比“with Python acts on uncorrected coordinates”与“Ours applies Coordinate-Injection correction at inference”，暗示校正后的工具调用优于未校正版本。但具体提升幅度、是否只在某些基准有效、是否引入额外误差，片段未说明。

（8）验证器与奖励可靠性仍是潜在失败模式。caption 错误项由多智能体验证器提取，若验证器漏检或误判，固定答案标签会把错误监督固化进 RL。片段没有报告验证 precision/recall 或人工抽检一致率。另一个风险是 reward hacking：模型可能学会迎合选项格式而非真正感知，尤其在 multiple-choice QA 中。论文需要用对抗样本、分布外测试或人工审计来排除这些解释；当前证据不足以确认已做。

（9）训练课程的效果尚未被证据拆开。共享 cold-start SFT、单轮 RL、多轮 RL 依次进行，理论上每一阶段都有作用；但片段没有给出“跳过某阶段”或“只做单轮”的完整对比。合理推断多轮阶段对密集案例贡献更大，单轮阶段对计数/颜色等更直接；这需要原文消融确认。

Q6: 有什么可以进一步探索的点？

（1）提高错误挖掘的可靠性与覆盖率。当前方法依赖多智能体从 dense caption 中提取 verified error items；可以探索主动学习、不确定性估计、自洽性投票、跨模型辩论和人类抽检，把验证器 precision/recall 作为一等指标优化。还可以从模型真实失败轨迹中在线挖掘错误，而不是只从静态 caption 构造，形成持续自我改进的感知课程。

（2）减少 QA 化的信息损失。固定答案的 chain QA 与 multiple-choice QA 便于规则奖励，但会压缩开放感知信号。可以研究混合奖励：选项奖励加 caption 事实性奖励、region-level grounding 奖励、属性级校准奖励；也可以设计多粒度 QA，让同一感知事实在不同粒度上被验证，避免只保留最容易被选项覆盖的维度。

（3）改进 agentic 感知的工具策略与坐标校准。Coordinate Injection 是一个起点，但坐标漂移可能来自图像分辨率、工具返回格式、坐标系定义和多轮状态更新。未来可以学习工具内参、做可微分坐标校准、让模型显式维护空间状态、或用执行反馈训练裁剪策略。还可以把 2D 坐标扩展到 3D、视频时间定位和多图空间关系。

（4）过程奖励的细粒度与抗奖励黑客。grounding-aware process reward 监督中间空间保真，但需要定义什么算“好”的中间证据。未来可以研究多粒度过程奖励、反事实证据奖励、轨迹级 credit assignment、以及奖励模型集成来降低 hacking。片段出现 token-level credit，说明这条线可以继续推进到更细的轨迹信用分配。

（5）跨领域迁移与 AI4Science 应用。论文的七个 benchmark 以通用 2D 图像感知为主；未来应测试医疗影像、遥感、显微镜、图表、工程图纸等高风险域。这些领域的感知错误代价高、标注昂贵、坐标/单位体系复杂，正好需要可验证感知奖励与过程监督。但论文当前没有证明这种迁移，不能直接宣称适用于科学发现。

（6）感知与推理的解耦评估。需要更系统的诊断基准，把“感知错误导致的推理失败”和“推理本身失败”分开。可以构造同一图像的感知题与推理题、干预感知证据、测量错误传播链；也可以做因果分析，看 PerceptRL 是否真的改善了证据提取，而不是让模型更会猜答案。

（7）效率与可扩展性。多智能体验证、两阶段 RL、多轮工具执行和过程奖励都昂贵。未来可以研究蒸馏、缓存、工具调用预算控制、早停、以及把感知奖励压缩成更轻量的 verifier。还需要报告训练和推理成本，否则难以判断方法在工业规模是否可行。

（8）理论理解。为什么 caption 预训练的监督会在后训练被稀释？感知错误如何在多轮工具轨迹中传播？可验证奖励在主观感知任务上的极限是什么？这些问题可以连接 representation learning、credit assignment、工具学习与多模态 grounding，形成比单一方法更长期的研究议程。

（9）安全与失败模式。视觉感知奖励可能被表面线索 hack，坐标错误在高风险场景会传播。未来应研究感知不确定性校准、拒答机制、工具执行沙箱、以及对抗性图像下的鲁棒性。当前论文的贡献主要在能力提升，安全边界尚未展开。

Q7: 总结一下论文的主要内容

论文的主线可以概括为：MLLM 的推理能力提升并不能自动修复感知瓶颈，而后训练阶段以 reasoning RL 为主，进一步稀释了早期 caption 预训练提供的视觉监督。作者因此提出 PerceptRL，把“感知”从不可验证的 caption 优化问题，改造成可验证的奖励问题，并把单轮感知扩展到多轮工具交互。

第一层是 Perception RL。作者不直接让模型生成或优化 caption，而是用多智能体流水线从 dense image captions 中抽取经过验证的感知错误项，再把对应视觉事实组织成可奖励 QA。具体有两种形式：chain QA 把相关错误串成链，保留错误之间的依赖；multiple-choice QA 把不相关错误组合成单选题，利用干扰项做固定答案判分。数据构造走两条 route：专用 route 针对高错误领域做定制验证，通用 route 用域无关流水线覆盖其他感知错误。这样既保留 caption 的信息密度，又得到固定答案标签和规则奖励，使 caption 派生监督可以进入 RL。

第二层是 Perception Agentic RL。单轮 Perception RL 仍无法处理细粒度、密集案例；模型需要多轮调用视觉工具，反复裁剪、放大和检查局部区域。但作者观察到 coordinate drift：即使模型学会调用工具，它仍会生成错误坐标；错误裁剪或标注会改变后续证据，使错误沿轨迹传播。为此，方法加入 Coordinate Injection，在 cold-start 轨迹中修正空间操作，并在推理时对坐标做校正；同时用 grounding-aware process reward 监督中间空间保真与最终答案正确性。这样，agentic RL 不只看最后答对没有，也看中间看到的证据是否空间一致。

训练课程分为共享 cold-start SFT、单轮 Perception RL、多轮 Perception Agentic RL 三个阶段。共享 cold-start 先让模型掌握工具调用和输出格式；单轮 RL 用 QA 规则奖励优化感知答案；多轮 RL 再用工具交互、坐标校正和过程奖励处理密集案例。两个 RL 阶段使用 DAPO，片段还显示 decoupled clipping 与 token-level credit 等实现细节，说明作者在多轮轨迹的稳定性与信用分配上做了工程处理。

实验在七个 benchmark 的共享 tool-use 设定下进行，覆盖 RealWorldQA、MMStar、BLINK、ERQA 以及计数、颜色、空间、状态等感知任务族。片段报告两个 counting benchmark 上 6.2 个百分点的增益，并称 RealWorldQA 与 MMStar 的提升说明收益不限于计数。完整方法行出现 85.2、82.6、75.1、64.7、62.8、73.8、71.9、42.1 等数字，显示整体较强但任务间不均衡；同时 Gemini-3 Flash 在 MMStar、BLINK、ERQA 上领先，说明并非全面 SOTA。

论文的贡献可以归为四点。第一，提出把 caption 感知错误转成可验证 QA 的 error-driven 数据构造，缓解 caption 的主观性和不可奖励性。第二，设计 chain QA 与 multiple-choice QA、专用与通用 route 的组合，兼顾相关错误、独立错误、领域精度和覆盖。第三，把感知 RL 推进到多轮 agentic 设定，识别 coordinate drift 这一关键失败模式，并用 Coordinate Injection 与 grounding-aware process reward 处理中间空间保真。第四，给出三阶段训练配方并在多个感知 benchmark 上验证。

局限也很明显。caption 错误挖掘依赖验证器质量；QA 化会压缩开放感知信号；专用 route 的领域划分和通用 route 的覆盖存在权衡；多轮工具与过程奖励成本高；坐标校正对 3D、视频、科学图像的泛化未知；七个 benchmark 仍以通用 2D 感知为主。片段中的公式和表格噪声也提醒我们，具体消融和数值映射必须回原文核对。总体而言，这篇论文的价值不在于宣称解决所有感知问题，而在于把“感知后训练如何可验证”拆成可操作的数据构造、奖励设计和 agentic 过程监督三个层面，为后续研究提供了一个较完整的系统范式。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与 agent 方向直接相关：论文把视觉感知做成多轮工具调用任务，并讨论 coordinate drift、过程奖励和坐标校正，这些都是 agentic RL 在真实工具环境中常见的空间状态与信用分配问题。

## 基本信息

- 作者：Tongkun Guan, Zhibo Yang, Mingkun Yang, Haocheng Wang, Wei Shen, Xiaokang Yang
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-10-28
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.37471`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先参考了 PDF 语义检索命中片段，但片段存在公式、表格和断行噪声，方法细节与实验数字多处以摘要和片段为准并标注了合理推断，完整消融与数值映射需回原文核对。
