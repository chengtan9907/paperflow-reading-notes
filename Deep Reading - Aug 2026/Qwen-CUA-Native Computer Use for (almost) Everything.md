---
user_id: "cheng tan"
paper_id: 6207
arxiv_id: "2608.02352v1"
title: "Qwen-CUA: Native Computer Use for (almost) Everything"
publish_date: "2026-08-03"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.02352v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.02352v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-05T01:53:39"
---
# Qwen-CUA: Native Computer Use for (almost) Everything

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：computer use agent · native interaction · mixture-of-experts · visual grounding

## 一句话总结

Qwen-CUA 是一个基于 397B-A17B Qwen 混合专家架构的原生计算机使用智能体，仅通过截图观察与键盘鼠标动作操作软件，借助大规模可验证任务训练、长视界轨迹优化和迭代式数据刷新，在八个计算机使用基准上超过 Qwen3.7 并与领先专有系统竞争，并进一步推出超万亿参数的 Qwen-CUA-Max。

## 摘要

> Native computer use offers a general route to agents that can operate almost any software through the same interface available to people. Realizing this promise, however, requires more than visual grounding: an agent must sustain long-horizon state, acquire large amounts of costly interactive experience, and learn from outcomes that are sparse but reliably verifiable. We introduce Qwen-CUA, a native computer-use agent with a 397B-A17B Qwen mixture-of-experts backbone. It observes only screenshots and acts through keyboard and mouse events, without DOM trees, accessibility metadata, or task-specific APIs. Its agent scaffold expands the active visual history to 20 screenshots and folds older screenshots in fixed-size blocks, retaining recent visual evidence while preserving reusable prompt prefixes. To scale training, we build a cloud rollout fleet with access to nearly 100,000 vCPUs and tens of thousands of concurrent environments, construct approximately 40,000 verifiable tasks, and collect personalized long-horizon workflows in everyday and professional software. We optimize complete trajectories with verifiable rewards and long-horizon trajectory slicing, and develop the model through iterative training runs that use each resulting policy to refresh the supervised data mixture and calibrate the reinforcement-learning task distribution. Across eight computer-use benchmarks, Qwen-CUA consistently outperforms Qwen3.7 and remains competitive with leading proprietary systems, reaching 86.2 on OSWorld-Verified and 18.5 / 48.4 binary / partial completion on OSWorld 2.0. Scaling the same recipe to a model with over one trillion total parameters yields Qwen-CUA-Max, which further improves these scores to 87.6 and 21.2 / 53.3. Qwen-CUA also reduces attack success on RedTeamCUA from 36.6 to 16.4 relative to Qwen3.7. Efficiency analyses, an internal browser deployment, and experiments combining native interaction with Bash further characterize its practical behavior. These results show that native computer use can serve as a broadly capable foundation, while scalable verifiable interaction and hybrid tool use are key to making it effective in practice.
> ![](images/4ca666f98a056b6d1b544ef1d434391555041a70f5a4ae7ee9f54c4431b47e09.jpg)
> Figure 1: Main results across eight computer-use benchmarks. OSWorld 2.0 reports binary completion (dark) and partial completion (light), while RedTeamCUA reports benign task success under attack. All other panels show a single score. Higher is better.

Q1: 这篇论文试图解决什么问题？

论文聚焦的是通用计算机使用代理（computer-use agent）的核心挑战。作者认为，真正的原生计算机使用应该通过人机交互的同一界面（截图 + 键盘鼠标）来控制软件，这比依赖 DOM 树、无障碍接口或任务专用 API 的 Web 代理更通用，但也带来三个关键难点：第一，智能体必须维持长时间跨度的状态，因为真实任务往往需要数十乃至上百步交互，模型需要记住之前的屏幕状态与已执行的动作；第二，获取大规模交互经验非常昂贵，每一步 rollout 都消耗一个状态环境并占用真实时间，且可靠监督往往只在最终状态出现；第三，尽管最终奖励稀疏，但它通常是可验证的（如任务是否完成），这为强化学习提供了机会。此外，模型还需要泛化到不同用户、不同桌面环境以及不同应用程序，这对纯视觉界面下的 grounding 能力和决策能力要求很高。论文试图回答的问题包括：如何设计一个既能感知屏幕又能输出低层鼠标键盘动作的模型？如何在有限上下文下管理长视界历史？如何通过大规模可扩展的训练方法（包括数据构造、奖励设计和迭代策略更新）来获得可比的性能？

Q2: 有哪些相关研究？

