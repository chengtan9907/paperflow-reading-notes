---
user_id: "cheng tan"
paper_id: 20910
arxiv_id: "2611.12411v1"
title: "PhysOPSD: Code as Privilege for Physically Plausible Video Generation"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12411v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12411v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:12:58"
---
# PhysOPSD: Code as Privilege for Physically Plausible Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：physically plausible video generation · on-policy self-distillation · executable code · privileged information

## 一句话总结

PhysOPSD 把可执行代码作为视频生成的特权物理上下文，借助 PhysOPSD-Engine 将真实物理场景转成可验证模拟程序，并以双层次物理特权和共享 rollout 噪声下的受控物理变化进行 on-policy 自蒸馏，从而在仅 3K 样本下跨骨干提升物理合理性并保持通用视频质量。

## 摘要

> physical world with intrinsic abstraction and control-
> lability. Powered by PhysOPSD-Engine, we translate
> real-world physical scenarios into verifiable simulation pro-
> While modern video generative models achieve remarkable grams. PhysOPSD then incorporates (i) dual-level physical
> visual fidelity, they remain noticeably brittle when gener- privilege to bridge the information gap via an intermediate
> ating physically plausible dynamics and complex interac-arXiv:2611.12411v1 bottleneck, alongside (ii) controlled physical variations un-
> tions. Targeted post-training is crucial, yet supervised fine- der shared rollout noise to expose causal dynamic shifts. Ex-
> tuning suffers from offline state mismatch, and reinforce- tensive experiments demonstrate that PhysOPSD substan-
> ment learning is hindered by sparse, ill-defined physical tially improves physical plausibility across diverse back-
> reward functions. On-Policy (Self-)Distillation (OPSD) bones, outperforming SFT and RL baselines with only 3K
> reconciles on-policy rollouts with dense corrective feedback, samples, while preserving general video generation quality.
> shifting the core challenge to designing effective privileged
> context. However, conventional modalities fall short: text
> prompts are inherently declarative, whereas real reference
> videos heavily entangle physical laws with visual appear-
> 1. Introduction ance without parametric control. To this end, we propose
> PhysOPSD, introducing executable code as privileged
> physical context for video generation. Unlike static vi- Recent video generation models [1, 10, 15, 40] have
> sual observations, executable code instantiates an editable made impressive strides in visual fidelity and tem-
> poral coherence, yet they remain noticeably brittle

Q1: 这篇论文试图解决什么问题？

1. 核心问题：论文针对的不是视频生成的视觉保真度，而是物理合理性，即在生成视频中保持刚体动力学、接触、碰撞、流体、光学反射等物理规律，以及多物体复杂交互的因果一致性。作者认为现代视频生成模型在视觉上已很逼真，但在这些方面明显脆弱，这限制了其作为世界模型或科学模拟工具的可靠性。2. 后训练的必要性：仅靠扩大预训练数据不足以纠正物理错误，需要针对性后训练。论文比较了三类范式：监督微调（SFT）使用离线专家视频或标注数据，但存在离线状态不匹配（offline state mismatch），即训练时的状态分布与模型推理时自己 rollout 出的状态分布不一致，误差会在自回归/扩散采样中累积；强化学习（RL）直接优化物理奖励，但物理合理性难以用稀疏、标量奖励完整定义，奖励函数设计困难且容易被策略钻空子，复杂交互的奖励尤其稀疏。3. OPSD 的潜力与瓶颈：On-Policy（Self-）Distillation 把 on-policy rollout 与密集纠正反馈结合，教师在学生自己访问的状态上给出逐 token/逐帧的纠正，从而缓解离线不匹配和奖励稀疏。但 OPSD 的核心挑战转移到“设计有效的特权上下文”（privileged context）：特权信息只在训练时可用，部署时学生只能看到文本或普通条件，因此教师必须借助特权信息产生密集监督，且这种监督要能迁移。4. 传统特权模态的不足：文本提示本质上是陈述性的（declarative），只能描述场景“是什么”，难以表达参数化因果和反事实；真实参考视频虽然包含物理规律，但把物理规律与视觉外观（纹理、光照、风格）严重纠缠，且没有参数化控制，无法回答“如果质量变大/摩擦变小会怎样”；静态视觉观察不可编辑，不能系统性地改变物理变量。5. 为什么选择可执行代码：可执行代码实例化一个可编辑的物理世界，具有内在抽象性和可控性；代码可以参数化（质量、摩擦、重力、材质）、可执行、可验证，能生成反事实配对；与像素视频相比，代码/模拟程序更接近物理因果的抽象表示。6. 由此产生的技术难点：如何把代码执行结果转成学生可消化的密集监督；如何设计中间瓶颈，既桥接代码特权与学生文本条件之间的信息差距，又避免学生直接复制模拟器的视觉外观；如何在共享 rollout 噪声下施加受控物理变化，让学生看到同一初始噪声下因物理参数不同而产生的动态差异，从而学习因果而非外观捷径；如何保持通用视频生成质量不退化。7. 隐含假设与边界：论文假设真实世界物理场景可以被程序化翻译为可验证模拟程序；模拟器保真度足以提供有用监督；自动代码生成（片段提到 Codex 与 GPT-6-Astra）足够可靠；物理规律可以通过参数化反事实被识别。若这些假设不成立，特权信息可能带来错误监督或 sim-to-real 差距。8. 证据缺口：检索片段未包含完整的 Related Work、方法公式和实验表格，因此无法核实具体物理定律覆盖范围、奖励/损失定义、失败率和计算成本；上述部分机制属于基于摘要与结论片段的合理推断，需回原文确认。

