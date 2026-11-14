---
user_id: "cheng tan"
paper_id: 20773
arxiv_id: "2611.10693v1"
title: "Machine Hearing: Can Audio LLMs Generalize Beyond Human Auditory Perception?"
publish_date: "2026-11-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Nov 2026/2611.10693v1.pdf"
pdf_url: "https://arxiv.org/pdf/2611.10693v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Nov 2026.md"
saved_at: "2026-11-11T01:57:28"
---
# Machine Hearing: Can Audio LLMs Generalize Beyond Human Auditory Perception?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：machine hearing · audio llm · benchmark · multimodal perception

## 一句话总结

论文提出 Machine Hearing Benchmark（MHB，6,128 个实例、13 个任务、7 个领域）检验 Audio LLM 能否从人类可听范围推广到更广泛的机器听觉信号，发现现有 ALLM 的零样本与监督适配均存在系统性差距，并据此提出轻量编码器扩展 MachEar 以缓解音频前端与异质信号之间的失配。

## 摘要

> Multimodal LLMs have made remarkable progress in perceiving the physical world, while their perceptual scope remains largely human-centric, focusing on what humans can directly see or hear. This progress raises the prospect of perception beyond human limits, such as extending hearing to sounds that are inaudible to humans. More broadly, the physical world is full of waves, vibrations, and oscillations, only a small fraction of which are directly audible to humans. This invites the extension of multimodal LLMs toward a broader form of machine hearing, which remains largely underexplored. To investigate this direction, we introduce the Machine Hearing Benchmark (MHB), comprising 6,128 instances across 13 tasks and seven domains. MHB spans highly heterogeneous input regimes, with sampling rates from approximately $10^{-5}$ to $10^6$ Hz, sequence lengths from $10^1$ to $10^6$ samples, and both single- and multi-channel inputs. Evaluations of current open-source and proprietary Audio LLMs (ALLMs) reveal limited zero-shot transfer to machine hearing. Supervised adaptation on machine-hearing data alone does not close this gap, suggesting that the challenge extends beyond data exposure to a systematic mismatch between existing audio front ends and machine-hearing inputs. To address this mismatch, we introduce MachEar, a lightweight encoder extension that complements the pretrained Audio LLM for machine-hearing inputs. The results show that MachEar equips a pretrained Audio LLM with stronger machine-hearing capabilities while largely preserving its existing audio understanding performance.

Q1: 这篇论文试图解决什么问题？

一、核心问题定位：论文要解决的不是单一任务精度，而是 Audio LLM 的感知范围被人类听觉经验窄化。现有 ALLM 在语音、音乐、环境声上表现强，但其前端、预训练语料和评测体系默认输入是 20 Hz–20 kHz、以人耳可听为中心的波形；地震波、工业振动、心音、水声、动物叫声、无线电与雷达波形、天文光变曲线等信号在频率、时长、通道数和信噪比上高度异质。论文由此提出“machine hearing”问题：能否让 ALLM 处理远超人类听觉范围的信号，并在此过程中保持原有音频理解能力。
二、为什么值得作为独立问题：作者在引言中把物理世界的波、振动和振荡作为统一动机，强调人耳可听部分只是很小子集。这个设定把传统的音频理解从“声音语义”推进到“任意可采样信号的理解”，与多模态 LLM 向科学、工业、医疗和遥感场景扩展的趋势一致。
三、技术难点：（1）输入 regime 极宽，采样率跨约 11 个数量级，序列长度跨 5 个数量级，还包含多通道输入；（2）任务异质，13 个任务覆盖分类、检测或更多形式，标签空间和评价指标不统一；（3）前端失配，传统音频前端（固定窗长、固定采样率、梅尔谱等）可能把非人听觉信号中的关键结构丢掉；（4）迁移与遗忘的权衡，增强机器听觉时不能破坏预训练 Audio LLM 的原有能力；（5）领域专精模型与通用模型各有胜负，说明“领域数据”本身不是充分条件。
四、隐含假设与可质疑点：论文把地震、无线电、雷达等统一称为 hearing，这是概念扩展而非严格意义上的听觉；合理推断，多数任务可被视作一维序列分类，若评测只要求分类标签，未必能证明 LLM 的语义理解或跨模态推理被真正调用。另一个风险是零样本评测中的 0.00 分可能来自标签空间、输出格式或答案解析不匹配，而不一定全是前端感知失败；现有片段没有给出 prompt 设计、解析规则和置信区间，需回原文核对。心音属于人类可听范围却被纳入 machine hearing，也说明该概念更像是“超越常规音频语料与前端”，而非严格的频率边界。
五、边界条件：MHB 的 6,128 个实例、13 个任务、7 个领域提供了有价值的压力测试，但不能外推为全部物理信号；每任务平均样本量有限，跨任务宏平均可能被难任务主导。论文的结论应理解为“在当前 ALLM 与当前前端条件下存在系统性差距”，而不是“机器听觉已被解决或不可解决”。