从检索到的相关工作和背景信息来看，计算机使用研究经历了从结构化接口表示向截图感知和接地 GUI 动作的转变。早期工作倾向于使用 DOM 树或无障碍树等结构化信息，而近期研究转向让模型直接感知截图并预测接地动作。这一转变要求模型理解高分辨率、密集的界面内容，并将语言指令直接映射到屏幕坐标，这就是论文中提到的视觉接地与原生代理模型方向。另外，相关工作也提及如何在 rollout 中获取监督信号，以及如何利用最终状态进行 verifiable 验证。论文还强调与在线智能体、长视界决策和混合命令行交互等研究思路的关联。由于检索证据有限，无法列出具体文献名称，但可以推断文中会讨论此前基于 DOM 的 Web 代理、基于视觉的 GUI 代理、以及利用强化学习训练通用代理的代表性工作。

Q3: 论文如何解决这个问题？

Qwen-CUA 的解决方案可分为模型架构、交互接口、上下文管理和训练方法四部分。

1. **模型架构**：采用 397B 总参数、17B 激活参数的 Qwen MoE 骨干，支持多模态输入（屏幕截图）和动作输出。
2. **原生交互接口**：每一步只接收当前桌面的截图，动作空间为键盘和鼠标事件（如点击、键入、滚动等），完全避开 DOM 树、可访问性元数据和任务特定 API，从而保持与人类可操作界面的一致性。
3. **长视界上下文管理**：Agent scaffold 将活动视觉历史扩展到 20 张截图，同时将更早的截图按固定大小块折叠，在保留近期视觉证据的同时控制 prompt 长度并保持前缀可复用，降低计算开销。
4. **大规模训练基础设施**：搭建云端 rollout 集群，使用接近 100,000 个 vCPU 和数万个并发环境，构造约 40,000 个可验证任务，并收集日常和专业软件中的个性化长视界工作流。
5. **奖励与优化**：使用可验证奖励优化完整轨迹，并通过长视界轨迹切片（trajectory slicing）来缓解稀疏奖励和长序列训练困难。
6. **迭代训练流程**：每个训练 run 得到的策略都会用于刷新监督数据混合（supervised data mixture）和校准强化学习任务分布，形成循环迭代，让模型持续在更贴近当前能力分布的数据上学习。

Q4: 论文做了哪些实验？

论文从三个互补角度评估 Qwen-CUA：标准化智能体基准、真实世界计算机使用任务部署、以及原生交互与命令行工具（Bash）结合的混合交互。具体实验包括：

- 在八个计算机使用基准上与 Qwen3.7 和多个领先专有系统对比。
- 在 OSWorld-Verified 和 OSWorld 2.0 上报告二进制/部分完成分数。
- 在 RedTeamCUA 上评估安全相关攻击成功率。
- 进行效率分析（基于检索到的片段提到 efficiency analyses）。
- 在内部浏览器部署场景中测试实际利用率（检索片段提到 internal browser deployment）。
- 尝试将原生交互与 Bash 结合，考察混合交互的效果。

Q5: 发现了什么实验现象？

实验发现可归纳为以下几点：

- **性能领先**：Qwen-CUA 在八个基准上一致超过 Qwen3.7，说明其训练方法与架构设计有效；同时与领先专有系统竞争，证明开源模型在计算机使用这一困难任务上可以达到接近商业产品的水平。
- **规模效应**：将同一训练配方扩展到超万亿参数得到 Qwen-CUA-Max，OSWorld-Verified 从 86.2 提升到 87.6，OSWorld 2.0 二进制/部分完成从 18.5/48.4 提升到 21.2/53.3，说明更大的模型能够在长视界操作任务中带来稳定增益。
- **安全改善**：RedTeamCUA 的攻击成功率从 Qwen3.7 的 36.6 降至 16.4，幅度超过一半，推测是因为更稳健的视觉 grounding 和更长的历史信息有助于避免被恶意 prompt 误导。
- **效率与部署**：论文提及效率分析、内部浏览器部署和 Bash 结合实验，推测这些实验会揭示原生交互在真实环境中的可行性和混合交互的收益，但检索证据没有给出具体数字，需要在原文中进一步核对。

Q6: 有什么可以进一步探索的点？

基于论文的定位与实验结果，可以探索的方向包括：

