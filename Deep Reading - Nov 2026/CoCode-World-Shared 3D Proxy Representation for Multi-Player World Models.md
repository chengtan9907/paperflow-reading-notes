---
user_id: "cheng tan"
paper_id: 21534
arxiv_id: "2611.15602v1"
title: "CoCode-World: Shared 3D Proxy Representation for Multi-Player World Models"
publish_date: "2026-11-11"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.15602v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.15602v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-14T17:46:33"
---
# CoCode-World: Shared 3D Proxy Representation for Multi-Player World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：multiplayer world model · shared 3d proxy · video generation · cross-view consistency

## 一句话总结

CoCode-World 用共享 3D proxy 把双人世界模型的结构控制与外观合成解耦，并借助预训练视频 Transformer 的联合上下文生成两路时空一致的玩家视角。

## 摘要

> Multiplayer generative world models must produce observations that agree on shared entities despite different camera motions and occlusions. Explicit world representations provide spatial control, but precise modeling of individual game maps limits scene diversity, while shared geometry alone cannot ensure consistent appearance across views. We present CoCode-World, a shared 3D proxy framework that separates structural control from visual synthesis for two-player video generation. Programmable proxies specify scene layout, motion, and occlusion, while RGB references guide character and scene appearance. To coordinate visual details across players, we concatenate paired proxy videos, appearance references, and target views in a consistent spatial layout and generate both views jointly with a pretrained video model. This formulation uses the model's native attention for cross-view communication within a single generation branch, with LoRA adaptation learning proxy adherence and appearance correspondence from paired observations. We construct a two-player GTA~V dataset that pairs synchronized RGB observations with proxy videos derived from shared map geometry and game-engine state. Experiments demonstrate appearance transfer to characters and scene styles unseen during training, with improvements in video quality and cross-view player identity over the evaluated baseline. Ablations further examine how proxy construction and cross-view communication affect generation. Project page: https://iapple233.github.io/CCW/.

Q1: 这篇论文试图解决什么问题？

一、任务定义
论文研究的是多人生成式世界模型中的双人视频生成：给定同一 3D 场景中两名玩家不同的相机轨迹，模型要生成两段视频，使共享实体在两个视角中保持身份、外观和空间关系一致，同时各自遵循本视角的相机运动、遮挡和局部观测。与单玩家视频生成相比，这不是简单的两段独立生成，而是要求跨视角的“同一性”与视角相关的“差异性”同时成立。

二、核心矛盾
1. 空间控制与外观一致性耦合。显式 3D 表示能约束布局、运动、遮挡，但几何本身不决定纹理、角色身份和风格；只用共享几何，跨视角外观仍可能漂移。
2. 场景泛化与几何精度冲突。精细、场景特定的地图几何能提供强控制，但把模型绑在单个游戏地图上，限制可玩环境的多样性；粗粒度 proxy 更可编程、更易迁移，但可能牺牲深度、遮挡和局部结构。
3. 跨视角通信的代价。联合去噪、跨 agent attention 或共享 latent 可以在隐空间协调，但不天然提供显式空间接口；把两路视角放进同一上下文又会带来 token 和注意力开销。

三、现有路线与缺口
论文把现有多人世界模型概括为在 learned latent space 中通过 joint denoising、cross-agent attention 或 shared representation 协调玩家观测；另一条线是显式状态方法，把世界演化与视角合成分开。后者更接近本文思路，但论文指出部分方法训练时依赖详细、场景特定的地图几何，难以迁移到不同场景，限制 playable environment 的多样性。于是论文选择“粗粒度、可编程 3D proxy”作为结构与视频渲染器之间的接口：coding agent 可以构建和更新世界，视频模型补足视觉细节。

四、隐含假设
1. 粗粒度 proxy 足以承载 gameplay 所需的关键结构（布局、运动、遮挡），细粒度外观可以由 RGB 参考和生成模型补足。这个假设是全文的核心；若 proxy 丢失关键深度/遮挡线索，生成可能退化，这是需要实验验证的边界。
2. 固定左右布局的拼接能让预训练视频模型的 native attention 自动建立跨视角对应；这要求模型能从配对监督中学会“同一实体在不同视角”的对应关系。
3. 成对 RGB 监督足以学习 proxy 遵循和外观对应；但论文片段没有给出损失设计、数据增强和失败样本分布，无法判断监督信号是否覆盖遮挡、快速运动、尺度变化等困难情形。
4. 双人设定能否代表多人互动，是外部效度问题；更多玩家会带来更复杂的遮挡、上下文长度和身份对应，需另行验证。

