---
user_id: "cheng tan"
paper_id: 15914
arxiv_id: "2610.19025v1"
title: "How Far Can Coding Agents Go in Visual Generation?"
publish_date: "2026-10-13"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Oct 2026/2610.19025v1.pdf"
pdf_url: "https://arxiv.org/pdf/2610.19025v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Oct 2026.md"
saved_at: "2026-10-18T02:00:47"
---
# How Far Can Coding Agents Go in Visual Generation?

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：coding agent · visual generation · code generation · visual language of thought

## 一句话总结

本文系统评估通用编程智能体通过可写程序进行视觉生成的能力，提出AgentDrawBench基准，发现这类智能体在结构保持与精确约束方面表现突出但在感知丰富性上有限，并提供了代码作为外部视觉思维语言及共享内部场景表征的初步证据。

## 摘要

> question answering task and find reconstructed visual code
> retains substantial information for question answering and,
> Modern image generation largely relies on specialized gen- in some domains, supports higher accuracy than the original
> erative models that leave the process of image construc- image. These abilities raise a further question: do agents
> tion implicit in learned parameters. We explore an alterna- draw on a shared internal representation of scene content be-
> tive: general-purpose coding agents that construct images yond a particular drawing program? We explore an internal
> through executable programs. Our study of zero-shot draw- visual language-of-thought hypothesis, in which a shared,
> ing on AgentDrawBench, a 200-prompt benchmark, reveals compositionally structured representation supports visual
> operations across drawing languages. Cross-harness draw-arXiv:2610.19025v1 a distinctive capability profile. Agents excel at preserving
> structure and satisfying precise visual constraints, achieving ings and images assembled from independent single-pixel
> competitive results in domains where compositional accu- queries exhibit related layouts and tones. Agents also main-
> racy matters more than photorealistic appearance, while tain self-generated room arrangements across viewpoints
> remaining limited in perceptual richness. Their visual ex- after visible drawings are removed. These results demon-
> pressiveness varies with the drawing interface, and a quali- strate that the coding agent would maintain a consistent and
> tative study on reconstruction shows how procedural skills coherent world inside its imagine even before the harness
> extracted from human demonstrations support different artis- gets involved and the generation starts.
> tic media. We then examine code as an external visual lan-
> guage of thought: a structured, executable representation
> that agents can construct and inspect. We test it in visual

Q1: 这篇论文试图解决什么问题？

1. 现有生成范式的局限：现代图像生成主要依赖扩散模型、GAN等专用生成模型，其构造过程隐含在高维参数中，缺乏显式的组合逻辑，难以精确控制、验证和解释。
2. 替代方案的动机：编程智能体通过生成可执行代码（如绘图指令）来构建图像，将图像生成转化为程序合成问题，使得生成过程透明、可审查、可迭代。
3. 核心研究问题：编码智能体到底能在视觉生成上走多远？其能力边界在哪里？特别是它们在结构保持和精确约束方面的优势是否可靠，以及在感知丰富性上的短板是否固有。
4. 更深层问题：智能体在不同绘图程序（如Python的turtle、SVG、Canvas等）之间操作时，是否依赖某种与具体程序无关的共享内部场景表示？即是否存在一种“视觉思维语言”，使得场景对象、空间关系、布局等信息能以组合结构的方式被内部编码，而非绑定在某一种绘图API上。
5. 实际意义：如果代码能作为外部视觉思维语言，那么我们可以利用它来探测和理解智能体的内部世界，甚至让智能体以代码形式进行推理和规划；如果存在共享内部表征，则可能支撑跨媒介、跨视角的一致生成。

Q2: 有哪些相关研究？

1. 程序合成与代码生成：已有大量工作研究如何让大语言模型生成可执行程序（如Codex、AlphaCode），但多数聚焦于算法或通用代码，而非视觉图像生成。
2. 视觉编程：早期有使用Logo/turtle等教育语言进行图形绘制，近期有一些工作让LLM生成SVG或Python绘图代码，但缺少系统性基准和深入能力分析。
3. 视觉推理与代码：一些研究将视觉问答转换到代码执行或程序表示，以利用代码的结构化优势；本文的视觉问答实验与此方向一致。
4. 思维语言与“思维链”：思维链（Chain-of-Thought）证明语言可以外化推理过程；本文提出的“外部视觉思维语言”可视为将代码作为视觉推理的外部载体，而“内部视觉思维语言”则类似于认知科学中的心理表象。
5. 跨模态一致性：多模态模型常在空间布局、数量、关系等属性上不可靠；本文通过单像素查询、跨harness一致等实验，间接探测智能体内部表征的稳定性。
6. 需要说明的是，摘要未提供具体参考文献，以上相关方向是基于该论文主题的合理推断，具体关联需查阅全文。

