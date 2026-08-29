---
user_id: "cheng tan"
paper_id: 9166
arxiv_id: "2608.23283v2"
title: "Apodex 1.1: Scaling Agentic Intelligence for Complex Work"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23283v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23283v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:09:28"
---
# Apodex 1.1: Scaling Agentic Intelligence for Complex Work

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agentic intelligence · working capability · long-horizon task · environment scaling

## 一句话总结

Apodex 1.1 提出并实践'工作能力（working capability）'这一模型级智能衡量标准——在真实目标上持续、可验证地推进——通过环境扩展（Environment Scaling）与智能体协调扩展（Agentic Coordination Scaling）两条互补路径，将长程任务分解、工具/文件/代码交互、失败恢复和可验证交付训练进统一策略与执行栈，在复杂专业工作、金融、科研、数学、编码和搜索任务上以显著小于前沿模型的规模进入领先性能区间，并提供 35B 参数的本地可部署版本 Apodex 1.1 Mini。

## 摘要

> General-purpose language models can reason and synthesize knowledge, but complex work also requires sustained interaction with files, information sources, and executable code, together with state maintenance, failure recovery, and verifiable delivery. We call this working capability: sustained, verifiable progress toward a real-world objective. Apodex 1.1 develops this capability along two complementary dimensions. Environment Scaling expands the diversity and verifiability of executable file, search, and code environments, while Agentic Coordination Scaling trains agents to decompose long-horizon tasks, delegate parallel work, integrate asynchronous results, and replan. A shared execution harness and AgentOS maintain task state and provenance across tools and agents, and training turns environment trajectories and coordination traces into reliable behavior. Across complex professional work, finance, scientific research, mathematics, coding, and search, Apodex 1.1 reaches the leading performance band despite using a substantially smaller model than many frontier systems. The 35B-parameter Apodex 1.1 Mini further retains strong working capability in a locally deployable form. These results ground agentic intelligence in useful, verifiable work completed over time and advance our goal of building a Heavy-Duty Solver for ambitious, long-running tasks.
> ![](images/bd0865774c2c47ec2c1132d12b1de9625454dc40b842a529c8ef3b33d4b739ca.jpg)
> ![](images/0e26f435c4e1d0d714e29a9d5bc3eaddf5b360d50a069477d78334de7e437857.jpg)
> ![](images/0930d909cc64723c89b2e5efce0839e5e84bc9c1d49ac7f6c78c5616507d96b3.jpg)
> FrontierScience-Research
> Scientific Research
> ![](images/01d0143dec748e3e58d5b21e238a1f508de701082617ddda1c88c7c959d9bbc3.jpg)
> BioMysteryBench
> Scientific Research / Human-difficult
> ![](images/7c4a8e065f35f3be45c382aa4082cd7ac4069dd2582def08677b6c8bb41cc55e.jpg)
> ![](images/77c261cca9f480c9cdba161e5c5c193bc62abadcd8738bdcb56dbdc82caba7e6.jpg)
> Figure 1: Apodex 1.1 reaches the leading performance band across professional work, finance, scientific research, and general reasoning.

Q1: 这篇论文试图解决什么问题？

1. 核心矛盾：能说对不等于能做成。论文引言明确指出，通用语言模型在知识、推理、数学、编码上进步迅速，但许多有价值任务即使模型能给出正确答案仍然困难，因为工作是在很长时间跨度中展开的——需要与文件、信息源和可执行代码持续交互，需要维护状态、从失败中恢复，并交付可验证的结果。2. 定义缺口：现有评测大多测量'静态正确率'，缺少对'工作能力'的模型级衡量标准。论文主张能力应被定义为：在一个不断变化的任务上持续取得进展、有效使用工具与证据、从失败中恢复并完成交付。3. 两个技术子问题：其一是环境扩展——如何扩大可执行文件、搜索、代码环境的多样性与可验证性，让模型有足够多、足够可信的交互空间；其二是智能体协调扩展——模型如何学会分解长时程任务、委派并行工作、整合异步结果、在计划失效时重新规划。4. 状态与溯源问题：多工具、多智能体协作下，任务状态和操作溯源（provenance）怎么跨组件维护，是基础设施层面的难点。5. 证据缺口说明：本次检索只覆盖摘要、引言、结论等碎片，问题陈述的框架清楚，但论文没有给出具体的失败案例数据或量化瓶颈分析，'长时程任务到底难在哪一步'的详细证据需回原文确认。

Q2: 有哪些相关研究？

