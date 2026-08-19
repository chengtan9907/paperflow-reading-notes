---
user_id: "cheng tan"
paper_id: 7993
arxiv_id: "2608.13606"
title: "MobileMem: Learning from a Year of Mobile Experiences"
institution: "浙江大学（ZJU）、OPPO、OpenKG等（依据GitHub仓库zjunlp和论文标注“OPPO, OpenKG”合理推断；具体作者单位列表需阅读原文确认）"
publish_date: "2026-08-17"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.13606.pdf"
pdf_url: "https://arxiv.org/pdf/2608.13606"
abs_url: "https://arxiv.org/abs/2608.13606"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-18T02:15:50"
---
# MobileMem: Learning from a Year of Mobile Experiences

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：long-term memory · mobile agent · benchmark · knowledge-grounded synthesis

## 一句话总结

论文提出MobileMem，一个面向设备端长期记忆的基准与框架，基于一年规模移动体验的知识引导合成管线，提供文本与多模态两种场景，用于评估智能体在长期时间推理、知识更新和隐式偏好推断等核心记忆能力，推动记忆从信息检索走向经验智能。

## 摘要

> The next generation of AI agents is increasingly moving beyond systems that answer isolated questions toward persistent personal assistants that can understand, remember, and continuously learn from users' experiences. Such assistants require long-term memory to accumulate and leverage user-specific experiences over time, yet existing benchmarks remain inadequate for realistic mobile settings, where experiences are heterogeneous, multimodal, evolving, and deeply personal. We introduce MobileMem, a benchmark and framework for studying on-device long-term memory, grounded in a year-scale collection of mobile experiences. MobileMem employs a knowledge-grounded synthesis pipeline to construct coherent and temporally consistent long-horizon trajectories from user-app sessions. It provides complementary text and multimodal settings covering multi-hop and temporal reasoning, knowledge updating, and implicit preference inference. Specifically, MobileMem enables agents to remember the past, understand the present, and adapt to the future. By modeling experiences rather than isolated facts, MobileMem moves memory beyond information retrieval toward experiential intelligence for continuous personal learning.

Q1: 这篇论文试图解决什么问题？

这篇论文聚焦的核心问题是：现有AI代理大多停留在单轮问答层面，而下一代个人助理需要在长时间跨度内持续理解、记忆和适应用户，因此长期记忆成为关键瓶颈。具体而言，当前研究面临四方面的缺口：第一，真实移动体验具有异构性（消息、日程、照片、应用行为等混合）、多模态性（文本、图像、界面元素并存）、时间演化性（用户偏好和习惯随时间变化）以及强个人化（与具体用户绑定），而现有记忆基准往往基于静态文档或人工构造的问答对，难以体现这些特征。第二，真实移动数据涉及严重隐私问题，难以大规模采集、共享和公开，也限制了端侧记忆系统的研究。第三，现有评估大多孤立测试单次记忆检索，缺乏对多跳时间推理、跨源信息融合、知识更新和隐式偏好推断等复合能力的系统性覆盖。第四，设备端部署还面临模型规模、延迟、隐私保护等工程约束，需要一个既能公平比较各类记忆组件（检索算法、RAG管线、记忆中间件、长上下文推理、智能体执行）又能反映真实移动场景的统一协议。MobileMem正是为了弥补这一空白而提出，通过知识引导合成管线在隐私安全前提下生成长期交互轨迹，并据此构建可重复、可扩展的基准与评估框架。

Q2: 有哪些相关研究？

