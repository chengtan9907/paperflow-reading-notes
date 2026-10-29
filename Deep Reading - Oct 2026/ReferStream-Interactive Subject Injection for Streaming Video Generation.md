---
user_id: "cheng tan"
paper_id: 18907
arxiv_id: "2610.37566"
title: "ReferStream: Interactive Subject Injection for Streaming Video Generation"
publish_date: "2026-10-28"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.37566.pdf"
pdf_url: "https://arxiv.org/pdf/2610.37566"
abs_url: "https://arxiv.org/abs/2610.37566"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-28T11:44:58"
---
# ReferStream: Interactive Subject Injection for Streaming Video Generation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：streaming video generation · subject injection · autoregressive video diffusion · kv cache

## 一句话总结

ReferStream 是一个免训练、面向流式自回归视频生成的交互式主体注入框架：它把新参考放到当前场景 latent 的目标位置中以在上下文中构造参考特征，并用瞬态 cache 注入与恢复机制只让参考影响注入 chunk，从而在不改动已输出帧和已建立场景的前提下引入新主体。

## 摘要

> 2026
> Injecting new subjects naturally during streaming video generation requires integrating newly
> supplied visual references into the existing video while preserving established subjects and theOct scene. Encoding references independently of the current scene provides little guidance for this
> integration, while keeping their features in persistent memory can interfere with established
> 27 context. We propose ReferStream, a framework that requires no additional training and combines
> reference construction using the current scene with transient cache injection. Joint Latent places
> reference latents in target regions of the latest scene latent, allowing reference features to be
> extracted in context. Cache Restoration uses these features to condition the injection chunk,
> then restores the overwritten global cache entries while retaining the newly generated history.
> This procedure incorporates new visual conditions without retraining the generator or revising[cs.CV] emitted frames. We further introduce ReferStream Bench, a new benchmark with scheduled
> reference arrivals, evolving instructions, and explicit annotations of subject presence for InShot
> and MultiShot generation. Evaluation follows the event schedule and measures both the fidelity of
> new subjects and the preservation of established subjects, alongside video quality, text adherence,
> and count accuracy. Experiments on ReferStream Bench show higher scores for text adherence,
> subject similarity, and count accuracy than the compared baselines, including offline methods
> given all references in advance. Project page: https://xushuolin.github.io/ReferStream/.

Q1: 这篇论文试图解决什么问题？

【任务定义】论文研究的是流式、因果视频生成中的交互式主体注入：用户在视频已经开始生成之后提交新的视觉参考，系统必须在下一个 chunk 边界把该参考对应的主体融入当前场景，同时保证已经输出的帧不变、已经建立的主体与场景不被破坏。参考可能在任意时刻到达，也可以有多个参考分批到达；每次到达还伴随目标框、事件指令和 cut flag。这个设定与离线参考控制不同：离线方法可以在生成前看到全部参考并统一规划，而在线方法面对的是信息到达时序约束，参考的到达时间本身决定了哪些信息可用、主体应在何时出现。
【与离线参考控制的区别】离线 subject-driven 视频方法通常把所有参考在时间零点给出，模型可以在整段视频范围内安排主体外观与运动；ReferStream 不允许回改历史，也不能把未来参考提前编码进已生成内容。因此核心不是简单地把参考特征拼进模型，而是让新信息只影响后续生成，并且不污染已有上下文。
【两个技术难点】第一，参考图像通常以独立尺度、姿态和背景描绘主体，直接编码只告诉模型主体长什么样，却不告诉模型它应以什么尺度、在什么位置、如何与当前场景的光照和遮挡关系结合；独立编码缺乏场景引导，容易出现外观复制、尺度错误或场景冲突。第二，流式生成依赖全局 sink、shot sink 和 rolling window 等持久 cache 维持长程一致性；如果把参考 K/V 长期写进全局记忆，参考特征会持续干扰既有主体和场景，造成漂移、重复或遗忘。但若参考影响完全不留痕，新主体又难以在后续 chunk 中延续。
【评测缺口】已有视频生成 benchmark 大多假设参考在生成前已经给定，缺少按事件时间到达的参考、演化指令和主体出现标注，也没有同时度量新主体保真度与既有主体保持的协议。论文因此引入 ReferStream Bench，用事件时间表组织 InShot 与 MultiShot 场景，并用主体存在性标注判断主体是否应出现、是否被重复生成、数量是否符合指令。
【隐含假设与边界】方法假设目标框可由用户显式给出或由 layout 默认解析；每个边界只应用一个更新，多个参考可被分组到同一更新中；冻结的 LongLive2-5B backbone 能提供足够的历史记忆；自动主体指标与视频质量评估器能够可靠反映注入效果。这些假设在复杂遮挡、自动布局失败、长时重复出现、多主体冲突和跨镜头身份保持下都可能失效。

