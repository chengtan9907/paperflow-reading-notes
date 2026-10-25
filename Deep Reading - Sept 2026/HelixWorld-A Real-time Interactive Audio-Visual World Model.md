---
user_id: "cheng tan"
paper_id: 14301
arxiv_id: "2609.38123v1"
title: "HelixWorld: A Real-time Interactive Audio-Visual World Model"
publish_date: "2026-09-29"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/pdf/Daily Note 2026/arXiv - Sept 2026/2609.38123v1.pdf"
pdf_url: "https://arxiv.org/pdf/2609.38123v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Sept 2026.md"
saved_at: "2026-10-12T12:54:52"
---
# HelixWorld: A Real-time Interactive Audio-Visual World Model

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：interactive world model · audio-visual generation · spatial audio · distillation

## 一句话总结

HelixWorld 提出一种实时交互式音视频世界模型，让视觉场景与相机姿态对应的空间立体声在用户控制下共同演化，并通过教师-学生两阶段训练在单 GPU 上实现 24 FPS 的流式生成，同时引入 HelixBench 对空间声学一致性进行系统评估。

## 摘要

> World simulation is inherently multisensory, demanding synchronized visual and acoustic dynamics in real time. Yet prevailing interactive world models remain strictly silent, focusing exclusively on visual rendering and control while overlooking the acoustic dimension. We present HelixWorld, a real-time interactive audio-visual world model where visual scenes and camera-grounded spatial stereo sound co-evolve natively under user interaction. We curate a high-fidelity spatial audio-visual dataset with true stereo acoustics and metric camera poses, upon which we pre-train a bidirectional teacher conditioned on 6-DoF camera trajectories and user actions. To enable low-latency causal interaction, we distill the teacher into a few-step streaming student via an online trajectory distillation loss, sustaining drift-free joint audio-visual rollouts at 24 FPS on a single GPU. Furthermore, we formalize spatial-acoustic consistency and introduce HelixBench to evaluate whether synthesized sound fields faithfully track dynamic viewpoint motion. Extensive experiments demonstrate that HelixWorld matches state-of-the-art silent world models in visual fidelity and responsiveness, while significantly surpassing existing baselines in camera-aligned spatial-acoustic immersion.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：交互式世界模型目前只建模视觉，忽略了物理交互中自然产生且承载关键信息的声学维度，导致虚拟环境缺乏沉浸感。具体而言，用户通过键盘动作或相机轨迹控制生成式 rollout 时，期望看到视觉场景的同时听到随视角变化、具有空间位置感的立体声，但现有方法无法做到。

论文将挑战拆分为三个层面：
1. 数据层面：缺乏高质量、与相机轨迹配对的空间音频数据，即同时包含真实立体声、度量相机位姿和可控交互信号的音视频数据，使得训练空间声学对齐的模型无从谈起。
2. 建模层面：简单改造现有模型无法奏效。级联式视频到音频（video-to-audio）方法（如文献[6,39]）只能以生成好的视频为输入，无法访问相机轨迹和用户动作，因此声音对自我运动（ego-motion）不敏感，而且串行处理带来顺序延迟，无法满足实时交互；双向音视频基础模型（如文献[11,13]）虽然联合建模两种模态，但缺少外部控制接口，无法接受用户动作和相机轨迹输入。
3. 训练/推理层面：把这些双向模型强行转换为因果自回归 rollout 会导致灾难性的误差累积和训练不稳定，长时程流式生成中会出现视觉退化与声学衰减（即漂移），无法维持长时间的同步生成。

因此论文要解决的是一个综合问题：在实现低延迟因果交互的同时，保证视觉与空间声学的联合一致性，特别是声音发射器位置、声场随相机视角运动的变化要与视觉内容精细对齐。技术上对应三个目标：可响应动作的空间音频视频联合生成、可实时流式的因果生成、以及可量化的空间声学一致性评估。论文还指出，尽管已有工作如 EchoWM [52] 开始把音频引入世界模型，但在动态相机运动下实现细粒度空间声学对齐仍是未解决的挑战，这构成了本文的切入点。