Q2: 有哪些相关研究？

一、Audio LLM 与多模态听觉理解：论文引言将 Chu et al. 2024、Tang et al. 2024、Xu et al. 2025 作为 ALLM 代表，指出这些模型在语音、音乐和环境声上强，但感知范围以人为中心。检索片段没有给出完整 Related Work 章节，因此这里只能按引用簇和实验基线重构；具体谱系需回原文核对。
二、机器听觉与非人感知：论文把地震波、工业振动、心音、水声、动物叫声、无线电波、光变曲线并置，试图把 audio LLM 从“人耳可听声音”扩展到“可采样物理信号”。这与传统音频事件检测、声学场景分类和语音识别不同：后者的标签空间围绕人类语义，前者面对的是传感器信号、频段和物理过程。合理推断，论文的相关工作会涉及信号处理、时间序列分类和领域基础模型三条线，但片段不足以确认其完整分类。
三、领域基础模型：实验对照包括 Time-MoE（通用时间序列）、SpectrumFM（频谱）、SeisLM（地震波形）、FISHER（工业信号）、SPEAR（语音与音频）。这些模型代表“领域专精”与“通用迁移”两种路线。论文的对照设计说明它关心的不是单一 SOTA，而是领域预训练能否在 MHB 的异质任务上稳定占优。
四、音频前端与编码器：论文的核心诊断是现有音频前端与 machine-hearing 输入存在系统性失配，因此 MachEar 被设计为轻量编码器扩展。消融片段显示 MachEar 至少包含 1D CNN 与 FFT 两条通路；这与传统音频前端中的时域卷积、频域变换和重采样问题直接相关。更完整的相关工作（如可学习前端、多分辨率编码、长序列 tokenization）在检索片段中未展开。
五、与本文的关系：本文的空白在于缺少一个跨领域、跨采样率、跨序列长度的统一 benchmark，以及缺少针对 ALLM 前端失配的轻量适配方法。它把“评测缺口”和“架构缺口”放在同一条论证链上：先证明现有模型不行，再证明仅靠监督数据不够，最后用编码器扩展补位。这个论证链有说服力，但也依赖 MHB 的任务构念效度和零样本评测协议；相关工作的完整对照需回原文确认。

Q3: 论文如何解决这个问题？