五、评价难点
跨视角一致性不是单一指标：DOVER/LAION 衡量画质与美学，DINO Adj. 衡量相邻帧外观稳定，Edge F1 衡量生成轮廓与 proxy 的结构一致，First-frame/参考保持衡量外观参考保留；这些指标可能彼此张力。例如更高的美学分数不保证更好的几何遵循，更强的身份一致性也可能来自复制参考而非理解 3D。另一个评价难点是 gameplay 语义正确性：指标能否判断“两个视角看到的是同一个角色/物体”而不只是像素相似，片段未展示人工评估或交互正确性测试。

六、证据边界
上述任务定义和 gap 判断有 Introduction 与 Method 片段直接支持；关于损失、数据划分、基线公平性和统计显著性的判断，提供的证据不足，需回原文核对。

Q2: 有哪些相关研究？

一、生成式世界模型与可玩视频生成
论文把自身定位在生成式 world model 的延长线上：模型根据交互合成视觉观测，把视频生成推向可玩环境。Introduction 引用 Bruce et al. 2024、Valevski et al. 2025、Sun et al. 2026b 作为这条线的代表。与普通视频生成不同，世界模型强调对动作/交互的响应和持续演化；本文不直接学习状态转移，而是用游戏引擎状态构造 proxy，再让视频模型渲染，因而更接近“视频模型作为渲染器”的分工。

二、多人/多视角生成
论文将多人/多视角生成的相关工作分为几类：
1. 联合建模同步观测与动作：Solaris 在 Minecraft 中联合建模同步观测和动作（Savva et al., 2026）。
2. 跨 agent 特征交换：ShareVerse 通过 cross-agent 机制交换特征（Zhu et al., 2026；具体机制片段未展开）。
3. 联合去噪/共享表示：Liu et al. 2026、Wu et al. 2026 等通过 joint denoising、cross-agent attention 或 shared representation 在 latent 空间协调玩家观测。
这些路线的共同点是“协调发生在学习到的隐空间”，优点是端到端、无需显式地图；缺点是空间控制不直接，跨视角一致性依赖模型隐式学会。本文的差异是把结构控制外置为共享 3D proxy，把跨视角协调放回像素/token 空间的联合上下文。

三、显式状态与世界表示
Cai et al. 2026 被论文视为“把世界演化与视角合成分离”的代表；Ye et al. 2026 则被指出训练时依赖详细、场景特定的地图几何，难以迁移到不同场景。Code World Model（Chen et al., 2026a）把可执行世界状态翻译成视觉观测，是“代码/程序化世界状态驱动视频生成”的近邻。本文与之的差别在于：不要求精确建模每个游戏地图，而是用粗粒度、可编程 proxy 作为共享结构接口，并用 RGB 参考补足外观。

四、视频模型作为渲染器与参考条件生成
方法使用预训练视频 Transformer（片段中称 H3 transformer / Video DiT），通过 reference interface 接收 proxy、外观参考和目标视角，联合去噪两路视频。这条线关联到以参考图/参考视频为条件的生成、LoRA 适配和原生 attention 的上下文通信。论文没有在可见片段中详细比较其他 reference-based 视频编辑/渲染方法，因此无法判断其相对这类方法的完整优势。

五、与本文最直接的区别
本文不是提出新的 latent 协调模块，也不是追求更精细的 scene-specific 重建；它把“共享 3D proxy”作为多人世界模型与视频渲染器之间的空间接口，并用配对视频监督学习外观对应。这个定位的潜在优势是场景多样性、结构可控和外观可迁移；潜在代价是 proxy 保真度、固定布局的归纳偏置和多人扩展的 token 成本。

六、证据边界
以上分类主要来自 Introduction 和 Method 的片段；ShareVerse、Liu et al.、Wu et al. 的具体机制只出现名称级信息，不能据此展开技术细节。完整 related work 表格、基线名单和与 Code World Model 等近邻的定量比较需回原文确认。

