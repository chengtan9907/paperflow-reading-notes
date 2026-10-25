---
user_id: "cheng tan"
paper_id: 16511
arxiv_id: "2610.26860v1"
title: "OpenVLHarness: Real World Visual Agentic Tool-Use with Multimodal Memory"
publish_date: "2026-10-19"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.26860v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.26860v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-20T13:07:53"
---
# OpenVLHarness: Real World Visual Agentic Tool-Use with Multimodal Memory

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：visual agentic tool use · multimodal memory · capability layer · evidence rendering

## 一句话总结

OpenVLHarness 提出围绕能力层（Capability Layer）与持久多模态记忆（Persistent Multimodal Memory）构建的通用视觉智能体工具使用框架，让 MLLM 编排异构感知、搜索、代码工具并跨轮次复用多模态证据，在 22 个基准、8 个后端上的实验显示相比强训练无关基线（如 OctoTools）带来显著且一致的提升。

## 摘要

> 论文指出，MLLM 在真实世界视觉任务上常因输入本身信息不足而失败（密集小目标定位、长尾实体检索、中间测量计算等），单纯增加推理预算收益递减（把基础模型 effort 从 xhigh 提到 max，模型 token 花费增至 2.7 倍，平均分仅提升 0.7 分）。为此 OpenVLHarness 构建通用工具使用框架：能力层把任务级工具接口与证据渲染统一起来，解决异构后端（分割掩码、深度图、检索网页等）调用约定与返回格式不一致问题；持久多模态记忆保存跨轮次工具产物（图像、网页、数据绑定），并通过 Python Coding Agent 支持对已存数据的计算。实验覆盖 22 个基准、8 个模型后端（grounded perception、视觉搜索与深度研究、通用 VQA 与幻觉、空间推理、视觉数学），OpenVLHarness 在几乎所有基准上超过 Base、OpenAI Operator-Tools（OT）、Visual Studio（VS）与 OctoTools 等基线，且推理 effort 的收益可进一步叠加；组件消融显示能力层与多模态记忆各有增量贡献；另有 harness 搜索与工具生成两种优化工作流，可在不更新 MLLM 权重的情况下按领域定制。

Q1: 这篇论文试图解决什么问题？

论文针对的核心问题是：多模态大语言模型（MLLM）在真实世界视觉任务中，仅凭输入图像与内部知识往往无法可靠完成任务——密集场景中需要定位大量微小目标、长尾实体需要外部知识检索、需要从中间测量/检测结果做数值计算；这类任务要求模型主动获取并组合多模态证据。论文先用 inference-time scaling 实验（图 1c）说明单纯增加推理 token 预算的边际收益递减：基础模型从 xhigh 到 max effort，模型 token 花费放大 2.7 倍，平均分仅提升 0.7 分（这一数字来自摘要与正文引言，证据充分），由此论证更关键的方向是让模型能够访问任务相关的多模态证据，而不是只对初始输入做更多推理。在此基础上，构建通用 harness 面临两个相互关联的挑战：（1）异构工具后端调用约定不同、返回格式各异（分割掩码、深度图、检索到的网页等），需要统一表达为模型可理解的证据；（2）后续推理步骤仍需要访问先前工具调用产生的中间产物，涉及跨轮次的状态保持。此外还存在若干次级问题：如何让模型在长轨迹中不丢失工具输出（记忆机制）、如何在检测结果上继续做数值计算（把检测结果作为数据绑定传给代码执行器）、如何高效探索工具组合（harness 搜索）与按目标任务定制（工具生成）、以及如何避免用错误/无关的中间证据误导最终答案（如例 4 中 OCR 读到无关标牌但不被带偏）。论文把这些归并为“能力层 + 持久多模态记忆”两个设计杠杆，而不是单独改进某个工具或某个模型。

Q2: 有哪些相关研究？

