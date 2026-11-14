---
user_id: "cheng tan"
paper_id: 21063
arxiv_id: "2611.12557v1"
title: "WanderBench: Can World Models Explore Dynamic Worlds from Video?"
publish_date: "2026-11-09"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.12557v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.12557v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T02:17:30"
---
# WanderBench: Can World Models Explore Dynamic Worlds from Video?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：world models · video generation · benchmark · camera control

## 一句话总结

论文提出 WanderBench——用 745 段单镜头视频、三档时长、键盘相机路线和六维 24 个经人类验证的子指标，评估视频条件世界模型在动态场景探索中是否同时保持相机控制、源动态与重访一致性；对八个模型的测试显示长时程下视觉质量与静态场景尚可但相机控制、源动态和重访一致性显著退化，作者据此提出免训练框架 WanderLand，通过逐帧注入同一时刻源证据与重访时自身早期静态内容，在五个基础模型上实现全维度提升（源动态保真度最高 +8.3）。

## 摘要

> Video world models conditioned on an input video make it possible to explore real, recorded dynamic scenes, enabling re-shooting of footage, free-viewpoint replay, and real-to-sim environments for embodied agents. Such uses require the generated rollout to stay faithful to the recorded world, yet existing benchmarks start from a single image or a text prompt, and video-conditioned generation is evaluated mainly on perceptual quality and camera following. These evaluations do not directly assess whether exploration preserves subject identity, event progression, and scene consistency upon revisiting a location. We introduce WanderBench, a benchmark for exploring dynamic worlds from video. It comprises 745 single-shot source videos of real and AI-generated content in three difficulty tiers (3-25 s), paired with camera routes given as keyboard actions, and evaluates six dimensions with 24 human-validated sub-metrics. Benchmarking eight models, we find that as clips grow longer, models keep the static scene and visual quality but lose camera control and the source dynamics, and that none keeps revisited places consistent. We further propose WanderLand, a training-free framework that conditions each frame of a frozen model on the same-instant source and, on revisits, on its own earlier static content. WanderLand improves five models on every dimension, by up to 8.3 points in source-dynamics fidelity.

Q1: 这篇论文试图解决什么问题？

一、论文要解决的问题
论文关注的是“视频条件世界模型能否在动态世界中做忠实探索”这一评测与能力问题。具体而言：给定一段真实录制的单镜头动态场景视频，模型需要沿着用户给定的新相机轨迹生成 rollout；生成结果不仅要看起来合理，还要忠实于源视频记录的世界，包括主体身份、事件进程、静态场景布局，以及离开后重新访问同一地点时的一致性。
二、现有范式留下的缺口
1. 现有世界模型多从单张图像或文本提示出发。此时场景动态完全由模型想象，无法对应真实发生过的事件；即使视觉质量高，也不能证明模型能重放或探索一个被记录的世界。
2. 视频条件世界模型提供了新可能：以源视频为条件，沿新相机轨迹重新观察场景，支持重取景、自由视点回放和 real-to-sim。但这类应用的要求比单图探索更严格：模型必须同时合成合理新视图并保持源世界忠实度。
3. 现有基准与评估不匹配。论文指出，现有 benchmark 从单图或文本提示开始；视频条件生成主要评估感知质量和相机跟随；这些指标不直接测量探索是否保持主体身份、事件进展和重访地点一致性。
三、问题分解
论文把“忠实探索”拆成六个可测维度：视觉质量、相机控制、源静态世界保真度、源动态保真度、时空一致性、动态重访一致性。其隐含逻辑是：一个世界模型不能只靠 Overall 或 FVD 式感知指标判断；相机是否按动作走、动态事件是否与源同步、重访是否一致，是彼此独立的能力维度，可能相互解耦。
四、技术张力
1. 视觉质量与相机控制可能解耦：模型可以生成干净稳定的画面，却没有真正执行相机动作；Table 2 的 Simple 层片段中，World-in-World 的 Visual Quality 为 84.0，但 Camera Control 仅 44.4，说明高感知质量不等于可控探索。
2. 静态场景保真与动态保真可能解耦：模型可以重建静态布局，却把源视频中的运动事件替换成合理但不一致的内容；Source-World Fidelity 与 Source-Dynamics Fidelity 被分列，正是为了暴露这种差异。
3. 长时程一致性与重访一致性是独立瓶颈：片段变长时，视觉质量和时空一致性下降较小，但相机控制、源动态和重访一致性下降更大；这说明长时程误差主要不是“画面变糊”，而是“世界不再忠实”。
4. 评测公平性张力：full-length 模型可一次渲染整段，frame-limited 模型最多 81 帧（TrajectoryCrafter 49 帧），只能在 Simple 层且只能评估其可生成帧；跨组比较 Overall 会混入协议差异。
五、隐含假设与边界
1. 单镜头源视频足以定义“世界”；论文不处理多镜头拼接、镜头切换或源视频本身有遮挡/缺失的情况。
2. 相机路线以键盘动作给出，动作空间与轨迹分布有限；不能直接代表连续自由视点、复杂交互或具身动作。
3. “忠实”被操作化为六维 24 子指标，且经人类验证；但子指标是否覆盖物理规律、光照、反射、透明材质等仍未知。
4. 源视频长度 3–25 s，长时程探索仍受限；重访一致性在更长时程、更多次重访下的表现未知。
六、对 heuristic_draft 的校准
heuristic_draft 的 one_sentence_summary 仅复述“提出 WanderBench”，没有体现研究问题、发现和方法；其 core_method 字段实际摘录的是 Results 中“长时程退化”的观察，而不是方法；key_results 摘录的是图注。这些错位会让读者误判论文贡献。本文以摘要、Introduction、Results、Discussion、Conclusion 和检索片段为准，重新组织问题分析。