- 进一步扩大模型规模或使用更高效的 MoE 结构，验证是否还能带来持续增益。
- 扩展到更多类型软件和工作流，尤其是专业软件、科学计算工具和新兴界面（如移动端、VR/AR）。
- 改进长视界记忆机制，例如引入外部记忆、分层历史压缩或基于注意力的关键帧选择。
- 降低训练和推理成本，探索更轻量的强化学习策略或离线 RL 方法，让更多研究团队能复现。
- 增强安全性和鲁棒性，研究如何抵御恶意屏幕内容或 prompt 注入攻击，尤其是降低 RedTeamCUA 等基准上的攻击成功率。
- 深入分析混合交互（原生 GUI + Bash）的增益与边界，找到最优的接口组合策略。
- 考虑将计算机使用能力迁移到科学工作流（如数据分析、软件操作自动化），与 AI-for-science 结合。
- 探索个性化适应：根据单个用户的使用习惯收集少量数据快速微调。

Q7: 总结一下论文的主要内容

论文提出 Qwen-CUA，一个原生计算机使用智能体。作者指出，人类操作计算机的通用界面是屏幕加键盘鼠标，因此一个真正通用的计算机代理应当只依赖截图观察和原生动作输出，而不是 DOM 树或专有 API。这不仅能覆盖几乎任何软件，还能更自然地泛化到用户自定义界面。然而，纯视觉界面带来三大挑战：长视界状态维持、交互经验获取成本高、奖励稀疏但可验证。

为此，Qwen-CUA 采用 397B-A17B 的 Qwen MoE 骨干，在每一步仅接收桌面截图，通过键盘鼠标事件动作完成任务。模型完全没有利用 DOM、无障碍元数据或任务特定 API。为了支撑长视界，agent scaffold 维护 20 张活动截图，并将更早的截图折叠为固定大小块，从而控制上下文长度并保持 prompt 前缀稳定，提高计算效率。

训练侧是论文的重点工程贡献。作者搭建了包含接近 100,000 个 vCPU 和数万个并发环境的云端 rollout 集群，构造了约 40,000 个可验证任务，并采集了个性化长视界工作流。训练中采用可验证奖励优化完整轨迹，并引入长视界轨迹切片来处理长序列的训练问题。更重要的是，论文采用迭代式训练流程：每个训练阶段产生的策略都会用于重新生成监督数据并调整强化学习任务分布，使模型持续在自身当前能力边缘学习。

实验覆盖八个计算机使用基准，包括 OSWorld-Verified、OSWorld 2.0、RedTeamCUA 等。结果显示 Qwen-CUA 在全部基准上一致超过 Qwen3.7，并与领先专有系统相当或更优：OSWorld-Verified 达到 86.2，OSWorld 2.0 的二进制/部分完成为 18.5/48.4。将相同配方扩展到超过一万亿总参数的 Qwen-CUA-Max 后，相应分数提升至 87.6 和 21.2/53.3，显示了规模化带来的明确收益。安全性方面，攻击成功率从 36.6 降至 16.4。论文还报告了效率分析、内部浏览器部署以及原生交互结合 Bash 的混合实验，进一步刻画了实际部署中的优势和局限。

整体来看，论文的核心主张是原生计算机使用是通用智能体的一条可行路径，而大规模可验证数据、长视界上下文管理和迭代式强化学习是支撑其成功的关键。该工作对计算机使用代理领域具有系统性贡献，也为后续研究提供了可复现的训练基础设施和方法论。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与智能体方向直接匹配（权重 0.10）：论文正是通用计算机使用智能体，涉及决策、长视界和工具使用。

## 基本信息

- 作者：Dunjie Lu, Shuai Bai, Tianyi Bai, Sicheng Fan, Chang Gao, Jian Guan, Feng Hu, Mianqiu Huang, Xingyang Huang, Yizhen Jiang, Yuheng Jing, Dehui Kong, Ning Li, Dayiheng Liu, Shixuan Liu, Zheng Liu, Que Shen, Bowen Wang, Junli Wang, Chencan Wu, Rui Xie, Tianbao Xie, Zhihui Xie, Haiyang Xu, An Yang, Tao Yu, Wenzhen Yuan, Xi Zhang, Zhenru Zhang, Mingkang Zhu, Zhaoqing Zhu, Yizhong Cao, Kai Dang, Binyuan Hui, Kaixin Li, Junyang Lin, Haiquan Wang, Zekun Wang, Yiheng Xu, Fan Yan, Mengqi Yuan, Danyang Zhang, Jiajun Zhang, Zhipeng Zhang, Fan Zhou, Fan Zhou
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-08-03
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.02352v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了论文元数据、摘要和 PDF 语义检索片段；因缺乏完整原文，部分内容（如效率分析具体数字、局限性细节）为合理推断，建议回原文核对。