Q3: 论文如何解决这个问题？

一、总体输入输出
给定两名玩家在同一共享 3D 场景中的相机轨迹、对应的 proxy 视频，以及成对的外观参考图，模型输出两段同步的 RGB 视频。方法把问题拆成两个接口：proxy 负责“在哪里、怎么动、谁挡住谁”，RGB 参考负责“长什么样”。生成阶段不再显式求解多视角几何，而是让预训练视频模型在联合上下文中完成渲染。

二、共享 3D proxy 的构造
论文用共享地图几何 M、每个玩家可见的实体状态 E_t(p) 和相机参数 C_t(p) 构造相机特定 proxy：P_t(p)=R(M, E_t(p), C_t(p))。其中 R 是投影/渲染过程（具体实现未在片段中展开）。proxy 是可编程的，可指定场景布局、运动与遮挡；数据集构建时从 private FiveM server 记录每个客户端的 RGB、相机参数、玩家与附近实体的身份和位姿，并导出同一坐标系下的静态地图几何。地图与持久实体 ID 用于把不同相机对同一场景的观测关联起来。动态观测保留各客户端在本地流送和网络条件下可见的状态。

三、外观参考与空间布局
RGB 参考 I 提供角色和场景的外观。方法把两名玩家的 proxy 视频、外观参考和目标视频按固定左-右布局拼成一个“画布”，使两路视角在空间上有一致排布。这个布局不是简单拼接，而是给预训练模型一个稳定的对应结构：同一实体在不同视角的代理位置、参考外观和目标区域处于可学习的位置关系中。训练时成对画布把两路观测同时暴露给每一次联合预测，模型从配对 RGB 监督中学习跨视角外观对应。

四、联合生成与跨视角通信
生成时，H3 transformer（图中也标为 Video DiT）联合去噪两路目标视角。关键设计是没有额外的 cross-agent 模块：两路视角、proxy、参考和目标都在同一上下文里，模型通过原生 attention 在单生成分支内进行跨视角通信。文本条件 c 也作为条件输入。生成完成后，对目标画布联合去噪、解码，去掉空间 padding，再裁成两名玩家的视频。这样跨视角一致性由共享上下文和配对监督共同承担。

五、LoRA 适配
backbone 是冻结的预训练视频模型，方法用 LoRA 适配，使模型学会两件事：遵循 proxy 的结构条件，以及在成对观测中建立外观对应。片段没有给出 LoRA 的 rank、插入层、学习率、训练步数和损失函数；因此只能确认“冻结主干 + LoRA”这一训练范式，不能复现具体配置。

六、设计权衡
1. 用粗粒度 proxy 代替精细地图，换取场景多样性和可编程性，但把深度/遮挡/局部结构的准确性压力转移给 proxy 构建和生成模型。
2. 用固定布局和原生 attention 代替显式跨视角模块，降低架构复杂度，但引入位置偏置和上下文长度成本。
3. 用 RGB 参考控制外观，支持未见过的角色/风格，但参考保持与身份一致性可能冲突；结论段也承认仍有 reference-preservation 误差。
4. 双人联合生成可扩展为 N 人拼接，但固定每视角分辨率和片段长度下，token 序列与注意力/显存随人数增长。

七、证据缺口
公式中的 R、实体状态编码、mask/遮挡处理、训练目标、是否使用 classifier-free guidance、推理步数等均未在提供的片段中出现；上述机制描述只到“可确认的架构关系”层面。

Q4: 论文做了哪些实验？

一、数据与 benchmark
论文构建双人 GTA V 数据集：通过 private FiveM server 采集两名玩家的同步 gameplay，记录 RGB、相机参数、玩家与附近实体身份/位姿，并导出共享静态地图几何。检索片段显示数据集含 7,846 个约 5 秒片段，类别占比为 Exploration 66.4%、Interaction 9.8%、Riding 18.7%、Fight 5.1%；这组统计对应训练集还是完整数据划分，片段未明示，需回原文核对。评测 benchmark 由 150 个双人案例组成，覆盖探索、载具骑行、玩家战斗、环境交互四类事件。proxy 轨迹采样自 GTA V gameplay，为两路视角提供同步结构条件。作者用 GPT Image 2.5 对相应参考图做风格化，改变玩家外观、背景环境和视觉风格；每个案例把风格化参考与原始 proxy 轨迹配对，测试渲染器在 appearance/scene 变化下是否保留共享 gameplay。

