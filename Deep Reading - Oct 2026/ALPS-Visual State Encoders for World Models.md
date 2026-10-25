---
user_id: "cheng tan"
paper_id: 17563
arxiv_id: "2610.30120v1"
title: "ALPS: Visual State Encoders for World Models"
publish_date: "2026-10-21"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.30120v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.30120v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-25T14:28:20"
---
# ALPS: Visual State Encoders for World Models

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world model · visual state encoder · self-supervised learning · vision transformer

## 一句话总结

ALPS 提出一种从零训练的 ViT-B/16 视觉状态编码器，用语言、像素重建和跨视角空间对应目标，把物理状态、语言对齐与像素重建统一到同一组 patch token 中，并在 15 个公开编码器的统一评测中取得 ViT-B 级别最低的 LIBERO 状态探针误差与 85.5% 的平均策略成功率。

## 摘要

> World models turn visual representations into predictions and actions. We argue that their visual foundation should combine physical-state information, native language alignment, and pixel reconstruction, so that a single representation can support prediction, instruction, and inspection. However, existing encoders typically specialize in only one or two of these capabilities; we therefore introduce ALPS, a pretrained visual state encoder that unifies all three in a single set of patch tokens. Using only still images and text, we pretrain a ViT-B/16 from scratch with language, reconstruction, and spatial correspondence objectives, and then refine it via cross-view self-distillation. In a unified evaluation across fifteen public encoders, ALPS achieves the lowest aggregate LIBERO state-probe error among ViT-B models (matching the 3.5x larger V-JEPA 2-L) while maintaining strong language alignment and accurate pixel reconstruction. Downstream, frozen ALPS tokens reach the highest mean policy success on LIBERO (85.5%) when paired with our native text tower, match or exceed leading encoders on world-model planning across PushT, Wall, and Granular, and allow predicted future tokens to be decoded directly into pixels for visual inspection. We hope this work helps establish visual pretraining as a shared foundation for understanding, prediction, and action in world models.

Q1: 这篇论文试图解决什么问题？

1. 核心问题：世界模型要把视觉观测转化为预测与动作，因此其视觉基础不应只服务于单一任务。论文明确提出理想表征应同时包含三类信息：（a）物理状态信息，如物体位置、空间关系、可供性；（b）原生语言对齐，使指令可以直接作用于视觉 token；（c）像素重建能力，使预测结果可被解码和人工检查。问题在于，现有视觉编码器通常只专精其中一两类，导致世界模型不得不在多个编码器、多个接口之间拼接，增加系统复杂度，也使评测难以横向比较。

2. 现有编码器的能力分工：从论文对比的 15 个公开编码器看，DINOv2/DINOv3、VC-1、PE-Spatial 等偏向空间/状态特征；SigLIP 2、CLIP 类模型偏向语言对齐；MAE 类模型偏向像素重建；V-JEPA 2 偏向视频预测表征；C-RADIOv3 则通过多教师蒸馏聚合多种能力。论文的出发点不是否认这些模型各自的有效性，而是指出「没有单一 patch token 集合被统一检验为同时满足三种接口」。

3. 评测碎片化是第二个问题：不同论文使用不同的线性探针、不同的控制基准、不同的重建协议，导致「哪个编码器更适合世界模型」无法直接回答。ALPS 的一个方法论贡献是构建统一评测：在同一套协议下比较 15 个编码器的状态信息、语言对齐和像素重建，并在 LIBERO 控制与 PushT/Wall/Granular 规划上检验下游效果。

4. 隐含假设与潜在风险：（a）用 LIBERO 帧上的线性状态探针（聚合相对位置误差）代表「物理状态信息」，合理推断其测量的是位置等可直接线性读出的量，未必覆盖速度、朝向、接触、遮挡和可供性；（b）用 LIBERO 行为克隆成功率代表「控制可用性」，受演示数量、策略架构和文本接口影响；（c）用 ImageNet 验证集上的轻量重建适配器代表「像素重建」，与机器人域存在分布差距，且轻量探针可能低估或高估编码器的生成潜力；（d）假设冻结 token 足以支撑下游，若下游需要微调，比较结论可能变化；（e）「一个表征服务三种能力」在容量上是否可持续，需要看消融和 scaling，而当前证据主要在 ViT-B 规模。

5. 边界条件：ALPS 仅用静态图像和文本预训练，而 V-JEPA 2 使用 16 帧视频；因此「匹配 3.5 倍参数量的 V-JEPA 2-L」是在聚合状态误差上的比较，不等于在视频动态、长时预测上全面等价。论文的控制实验限于 LIBERO 仿真，规划实验限于 PushT、Wall、Granular 等相对短程任务，检索片段未显示真实机器人或高自由度操作结果。