Q2: 有哪些相关研究？

一、视频生成与交互式世界模型
论文把工作放在从固定长度视频生成走向交互式世界模型的趋势中：用户可在生成过程中持续发出相机或动作控制，探索不断展开的视觉世界；应用包括游戏创作、虚拟制作和机器人策略学习。Introduction 引用了一组代表性工作（如 [8,14,17,20,39,47,49,57]），说明该方向已从“生成一段视频”转向“可交互地探索场景”。
二、单图/文本条件世界模型
现有大多数世界模型以单张图像或文本提示为条件。其场景动态完全由模型想象，不对应真实事件；因此即使生成质量高，也无法保证重放的是被记录的世界。这是论文要区分的核心：视频条件世界模型要求“探索一个已记录世界”，而不是“想象一个新世界”。
三、视频条件新视图生成
与论文最直接相关的是以源视频和目标相机轨迹为条件的新视图生成。检索片段给出了一个代表性描述：某类方法联合条件于源视频与目标相机轨迹，生成新视图，同时保持源视频的外观一致性和动态同步；WorldForge 则通过 reference-fusion correction 和 denoising guidance 改进轨迹控制视频生成。论文评估的八个模型覆盖 full-length 与 frame-limited 两类，包括 InSpatio-World 1.5、World-in-World、WorldForge v2、ReCamMaster、CameraAnything、UniWorld-View、WorldForge v1、TrajectoryCrafter；这些是理解其技术谱系的直接对象。
四、世界模型基准
论文在 Table 1 中与代表性视频生成/世界模型 benchmark 比较，强调自身同时具备视频/动作输入和长时程评估，并用六维 24 子指标覆盖忠实度。检索片段中可见 DrivingGen（自动驾驶生成式世界模型 benchmark）作为参考文献，说明 adjacent benchmark 关注特定应用域；但片段不足以判断 DrivingGen 是否被作为直接 baseline。现有 benchmark 的共性缺口是：从单图或文本开始，主要看感知质量与相机跟随，缺少对主体身份、事件进程和重访一致性的直接测量。
五、免训练测试时控制与重访记忆
WanderLand 属于免训练、测试时证据注入路线：不重新训练基础模型，而是在生成每个输出帧时按固定顺序注入证据。检索片段显示其第一步利用模型自身几何前端估计深度和相机；摘要补充其后条件于同一时刻源视频，并在重访时条件于自身早期静态内容。相关研究中的 reference-fusion correction、denoising guidance 以及 World-in-World/WorldForge 这类免训练框架，构成方法谱系；但论文强调 World-in-World 与 WorldForge 未被纳入 WanderLand 增强实验，因为它们本身是建立在预训练世界模型上的免训练框架，不暴露 WanderLand 重建所需的输入。
六、4D 重建与 real-to-sim
Discussion 把 WanderBench 定位为从单视图评估走向时间对齐多视图评估：每条探索轨迹是同一动态场景的新视图，并与源同步；表现好的模型有望从单一视频端到端生成全场景 4D Gaussian Splatting。这与动态 NeRF/3DGS、real-to-sim 和具身仿真相邻，但论文没有展开这些文献，相关连接应视为合理推断而非论文已综述的内容。
七、证据缺口
提供的 sections 中没有完整 Related Work 章节，只有 Introduction 的引用群和少量检索片段；因此无法逐一列出每篇相关工作的具体贡献。上述分类是基于论文自述与评估模型名单的重建，具体引用编号和 baseline 关系需回原文核对。