论文把相关工作放在三条线上。第一条是 MLLM 作为推理引擎（reasoning engines over images, documents, webpages, videos）以及 inference-time scaling 的收益边界；论文自己的图 1c 实验（effort 从 xhigh 到 max，token 2.7 倍、分数 +0.7）直接与这条线对话。第二条是视觉工具使用智能体（visual tool-use agents）：论文引用了 Hong et al. 2026a（对应 RealX-Bench 与相关 agent 工作）、Wu et al. 2025a（对应 FVQA 与相关 agent 工作）、Narayan et al. 2025、Geng et al. 2026（对应 BrowseComp-VL）、Su et al. 2025、Wu et al. 2025b、Surís et al. 2023、Gupta & Kembhavi 2023 等；这些工作把 MLLM 连接到感知模型、搜索引擎和代码执行。第三条是通用工具编排/训练无关基线：实验结果中出现的 OT（OpenAI Operator-Tools）、VS（Visual Studio，指微软的 Visual Studio 工具链工作，证据片段中 'Base OT VS CX Ours' 与 'Base OT VS Ours' 出现）、OctoTools（文中称其为 strongest training-free baseline，平均 31.8）以及 CX（可能为某工具链，证据不足，需回原文确认）代表这一方向。论文的定位是：现有视觉 agent 工作多针对特定任务/单一工具链，缺少一个同时解决“异构后端调用约定 + 跨轮次多模态证据持久化”的通用 harness；与 OctoTools 等训练无关编排框架相比，OpenVLHarness 的特点在于证据渲染（把掩码、深度图、网页转成模型可读形式）与多模态记忆（跨轮次复用、数据绑定 + Coding Agent 计算）。需要说明：由于检索证据只覆盖部分文本，文献综述的完整范围（如是否讨论 ReAct、SoTA、Set-of-Marks、视觉 grounding 等经典范式）无法从现有片段确认，相关叙述以论文明确引用的工作为准。

Q3: 论文如何解决这个问题？

OpenVLHarness 的解决方案包含两大核心机制与两个优化工作流。（1）能力层（Capability Layer）：在工具层之上建立“任务级工具接口 + 证据渲染”。任务级接口把异构后端的调用约定统一（例如分割模型、检测模型、OCR、搜索、代码执行各自的输入输出格式），证据渲染则把工具返回的原始产物（分割掩码、bbox 列表、深度图、检索网页、生成的图像）转成 MLLM 可以直接解读的文本/图像形式——例如把检测框编号后渲染成标注图像，模型在后续步骤中直接引用“tool generated image 3 / 7”等记忆条目，而不是面对未渲染的原始张量或 JSON。（2）持久多模态记忆（Persistent Multimodal Memory）：harness 维护跨轮次的记忆条目，包括工具生成的图像（tool generated image N）、网页 URL（webpage url N）与数据绑定（data bindings）；每个工具调用的输出会登记为新的记忆条目（如“registers 1 new webpage URL”“registers 3 new images in memory”），模型可以在后续回合中继续引用。关键设计是“数据绑定 + Python Coding Agent”：检测/分割结果以数据绑定的形式进入内存，模型需要做集合合并、计数、重叠判定等计算时，可以重写目标、派发代码执行器对已存数据做运算，而不必重新检测——例 1 中第一次代码运行返回“零匹配、并集为零”的错误结果，模型没有重检测，而是重述目标（按 box 重叠或中心距离匹配、不做掩码解码），第二次运行找出 5 个漏检的圆形药丸，最终答出 134+5=139。记忆的更新通过“环境更新”（Environment Update）机制在每个工具调用后同步给模型，并区分“不是新问题”的环境更新与正式提问，避免模型把记忆更新误当成新指令。（3）两个优化工作流：harness 搜索（在给定领域上搜索更优的工具组合/编排策略）与工具生成（按目标任务生成/定制工具），两者都不更新底层 MLLM 权重，用于把系统 specialize 到选定领域。推理时层面，harness 与模型 effort 设置正交，论文在不同 effort（low/medium/max）下都能观察到 harness 增益，说明工具带来的证据增益可以叠加在推理算力之上而非替代它。

Q4: 论文做了哪些实验？