6. 判断：论文真正试图解决的是「世界模型视觉基础的能力统一与可比评测」这一双重问题。其贡献不只是某个编码器，而是提出一套三接口的预训练配方与评测框架。证据支持「三种能力可以在一个 ViT-B/16 中并存」这一较弱命题；是否支持「一个编码器可以替代所有专用编码器」这一较强命题，仍需看更多任务、更大规模和真实部署。

Q2: 有哪些相关研究？

1. 自监督视觉编码器：论文对比的模型包括 MAE、DINOv2、DINOv3、VC-1、OpenVision 3、C-RADIOv3、PE-Spatial、TIPSv2、SigLIP 2、V-JEPA 2-L 等。它们分别代表掩码重建、自蒸馏、视频预测、多教师蒸馏、语言监督等路线。ALPS 与这些工作的关系不是提出全新的自监督目标，而是把语言、重建、空间对应三类目标放进同一个 ViT-B/16 预训练流程，并用统一协议检验。

2. 语言对齐视觉编码器：CLIP/SigLIP 2 类模型通过图像-文本对比学习获得零样本语义接口，但其 patch token 是否保留足够的细粒度空间状态，常受质疑。ALPS 的「原生文本塔」显然借鉴了这一路线；论文声称其语言对齐在 Stage 2 后仍保持，并用线性 CKA 等指标与 SigLIP 2 比较。检索片段还提到 C-RADIOv3 使用 DFN CLIP ViT-H/14 教师来构造语言接口，这说明多教师蒸馏是当前获得多能力表征的一条竞争路线。

3. 世界模型与视频表征：V-JEPA 2 是论文最直接的对照之一，它用视频预训练学习可预测的表征，参数量约为 ALPS 的 3.5 倍。ALPS 的差异在于：不使用视频，而用静态图像 + 跨视角自蒸馏来注入空间对应；不把预测目标放在视频帧上，而把「未来 token 可解码为像素」作为可检查性接口。二者在聚合状态误差上相当，但片段显示「互补优势」，说明视频动态信息仍是 ALPS 的潜在短板。

4. 机器人控制与规划基准：LIBERO 是语言条件桌面操作的行为克隆基准；PushT、Wall、Granular 是常用于世界模型规划的二维/物理任务。ALPS 把冻结视觉 token 接入 LIBERO transformer policy，并在规划任务中比较不同编码器。相关工作还包括以 Dreamer 系列、V-JEPA 规划、扩散策略等为代表的世界模型/策略学习路线，但检索片段未展开这些引用，具体谱系需回原文。

5. 表征评测方法：线性探针、CKA、ℓ1/LPIPS、行为克隆成功率是常见工具。ALPS 的新意在于把三者组织成「状态—语言—像素」的统一矩阵，并额外报告分辨率、预测时程、解码器强度等压力测试。与此相关的是「表征是否线性可分」与「表征是否保留生成细节」之间的张力：对比学习倾向丢弃低层细节，重建目标倾向保留细节但可能浪费容量，空间对应目标则强调几何一致性。

6. 研究缺口：现有工作很少同时回答「同一编码器能否在三种接口上都不明显逊色」，也缺少跨编码器的统一复现。ALPS 填补的是这个评测与配方空白；其相关研究定位更接近「系统性整合与严格比较」，而非单一新算法。

Q3: 论文如何解决这个问题？

1. 总体路线：ALPS 是一个两阶段预训练的 ViT-B/16 视觉状态编码器。输入为静态图像和文本，输出为单组 patch token；这些 token 同时被语言头、重建解码器和空间对应头约束，随后通过跨视角自蒸馏精炼。下游使用时冻结编码器，只训练线性探针、策略头或世界模型头。

2. Stage 1 的三个目标：
（a）语言目标：论文提到 captioning loss 与对比对齐两类信号，并训练一个原生文本塔。合理推断，captioning 促使 patch/global token 保留可描述语义，对比对齐则把图像嵌入与文本嵌入拉到同一空间；两者结合使冻结 token 可以直接接收指令。
（b）重建目标：从 patch token 解码像素，迫使 token 保留低层外观与空间细节。论文在评测时使用轻量适配器，以避免把「解码器容量」误当成「编码器信息」。
（c）空间对应目标：通过跨视角 patch matching 与坐标回归，让同一物体在不同视角下的 patch 建立对应；检索片段提到空间坐标被缩放到 14×14 的预训练范围，说明其使用 patch 网格上的相对坐标。该目标直接针对「物理状态/几何」能力。

