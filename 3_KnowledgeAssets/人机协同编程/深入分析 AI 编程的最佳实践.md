# 从 Cloudflare Workers OAuth Provider 学习 AI 结对编程：深入分析 AI 编程的最佳实践

最近 Cloudflare 发布了一个开源的针对 Cloudflare Workers 的 OAuth 2.1 Provider Framework[1]，有趣的是这个项目是完全和 Claude 结对编程生成的，最开始负责这个项目的工程师 Kenton Varda 是不相信 AI 是能完成这样的任务的，但是抱着试试看的心态，预计 AI 会生成一个烂代码，然而看上去代码居然很不错！当然也不是真的完美，不过只要指出问题 AI 马上就能修复，很是出人意料。

当然这并不是完全的 Vibe Coding 产物，每一个 PR 都是专业工程师认真审查后才合并的。这个项目把每一次用到的 Prompt 都放到了提交历史里面，如果你有兴趣的话可以阅读一下提交历史，就能知道他们是如何通过一个个 Prompt 从零开始搭建出来的，尤其是其中的 Prompt 有很多学习之处。

我大概把提交记录浏览了一下，一共 51 条提交记录，当然我也没有手动一条条看，其实是让 ClaudeCode 帮我分析所有提交记录，整理成一份分析报告，我再结合分析报告去看了一下原始提交记录。

最值得学习的是第一条记录，和很多新手不一样，人家的提示词并非一句简单的“帮我写个 OAuth 库”，而是一个包含了近 130 行示例代码的详尽 Prompt，Kenton 在这个“开篇故事”里，清晰地描述了他期望的 API 是如何被调用的，指定了技术栈（TypeScript、Cloudflare Workers），并明确了安全底线（例如，Token 必须哈希存储，不能存明文）。有了示例和详细说明，AI 对于要完成的任务就能很明确。

另外还有一点就是他也不是一次性生成一个完整项目，第一个版本只有一个代码文件。后续再不断迭代，就是工程师对 AI 的不断引导和修正。

当然 AI 虽然生成了一份看起来不错的代码，但是 Kenton 很快发现了问题：AI 使用了 Nodejs 版本的 crypto 库，但 Cloudflare workers 是不支持的，必须使用 WebCrypto，但好在你不需要手动修复，只要说一句：“你用了 Node.js 的 crypto 库，我们这里应该用 WebCrypto”，AI 马上就帮你修复好了！

最能体现 AI 和资深工程师能力差距的一条提交记录是在升级到 OAuth 2.1 时。AI 严格按照规范，实现了一次性的 Refresh Token。但 Kenton 指出，这种“理论完美”的设计在现实世界中可能因为网络问题导致严重的用户体验断裂。经过几轮探讨，他们最终实现了一个更稳健的方案：在旧 Token 失效前，允许新旧 Token 在一个短暂的窗口期内共存。

这完美体现了人机协作的价值：AI 负责快速实现规范，而人类专家则凭借经验，为其注入应对现实复杂性的智慧和实用主义。

AI 的局限性：那些需要“人类出手”的时刻
这其中的人和 AI 结对过程也并非一帆风顺。你可以清晰的看到 AI 的局限性：

1. 环境感知不足：除了错用 Node.js API，AI 对特定平台的特性（如 Workers KV 的 list() 功能）也需要人类提醒才能有效利用。
2. 安全细节需要明确：你需要明确告诉它“所有敏感信息都要哈希存储”，它不会主动思考所有潜在的安全隐患。
3. 偶尔的“幻觉”和固执：有好几次，Kenton 指出问题后，AI 声称已经修改，但实际上代码并没有变化，最终需要工程师手动修复。
4。** 过度工程化的倾向**：在实现加密功能时，AI 引入了一个不必要的“备份密钥”概念，增加了复杂性，被 Kenton 要求移除。
有时，对于一些简单的样式修改或代码整理，Kenton 也不得不亲自上手，因为通过 Prompt 反复纠正的效率反而更低。这提醒我们，也不要太过于依赖 AI，有时候手动修改可能还更高效！

AI 编程的最佳实践：如何更好的和 AI 结对编程
从 Cloudflare 这个项目的实践中，可以总结出几个行之有效的最佳实践：

1. 给 AI 提供丰富的初始上下文

提供代码示例、详细的技术要求、明确的设计目标，让AI精准地理解任务，而不是一个开放式问题。

明确告知 AI 平台的特性，如 Cloudflare Workers 特有的 API，防止 AI 错误地使用其他环境的技术栈。

2. 小步迭代

将复杂任务分解成小步骤，像与同事对话一样，通过多轮反馈逐步优化和纠正。

3. 指令要具体而精确

模糊的指令只会得到模糊的结果。明确指出“哪里不对”，并给出“应该怎样改”的具体方案。

4. 人机分工，各司其职

让 AI 负责繁重的代码生成和实现工作，人类则聚焦于架构设计、经验判断和最终质量的把关。

5. 知道何时亲自动手

当 AI 在简单的编辑任务上挣扎时，不要犹豫，直接手动完成。人机协作的关键是发挥各自的优势。

6. 把你的生成代码的提示词记录下来

Kenton 将每个提示词都保存在提交记录中，这创造了一种新型的文档——不仅记录了"做了什么"，还记录了"为什么这么做"。在公司内部，这也会是很好的学习素材，以后别人维护的时候看看你的提示词可能就明白为什么要这么写了。

最后
Cloudflare 的这个项目很好的说明了一个问题：AI 编程不是要取代程序员，而是将程序员提升为“架构师+指挥家”。 AI 就像是一个不知疲倦、才华横溢的乐手，而人类则负责谱写乐曲、把握节奏和最终的艺术表达。

尽管 AI 仍有局限，但它已经能胜任绝大部分代码实现工作。这种人机协作模式，将人类的设计巧思与 AI 的实现能力高效结合，可能正是未来软件开发的方向，就像我现在越来越依赖于让 AI 帮我干一些“体力活”，但会花更多精力在架构设计和代码审查上面。

AI 对软件工程的变革，才刚刚开始，你我有幸亲历其中，积极拥抱拭目以待。