Q2: 有哪些相关研究？

【主线一：自回归与流式视频扩散】论文建立在 chunk-by-chunk 的自回归视频扩散之上：模型逐块去噪，同时保留已生成前缀为干净 latent，因此不同时间位置可以有不同的噪声水平。生成时依赖 cached history 维持长程一致性。LongLive 与 LongLive2 支持在生成过程中改变文本指令，Anchor Forcing 则改进了 prompt 变化下的记忆连续性。这一主线提供了交互式注入的基础设施，但主要处理文本条件变化，不处理视觉参考在生成中途到达。
【主线二：视觉参考与主体控制】Phantom、VACE 和 HOMIE 通过视觉参考控制主体外观，AlcheMinT 进一步为参考主体指定时间区间。这些方法说明参考条件可以控制 subject appearance，但通常假设参考在生成前已经可用，或者至少可以在离线阶段统一规划；它们没有把参考到达时刻作为一等变量，也没有处理参考到达后如何在不修改已输出帧的情况下适配既有场景。
【主线三：长视频记忆与 cache 管理】流式生成器通常组合 global sink、shot sink 和 rolling window，并对重叠 token 去重，以有限 cache 维持长视频一致性。持久记忆有助于连续性，但也会把参考特征的影响长期保留，干扰既有上下文。ReferStream 的切入点正是把参考写入视为一次可回滚的瞬态写操作：只改注入 chunk 所需的全局条目，提交后恢复原条目。
【主线四：评测协议】已有主体驱动视频评测多在离线设定下比较参考相似度、文本遵循和视频质量，缺少 scheduled reference arrival、演化指令和既有主体保持的联合度量。ReferStream Bench 的贡献在于把这些事件时间表、主体存在性标注和 InShot/MultiShot 设置组织成可执行协议。
【研究缺口】综合来看，论文补的是三处空白：一是把参考到达时间纳入生成条件，形成因果的交互式主体注入任务；二是提出无需训练的 Joint Latent 与 Cache Restoration 组合，使冻结生成器可以吸收新参考而不回改历史；三是建立事件对齐的 benchmark，同时考察新主体保真度、既有主体保持、文本遵循、数量准确率和视频质量。需要注意，论文在提供片段中对相关工作的展开较简洁，引用编号 [1,5,8,11,15,21,29,30,33] 的具体贡献未全部展开；以上归纳只基于摘要、Introduction、Method 和 Results 中明确出现的线索，未对未展示的引用内容做臆测。

Q3: 论文如何解决这个问题？