根据论文引用片段（涉及Qwen-image技术报告等）和该领域的一般脉络，相关研究可从以下维度梳理，具体对应关系需以原文Related Work为准：（1）智能体记忆架构：MemGPT通过上下文层次化和外部记忆扩展LLM上下文，Generative Agents利用记忆流模拟人类式行为，近期还有记忆压缩、记忆合并、分层记忆等方向。（2）检索增强生成（RAG）：将外部知识库与LLM结合，从向量检索、重排到混合检索，RAG已成为长期记忆场景中事实性回答的常用方案。（3）长期对话与个人助理：例如ChatGPT的记忆功能、Gemini的个性化助手等商业系统，但大多封闭、缺乏公开基准。（4）移动端AI与端侧智能：包括屏幕理解、app操作自动化、端侧小模型等，但通常聚焦会话级任务而非跨时间记忆。（5）多模态大模型：Qwen-VL、GPT-4V等为理解屏幕截图、图像等多模态输入提供基础，但将其用于长期记忆评估的公开研究仍少。（6）时间推理与知识更新：包括TimeQA等时间敏感问答数据集、模型编辑与动态检索等技术。MobileMem的独特之处在于把上述要素整合进以“用户体验”为基本单位、时间一致且隐私友好的长时程合成数据基准中，并统一了不同记忆模块的评估协议，这一角度在现有公开基准中较为少见。

Q3: 论文如何解决这个问题？

MobileMem的方法体系包含三层核心设计。第一层是知识引导的长期数据合成管线：不直接采集真实用户原始数据，而是从用户先验知识（如人口属性、兴趣偏好、常用应用、日程安排等种子信息）出发，在保护隐私的前提下生成覆盖一年时间尺度的交互轨迹。该管线强调轨迹在事件层面和偏好层面的时间一致性与演化性，例如用户兴趣变化、习惯形成、关系发展等，从而比静态问答对更接近真实体验。第二层是互补的文本与多模态场景实例化：文本场景（推测为MobileMem-TEXT）围绕消息、备忘录、日程等文本应用会话；多模态场景（MobileMem-OMNI）融合真实用户参与、合成隐私图像和双语设置，并生成跨事件、跨时间的多跳问答对，要求模型在视觉与文本信息之间进行联合推理。任务类型涵盖多跳推理、时间推理、知识更新（判断新旧信息矛盾并正确整合）和隐式偏好推断（从行为模式而非明示语句中推测兴趣）。第三层是统一的评估框架：MobileMem将记忆系统解耦为可替换组件——检索算法、RAG管线、记忆中间件、长上下文推理模块和智能体执行策略，并提供统一评估协议，支持在不同记忆架构和检索策略之间进行可复现的比较。评估重点落在长期时间推理、跨源信息融合、复杂记忆组织和个性化摘要生成等核心能力维度上，以此形成对“经验智能”而非单纯信息检索的度量。

Q4: 论文做了哪些实验？

论文构建的实验载体包括两个互补的基准场景：文本场景与多模态场景（后者在检索证据中被称作MOBILEMEM-OMNI）。文献中提供了对话级示例（Figure 10）来展示用户交互和多模态数据广度，同时声明评估协议旨在覆盖不同记忆架构与检索策略的定量比较。但本次检索到的证据片段没有包含具体实验设置（如baseline模型、评估指标、数据规模、训练/测试划分）或定量结果数值。因此，详细的实验设计、对比方法和结果数据需要回到PDF原文的Experiments和Results部分确认。合理推测论文会评估若干代表性基线（如向量检索、RAG、长上下文模型、记忆中间件等）在这两个场景上的表现，但具体的数字和消融设计暂无法从现有材料中核实。

Q5: 发现了什么实验现象？

当前检索证据中缺乏可用的实验观察结果。可以合理推断，论文的实验部分通常会报告不同记忆系统在长期时间推理、跨源信息融合等任务上的性能差异，并可能展示诸如“长上下文化记忆在真实体验上难以胜过显式记忆维护”或“多模态场景下跨模态检索是主要瓶颈”之类的发现，但这些均属猜测。若原文包含实验现象、失败案例或指标间张力，需以论文Results和Discussion部分为准。这里不编造具体数据或趋势。

Q6: 有什么可以进一步探索的点？

