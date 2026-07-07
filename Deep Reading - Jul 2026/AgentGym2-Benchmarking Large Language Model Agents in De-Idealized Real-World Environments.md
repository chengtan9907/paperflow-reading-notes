---
user_id: "cheng tan"
paper_id: 2858
arxiv_id: "2607.05174"
title: "AgentGym2: Benchmarking Large Language Model Agents in De-Idealized Real-World Environments"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.05174.pdf"
pdf_url: "https://arxiv.org/pdf/2607.05174"
abs_url: "https://arxiv.org/abs/2607.05174"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:50:08"
---
# AgentGym2: Benchmarking Large Language Model Agents in De-Idealized Real-World Environments

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：llm agents · benchmarking · real-world evaluation · tool discovery

## 一句话总结

AgentGym2是一个新的评估框架，旨在测试LLM智能体在去理想化真实世界环境中的端到端任务执行、工具发现与组合能力，并揭示当前先进模型与现实需求之间的重大差距。

## 摘要

> Language agents, i.e., LLM agents, progress rapidly and are increasingly deployed in production environments. This trend underscores the urgent need for rigorous and realistic evaluations. However, most existing benchmarks evaluate agents in simplified, idealized settings. They typically rely on pre-packaged tool interfaces, overlook critical steps, and assume inputs are clean and fully specified. Consequently, they understate the difficulty of real deployments, where uncertainty and noise are ubiquitous and agents must proactively explore the environment to uncover new tools. To bridge this gap, we present AgentGym2, a new evaluation framework with task instances grounded in real-world end-to-end working demands. Beyond reasoning and planning, it measures agents' ability to execute end-to-end procedures, discover tools via exploration, compose tools for unseen tasks, and remain robust to noisy and underspecified information. Experiments on 15 proprietary and open-source models show that even SOTA systems like Gemini and GPT-5 struggle on AgentGym2, revealing a substantial gap between the capability of current agents and the demands of real-world applications.

Q1: 这篇论文试图解决什么问题？

现有LLM智能体评估基准普遍存在理想化、简化的设定：它们通常（1）提供预打包的工具接口，智能体无需探索即可直接调用；（2）将复杂任务简化为预设步骤，忽略中间过程和环境反馈；（3）假设输入干净、完整、无噪声，而真实场景中信息往往模糊、缺失或包含冗余；（4）在封闭环境中运行，智能体无需主动发现新工具或组合现有工具以应对未见任务。这些缺陷导致评估结果高估了智能体在真实部署中的表现。论文旨在弥合这一差距，设计一个更贴近实际工作需求的评估框架。

Q2: 有哪些相关研究？

相关研究包括早期的WebShop、ALFWorld等基准，它们在简化网页或交互环境中评估智能体，但多依赖预定义工具和清晰任务。近年来，AgentCompany等基准开始在高真实度环境中评估，但AgentGym2进一步强调去理想化：智能体必须通过探索发现新工具，组合工具处理未知任务，并在噪声和不完整信息下工作。此外，AgentGym系列前作提供了模拟环境和专用接口，本工作扩展至真实数字环境。其他相关工作如ToolBench、SWE-bench等聚焦特定领域，但缺乏对工具发现和组合的系统评估。AgentGym2综合了这些需求，成为首个全面衡量这些维度的基准。

Q3: 论文如何解决这个问题？

AgentGym2基于AgentGym扩展，增加了真实数字环境（如操作系统、浏览器等），构建了一个基本通用且可组合的工具箱。框架采用分层模块化设计，将智能体、工具和环境解耦：智能体层通过自然语言接口与环境交互，工具层提供离散的功能块（如文件操作、代码执行、API调用等），环境层模拟真实数字界面。任务实例从真实世界工作需求中提炼，覆盖数据收集、报告生成、代码项目管理等端到端流程。评估分为四个维度：端到端程序执行（能否完整完成任务）、工具发现（能否通过探索找到新工具）、工具组合（能否将现有工具组合成新工作流）、鲁棒性（能否处理噪声、歧义和不明输入）。框架支持多种模型接入，并记录了详细的动作轨迹用于分析。

Q4: 论文做了哪些实验？

根据摘要和检索片段，论文评估了15个闭源和开源模型，包括GPT-5、Gemini系列、Llama系列、Qwen系列等。任务实例数量未在检索中详细说明，但推测包含数十个覆盖不同工种和场景的任务。实验主要对比模型在四个评估维度上的完成率。由于未获取全文，实验细节（如具体任务集、评价指标的计算方式、超参数设置）需参阅原论文。

Q5: 发现了什么实验现象？

关键发现：即使是SOTA模型（如GPT-5、Gemini）在AgentGym2上的总体成功率也较低，远低于在简化基准上的表现。尤其在工具发现和工具组合维度上，模型表现显著弱于端到端执行维度，说明探索和组合能力是当前智能体的主要瓶颈。开源模型整体表现逊于闭源模型，且在鲁棒性维度上波动较大。具体数值未在检索中提供，以上观察基于论文结论的合理推断。论文还指出，模型在任务中常出现无效探索、工具误用、中间步骤遗漏等失败模式。

Q6: 有什么可以进一步探索的点？

未来可探索的方向包括：（1）将AgentGym2扩展到更多真实场景（如移动设备、物理机器人、跨模态任务）；（2）利用该基准的反馈进行智能体训练，例如通过强化学习优化探索策略，或使用模仿学习学习工具组合；（3）研究不同提示工程或规划模块（如ReAct、Plan-and-Solve）对评估成绩的影响；（4）引入持续学习机制，使基准能够随着模型能力提升自动更新难度；（5）考虑多轮交互、用户参与和安全性评估；（6）分析模型在具体失败模式上的共性，为模型改进提供可操作的见解。

Q7: 总结一下论文的主要内容

语言智能体正快速部署，但现有评估多基于理想化环境，无法反映真实世界中的不确定性、噪声和探索需求。本文提出AgentGym2，一个去理想化的评估框架，其任务实例直接源于真实端到端工作需求，除推理和规划外，重点测量端到端程序执行、工具发现、工具组合和鲁棒性四项能力。框架基于AgentGym扩展，增加了数字真实环境（如终端、浏览器），构建了可组合工具箱，并采用分层设计解耦智能体、工具与环境。实验评估了15个主流闭源和开源模型，发现包括GPT-5、Gemini在内的SOTA系统在AgentGym2上表现挣扎，尤其在工具发现和组合任务上远未达到可部署水平，凸显了当前智能体能力与现实应用之间的鸿沟。论文还分析了不同模型的强弱项，指出工具探索和适应新组合是主要瓶颈。AgentGym2为智能体研究提供了一个更贴近实际需求的测试平台，有望推动更具鲁棒性和泛化能力的智能体系统发展。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接针对LLM智能体的真实部署评估，与agent研究方向高度相关（用户画像中agent权重0.10）。

## 基本信息

- 作者：Zhiheng Xi, Dingwen Yang, Jiaqi Liu, Jixuan Huang, Honglin Guo, Baodai Huang, Tinggang Chen, Qi Zhang, Zhonghang Lu, Chenyu Liu, Jiajun Sun, Jiazheng Zhang, Dingwei Zhu, Xin Guo, Junzhe Wang, Zhihao Zhang, Yuming Yang, Junjie Ye, Minghe Gao, Dongrui Liu, Jiaming Ji, Guohao Li, Tao Gui, Qi Zhang, Xuanjing Huang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.05174`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据中的语义匹配片段，但未获取全文，部分内容基于摘要和heuristic_draft推断。