3. Stage 2：跨视角自蒸馏。论文在 Stage 1 之后用跨视角自蒸馏进一步精炼表征。合理推断其形式为：同一场景的不同视角分别送入教师/学生（或同一网络的不同视图），让学生 patch 预测教师在对应位置的表示，从而强化视角不变性与空间对应。检索片段显示该阶段能提升状态信息，并且「只有 ALPS 保持语言对齐」，说明 Stage 2 的主要收益在状态侧，但设计上需要避免语言能力漂移。

4. 架构与接口：ViT-B/16 的 patch token 是统一接口；语言塔是原生训练的，而不是外挂冻结 BERT；重建解码器只在训练/评测需要时使用，下游世界模型可以直接在 token 空间预测，再把预测 token 送入解码器得到像素。这样同一个编码器可服务于「状态探针」「文本指令」「像素检查」三种用途。

5. 训练数据：检索片段提到约 5M 采样图像；摘要只说是静态图像和文本。数据的具体来源、过滤、caption 生成方式在片段中不完整。消融实验使用从零训练 10% Stage 1 的变体，说明作者在受控预算下比较各目标。

6. 设计权衡：（a）从零训练 ViT-B/16 而非蒸馏大模型，使结论更干净，但成本较高；（b）不使用视频，避免视频数据与时空Transformer的复杂度，却可能牺牲动态预测；（c）语言、重建、空间三目标可能相互竞争，论文用 Stage 2 与消融来寻找平衡；（d）冻结下游简化比较，但可能低估微调后的专用编码器。

7. 证据缺口：损失权重、视图生成方式、教师是否动量更新、文本塔规模、训练步数、数据配比等关键细节在检索片段中缺失，需回原文 Method 与附录确认。

Q4: 论文做了哪些实验？

1. 统一编码器比较：在 15 个公开编码器上同时评测状态、语言、像素三类能力。状态侧使用 LIBERO 帧的线性探针，报告聚合相对位置误差，并分解为 readout、+1 s、+3 s 三种设置；另外在 256×256 渲染帧上复测。语言侧报告标准化 token 与 global embedding 的对齐指标（片段出现 .81 线性 CKA 与 .97 的 global language alignment，具体口径需核对）。像素侧在 19,000 张 ImageNet 验证图像上训练轻量适配器，在 1,000 张留出图像（每类 1 张）上测 ℓ1 与 LPIPS，并额外用更强解码器复核。

2. 控制实验：在 LIBERO 四个套件上，每个任务 50 条演示，训练 LIBERO transformer policy 做行为克隆；视觉 token 冻结。指令嵌入分别使用冻结 BERT 或各编码器原生文本塔；每个策略评估 3 次 × 50 条 rollout。该设计同时检验视觉状态可用性与语言接口兼容性。

3. 世界模型规划：在 PushT、Wall、Granular 上比较不同编码器的规划表现；报告不同预测时程 k 的误差，并设置「重复最后一帧」的静态基线，以确认模型学到非平凡运动。检索片段提到 k=5 的误差差距，但句子被截断，无法引用具体数值。

4. 消融：Table 5 的每个变体从零训练 10% 的 Stage 1，以控制预算。消融涉及语言 captioning loss、重建目标、空间对应目标，以及 Stage 2 跨视角精炼。片段明确说「三种改动都提升状态信息，但只有 ALPS 保持语言对齐」，说明消融关注能力间权衡。

5. 分辨率与解码器压力测试：在 256×256 帧上复测状态误差（Tab. S6）；用更大生成解码器检验重建优势是否仍成立（Sec. S2）；这些实验针对「轻量探针是否低估编码器」的质疑。

6. 规划协议细节：检索片段显示 action 通道填入的是 ground-truth recorded actions（Tab. S17 有分析）。这意味着规划评测更接近「给定真实动作序列，检验视觉动态预测与解码」，而非完全从策略生成动作的闭环规划；读者需区分这两类主张。

7. 公平性相关：C-RADIOv3 使用其文本接口而不外接 BERT；其他编码器可能使用各自原生文本塔或冻结 BERT。不同文本接口会混淆控制结果，论文通过「native text tower」与「frozen BERT」两组设置部分控制该因素，但 85.5% 的主结果依赖原生文本塔，需看消融分离视觉与语言贡献。

8. 未见的实验：检索片段未显示真实机器人、跨 embodiment、长期规划、视频动态预测、scaling 到 ViT-L/H 的结果；这些是明显的证据缺口。

Q5: 发现了什么实验现象？