基于论文引言、应用和局限性，可提炼以下可进一步探索的方向：（1）真实用户参与与隐私保护：论文已引入真实用户参与、合成隐私图像和双语设置，未来可进一步研究差分隐私、联邦学习等技术在端侧记忆构建中的有效结合。（2）多模态长期记忆：围绕视觉推理、跨模态检索和图文联合记忆组织，设计更细粒度的任务与指标。（3）记忆更新与遗忘机制：长期记忆中如何及时修正过期知识、消除矛盾，以及是否需要显式遗忘模型。（4）隐式偏好推断的深化：从行为日志推断偏好时，如何区分短期波动与长期稳定偏好，并用于个性化服务。（5）记忆压缩与抽象：一年规模的体验数据量庞大，如何通过摘要、分层存储、情景记忆与语义记忆分离等方式高效组织。（6）端侧部署与效率：量化评估不同记忆模块在计算、内存和延迟上的代价，推动轻量级端侧记忆系统。（7）统一评估协议的扩展：将MobileMem的评估框架扩展到更多设备类型、多语言和多文化场景，并建立与线下真实用户满意度的关联。（8）从记忆到行动：探索长期记忆如何指导智能体在开放世界中的决策规划，实现“记住过去→理解现在→适应未来”的闭环。

Q7: 总结一下论文的主要内容

MobileMem论文围绕AI代理从“孤立问答”向“持续个人助理”演进的趋势，提出一个系统性研究设备端长期记忆的基准与框架。论文首先指出现有记忆基准在移动场景中的不足：真实移动体验是异构、多模态、动态演化且高度个人化的，而多数基准依赖静态文本或人工构造的问答对，缺乏时间一致性和隐私可获取性。为应对这些挑战，MobileMem设计了知识引导的合成管线，从用户先验知识出发生成覆盖一年时间尺度的、时间连贯的交互轨迹，在隐私保护前提下提供可扩展的训练和评估数据。基于此，论文实例化了两个互补场景：文本场景聚焦消息、日程等应用会话；多模态场景（MOBILEMEM-OMNI）结合真实用户参与、合成隐私图像和双语数据，并生成多跳问答对，要求模型跨事件和时间进行推理。任务设计覆盖多跳推理、时间推理、知识更新和隐式偏好推断，几乎涵盖了长期记忆的核心认知能力。评估方面，MobileMem提出统一评估协议，将记忆系统拆分为检索算法、RAG管线、记忆中间件、长上下文推理和智能体执行等可插拔组件，便于公平比较不同架构与策略。论文强调其核心贡献不在于单一数据集，而在于确立一套以“体验”为单位、以“经验智能”为目标的评估范式，并定义了长期时间推理、跨源信息融合、复杂记忆组织和个性化摘要生成四个能力维度。这一工作有望推动记忆系统从简单的信息检索迈向持续的个性化学习与适应，为下一代移动智能体提供标准化的研究平台。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的“agent”方向高度相关，论文直接研究智能体长期记忆与个性化助手。

## 基本信息

- 作者：Xinle Deng, Yida Xue, Xiangyuan Ru, Haoming Xu, Shuofei Qiao, Mengru Wang, Yijun Chen, Buqiang Xu, Chen Jiang, Yuchen Eleanor Jiang, Lizhong Wang, Jianfeng Wang, Li Zeng, Haofen Wang, Guilin Qi, Huajun Chen, Ningyu Zhang
- 机构：浙江大学（ZJU）、OPPO、OpenKG等（依据GitHub仓库zjunlp和论文标注“OPPO, OpenKG”合理推断；具体作者单位列表需阅读原文确认）
- 来源：arxiv
- 主题/分类：cs.AI, cs.CL, cs.LG, cs.MA, cs.MM
- 日期：2026-08-17
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.13606`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF语义检索命中的证据片段（摘要、Introduction、Applications、Conclusion等），并结合元数据推断；具体实验数值、baseline和详细方法需回原文确认。