Q3: 论文如何解决这个问题？

一、总体思路
论文的“解决方案”由两部分组成：WanderBench 是评测基准，WanderLand 是提升现有模型忠实度的免训练框架。二者不是同一层面的贡献：前者定义问题与度量，后者是在该度量下验证一种测试时干预。
二、WanderBench 的设计
1. 数据：745 段单镜头源视频，混合真实与 AI 生成内容，分三个难度层级，总时长覆盖 3–25 s；Table 2 显示 Simple 层为 3–8 s、451 段，其余层级的具体时长边界与数量在可见片段中未给出。
2. 动作条件：每条源视频配对相机路线，以键盘动作表示；评估时把动作序列转换为每帧相机姿态（相对源相机），再通过各模型原生条件接口传入。不同帧率的模型接收同一动作序列并重采样到自己的时间网格。
3. 度量：六个维度、24 个经人类验证的子指标。六维为 Visual Quality、Camera Control、Source-World Fidelity、Source-Dynamics Fidelity、Spatiotemporal Consistency、Dynamic Revisit Consistency；Overall 为六维等权平均。检索片段给出两个子指标线索：Synchronized View Fidelity 将同一时刻源帧重投影到实现视图，与输出在共视区域比较；Source-Dynamics Fidelity 下设 Motion Timeline Agreement，比较运动时间线。其他子指标未在证据中展开。
4. 评估协议：八个模型分 full-length（一次渲染整段：InSpatio-World 1.5、World-in-World、WorldForge v2）与 frame-limited（最多 81 帧，TrajectoryCrafter 49 帧：ReCamMaster、CameraAnything、UniWorld-View、WorldForge v1、TrajectoryCrafter）。所有模型接收相同源视频和动作；frame-limited 模型只评估 Simple 层，且只评估其能生成的帧。
三、WanderLand 的三步证据注入
WanderLand 不训练基础模型，而是对每个输出帧按固定顺序取证据。检索片段明确第一步为 Synchronized camera：利用模型自身几何前端估计深度和相机，使源证据与目标视图在几何上同步。摘要给出另外两条核心条件：
1. 同一时刻源条件：每一帧都条件于源视频中同一时刻的内容，以保持源动态、主体和事件进程，而不是让模型自由想象。
2. 重访时自身静态内容：当相机回到曾访问的位置，条件于模型自己早先生成的静态内容，以填充源视频未覆盖的区域并维持重访一致性。
Results 图注补充：WanderLand 保留源视频中可见的内容，并用静态内容填充源未覆盖区域；TrajectoryCrafter 因 49 帧限制，输出在 3 s 后结束。
四、适用边界与权衡
1. 模型无关但接口相关：WanderLand 需要相机和源渲染输入（Conclusion 明确），并依赖基础模型自身几何前端；对不暴露这些中间输入的模型不适用。检索片段称 World-in-World 与 WorldForge 未被纳入，因为二者本身是免训练框架且不暴露 WanderLand 重建所需输入。
2. 免训练优点：不需要重新训练，可快速适配多个基础模型；在五个模型上每维度提升，说明证据注入具有跨模型通用性。
3. 代价：逐帧几何估计与源渲染可能增加推理开销；长时程重访依赖自身早期生成内容，若早期内容有误，误差可能累积；源动态与生成动态的时间对齐仍是难点。
五、与 heuristic_draft 的校准
heuristic_draft 的 core_method 字段写成“模型在 Visual Quality 和 Spatiotemporal Consistency 上损失小，在 Camera Control、Source-Dynamics Fidelity 和 Dynamic Revisit Consistency 上损失大”，这是实验结果而非方法。WanderLand 的实际方法是三步证据注入；WanderBench 的实际“方法”是基准构建与评估协议。本文按此纠正。

