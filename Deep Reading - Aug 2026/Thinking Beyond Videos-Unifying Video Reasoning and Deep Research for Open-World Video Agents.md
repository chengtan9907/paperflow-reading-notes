---
user_id: "cheng tan"
paper_id: 9137
arxiv_id: "2608.23329v2"
title: "Thinking Beyond Videos: Unifying Video Reasoning and Deep Research for Open-World Video Agents"
publish_date: "2026-08-24"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.23329v2.pdf"
pdf_url: "https://arxiv.org/pdf/2608.23329v2"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-29T01:06:43"
---
# Thinking Beyond Videos: Unifying Video Reasoning and Deep Research for Open-World Video Agents

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：video deep research · video agent · tool use · reinforcement learning

## 一句话总结

本文提出 VideoRover，一种统一的视频深度研究框架，通过迭代协调视频裁剪、多模态搜索与网页浏览，使智能体能在长视频中定位稀疏证据并获取外部知识，并以 26K SFT 轨迹和 3K RL 实例训练，在 VideoDR 和 VideoRover-Bench 上达到与专有模型相当的性能。

## 摘要

> Open-world video understanding often requires a model to locate sparse visual evidence and acquire external knowledge that is absent from the video and its parametric memory. While Thinking-with-Videos enables active temporal perception and Deep Research supports multi-step information seeking, the two capabilities are typically developed in isolation. We introduce VideoRover, a unified Video Deep Research framework that iteratively coordinates video cropping, multimodal search, and webpage browsing. Given a video-question pair, VideoRover uses each tool result to select the next action, so localized video clips guide external retrieval and retrieved evidence triggers further video inspection and verification. To develop this capability, we construct an automated data curation pipeline, producing 26K verified SFT trajectories and 3K challenging RL instances. We also introduce VideoRover-Bench, a benchmark stratified by video duration and research difficulty. Experiments on VideoDR and VideoRover-Bench show that our VideoRover-8B-RL achieves performance comparable to proprietary models in the direct-answer setting without tool use while outperforming larger open-source models equipped with the same tool suite. Ablation studies and training dynamics further validate the complementary roles of active video grounding, external retrieval, and long-horizon reinforcement learning.

Q1: 这篇论文试图解决什么问题？

这篇论文试图解决的核心问题是：开放世界视频理解任务中，模型不仅需要从长视频中定位稀疏的视觉证据（例如某个瞬间的物体、动作或事件），还需要获取视频本身和模型参数记忆中都缺失的外部知识（例如背景资料、语义概念、事实信息）。这类任务被称为“视频深度研究”（Video Deep Research）。现有工作将两种能力割裂开发：一方面，“Thinking-with-Videos”方法通过主动的时序感知（如关键帧选择、自适应视频重采样）来改善对视频内稀疏证据的获取，但主要局限于视频内部推理；另一方面，“Deep Research”智能体擅长多步信息检索（如网页搜索、多模态搜索），但主要作用于文本、静态图像和网页，缺少对视频内容的实时定位和动态决策。这种割裂导致模型在面对需要同时进行视频内证据发现和视频外知识获取的开放世界任务时，无法有效地将两者串成连贯的决策链。具体来说，存在三个技术缺口：（1）如何根据当前证据缺口决定下一步是观察视频的哪个片段、搜索哪个关键词、还是浏览哪个网页；（2）视频动作和检索动作如何更新一个共享的记忆状态，使得后续决策能够利用跨模态的累积证据；（3）如何构建高质量的监督数据和奖励信号来训练这种长时程、跨模态的决策智能体。论文的出发点正是要弥合这两个能力孤岛，实现“边看视频、边做研究”的统一代理能力。

Q2: 有哪些相关研究？