Q2: 有哪些相关研究？

1. 视频生成与物理合理性：论文引言引用多条视频生成工作（摘要片段显示编号 [1,10,15,40]），但检索片段未包含完整参考文献列表，无法逐一对应具体模型。可确认的是，作者把当前视频生成模型的能力概括为视觉保真度和时间连贯性进步显著，但物理动力学与复杂交互仍然脆弱。这与近年的视频扩散/流匹配模型、自回归视频生成以及世界模型研究形成对话。2. 物理基准与评估：片段明确提到沿用 PhyT2V 的 pipeline 进行评估，并在 200 个动作类别上考察物理常识（physical commonsense），同时覆盖 T2V 和 V2V 设置。还出现“w/ Refined Text”变体，说明作者比较了文本提示精炼前后的条件设置。这属于物理视频生成评估的邻近工作，但片段未给出完整基准名称和指标定义。3. 后训练范式：论文把 SFT 与 RL 作为主要对照。SFT 的离线状态不匹配问题与 imitation learning 中的 distribution shift / compounding error 相关；RL 的稀疏、定义不清奖励与 RLHF、RLVR 以及物理奖励设计相关。OPSD 则连接 on-policy distillation、self-distillation、dense corrective feedback 等研究。片段中的算法伪代码出现 AdamW、ClipGradNorm 和 EMA 教师更新（¯θ←β¯θ+(1−β)θ），说明训练实现带有教师-学生蒸馏的典型结构。4. 特权信息蒸馏：PhysOPSD 的核心概念“特权上下文”与 privileged information distillation、teacher with privileged inputs、imitation learning with privileged information 一脉相承。区别在于，论文把特权信息从文本/参考视频扩展到可执行代码与模拟程序，并强调通过中间瓶颈桥接信息差距。5. 代码与程序化监督：论文与 code-as-policy、程序化场景生成、物理引擎/可微分模拟、sim-to-real 等方向相邻。片段提到 PhysOPSD-Engine 把真实世界物理场景翻译成可验证模拟程序，并出现“dense geometric and abstract wireframe draft conditioning”，说明方法借用了几何草稿、线框草稿等条件控制思想，与视频生成中的 depth/normal/optical flow/wireframe 条件控制相关。6. 条件视频生成与可控性：片段显示评估涉及 T2V 和 V2V 描述、物体/装置/支撑/容器/开口/空间关系的保持，以及透明容器与液体的区分。这连接了文本到视频、视频到视频、结构化提示和物理场景描述的研究。7. 与 coding agent 的关系：结论明确希望“code-as-privilege”启发未来研究，用 coding agent 赋能生成模型而不是取代它们。这与 LLM 代码生成、工具调用、agentic simulation 构建等方向交叉。8. 证据缺口：由于检索片段缺少完整的 Related Work 章节，以上分类是基于摘要、结论和片段关键词的邻近研究定位，不代表论文实际引用列表；具体对比方法、引用编号和基准名称需回原文核对。