Q3: 论文如何解决这个问题？

1. 提出AgentDrawBench：一个包含200个多样化提示的基准，覆盖需要结构保持、精确约束、组合布局以及一定感知细节的场景，用于零样本评估编码智能体的绘图能力。
2. 零样本绘图协议：不给智能体任何微调或示例，直接让其生成代码来满足提示，考察其现有机能。
3. 多界面比较：在不同绘图harness（如不同图形API或程序环境）下进行绘制，分析界面如何影响表现力与结构保持。
4. 重建研究：从人类演示中提取程序性技能，用于不同艺术媒介的图像重建，定性分析这些技能的可迁移性。
5. 外部视觉思维语言测试：让智能体生成可执行代码构建图像，然后从代码重建视觉表示，将其用于视觉问答任务，比较与原始图像的性能。
6. 内部共享表征实验：设计多个实验——跨harness生成同一场景的绘图并比较布局/色调；用独立单像素查询组装图像并观察其与整体绘图的相关性；让智能体生成房间布局后移除可见绘制，再从不同视角重新绘制，检验其是否维持内部一致的场景记忆。

Q4: 论文做了哪些实验？

1. 零样本绘图评估：在AgentDrawBench的200个提示上，让编码智能体生成绘制代码，评估其在结构保持、约束满足、组合准确性和感知丰富性等维度上的表现。
2. 界面/程序变量实验：对比在不同绘图接口或程序库（如turtle、SVG、matplotlib、HTML canvas等）下生成的图像，考察表现力差异。
3. 技能重建实验：选取人类绘制的示范，提取其程序化技能，并迁移到不同艺术媒介，定性观察重建效果。
4. 视觉问答实验：对同一场景，分别使用原始图像和从生成代码重建的图像作为视觉问答输入，比较回答准确率。
5. 跨harness一致性实验：用不同绘图harness为同一提示生成图像，检查布局和色调的相似性。
6. 单像素查询实验：独立地请求智能体生成单个像素的颜色/位置等信息，再将所有像素组装成完整图像，观察组装图与直接绘图是否在布局、色调上相关。
7. 场景记忆实验：先让智能体创建一个房间场景，移除所有可见绘制输出，然后要求其从不同视角重新生成该房间，检验跨视角一致性。
需要说明：具体实验规模、统计方法、baseline等细节摘要未提供，需查看正文。

Q5: 发现了什么实验现象？

1. 能力分布特点：智能体在结构保持和满足精确视觉约束上表现出色；在需要组合准确性的领域（如图表、图样、几何布局）能匹敌或超过专用模型，但在照片级真实感和细腻纹理等感知丰富维度上明显不足。
2. 界面影响：视觉表现力随绘图接口变化，说明智能体的输出质量部分受工具表达力约束，而非完全由内部能力决定。
3. 代码作为外部思维语言的有效性：从生成代码重建的视觉表示保留了大量问答所需信息；在部分领域其问答准确率甚至高于直接用原始图像，提示代码表示可能去除了无关噪声、突出了语义结构。
4. 内部表征一致性的证据：跨harness绘制的同一场景在布局和色调上相关；由独立单像素查询组装出的图像与整体直接绘制也表现出相关性；移除可见绘制后，智能体仍能跨视角保持房间布局的一致性。
5. 这些现象支持“内部视觉思维语言”假说：智能体在具体绘图开始前，已经存在一种与程序无关的场景级表征，该表征具有组合结构并支持跨语言、跨视角的视觉操作。

Q6: 有什么可以进一步探索的点？