Q4: 论文做了哪些实验？

一、基准构建与人类验证
论文构建 WanderBench：745 段单镜头源视频，真实与 AI 生成混合，三难度层级（3–25 s），配对键盘动作相机路线，六维 24 子指标，并称子指标经人类验证。可见片段未给出标注者数量、标注界面、一致性统计或自动指标与人类判断的相关性；这些是评估构建质量的关键缺口。
二、主评估协议
1. 输入统一：所有模型接收相同源视频和相同动作序列；动作转换为相对源相机的每帧相机姿态，通过模型原生条件接口、原生分辨率和帧率传入；帧率不同则重采样动作到模型时间网格。
2. 模型分组：full-length 模型一次渲染整段，包括 InSpatio-World 1.5、World-in-World、WorldForge v2；frame-limited 模型最多 81 帧（TrajectoryCrafter 49 帧），包括 ReCamMaster、CameraAnything、UniWorld-View、WorldForge v1、TrajectoryCrafter。
3. 评估范围：frame-limited 模型只处理每个片段开头到最大长度，并且只在 Simple 层评估；full-length 模型可在三个难度层级评估。这意味着跨组 Overall 不可直接比较。
4. 指标：六维 24 子指标，Overall 为六维等权平均；Table 2 按难度层级报告，粗体标每层最佳，灰色行表示 frame-limited。
三、主结果
Table 2 的 Simple 层片段给出部分数值：InSpatio-World 1.5 为 Visual 74.9、Camera 75.4、Source-World 87.5、Source-Dyn 65.8、Spatiotemp 83.4、Revisit 54.5、Overall 73.6；World-in-World 为 Visual 84.0、Camera 44.4、Source-World 90.5、Source-Dyn 65.8、Spatiotemp 83.3、Revisit 53.1（Overall 被截断）。这些数值说明：静态世界保真度可以很高，但动态保真度和重访一致性明显更低；视觉质量最高的模型未必相机控制最好。Medium/Long 层的具体数值未在证据中给出，不能据此判断完整排序。
四、WanderLand 增强实验
作者把 WanderLand 应用于五个基础模型，并在定性图中展示：第一行为源视频，其余行为五个基础模型加 WanderLand 的输出；TrajectoryCrafter 因 49 帧限制，输出在 3 s 后结束。定量上，摘要称 WanderLand 在五个模型上提升每个维度，源动态保真度最多提升 8.3 点；但可见片段没有给出五个模型的完整名单、逐模型逐维度数值、统计显著性检验或计算开销。检索片段还说明 World-in-World 与 WorldForge 未被纳入，因为二者不暴露 WanderLand 所需输入；具体版本需回原文确认。
五、长时程分析
论文的核心实验观察是“随片段变长”的维度间退化差异：视觉质量和时空一致性损失较小，相机控制、源动态保真度和动态重访一致性损失较大；一个 rollout 可以看起来干净稳定，却没有真正执行相机动作或保持源事件。这不是单次消融，而是跨难度层级的趋势分析；但可见证据只给出趋势描述和一个截断句，缺少每层斜率、置信区间和模型间方差。
六、实验设计中的公平性与缺口
1. frame-limited 模型只在 Simple 层评估，且只评估可生成帧；与 full-length 模型在 Long 层比较不公平。
2. 各模型使用原生分辨率/帧率，视觉质量比较可能受分辨率、压缩、后处理影响。
3. Overall 等权平均假设六维同等重要；对 real-to-sim 或重访应用，Revisit 和 Source-Dyn 可能应加权更高。
4. 人类验证细节缺失；24 子指标是否独立、是否存在冗余、自动子指标与人类评分的一致性均未知。
5. 未报告失败案例比例、推理时间、显存占用；WanderLand 的“免训练”优势是否以显著计算开销换取，尚不能判断。

Q5: 发现了什么实验现象？

