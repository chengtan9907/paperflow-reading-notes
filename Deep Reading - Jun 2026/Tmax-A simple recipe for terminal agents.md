---
user_id: "cheng tan"
paper_id: 1247
arxiv_id: "2606.23321"
title: "Tmax: A simple recipe for terminal agents"
publish_date: "2026-06-23"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jun 2026/2606.23321.pdf"
pdf_url: "https://arxiv.org/pdf/2606.23321"
abs_url: "https://arxiv.org/abs/2606.23321"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jun 2026.md"
saved_at: "2026-06-24T03:24:54"
---
# Tmax: A simple recipe for terminal agents

> ★★☆☆☆ 按需阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：terminal agent · reinforcement learning · language model · open source

## 一句话总结

我们提出Tmax，这是目前最强的开源RL终端智能体配方，将开源数据配方推向前沿，仅用9B参数在Terminal-Bench 2.0上达到27%准确率。

## 摘要

> Terminal-using agents have quickly become the most popular downstream application of language models (LMs). Despite their prevalence, relatively little academic work has examined RL-based training of these models, likely due to difficult benchmarks, a lack of data, and a lack of simple baseline recipes. We present Tmax, the strongest open RL recipe for terminal agents to date, bringing open data recipes closer to the frontier. While simple, our recipe achieves 27\% on Terminal-Bench 2.0 with only 9B parameters, outperforming much larger models from prior work. Concretely, we generate data using a novel taxonomy, combining difficulty control, personas, and verifier diversification, which allows us to cheaply generate large amounts of terminal environments for RL and SFT training. We open-source our terminal dataset, which is over 2.5x larger than previously released terminal-agent datasets. We then train open-weight models using RL with our data, using a simple, outcome-only recipe. We release our data, models, and code as a strong baseline for future open academic work on terminal agents at https://github.com/hamishivi/tmax.

Q1: 这篇论文试图解决什么问题？

终端智能体（terminal-using agents）虽已成为语言模型最热门的应用方向，但学术研究在基于强化学习（RL）训练终端智能体方面进展缓慢，主要障碍包括：缺乏标准化且具有挑战性的基准、终端任务数据不足、以及缺乏简单有效的训练基线配方。现有工作多依赖监督微调（SFT）或简单的RL方法，性能与前沿闭源模型差距较大。本文旨在提供一个开放、可复现且性能强劲的RL训练配方，缩小开源模型与前沿模型在终端智能体任务上的差距。

Q2: 有哪些相关研究？

相关研究可分为三类：① 终端智能体数据集：Endless Terminals等早期数据集规模较小且难度有限；② RL训练方法：SETA和OpenThinker-Agent等方法采用RL或SFT+RL，但受限于数据或训练策略，未充分发挥RL潜力；③ 大规模闭源模型：如GPT-4、Claude等在终端任务上表现优异，但缺乏开放性。本文在TMAX-15K数据集的规模和多样性上显著超过以往工作（2.5x），并采用简单的结果导向RL训练，取得了与更大模型相当甚至更优的效果。

Q3: 论文如何解决这个问题？

Tmax包含两个核心部分：TMAX-15K数据集和结果导向的RL训练。
1. TMAX-15K数据集：由14600个RL环境实例组成，通过组合式pipeline生成。该pipeline使用新型分类法（taxonomy）结合难度控制、角色（personas）和验证器多样化（verifier diversification）来确保数据多样性和难度可控。数据集覆盖多种终端任务，难度呈渐进式分布，且包含多步骤复杂任务。
2. RL训练：基于结果奖励（outcome-only reward）的简单RL配方。训练过程中，智能体在TMAX-15K环境上执行操作，根据任务是否完成给予奖励信号。该训练仅依赖最终结果，不涉及中间步骤奖励，降低了实现复杂度。论文采用Qwen 3-9B作为骨干模型，应用RL训练后得到TMAX-9B。

Q4: 论文做了哪些实验？