Q2: 有哪些相关研究？

论文将相关工作归纳为几条主线：
1. 交互式世界模型：已有大量工作（文献[1,3,7,12,23,37,40,53]）通过键盘动作或相机轨迹实时控制生成式 rollout，用于物理仿真和虚拟环境。这类工作几乎全部聚焦视觉流，代表方法包括可接受 6-DoF 相机轨迹和离散动作的视频扩散模型（如 Sun et al. [37]），其动作通常被离散化为有限词表（如 9×9 网格的平移与视角旋转速度 bin，文献[23,37]）。这些模型证明了视觉层面的可控生成是可行的，但完全忽略了音频。
2. 可控相机轨迹的视频生成：相机姿态通过投影相关位置编码（PRoPE）[24] 注入视觉 Transformer 的注意力中，本文的低层条件注入方法直接继承了这一技术路线，并扩展到音视频联合模型。
3. 视频到音频生成（V2A）：级联方法（文献[6,39]）从生成视频中合成声音，本文实验中也构造了基于 V2A 模型（文献[28,29,39]）的配音基线。这类方法的问题在于：声音无法感知相机运动和用户动作，且串行生成延迟大，无法满足实时交互；它们也没有把声音绑定到空间中的发射器位置。
4. 双向音视频基础模型：文献[11,13] 等联合建模音频与视频的生成模型具备跨模态理解与生成能力，但缺少外部控制接口，且直接转为因果自回归 rollout 会遭遇严重的不稳定性。
5. 含音频的世界模型：EchoWM [52] 是目前唯一支持音视频生成的世界模型，但它只能保留粗略的空间线索，声学质量和语义绑定不足，说明仅仅在视觉世界模型上叠加音频表征并不足以获得精细的空间声学一致性。
6. 流式/长时程生成技术：论文引用了流式长时调优方法（streaming long tuning [46]），通过分段 rollout 和去耦的 KV cache 管理来抑制长期推理中的漂移，本文将这一思想迁移到音视频联合生成场景。
7. 蒸馏加速：少步生成与分布匹配蒸馏（如 DMD loss）是扩散模型实时化的常用手段，本文将其改造为在线轨迹蒸馏损失，用于把双向教师蒸馏为面向因果交互的少步流式学生。

整体来看，本文处在交互式世界模型、视频生成、音频生成与流式推理多条研究线的交叉点，其差异化在于同时强调“交互控制”、“空间声学对齐”和“实时流式”三个属性，而此前的工作往往只覆盖其中一两个。

Q3: 论文如何解决这个问题？

HelixWorld 采用两阶段训练流水线，目标是同时满足动作跟随（控制视觉动态与空间声音）和因果交互（实时生成）两个要求。

第一阶段：双向教师训练（Bidirectional Teacher Training）
- 训练数据：作者在自建的高保真空间音视频数据集上训练，该数据集包含真实立体声（true stereo acoustics）和度量相机位姿（metric camera poses），并经过“计算优先”（compute-prioritized）的数据整理流程，以保证连续动态、真实立体声和同步控制信号的可用性。
- 条件注入：模型同时接受连续相机轨迹和离散用户动作作为控制输入。连续 6-DoF 相机位姿通过投影相关位置编码（PRoPE）[24] 注入：对于第 t 帧，用投影矩阵 Pt = diag(Kf,t,1)Wt ∈ R^{4×4} 变换视觉注意力中的 query 和 key，使注意力机制感知相机几何。离散动作用 81 类词表表示，覆盖 9×9 网格的平移与视角旋转速度分箱（文献[23,37]）；每个时刻的动作索引 at 被嵌入为向量，与扩散模型的时间步嵌入一起通过 AdaLN-Zero 调制（h_cond,t = Emb_sigma(sigma) + MLP_phi(a_t)）注入视频分支，再通过跨模态注意力传播到音频分支。这样视觉分支和音频分支都获得统一的交互控制信号。
- 渐进式训练：为避免直接在未对齐的控制信号上全量微调导致训练不稳定，论文采用三步渐进策略：第一步只训练控制相关模块（action MLP、PRoPE、AdaLN-Zero），第二步加入视频分支的联合训练，第三步才进行完整的音视频联合训练，使模型逐步建立动作、视觉与音频之间的对齐。