一、核心退化趋势
论文最重要的实验现象是：随着片段变长，模型在 Visual Quality 和 Spatiotemporal Consistency 上损失较小，但在 Camera Control、Source-Dynamics Fidelity 和 Dynamic Revisit Consistency 上损失大得多。换句话说，长时程生成的主要失败不是“画面变糊”或“闪烁”，而是“世界不再按源视频和用户动作演化”。一个 rollout 可以干净、稳定、视觉上可信，却没有真正执行相机轨迹，也没有保持源视频中的事件进程。
二、维度间解耦与张力
1. 视觉质量 vs 相机控制：Simple 层片段中 World-in-World 的 Visual Quality 为 84.0，但 Camera Control 仅 44.4；InSpatio-World 1.5 的 Visual 为 74.9，Camera 为 75.4。前者说明高感知质量可以伴随弱可控性；后者说明相机控制较好的模型视觉质量未必最高。Overall 等权平均会掩盖这种张力。
2. 静态世界 vs 动态世界：InSpatio-World 1.5 的 Source-World Fidelity 为 87.5，World-in-World 为 90.5，但两者 Source-Dyn Fidelity 均为 65.8。静态布局可以重建得较好，而动态事件保真度明显落后；模型可能生成“合理但不对应源事件”的运动。
3. 时空一致性 vs 重访一致性：两者不是同一件事。模型可以在短时程内保持帧间平滑，却在回到同一地点时给出不一致的布局或内容。InSpatio-World 1.5 的 Spatiotemp 为 83.4，但 Revisit 仅 54.5；World-in-World 为 83.3 与 53.1。重访一致性是更难的长期记忆/3D 一致性问题。
三、重访一致性瓶颈
摘要和结论都强调：没有任何模型能在重访地点时保持一致性。这是跨模型的普遍负结果，而不是个别模型缺陷。其含义是：当前视频条件世界模型更擅长“向前生成”，不擅长“回到过去并保持一致”；这对自由视点回放、real-to-sim 和 4D 重建是致命短板。
四、WanderLand 的增益与边界
WanderLand 在五个基础模型上提升每个维度，源动态保真度最多 +8.3。这说明测试时注入同一时刻源证据和重访静态内容确实能缓解“想象漂移”。但增益有边界：
1. 它需要相机和源渲染输入，依赖基础模型几何前端；World-in-World、WorldForge 这类不暴露中间输入的免训练框架无法直接套用。
2. 重访时依赖模型自身早期静态内容；如果早期生成错误，后续重访可能“一致地错”，而不是回到源世界。
3. 增益是相对基础模型的，不代表绝对性能足够；论文未给出 WanderLand 后各维度是否达到应用阈值。
4. 逐帧证据注入可能增加计算开销；可见证据未报告延迟或吞吐。
五、失败模式与反直觉结果
1. “干净但错误”：视觉质量高、时空一致性好，但相机控制和源动态失败；这提醒评估不能只看感知指标。
2. “静态对、动态错”：静态场景保真度高，动态事件被替换；说明模型可能把动态场景当成静态 3D 场景做新视图合成。
3. “短时程可行、长时程漂移”：frame-limited 模型只评估开头，可能高估实际可用性；full-length 模型在长时程暴露相机控制和重访问题。
4. “重访一致性最低”：即使相机控制尚可，回到原地仍可能不一致；这是长时程记忆和场景表示的核心失败。
六、证据缺口
可见片段只给出 Simple 层两个模型的完整/部分数值，缺少 Medium/Long 的逐模型结果、WanderLand 的逐模型增益、消融实验（去掉某一步证据注入会怎样）、统计显著性和人类评分细节。因此上述现象的方向可信，但幅度和排序需回原文表格确认。

Q6: 有什么可以进一步探索的点？

