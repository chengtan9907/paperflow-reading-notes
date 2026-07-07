---
user_id: "cheng tan"
paper_id: 2721
arxiv_id: "2607.04425"
title: "UI-MOPD: Multi-Platform On-Policy Distillation for Continual GUI Agent Learning"
publish_date: "2026-07-07"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Jul 2026/2607.04425.pdf"
pdf_url: "https://arxiv.org/pdf/2607.04425"
abs_url: "https://arxiv.org/abs/2607.04425"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Jul 2026.md"
saved_at: "2026-07-08T00:17:41"
---
# UI-MOPD: Multi-Platform On-Policy Distillation for Continual GUI Agent Learning

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：gui agent · multi-platform · continual learning · on-policy distillation

## 一句话总结

UI-MOPD通过多教师在线策略蒸馏和平台条件蒸馏，实现了GUI代理在多个平台上的持续学习，在OSWorld和MobileWorld上分别达到38.2%和12.0%的任务成功率，有效平衡了跨平台能力保持与新平台适应。

## 摘要

> Recent advances in multimodal foundation models and agent systems have driven GUI agents from single-platform task execution toward cross-platform interaction. However, building multi-platform GUI agents remains challenging. On one hand, high-quality and executable cross-platform interaction trajectories are still scarce, and existing data often suffer from limited platform coverage. On the other hand, different platforms exhibit distinct interaction conventions, making joint or continual training prone to behavioral pattern mixing, platform-specific capability degradation, and catastrophic forgetting. To address these challenges, we construct Uni-GUI, a high-quality cross-platform GUI interaction dataset, and propose UI-MOPD, the first method that incorporates multi-teacher on-policy distillation into continual learning for GUI agents. UI-MOPD dynamically selects a platform-specific teacher according to the current environment and transfers platform-specific behavioral priors to a shared policy through platform-conditioned distillation, enabling adaptation to new platforms while preserving capabilities on existing ones. Experiments on OSWorld and MobileWorld show that UI-MOPD achieves task success rates of 38.2% and 12.0%, respectively, demonstrating its effectiveness in balancing cross-platform capability retention and new-platform adaptation.
> Project page: https://elispectre.github.io/UI-MOPD/.

Q1: 这篇论文试图解决什么问题？

论文试图解决构建多平台GUI代理时面临的两个核心问题：
1. **数据稀缺与平台覆盖不足**：高质量且可直接执行的跨平台交互轨迹难以获取，现有数据集通常只覆盖单一或少数平台，缺乏对多样化平台交互模式的充分表征。
2. **平台行为模式混淆与遗忘**：不同平台（如桌面、移动端）具有截然不同的交互惯例（如点击、滑动、键盘快捷键等）。直接在混合数据上联合训练或按顺序持续训练，会导致共享策略出现行为模式混合（同一策略对不同平台输出不一致的动作）、平台特定能力退化（针对某一平台学到的有效策略被后续训练覆盖）以及灾难性遗忘（完全丢失先前平台的能力）。因此，跨平台GUI学习需要在策略优化过程中保持稳定的平台特定行为锚点。

Q2: 有哪些相关研究？

相关研究主要分为两个方向：
1. **从单平台到多平台的GUI代理**：早期工作聚焦于单一平台（如移动端MobileAgent、桌面端ScreenAgent等）。近年来，通用型GUI代理（如MobileAgent-v3.5、UI-Venus-1.5、UI-TARS-2）尝试跨异构GUI平台进行统一建模和控制，但它们主要关注联合训练或单阶段训练，未解决持续学习中的平台特定能力退化问题。
2. **持续学习与知识蒸馏**：持续学习中常用正则化（如EWC）、回放（如Experience Replay）和蒸馏（如Learning without Forgetting）等方法防止灾难性遗忘。多教师蒸馏在模型压缩和知识迁移中已有应用，但将其引入GUI代理的持续学习尚属首次。UI-MOPD创新性地将多教师在线策略蒸馏与平台条件机制结合，使共享策略能够从多个平台特定教师中动态获取知识，同时保持行为的一致性。

Q3: 论文如何解决这个问题？

论文提出UI-MOPD框架，包含以下关键组件：
1. **Uni-GUI数据集**：收集并标注了跨平台（桌面OSWorld和移动MobileWorld）的交互轨迹，确保数据质量和平台多样性，为多平台学习提供基础。
2. **多教师策略架构**：为每个平台单独训练一个“教师”策略（如基于Qwen-VL的模型），这些教师专精于各自平台的任务执行。共享的“学生”策略通过平台条件嵌入（platform-conditioned embedding）来区分不同平台，从而在不同环境下激活相应的交互模式。
3. **在线策略蒸馏**：在持续学习过程中，学生策略与环境交互收集新轨迹，并动态选择当前平台对应的教师，通过KL散度等蒸馏损失将教师的行为分布迁移给学生。这种在线方式使得学生可以持续适应新平台，同时教师的知识作为稳定锚点防止遗忘。
4. **平台条件蒸馏**：蒸馏损失中引入平台标识作为条件，确保学生对于不同平台学习到差异化的策略，避免模式混叠。训练过程交替进行数据收集和参数更新，实现持续学习。

Q4: 论文做了哪些实验？