第二阶段：流式学生蒸馏（Few-step Streaming Student Distillation）
- 在线轨迹蒸馏损失：为了把双向教师的能力转化为低延迟因果交互，论文引入在线轨迹蒸馏损失（online trajectory distillation loss，对应 DMD loss），在教师-学生联合 rollout 过程中持续对齐轨迹分布，使学生模型只需少数几步去噪即可生成。
- 流式长时程调优：为了抑制长时间 rollout 中的视觉退化和声学衰减，论文采用流式长时调优 [46]：学生模型按段（segment）展开序列，维护窗口化的 KV cache——区分历史（History）、当前段（Current）和已驱逐的 Sink Token，当前段参与反向传播，历史部分的 KV 被去耦（detached），从而在有限显存内进行长序列训练。
- 因果推理结构：学生模型被设计为支持流式因果生成，即每一帧/每一段只依赖过去的信息，不依赖未来，从而支持实时交互。

评估体系：HelixBench
- 论文将空间声学一致性（spatial-acoustic consistency）形式化为交互式世界模型区别于被动生成模型的基本准则，并提出 HelixBench 基准，包含 1,015 个人工验证的交互案例，从空间声学一致性、时间同步和多模态语义三个维度评估交互式音视频生成的音频-视觉对齐质量。

总体而言，论文的解决方案是：用“数据整理 + 渐进式教师训练”建立空间声学与交互控制的对齐能力，再用“在线轨迹蒸馏 + 流式长时调优”把这种能力转化为单 GPU 上可实时运行的因果流式生成器，最后用专门设计的多维基准来验证空间声学一致性。

Q4: 论文做了哪些实验？

论文的实验围绕三个问题展开：视觉质量与控制响应是否达到静默世界模型的水平；音频保真度与音视频对齐是否优于现有音视频生成基线；空间声学一致性是否真正成立。

1. 实现与训练配置：HelixWorld 基于开源模型 LTX-2.3 构建，遵循两阶段训练流程（教师训练 + 学生蒸馏），完整实现细节放在附录 B。

2. 基线对比：
- 视觉/控制基线：比较多个强基线（文献[5,8,9,35,37,43,50,55]），其中大多数是开源的静默世界模型；
- 音视频世界模型基线：EchoWM [52]，是目前唯一支持音视频生成的世界模型；
- 级联配音基线：使用视频到音频（V2A）模型（文献[28,29,39]）对 HelixWorld 的输出进行静音再配音实验，即把 HelixWorld 生成的视频静音后用外部 V2A 模型重新配音，保持视觉内容不变，以隔离音频生成性能。

3. 评估协议：
- 视频质量与控制响应：在 WBench 导航分割集 [51] 上评测，关注视觉质量、行动跟随（action following）与物理合理性；
- 音频质量与多模态一致性：在 HelixBench 上评测，覆盖音频保真度（audio fidelity）、时间同步（temporal sync）、语义对齐（semantic alignment）和空间一致性（spatial consistency）四个维度。

4. 主要结果：
- 在 WBench 导航上，HelixWorld 在视觉质量和控制响应方面优于大多数开源静默基线，展现出可靠的行动跟随和物理合理性；
- 在 HelixBench 上，HelixWorld 同时超过 EchoWM 和级联配音流水线；
- 级联 V2A 模型虽然可以达到合理的文本/语义对齐，但无法捕获声音发射器的位置，空间一致性得分为负；
- EchoWM 保留了粗略的空间线索，但在声学质量和语义绑定上落后。