一、总体路线：论文采用“先建 benchmark，再做诊断，最后提方法”的三段式。第一步用 MHB 把机器听觉拆成 7 个领域、13 个任务、6,128 个实例；第二步评测开源与专有 ALLM 的零样本表现，并做监督适配，判断差距是否只是数据暴露不足；第三步提出 MachEar，把轻量编码器扩展附加到预训练 Audio LLM 上，试图在不重训整个模型的情况下补足前端。
二、MHB 的构造：输入覆盖约 10^-5 到 10^6 Hz 的采样率、10^1 到 10^6 个采样点的序列长度，以及单通道和多通道输入。领域包括光变曲线、地震波、工业振动、心音、水声、动物叫声、无线电波等；任务示例包括无线电调制方式分类、雷达脉冲段的波形类型分类、水下船只分类等。具体任务到领域的完整映射在检索片段中因表格断行而不完整，需回原文核对。
三、评测协议：论文对当前开源与专有 ALLM 做零样本评测；对机器听觉数据做监督适配；另外把 Time-MoE、SpectrumFM、SeisLM、FISHER、SPEAR 等领域基础模型冻结后作为对照。这个设计能区分“通用 LLM 的零样本能力”“有监督数据后的适配能力”和“领域专精模型的迁移能力”。主要指标在消融中使用 weighted Macro-F1，但主表是否统一采用该指标需回原文确认。
四、MachEar 的设计逻辑：MachEar 是轻量编码器扩展，目标是补充预训练 Audio LLM 的音频前端，而不是替代整个模型。消融证据显示它至少包含 1D CNN 通路和 FFT 通路：去掉 1D CNN 对短序列 regime 影响最大，去掉 FFT 则四个 regime 普遍下降。合理推断，1D CNN 负责局部时域波形特征，FFT 负责频域结构；二者互补，使模型能处理不同采样率和序列长度。具体融合方式、参数规模、训练目标、是否使用 adapter 或侧路编码器，现有片段不足以还原。
五、设计权衡：（1）轻量 vs 容量，扩展太大就失去“可附加”的意义，太小又难以覆盖极端采样率；（2）通用 vs 专精，MachEar 要在多个领域间共享，而领域基础模型可以专精但易负迁移；（3）增强 vs 保持，论文强调增强机器听觉的同时较大程度保留原有音频理解，这需要防止灾难性遗忘；（4）统一前端 vs 按 regime 路由，MHB 的异质性可能更适合多分支或条件计算，但论文是否采用该策略尚不清楚。
六、与已有方法的区别：领域基础模型通常从零预训练或领域微调，MachEar 则复用预训练 ALLM 的表征与语言接口；这使它的目标不是替代 SeisLM、FISHER 等模型，而是让通用 Audio LLM 具备更宽的输入适配能力。这个定位是本文方法贡献的关键，也是其局限所在：如果任务主要靠领域特征而非语言推理，轻量前端可能比完整领域模型更受限。

Q4: 论文做了哪些实验？

一、ALLM 零样本主实验：论文评测当前开源与专有 Audio LLM。检索片段可见 Gemini-3-Flash、GPT-Audio-Mini、Doubao-2.0-Mini、Doubao-2.0-Lite 等专有模型在六列任务上的分数，例如 Gemini-3-Flash 为 35.62、0.00、31.88、23.16、16.05、15.68；GPT-Audio-Mini 为 2.75、0.00、15.64、9.14、16.96、18.57；Doubao-2.0-Mini 为 32.73、0.00、31.02、14.89、7.74、11.91；Doubao-2.0-Lite 为 33.10、0.00、31.36、16.47、7.74 等。由于表头在 PDF 中断行，不能可靠断言每列对应哪个任务；但 0.00 的出现说明部分模型在部分任务上完全失效。整体结论是零样本迁移有限。
二、监督适配实验：论文进一步用机器听觉数据做监督适配。结果显示性能会提升，但不能完全闭合与理想水平之间的差距。作者据此论证：问题不只是数据暴露不足，而是现有音频前端与机器听觉输入之间存在系统性失配。这个实验是全文论证链的关键，因为它把“数据问题”与“架构问题”区分开。
三、领域基础模型对照：Table 2 冻结评估 Time-MoE、SpectrumFM、SeisLM、FISHER、SPEAR。第一组六列（GWOSC、LEAVES、Iquique、Llaima、Yaseen、CirCor-loc）中，Time-MoE 为 59.18、67.07、46.82、94.11、93.09、27.87；SpectrumFM 为 52.86、61.70、48.85、79.48、76.05、26.52；SeisLM 为 48.75、58.42、60.42、93.33、93.08、29.12；FISHER 为 68.75、0.00、0.00、0.00、94.20、38.57；SPEAR 为 78.91、44.82、52.28、95.51、93.88、51.07。第二组表格列名断行严重，可见 Time-MoE、SpectrumFM、SeisLM、FISHER、SPEAR 在电磁、水声、生物声学、制造等任务上的分数，例如 FISHER 在该组出现 0.00、0.00、23.17、48.69、92.57、73.93、53.69，SPEAR 为 67.42、31.02、25.96、58.69、77.55、60.57、53.34。由于列名无法可靠映射，这里只报告数值模式，不把它解释为具体任务排名。
四、MachEar 消融：论文按 G1–G4 四个 regime 做消融。去掉 1D CNN 通路后，G1 的 weighted Macro-F1 从 70.31% 降到 64.04%，G2 从 75.55% 降到 62.63%；说明 1D CNN 对短序列 regime 尤其重要。去掉 FFT 通路后，G1–G4 分别下降 2.82、5.15、1.68、5.46 个百分点，说明频域通路的影响更广。G1–G4 的具体划分在检索片段中没有定义，合理推测与序列长度或采样率 regime 有关，但需回原文确认。
五、保持原有音频能力：结论声称 MachEar 在增强机器听觉的同时较大程度保留原有音频理解性能。检索片段没有给出具体保留率、对照 benchmark 或遗忘幅度，因此这项目标目前只能作为作者声明，不能量化复核。
六、缺失的实验信息：主表的完整指标定义、训练超参、数据划分、是否去重、是否防止预训练污染、推理成本、参数规模、多通道处理方式、统计显著性检验，在现有证据中均不完整。阅读时应优先回原文核对这些问题，而不是只记住总体结论。