二、评价指标
论文遵循 SolarRenderer 的评测协议，覆盖四个维度：
1. 视频质量：DOVER。
2. 帧美学：LAION aesthetic predictor。
3. 时序外观稳定：DINO Adj.，用相邻帧 DINOv2 embedding 的余弦相似度衡量。
4. 结构遵循：Edge F1，衡量生成轮廓与 proxy 轮廓的一致性。
5. 参考保持：First-frame 指标，片段显示其用 DINOv2 相似度比较“最终一秒”与某参考，但句子被截断；具体定义需回原文确认，不能简单等同于“首帧相似度”。
这些指标分别对应画质、美学、时序稳定性、几何结构和参考保真，但没有一个直接衡量 gameplay 语义正确性（例如两视角是否真的看到同一实体、交互是否按规则发生）。

三、对比基线
Results 开头说与 video generation 和 rendering baselines 比较；Conclusion 明确点名 Seedance 2.5，并报告在录制的外观迁移案例上，DOVER、LAION aesthetic quality、edge alignment、cross-view player-identity consistency 高于 Seedance 2.5。完整 baseline 名单、各自配置和数值未在提供片段中出现，因此不能复核“相对所有基线”的提升，只能确认“相对 Seedance 2.5”的方向性结论。

四、主要结果
1. 外观迁移：摘要报告可将外观迁移到训练中未见的角色和场景风格；这说明方法不是只记住训练外观，而是能利用参考图控制角色/场景外观。但片段没有给出按类别、按风格或按参考类型的分解。
2. 视频质量与身份一致性：结论段报告 DOVER、LAION 美学、Edge F1、跨视角玩家身份一致性优于 Seedance 2.5；同时承认仍有 adjacent-frame 和 reference-preservation 误差。也就是说，不是所有指标都全面提升，画质/身份收益与稳定性/参考保持之间存在张力。
3. 定性比较：Results 提到 qualitative comparisons，但提供的是文字片段，没有图，无法判断视觉例子的代表性和失败样本比例。

五、消融
论文消融 proxy 构建和跨视角通信。检索片段中可见“Raw”的粗几何无法准确表示复杂环境中的深度关系、遮挡和局部场景结构，导致歧义并伴随图像发黑；这提示 proxy 的构建方式不是无关紧要的预处理，而是影响生成质量的关键变量。跨视角通信的消融说明作者认为共享上下文中的 native attention 对身份一致性重要；但具体去掉/替换方式、量化结果和结论强度未在片段中给出。

六、实验设计评价
优点：
1. 使用配对同步数据，能直接监督跨视角对应，而不是只做单视角生成。
2. 评测覆盖画质、美学、时序、结构、参考保持多个维度，并设置 proxy/通信消融。
3. 用风格化参考做 appearance transfer，比固定外观更能测试参考控制。
4. 公开项目页，利于复核视觉结果。

局限：
1. 只有 GTA V 一个游戏/数据源，场景多样性仍受游戏引擎和地图资产限制。
2. benchmark 150 个案例，规模有限；类别分布可能不均衡（训练统计中 Fight 仅 5.1%），但片段没有给出 benchmark 的类别分布。
3. 风格化参考由 GPT Image 2.5 生成，参考本身的质量和风格偏差会影响结论；没有看到人工评估或用户研究。
4. 指标多为感知/结构指标，缺少动作可控性、交互正确性、物理合理性、跨视角几何误差等任务级评估。
5. 片段没有给出均值/方差、显著性检验、失败率，无法判断提升幅度和稳定性。

七、证据缺口
具体数值表、baseline 配置、推理设置、训练数据划分、消融的完整设置均未提供；以上只复述片段明确出现的信息，并把不确定处标为需回原文核对。

Q5: 发现了什么实验现象？