5. 消融与稳定性验证：方法部分提到，长时程流式生成中仍会出现视觉退化和声学衰减，流式长时调优（streaming long tuning）通过分段展开和去耦 KV cache 来抑制这种漂移；在线轨迹蒸馏损失用于维持无漂移的联合音视频 rollout。论文摘要中明确报告了单 GPU 上 24 FPS 的实时性能。

注：由于当前可获取的文本不包括完整实验表格和具体数值，以上是对实验设计与结果的概括性复述；具体指标数值和消融表需要回到原文 Table 3、Table 4 及附录 D 确认。

Q5: 发现了什么实验现象？

从摘要、方法节与结论中的证据来看，实验揭示了以下可观察现象：

1. 视觉保持：HelixWorld 在视觉保真度和响应性上与现有最先进的静默世界模型持平（matches state-of-the-art silent world models），说明引入音频分支和蒸馏过程并没有显著牺牲视觉生成质量。这是一个关键的正面结果，表明“视觉+音频”联合建模可以达到与纯视觉模型相当的水平。

2. 空间声学优势：HelixWorld 在相机对齐的空间声学沉浸感上显著超越现有基线，说明其核心设计——以相机轨迹和动作为条件联合生成音视频——确实把声音绑定到了空间位置和视角运动上，而不是仅仅生成“有声音”的视频。

3. 级联 V2A 的失败模式：级联配音基线在文本/语义对齐上表现尚可，但空间一致性得分为负，无法捕获发射器位置。这是一个反直觉但重要的现象：语义正确的音频并不等于空间正确的音频；把生成视频交给外部 V2A 模型配音，声音会失去与视角运动对应的空间锚定。

4. EchoWM 的不足：EchoWM 虽然支持音视频生成，但只能保留粗略的空间线索，在声学质量和语义绑定上落后于 HelixWorld。这说明仅仅把音频通道加入世界模型而不对相机轨迹和空间声学进行显式建模，不足以获得精细的空间声学一致性。

5. 长时程漂移：方法部分明确提到，长 rollout 仍然可能出现视觉退化（visual degradation）和声学衰减（acoustic fading）。这说明联合生成中的误差累积是真实存在的风险，而论文的流式长时调优和窗口化 KV cache 设计正是针对这一现象的补救措施。这里隐含的 scaling trend 是：序列越长，漂移越明显，需要专门的流式训练而非简单截断。

6. 蒸馏稳定性：在线轨迹蒸馏损失支撑了无漂移的联合音视频 rollout，且最终达到了 24 FPS 的推理速度，说明少步学生模型在因果流式设置下没有出现模式坍塌或轨迹发散，至少在其评测范围内是稳定的。

7. 可控性：HelixWorld 在 WBench 导航上表现出可靠的行动跟随，证明 81 类离散动作词表和相机轨迹注入能够有效传导到音视频两条分支——不仅是视觉在跟随控制，空间声音也在跟随控制。

需要说明的是，当前文本中没有提供消融表的具体数值、失败案例的定性描述或指标之间的完整张力分析；这些现象主要来自摘要和结论的定性陈述，定量细节应参考原文表格与附录。

Q6: 有什么可以进一步探索的点？

基于论文现有贡献与限制，可以进一步探索的方向包括：

1. 动作与控制的扩展：把 81 类离散动作词表扩展为连续动作空间或更细粒度的行为组合（如物体交互、语音指令、多智能体协同），从而支持更自然的用户控制和更复杂的任务（例如具身智能体在环境中执行长程任务时同步听到反馈声音）。

2. 空间音频格式升级：当前使用立体声（stereo），可扩展到 Ambisonics、object-based audio 或多通道环绕声，以更精细地刻画声源方向、距离、房间混响、遮挡和材质反射，真正逼近物理声场。

