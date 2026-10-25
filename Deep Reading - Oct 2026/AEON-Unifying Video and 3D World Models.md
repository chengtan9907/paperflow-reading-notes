---
user_id: "cheng tan"
paper_id: 16960
arxiv_id: "2610.28631"
title: "AEON: Unifying Video and 3D World Models"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.28631.pdf"
pdf_url: "https://arxiv.org/pdf/2610.28631"
abs_url: "https://arxiv.org/abs/2610.28631"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-21T11:41:45"
---
# AEON: Unifying Video and 3D World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · d gaussian splatting · video diffusion model · domain adapter

## 一句话总结

AEON 提出一个统一的生成式世界模型框架：在预训练视频扩散模型的隐空间中直接生成可渲染的 4D Gaussian Splatting（4DGS）作为显式世界表示，从而融合视频世界模型的真实外观与 3D 世界模型的几何一致性，支持持久、可实时探索的动态 4D 环境模拟。

## 摘要

> Current world models for explicit world simulation typically follow two distinctarXiv:2610.28631v1 pathways: (1) video generative models (e.g., Sora), which simulate realistic ap-
> pearances but fail to preserve 3D consistency; and (2) 3D generative models (e.g.,
> Marble), which generate geometrically consistent scenes but often yield static,
> less realistic observations. We present AEON, a unified framework that bridges
> video-based and 3D-based world models and combines the benefits of both. The
> key insight of AEON lies in a generative model that operates within the text- or
> image-conditioned video latent space while producing renderable 4D graphics
> primitives—specifically, 4D Gaussian Splatting (4DGS)—as the explicit world rep-
> resentation. We train a 4D reconstruction model to predict both 4DGS and camera
> poses from monocular videos, serving as our initialized 4D decoder. Furthermore,
> we design a domain adapter that translates generative video latents into reconstruc-
> tive 4D latents through a distillation process on the video VAE. AEON naturally
> supports video-based world modeling by rendering 4DGS at predicted camera
> †The corresponding authors are Yu-Shen Liu and Junsheng Zhou.
> 40th Conference on Neural Information Processing Systems (NeurIPS 2026).
> trajectories, and constructs a persistent 4D environment with scene dynamics for
> realistic world simulation. Consequently, AEON empowers a diverse range of
> world modeling applications and achieves state-of-the-art performance on standard
> benchmarks. Project Page: https: // weiqi-zhang. github. io/ AEON .

Q1: 这篇论文试图解决什么问题？

1. 领域背景与动机：世界建模已成为 AI 研究的前沿，在具身智能（embodied AI）、自动驾驶、游戏设计等领域有巨大潜力。但显式世界模拟的最优表示尚无共识，领域被视频基与 3D 基两条范式割裂：视频世界模型（如 Sora 一类）和 3D 世界模型（如 Marble 一类）。论文认为两类方法有正交的优势和各自范式特有的根本瓶颈，因此需要更强大的统一表示。
2. 论文提出的“好世界模型”判据：作者从基本问题出发，提出理想世界模型应具备五条性质：(1) 生成与真实域对齐的逼真外观；(2) 建模动态场景演化（如火焰、波浪）和运动物体；(3) 保持无几何歧义的 3D 一致环境；(4) 保证场景持久性，支持连续、可重复的模拟；(5) 支持实时探索，服务高效交互。这五条性质分别对应视频模型的强项（1、2）和 3D 模型的强项（3、4、5），单一范式无法同时满足。
3. 两条范式的具体缺陷：视频生成模型擅长照片级外观和时间演化，但缺少显式几何约束，跨视角渲染会出现 3D 不一致；3D 生成模型能给出几何一致的三维场景，但往往缺乏真实外观与动态场景演化，输出偏静态。这样，现有世界模型在“真实感外观—动态演化—3D 一致性—持久性—实时可探索”的联合目标上各有短板。
4. 统一表示为何困难：需要一种表示，既能寄生在视频扩散模型的生成先验之上，又能解码为显式、可持久保存、可实时渲染的几何基元。难点在于生成式视频隐空间与重建式 4D 隐空间之间存在域差距：前者编码的是帧序列外观与运动，后者编码的是三维几何、外观和时变属性。直接把扩散模型输出转成 4D 表示会引入几何退化，而单独训练 3D/4D 生成模型又难以继承视频模型的大规模真实感先验。
5. AEON 的切入点：以 4D Gaussian Splatting（4DGS）作为基础表示——每个高斯基元同时编码几何、外观、运动速度和寿命——在预训练视频扩散模型的隐空间中运行生成模型，同时输出 4DGS。通过训练 4D 重建模型作为初始 4D 解码器，再利用域适配器桥接两个隐空间，从而同时继承视频模型的真实感生成能力和 3D/4D 模型的几何一致性、持久性与可探索性。
6. 问题边界：论文面向的是“显式世界模拟”而非隐式状态预测，重点是生成和重建可交互的 4D 场景；评估覆盖 4D 生成、动态场景重建和新轨迹渲染。论文没有直接处理物理规律学习或 agent 闭环控制，但把这些问题作为世界模型的应用场景。

