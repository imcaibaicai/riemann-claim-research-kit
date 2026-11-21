# 黎曼猜想 AI 证明声称 · Agent 资料索引

> [中文](#黎曼猜想-ai-证明声称--agent-资料索引) / [English](#riemann-hypothesis-ai-proof-claim--agent-research-index)

2026-11-20，Anthropic 数学家 Levent Alpöge 声称与 Claude 合作证明黎曼猜想，并发布 14 卷、约 1737 万行的 Lean 4 代码。一手材料、代码、独立验证页和社区讨论散落在个人网站、MIT 页面、新闻站点与论坛里。本仓库把它们整理成一个可检索的入口集合。

## 从哪开始

- 想用 Agent 自动抓取这批资料：先读 [AGENT.md](AGENT.md)。里面有可直接执行的步骤、精确哈希，以及十个实际遇到过的故障与处理方式。
- 想了解事件全貌：读 [REPORT.md](REPORT.md)。
- 想知道代码对应哪段证明：读 [sources/PROOF_SKELETON.md](sources/PROOF_SKELETON.md)，再按 AGENT.md 的步骤下载解压，读其中的 `lean/src/Main.lean`。

## 取材方式

一手链接：

| 内容 | 地址 |
|---|---|
| Alpöge 四页说明 | `https://alpo.ge/remark.pdf` |
| Lean 源码 14 卷 | `https://alpo.ge/lean.tar.gz.part01` 至 `part14` |
| MIT Sutherland 独立验证页 | `https://math.mit.edu/~drew/grh/` |
| 源码镜像 | `https://github.com/Deicyde/riemann-hypothesis` |

源码包（239,275,554 字节，SHA256 `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc`）与 7.7GB 证明导出按 AGENT.md 的步骤获取。四页说明用 PDF 解析器提取文字层。新闻正文按 [sources/SOURCE_INDEX.md](sources/SOURCE_INDEX.md) 的 URL 直接抓取。

仓库内提供一手链接、逐字摘录、评论归档与代码依赖骨架；原始的 PDF、网页文本、源码包与证明导出按上述步骤重取。

## 目录

| 文件 | 内容 |
|---|---|
| `AGENT.md` | 获取步骤、命令、哈希、故障与处理 |
| `REPORT.md` | 事件事实、四页说明逐字要点与 20 步时间线、验证状态、资料缺口、校验输入清单 |
| `sources/SOURCE_INDEX.md` | 全部来源的 URL、抓取状态与获取方式 |
| `sources/PROOF_SKELETON.md` | 从 `Main.lean` 往下 traced 的四层依赖链、模块规模、逆推线索 |
| `sources/skeleton-defs.txt` | 骨架定理定义位置扫描结果 |
| `notes/01-claims.md` | 一手声称与文献笔记 |
| `notes/02-lean-code.md` | 代码托管、结构与下载核查 |
| `notes/03-commentary.md` | HN、MathOverflow、博客与中文社区评论归档 |
| `notes/04-backfill.md` | 补漏扫描与逆推输入整理 |

MIT 验证页记录的检查项包括：175,187 个模块编译通过、五个顶层定理仅依赖三条标准公理、多个独立内核接受导出的证明。这些数字与出处在 REPORT.md 第四节逐条列出。

## 归属

本仓库的索引、笔记与引文编目为自撰内容。所引原文、报道与代码归各自权利人所有；引用用于研究与定位，原始材料按一手链接获取。

---

# Riemann Hypothesis AI Proof Claim · Agent Research Index

On 2026-11-20, Anthropic mathematician Levent Alpöge claimed to have proven the Riemann Hypothesis in collaboration with Claude, releasing 14 volumes of Lean 4 code totaling roughly 17.37 million lines. Primary sources, the code, independent verification pages, and community discussions are scattered across personal websites, MIT pages, news outlets, and forums. This repository organizes them into a searchable collection of entry points.

## Where to Start

- To fetch this material automatically with an Agent, start with [AGENT.md](AGENT.md). It contains directly executable steps, exact hashes, and ten real-world failure cases with their workarounds.
- For the full picture of the event, read [REPORT.md](REPORT.md).
- To see which part of the proof the code corresponds to, read [sources/PROOF_SKELETON.md](sources/PROOF_SKELETON.md), then follow the steps in AGENT.md to download and extract, and read `lean/src/Main.lean` inside.

## Sourcing

Primary links:

| Content | Address |
|---|---|
| Alpöge four-page note | `https://alpo.ge/remark.pdf` |
| Lean source, 14 volumes | `https://alpo.ge/lean.tar.gz.part01` through `part14` |
| MIT Sutherland independent verification page | `https://math.mit.edu/~drew/grh/` |
| Source mirror | `https://github.com/Deicyde/riemann-hypothesis` |

The source archive (239,275,554 bytes, SHA256 `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc`) and the 7.7 GB proof export are obtained following the steps in AGENT.md. The text layer of the four-page note was extracted with a PDF parser. News article bodies were fetched directly from the URLs in [sources/SOURCE_INDEX.md](sources/SOURCE_INDEX.md).

This repository provides primary links, verbatim excerpts, commentary archives, and a code dependency skeleton; the original PDFs, web page text, source archive, and proof export are re-fetched using the steps above.

## Repository Layout

| File | Contents |
|---|---|
| `AGENT.md` | Fetch steps, commands, hashes, failures and workarounds |
| `REPORT.md` | Event facts, verbatim highlights of the four-page note with a 20-step timeline, verification status, information gaps, and a checksum input list |
| `sources/SOURCE_INDEX.md` | URLs, fetch status, and retrieval methods for every source |
| `sources/PROOF_SKELETON.md` | Four-layer dependency chain traced down from `Main.lean`, module sizes, and reverse-engineering leads |
| `sources/skeleton-defs.txt` | Scan results for skeleton theorem definition locations |
| `notes/01-claims.md` | First-party claims and literature notes |
| `notes/02-lean-code.md` | Code hosting, structure, and download verification |
| `notes/03-commentary.md` | Archived commentary from HN, MathOverflow, blogs, and Chinese communities |
| `notes/04-backfill.md` | Gap-filling scan and reverse-engineering input prep |

The checks recorded on the MIT verification page include: 175,187 modules compiled successfully, five top-level theorems depending on only three standard axioms, and multiple independent kernels accepting the exported proof. These figures and their sources are listed item by item in Section 4 of REPORT.md.

## Attribution

The index, notes, and citation catalog in this repository are original work. Quoted source texts, reports, and code remain the property of their respective rights holders; quotations are used for research and navigation, and original materials are obtained via the primary links.