3. 物理声学建模：把几何声学/波动声学先验（如射线追踪、房间脉冲响应）融入生成模型，使声音不仅与相机运动对齐，还与场景几何和物体材质一致；也可以探索神经声场表示与扩散模型的结合。

4. 评估体系强化：HelixBench 目前依赖 1,015 个人工验证案例，可扩大规模、引入更多场景类别（室内外、多声源、运动声源、遮挡场景），并设计客观可复现的度量（如基于空间音频的互相关、到达方向估计误差、心理声学模型）来补充人工评价。

5. 长时程与跨场景泛化：进一步延长可稳定生成的 rollout 时长（例如分钟级场景），并探索在未见过的场景类别、相机运动模式和动作组合上的泛化能力；研究更高效的流式记忆机制（如分层记忆或可学习的状态压缩）以替代当前窗口化 KV cache。

6. 训练范式改进：研究无需严格配对数据即可学习空间声学一致性的自监督/弱监督方案（如从大量无位姿的野外视频中提取空间音频线索），降低数据采集门槛；也可以探索更强大的蒸馏损失（如在线强化学习、对抗式蒸馏）进一步压缩步数并减少教师-学生分布偏移。

7. 下游应用融合：把 HelixWorld 接入具身智能、自动驾驶仿真、VR/AR 内容生成和游戏引擎，作为策略训练或用户测试的环境；探索如何用交互式音视频世界模型为 agent 提供多模态想象空间，辅助规划与决策。

8. 多模态一致性理论的深化：把“空间声学一致性”形式化为可计算的约束/损失，研究视觉-声学空间映射的理论关系（如相机内外参如何决定声场变换），并推广到触觉等其他模态。

9. 实时性与部署优化：在单 GPU 24 FPS 基础上，进一步探索端侧部署、低功耗推理和更高的帧率/分辨率，同时保持音频与视频的长期同步。

这些方向大多是从当前方法边界出发的合理推断，论文并未在原文中逐一讨论，具体可行性和难点需要结合实际实验验证。

Q7: 总结一下论文的主要内容

HelixWorld 的核心主张是：世界模拟本质上是多感官的，真实物理交互既产生视觉也产生承载关键信息的声音（如表面材质、接触力、屏幕外事件），因此一个真正的交互式世界模型必须让视觉场景与空间立体声在用户控制下共同演化。然而现有交互式世界模型几乎全部静默，只处理视觉渲染和控制，忽略声学维度；即便最近有工作尝试把音频引入世界模型（EchoWM），也无法在动态相机运动下实现精细的空间声学对齐。本文正是针对这一缺口，提出一个同时满足“交互控制”、“空间声学对齐”和“实时流式生成”三个属性的音视频世界模型——HelixWorld。

论文首先指出直接改造现有模型会遇到三重阻碍：级联视频到音频方法无法访问相机轨迹和用户动作，声音对自我运动不敏感，且串行延迟不能满足实时性；双向音视频基础模型缺少外部控制接口，难以接受用户输入；而把这些模型强行变成因果自回归 rollout 会导致严重的误差累积和训练不稳定。因此，HelixWorld 设计了专门的解决方案，包含数据、训练和评估三个部分。

在数据层面，作者构建了高保真空间音视频数据集，特点是包含真实立体声和度量相机位姿，可以同时提供连续动态、真实声学条件和同步控制信号。数据整理采用“计算优先”（compute-prioritized）的策略，以确保训练样本的有效性和多样性。