Q3: 论文如何解决这个问题？

1. 总体框架：PhysOPSD 是一个后训练框架，不是单一视频生成模型。它把可执行代码作为特权物理上下文，嵌入 on-policy self-distillation 循环。整体可以理解为三步：PhysOPSD-Engine 将真实世界物理场景翻译成可验证的模拟程序；执行程序得到模拟场景、渲染草稿和物理参数化结果；这些结果被转换为双层次物理特权，教师在学生 on-policy rollout 上提供密集纠正，学生最终只依赖文本/普通条件部署。2. PhysOPSD-Engine：片段显示引擎涉及场景到代码的生成，可能使用 coding model（片段出现“Codex”框架和“GPT-6-Astra”编码模型）生成可执行程序，再执行并渲染。引擎的目标是“可验证的模拟程序”，即代码不仅要能跑，还要能检查物理场景是否按预期构建。片段还出现“dense geometric and abstract wireframe draft conditioning”，说明引擎输出的特权可能包括密集几何草稿（如深度、法线、分割、光流等）和抽象线框草稿（如物体骨架、边缘、结构线框）。3. 双层次物理特权：论文明确说“dual-level physical privilege”，但检索片段只给出“dense geometric”和“abstract wireframe draft”两个词。合理推断：第一层是密集几何/像素对齐的模拟草稿，提供接触、遮挡、运动轨迹等细粒度物理线索；第二层是抽象线框/结构表示，提供物体拓扑、关节、支撑关系等高层因果结构。两层互补，避免单一表示过强或过弱。4. 中间瓶颈：论文强调“via an intermediate bottleneck”桥接信息差距。合理推断：教师看到完整的代码、模拟视频、几何草稿和线框，学生只看到文本/噪声；如果直接把模拟视频作为蒸馏目标，学生可能复制外观而非物理；中间瓶颈把特权信息压缩成低维表示（如 latent token、结构化特征、物理参数嵌入），只保留因果相关部分。瓶颈强度是核心权衡：太强则信息不足，太弱则泄漏外观/捷径。5. 共享 rollout 噪声下的受控物理变化：论文提出在 shared rollout noise 下施加 controlled physical variations，以暴露 causal dynamic shifts。合理推断：对同一初始噪声和同一文本条件，改变模拟程序中的物理参数（质量、摩擦、重力、弹性、材质、容器开口等），生成一对或多对模拟结果；学生模型在相同噪声下 rollout，教师用配对差异告诉学生“哪个动态变化是由哪个物理变量引起的”。这种设计把学习信号从静态外观转向因果动态，避免模型利用视觉相关性作弊。6. On-policy self-distillation 训练：学生模型首先在文本条件下生成 rollout；教师（可能由学生 EMA 构成，片段出现 ¯θ←β¯θ+(1−β)θ）在特权上下文（代码/模拟/草稿）辅助下对同一 rollout 给出密集纠正；损失拉近学生与教师在去噪轨迹或 token 分布上的差异。片段中的算法伪代码还出现 AdamW、ClipGradNorm(θ,1.0)，说明训练使用标准优化与梯度裁剪。与 RL 不同，不需要手工物理奖励；与 SFT 不同，训练状态来自学生自己的 on-policy 分布，减少离线状态不匹配。7. 代码作为特权的优势：可编辑（改参数就能生成反事实）、可验证（程序执行结果可检查）、可抽象（代码结构接近因果变量）、可扩展（不同物理引擎/场景可插拔）。论文还强调代码实例化“editable physical world with intrinsic abstraction and controllability”，这是它与文本提示、参考视频的关键区别。8. 部署阶段：学生模型不再需要代码、模拟器或教师，只接收文本/视频条件，因此方法可以嫁接到多种视频生成骨干。论文称在 diverse backbones 上验证，说明框架与具体骨干解耦。9. 关键设计权衡与风险：代码生成错误会传播；模拟器保真度决定监督上限；中间瓶颈可能损失细节；共享噪声配对需要可参数化物理场景；教师特权过强会导致学生依赖特权；训练计算成本包括代码执行、渲染和教师前向。片段未给出这些权衡的定量消融，需回原文确认。