1. 证据覆盖不足：检索片段没有命中论文的 Related Work 章节，因此以下定位属于'基于片段线索 + 领域常识的合理推断'，引用关系需以原文为准。2. 工具使用与智能体循环：Apodex 1.1 的'推理—搜索—文件操作—代码执行—失败恢复'栈属于语言模型智能体（LLM agent）路线，延续 ReAct 式思考-行动循环、function calling、代码解释器等范式；Environment Scaling 可视为对这类工具环境多样性和可验证性的系统化扩展。3. 长程任务与评测基准：全文中直接命中的'Humanity's Last Exam（General Reasoning and Search）'说明论文使用了该基准的通用推理与搜索类别；图 1 还涉及 FrontierScience-Research 与 BioMysteryBench（科学研究/人类难度），表明其评测与 AI for Science 基准线（生物谜题、科研任务）有关。4. 多智能体编排：Agentic Coordination Scaling（分解、委派、异步整合、重规划）与多智能体编排框架（如 AutoGen、MetaGPT 一类的并行与协作范式）在问题上有亲缘性，但本文路线是把协调行为训练进策略，而非纯提示编排——这一差别是合理推断，需原文确认。5. Agent 操作系统：AgentOS 与共享 execution harness 维护跨工具、跨智能体的状态与溯源，属于'智能体操作系统/执行基础设施'这一新兴方向的范畴。6. 轨迹训练：'把环境轨迹和协调痕迹转化为可靠行为'接近基于交互轨迹的强化学习/离线 RL 思路，与基于过程奖励、结果奖励的推理模型训练范式相关，但具体算法与数据配方在可见证据中没有出现。

Q3: 论文如何解决这个问题？

1. 统一策略与执行栈：从证据片段看，Apodex 1.1 被组织为'用于长时程工作的公共策略与执行栈'，模型统一学习推理、搜索、文件操纵、代码执行、从失败动作中恢复、协调并行工作并交付可验证结果——这暗示训练目标不是单点工具调用，而是在一个环境-执行闭环中的完整行为策略。2. 环境扩展（Environment Scaling）：扩大可执行文件、搜索、代码环境的多样性与可验证性；'可验证性'是关键约束，意味着环境需要有明确的成功判据，为训练和评测提供可靠信号。3. 智能体协调扩展（Agentic Coordination Scaling）：训练智能体分解长时程任务、委派并行工作、整合异步结果、在情况变化时重新规划；这表明多智能体/多任务并发不是靠外部编排器，而是作为模型能力被学习。4. 基础设施：共享 execution harness 与 AgentOS 跨工具和智能体维护任务状态与溯源（provenance），解决'做到哪一步、为什么这样做'的可追溯问题。5. 训练信号：把环境轨迹与协调痕迹转化为可靠行为——合理推断其包含对轨迹数据的模仿/强化学习，但具体算法、奖励设计、数据规模在检索片段中缺失。6. 模型级视角：方法论的落点是'能力应通过随时间推进真实目标来度量'，把智能体智能锚定在有用的、可验证的工作完成上，而不是静态问答或单步工具调用。

Q4: 论文做了哪些实验？

1. 评测域：复杂专业工作、金融、科学研究、数学、编码、搜索——覆盖六个任务族，强调真实工作场景而非纯学术 benchmark。2. 可见基准：检索命中'Humanity's Last Exam（General Reasoning and Search）'，说明使用了该基准的通用推理与搜索类别；图 1 显示 FrontierScience-Research（科研）与 BioMysteryBench（科研/人类难度）两个科研向基准；图题文字'Apodex 1.1 reaches the…'表明图 1 是性能对比图，但具体曲线和对比对象被截断。3. 模型配置：Apodex 1.1（完整版，参数规模在可见片段中未给出，仅表述为'显著小于许多前沿系统'）与 Apodex 1.1 Mini（35B 参数，面向本地部署）。4. 证据缺口：本次检索没有命中任何实验表格、具体分数、baseline 名单、消融实验、扩展（scaling）曲线、训练数据量或计算量数据；这些是本报告无法补全的关键信息，必须回原文核对，不应被推断替代。5. 测试设置推断：从'35B Mini 本地可部署'可合理推断论文还做了部署形态对比或至少是能力保留评估，但具体协议未见。

Q5: 发现了什么实验现象？

1. 核心现象——规模与性能解耦：Apodex 1.1 在'显著小于前沿系统'的模型规模下进入领先性能区间；这说明在 agentic 工作能力上，环境多样性与协调训练可以在一定程度上替代原始参数规模，是本报告可确认的最有信息量的观察（基于摘要明确表述）。2. 科研任务表现：图 1 将 FrontierScience-Research 与 BioMysteryBench（标注为'科学研究/人类难度'）列为展示项，合理推断科研类长程任务是该模型的优势场景，且这类'人类难度'任务的命中意味着工作能力训练可能覆盖开放的、需要证据综合的科研流程。3. 小模型能力保留：35B 的 Mini 版本'保留强工作能力'，推测工作能力是一种可蒸馏/可压缩的策略行为，而非仅在超大模型上涌现的性质——该解释属推测，论文未给出蒸馏细节。4. 未见内容：当前证据不包含消融趋势、scaling trend、负结果、失败案例或指标间张力（如正确率 vs 完成率、速度 vs 可靠性），不能凭空补充；这些是阅读原文时最值得追踪的观察点。

Q6: 有什么可以进一步探索的点？