在训练层面，HelixWorld 采用两阶段方案。第一阶段训练双向教师模型：模型同时接收连续 6-DoF 相机轨迹和离散用户动作作为控制信号。连续相机位姿通过 PRoPE（投影相对位置编码）注入，用投影矩阵 Pt = diag(Kf,t,1)Wt 变换视觉注意力 query/key，使注意力机制感知相机几何；离散动作使用 81 类词表（9×9 网格的平移与视角旋转速度分箱），动作索引嵌入后与扩散时间步嵌入相加，再通过 AdaLN-Zero 调制注入视频分支。两条控制通路都进入视频分支，并通过跨模态注意力传播到音频分支。为防止直接全量训练不收敛，论文采用渐进式训练：先训练控制相关模块，再加入视频分支联合训练，最后进行完整音视频联合训练。第二阶段把教师模型蒸馏为少步流式学生模型：通过在线轨迹蒸馏损失（涉及 DMD loss）让学生模型在教师轨迹的指导下学习，每一步只做少数几步去噪；同时采用流式长时调优（streaming long tuning [46]），按段展开序列，用窗口化 KV cache 管理历史与当前段（历史部分去耦、当前段反向传播），以抑制长时间 rollout 中视觉退化和声学衰减。最终学生模型支持低延迟因果交互，在单 GPU 上以 24 FPS 实时运行，且能维持无漂移的联合音视频 rollout。

在评估层面，论文形式化了“空间声学一致性”（spatial-acoustic consistency），将其确立为交互式世界模型与被动生成模型之间的核心区分准则，并提出 HelixBench——一个包含 1,015 个人工验证交互案例的基准，从空间声学一致性、时间同步、语义对齐（以及音频保真度）等维度评估生成声场是否忠实跟踪动态视角运动。

实验部分，HelixWorld 基于开源 LTX-2.3 实现，与多个强基线对比：静默世界模型（文献[5,8,9,35,37,43,50,55]）、EchoWM [52]、以及用 V2A 模型（文献[28,29,39]）构建的级联配音基线（将 HelixWorld 输出静音后重新配音）。视频质量与控制响应在 WBench 导航分割集上评测，结果显示 HelixWorld 在视觉保真度和控制响应上优于大多数开源静默基线，表现出可靠的动作跟随和物理合理性。音频质量与多模态一致性在 HelixBench 上评测，结果显示 HelixWorld 在音频保真度、时间同步、语义对齐和空间一致性上均超过 EchoWM 和级联配音流水线。值得注意的是，级联 V2A 模型虽然能达到合理的文本/语义对齐，但无法捕获发射器位置，空间一致性得分为负；EchoWM 则保留了粗略空间线索，但在声学质量和语义绑定上落后。这验证了论文的核心论点：空间声学一致性不能通过简单的视觉-音频级联或粗粒度的联合建模获得，必须有意识地以相机轨迹和用户动作为条件进行联合生成。

最后，论文总结认为，HelixWorld 提供了实时交互式音视频世界模型的可行范式，其贡献包括：提出视觉与空间立体声协同演化的模型架构、构建带度量相机位姿的空间音视频数据集、设计抑制漂移的因果蒸馏流水线、形式化空间声学一致性并发布 HelixBench 评估基准。整体来看，论文在技术上打通了“可控音视频联合生成”与“实时流式推理”之间的空隙，为虚拟环境、具身智能和沉浸式交互应用提供了新的基础模型方案。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：与用户画像中的生成方向（权重 0.10）直接相关：HelixWorld 是音视频联合生成的前沿工作，特别适合关注可控多模态生成的研究者。

## 基本信息

- 作者：Lei Ke, Jiahao Pan, Zeyue Tian, Jiaming Wang, Haoyuan Huang, Kam Man Wu, Pengjun Fang, Hongyu Liu, Chenyang Qi, Lin Wang, Ruibin Yuan, Weijia Chen, Fangneng Zhan, Qifeng Chen, Wei Xue, Yike Guo
- 机构：未提供
- 来源：arxiv
- 主题/分类：未知
- 日期：2026-09-29
- 推荐级别：**按需阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2609.38123v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了检索到的论文片段（Introduction/Method/Results/Conclusion）与摘要信息，未获取完整 PDF 全文，部分细节（如具体实验数值、消融表、数据构建流程）为合理推断或标注需回原文确认。