一、外观与结构分离确实产生了可迁移性，但证据是方向性的
摘要和结论都强调对未见角色/场景风格的外观迁移，说明 RGB 参考 + proxy 的分工可以让模型在结构条件不变时替换外观。这是论文最核心的实证卖点。但片段没有给出“未见”是按角色 ID、风格类别还是场景划分，也没有按类别报告迁移成功率；因此只能确认作者声称存在这种迁移，不能量化其泛化边界。

二、跨视角身份一致性提升，但并非所有指标同步提升
结论段明确说 DOVER、LAION 美学、Edge F1、跨视角玩家身份一致性高于 Seedance 2.5，同时仍有 adjacent-frame 和 reference-preservation 误差。这揭示了一个重要张力：更好的画质/美学/身份一致性，不一定带来更好的相邻帧稳定性和参考保持。可能的原因是联合上下文让两路视角更“像”同一个角色，但固定布局、proxy 噪声或参考图风格化会引入时序抖动/参考漂移；这只是合理推断，需看原表确认。

三、proxy 构建是性能瓶颈，不是可忽略的预处理
Results 片段提到 Raw 的粗几何无法准确表示复杂环境中的深度关系、遮挡和局部场景结构，导致歧义并伴随图像发黑。这个现象很重要：结构条件并不是“越粗越鲁棒”；当 proxy 丢失关键遮挡/深度线索时，生成模型可能用黑边、错误遮挡或背景填充来补偿，反而降低画质。它支持论文选择“共享 3D proxy + 实体状态”的动机，也说明 proxy 保真度与生成质量之间存在耦合。具体是哪个消融配置、图像发黑的比例和指标变化，片段未给出。

四、跨视角通信消融指向共享上下文的作用，但贡献大小未知
论文把跨视角通信作为消融维度，说明作者认为“同一上下文中的 native attention”是身份一致性的关键，而不是仅靠 proxy 几何。若去掉联合上下文或改变布局，预期跨视角身份一致性会下降；但片段没有给出消融结果，无法判断该设计与 LoRA、参考布局各自贡献多少。这里需要回原文看消融表和定性图。

五、类别不平衡可能影响结论外推
检索片段给出的数据统计中，Exploration 占 66.4%，Interaction 9.8%，Riding 18.7%，Fight 5.1%。如果 benchmark 也接近这种分布，战斗和交互类事件样本较少，跨视角一致性在复杂动态交互上的结论可能不如探索类稳健。片段没有给出按类别的指标，因此这是一个评价设计上的合理担忧，而不是已证实的失败模式。

六、固定左右布局带来归纳偏置
方法把两路 proxy、参考和目标按固定左-右布局拼接，这让预训练模型容易建立位置对应，但也可能让模型依赖“左边是玩家 1、右边是玩家 2”的捷径，而不是真正理解 3D 对应。若测试时交换视角、改变布局或出现非对称遮挡，性能是否保持，片段未报告。这个风险是合理推断，需看是否做了 layout/permutation 消融。

七、多人扩展的 scaling 趋势是明确的负向约束
结论明确说当前限于两人；固定每视角分辨率和片段长度时，每增加一个玩家都会增加 token 序列，从而提高注意力计算和显存需求。作者还提到初步尝试表明，在激进蒸馏下保持生成质量仍有挑战。这是一个明确的 scaling 趋势：方法原理上可拼 N 人，但工程上受上下文长度限制，且蒸馏/压缩不是免费午餐。

八、缺少失败案例的系统呈现
结论承认 adjacent-frame 和 reference-preservation 误差，但片段没有失败案例图、失败类型分类或按遮挡/快速运动/参考风格极端变化的分解。对于世界模型而言，失败模式（角色身份漂移、遮挡穿透、两视角实体错位、背景闪烁）比平均指标更有信息量；这部分需要回原文的 qualitative/limitations 部分核对。

Q6: 有什么可以进一步探索的点？

一、扩展到 N 玩家与高效跨视角注意力
论文明确把 N 人扩展列为未来方向：原理上可把更多视角拼进同一上下文，但 token 与注意力/显存随人数增长。可探索稀疏/窗口注意力、跨视角 token 压缩、分层生成（先低分辨率共享结构再分视角细化）、缓存/流式生成，以及按可见性动态分配计算。还需要研究玩家动态加入/离开、视角数可变时的训练策略。