Q5: 发现了什么实验现象？

一、零样本接近失效，且出现 0.00：ALLM 在 MHB 上的零样本表现整体有限，部分模型在部分任务上直接为 0.00。这个现象至少有两种解释：其一是模型确实无法感知该信号；其二是标签空间、输出格式或答案解析不匹配导致评测端失败。论文倾向前者并把它归因于前端失配，但现有片段没有展示 prompt 与解析细节，因此不能排除评测混淆。阅读时应把 0.00 视为“需要拆解失败模式”的信号，而不是单一的感知结论。
二、监督适配能提升但不能闭合：这是全文最重要的实验现象。若差距只是数据分布不同，给机器听觉数据做监督训练应该大幅缩小差距；实际提升有限，说明现有音频前端的归纳偏置与异质信号不匹配。这个结果把论文从“benchmark 论文”推进到“架构诊断论文”，也是 MachEar 的立论基础。
三、领域专精不是万能药：FISHER 在部分制造任务上强，但在 GWOSC 之外的多个任务上出现 0.00，说明领域预训练可能带来强负迁移；SeisLM 在 Iquique 上领先但在 Llaima 上不是第一；SpectrumFM 在两个电磁任务上都没有排第一。反过来，SPEAR 这个音频基础模型在多个非传统音频任务上仍有竞争力，Time-MoE 这个通用时间序列模型在 LEAVES 和 RadSeg 上领先。结论是“预训练领域与目标任务对齐”比“模型是否号称领域专精”更重要。
四、不同架构补不同 regime：1D CNN 的消融在短序列 regime 最明显，FFT 的消融在四个 regime 都下降。这说明 MachEar 的收益不是单一模块的笼统提升，而是时域局部特征与频域全局结构的分工。合理推断，极端短序列更需要时域卷积，极端低/高采样率更需要频域变换与重采样鲁棒性；但 G1–G4 的定义未在片段中给出，不能把该解释当成定论。
五、增强与保持之间存在张力：论文强调 MachEar 增强机器听觉的同时保留原有音频理解，但保留程度没有在片段中量化。若扩展过强，可能改变预训练音频编码器的表征分布，造成遗忘；若扩展过弱，又难以覆盖 10^-5–10^6 Hz 的跨度。这个张力是后续方法设计的核心。
六、benchmark 异质性带来的评价风险：13 个任务、7 个领域的难度差异很大，宏平均可能被少数难任务主导；每任务平均约 472 个实例，统计噪声不可忽视。部分任务（如心音）本身在人类可听范围内，与“超越人类听觉”的标题存在概念张力；这提示 MHB 更像“超越常规音频语料与前端”的 benchmark，而不是严格的“非人听觉”测试。
七、潜在失败模式：长序列 10^6 采样点可能被截断或池化；多通道输入可能被混为单通道；不同采样率若被统一重采样，可能引入混叠或丢失高频；标签空间若跨任务共享，可能掩盖任务特化。这些 failure mode 在现有片段中未被系统报告，需回原文核对。

Q6: 有什么可以进一步探索的点？