【总体思路】ReferStream 不训练生成器，而是冻结 LongLive2-5B，只修改参考条件方式和部分全局 cache 条目。系统把用户交互建模为事件序列：第 j 个事件 e_j = (k_j, R_j, B_j, p^e_j, c^e_j)，其中 k_j 是 chunk 边界，R_j 是有序参考列表，B_j 是归一化目标框，p^e_j 是事件指令，c^e_j 是 cut flag。每个边界应用一个更新，把分配给该边界的参考分组处理；目标框可以显式给出，也可以由 layout 默认解析。
【步骤一：Joint Latent 在当前场景中构造参考特征】对每个事件，先取最新的干净场景 latent，把参考图像 resize 后放入目标框对应的区域，形成 joint latent z_joint。随后在空 cache 中做一次干净 DiT 前向，扩散时间 u=0，条件为事件指令 e_p；e_p 把可用参考描述和布局绑定到当前指令。该前向返回参考 token 在选定层中的 K/V slices，记为 R_j = Φ_θ(z_joint^j, 0, e_p^j; ∅, π_Ij)。这里 π_Ij 表示参考 token 的索引或层选择，∅ 表示空 cache。关键设计是：参考特征不是在独立图像上编码出来的，而是在当前场景 latent 的目标区域中提取，因此能看到当前尺度、邻域内容、光照和部分遮挡关系，比独立编码更适合指导融入。
【步骤二：Cache Restoration 做瞬态注入】注入前先快照被参考写入位置上的原始全局 cache 条目：B_j = copy(M^-_j[I_j])。然后把这些位置的参考 K/V 写入 cache：M^f = Write_Ij(M^-_j, R_j)。用修改后的 cache 对注入 chunk 去噪：(ẑ_kj, M_den) = D_θ(ϵ_kj, p_kj; M^f, c_kj)；随后执行一次干净提交和 cache 维护：M_j = U_θ(M_den, ẑ_kj, p_kj; c_kj)；最后把原始全局条目恢复：M^+ = Write_Ij(M_j, B_j)。这样参考特征只直接影响注入 chunk，提交后全局记忆回到原来的上下文，但新生成的 chunk 作为历史保留下来。
【新主体如何延续】从机制描述可以合理推断：注入 chunk 在参考条件下生成了带有新主体的内容，该内容经过 clean commit 进入历史；后续 chunk 不再直接读取参考 K/V，而是通过已经生成的历史 latent 看到新主体。这种设计的优点是避免参考特征长期污染全局记忆，缺点是主体的后续一致性依赖生成历史本身，若注入 chunk 没有把主体渲染稳定，后续可能漂移。
【与 backbone 的关系】LongLive2-5B 原本的更新包含 D_θ 去噪和 U_θ 提交，cache 由 global sink G、shot sink S 和 rolling window L 组成，重叠 token 去重。ReferStream 保留模型参数固定，只改参考条件路径和选定的全局条目，因此是一个推理期插件式方法，不需要额外训练数据或重新训练生成器。
【设计权衡】Joint Latent 提升参考与场景的结合质量，但依赖目标框和布局；Cache Restoration 提升既有主体保持，却可能削弱参考特征的持续影响，形成 preservation 与 new-subject fidelity 的张力。论文的消融也显示，强化历史保持会降低重复，但同时压低 New-ID。

Q4: 论文做了哪些实验？

【Benchmark 设计】论文在 ReferStream Bench 上评测，覆盖 InShot 与 MultiShot 两类设置。Benchmark 包含按计划到达的参考、演化指令和显式主体出现标注；事件按物理时间定义，评测按事件时间表对齐。允许的镜头切换和请求的主体离场决定哪些观测适用；主体存在性标注用于判断新主体是否出现、是否被重复生成以及数量是否符合指令。论文把该 benchmark 描述为 curated task resource，而不是大规模自然分布数据集。
【在线事件条件协议】ReferStream 与 LongLive 2.0 在同一事件时间表下比较。LongLive 2.0 接收 stage captions 和声明的 cut，但不接收参考图像，包括初始化阶段也不接收；ReferStream 在计划 reveal 时接收视觉参考。主比较使用已生成的 ReferStream 输出，并按 benchmark 时间线对齐，细节在 Appendix A.3。该比较评估完整系统，而不是单独组件；组件贡献由 4.4 的 paired ablations 考察。
【离线全参考协议】VACE-14B、Phantom-14B 和 HOMIE-14B 在时间零点接收全部参考和完整时间顺序脚本。离线运行使用 seed 0、50 个采样步、832 × 480 输出；VACE 与 Phantom 使用 16 fps，HOMIE 使用 24 fps，帧数按请求时长匹配，文本上下文在需要时扩展。LongLive 与 ReferStream 视频使用 1280 × 704。评测把所有方法 resize 到 832 × 480 RGB，并按物理时间采样。论文明确说明这些运行不在匹配分辨率下比较速度。
【评测指标】指标覆盖文本遵循、主体相似度、主体身份保持、重复生成、数量准确率和视频质量。片段中出现的具体指标包括 Existing-ID、New-ID 和 duplication；Existing-ID 衡量既有主体身份保持，New-ID 衡量新主体与参考的相似度，duplication 衡量每个主体被意外复制的程度，count accuracy 衡量指令要求的数量是否正确。所有方法使用同一套 evaluator 和阈值；具体公式和预处理在 Appendix A.4，提供的片段没有给出完整定义。
【消融协议】常见比较集使用 48 个 paired InShot 案例，seed 0；初始化子集包含 39 个十秒案例。论文用这些 paired ablations 检查 Joint Latent 与 Cache Restoration 的贡献。由于只使用一个 seed 和较小案例集，消融结果适合看趋势，不足以支撑跨种子稳健性结论。
【公平性与边界】在线 baseline LongLive 2.0 是纯文本条件，不接收参考，因此它检验的是有无参考注入的差异，而不是一个同样具备 subject 控制能力的在线系统。离线 baseline 在信息上更强，提前拿到全部参考和完整脚本，但不是因果流式设定；因此比较不是同分辨率、同帧率、同计算预算的架构对比。论文还说明，参考资产不一致的条目会被统一排除，排除规则和时间对齐规则在 Appendix A 中记录，这会直接影响 benchmark 的可比性和复现。