论文在四个任务域、22 个数据集、8 个模型后端上评测（表 1 汇总，split/规模/打分细节在 Appendix D）。四个任务域及数据集如下。（1）Grounded perception（grounded 感知）：CountQA（Tamarapalli et al., 2025）、FSC-147（Ranjan et al., 2021）、OdinW-13-All（Li et al., 2022）、FSCD-147 与 FSCD-LVIS（Nguyen et al., 2022），目标是物体计数与开放词汇视觉 grounding。（2）Visual search and deep research（视觉搜索与深度研究）：从 11 个多模态信息寻求任务中取 6 个——FVQA（Wu et al., 2025a）、MMSearch（Jiang et al., 2024）、MMSearch-Plus（Tao et al., 2026）、HLE-VL（Phan et al., 2025）、BrowseComp-VL（Geng et al., 2026）、RealX-Bench（Hong et al., 2026a），其余 5 个在 Appendix B.5 评测。（3）General VQA and hallucination（通用 VQA 与幻觉鲁棒性）：RealWorldQA（xAI, 2024）、SimpleVQA（Cheng et al., 2025）等 6 个基准（检索片段只列全前两个，其余需查原文）。（4）Spatial reasoning 与 visual math（空间推理与视觉数学）也在评测范围内（结论与结果片段提到 spatial reasoning；表 1 中出现 CountQA/FSC/OdinW 等数字）。模型后端：Qwen3-VL-8B、Qwen3-VL-32B、GPT-6 Luna（medium）、GPT-6 Astra（medium）、GPT-5、GPT-6 Sol 等共 8 个（片段中至少见到 6 个名字；论文摘要说 eight backbones）。基线：每个模型未加工具的 Base、OT（OpenAI Operator-Tools）、VS（Visual Studio）、CX（出现于表头 'Base OT VS CX Ours'，指代不明，需回原文确认）、OctoTools（训练无关强基线）以及 OpenVLHarness 的 low/medium/max effort 设置。评测协议：表 1 报告了各域各数据集的分数；另外做了推理 effort 与 harness 的交叉实验（low/medium/max），例如视觉搜索域在 max effort 下领先最强基线 15.6 和 12.4 分（片段原文），以及低 effort 下 OpenVLHarness 在通用 VQA/幻觉与空间推理上略低于 Base（61.5 vs. 某个数值；片段截断）。组件消融：分别去掉能力层与多模态记忆，验证各自增量。优化工作流实验：harness 搜索与工具生成在选定领域上的效果。此外附录包含大量轨迹样例（Example 1 到 Example 7+）：计药丸（SAM 3 检测 + 代码合并）、PascalVOC 密集人员检测（OdinW-13-All 子集）、巴黎时装周街拍溯源（MMSearch-Plus 风格）、大萧条事件年代差（MMSearch-Plus，Qwen3-VL-32B，中文推理）等，这些样例展示了反图搜失败转文字搜索、缩放 + OCR 读取无关标牌不被误导、单页服务器错误后恢复等失败恢复行为。

Q5: 发现了什么实验现象？

从检索片段与摘要能确认的实验现象如下。（1）推理预算收益递减：基础模型 effort 从 xhigh 到 max，token 花费 2.7 倍、平均分仅 +0.7（图 1c）——这是论文动机的核心测量。（2）harness 增益与 effort 正交且可叠加：在视觉搜索与深度研究域，OpenVLHarness 在 low effort 下就超过其他方法的所有 effort 设置，max effort 时领先最强基线 15.6 与 12.4 分；在通用 VQA/幻觉与空间推理域，harness 从更多推理中受益——low effort 时 OpenVLHarness 略低于 Base（61.5 vs. 片段截断的数值），说明这些任务上证据获取的即时收益小于纯推理，但提高 effort 后 harness 反超。（3）训练无关强基线对比：在某个聚合平均上（片段未指明具体数据集组合），Qwen3-VL-8B 从 34.2 提升到 45.4（+11.2），超过 OctoTools（31.8）13.6 分；Qwen3-VL-32B 从 38.3 到 50.9（+12.6）；GPT-5 从 44.4 到 55.7；GPT-6 Sol 从 52.7 起（截断）。这显示训练无关 harness 的收益跨模型规模与厂商一致。（4）计数类任务的典型模式：例 1 中 SAM 3 用 "pill" 查到 134 个、用 "round pill" 查到 72 个，两者重叠且都不是答案；模型派代码合并，第一次运行返回零匹配/零并集（明显错误，因为两个查询显然有重叠），模型重述目标（box 重叠或中心距离匹配、不做掩码解码）后第二次运行找到 5 个漏检，最终 134+5=139 精确命中；无工具时模型答 137。（5）恢复行为：例 5 中一次网页访问遇到服务器错误（HTTP 500），agent 用文本搜索恢复并给出正确答案 9 年；例 4 中反图搜失败时 agent 把照片转成文字再搜，定位到确切街拍图库；缩放 + OCR 读到无关标牌但未被误导。（6）密集场景检测：例 2 中 PascalVOC 二十类检测，人员重叠严重、有人只露出半张脸，agent 对拥挤桌面缩放并发起更窄查询，最终用 referring expression 找到半隐藏的人，22/23 命中。这些现象共同说明：失败模式主要是工具输出与模型目标之间的“语义-数值错配”（检测集合需要后处理合并、计数），以及单次工具调用失败时的路径恢复；而设计上通过内存数据绑定 + 代码执行 + 环境更新来应对。

Q6: 有什么可以进一步探索的点？