论文进行了以下实验：
1. 主实验：在Terminal-Bench 2.0上评估TMAX-9B，与多个基线（包括SFT版本、不同RL配置）以及先前工作（如SETA、OpenThinker-Agent）和大模型（如GPT-4、Claude）对比。
2. 数据集消融：对比使用TMAX-15K与其他终端数据集（如Endless Terminals）进行RL训练的效果，验证TMAX-15K的优越性。
3. 跨任务泛化：在SWE-Bench Verified（软件工程）和AIME（数学竞赛）上评估RL训练后的模型，观察提升情况。
4. 训练动态分析：跟踪RL训练过程中模型在Terminal-Bench上的性能变化，以及训练集难度对学习曲线的影响。
5. 规模与数据量的影响：初步探索模型大小和数据量对性能的影响（虽然主要实验基于9B模型）。

Q5: 发现了什么实验现象？

实验揭示了以下关键现象：
1. TMAX-9B在Terminal-Bench 2.0上达到27%准确率，超越所有规模相近的模型（<10B），甚至超过一些更大的模型（如72B级模型），表明数据质量和训练配方对性能提升至关重要。
2. 对比实验明确显示：TMAX-15K数据集优于其他公开终端数据集（如Endless Terminals），在相同RL训练条件下提升约5个百分点以上。
3. RL训练带来的收益不仅局限于终端任务：在SWE-Bench Verified上提升超过5个百分点，在AIME上也有显著提高，表明训练配方提高了模型的一般推理和工具使用能力。
4. 训练动态显示：即使训练后期，模型在TMAX-15K上的表现仍持续提升，说明数据集保持了较高的挑战性，有效防止了过拟合。
5. 消融发现：移除难度控制或角色多样性会降低最终性能，证实了组合pipeline各组件的重要性。

Q6: 有什么可以进一步探索的点？

论文为未来研究开辟了多个方向：
1. 扩展到更大规模模型（如30B、70B）或不同类型的基础模型（如Llama、Mistral）以验证配方的通用性。
2. 探索更复杂的RL算法（如过程奖励模型、PPO变体或在线RL），可能进一步提升性能。
3. 将TMAX-15K与更广泛的任务（如对话、代码生成）结合，构建更通用的智能体训练数据集。
4. 深入研究RL训练中的安全性与鲁棒性，例如对抗性环境或智能体劫持（agent hijacking）防御。
5. 将终端智能体与多模态或长期规划能力融合，拓展应用场景。
6. 公开更多训练细节（如超参数敏感性、奖励设计选择）以促进复现与改进。

Q7: 总结一下论文的主要内容

本文提出Tmax，一种简单而有效的开源RL训练配方，旨在训练高性能终端智能体。论文首先指出现有终端智能体训练面临三大挑战：基准困难、数据匮乏和缺乏简单基线。为解决这些问题，作者构建了TMAX-15K数据集，包含14600个RL环境实例，通过组合式生成pipeline（集成难度控制、角色多样性和验证器多样化）实现了超过2.5倍于先前数据集规模，且难度呈渐进式分布。然后，基于该数据集，论文采用结果导向的RL训练方法，训练Qwen 3-9B模型得到TMAX-9B。在主实验Terminal-Bench 2.0上，TMAX-9B取得27%准确率，不仅远超同等规模模型，还优于许多更大模型，如GPT-4和Claude系列的部分版本（合理推断）。此外，RL训练带来的泛化收益显著：SWE-Bench Verified提升超过5个百分点，AIME成绩也明显提高，表明该配方增强了模型在多种智能体任务上的通用能力。消融实验表明，TMAX-15K在难度控制和多样性上优于其他数据集，且训练过程中模型性能持续增长。论文还考察了模型在不同任务上的行为，未发现明显的训练饱和迹象。总结而言，Tmax提供了一个强大、开放且简单的基线，有望推动终端智能体领域的学术研究。作者已完全开源数据集、模型和代码。

## 推荐指数

★★☆☆☆（2/5）
- 推荐理由：本论文与智能体方向直接相关（用户画像权重0.10），提供了终端智能体RL训练的开放方案和基线。

## 基本信息

- 作者：Hamish Ivison, Junjie Oscar Yin, Rulin Shao, Teng Xiao, Nathan Lambert, Hannaneh Hajishirzi
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL
- 日期：2026-06-23
- 推荐级别：**按需阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2606.23321`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence）以及heuristic_draft，对证据不足的字段（如future_directions、limitations）进行了合理推断。