根据摘要和引言片段，相关工作主要分为两大方向：
1. **Thinking-with-Videos（视频内思考）**：这类方法致力于提升模型对视频时间维度的主动感知，常见手段包括关键帧选择、自适应视频重采样等。这些方法虽然能较好地定位视频中稀疏的时序证据，但推理范围基本局限于输入视频本身，无法获取超出视频和参数记忆的外部信息。论文指出，现有方法主要是在视频内部进行推理。
2. **Deep Research（深度研究）**：这类智能体（agent）通常被设计为多步信息获取和执行器，能够进行网页浏览、文本/图片搜索等。但正如论文所述，这些研究智能体主要作用于文本、静态图像和网页，代表性的工作包括 Jin et al. (2025)、Wu et al. (2026)、Huang et al. (2026) 等。这些方法缺乏对视频内容的主动定位能力，因而难以处理以视频为信息源的任务。
值得注意的是，论文提到了“rch agents” 的片段，可能指的是“search agents”或“research agents”，从上下文看应该是“research agents”。这些工作忽略了跨模态的决策过程，即如何将视频观察与外部检索结合在一起。
3. **视频问答与视频智能体**：虽然摘要中没有直接提及，但可以推测该工作继承了一般视频问答（VideoQA）和视频基础模型的研究脉络，将传统静态理解拓展为动态的工具使用和推理。
4. **数据合成与强化学习**：论文构建了自动化数据合成流水线，并采用强化学习训练，这与当前大模型智能体中常用的“可验证奖励 + RL”范式（如 DeepSeek-R1、OpenAI o1 等）有关。不过论文没有在摘要中明确提及这些工作，属于合理推断。

Q3: 论文如何解决这个问题？

VideoRover 的核心思想是将“视频内思考”和“外部深度研究”统一为一个迭代的决策过程。具体方法如下：
1. **统一的代理框架**：给定一个视频-问题对，VideoRover 持续维护一个包含当前证据和待解决问题的“记忆状态”。每一步，模型根据当前状态选择一个工具动作，工具执行后返回结果，结果再更新记忆状态，如此循环直到得到最终答案。可用的工具包括：视频裁剪（在视频中定位特定时间片段）、多模态搜索（通过图像检索或文本查询获取互联网信息）、网页浏览（访问具体网页获取内容）。
2. **自适应研究规划**：根据当前证据缺口，模型决定下一步动作。例如，若感觉某个画面包含关键信息，会选择视频裁剪进行局部观察；若发现视频中出现了未知实体，会选取关键帧进行图像检索或构造文本查询；若检索结果需要进一步验证，则会访问相关网页。这种规划是动态的，由工具结果驱动。
3. **迭代协调**：每个工具结果都会反馈到决策过程中，使得局部视频片段能够引导外部检索（例如捕获的关键帧作为图像查询），而检索到的证据又会触发进一步的视频检查（例如知道了待查找的目标后重新定位视频中的对应片段）和验证（通过浏览网页确认事实）。
4. **训练数据合成流水线**：由于人工标注视频深度研究轨迹非常昂贵，作者设计了一条自动化数据合成流水线，生成 26K 条经过验证的 SFT 轨迹和 3K 条挑战性的 RL 实例。这些轨迹包含了从初始视频观察到最终答案的完整动作序列，并且经过了自动化验证以保证质量。
5. **两阶段训练策略**：先使用 SFT 轨迹进行监督微调，让模型学会基本的多步工具使用；再在 RL 实例上进行强化学习，通过长时程奖励信号优化模型的规划能力。实验中的模型为 VideoRover-8B-RL，基础模型大小约为 8B 参数。
6. **基准构建**：作者提出了 VideoRover-Bench，按照视频时长和研究难度两个维度对测试样本进行分层，从而能够更细致地评估不同视频长度和不同研究难度下的模型表现。

Q4: 论文做了哪些实验？

论文在摘要中明确提到了两个实验基准：VideoDR 和 VideoRover-Bench。实验设计描述如下：
1. **模型与基线**：主推模型为 VideoRover-8B-RL（8B 参数，经过 SFT + RL）。对比对象包括：专有模型（proprietary models，未指明确切名字，可能是 GPT-4V 或 Gemini 等，但论文未给出）和更大规模的开源模型（“larger open-source models”）。所有对比模型在需要工具时都配备与 VideoRover 相同的工具套件（即公平对比）。此外，论文也做了“直接回答”设定（即不使用工具）下的对比。
2. **自动数据合成与训练规模**：构建了 26K 条 SFT 轨迹和 3K 条 RL 实例。可以合理推断数据集涵盖各种视频类型和问题类型，但论文未披露具体构成。
3. **基准分层**：VideoRover-Bench 根据视频时长和研究难度分层，意味着测试集允许按这两个维度切分，评估模型在不同难度/时长上的性能变化。
4. **消融研究与训练动态**：摘要提到进行了消融实验和训练动态分析，用于验证主动视频定位、外部检索和长时程强化学习各自的贡献。“训练动态”可能指 reward 曲线、动作序列成功率等，但论文未给出具体细节。
5. **评估设定**：主要报告了在直接回答（direct-answer）设定和工具使用（tool-use）设定下的性能。直接回答设定下，模型仅根据视频和问题直接输出答案，不调用工具；工具使用设定下，模型可以调用工具。
需要指出的是，由于我们只获得了摘要和引言片段，实验中具体的数值（如准确率、成功率）以及与其他模型的成绩对比表并没有出现在检索证据中，因此不能编造任何具体数字。但可以确认论文声称核心结果：VideoRover-8B-RL 在无工具的直接回答设定下与专有模型相当，并且在配备相同工具集的情况下优于更大开源模型。

