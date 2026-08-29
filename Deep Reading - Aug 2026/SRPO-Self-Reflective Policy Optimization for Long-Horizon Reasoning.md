---
user_id: "cheng tan"
paper_id: 9187
arxiv_id: "2608.23493v1"
title: "SRPO: Self-Reflective Policy Optimization for Long-Horizon Reasoning"
institution: "未明确说明（根据作者姓名推测为中国研究团队，如武汉大学或上海交通大学相关实验室，但需回原文确认）"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23493v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23493v1"
generation_provider: "openai-compatible"
generation_model: "gemini-3-flash-preview"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:13:31"
---
# SRPO: Self-Reflective Policy Optimization for Long-Horizon Reasoning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/gemini-3-flash-preview

🏷 关键词：self-reflection · policy optimization · long-horizon reasoning · large language models

## 一句话总结

SRPO 提出了一种自反思策略优化框架，通过将稀疏的终端反馈转化为密集的“反思补丁”指令，使大语言模型在无需外部评论者的情况下，以极高的计算效率实现长程推理能力的内化。

## 摘要

> Self-reflection is a powerful mechanism for credit assignment in human learning, converting sparse outcome feedback into actionable guidance. However, its potential for post-training Large Language Models (LLMs) remains underexplored. We propose Self-Reflective Policy Optimization (SRPO), a framework that internalizes this capability. SRPO enables LLMs to analyze their own completed trajectories, synthesize errors into concise "reflection patches," and use reflection-conditioned teacher scores on student on-policy rollouts as dense token-level training signals. This process effectively transforms sparse terminal supervision into dense, token-level learning signals without requiring external critics, separate reward models, or larger teacher models. We demonstrate that SRPO achieves state-of-the-art performance across mathematical reasoning and long-horizon agentic benchmarks with exceptional data efficiency. Using a Qwen3-8B base model, SRPO attains 73.3% on AIME'24 using only 8% (0.08x) of the training FLOPs required by scaled supervised fine-tuning, while significantly improving success rates on WebShop (64.7%), ALFWorld (76.8%), and SWE-Bench-Lite (31.2%). Code is available at https://github.com/Galleons2029/SRPO

Q1: 这篇论文试图解决什么问题？

### 核心挑战：长程推理中的信用分配
在解决复杂的数学问题或执行多步智能体任务（如软件工程）时，模型面临的主要困难是**稀疏反馈（Sparse Feedback）**。通常只有在整个任务结束时才能获得成功或失败的信号，这使得模型难以判断中间哪一步是导致失败的关键，即“信用分配问题（Credit Assignment Problem）”。

### 现有方法的局限性
1. **强化学习（RL）的低效性**：传统的 PPO 等算法依赖于稀疏的终端奖励，收敛速度极慢，且需要复杂的奖励模型（RM）或外部评论者（Critic）。
2. **监督微调（SFT）的局限**：SFT 依赖于高质量的专家轨迹，但在长程任务中，专家数据获取成本极高，且模型难以从自身的错误中学习。
3. **推理时反思的开销**：虽然在推理阶段加入反思步骤（如 Reflexion）可以提升性能，但会显著增加推理延迟和 Token 消耗，且模型并未真正“学会”如何避免错误。
4. **语义漂移（Semantic Drift）**：在长轨迹中不断追加反思内容会导致上下文过长，进而引发语义崩溃或模型注意力分散。

Q2: 有哪些相关研究？

### 强化学习与策略优化
现有的 LLM 后训练主要依赖于基于人类反馈的强化学习（RLHF）。然而，在推理任务中，如何构建有效的密集奖励函数仍是难题。SRPO 借鉴了 RL 的思想，但通过自反思机制替代了显式的奖励模型。

### 自反思机制（Self-Reflection）
先前的研究（如 Reflexion, Self-Refine）证明了 LLM 可以通过观察自身输出并进行修正来提升表现。SRPO 的不同之处在于，它不仅仅将反思作为推理时的插件，而是通过训练将其**内化（Internalize）**到模型参数中。

### 课程学习与数据增强
SRPO 与 STaR (Self-Taught Reasoner) 等方法有相似之处，即利用模型生成的正确答案进行迭代学习。但 SRPO 进一步利用了失败的尝试，通过分析“为什么失败”来提供比单纯“正确答案”更丰富的监督信号。

Q3: 论文如何解决这个问题？

### SRPO 框架核心流程
SRPO 采用了一种“训练时有反思，推理时无反思”的非对称设计，主要分为两个阶段：

#### 1. 反思引导的状态增强（Stage 1: Reflection-Guided State Augmentation）
* **初始采样**：模型针对给定问题 $x$ 生成初始轨迹 $y$。
* **自反思生成**：如果 $y$ 失败，模型会分析错误原因，生成简洁的“反思补丁（Reflection Patch）” $r$。这个补丁包含了对错误的诊断和改进建议。
* **带记忆的重置（Reset-with-Memory）**：这是 SRPO 的关键创新。模型不是在错误的轨迹后面追加反思，而是将反思补丁 $r$ 置于原始提示 $x$ 之后，形成增强提示 $(x, r)$，然后从初始状态重新生成轨迹。这避免了长轨迹导致的语义漂移，并保持了任务规范的忠实度。