Q4: 论文做了哪些实验？

1. 实验目标：验证 PhysOPSD 是否能在多种视频生成骨干上提升物理合理性，是否比 SFT 和 RL 更样本高效，是否保持通用视频生成质量，以及双层次特权和共享噪声受控变化是否有效。2. 骨干与设置：论文明确说在 diverse backbones 上实验，但检索片段未列出具体骨干名称。可确认的是覆盖 T2V 和 V2V 两种设置：片段出现“between T2V and V2V descriptions”，说明作者同时考察文本到视频和视频到视频条件。3. 数据预算：核心声明是仅用 3K 样本（“only 3K samples”）就优于 SFT 和 RL 基线。片段还出现“practical budget of 3K samples”和“sample efficiency”表述。4. 基线：SFT 和 RL 是主要对照。论文论证 SFT 有离线状态不匹配、RL 有稀疏/定义不清奖励，因此 PhysOPSD 作为 OPSD 框架与二者比较。片段未给出基线具体配置、数据量是否对齐、RL 奖励函数细节。5. 评估基准：片段明确提到“standard physics benchmarks”和“physical commonsense across 200 action categories”，并说“Following [42], we assess ... pipeline from PhyT2V on the...”。合理推断：作者使用 PhyT2V 相关 pipeline，在 200 个动作类别上评估物理常识，可能结合自动指标与人类评估；但片段未给出指标名称、评分细则和数值。6. 条件变体：片段出现“w/ Refined Text”，说明实验比较了文本提示精炼前后的效果；还出现“attributes consistent across variants and between T2V and V2V descriptions”，以及“Preserve actual objects, apparatus, supports, containers, openings, and spatial relationships. Distinguish transparent containers from their liquid...”，这些像是场景描述/数据构建指南，用于确保不同变体之间物体属性一致、保持装置/支撑/容器/开口/空间关系，并区分透明容器与液体。7. 定性示例：片段展示多个场景：茶壶放在旋转展示台上，镜子中的反射应同步移动；锤子砸混凝土块应开裂；投手倾倒红色液体应形成稳定流；UniVideo 等。这些例子用于展示基线失败和 PhysOPSD 改进，但属于定性证据。8. 消融：论文很可能包含双层次特权、共享 rollout 噪声、中间瓶颈、样本量的消融，但检索片段只提到“dense geometric and abstract wireframe draft conditioning”均能可靠合成物理一致动态，未给出完整消融表。9. 通用质量：论文声称“preserving general video generation quality”，但片段未给出 FVD、VBench、人类偏好等具体指标。10. 证据缺口：没有具体数值、置信区间、骨干清单、训练时长、计算资源、失败率、人类评估协议；上述实验结构部分来自摘要与结论的明确陈述，部分为合理推断，需回原文核对。

Q5: 发现了什么实验现象？