Q5: 发现了什么实验现象？

【主结果方向】在 ReferStream Bench 的 InShot 与 MultiShot 设置上，论文报告 ReferStream 在文本遵循、主体相似度和数量准确率上高于对比基线，包括提前获得全部参考的离线方法 VACE、Phantom 和 HOMIE。这个结果说明：即使离线方法拥有更完整的信息，在线因果注入配合 Joint Latent 与 Cache Restoration 仍可能在主体相似度、指令遵循和数量控制上取得优势；但论文没有在提供片段中给出完整的主表数值和每个设置的分项数值，不能据此推断所有指标全面领先。
【视频质量是混合结果】论文明确说视频质量结果 mixed。这意味着注入新主体和恢复 cache 可能带来画质损失或视觉伪影，方法并不是在所有维度上单调优于基线。对实际采用而言，需要把主体保真度和整体视频质量分开看，不能只引用文本遵循和相似度的正向结论。
【Joint Latent 的定性失败案例】消融显示，没有 joint construction 时，红色水壶会改变形状。这说明独立编码参考特征时，模型缺少当前场景提供的尺度、邻域和结构约束，容易把参考主体的形状和外观生硬地复制到视频中；Joint Latent 的作用不仅是提供外观，还在于把参考放到当前场景的目标区域中，让特征提取带上上下文。
【Cache Restoration 对既有主体的保持】Cache Restoration 在注入 chunk 提交后恢复原始全局条目，使既有主体保持得到改善：片段显示它把 Existing-ID 从 69.36 提升，但未给出提升后的完整数值。这个趋势符合设计动机：参考 K/V 如果长期留在全局记忆中，会持续干扰既有主体；恢复原条目可以降低这种污染。
【保持与保真的张力】另一组消融显示，某个更强调历史保持的配置把 new/existing duplication 分别从 16.67/24.13 降到 14.41/23.26，同时 New-ID 从 63.10 降到 51.31。也就是说，抑制重复生成和保持既有内容，可能以降低新主体与参考的相似度为代价。Figure 5 被用来说明这些目标需要一起考虑；这揭示了一个核心 trade-off：参考影响越强，新主体越像参考，但越可能污染历史；参考影响越短，历史越干净，但新主体保真度可能下降。
【指标解释的边界】Existing-ID 在初始化时标注的是另一个预期主体，它并不测试后续参考到达之后的保持；因此该指标不能单独证明跨事件的主体保持能力。duplication 衡量每个主体被意外复制的程度，与 count accuracy 相关但不完全相同；自动主体指标仍需进一步验证。
【统计与复现边界】主比较和消融主要建立在 seed 0 上，常见消融集只有 48 个 paired InShot 案例；论文在结论中明确说比较没有建立跨生成种子的稳健性。主比较还使用已生成的 ReferStream 输出并按 benchmark 时间线对齐，复现时需要核对 Appendix A.3 的对齐规则。离线与在线方法的分辨率、帧率不同，论文不做匹配分辨率的速度比较，因此不能从这些实验推断实时性或效率优势。
【InShot 与 MultiShot】论文在两个设置上都报告了优势，但提供片段没有给出逐设置的完整数值。合理推断 MultiShot 更难，因为镜头切换会改变场景上下文，Cache Restoration 需要同时处理 cut 和主体保持；但具体难度差异需要回主表和附录确认，不能从摘要直接下结论。

Q6: 有什么可以进一步探索的点？