1. 状态信息：ALPS 的聚合相对位置误差为 .829，在对比的 ViT-B 编码器中最低；C-RADIOv3 为 .864，DINOv3 为 .870，三者构成领先组，PE-Spatial 为 .936。ALPS 在 readout、+1 s、+3 s 三个单独设置以及 256×256 分辨率上都取得最低误差，说明优势不是单一设置或单一分辨率的偶然结果。

2. 与 V-JEPA 2-L 的关系：V-JEPA 2-L 用 3.5 倍参数量和视频预训练，达到与 ALPS 相同的聚合误差；片段同时指出两者「互补优势」。合理推断：V-JEPA 在某些状态设置或动态相关维度上更强，ALPS 在参数效率与静态空间对应上更强；不能简单概括为 ALPS 全面优于 V-JEPA。

3. 语言对齐：论文强调 Stage 2 后语言对齐仍保持，检索片段出现 .81 的线性 CKA 与 .97 的 global language alignment。由于片段断句，两个数字对应的具体指标（token 级、全局嵌入、与哪个教师比较）需回原文核对。可确认的观察是：多目标训练没有把语言能力牺牲掉，这与「重建/空间目标会挤掉语义」的常见担忧相反。

4. 重建：检索片段显示 ALPS 的 LPIPS 为 .020，仅次于 C-RADIOv3 的 .018，优于 DINOv3 的 .024 与第一阶段的 .025。也就是说，ALPS 不是像素重建的绝对第一，但處於前列，且 Stage 2 可能进一步改善重建。轻量适配器协议下仍有优势，且论文声称在更强解码器下优势保持（Sec. S2）。

5. 控制：冻结 ALPS token 配合原生文本塔在 LIBERO 上达到 85.5% 平均成功率，为对比编码器中最高。逐任务表出现 23、30、47、85 等差异很大的成功率，说明任务级波动显著；由于片段列名缺失，不能把具体数字归给具体模型，但可以确认「平均值掩盖了任务异质性」，ALPS 并非每个任务都最好。

6. 消融趋势：三种能力相关改动都能提升状态信息，但只有完整 ALPS 保持语言对齐。这揭示了核心张力：状态/空间目标的收益可能以语言语义漂移为代价；Stage 2 跨视角精炼可以增强状态，但需要防止语言退化。片段还讨论去掉 captioning loss、仅用对比对齐训练文本塔的变体，并比较其表征漂移与 SigLIP 2；具体方向因断句不完整，需回原文确认。

7. 规划：静态基线（重复最后一帧）明显更差，说明世界模型确实捕捉到非平凡运动；随着预测时程 k 增大，误差上升，k=5 时不同编码器的差距被截断。更大解码器能提升像素质量，但规划误差主要取决于 token 动态而非解码器。

8. 反直觉/负结果：（a）ALPS 的 LPIPS 不是第一，说明「统一三能力」不等于「每项都 SOTA」；（b）聚合状态误差与 V-JEPA 2-L 打平，但后者参数大 3.5 倍，提示参数效率优势，也提示 scaling 比较尚未完成；（c）部分 LIBERO 任务成功率很低，说明语言条件组合与长程操作仍是失败模式；（d）规划使用 ground-truth action 通道，可能高估了「从视觉预测动作」的能力。

Q6: 有什么可以进一步探索的点？

1. 规模律与视频预训练：当前只验证 ViT-B/16。需要把 ALPS 扩展到 ViT-L/H，并在匹配数据与算力下与 V-JEPA 2、DINOv3、C-RADIOv3 比较；同时加入视频或时序目标，检验动态预测是否能在不牺牲语言/重建的情况下提升。

2. 真实机器人与跨 embodiment：LIBERO 是仿真桌面操作，未来应在真实机器人、不同机械臂、不同相机视角上验证冻结 token 的迁移性；特别需要看遮挡、光照、材质、透明物体等域偏移。

3. 动作条件化与闭环规划：当前规划评测填入 ground-truth actions，未来应让世界模型从视觉/语言生成动作，做闭环 rollout；研究动作条件 token、latent action、policy-world model 联合训练，避免「给定真实动作」的高估。

4. 更完整的物理状态：状态探针目前以相对位置为主，未来应覆盖速度、加速度、朝向、关节角、接触力、可供性、物体 permanence；也可引入 3D 几何、多视角一致性、深度/法线辅助目标。

5. 评测学：建立更严格的统一协议，包括 in-domain 重建（机器人场景而非仅 ImageNet）、组合语言指令、长程任务、OOD 场景、规划成功率的置信区间；报告每任务分布而非只看均值；分离视觉 token 与文本塔的贡献。