1. 样本效率是核心现象：论文反复强调仅 3K 样本即优于 SFT 和 RL 基线。这说明在 on-policy 蒸馏框架下，密集纠正反馈可能比大量离线监督或稀疏奖励更有效。需要核对的是：3K 样本是否与基线同预算、是否经过挑选、是否覆盖 200 个动作类别。2. 跨骨干一致性：论文称在 diverse backbones 上显著提升物理合理性，暗示方法不是绑定某个特定视频生成模型。若成立，这是框架级贡献；但片段未列出骨干名称和每骨干的提升幅度，无法判断是否存在“部分骨干提升、部分持平”的异质性。3. 物理指标与通用质量的张力：论文声称在提升物理合理性的同时保持通用视频生成质量。这对抗了“物理后训练会牺牲视觉美感/多样性”的常见担忧。但检索片段未给出通用质量指标的具体数值，无法判断“保持”是严格不下降还是在容差内。4. 双层次条件的有效性：片段明确说“reliably synthesizes physically consistent dynamics under both dense geometric and abstract wireframe draft conditioning”，说明密集几何草稿和抽象线框草稿两种条件都能带来物理一致动态。可能的消融趋势是：两者互补，密集几何负责接触/遮挡/运动，线框负责结构/拓扑；但片段没有给出单独使用任一层的对比。5. 共享噪声下受控变化的作用：论文强调“controlled physical variations under shared rollout noise to expose causal dynamic shifts”。实验现象应是：同一初始噪声下，改变物理参数会产生可预测的动态差异，而模型若能捕获因果，就会在配对样本上表现一致；若只学外观，则会在反事实配对中失败。片段未给出配对准确率或因果指标。6. 定性失败案例：片段中的茶壶/镜子例子显示，基线可能生成与镜子反射方向相反的旋转（“The teapot's reflection in the mirror rotates in the opposite direction”），而加入 PhysOPSD 后反射应同步移动。锤子砸混凝土、倒水等例子展示接触、断裂、流体连续性。这些是论文精选的定性证据，不能替代统计结果。7. 复杂场景的长尾困难：片段提到透明容器与液体的区分、物体/装置/支撑/容器/开口/空间关系的保持，说明评估关注组合场景和物理属性绑定。这类场景容易暴露模型在透明材质、容器开口、液体体积守恒等方面的失败。8. 负结果与失败模式：检索片段未提供负结果或失败率。合理推测的失败模式包括：代码生成错误导致模拟程序与文本不符；模拟器无法覆盖的物理现象（复杂流体、软体、光学）产生错误监督；中间瓶颈过强导致纠正信号不足；共享噪声配对中物理参数变化引起外观变化，学生可能仍然走捷径；长视频误差累积。9. 指标间张力：物理合理性指标（如接触正确性、运动轨迹）与视觉质量指标（如清晰度、美学）可能不同步；论文称二者兼得，但需要看 per-backbone 表格和人类评估。10. 证据缺口：没有具体数值、方差、显著性检验、消融表、失败案例分析；上述现象中，样本效率、跨骨干、双层次条件、定性例子有片段支持，因果配对和失败模式多为合理推断。

Q6: 有什么可以进一步探索的点？

1. 扩展物理覆盖：当前方法依赖 PhysOPSD-Engine 可程序化的场景。未来可以扩展到更复杂的物理现象：可变形体、软体、破碎、颗粒、流体、气泡、热力学、电磁、复杂光学（反射/折射/焦散）、多物体接触与关节约束。每类现象都需要模拟器保真度和代码生成模板的支持。2. 提高模拟器保真度与可微性：如果模拟器与渲染器可微分，特权信息可以更细粒度地回传；可以研究可微分物理引擎与视频扩散模型的联合训练，减少 sim-to-real 差距。3. 代码生成可靠性：PhysOPSD 的关键瓶颈是自动把场景转成可验证程序。未来可以用更强的 coding agent、自动单元测试、物理约束检查、执行反馈修复、程序合成搜索来提高代码成功率；还可以研究代码失败时的回退机制和不确定性估计。4. 特权上下文的自动化与多模态化：除了代码，可以融合深度、法线、光流、分割、物体状态轨迹、物理参数向量等多模态特权；研究如何自动从真实视频反推模拟程序（inverse graphics / program induction），减少人工场景构建。5. 中间瓶颈的理论与设计：需要理解特权信息量、瓶颈容量与泛化之间的权衡；可以借鉴信息瓶颈、因果表示学习、privileged information distillation 的理论，设计只保留因果变量的瓶颈。6. 训练效率与规模化：当前 3K 样本很高效，但执行模拟、渲染草稿、教师前向的成本可能很高；未来可以研究缓存、蒸馏到更小教师、异步 rollout、课程学习、主动选择最有信息量的物理反事实。7. 评估体系：需要更细粒度的物理基准，包括因果配对指标、反事实一致性、接触/支撑/容器/液体守恒、长视频误差累积、跨域泛化；还需要人类评估与自动指标的相关性研究。8. 与 RL 结合：代码模拟器可以作为 RL 环境或奖励模型，提供密集物理奖励；OPSD 与 RL 的混合可能兼顾密集纠正和长期目标。9. 长视频与交互式世界模型：将方法扩展到长时程生成、交互式视频、可控相机、多智能体交互；研究物理一致性在长时间 rollout 中是否仍保持。10. 科学与工程应用：用可验证模拟程序监督视频生成，可迁移到机器人操作视频、实验教学、生物/化学/物理实验模拟、天气与流体仿真等 AI for science 场景；需要领域专家设计模拟器和评估。11. 安全与可靠性：执行自动生成代码有安全风险；模拟器可能带有偏见或简化；需要沙箱、代码审计、物理合理性证书。12. 理论问题：为什么代码特权比文本/视频特权更有效？信息瓶颈如何影响因果迁移？on-policy 分布如何覆盖物理反事实？这些值得形式化研究。

