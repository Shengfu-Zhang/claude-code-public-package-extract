# Claude Code 2.1.88 — 公共发布包源码整理

> **PS：** 后续更深入的源码分析和跟进笔记，我会持续发布到个人博客：[shengfu-zhang.github.io](https://shengfu-zhang.github.io)。如果这个 GitHub 仓库后续发生调整，长文索引也会以博客为准。
>
> **致谢：** 2026 年 3 月 31 日，Chaofan Shou（[@Fried_rice](https://x.com/Fried_rice)）在 X 上的公开帖子让更多人注意到这个包的内容；本仓库保留这份发现归因，也参考了社区已有的公开整理与写法。

> [!IMPORTANT]
> 本仓库为非官方整理版，与 Anthropic 无任何隶属关系。
> Claude Code 相关源码的知识产权仍归 Anthropic 所有。
> 本仓库仅收录从公开 npm 包 `@anthropic-ai/claude-code` `2.1.88` 中提取/整理出的文件，以及少量目录整理、路径规范化与说明文档。
> 本仓库仅供研究、参考、互操作性讨论与学习交流使用，不主张对 Anthropic 源码拥有任何权利。
> 如您是权利人并希望移除相关内容，请联系仓库维护者，我们会尽快下线处理。

**语言：** [English](README.md) | **中文**

## 仓库概览

这个仓库把四类对研究公开发布包有价值的内容放在了一起：

- 原始 npm tarball
- 本地导出的源码压缩包
- 可直接浏览的 `src/` 提取结果
- 一套便于快速导航和分析的生成文档

目标很简单：让公开发布包更容易研究和引用，同时明确这不是官方上游仓库。

## 仓库内容

| 路径 | 说明 | 适用场景 |
|------|------|----------|
| `src/` | 可浏览的 TypeScript 源码树 | 你想直接在 GitHub 上读代码 |
| `artifacts/extracted/claude-code-2.1.88-src.zip` | 提取后源码树的下载压缩包 | 你想直接下载整理好的源码结果 |
| `artifacts/original/claude-code-2.1.88.tgz` | 原始 npm 包归档 | 你想验证来源、查看原始发布物，或自己重跑提取 |
| `scripts/extract-sources.js` | 本地提取过程中使用的辅助脚本 | 你想基于本地 `cli.js.map` 重新提取 |
| `.agents/summary/` | 生成的架构与导航文档 | 你想先看结构化总览，再深入源码 |
| `AGENTS.md` | 面向编码代理的精简仓库指南 | 你想最快理解仓库的入口、目录和关键子系统 |

当前本地提取结果：

- `src/` 共 `1,902` 个文件
- 其中 `1,884` 个是 `.ts` / `.tsx`
- 提取后的 JS/TS 代码总量约 `514,587` 行
- 本地 `src/` 目录大小约 `33 MB`

说明：

- `artifacts/extracted/claude-code-2.1.88-src.zip` 只包含提取后的源码树。
- `artifacts/original/claude-code-2.1.88.tgz` 保留原始发布包，其中包含 `cli.js`、`cli.js.map` 以及 vendor/native 二进制文件，所以体积明显更大。
- 大多数人只需要 `src/` 或提取后的 zip；原始 `.tgz` 主要用于来源证明和复现。

## 生成文档

仓库现在包含一套面向人类读者和 AI 编码助手的生成文档：

- `.agents/summary/index.md`：主知识库索引
- `.agents/summary/architecture.md`、`components.md`、`interfaces.md`、`data_models.md`、`workflows.md`、`dependencies.md`：更细化的结构拆解
- `.agents/summary/review_notes.md`：提取局限、缺失项和文档审查备注
- `AGENTS.md`：位于仓库根目录的精简导航入口

如果你想最快建立整体认知，建议先看 `AGENTS.md` 或 `.agents/summary/index.md`，再进入 `src/`。

## 交互式教学控制台

仓库里还包含一个浏览器中的教学型演示，用来帮助你在不通读整棵源码树的前提下先抓住运行时结构：

- `index.html`：GitHub Pages 的根入口页
- `artifacts/demo/index.html`：`artifacts/demo/` 目录下的短入口
- `artifacts/demo/claude-code-loop-simulator.html`：英文教学控制台
- `artifacts/demo/claude-code-loop-simulator.zh.html`：中文入口

它现在展示的是一套更偏“引导式学习”的回放界面：

- 共 10 个引导演示，覆盖直接回答、递归工具调用、技能/插件、代理团队、权限拒绝、任务取消、slash command 输入拦截、压缩与续写、工具报错恢复，以及最后把所有分支串起来的完整场景
- 默认先看 `Transcript`，再看 `Inspector`，更深的调试细节会折叠在后面，不会一上来就把页面塞满
- `Source Lens` 会直接抓取仓库里的真实文件，并高亮当前那一行对应的源码接缝
- `File Guide` 会按当前场景告诉你哪些文件值得先看、哪些可以暂缓、哪些本来就缺失于提取结果
- 每个可见 payload 都会明确标出“这一步系统准备回什么”以及“为什么会这么回”

建议的使用顺序：

1. 如果这个仓库启用了 GitHub Pages，可以直接打开仓库根路径的 landing page，或者访问 `/artifacts/demo/` 这个短入口；它们现在都会通过 `index.html` 解析到正确页面。
2. 本地使用时，先在仓库根目录启动一个简单的 HTTP 服务，再打开教学控制台。比如直接运行 `python3 -m http.server 8123`。如果你是直接双击 HTML，或者通过文件浏览器用 `file://` 打开，`Source Lens` 可能会失效，因为浏览器会拦截这类实时文件读取。
3. 从 `Plain Assistant Answer` 开始，先理解最短路径。
4. 再依次看 `Intercepted Input / Slash Command`、`Tool Loop And Recursion`、`Compaction And Continuation`、`Tool Error And Recovery`，把主要接缝一条条走清楚。
5. 等前面的单条分支都看顺了，再打开 `Everything Combined` 做总复习。
6. 阅读顺序建议固定成一套动作：先看左侧 `Transcript`，再看右侧 `Inspector`，最后用 `Source Lens` 回到真实源码。

这个控制台是“回放模型”，不是实时埋点调试器。它的目标是帮助你理解结构和控制流，而不是声称每个值都来自真实捕获的会话。

为什么这样组织：

- 学习顺序本身就是架构顺序：先看可见输出，再看控制权归属，最后再去源码里核对证据
- 真正稳定的中心仍然是 `QueryEngine` 和 `queryLoop()`；终端 Ink UI 很重要，但它不是唯一也不是不可替代的外层
- 这个 demo 有意把当前 REPL 当成“可替换的一层 UX 外壳”，因为这是最容易迁移到网页、IDE、SDK 或其他前端时仍然成立的理解方式
- `Source Lens` 和 `File Guide` 的作用，就是把每一步回放重新钉回具体文件，包括像 `src/types/message.js` 这种提取结果里本来就缺失的关键缺口

## 阅读策略：先抓核心循环

如果你的目标是理解架构本身，建议先看运行时中心，再看终端 UX 外壳。

- `src/query.ts` 和 `src/QueryEngine.ts` 才是真正的重心。turn 的包装、工具调用编排、递归续跑，核心都在这里。
- `src/screens/REPL.tsx` 对交互细节很重要，但它本质上是一个可替换的界面层。无论是网页调试器、IDE 面板、headless SDK，还是别的前端，都可以复用同一套引擎，而不必复用这层 Ink UI。
- 技能、插件、权限系统、代理团队，更适合被理解为围绕同一会话引擎展开的旁路分支，而不是各自独立的一套产品。

换句话说：当前 UX 层很有价值，但不是不可替代的核心。最稳定、最可复用的心智模型应该是 `QueryEngine` 包住 `queryLoop()`，然后再往外看其它层。

教学控制台也是按这个原则设计的：先给你看回放出来的可见结果，再逐层展开 wrapper 的归属关系，最后把当前 seam 直接对回源码。终端 UI 在里面只是其中一层，不是整套架构的中心。

## 为什么保留这个仓库

- 官方发布包在打包状态下不方便直接阅读。
- 把 tarball、提取结果和辅助脚本放在一起，更方便复现与交叉验证。
- 生成文档可以显著降低阅读这类大型提取源码树时的冷启动成本。
- 更长的架构分析和后续笔记，我会放在博客里，而不是继续把首页 README 写成超长分析文档。

## 快速使用

```bash
# 查看提取后的源码树
cd src

# 查看精简导航文档
less ../AGENTS.md

# 查看生成的知识库索引
less ../.agents/summary/index.md

# 查看原始发布包
tar -xzf artifacts/original/claude-code-2.1.88.tgz

# 查看源码压缩包内容
unzip -l artifacts/extracted/claude-code-2.1.88-src.zip | head

# 基于已解包的 package/cli.js.map 重跑提取
node scripts/extract-sources.js
```

## 提取说明

这里的文件是基于公开分发的包制品在本地整理出来的。大致流程是：

1. 从公开 npm 包 `@anthropic-ai/claude-code` `2.1.88` 出发。
2. 从随包分发的 bundle / source map 数据中恢复源码路径和文件内容。
3. 对路径做最小化规范处理，整理成便于浏览的源码树。

这只是研究归档，不是官方开发仓库。某些仅限内部或受 feature gate 控制的代码，仍然可能不会出现在公开制品里。

## 这个提取版本的已知限制

如果你打算基于这个仓库阅读代码或做互操作性研究，建议先了解这些边界：

- 这不是正常的上游开发仓库。恢复出来的树中没有标准作者仓库常见的 `package.json`、lockfile 或 CI 配置。
- 部分恢复文件看起来更像转换后的产物，而不是手写源码；它们适合用来判断行为和依赖，但不适合作为代码风格范本。
- 一个核心消息类型文件 `src/types/message.js` 在很多地方被引用，但并未出现在提取结果中，因此凡是依赖精确消息结构的重构都应谨慎处理。
- 某些受 feature gate 控制、依赖内部组件、原生二进制或 vendor 资源的行为，可能只在 `src/` 中留下外围调用点，而没有完整实现细节。

## 重要声明

- 请不要把本仓库当作开源项目理解。
- 请不要将 Anthropic 的专有代码用于商业产品、再分发或其他未授权用途。
- 请不要将本仓库描述为 Anthropic 官方仓库。
- 仓库内新增的 README、目录整理和分析说明属于整理性内容，不代表对产品源码本身主张任何权利。
- 如权利人提出删除要求，维护者会尽快处理。

## 致谢

- 感谢 Anthropic 发布 Claude Code 产品及相关 npm 包。
- 感谢 Chaofan Shou（[@Fried_rice](https://x.com/Fried_rice)）在 2026 年 3 月 31 日于 X 上公开发帖，让更多人注意到该包内容。
- 感谢 Kuber Mehta 的公开仓库整理和博客文章，对社区讨论与问题梳理起到了帮助作用：
  - [仓库](https://github.com/Kuberwastaken/claude-code)
  - [博客文章](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it)