二、从游戏引擎状态走向学习到的世界演化
当前 proxy 来自共享地图几何和游戏引擎状态；结论把“learned state transitions and sustained interaction”列为未来工作。一个自然方向是用学习到的 dynamics model 替代或补足引擎状态，让 coding agent 或世界模型自己维护实体状态、物理和交互历史，同时保持 proxy 的可解释/可编辑性。难点是长时演化误差累积，以及生成模型与状态模型之间的接口设计。

三、proxy 的保真度与不确定性
Raw 粗几何的失败说明 proxy 构建是关键变量。可探索更丰富的 proxy：深度、法线、语义分割、实体 ID、关节/姿态、遮挡 mask、不确定性图；也可让模型在 proxy 不可靠时学会降权或回退到生成先验。需要回答“多粗的 proxy 足够”“哪些结构线索不可丢失”“如何表示动态遮挡”等问题。

四、外观控制与身份保持的解耦
RGB 参考支持未见角色/风格，但 reference-preservation 仍有误差。未来可研究更强的身份/风格解耦、参考图质量鲁棒性、多参考融合、跨视角光照/材质一致性，以及参考与 proxy 冲突时的仲裁机制。还可探索文本/图像/视频混合控制，让用户用自然语言编辑场景而不仅是换参考图。

五、评估体系与 benchmark 扩展
需要更大规模、多游戏、多场景、多风格的 benchmark；按类别（探索/交互/骑乘/战斗）报告指标；加入人工评估、交互正确性、动作可控性、物理合理性、跨视角几何误差、时序一致性等任务级指标；报告方差/显著性/失败率。还应测试布局交换、视角数变化、遮挡极端情况、快速运动、参考风格分布外等压力条件。

六、效率与部署
结论提到 device–edge–cloud 部署和 AI Flow 框架。未来可研究蒸馏、少步生成、量化、流式推理、边缘端缓存，以及多人同步时的延迟/带宽权衡。多人世界模型若要在交互场景落地，推理延迟和跨客户端同步可能比离线画质更关键。

七、跨领域应用
共享 proxy 接口可支持协作虚拟环境、多人游戏、远程呈现、协同设计等。更远的推测：多机器人/多智能体共享场景表示、仿真到现实的数据生成、教育/训练模拟。但这些应用需要解决资产版权、隐私、安全、实时性和真实感评估等问题；论文片段没有展开这些边界。

八、与 agent 的结合
Introduction 提到 coding agent 构建和更新世界。未来可研究 agent 如何根据交互自动生成/修改 proxy、选择视角、维护实体状态，以及如何利用生成视频作为反馈。这与用户画像中的 agent 方向弱相关：论文主体不是 agent 算法，但“agent 作为世界构建接口”是一个可探索的交叉点。

Q7: 总结一下论文的主要内容

一、论文要解决的问题
CoCode-World 关注多人生成式世界模型中的跨视角一致性：两名玩家在同一场景中各有相机，看到的遮挡和朝向不同，但生成画面必须对共享角色、物体和场景结构给出一致解释。现有两条路线各有缺口：一条是在学习到的 latent 空间做 joint denoising、cross-agent attention 或 shared representation，优点是端到端，缺点是空间控制不直接；另一条是显式状态/世界表示，把世界演化与视角合成分开，但部分方法依赖精细、场景特定的地图几何，限制场景多样性。论文要补的空白是：能否用粗粒度、可编程的 3D proxy 作为结构与视频渲染器之间的共享接口，让视频模型补足外观细节，同时保持跨视角一致。

二、方法主线
方法把问题拆成“结构”和“外观”两个条件。结构由共享 3D proxy 提供：用地图几何 M、每个玩家可见的实体状态 E_t(p) 和相机参数 C_t(p) 投影出相机特定 proxy，P_t(p)=R(M, E_t(p), C_t(p))，指定场景布局、运动与遮挡。外观由 RGB 参考 I 提供，指定角色和场景的长相。生成时，把两名玩家的 proxy 视频、外观参考和目标视频按固定左-右布局拼成一个画布，交给预训练视频 Transformer（片段中称 H3 transformer / Video DiT）联合去噪两路目标视角；跨视角通信依赖模型原生 attention 在单一生成分支内完成，而不是额外设计 cross-agent 模块；主干冻结，用 LoRA 适配 proxy 遵循和成对外观对应。解码后去掉 padding，裁成两段玩家视频。这个设计的核心是把“多人一致性”从隐空间协调转为共享空间布局 + 共享上下文 + 配对监督。