一、把“机器听觉”操作化：未来工作应明确 machine hearing 的边界：是按频率划分（次声/超声）、按传感器划分（地震/无线电/水声）、还是按“超出常规音频语料”划分。心音这类人类可听信号被纳入，说明当前定义混合了频率、场景和前端三个维度。可探索的方向是建立任务 taxonomy，并区分“感知”与“推理”：哪些任务只需前端特征，哪些需要 LLM 的语义先验或跨模态推理。
二、前端架构：MachEar 的消融提示时域卷积与频域变换各有分工。后续可探索多分辨率前端、可学习滤波器组、动态重采样、抗混叠设计、复数域编码、长序列 tokenization、多通道融合与通道选择。还可以研究条件计算：按采样率或序列长度路由到不同分支，避免一个前端硬扛 10^-5–10^6 Hz。
三、跨域泛化与负迁移：FISHER 的 0.00 与 SPEAR 的跨域竞争力说明领域专精和通用迁移各有风险。可探索 mixture-of-encoders、领域提示、adapter 路由、元学习、少样本适配、持续学习，以及显式的负迁移检测。一个有价值的问题是：在什么条件下领域预训练会帮助而不是伤害 ALLM？
四、评测协议：需要报告每任务置信区间、类别平衡、prompt 敏感性、答案解析鲁棒性、校准误差和推理成本；需要区分“模型不会”与“模型不会按格式回答”。还应加入人类基线或传统信号处理基线，回答标题中的“beyond human auditory perception”到底在什么意义上成立。现有片段未显示人类被试基线，这是明显的证据缺口。
五、数据与基准扩展：MHB 的 7 个领域仍有覆盖盲区，可加入更多物理信号（引力波、射电天文、机械故障、脑电/肌电、流量/压力传感器）、更多采样率极端、更多多通道与缺失通道场景、流式与在线设定、跨传感器迁移。还需处理预训练污染：基础模型可能见过这些数据集，零样本分数可能被高估。
六、与 agent 和工具调用结合：对关注 agent 的读者，machine hearing 可以视为感知前端工具。可探索让 agent 根据信号类型调用重采样、滤波、时频分析、领域模型等工具，再把结果交给 LLM 做诊断、报告和决策。这比让单一 ALLM 端到端吞下所有信号更符合当前系统能力边界。
七、AI for science 与生成：在地震监测、工业故障诊断、心音筛查、水声目标识别、射电频谱监测等场景，可探索少样本、跨站点、跨设备泛化；在生成方向，可研究条件信号生成、仿真增强、反事实解释，以及用生成数据补足稀有事件。需要注意医疗、无线电监听等领域的安全、隐私与合规边界。
八、理论问题：为什么 1D CNN 对短序列更关键、FFT 对多 regime 更关键？能否用信息论、采样定理、归纳偏置理论解释？机器听觉的 scaling law 是否与文本/视觉不同？这些问题的答案会决定未来是继续做轻量前端，还是走向原生多模态信号 LLM。

Q7: 总结一下论文的主要内容