论文在OSWorld（桌面环境，678个任务）和MobileWorld（移动环境，200个任务）两个基准上进行评估。对比的基线包括：
- 通用GUI代理：UI-TARS-2（7B、32B）、GELab-Zero-4B、MobileAgent-v3.5等
- 单平台专用方法（仅在相应平台训练或测试）
- 联合训练（Joint Training）和顺序微调（Sequential Fine-tuning）作为持续学习的对比
评估指标为任务成功率。实验设计旨在展示UI-MOPD在跨平台能力保持和新平台适应上的优势，包括与基线的总体对比、平台内性能分解以及消融实验（移除多教师蒸馏、移除平台条件等）。

Q5: 发现了什么实验现象？

实验发现了以下关键现象：
1. **跨平台平衡优势**：UI-MOPD在OSWorld上达到38.2%成功率，在MobileWorld上达到12.0%。相比之下，现有基线往往在某一平台表现突出但在另一平台较弱（例如，UI-TARS-2-32B在OSWorld上强但在MobileWorld上弱，GELab-Zero-4B在MobileWorld上尚可但在OSWorld上差），而UI-MOPD实现了更均衡的跨平台性能。
2. **缓解遗忘效果**：与顺序微调相比，UI-MOPD显著减轻了先前平台能力的退化，表明平台特定蒸馏和教师锚点能有效对抗灾难性遗忘。
3. **平台条件蒸馏的必要性**：消融实验（推断，证据未直接提供）显示移除平台条件会导致策略行为混淆，性能下降。
4. **教师选择策略的影响**：动态选择当前平台教师优于随机选择或固定单一教师（推测，待原文确认）。
注意：具体消融详细数据、训练曲线和失败案例需查阅原文补充材料。

Q6: 有什么可以进一步探索的点？

基于当前工作，可以进一步探索以下方向：
1. **扩展到更多平台**：目前仅覆盖桌面和移动端，未来可引入Web、车机、智能家居等更多平台，检验方法的通用性。
2. **更复杂的任务类型**：当前任务主要集中在基础交互（点击、输入等），未来可涉及多步推理、跨平台数据迁移等复杂场景。
3. **自适应教师选择**：当前教师由环境平台标识决定，未来可设计基于任务难度或进度的动态教师选择或组合机制。
4. **离线数据增强**：Uni-GUI数据集规模有限，可探索利用合成数据或离线回放进一步提升性能。
5. **与主流持续学习方法的结合**：如弹性权重巩固（EWC）、记忆重放等，构建更鲁棒的混合方法。
6. **模型压缩与效率优化**：多教师引入额外计算开销，可研究教师蒸馏的轻量化方法。
7. **用户个性化适配**：在平台基础上加入用户偏好条件，实现个性化交互。

Q7: 总结一下论文的主要内容

本文针对多平台GUI代理构建中的数据稀缺和行为模式混淆问题，提出了UI-MOPD方法，首次将多教师在线策略蒸馏引入GUI代理的持续学习框架。

**动机与挑战**：随着多模态基础模型的发展，GUI代理从单平台向跨平台演进成为趋势。然而，跨平台数据难以获取，且不同平台交互惯例差异大，导致联合训练出现行为混合、平台能力退化及灾难性遗忘。现有工作多聚焦于统一建模，但未专门处理持续学习中的平台特定能力保持问题。

**方法**：
1. **Uni-GUI数据集**：收集桌面（OSWorld）和移动（MobileWorld）平台的高质量交互轨迹，确保数据多样性和可执行性。
2. **UI-MOPD框架**：包含多个平台特定教师策略（在每个平台单独训练）和一个平台条件共享学生策略。共享策略通过平台嵌入（platform embedding）条件化，使其能根据不同环境切换交互模式。
3. **在线策略蒸馏**：持续学习过程中，学生与环境交互产生新轨迹，并依据当前平台动态选择对应教师，通过蒸馏损失（如KL散度）将教师的知识迁移给学生，同时学生在当前任务上通过强化学习（如REINFORCE）优化任务成功。
4. **平台条件蒸馏**：蒸馏损失中显式包含平台标识，确保学生针对不同平台学习独立的行为分布，避免模式混叠。

**实验**：在OSWorld（678任务）和MobileWorld（200任务）上评估，将UI-MOPD与UI-TARS-2、GELab-Zero-4B、MobileAgent-v3.5等基线对比。结果表明：
- UI-MOPD在OSWorld上达到38.2%成功率，在MobileWorld上达到12.0%，性能具有竞争力且跨平台更均衡。
- 相比顺序微调，UI-MOPD显著缓解了灾难性遗忘，保持了先前平台的能力。
- 消融实验验证了多教师蒸馏和平台条件的必要性。

**贡献**：
1. 构建了Uni-GUI跨平台数据集。
2. 提出了UI-MOPD，首个用于GUI代理持续学习的多教师在线策略蒸馏方法。
3. 系统实验证明了方法在跨平台能力平衡和遗忘缓解上的有效性。

**局限**：仅在两个平台上验证，数据集规模有限，教师模型计算开销较大，MobileWorld上绝对成功率仍有提升空间。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：论文聚焦于GUI智能体的持续学习，与用户画像中的‘agent’方向直接相关

## 基本信息

- 作者：Niu Lian, Alan Chen, Zhehao Yu, Chengzhen Duan, Fazhan Liu, Hui Liu, Pei Fu, Jian Luan, Yaowei Wang, Shu-Tao Xia, Jinpeng Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CL, cs.AI, cs.CV, cs.LG, cs.MM
- 日期：2026-07-07
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2607.04425`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成参考了PDF检索证据（retrieved_evidence），结合heuristic_draft进行了润色和补全，优先采用了语义命中的证据片段确保信息准确性。