Q5: 发现了什么实验现象？

根据摘要可以提炼出以下实验观察：
1. **直接回答设定下的能力涌现**：VideoRover-8B-RL 在无工具使用的直接回答设定下达到了与专有模型相当的性能。这一现象表明，通过工具使用训练（即使训练时使用工具），模型内在的视频理解和推理能力也显著增强，工具学习带来的收益泛化到了纯推理场景。这是一种“通过使用工具提升底层能力”的迁移现象，类似于训练时使用搜索但测试时关闭搜索仍能提高准确率，可能是训练过程促使模型学会了更细致的视频证据定位。
2. **对更大开源模型的超越**：在配备相同工具集的情况下，VideoRover-8B-RL（8B）优于更大的开源模型。这说明在特定的视频深度研究任务上，训练数据质量、训练范式和任务设计比单纯的模型参数量更重要。这可能得益于针对性的数据合成和 RL 优化，使得小模型能够高效利用工具。
3. **组件互补性**：消融研究验证了主动视频定位、外部检索和长时程 RL 三个组件的互补作用。这意味着去掉任何一个组件都会导致性能下降，三者缺一不可。主动视频定位保证了从视频中获取证据的准确性，外部检索补充了超出视频的知识，而长时程 RL 则确保了多步决策的连贯性和最优性。
4. **训练动态的启示**：论文提到训练动态验证了上述互补作用，这暗示性能可能随着 RL 训练逐步提升，且不同能力（如选择视频裁剪的时机、检索相关性）在不同训练阶段显露。
由于没有看到具体消融数据，以上观察只是基于摘要的定性描述。更细的反直觉结果或失败案例，如特定难度下性能骤降、工具相互作用不良等，论文并未在摘要中提及，无法在此给出。

Q6: 有什么可以进一步探索的点？

基于论文提出的框架，可以探索以下进一步的方向：
1. **更复杂的工具协同**：当前工具集合包括视频裁剪、多模态搜索和网页浏览，未来可以扩展更多工具，例如代码执行、数据库查询、多模态文档阅读、甚至与外部专业系统（如 GIS、医学影像库）交互。如何让模型在更大的工具集上高效选择，是一个开放问题。
2. **长视频和实时流**：VideoRover-Bench 已按视频时长分层，但现有模型是在离线视频上工作。对于超长视频（如数小时的监控录像）或实时流，如何保持记忆的有效性和计算效率，需要更高效的时间索引和记忆压缩机制。
3. **奖励模型与验证器**：当前 RL 实例有自动验证，但验证器的设计可能是一个瓶颈。未来可以训练可学习的验证模型来检查答案的证据支持，甚至构建反馈循环来改善数据合成。
4. **多轮交互和主动性**：目前的“深度研究”可能是一次性的批量研究，未来可以让用户与智能体对话，根据用户反馈调整研究方向，实现交互式视频研究。
5. **跨语言和跨文化知识**：外部知识可能涉及多种语言或本地化常识，模型应能利用不同语言的网页资源，这对多语言检索和跨语言对齐提出挑战。
6. **可解释性和引用验证**：深度研究要求给答案附上证据链。未来需要研究如何让模型输出可追溯的证据路径，以及如何自动检测检索到的事实与视频的冲突。
7. **高效训练范式**：26K SFT + 3K RL 的数据量相对可观，但如何用更少的轨迹达到类似效果，或采用在线数据合成与强化学习交汇的框架，是一个效率方向。
8. **鲁棒性研究**：当视频质量差、内容误导、搜索返回恶意或无关信息时，模型应具备抵抗和恢复能力；可在对抗性设置下探索。
9. **从研报到决策**：将 Video Deep Research 扩展到多模态决策任务，如基于视频证据的机器人操作规划、智能体在开放世界中的自主探索，这些方向是自然延伸。
需要说明的是，以上方向大部分是基于论文问题的合理推测，而非论文中给出的明确未来工作（因为原文不可及）。