1. 扩大基准规模和多样性：从200提示扩展到涵盖3D、动态、交互式场景的更大基准，并加入更多衡量感知丰富性的指标。
2. 提升感知丰富性：探索将代码生成与扩散模型/渲染器结合，在保持结构化控制的同时获得高保真纹理和光影。
3. 机制研究：利用可解释性方法，更直接地探测“内部视觉思维语言”的实现，而不是仅通过行为实验间接推断。
4. 跨媒介迁移：将内部场景表征迁移到视频、3D、虚拟环境等更复杂生成任务中，验证其通用性。
5. 应用探索：将代码作为可检查的中间表示，用于人机协作创作、可解释的视觉推理、程序化内容生成（游戏关卡、建筑布局等）。
6. 与其他智能体能力结合：将视觉生成代码与规划、工具使用、自我修正相结合，构建更强的多模态智能体。
7. 理论建模：为“外部/内部视觉思维语言”建立形式化定义和评估协议，推动该方向成为可量化的研究范式。

Q7: 总结一下论文的主要内容

本文站在专用生成模型（如扩散模型）的对立面，提出并系统考察了“编码智能体即视觉生成器”的新范式。作者首先指出现有生成模型把图像构造过程隐藏在学到的参数中，导致生成结果难以精确控制、验证和解释。为克服这一局限，他们让通用编码智能体通过编写可执行程序来绘制图像，将视觉生成转化为程序合成问题。为了科学评估这种能力，作者构建了包含200个提示的AgentDrawBench基准，覆盖从简单图案到复杂场景的任务，并在零样本条件下测试智能体的绘图表现。

研究发现了一个清晰的“能力轮廓”：编码智能体非常擅长保持结构和满足精确的视觉约束，在组合准确性比照片真实感更重要的场景（如几何图形、图表、符号布局）中能取得有竞争力的结果；但在感知丰富性——细腻纹理、光影、复杂材质——上明显薄弱。此外，智能体的视觉表现力会随绘图界面发生变化，说明所使用的图形程序本身是生成质量的重要变量。一项定性重建实验进一步表明，从人类绘画演示中提取出的程序性技能可以被迁移到不同的艺术媒介，提示这些技能具有一定程度的通用性。

论文随后将焦点从“智能体能否用代码生成图像”提升到“代码能否作为视觉思维的语言”。他们把生成代码视为一种外部视觉思维语言：一种结构化的、可执行且可检查的场景表示。通过在视觉问答任务中比较原始图像与由代码重建的图像，他们发现重建视觉代码保留了充分的场景信息，在部分领域甚至取得比原始图像更高的回答准确率。这一结果暗示代码可能是一种更紧凑、更具语义结构的图像表示。

更进一步，作者追问：智能体在不同的绘图程序之间切换时，是否依赖某个与具体程序无关的共享内部场景模型？他们提出“内部视觉思维语言”假说，并设计了三类行为实验来检验：1）跨harness绘制同一场景，比较生成图之间的布局和色调；2）让智能体以独立单像素查询的方式构建图像，然后与整体绘制结果比较；3）先让智能体生成一个房间布局，随后移除所有外部可见绘制，再从不同视角重新绘制。三类实验均得到支持性证据：跨harness绘图在布局和色调上相关，单像素组装图与直接绘制的图像存在相关关系，且智能体在移除外部提示后仍能跨视角保持空间一致性。

总体而言，论文通过行为实验论证了编码智能体在开始绘制之前，其“内心”已经构建了一个一致、连贯的场景表征；代码既是这种表征的外部投影，也是探针。这项工作为视觉生成提供了一个不同于扩散模型的可解释、可控制的路线，并对智能体内部世界的一致性提出了可操作的实验范式。不过，论文也明确指出当前编码智能体的感知丰富性不足，且该结论建立在200个提示的小型基准和间接行为证据之上，未来仍需要更大规模、更直接的研究来巩固和完善这些发现。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：直接聚焦于agent在视觉生成任务中的能力，与你的智能体研究方向（权重0.10）相关。

## 基本信息

- 作者：Hengguang Zhou, Yuanhao Ban, Jiaqi Feng, Xiaohuan Pei, Justin Cui, Cho-Jui Hsieh
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.CV, cs.LG
- 日期：2026-10-13
- 推荐级别：**推荐阅读**
- 解析来源：PDF 全文
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2610.19025v1`
- 解析说明：本报告已结合 PDF 全文结构做自动提炼。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本次分析主要依据论文摘要和语义检索到的摘要片段生成，未获得完整正文；部分相关工作和数值细节基于合理推断，需查证原文。