一、论文的主问题：论文问的是 Audio LLM 能否把感知范围从人类可听声音推广到更广义的机器听觉。作者指出，现有多模态 LLM 的听觉能力主要围绕语音、音乐和环境声，这些都属于人耳可听、以人为中心的音频；而物理世界充满波、振动和振荡，从地震、机器振动、心音、水声、动物叫声到无线电、雷达和天文光变曲线，频率、时长、通道和信噪比高度异质。人耳 20 Hz–20 kHz 只是其中很小一段，因此需要系统评估 ALLM 能否处理远超人类听觉范围的信号。
二、benchmark 设计：作者构建 MHB，包含 6,128 个实例、13 个任务、7 个领域。输入覆盖约 10^-5 到 10^6 Hz 的采样率、10^1 到 10^6 个采样点的序列长度，以及单通道和多通道输入。领域包括光变曲线、地震波、工业振动、心音、水声、动物叫声、无线电波等；任务示例包括无线电调制方式分类、雷达脉冲段波形类型分类、水下船只分类。这个设计的核心不是单一任务 SOTA，而是用极端异质的输入 regime 对现有音频前端形成压力测试。
三、评测与诊断：论文先评测当前开源与专有 ALLM 的零样本表现，结果显示迁移有限，部分模型在部分任务上出现 0.00。随后用机器听觉数据做监督适配，性能提升但不能完全闭合差距。作者据此提出全文最关键的判断：问题不只是数据暴露不足，而是现有音频前端与机器听觉输入之间存在系统性失配。换句话说，给模型更多领域数据并不能自动让它“听见”这些信号，前端的归纳偏置才是瓶颈之一。
四、领域基础模型对照：论文进一步冻结评估 Time-MoE、SpectrumFM、SeisLM、FISHER、SPEAR 等领域基础模型。结果显示领域专精并非万能：FISHER 在部分制造任务上强，但在多个其他任务上出现 0.00；SeisLM 在 Iquique 上领先但在 Llaima 上不是第一；SpectrumFM 在两个电磁任务上都没有排第一；反过来，SPEAR 在多个非传统音频任务上仍有竞争力，Time-MoE 在 LEAVES 和 RadSeg 上领先。作者由此论证，预训练领域与目标任务的对齐程度比“是否号称领域专精”更重要。
五、MachEar 方法：为缓解前端失配，作者提出 MachEar，一种可附加到预训练 Audio LLM 的轻量编码器扩展。它不替代整个模型，而是补充预训练音频编码器，使 ALLM 能处理更宽采样率和序列长度的输入。消融片段显示 MachEar 至少包含 1D CNN 与 FFT 两条通路：去掉 1D CNN 后，G1 的 weighted Macro-F1 从 70.31% 降到 64.04%，G2 从 75.55% 降到 62.63%；去掉 FFT 后，G1–G4 分别下降 2.82、5.15、1.68、5.46 个百分点。作者据此说明时域卷积对短序列 regime 更重要，频域变换对多个 regime 都有广泛影响。结论还声称 MachEar 在增强机器听觉的同时较大程度保留原有音频理解性能，但检索片段未给出具体保留率。
六、论证链与证据强度：全文的论证链是“现有 ALLM 不行 → 监督适配也不够 → 所以是前端架构问题 → 用轻量编码器扩展补位 → 增强机器听觉且保持原能力”。这条链条清晰，实验设计也有层次：零样本、监督适配、领域基础模型对照、MachEar 消融。但证据强度有几处需要打折：主表指标定义与 prompt/解析细节未在片段中展开，0.00 可能混入评测协议失败；每任务平均样本量约 472，统计噪声不可忽视；G1–G4 的定义缺失；保留原能力的声明缺少量化；人类基线缺失，使“beyond human auditory perception”难以严格验证。
七、定位与评价：这篇论文的价值主要不在单一模型 SOTA，而在于把“机器听觉”作为一个系统性问题提出来，并用跨领域 benchmark 把 Audio LLM 的感知边界暴露出来。它同时指出一个容易被忽略的架构事实：多模态 LLM 的听觉能力高度依赖音频前端，前端的采样率、窗长、频谱变换和 tokenization 会决定哪些信号根本进不了模型。MachEar 是一个务实的轻量补丁，而不是终局方案；它提示未来研究可以在前端多分辨率化、条件计算、领域路由和 agent 工具调用等方向继续推进。对关注 AI for science、agent 感知前端和跨域评测的读者，这篇论文提供了可复用的任务组织方式和诊断思路；但对其“超越人类听觉”的强主张，应结合完整实验协议、人类基线和失败模式分析再下判断。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：这篇论文的系统性很强：先建跨 7 领域、13 任务的 benchmark，再做零样本/监督适配/领域模型三层诊断，最后提出轻量前端扩展；适合作为“系统性工作优先”的阅读样本。

## 基本信息

- 作者：Liwei Liu, Jun Tao, Xingyuan Li, Xuenan Xu, Chen Zhang, Junyi Wang, Boya Zhang, Xiaoyu Yang, Zhen-Hua Ling, Wen Wu, Chao Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.SD, cs.CL
- 日期：2026-11-07
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2611.10693v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成优先使用了 PDF 检索证据中的 Introduction、Method、Conclusion 与表格片段，并对 heuristic_draft 中参考文献误置、消融结果误置和摘要残缺做了纠正；机构、完整任务映射、训练细节与人类基线等未能从证据确认的信息均明确标注为缺口。
