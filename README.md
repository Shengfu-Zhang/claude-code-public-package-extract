# Claude Code 2.1.88 — Public Package Source Extract

> **PS:** I will publish deeper analysis and follow-up notes on my blog: [shengfu-zhang.github.io](https://shengfu-zhang.github.io). If this GitHub repo changes later, that site is where I plan to keep the long-form writeups indexed.
>
> **Attribution:** Public rediscovery credit goes to Chaofan Shou ([@Fried_rice](https://x.com/Fried_rice)) on X on March 31, 2026. This README also keeps visible credit to the broader public writeups that documented the package contents.

> [!IMPORTANT]
> This repository is unofficial and is not affiliated with Anthropic.
> The underlying Claude Code source remains Anthropic's proprietary work.
> This repo only preserves files extracted from the public npm package `@anthropic-ai/claude-code` version `2.1.88`, plus light organization, path cleanup, and documentation.
> It is shared for research, reference, interoperability discussion, and education only.
> No ownership is claimed over Anthropic's code. If you are a rights holder and want any material removed, please contact the repository owner and it will be taken down promptly.

**Language:** **English** | [中文](README_ZH.md)

## Overview

This repository keeps together three things that are useful for studying the public Claude Code package:

- the original npm tarball
- a locally extracted source archive
- a browsable `src/` tree reconstructed from the distributed package artifacts

The goal is simple: make the public package easier to inspect without pretending this is an official upstream repository.

## Repository Contents

| Path | Description | Use it when |
|------|-------------|-------------|
| `src/` | Browsable extracted TypeScript source tree | You want to read code directly on GitHub |
| `artifacts/extracted/claude-code-2.1.88-src.zip` | Downloadable zip of the extracted source tree | You want the extracted tree locally without rebuilding it |
| `artifacts/original/claude-code-2.1.88.tgz` | Original npm package archive | You want provenance, the published bundle, or to reproduce extraction |
| `scripts/extract-sources.js` | Local helper script used during extraction work | You want to rerun extraction from a local `cli.js.map` |

Current local extract:

- `src/` contains `1,902` files
- `1,884` files are `.ts` / `.tsx`
- the extracted JS/TS tree is about `514,587` lines
- the local `src/` directory is about `33 MB`

Notes:

- `artifacts/extracted/claude-code-2.1.88-src.zip` contains only the extracted source tree.
- `artifacts/original/claude-code-2.1.88.tgz` contains the original bundled package, including `cli.js`, `cli.js.map`, and vendor/native binaries, so it is much larger.
- Most people only need `src/` or the extracted zip; the original `.tgz` is mainly for provenance and reproducibility.

## Why This Repo Exists

- The published package is much harder to inspect in bundled form.
- Keeping the tarball, extracted tree, and helper script together makes analysis more reproducible.
- Longer architecture notes and follow-up analysis will live on my blog instead of turning this front page into a giant dump.

## Quick Use

```bash
# inspect the extracted source tree
cd src

# inspect the original published package
tar -xzf artifacts/original/claude-code-2.1.88.tgz

# inspect the zipped extract
unzip -l artifacts/extracted/claude-code-2.1.88-src.zip | head

# rerun extraction from an unpacked package/cli.js.map
node scripts/extract-sources.js
```

## Extraction Notes

The files here were reconstructed locally from publicly distributed package artifacts. In practice, that means:

1. Start from the public npm package `@anthropic-ai/claude-code` `2.1.88`.
2. Recover source paths and file contents from the shipped bundle and source map data.
3. Normalize the extracted paths into a regular source tree for browsing.

This is a research archive, not an official development checkout. Some internal-only or feature-gated code may still be absent from public artifacts.

## Important Disclaimer

- Do not treat this repository as open source.
- Do not reuse Anthropic's proprietary code in commercial products or closed redistributions.
- Do not represent this archive as an official Anthropic repository.
- The added README text, organization, and commentary are original editorial work; the product code itself is not.
- Rights-holder removal requests will be honored promptly.

## Credits

- Anthropic for the original Claude Code product and publicly distributed npm package.
- Chaofan Shou ([@Fried_rice](https://x.com/Fried_rice)) for the March 31, 2026 X posts that drew renewed attention to the package contents.
- Kuber Mehta for an early public repo writeup and blog post that helped document the issue and wider discussion:
  - [Repository](https://github.com/Kuberwastaken/claude-code)
  - [Blog post](https://kuber.studio/blog/AI/Claude-Code's-Entire-Source-Code-Got-Leaked-via-a-Sourcemap-in-npm,-Let's-Talk-About-it)
