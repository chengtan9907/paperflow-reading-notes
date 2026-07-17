---
user_id: "cheng tan"
paper_id: 4329
arxiv_id: "2607.14895v1"
title: "Leveraging Instruction Tuning and Merging for Reasoning Model Adaptation"
publish_date: "2026-07-16"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.14895v1.pdf"
pdf_url: "https://arxiv.org/pdf/2607.14895v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-18T00:56:19"
---
# Leveraging Instruction Tuning and Merging for Reasoning Model Adaptation

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：instruction tuning · model merging · reasoning language model · model adaptation

## 一句话总结

本文提出一种通过指令微调并合并模型参数来高效适应推理语言模型的方法，在可验证和难验证领域均能提升性能且成本极低。

## 摘要

> Reasoning language models (RLMs) have demonstrated impressive performance in domains such as mathematics and coding. These domains permit reliable verification of model outputs, which is important for enabling the reinforcement learning that drives RLM performance gains. However, training RLMs on domains that lack reliable verifiers remains challenging. Meanwhile, for both verifiable and unverifiable domains, large amounts of unused supervised fine-tuning data with human-written solutions exist. In this work, we show that these data can be used efficiently to further improve RLM performance. For this, we first use classic instruction tuning, supervised fine-tuning without reasoning traces, on the RLM. Next, we merge our instruction-tuned model with the original reasoning model, recovering its reasoning behavior on the target domain. Our extensive evaluation demonstrates that our technique improves RLM performance in both verifiable and hard-to-verify domains, including coding and text summarization, while preserving RLM capabilities across other domains. Importantly, our method is highly cost-effective, enabling such improvements for less than USD $3.

Q1: 这篇论文试图解决什么问题？

推理语言模型在数学、编程等可验证领域性能很好，因为这些领域可以通过可靠验证器进行强化学习。但在缺乏可靠验证器的领域（如文本摘要、开放问答）中，训练RLM面临挑战，因为无法直接使用RL来优化。同时，存在大量包含人工编写的标准解答的监督微调数据，但这些数据不包含推理链，传统上被认为对RLM训练效率低下。核心问题：如何高效利用已有的SFT数据来进一步提升RLM在可验证和难验证领域的性能，同时保持模型在其他领域的推理能力，且成本低廉。

Q2: 有哪些相关研究？

相关研究包括指令微调（Instruction Tuning），即使用任务指令和标准输出进行微调；模型合并（Model Merging），如Task Arithmetic、Model Soup等参数加权合并方法；以及推理模型训练，通常使用强化学习（如GRPO）或蒸馏方法训练长思维链。本工作结合指令微调和模型合并，利用现有无推理链的SFT数据，无需验证器即可实现RLM的高效适应。

Q3: 论文如何解决这个问题？

方法分为两步：1. 指令微调：在目标领域的数据集上进行标准监督微调，只使用输入-输出对（无推理链），使模型学习目标任务输出分布。2. 模型合并：将指令微调后的模型与原始推理模型进行参数合并（线性插值），合并系数基于目标任务小规模校准集选择，以最大化推理性能。整个过程无需验证器或奖励模型，适应成本不到3美元。

Q4: 论文做了哪些实验？

实验评估了四个开源RLM（包括OpenThinker 7B等），任务包括编程（可验证）和文本摘要（难验证）。基线与对比方法可能包括直接SFT、RL训练等。评估指标：编程使用通过率或单元测试，摘要使用ROUGE-L。消融研究包括合并系数选择策略、数据量影响等。

Q5: 发现了什么实验现象？

指令微调后，模型在目标任务上性能提升，但原始推理能力可能下降。模型合并后，推理能力得到恢复，同时在目标任务上保持提升。该方法在编程和摘要领域均有效，且具有极低成本和鲁棒性。合并系数的选择是关键，基于校准集自动选择能取得最佳权衡。

Q6: 有什么可以进一步探索的点？

探索更复杂的合并策略（如层特定合并、加权系数自适应）；扩展应用到更多领域（如对话、推理密集型任务）；研究合并的可解释性；结合其他参数高效微调方法；探索当目标领域与原始分布差异较大时的性能边界；更系统的系数选择方法。

Q7: 总结一下论文的主要内容

本文提出了一种简单、低成本的方法来适应推理语言模型到新的领域，尤其是那些缺乏可靠验证器的领域。方法先进行指令微调，然后与原始模型合并以恢复推理能力。合并系数通过校准集确定。在四个开源RLM上的实验表明，该方法在编程和文本摘要任务上均能提升性能，同时保持其他领域能力。成本极低（不到3美元），无需验证器，适合实际部署。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：指令微调在推理模型上的特殊应用

## 基本信息

- 作者：Yu-Du Feng, Niels Mündler-Sasahara, Mark Vero, Martin Vechev
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.LG, cs.CL
- 日期：2026-07-16
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.14895v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了论文摘要和引言片段（来自PDF语义检索证据），对未覆盖部分进行了合理推断。