三、数据与训练
作者通过 private FiveM server 采集双人 GTA V gameplay，对每个客户端记录 RGB、相机参数、玩家与附近实体的身份和位姿，并导出同一坐标系下的静态地图几何；地图与持久实体 ID 用于跨相机关联同一场景。动态观测保留各客户端在本地流送和网络条件下可见的状态。检索片段显示数据集含 7,846 个约 5 秒片段，类别占比为 Exploration 66.4%、Interaction 9.8%、Riding 18.7%、Fight 5.1%；这组统计对应训练集还是完整数据划分，片段未明示。训练时成对画布把两路观测同时暴露给每一次联合预测，使模型从配对 RGB 监督中学习跨视角外观对应。

四、实验与发现
评测 benchmark 由 150 个双人案例组成，覆盖探索、载具骑行、玩家战斗、环境交互四类事件；proxy 轨迹来自 GTA V gameplay，参考图用 GPT Image 2.5 风格化，改变玩家外观、背景和视觉风格，以测试 appearance transfer 下的结构保持。指标包括 DOVER、LAION aesthetic predictor、DINO Adj.、Edge F1、First-frame 参考保持。结论段报告：在录制的外观迁移案例上，DOVER、LAION 美学、Edge F1、跨视角玩家身份一致性高于 Seedance 2.5；同时仍有 adjacent-frame 和 reference-preservation 误差。消融考察 proxy 构建和跨视角通信；结果片段提到 Raw 的粗几何无法准确表达复杂环境中的深度、遮挡和局部结构，导致歧义并伴随图像发黑，说明 proxy 保真度是关键变量。完整 baseline、数值表和失败率未在提供片段中出现。

五、贡献与边界
论文的主要贡献可以概括为：提出共享 3D proxy 的双人世界模型框架，把结构控制与视觉合成解耦；设计固定空间布局的联合生成，让预训练视频模型用原生 attention 完成跨视角通信，并用 LoRA 适配；构建带同步 RGB、相机参数、实体状态和地图几何的双人 GTA V 数据集；建立 150 案例、四类事件的评测协议；报告外观迁移、视频质量和跨视角身份一致性的改进；通过消融指出 proxy 构建和跨视角通信的重要性。边界也很明确：当前只支持两人；固定每视角分辨率和片段长度时，增加玩家会线性增加 token 序列，带来注意力计算和显存压力；作者初步尝试发现激进蒸馏下保持质量仍有挑战；数据只来自 GTA V，跨游戏/真实场景泛化未证实；指标偏感知/结构，缺少 gameplay 语义正确性和交互可控性评估。

六、阅读判断
这篇论文值得读的地方不是某个单点模块，而是“共享 3D proxy 作为多人世界模型接口”的问题重构：它把跨视角一致性从隐空间协调问题转为结构条件、外观参考和联合上下文的组合问题，并用配对数据把三者对齐。需要重点核对的是：proxy 到底有多粗、哪些结构线索不可丢；固定布局是否让模型学到位置捷径；跨视角身份一致性的提升是否以时序稳定性/参考保持为代价；以及 N 人扩展是否只是原理上可行。若关注生成式世界模型、多人/多视角视频生成、游戏数据驱动的视频渲染，这篇论文提供了较完整的问题设定、数据构造和评测框架；若关注 agent 算法本身，则它更多是“coding agent 构建世界”的应用接口，而不是 agent 方法论文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关：论文研究多人/多视角视频生成、生成式世界模型和预训练视频 Transformer 的参考条件生成，属于你画像中 generation 方向的核心议题。

## 基本信息

- 作者：Qirui Li, Xin Zhang, Yabo Chen, Jinkun Hao, Paul L Rosin, Yu-Kun Lai, Haibin Huang, Yin Chen, Ran Yi, Chi Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-11-11
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.15602v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Method、Results 与 Conclusion 片段，并优先用这些证据校正 heuristic_draft；片段未给出的数值、完整 baseline 和训练配置没有被补造。