Q7: 总结一下论文的主要内容

1. 论文要解决的问题：现代视频生成模型在视觉保真度和时间连贯性上进步显著，但在物理上合理的动力学和复杂交互上仍然脆弱。作者认为，仅靠扩大预训练不够，需要针对性后训练；但 SFT 存在离线状态不匹配，RL 存在稀疏且定义不清的物理奖励。OPSD 把 on-policy rollout 与密集纠正反馈结合，缓解了上述问题，但把核心挑战转移到如何设计有效的特权上下文。2. 关键洞察：传统特权模态不够用。文本提示是陈述性的，难以表达参数化因果；真实参考视频把物理规律与视觉外观纠缠在一起，且没有参数控制；静态视觉观察不可编辑。可执行代码则实例化一个可编辑的物理世界，具有内在抽象性和可控性，可以参数化、执行、验证和生成反事实。3. 方法：作者提出 PhysOPSD，把可执行代码作为视频生成的特权物理上下文。PhysOPSD-Engine 将真实世界物理场景翻译成可验证的模拟程序，并生成模拟场景与渲染草稿。PhysOPSD 包含两个核心组件：双层次物理特权，通过中间瓶颈桥接信息差距；共享 rollout 噪声下的受控物理变化，以暴露因果动态变化。训练采用 on-policy self-distillation：学生在文本条件下 rollout，教师在代码/模拟/草稿特权辅助下提供密集纠正，学生最终只依赖文本部署。片段显示教师可能采用 EMA 更新，训练使用 AdamW 和梯度裁剪。4. 实验：论文在多种视频生成骨干上验证，使用标准物理基准，覆盖 T2V 和 V2V，评估 200 个动作类别的物理常识，并沿用 PhyT2V 的 pipeline。核心结果是仅用 3K 样本就优于 SFT 和 RL 基线，同时保持通用视频生成质量。定性例子包括茶壶在镜子前的反射同步、锤子砸混凝土开裂、投手倒红色液体等。片段还显示 dense geometric 和 abstract wireframe 两种草稿条件都能可靠合成物理一致动态。5. 贡献与意义：论文把“代码作为特权”提升为一种后训练范式，强调 coding agent 应赋能生成模型而不是取代它们；框架与骨干解耦，具有通用性；样本效率高，适合数据稀缺的物理场景。6. 局限与边界：方法依赖代码生成和模拟器保真度，存在 sim-to-real 差距；中间瓶颈和共享噪声配对需要精细设计；评估覆盖有限，长视频、开放世界、复杂流体/光学仍待验证；检索片段缺少具体数值、消融和失败率，需回原文核对。7. 结论：PhysOPSD 展示了用可执行代码提供密集物理监督的可行性，为物理 grounded 的视频生成和世界模型研究提供了新方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关：物理合理性是视频生成从视觉逼真走向世界模型的关键瓶颈。

## 基本信息

- 作者：Harold Haodong Chen, Rui Peng, Lingteng Qiu, Kejie Qiu, Chao Xu, Zilong Dong
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12411v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的摘要与结论片段，并对全文缺失导致的实验数值、消融和基线细节缺口做了行内标注。