【多 seed 与统计稳健性】最直接的方向是扩展到多个生成种子、更多案例和置信区间，验证文本遵循、主体相似度、数量准确率和视频质量的提升是否稳定。当前结论主要依赖 seed 0 和较小的 paired InShot 消融集，跨种子方差可能改变方法排序。
【自动主体指标验证】New-ID、Existing-ID、duplication 和 count accuracy 目前依赖自动 evaluator；需要与人类判断做相关性分析，特别是在遮挡、姿态变化、尺度变化、运动模糊、光照变化和部分出画的情况下。还应区分身份相似度、外观相似度和上下文合理性，避免单一 embedding 相似度掩盖伪影。
【更长流与更多主体】可以研究更长时间跨度、多主体同时到达、主体重复进出、主体离场后再次出现、跨镜头身份保持等场景。随着流长度增加，global sink、shot sink 和 rolling window 的容量与淘汰策略会直接影响注入效果；需要分析 cache 预算、恢复频率和历史长度的影响。
【事件调度与交互控制】可以探索参考在任意时刻到达、多个参考冲突、参考替换或撤销、用户纠正、主体数量变化、主体离场指令等更复杂的交互。还需研究到达时间敏感性：如果参考早一个 chunk 或晚一个 chunk 到达，主体出现时间和场景一致性会如何变化。
【布局与物理合理性】Joint Latent 依赖目标框和 layout 默认解析；未来可以研究自动布局、深度感知放置、遮挡关系、光照协调和物理合理性，减少对显式 box 的依赖。对于参考主体与场景中已有物体的交互，还需要处理碰撞、遮挡和阴影。
【瞬态注入的时长与强度】Cache Restoration 目前把参考直接影响限制在注入 chunk；可以研究自适应注入时长、分层注入强度、按主体可见性决定恢复时机，或学习何时应该保留参考影响、何时应该恢复历史。这与 continual learning 中的 stability-plasticity trade-off 有类比。
【效率与实时性】论文没有做匹配分辨率的速度比较；未来应测量额外的前向提取、cache 写入/恢复和布局处理带来的延迟，评估在真实流式交互中的可行性。还需要研究低延迟场景下的 chunk 大小、采样步数和 cache 更新频率的权衡。
【Benchmark 扩展与安全】ReferStream Bench 是 curated task resource，可以扩展到真实用户会话、长尾主体、复杂脚本和多镜头叙事；同时需要研究真实人物参考的 consent、deepfake 防护、水印和滥用限制。
【跨领域联系】Cache Restoration 的写-恢复机制可以类比 agent 的 transactional memory：临时写入外部信息，提交后恢复长期记忆，只保留必要的生成历史。对交互式世界模型、教育视频、影视预可视化和科学可视化中需要中途注入新实体的场景，这种瞬态条件注入思路有迁移价值。

Q7: 总结一下论文的主要内容