Q2: 有哪些相关研究？

1. 视频基世界模型：以 Sora 为代表的视频生成模型（论文引用 [26,38]）能模拟真实外观和动态，但缺少 3D 一致性。相关方法被用作生成先验，也用于 4D 生成的 baseline 或骨干。AEON 在预训练视频扩散模型（Wan 2.1 / Wan 2.2）的隐空间上工作，因此视频生成路线既是对比对象，也是其生成先验来源。
2. 3D 基世界模型：以 Marble 为代表的 3D 生成模型（论文引用 [45,14]）输出几何一致的场景，但通常静态、真实感不足。AEON 与之形成互补，并主张用统一表示融合两条路线。
3. 3D/4D 生成与重建中的表示方法：论文指出视频数据集之外，3D 生成与重建更广泛地建立在静态高斯泼溅（相关文献 [62,59,58,56,47]）或距离场（[63,64,25,24]）之上；这类表示缺少时间维度和动态演化，难以直接支撑世界模型。AEON 采用 4DGS 作为统一表示，并借鉴动态场景重建中的 4D 高斯表达（[41,53,60] 等）。
4. 视频到 4D 重建与动态场景重建：实验对比的 baseline 包括 L4GM [31]、4DGT [48]、MoVies [22]、NeoVerse [50]、StreamSplat [46] 等。这些方法各有侧重：有的做逐帧点云（如 4DNex [7]），有的做逐帧 3DGS（如 Lyra [3]），有的做 4D 重建。AEON 与它们的关键区别是输出统一的持久 4DGS 表示，而不是逐帧重建结果，从而避免跨帧不一致。
5. 图像/文本到视频再到 4D 的生成路线：4D 生成领域已有 4DNex、Lyra 等工作；新视角轨迹渲染方面对比了 TrajectoryCrafter [54] 等方法。AEON 在这些任务上作为统一框架与专门方法竞争。
6. 隐空间桥接与蒸馏：论文的域适配器属于生成式视频隐空间与重建式 4D 隐空间的桥接，使用教师-学生式知识蒸馏（包括属性空间对齐、密集几何对齐和 L1 特征对齐 Lfeat）。这与视觉领域常见的 latent diffusion 蒸馏、多模态特征对齐思路相关，但应用对象是“视频生成模型—4D 重建模型”这一特定跨域组合。
7. 说明：由于提供的正文片段只保留部分引用编号，部分具体方法名与论文标题无法一一核实；上述分类以片段中明确出现的名字与引用编号为准。

Q3: 论文如何解决这个问题？