一、长时程相机控制
1. 显式几何约束：把每帧相机姿态作为硬约束或闭环控制信号，而不是仅通过条件接口软注入；可利用深度、光流和相机内参做测试时校正。
2. 动作对齐训练：构建动作-视频配对数据，训练模型在执行长程轨迹时保持相机控制；WanderBench 可作为评测目标。
3. 误差累积分析：研究相机控制误差随时间和重访次数的增长规律，区分“动作没执行”与“执行了但视角估计错”。
二、源动态保真
1. 更强的时间对齐：WanderLand 已按同一时刻源条件注入，未来可加入运动时间线一致性损失、事件级对齐或光流约束。
2. 动态对象建模：源视频中的非刚性物体、遮挡、出画再入是难点；需要显式跟踪和持久对象记忆。
3. 源视频质量鲁棒性：真实视频可能有运动模糊、压缩伪影、光照变化；评估这些条件下的保真度退化。
三、重访一致性与 4D 表示
1. 持久场景记忆：用 3D/4D 表示（如 4D Gaussian Splatting）缓存已探索区域，重访时检索并融合，而不是仅条件于早期生成帧。
2. 时间对齐多视图：Discussion 提出表现好的模型可从单一视频端到端生成全场景 4D Gaussian Splatting；这需要跨视图时间同步、遮挡处理和几何一致。
3. 重访协议扩展：增加多次重访、不同路径回到同一点、动态物体在重访间变化等设置，区分静态布局一致与动态状态一致。
四、评估扩展
1. 更长时长与更多层级：当前 3–25 s，未来扩展到分钟级；报告各层级的退化斜率而非单点分数。
2. 人类验证细节：公开标注协议、标注者一致性、自动指标与人类判断的相关性；检验 24 子指标是否冗余。
3. 加权 Overall：针对 real-to-sim 或自由视点回放，给 Camera Control、Source-Dyn、Revisit 更高权重；或报告帕累托前沿而非单一均值。
4. 计算效率：把推理延迟、显存、交互帧率纳入基准；世界模型的实际可用性不仅取决于质量。
五、方法与系统
1. 降低 WanderLand 输入依赖：减少对相机和源渲染输入的依赖，适配黑盒模型；探索可学习的证据融合而非固定三步。
2. 与训练结合：免训练方法有效但有上限；未来可在训练中内化源证据和重访记忆，同时保留测试时校正。
3. 与 reference-fusion correction、denoising guidance 结合：这些技术与 WanderLand 的证据注入可能互补。
六、跨领域
1. 具身智能体：把 WanderBench 扩展到动作条件、物理交互和多智能体；real-to-sim 的策略学习需要世界模型对动作后果负责。
2. 机器人与自动驾驶：DrivingGen 等 benchmark 关注自动驾驶；WanderBench 的六维框架可迁移到驾驶场景的动态保真和重访。
3. 虚拟制作与游戏：重取景、自由视点回放需要长时程一致和低延迟；当前结果提示还有明显距离。
4. 科学仿真：若用于科学场景，需要额外的物理规律、材质和光照验证；论文未涉及，属于推测性方向。

Q7: 总结一下论文的主要内容