【1. 问题与设定】论文研究流式视频生成中的交互式主体注入：视频已经开始生成后，用户才提交新的视觉参考，系统必须在下一个 chunk 边界把参考主体融入当前场景，同时不修改已经输出的帧、不破坏已经建立的主体和场景。典型场景是用户先看到包含雨伞的大堂，再上传一张猫的照片，并要求猫从右侧进入；参考只规定猫的外观，系统还要决定它如何以合适尺度、位置和光照融入当前视频，并保留雨伞。多个参考可以在不同时间到达，指令也会演化。
【2. 与已有方法的关键差异】自回归视频扩散可以逐 chunk 生成并依赖 cached history，LongLive/LongLive2 支持生成中改文本，Anchor Forcing 改进了 prompt 变化下的记忆连续性；Phantom、VACE、HOMIE 等视觉参考方法可以控制主体外观，AlcheMinT 还能指定参考主体的时间区间。但这些方法大多假设参考在生成前已经给定，或者以离线方式统一规划。ReferStream 把参考到达时间作为一等约束：新信息只能在到达之后影响生成，已输出内容不可回改。
【3. 两个核心困难】第一，参考图像是在独立尺度、姿态和背景下描绘主体的，独立编码只提供外观，不提供如何融入当前场景的引导；直接拼接容易产生尺度错误、外观复制和场景冲突。第二，流式生成依赖持久 cache 维持长程一致性；把参考特征长期写入全局记忆会干扰既有主体，造成漂移、重复或遗忘，但完全不留影响又无法让新主体延续。
【4. 方法总览】ReferStream 是一个免训练框架，冻结 LongLive2-5B，只改参考条件路径和部分全局 cache 条目。它把用户交互建模为事件 e_j = (k_j, R_j, B_j, p^e_j, c^e_j)，在 chunk 边界应用。每个事件包含参考列表、目标框、事件指令和 cut flag；目标框可以显式给出，也可以由 layout 默认解析。核心组件是 Joint Latent 与 Cache Restoration。
【5. Joint Latent】Joint Latent 先取最新干净场景 latent，把参考 resize 后放入目标框区域，形成 joint latent；然后在空 cache 中做一次 u=0 的干净 DiT 前向，条件为事件指令，提取参考 token 在选定层中的 K/V。这样参考特征是在当前场景上下文中提取的，能看到尺度、邻域内容、光照和部分遮挡，而不是在独立图像上编码。公式上可写成 R_j = Φ_θ(z_joint^j, 0, e_p^j; ∅, π_Ij)，其中 π_Ij 表示参考 token 的索引或层选择。
【6. Cache Restoration】Cache Restoration 在注入前快照被写入位置上的原始全局 cache 条目，把参考 K/V 写入这些位置，用修改后的 cache 去噪注入 chunk，执行 clean commit 和 cache 维护，最后恢复原始全局条目。这样参考特征只直接影响注入 chunk；提交后全局记忆回到原来的上下文，但新生成的 chunk 作为历史保留。新主体在后续 chunk 中的延续，机制上依赖已经生成的历史 latent，而不是持续存在的参考 K/V。
【7. Benchmark】论文提出 ReferStream Bench，包含按计划到达的参考、演化指令和显式主体出现标注，覆盖 InShot 与 MultiShot。事件按物理时间定义，评测按事件时间表对齐；允许的 cut 和请求的主体离场决定哪些观测适用。指标覆盖文本遵循、主体相似度、既有主体保持、重复生成、数量准确率和视频质量；具体 evaluator 在 Appendix A.4。
【8. 实验协议】在线比较中，LongLive 2.0 接收 stage captions 和 cut，但不接收参考；ReferStream 在计划 reveal 时接收参考。离线比较中，VACE-14B、Phantom-14B、HOMIE-14B 在时间零点接收全部参考和完整脚本。离线运行使用 seed 0、50 步、832 × 480；VACE/Phantom 为 16 fps，HOMIE 为 24 fps；LongLive 与 ReferStream 为 1280 × 704。评测统一 resize 到 832 × 480 并按物理时间采样，不做匹配分辨率的速度比较。消融使用 48 个 paired InShot 案例、seed 0。
【9. 主要发现】论文报告 ReferStream 在文本遵循、主体相似度和数量准确率上高于对比基线，包括提前获得全部参考的离线方法；视频质量结果 mixed。消融显示：没有 Joint Latent 时，红色水壶会改变形状，说明独立编码缺少场景约束；Cache Restoration 把 Existing-ID 从 69.36 提升，说明恢复全局条目有助于既有主体保持；另一组保持优先的配置把 new/existing duplication 从 16.67/24.13 降到 14.41/23.26，但 New-ID 从 63.10 降到 51.31，揭示保持与保真之间的张力。
【10. 局限与意义】论文明确说比较没有建立跨生成种子的稳健性，自动主体指标仍需进一步验证；视频质量 mixed，在线/离线比较也不是同分辨率、同帧率、同计算预算。Existing-ID 在初始化时只标注另一个预期主体，不测试后续参考到达后的保持；主比较还依赖已生成输出与 benchmark 时间线的对齐。总体而言，这篇论文的贡献不只是提出一个免训练注入模块，而是把交互式主体注入定义成一个包含任务设定、事件协议、benchmark 和评测指标的完整问题；其核心洞察是同时考虑参考特征在什么场景上下文中构造，以及它们在持久记忆中影响多久。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与生成方向直接相关：论文处理流式视频生成、视觉参考条件和 KV cache 控制，属于推理期可控生成，而不是只做离线视频合成。

## 基本信息

- 作者：Shuolin Xu, Guanyuan Li, Boyu Li, Kelu Yao, Xiaogang Xu, Hongchuan Yu, Shuicheng Yan, Xiaobin Hu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV
- 日期：2026-10-28
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.37566`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Method、Results、References 片段，并与摘要、Introduction、Method、Results、Conclusion 交叉核对；检索片段未覆盖的具体数值、指标公式和附录细节均已在文中标注为需回原文确认。