AEON 的整体思路是：在一个预训练视频扩散模型的隐空间中运行一个生成模型，但输出不是像素帧，而是显式 4D Gaussian Splatting 世界表示。给定文本提示或条件图像，系统生成一组同时编码外观与动态的 4D 高斯，既可用于照片级渲染，也可用于持久 3D 模拟。
1. 4D Gaussian Splatting 作为世界表示（Sec. 3.1）：AEON 采用 4D 高斯基元作为基础场景表示。每个基元 gi 的参数集为 {xi, si, ri, oi, ci, ti, li, vi, ωi}，其中 xi 是位置、si 是尺度、ri 是旋转、oi 是不透明度、ci 是颜色、ti 与 li 与时间相关（推测 ti 表示时间属性、li 表示寿命/持续时间，原文片段未给出完整定义）、vi 是速度、ωi 是时变可见性相关参数。该表示通过速度驱动的运动（velocity-driven motion）和时变可见性（time-varying visibility）建模动态场景，使每个基元同时承担几何、外观与时间演化的编码，从而支撑持久、高效渲染的 4D 世界。
2. 4D 高斯重建模型（4DRM，Sec. 3.2）：AEON 训练一个 4D 重建模型，从单目视频中预测 4DGS 和相机姿态。4DRM 有四个任务头：深度头（depth）、高斯外观头（Gaussian appearance）、动态演化头（dynamic evolution）和相机姿态头（camera poses）。该模型作为生成式 4D 解码器的初始化，为后续生成提供几何引导。消融实验显示，若只用渲染监督而缺少 4DRM 教师，性能明显下降，尤其在 DaVis 上，说明 4DRM 提供的几何先验是关键。
3. 域适配器（Sec. 3.3）：域适配器桥接生成式视频隐空间与重建式 4D 隐空间，通过蒸馏把视频扩散模型产生的隐变量映射到 4DRM 可解析的隐变量。蒸馏过程至少包含三类对齐：属性空间对齐（attribute-space alignment），对每个 4D 基元的位置、旋转、不透明度、颜色、速度、寿命等属性施加基元级对应约束，是最直接的知识迁移；密集几何对齐（dense geometric alignment），在属性空间监督之外补充几何层面的约束；Lfeat 特征对齐，通过 L1 损失把学生的中间特征（在桥接输出处）与对应的教师激活对齐。Lfeat 的权重 λ(t) 采用余弦衰减调度：训练早期施加强正则，防止适配器发生灾难性漂移（catastrophic drift），随后逐步放松，让适配器在后期获得更大学习自由度。
4. 多阶段训练策略（Sec. 3.4）：论文采用多阶段训练，将 4D 重建模型、域适配器与下游生成/解码过程联合优化，最终端到端训练整个流程。论文强调“端到端”优化，但具体的阶段划分细节（各阶段数据、损失配比、训练顺序）在提供的片段中没有展开，需要回到原文确认。
5. 推理与使用方式：AEON 天然支持视频式世界建模——沿预测的相机轨迹渲染 4DGS 即可得到视频；同时由于 4DGS 是持久显式表示，可构建带场景动态的持久 4D 环境，支持连续、重复和实时探索式模拟。
6. 骨干泛化：AEON 可以替换视频扩散骨干（论文验证了 Wan 2.1 和 Wan 2.2），无需修改架构设计，说明域适配器与 4D 解码器对骨干的选择具有一定通用性。

Q4: 论文做了哪些实验？

论文实验覆盖三个任务：4D 生成、视频到 4D 重建、新轨迹渲染，另有消融研究。
1. 4D 生成（Sec. 4.1）：在 30 个由图像-文本提示指定的场景上评估 AEON，对比 4DNex [7] 和 Lyra [3]。与 4DNex 的逐帧点云、Lyra 的逐帧 3DGS 不同，AEON 预测统一的 4DGS 表示，支持实时探索。评估视频沿共享的预设相机轨迹渲染，使用 VBench [13] 指标：I2V Subject、Subject Consistency、Motion Smoothness、Aesthetic Quality。定量结果（Table 2）：4DNex 为 0.796 / 0.790 / 0.968 / 0.342，Lyra 为 0.865 / 0.864 / 0.974 / 0.459，AEON（Wan 2.1）为 0.912 / 0.864 / 0.976 / 0.519，AEON（Wan 2.2）为 0.913 / 0.875 / 0.980 / 0.531。AEON 在所有指标上超过 baseline，使用 Wan 2.2 骨干时优势更明显。
2. 视频到 4D 重建（Sec. 4.2）：在 Dycheck [9] 和 DaVis [30] 数据集上对比 SOTA 重建方法：L4GM [31]、4DGT [48]、MoVies [22]、NeoVerse [50]、StreamSplat [46]。论文指出与 4DGT 等逐帧重建方法不同，AEON 建模持久 4D 高斯，带有显式速度和持续时间，从而自然保证时间一致性。定性图（Figure 3）展示了在 Dycheck 和 DaVis 上的重建比较。定量表格在提供的片段中不完整，无法逐一列出精确数值。
3. 新轨迹渲染（Novel trajectory rendering）：片段中出现与 TrajectoryCrafter [54] 等方法的比较表格残片，包含 Subj. Consist.、Back. Consist.、Temp. Flick.、Motion Smooth.、Aesth. Quality、Imag. Quality 等指标列，但具体数值被截断。可确认的是论文声称 AEON 在新视角/新轨迹渲染上“在不同视角变化下保持强一致性和视觉质量”，具体数值需回原文核对。
4. 消融研究：片段明确提到两类消融。其一，去掉教师 4DRM、只用渲染监督会导致明显性能下降，尤其在 DaVis 上，说明 4DRM 作为几何基础的重要性；其二，去掉 Duration（w/o Duration）会禁用时间不透明度演化机制（temporal opacity evolution），片段在“突出……”处截断，合理推断这会损害动态场景中物体的出现/消失建模。其他消融（如 Lfeat 的有无、属性对齐与几何对齐的贡献）推测存在，但片段未直接给出。
5. 说明：由于语义检索命中的 Results 片段存在表格残片和截断，部分数值与实验设置只能以提供的 Table 2 及定性描述为准；其他表格数字建议从 arXiv 原文补齐。