1. 评测学方向：把'工作能力'操作化为可复现的度量体系——如何量化'持续进展''失败恢复''可验证交付'，以及如何设计比静态 benchmark 更能反映长时程工作质量的评测协议，是本文框架自然延伸的开放问题。2. 环境扩展的边界：从受控的文件/搜索/代码环境走向真实软件仓库、真实科研实验协议、开放网络；环境越开放，verifiability 定义越难，是值得探索的难点。3. 协调扩展的深化：层级式委派、多智能体竞争/协作混合拓扑、异步结果的一致性仲裁、重规划触发条件的形式化。4. 训练配方消融：环境轨迹信号与协调痕迹信号各自对最终能力的贡献占比、RL 与离线模仿的配比、奖励信号的可验证性如何影响策略质量——这与用户偏好的系统性工作方式高度契合。5. 状态与溯源的形式化：AgentOS 的状态抽象、跨会话恢复、provenance 的可审计性，以及失败后回滚策略。6. 小模型路线：35B Mini 的训练/蒸馏方法、本地部署带来的隐私与成本优势、小模型在长时程任务上的能力上限。7. AI for Science 落地：结合 BioMysteryBench 等科研基准，把'工作能力'用于生物/科学发现管线（假设生成、实验设计、结果验证闭环）。8. 说明：以上方向是基于论文问题框架的合理推断与延伸，不是论文明确承诺的未来工作。

Q7: 总结一下论文的主要内容

Apodex 1.1 是一份以'智能体智能的规模化'为主题的技术报告，核心论点是：语言模型在知识、推理、数学、编码上的快速进步，并不等于能完成复杂工作。许多任务即使模型能说出正确答案，依然因为工作过程在长时间跨度中展开而难以完成——它要求与文件、信息源和可执行代码持续交互，要求维护状态、从失败中恢复、并交付可验证的结果。作者把这种能力命名为'工作能力（working capability）'，即在一个真实世界目标上持续、可验证地取得进展，并主张以模型级视角来衡量智能体智能：能力应被度量为一个模型能否在不断变化的任务中持续进步、有效使用工具与证据、从失败中恢复并完成交付。技术路线上，论文沿两条互补维度扩展能力：其一是环境扩展（Environment Scaling），扩大可执行文件、搜索、代码环境的多样性与可验证性，为模型提供更丰富且成功判据可信的交互空间；其二是智能体协调扩展（Agentic Coordination Scaling），训练智能体学会分解长时程任务、委派并行工作、整合异步结果并在必要时重新规划。基础设施层面，一个共享 execution harness 与 AgentOS 跨工具和跨智能体维护任务状态与溯源（provenance），保证长时程工作的可跟踪性和可恢复性；训练环节把环境轨迹与协调痕迹转化为可靠行为，使上述能力不再依赖外部编排脚本，而是内化为模型策略。评估覆盖复杂专业工作、金融、科学研究、数学、编码和搜索，可见证据表明论文使用了 Humanity's Last Exam（通用推理与搜索类别）、FrontierScience-Research 与 BioMysteryBench（科学研究/人类难度）等基准，并报告 Apodex 1.1 在显著小于许多前沿系统的模型规模下达到领先性能区间；35B 参数的 Apodex 1.1 Mini 进一步在本地可部署形态下保留较强工作能力。结论部分把这项工作定位为迈向 Heavy-Duty Solver（面向雄心勃勃、长时间运行任务的求解器）的一步，并重申模型级智能观。需要强调，本次分析基于摘要、引言、结论与个别章节碎片，论文中的具体分数、baseline、训练细节、消融和失败案例分析均未在检索证据中出现，需阅读完整原文核实。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：主题与你画像中的 agent 方向（权重 0.10）直接重合：论文主张把智能从'推理能力'转向'可验证的持续工作能力'，是智能体研究的模型级路线，值得作为你判断该方向技术趋势的参考样本。

## 基本信息

- 作者：B. An, B. Li, B. Wang, B. Zhang, B. L. Wang, C. Feng, C. Wei, C. Xue, C. Zhang, D. Ng, D. Ye, E. Min, F. Chen, F. Liu, F. Yang, F. Ye, G. Sun, H. Ji, H. Xu, H. Yang, H. Ye, H. Zhang, H. Zhao, J. Li, J. Lin, J. Xia, K. Jin, K. Wang, K. Yang, L. Bing, L. Lei, L. Su, Le. Wang, Lu. Wang, N. Wang, Q. Ren, Q. Yang, R. Li, S. Bai, S. Du, S. Li, S. Lin, S. Nie, S. Wang, S. Zhang, S. Z. Wang, T. Ge, Ta. Q. Fang, Ti. Q. Fang, W. Fang, W. Li, W. Zhang, X. Chen, X. Li, X. Tang, X. Wang, X. Xu, X. Zhang, X. Q. Wang, X. Y. Wang, Y. Deng, Y. Gao, Y. Hu, Y. Li, Y. Sui, Y. Wang, Y. Xiao, Y. Zhang, Y. Zhou, Z. Chen, Z. Cheng, Z. Feng, Z. Liang, Z. Liu, Z. Zhang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23283v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成仅参考了少量 PDF 语义检索命中片段（Abstract、Introduction、Conclusion、Apodex 1.1 章节碎片）与 arXiv 元数据，未见完整 PDF 正文，具体实验数值、baseline、训练细节与作者单位均缺失，相关推断已在文中标注。