一、研究动机与问题
论文从视频世界模型的范式转变写起：早期视频生成关注固定长度片段的视觉质量、运动真实感和时序连贯；近期研究转向交互式世界模型，用户可在生成过程中持续发出相机或动作控制，探索不断展开的视觉世界。这类模型可用于游戏创作、虚拟制作和机器人策略学习。但大多数世界模型以单图或文本提示为条件，场景动态完全由模型想象，不对应真实发生过的事件。视频条件世界模型则提供另一种可能：给定真实录制的动态场景视频，沿新相机轨迹重新观察，支持重取景、自由视点回放和 real-to-sim。论文的核心问题是：现有模型能否在探索动态世界时保持对源视频的忠实？现有基准从单图/文本开始，视频条件生成主要评估感知质量和相机跟随，不直接测主体身份、事件进程和重访一致性，因此需要新基准。
二、WanderBench 的构建
作者提出 WanderBench：745 段单镜头源视频，真实与 AI 生成内容混合，分三个难度层级，总时长 3–25 s；每条视频配对以键盘动作给出的相机路线；评估覆盖六个维度、24 个经人类验证的子指标。六维是 Visual Quality、Camera Control、Source-World Fidelity、Source-Dynamics Fidelity、Spatiotemporal Consistency、Dynamic Revisit Consistency；Overall 为等权平均。Table 2 显示 Simple 层为 3–8 s、451 段；检索片段还给出子指标例子：Synchronized View Fidelity 将同一时刻源帧重投影到实现视图，与输出在共视区域比较；Source-Dynamics Fidelity 下设 Motion Timeline Agreement。基准的评估协议强调：所有模型接收相同源视频和动作；动作转换为相对源相机的每帧姿态，通过模型原生条件接口、分辨率和帧率传入；不同帧率重采样；frame-limited 模型只评估开头到其最大帧数。
三、评估模型与主结果
作者评估八个视频条件世界模型，按生成长度分两组：full-length（一次渲染整段）包括 InSpatio-World 1.5、World-in-World、WorldForge v2；frame-limited（最多 81 帧，TrajectoryCrafter 49 帧）包括 ReCamMaster、CameraAnything、UniWorld-View、WorldForge v1、TrajectoryCrafter。Table 2 的 Simple 层片段显示：InSpatio-World 1.5 的 Visual 74.9、Camera 75.4、Source-World 87.5、Source-Dyn 65.8、Spatiotemp 83.4、Revisit 54.5、Overall 73.6；World-in-World 的 Visual 84.0、Camera 44.4、Source-World 90.5、Source-Dyn 65.8、Spatiotemp 83.3、Revisit 53.1。仅从这些数值即可看出：静态世界保真度可以很高，动态保真度和重访一致性明显更低；视觉质量最高的模型相机控制可能最差。论文的核心发现是：随着片段变长，模型保持静态场景和视觉质量，但丢失相机控制、源动态和重访一致性；没有任何模型能在重访地点保持一致性。一个 rollout 可以看起来干净稳定，却没有真正执行相机动作或保持源事件。
四、WanderLand 方法
为缓解上述问题，作者提出 WanderLand：免训练框架，不重新训练基础模型，而是对每个输出帧按固定顺序注入证据。检索片段明确第一步是 Synchronized camera：利用模型自身几何前端估计深度和相机，使源证据与目标视图几何同步。摘要给出另外两条：每一帧条件于同一时刻的源视频，以保持源动态和主体；在重访时条件于模型自身早先生成的静态内容，以填充源未覆盖区域并保持重访一致。Results 图注补充：WanderLand 保留源视频可见内容，用静态内容填充未覆盖区域；TrajectoryCrafter 因 49 帧限制，输出在 3 s 后结束。作者把 WanderLand 应用于五个基础模型，声称每个维度都有提升，源动态保真度最多 +8.3。检索片段说明 World-in-World 与 WorldForge 未被纳入，因为二者本身是免训练框架且不暴露 WanderLand 重建所需输入；Conclusion 也明确 WanderLand 需要相机和源渲染输入。
五、讨论与结论
Discussion 把 WanderBench 的意义提升到评估范式：从单视图评估走向时间对齐多视图评估；每条探索轨迹是同一动态场景的新视图，并与源同步。作者期望在此表现好的模型能从单一输入生成时间对齐多视图视频，进而支持从单个视频端到端生成全场景 4D Gaussian Splatting。结论重申：当前世界模型保留静态场景和视觉质量，但长时程下丢失相机控制、源动态和重访一致性；WanderLand 是有效的免训练改进，但有输入依赖。作者希望 WanderBench 推动世界模型在探索时忠实于记录世界。
六、证据边界与阅读提示
提供的 sections 中 method 为空，WanderLand 三步的第二步/第三步名称和实现细节主要来自摘要与 Results 片段，需回原文核对；Medium/Long 层的具体数值、五个基础模型的完整名单、人类验证协议、消融实验、计算开销均未在可见证据中完整给出。Table 2 片段只覆盖 Simple 层两个模型，不能据此推断完整排名。论文为 arXiv preprint（2026-11-09），模型版本固定，结论可能随新模型变化。阅读时应把“基准构建”和“WanderLand 方法”分开评价：前者是系统性评测贡献，后者是测试时干预，二者证据强度不同。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与 generation 方向直接相关：论文覆盖视频条件世界模型、新视图合成、长时程生成和测试时证据注入，可作为视频生成评测与改进的参考。

## 基本信息

- 作者：Yuhang Chen, Jiheng Li, Jiale Wang, Zai Wang, Guangyue Liu
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-11-09
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.12557v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 语义检索命中的 Introduction、Results、Discussion、Conclusion 片段，并优先采用这些证据；heuristic_draft 中 one_sentence_summary 过简、core_method 与 key_results 存在结果/图注错位，已在对应字段纠正，未检索到的 Medium/Long 数值、人类验证协议和完整方法步骤均标注为证据缺口。
