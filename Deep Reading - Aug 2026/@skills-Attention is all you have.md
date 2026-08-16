---
user_id: "cheng tan"
paper_id: 7647
arxiv_id: "2608.12610v1"
title: "@skills: Attention is all you have"
publish_date: "2026-08-12"
pdf_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/arXiv - Aug 2026/2608.12610v1.pdf"
pdf_url: "https://arxiv.org/pdf/2608.12610v1"
generation_provider: "openai-compatible"
generation_model: "deepseek-v4-flash"
response_language: "zh"
report_version: "2026-06-17-v6"
daily_note_path: "/Users/mario/Downloads/Projects/PaperFlow/data/exports/Daily Note 2026/Daily Note - Aug 2026.md"
saved_at: "2026-08-14T11:51:16"
---
# @skills: Attention is all you have

> ★★★★☆ 推荐阅读 · 模型 openai-compatible/deepseek-v4-flash

🏷 关键词：agent skills · skil.md · prompt engineering · agent tooling

## 一句话总结

本文提出 @skills 开放协议，将技能安装（install）所捆绑的内容、持久化与自动触发三件事拆开，用文件路径即用即取的方式解决系统提示词槽位稀缺导致的技能长尾无法被使用的问题。

## 摘要

> Agent skills package procedural knowledge as SKILL.md files: 56,804 are published today, and teams write many more privately. The dominant way to deliver one is to install it, after which its description sits permanently in the system prompt, competing for fewer than a hundred reliable slots in which the model may or may not match it to a request. So the long tail has no path to use, and a team's own playbooks compete with everything else for the same scarce room. Our observation is that installing bundles three separable functions—content, persistence, and auto-triggering—and only the last needs the prompt. We propose @skills, an open protocol that separates them. A path addresses any skill, any subtree, or a whole collection, and reading it is using it, so nothing installs and nothing becomes resident; :save vendors a copy at that same path into the project's git-tracked tree, to adapt and own it; :install adds one .gitignore-style line, which is the only thing that costs prompt residency. A directory is a menu, so a “bundle” is just a directory and all-or-nothing delivery does not exist. There is no manifest, lockfile, or registration, because a file tree needs none, and SKILL.md is unchanged. The protocol is additive, ships as an installable package with an open specification (https://github.com/SylphAI-Inc/atskills), and turns any agent that can read files and run commands into a client from a single instruction file. The protocol is implemented in the AdaL CLI (https://adalagent.ai). Because a file tree addresses skills well but cannot find them, the protocol also pairs with a free agent skills hub (https://atskills.one) confined to the jobs paths cannot do: search and ranking across the whole public corpus, hosting for skills that have no repository, private and team collections, and one-screen authoring for the non-developers whose procedural knowledge skills capture best. The hub is a service and never a requirement—gh: and local paths resolve with no hub involvement, and GitHub-hosted skills keep their gh: identity even when the hub indexes them. Install less, use more.
> Keywords AI agents · agent skills · prompt engineering · context management · tool use · open protocols

Q1: 这篇论文试图解决什么问题？

1. **核心问题**：现代 coding agent 以 SKILL.md 技能包封装程序性知识，但唯一主流的交付方式“安装”会让技能描述永久驻留系统提示词。系统提示词中可靠槽位不到一百个，技能数量却成千上万，导致绝大多数长尾技能根本没有被使用的路径。
2. **次要问题**：安装把“内容”“持久化”“自动触发”三件事捆绑在一起，其中只有自动触发真正需要提示词驻留，捆绑使得本可轻量使用的技能被迫占用稀缺资源。
3. **生态问题**：团队自有的 playbook 与公开技能、第三方技能竞争同一批提示词槽位；跨 75 个 agent 的社区 skills CLI 中，同一格式有 54 个不同的项目级位置，技能安装位置碎片化，进一步加剧了“装在哪、是否生效、是否冲突”的混乱。
4. **设计张力**：文件树擅长寻址（address）但不擅长发现（find），因此协议还需要一个 hub 补充搜索与排名能力；但 hub 不能成为依赖，否则会破坏协议的去中心化。
5. **隐含假设**：论文假设当前技能的瓶颈不是模型能力，而是提示词中的描述槽位稀缺；其论据之一是相关工作中引用的结论——coding agent 在 repo 级场景下，周边结构而非原始模型能力驱动性能。

Q2: 有哪些相关研究？

1. **Agent Skills 生态**：Anthropic 引入 Agent Skills 格式，将能力封装为 SKILL.md 文件，安装后描述进入系统提示词，相关文件按需加载；本文将该原则从单个已安装技能内部扩展到整个生态的交付层。
2. **相关工作中的性能结论**：论文引用 [60,61] 指出，coding agent 在 repo 级设置中，周围结构（上下文工程、仓库结构）而非原始模型能力主导性能，这为“提示词槽位是瓶颈”提供了依据。
3. **技能的三类角色**：论文将技能内容分为领域知识（专业知识蒸馏为指令）、提供方集成（服务教会任何 agent 驱动其产品）、团队工作流（团队定制的流程）；这三类角色的交付需求不同，统一使用安装机制并不合适。
4. **现有安装机制的碎片化**：论文在背景部分指出，Agent 通过将技能复制到 .claude/skills/、.windsurf/skills/、.adal/skills/ 等 agent 专属目录来采用技能；75 个 agent 的社区 CLI 支持 54 个不同的项目级位置，说明格式统一但安装路径不统一。
5. **与文件系统的关系**：本文方案本质是“把技能分发回归文件系统”，这与传统包管理器（manifest、lockfile、注册中心）形成对比，也与当前 repo 级 agent 的上下文工程趋势一致。

Q3: 论文如何解决这个问题？

1. **核心观察**：安装（install）捆绑了三件事——内容（文件本身）、持久化（安装后长期存在于环境中）、自动触发（描述进入系统提示词以便模型知道何时使用）。只有自动触发必须占用提示词槽位。
2. **协议设计**：@skills 协议将三者分离：
 - **路径即使用**：任意路径（本地路径、gh: 远程路径）都可寻址一个技能、一个子树或整个集合；agent 读取该路径即视为使用该技能，不需要安装，不占用提示词。
 - **:save**：将远程路径对应的技能复制一份到项目 git 跟踪树中的同一路径，以便团队适配和自主维护。
 - **:install**：只向配置文件添加一行 .gitignore 风格的条目，这是唯一产生提示词驻留的行为；其作用是让该技能自动触发。
3. **目录即菜单**：一个目录天然是菜单，因此“技能包”就是目录，不存在“全有或全无”的打包交付；用户可按需寻址子目录。
4. **去除元数据负担**：不需要 manifest、lockfile 或注册中心，因为文件树本身无需这些；SKILL.md 文件格式保持不变，协议是增量的，可叠加在现有生态之上。
5. **客户端兼容性**：协议以单个指令文件形式工作，任何能读文件和执行命令的 agent 都能成为客户端，不限定特定 agent 厂商。
6. **配套 hub**：由于文件树善于寻址但不善于发现，协议配套一个免费技能中心（atskills.one），负责路径无法完成的工作：公共语料库的搜索与排名、为没有仓库的技能提供托管、私有/团队集合、面向非开发者的单屏创作。hub 是服务而非必需，gh: 和本地路径无需 hub 即可解析。
7. **实现**：协议在 AdaL CLI 中实现，开放规范地址为 https://github.com/SylphAI-Inc/atskills。

Q4: 论文做了哪些实验？

1. **现有证据缺口**：检索到的证据中几乎没有传统意义上的实验部分（无量化评测、无 baseline 对比的细节）。论文核心是协议设计与生态分析，实验性内容可能集中在生态统计与 CLI 实现的可用性展示，但证据不足。
2. **可推断的验证方式**：合理推断论文会通过小规模用户测试或示例场景展示协议用法，例如展示同一技能通过路径读取与通过安装触发的差异；也可能统计提示词槽位占用变化。但这些细节在当前证据中未出现，需要回原文确认。
3. **生态数据**：摘要给出 56,804 个公开技能的数量；背景部分提到 75 个 agent 的社区 CLI 支持 54 个不同的项目级位置。这些属于生态测量而非受控实验。
4. **建议**：若读者需要实验细节，应查阅原文可能有实验/评估章节；本字段依据当前检索证据只能记录以上内容。

Q5: 发现了什么实验现象？

1. **生态统计观察**：公开技能已达 56,804 个，且团队私有技能更多，说明技能数量远超系统提示词可容纳的百级槽位，长尾问题真实存在。
2. **安装路径碎片化**：75 个 agent 中有 54 个不同项目级技能位置，说明同一 SKILL.md 格式缺乏统一交付语义，安装机制随 agent 厂商而异。
3. **技能角色分化**：技能内容承担三类角色（领域知识、提供方集成、团队工作流），不同角色的更新频率、来源可信度和使用方式差异大，统一安装机制不合理。
4. **性能与结构的关系**：相关工作中引用 [60,61] 支持“repo 级结构与上下文工程比原始模型能力更影响性能”的观察，这为把提示词槽位让位给更关键信息提供了动机。
5. **负结果/张力**：无明显负结果；可推测的张力是“路径即使用”可能牺牲自动触发的便利性，因为模型需要额外指令才能知道何时读取路径；本文通过保留 :install 一行配置来缓解。
6. **反直觉点**：传统软件工程强调安装、依赖锁定、注册中心，本协议反其道而行，声称文件树天然不需要这些；这是对包管理范式的挑战。

Q6: 有什么可以进一步探索的点？

1. **提示词槽位动态分配**：是否可以把“自动触发”做成运行时按请求动态选择，而非静态驻留；@skills 的一行 install 是静态近似，可探索基于检索的即时触发。
2. **技能发现与排名**：hub 的搜索/排名算法如何设计，如何避免中心化带来的审查或排名偏置；能否用联邦或 P2P 方式补充。
3. **技能版本与演化**：路径寻址如何追踪更新？:save 的 vendor 机制与上游更新如何同步，是否需要类似依赖更新的工具。
4. **安全与供应链**：可执行命令的 agent 读取任意路径技能，如何验证技能来源、内容完整性、恶意指令；SKILL.md 本质是 prompt injection 面，需要安全研究。
5. **跨 agent 标准化**：能否推动 54 个安装位置收敛为统一语义；@skills 协议是否会被各 agent 厂商采纳。
6. **非开发者创作**：hub 单屏创作面向非开发者，但程序性知识的形式化仍是挑战；如何自动把人类流程转化为可执行的 SKILL.md。
7. **与上下文工程结合**：repo 级结构被证明影响性能，技能路径作为仓库结构的一部分可能成为新的上下文工程原语；可研究技能引用对模型推理的直接影响。
8. **评测方法学**：目前缺少量化实验，未来可设计对比实验：同一任务集合下，安装式交付 vs @skills 路径式交付在槽位占用、任务成功率、延迟上的差异。

Q7: 总结一下论文的主要内容

论文《@skills: Attention is all you have》针对 coding agent 技能生态的核心矛盾——技能数量爆炸（公开 56,804 个）与系统提示词可靠槽位不足（少于一百个）——提出一个开放协议 @skills。其论证主线是：安装（install）这个单一操作错误地捆绑了内容、持久化和自动触发三个可分离功能；只有自动触发需要提示词驻留，而内容与持久化完全可以交由文件系统处理。协议因此设计为：路径即使用（读取即生效），:save 用于把远程技能复制进项目 git 树以便适配，:install 只添加一行 .gitignore 风格配置作为自动触发的唯一代价。目录即菜单，所以不存在全有或全无的捆绑交付；无 manifest、无 lockfile、无注册中心，因为文件树不需要这些。协议以单个指令文件工作，使任何能读文件、能执行命令的 agent 都能成为客户端，具有很强的增量兼容性。论文同时承认文件树“擅长寻址但不擅长发现”，因此配套一个免费 hub（atskills.one）负责搜索、排名、托管、私有/团队集合和面向非开发者的创作，但 hub 只是可选项，gh: 与本地路径无需 hub。技术主线上，协议本身不改变 SKILL.md 格式，只是改变交付语义，并通过 AdaL CLI 实现；相关背景分析了现有生态的碎片化（75 个 agent 有 54 个技能目录位置）以及技能的三种角色（领域知识、提供方集成、团队工作流），并引用 repo 级结构驱动性能的结论支撑其动机。实验主线在当前检索证据中不完整，缺少量化评测细节；论文更像一种生态协议提案而非实验论文。总体而言，本文贡献在于提出并开源了一个极简、增量、去中心化的技能交付新范式，挑战了“安装”作为唯一交付方式的默认假设，并为后续 agent 技能生态研究提供了新方向。

## 推荐指数

★★★★☆（4/5）
- 推荐理由：与用户画像中 agent 方向直接相关（权重 0.10），属于 agent 基础设施/工具链类别。

## 基本信息

- 作者：Li Yin, Zhi Li, Zhan Shi, Haoran Zhang, Haebin Seong, Zhangyang, Wang
- 机构：未提供
- 来源：arxiv
- 主题/分类：cs.AI
- 日期：2026-08-12
- 推荐级别：**推荐阅读**
- 解析来源：摘要 + 元数据
- 生成模型：openai-compatible / deepseek-v4-flash
- arXiv ID：`2608.12610v1`
- 解析说明：本报告当前基于摘要和元数据自动生成，方法与实验细节建议回到原文核对。 已结合全文切块语义检索证据生成。 当前精读正文已将用户兴趣 embedding 检索链路作为主要证据排序信号之一。 本生成主要依据摘要与检索到的 Introduction/Related Work/Background 片段；由于原文实验部分证据缺失，实验相关字段基于有限信息推断并已标注，建议结合原文进一步确认。
