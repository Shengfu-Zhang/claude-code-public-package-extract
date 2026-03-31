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

这个仓库把三个对研究公开发布包有价值的部分放在了一起：

- 原始 npm tarball
- 本地导出的源码压缩包
- 可直接浏览的 `src/` 提取结果

目标很简单：让公开发布包更容易研究和引用，同时明确这不是官方上游仓库。

## 仓库内容

| 路径 | 说明 | 适用场景 |
|------|------|----------|
| `src/` | 可浏览的 TypeScript 源码树 | 你想直接在 GitHub 上读代码 |
| `artifacts/extracted/claude-code-2.1.88-src.zip` | 提取后源码树的下载压缩包 | 你想直接下载整理好的源码结果 |
| `artifacts/original/claude-code-2.1.88.tgz` | 原始 npm 包归档 | 你想验证来源、查看原始发布物，或自己重跑提取 |
| `scripts/extract-sources.js` | 本地提取过程中使用的辅助脚本 | 你想基于本地 `cli.js.map` 重新提取 |

当前本地提取结果：

- `src/` 共 `1,902` 个文件
- 其中 `1,884` 个是 `.ts` / `.tsx`
- 提取后的 JS/TS 代码总量约 `514,587` 行
- 本地 `src/` 目录大小约 `33 MB`

说明：

- `artifacts/extracted/claude-code-2.1.88-src.zip` 只包含提取后的源码树。
- `artifacts/original/claude-code-2.1.88.tgz` 保留原始发布包，其中包含 `cli.js`、`cli.js.map` 以及 vendor/native 二进制文件，所以体积明显更大。
- 大多数人只需要 `src/` 或提取后的 zip；原始 `.tgz` 主要用于来源证明和复现。

## 为什么保留这个仓库

- 官方发布包在打包状态下不方便直接阅读。
- 把 tarball、提取结果和辅助脚本放在一起，更方便复现与交叉验证。
- 更长的架构分析和后续笔记，我会放在博客里，而不是继续把首页 README 写成超长分析文档。

## 快速使用

```bash
# 查看提取后的源码树
cd src

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