Q5: 发现了什么实验现象？

1. 4D 生成上的优势：AEON 在 VBench 四个指标上全面领先 4DNex 和 Lyra。提升最大的是 Aesthetic Quality（从 0.342/0.459 提升到 0.519/0.531），其次是 I2V Subject（从 0.796/0.865 提升到 0.912/0.913）；Subject Consistency 也从 0.790/0.864 提升到 0.864/0.875。这支持论文的核心主张：持久 4D 高斯与显式速度/持续时间建模优于逐帧重建。
2. 跨骨干泛化现象：AEON 在 Wan 2.1 与 Wan 2.2 两种视频扩散骨干上都取得强结果，无需架构改动。Wan 2.2 在四个指标上全面优于 Wan 2.1，说明更强的视频先验可以直接转化为 4D 生成质量的提升，域适配器没有成为迁移瓶颈。
3. 逐帧 vs 持久表示的反差：论文指出逐帧重建方法（如 4DNex 的逐帧点云、Lyra 的逐帧 3DGS、4DGT 的逐帧重建）缺乏跨帧一致性；AEON 用统一的持久 4DGS 表示，配合显式速度与持续时间，自然维持跨视角、跨帧的时空一致性。这一设计差异被定量结果支持。
4. 几何教师的重要性：消融显示，去掉 4DRM 教师、仅用渲染监督时性能显著下降，尤其在 DaVis 上。这说明从视频扩散隐空间直接解码 4DGS 时，如果没有重建式几何先验，渲染损失本身不足以约束几何质量。这与“域适配器需要几何锚点”的方法设计一致。
5. Duration 机制的作用：禁用 Duration（时间不透明度演化）会造成性能退化。合理推断：Duration 控制高斯基元随时间的出现/消失，对于火、波浪、运动物体等动态现象至关重要；去掉后模型可能难以表达场景元素的生命周期。具体数值片段缺失，需回原文补全。
6. 未报告或张力点：论文没有在提供的片段中报告推理时间、显存占用等效率指标；VBench 是视频质量指标，用它评估 4D 生成的一致性是一种间接评价，可能不足以捕捉长期 3D 几何漂移。这些是需要读者自行权衡的方面。

Q6: 有什么可以进一步探索的点？

1. 论文明确提出的方向（Conclusion）：当前生成的 4D 场景时间范围受视频扩散模型序列长度限制。由于 AEON 以持久 4DGS 存储世界而非帧，作者提出自然的扩展路径：从当前 4DGS 的最新相机位姿渲染条件帧，生成下一段视频窗口，解码为新 4D 基元并与现有世界融合。这是一种“循环世界扩展”机制，可以突破单次生成的时间上限。
2. 更长时域与更大场景：在循环扩展基础上，可以探索长时间动态场景的累积误差控制、跨窗口一致性和内存管理；更大范围场景（城市、室内多房间）的增量式 4D 世界构建也是自然延伸。
3. 更强的物理与因果建模：当前 AEON 从视频先验中继承外观和运动，但未必具备显式物理规则（重力、碰撞、流体）学习能力。可在 4DGS 之上加入物理仿真约束或物理感知损失，使世界模型真正可用于具身 agent 规划。
4. 交互式与闭环控制：论文强调实时探索，但尚未展示与策略/控制器闭环交互。未来可将 AEON 作为可微分 4D 环境，供强化学习 agent 训练“想象中规划”，或与 LLM 结合做高层任务分解。
5. 域适配器的泛化：已验证 Wan 2.1/2.2 两种骨干，可进一步扩展到更多视频扩散模型、不同分辨率/时长、不同视频 VAE 结构；也可探索无需特定 4DRM 教师的自监督适配。
6. 评估体系：现有评估依赖 VBench 等视频指标，缺少统一的 4D 世界模型基准（几何一致性、持久性、可探索性、物理合理性）。构建这样的基准是推动领域收敛的重要方向。
7. 科学仿真应用：结合用户画像中的 ai-for-science 方向，可以探索将 AEON 用于科学可视化、动态场景重建、医学/材料动态过程模拟；但论文当前场景以自然视频为主，落地到科学领域需要额外验证（推测）。