6. 表征解耦与可解释性：分析哪些 patch/通道编码状态、哪些编码语言、哪些编码外观；研究 token 预算、register token、多尺度特征、稀疏化对三接口的影响；探索「重建目标是否必要」以及「更强解码器能否替代编码器中的细节」。

7. 数据与训练策略：5M 图像的数据配比、caption 质量、合成数据比例、跨视角对生成方式都值得系统研究；可尝试从大教师蒸馏、课程学习、目标权重动态调整，以缓解三目标竞争。

8. 安全与鲁棒性：世界模型用于决策时，需评估对抗扰动、分布外检测、失败预警；像素解码接口也可能泄露训练数据或产生误导性可视化，需要研究可检查性与安全性之间的权衡。

Q7: 总结一下论文的主要内容

【论证主线】论文从一个系统性主张出发：世界模型的视觉基础不应是「只懂语义」或「只懂重建」的单一编码器，而应同时保留物理状态、语言对齐和像素重建三种接口。作者指出，现有编码器在这三项能力上各有偏科，且评测协议分散，导致无法判断「一个表征能否通吃」。因此，ALPS 的目标不是提出又一个自监督损失，而是给出一套可复现的三能力预训练配方，并用统一评测回答「统一是否可行」。

【技术主线】ALPS 用 ViT-B/16 从零训练，输入静态图像与文本。Stage 1 同时优化语言目标（captioning 与对比对齐，配原生文本塔）、像素重建目标和空间对应目标（跨视角 patch matching 与坐标回归）。这三个目标分别对应「可指令」「可检查」「可测状态」。Stage 2 用跨视角自蒸馏精炼表征，提升视角不变性与空间对应，同时尽量保持语言对齐。下游冻结 patch token，接线性状态探针、LIBERO 行为克隆策略或世界模型规划头；预测的未来 token 可直接送入解码器变成像素。

【实验主线】论文在 15 个公开编码器上做统一比较：状态侧用 LIBERO 线性探针的聚合相对位置误差，并分解 readout、+1 s、+3 s 与 256×256 分辨率；语言侧用 token/global 对齐与 CKA；像素侧用 ImageNet 19k 训练、1k 留出的轻量解码适配器测 ℓ1/LPIPS，并用更强解码器复核。控制侧在 LIBERO 四个套件、每任务 50 条演示、3×50 rollout 下比较冻结 token 的行为克隆成功率。规划侧在 PushT、Wall、Granular 上比较不同编码器的世界模型 rollout，并设静态基线。消融在 10% Stage 1 预算下拆解三个目标与 Stage 2。

【关键发现】ALPS 的聚合状态误差为 .829，优于 C-RADIOv3（.864）、DINOv3（.870）和 PE-Spatial（.936），并在各子设置与分辨率上保持一致；它以 ViT-B 规模匹配 3.5 倍参数、视频预训练的 V-JEPA 2-L，但两者各有所长。语言对齐在 Stage 2 后仍保持（片段出现 .81 线性 CKA 与 .97 global alignment，口径需核对）。重建 LPIPS 为 .020，仅次于 C-RADIOv3 的 .018，优于 DINOv3 的 .024 与第一阶段 .025，说明统一三能力不以像素质量为代价。控制上，冻结 token + 原生文本塔达到 85.5% 平均成功率，为对比编码器最高；但逐任务波动大，ALPS 并非每个任务都最好。规划上，模型显著优于静态基线，误差随时程上升；但 action 通道使用 ground-truth actions，需谨慎解读「规划」主张。

【结论与边界】论文支持「三种能力可以在单一 ViT-B/16 中共存」这一命题，并提供了目前较少见的跨 15 编码器统一证据。它尚不支持「ALPS 全面替代专用编码器」：LPIPS 不是第一，部分任务失败，真实机器人、视频动态、更大规模与闭环动作生成均未验证。论文的真正价值在于把「世界模型视觉基础」从口号变成可测的三轴协议，并给出一个可复现的起点。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：对 agent/具身智能方向：ALPS 直接讨论世界模型的视觉前端，冻结 token 接 LIBERO 策略与 PushT/Wall/Granular 规划，和「用视觉表征支撑决策」的核心问题高度相关；可作为选择视觉 backbone 与评测协议的参考。

## 基本信息

- 作者：Yanqing Liu, Letian Zhang, Chengzhi Liu, Zijun Wang, Jieru Mei, Hongliang Fei, Yuyin Zhou, Cihang Xie
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-21
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.30120v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Results 片段（共 81 个 chunk），但 Introduction/Method 原文为空，方法细节与部分数值已标注需回原文核对。