#### 2. 策略内化与优化（Stage 2: Policy Internalization）
* **密集 Token 级监督**：利用增强提示 $(x, r)$ 下生成的成功轨迹作为“教师”，为原始提示 $x$ 下的学生采样提供 Token 级的 KL 散度约束或交叉熵损失。
* **内化训练**：通过这种方式，模型学习在没有显式反思补丁的情况下，也能做出原本需要反思引导才能做出的正确决策。最终模型在推理时只需输入 $x$，即可直接输出高质量轨迹。

Q4: 论文做了哪些实验？

### 实验设置
* **基础模型**：Qwen3-8B。
* **基准测试**：
 * **数学推理**：AIME'24, MATH 500。
 * **长程智能体**：WebShop（电商购物）、ALFWorld（室内任务执行）、SWE-Bench-Lite（软件工程修复）。
* **对比基线**：包括 SFT、PPO、DPO 以及推理时反思方法（如 Reflexion）。

### 关键指标
* **成功率（Success Rate）**：任务完成的准确度。
* **数据效率（Data Efficiency）**：达到特定性能所需的训练 FLOPs 或样本量。

Q5: 发现了什么实验现象？

### 核心发现
1. **极高的计算效率**：SRPO 在 AIME'24 上达到 73.3% 的准确率，仅消耗了 SFT 达到同等水平所需 FLOPs 的 **8% (0.08x)**。这证明了密集反思信号比海量专家数据更有效。
2. **长程任务的突破**：在 SWE-Bench-Lite 这一极具挑战性的软件工程任务中，SRPO 达到了 31.2% 的成功率，显著超过了传统的微调方法。
3. **非对称增益**：实验证实，通过“带记忆的重置”机制，模型能够有效地将反思补丁中的高阶逻辑内化。推理时不携带补丁的性能接近甚至在某些情况下超过了携带补丁的原始表现。
4. **消融实验**：如果移除反思补丁，仅使用成功的轨迹进行训练，性能会大幅下降，证明了“错误分析”在信用分配中的核心作用。
5. **抗漂移能力**：相比于迭代追加反思的方法，SRPO 的重置机制保持了模型对原始任务目标的关注，避免了在长对话中迷失方向。

Q6: 有什么可以进一步探索的点？

### 可探索的方向
1. **多模态反思**：将 SRPO 扩展到视觉-语言任务中，让模型反思其在视觉感知上的错误。
2. **反思补丁的自动化优化**：目前补丁的格式相对固定，未来可以探索如何让模型自适应地调整反思的粒度和深度。
3. **跨模型迁移**：研究由大模型生成的反思补丁是否可以更有效地指导小模型的策略优化。
4. **在线 SRPO**：将目前的离线训练流程转化为完全在线的强化学习过程，实现持续进化。

Q7: 总结一下论文的主要内容

这篇论文介绍了 SRPO（自反思策略优化），这是一种旨在解决大语言模型在长程推理任务中信用分配难题的新型后训练框架。SRPO 的核心思想是利用模型自身的反思能力来产生密集的监督信号。具体而言，当模型在尝试任务失败时，它会生成一个“反思补丁”，指出错误并提供改进方向。通过一种创新的“带记忆重置”机制，模型在包含反思补丁的增强语境下重新尝试任务。如果尝试成功，这一过程就产生了一对“有反思引导的成功轨迹”和“无反思引导的原始尝试”。SRPO 利用前者作为教师信号，通过 Token 级的优化目标，强制模型将反思中的逻辑内化到其基础策略中。实验结果令人印象深刻，SRPO 在数学推理（AIME'24 73.3%）和复杂智能体任务（SWE-Bench-Lite 31.2%）上均取得了 SOTA 性能，且在计算资源消耗上比传统的 SFT 节省了 92%。该研究为实现高效、自主进化的推理模型提供了一条极具潜力的路径，强调了“从错误中学习”和“内化反思”在构建高级人工智能中的重要性。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：该论文与智能体（Agent）方向高度相关，特别是解决长程任务中的信用分配问题。

## 基本信息

- 作者：Jialong Liu, Yuling Shi, Ning Yang, Xiaodong Gu, Zuchao Li
- 机构：未明确说明（根据作者姓名推测为中国研究团队，如武汉大学或上海交通大学相关实验室，但需回原文确认）
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / gemini-3-flash-preview
- arXiv ID：`2608.23493v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了 PDF 检索证据，特别是关于 SRPO 框架的两个阶段、Reset-with-Memory 机制以及在 AIME'24 和 SWE-Bench-Lite 上的具体实验数值。