Q7: 总结一下论文的主要内容

论文围绕“什么是好的世界模型”这一根本问题展开。作者指出，显式世界模拟领域缺乏统一的表示，分裂为视频基和 3D 基两种范式：视频生成模型（如 Sora）能模拟真实外观和动态，但缺乏 3D 一致性；3D 生成模型（如 Marble）几何一致但往往静态、不真实。论文提出理想世界模型的五条标准：真实外观、动态场景演化、3D 一致环境、场景持久性和实时探索。没有任何单一范式能同时满足这些条件，因此需要一种统一表示。
AEON 的核心技术路线是：把 4D Gaussian Splatting（4DGS）作为基础世界表示，并让生成过程发生在预训练视频扩散模型的隐空间中。这样，模型既能使用视频扩散模型的大规模真实感先验，又能输出显式、持久、可实时渲染的 4D 几何基元。
技术主线分四部分。第一，4DGS 表示设计：每个高斯基元 gi = {xi, si, ri, oi, ci, ti, li, vi, ωi} 同时编码几何（位置、尺度、旋转）、外观（不透明度、颜色）和时间演化（时间、寿命、速度、时变可见性），通过速度驱动的运动和时变可见性建模动态场景。第二，4D 重建模型（4DRM）：从单目视频中联合预测深度、高斯外观、动态演化和相机姿态，四个任务头分别对应这些输出，为生成式 4D 解码奠定几何基础。第三，域适配器：通过蒸馏把生成式视频隐变量转换为重建式 4D 隐变量。蒸馏包含属性空间对齐（对每个基元的位置、旋转、不透明度、颜色、速度、寿命施加对应约束）、密集几何对齐和 Lfeat 特征对齐（用 L1 损失对齐学生中间特征与教师激活，权重 λ(t) 采用余弦衰减，早期强正则防止灾难性漂移，后期放松让适配器自由学习）。第四，多阶段端到端训练：把 4D 重建、域适配和生成解码联合优化。
实验主线覆盖三个任务。在 4D 生成任务上，AEON 在 30 个图像-文本场景中对比 4DNex 和 Lyra，使用 VBench 指标评估沿共享相机轨迹渲染的视频，AEON 全部指标领先，且 Wan 2.2 骨干优于 Wan 2.1；在视频到 4D 重建任务上，AEON 在 Dycheck 和 DaVis 上对比 L4GM、4DGT、MoVies、NeoVerse、StreamSplat 等 SOTA 方法，强调持久 4DGS 相对逐帧重建的时间一致性优势；在新轨迹渲染上，AEON 在不同视角变化下保持强一致性和视觉质量。消融实验表明，去掉 4DRM 教师、仅用渲染监督会造成明显性能下降（尤其 DaVis），去掉 Duration 会破坏时间不透明度演化，说明几何先验和生命周期建模都是关键组件。
论文最终断言：AEON 在 4D 生成、动态场景重建和新轨迹渲染上取得 SOTA 性能，并在单一统一框架内支持广泛的世界建模应用。局限是生成时间范围受视频扩散模型序列长度限制，作者提出基于持久 4DGS 的循环窗口扩展作为未来工作。整体上，这是一篇“表示统一 + 隐空间桥接 + 多任务验证”的系统性方法论文。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的“generation（0.10）”直接相关：AEON 把视频生成与 3D/4D 生成统一到同一隐空间-解码器框架，是生成模型与显式表示结合的代表性工作。

## 基本信息

- 作者：Weiqi Zhang, Wenyuan Zhang, Yu-Shen Liu, Junsheng Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.28631`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了语义检索命中的 PDF 片段（主要来自 Introduction、Method、Results、Conclusion），并结合论文提供的全文 sections 做信息归纳；部分表格与引用编号属于片段残片或截断文本，已在相应字段标注并建议回原文核对。