论文直接可延伸的方向：（1）把能力层扩展到更多异构工具（如视频理解、3D/深度估计、音频、物理仿真、数据库/表格工具），验证证据渲染范式是否继续成立；（2）把持久多模态记忆升级为可学习机制——目前记忆是 harness 外部维护的显式条目，可探索让模型自己决定记忆的写入/遗忘/压缩（记忆摘要），或与检索增强记忆（memory retrieval over long trajectories）结合；（3）harness 搜索与工具生成目前是不更新权重的领域特化手段，可探索与轻量微调/适配器结合，或在更大工具空间上做自动组合搜索；（4）推理 effort 与工具使用的联合优化：论文已经显示两者正交可叠加，但最优 effort-工具预算分配（在 token 成本约束下）尚未被建模，可做成本-收益曲线；（5）数据绑定 + Coding Agent 的计算模式值得推广到更复杂推理（多集合运算、几何计算、统计验证），并可研究绑定表示（mask vs bbox vs 点）对计算成功率的影响；（6）失败恢复策略的通用化：例 5 展示单次工具失败后的文本搜索回退，可系统化 fail-over 策略与重试预算；（7）幻觉与证据归因：在通用 VQA/幻觉域，harness 低 effort 时略低于 Base，说明工具引入的证据并不总是即时有用，可研究何时该用工具（tool-use gating/预算控制）；（8）评测协议层面，论文开放了 harness、工具、评测协议与优化工作流，可推动统一的多模态 agentic tool-use benchmark 标准（类似 WebArena 之于网页 agent）；（9）跨语言推理（例 5 全程中文推理）与多语言搜索能力值得单独评测。

Q7: 总结一下论文的主要内容

论文的论证主线：真实世界视觉任务需要的证据常常不在输入图像里，而在感知模型的输出、外部网页和中间计算中；推理算力投入再多也有边际收益递减（xhigh→max：token 2.7 倍、分数 +0.7），因此下一步进展应来自“让模型获取并组合多模态证据”，即视觉 agentic tool use。技术主线：为解决工具异构与跨轮次状态两大难题，OpenVLHarness 提出两层设计——能力层（Capability Layer）统一任务级工具接口并用证据渲染把掩码/检测/深度/网页变成模型可读形式；持久多模态记忆（Persistent Multimodal Memory）跨轮次保存工具生成图像、网页 URL 与数据绑定，并提供环境更新机制同步记忆状态；数据绑定 + Python Coding Agent 让模型对已存检测结果做集合运算与计数，失败时只需重述目标而非重新检测。实验主线：22 个基准、8 个后端、4 个任务域（grounded perception：CountQA/FSC-147/OdinW-13-All/FSCD-147/FSCD-LVIS；视觉搜索与深度研究：FVQA/MMSearch/MMSearch-Plus/HLE-VL/BrowseComp-VL/RealX-Bench 等 6+5 个；通用 VQA 与幻觉：RealWorldQA/SimpleVQA 等 6 个；另有空间推理与视觉数学），对比 Base、OT、VS、CX、OctoTools 等基线。主要结果：训练无关聚合平均上 Qwen3-VL-8B 34.2→45.4（+11.2，超 OctoTools 13.6 分）、Qwen3-VL-32B 38.3→50.9（+12.6）、GPT-5 44.4→55.7；视觉搜索域 max effort 领先最强基线 15.6/12.4 分；组件消融确认能力层与多模态记忆各自贡献；harness 搜索与工具生成提供不更新权重的领域特化途径。轨迹样例进一步展示了具体机制：SAM 3 药丸计数 134+5=139（无工具 137）、PascalVOC 密集人员 22/23、巴黎时装周街拍溯源、大萧条事件年代差 9 年（中文推理 + 服务器错误恢复）。最终论文开源 harness、工具、评测协议与优化工作流。局限与边界：通用 VQA/幻觉与空间推理在低 effort 时 harness 略低于 Base，说明工具证据在这些任务上不是无条件增益；例 1 第一次代码运行失败说明“数据绑定 + 代码计算”路径仍依赖模型对目标的重新表述能力；异构工具的完整列表、OT/VS/CX 的确切定义与剩余 5 个搜索基准、通用 VQA 6 个基准中的另外 4 个、8 个后端中未在检索片段出现的名字，需回原文（表 1、Appendix B/D）确认。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体（agent, 0.10）方向直接重合：论文是视觉 agentic tool-use 的通用 harness 工作，其能力层 + 多模态记忆架构、环境更新协议、失败恢复轨迹都可作为 agent 系统设计的对照案例。

## 基本信息

- 作者：Yu Zhou, Cheng-Fu Yang, Rui Sun, Di Wu, Sibo Peng, Mingyang Zhang, Zhen Yang, Ruiyi Zhang, Nanyun Peng, Zhe Gan, Kai-Wei Chang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CV, cs.LG, cs.MM
- 日期：2026-10-19
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.26860v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了 PDF 语义检索证据（abstract/introduction/method/results/conclusion 多个片段）并结合启发式草稿补全；论文元数据中的 abstract 字段为抓取噪声，已忽略，改用检索到的正文片段归纳。