Q7: 总结一下论文的主要内容

这篇论文发表于 arXiv（编号 2608.23329v2），题为“Thinking Beyond Videos: Unifying Video Reasoning and Deep Research for Open-World Video Agents”。作者团队提出了一种统一的视频深度研究（Video Deep Research）框架，名为 VideoRover，旨在解决开放世界视频理解中一个关键问题：模型既要能定位视频中稀疏的视觉证据，又要能获取视频和其参数记忆之外的外部知识。

论文的论证主线是：现有两大技术方向——Thinking-with-Videos 和 Deep Research——每个都分别解决了问题的一半，但将它们结合起来至关重要。Thinking-with-Videos 让模型通过主动的时间感知（如关键帧选择、自适应视频重采样）找到视频内的线索，却受限于视频内部知识；Deep Research 让智能体通过网页浏览、图片搜索等多步信息获取来补充背景常识，却主要处理文本和静态图像，缺乏对视频内容的动态定位。这种割裂导致开放世界视频任务（如“视频中出现的这种植物的学名和分布范围”、“判断某事件发生的背景”）无法被端到端地解决。

技术主线上，VideoRover 将视频观察与外部检索统一到一个迭代的代理决策循环中。给定一个视频-问题对，模型维护一个包含当前证据的公共状态；每一步根据状态选择一个工具动作，包括裁剪视频特定片段、用关键帧进行多模态搜索、构造文本查询或浏览网页；工具结果更新状态，然后重复这个过程，直到产生最终答案。这种设计使得视频中的局部线索能引导外部检索，反过来，检索到的外部知识又能帮助模型回到视频中对齐和验证。为了训练这种能力，作者开发了自动化数据合成流水线，生成了 26K 条经验证的 SFT 轨迹和 3K 条困难 RL 实例。同时提出了一个分层基准 VideoRover-Bench，根据视频时长和研究难度对测试进行分层。

实验主线上，论文在 VideoDR 和 VideoRover-Bench 上进行评测，主推模型为 VideoRover-8B-RL。实验结果显示：（1）在无工具使用的直接回答设定下，该模型性能与专有模型相当；（2）在启用工具的场景下，该模型优于配备相同工具集的更大规模开源模型；（3）消融研究和训练动态验证了主动视频定位、外部检索和长时程强化学习三者互补。这些结果说明，通过高质量合成数据和精心设计的强化学习，一个小得多的模型也能在视频深度研究任务上超越大模型。

总体而言，该论文的贡献包括：（1）提出统一的视频深度研究框架，将工具使用和视频理解结合为一个闭环；（2）构建自动化数据合成流水线，解决了训练数据缺乏的问题；（3）提出分层评测基准，便于分析不同长度和难度下的性能；（4）通过大规模实验展示了小模型的竞争力，验证了组件互补性。论文的局限可能在于数据合成依赖自动化验证器，真实场景中可能存在奖励噪声；视频长度和研究难度的上限可能受限于现有模型容量；且评测集中于基准，对真实开放世界应用仍需进一步验证。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中的 agent（权重 0.10）直接相关，属于多工具智能体在视频领域的应用。

## 基本信息

- 作者：Wenqi Liu, Shijie Ma, Yunxiao Wang, Meng Liu, Qile Su, Han Liu, Bohan Hou, Zeyu Wang, Xuanyu Zheng, Changyi Liu, Tianke Zhang, Haonan Fan, Kaiyu Jiang, Yingxin Li, Jiankang Chen, Xu Wang, Hongyi Fu, Jianxiong Wang, Bin Wen, Tingting Gao, Han Li, Jianhua Yin, Yinwei Wei, Xuemeng Song
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.AI
- 日期：2026-08-24
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.23329v2`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次生成主要参考了提供的最强检索证据（摘要和引言片段），并基于这些片段进行了合理推断；由于未获得完整 PDF 正文，部分细节（如具体实验数值、消融表格）未包含，也没有编造任何数字。
