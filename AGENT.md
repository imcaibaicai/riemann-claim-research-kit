# Agent 获取手册：黎曼猜想 AI 证明声称

> 目的：让另一个 Agent 直接拿到一手材料并定位到代码入口，少走联网乱搜与反复报错的弯路。所有 URL 与数值均经实际抓取核对（2026-11-21）。

## 0. 快速事实

| 项 | 值 |
|---|---|
| 事件 | 2026-11-20，Anthropic 数学家 Levent Alpöge 在个人网站发布四页说明，声称与 Claude 合作证明黎曼猜想与 GRH |
| 一手声称 | `https://alpo.ge/remark.pdf` |
| 代码分卷 | `https://alpo.ge/lean.tar.gz.part01` 至 `part14` |
| 独立验证页 | `https://math.mit.edu/~drew/grh/` |
| 代码镜像 | `https://github.com/Deicyde/riemann-hypothesis` |
| 拼回包大小 | 239,275,554 字节 |
| 拼回包 SHA256 | `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc` |
| 解压后规模 | 175,187 个 `.lean` 文件；17,374,215 行 |
| 顶层定理 | 5 个；仅依赖 `propext`、`Classical.choice`、`Quot.sound` |
| 数学主线 | Stepanov 论证 + theta lift 链 + 链长 √X（不是 Nyman–Beurling） |

## 1. 获取步骤（可直接执行）

以下命令在任意空目录下执行，原始抓取物按第 1 至 5 步重新获取。命令为 bash／WSL／Git Bash 语法；Windows 原生 shell 用等价 cmdlet 或 Python 实现，拼包哈希可用 `Get-FileHash lean.tar.gz -Algorithm SHA256`。

**第 1 步：四页说明**

```
curl -L -o remark.pdf https://alpo.ge/remark.pdf
```

PDF 约 245KB。文字提取用 `pdfminer.six` 或 `pdfplumber`（需 `pip install`）。提取正文 12,222 字符，含声称原文、20 步时间线、14 卷下载地址。

**第 2 步：14 卷代码**

```
for i in $(seq -w 1 14); do curl -L -O https://alpo.ge/lean.tar.gz.part$i; done
```

part01 至 part13 各 18,000,000 字节，part14 只有 **5,275,554** 字节（见坑 1）。

**第 3 步：校验并拼包**

```
cat lean.tar.gz.part* > lean.tar.gz
sha256sum lean.tar.gz
# 期望：9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc
```

**第 4 步：解包**

```
tar -xzf lean.tar.gz
```

175,190 个成员，解压约 1.03GB。Windows 上一次性解包 17.5 万个小文件会超过 5 分钟，需要按「目标不存在或大小不符才解」的规则分批续解（见第 2 节）。

**第 5 步：MIT 独立验证材料**

```
curl -L -o challenge.ndjson.zst https://math.mit.edu/~drew/grh/challenge.ndjson.zst
```

challenge 47MB，SHA256 `c24ef99dab3556a52c7fb886e03cf22a242f3b96f82ae4cc40539e5d2f678ac6`。同页还有 `SHA256SUMS`、`audit.log`、`Audit.lean`、`comparator.json`、`nanoda.log`、`con-leche-master.log`、`con-leche-compare.log`、`project-oleans.sha256`，体量都很小，建议一并取。

**第 6 步：读代码入口，按此顺序**

1. `lean/src/Main.lean`（12 行，5 个顶层定理）
2. `lean/src/Statements.lean`（16 行，4 个自定义命题）
3. `lean/src/Supplements/Kernels/MainTheorems.lean`（`rhOfAll`、`allChi`、`critAll`、`claimAll`）
4. `lean/src/Formalism/GammaFactorNonvanishing/CritLine.lean`（经典归约，32 行，可直接通读）
5. `lean/src/Development/AutomorphicArithmetic/CharacterBoxes/MainConclusion.lean`
6. `lean/src/Development/AutomorphicArithmetic/Foundations/GaloisRepresentations/IntegralStructures/RowSrc.lean`（核心入口 `galoisLocalGlobalStrong_recParameter`）

完整四层依赖链见 `sources/PROOF_SKELETON.md`。

## 2. 故障与处理

| 坑 | 症状 | 原因 | 正确做法 |
|---|---|---|---|
| part14 重复追加 | 续传后 part14 变成 21,102,216 字节，包总长 255,102,216 | 服务器每次响应只给 5,275,554 字节，Range 请求被忽略；朴素续传会把同一段追加 4 次 | 按官方总长 239,275,554 截断 part14，重拼后校验 SHA256 |
| gzip 忽略尾部垃圾 | 拼错的包照样能 `tar -tzf` 列出全部成员 | gzip 流在第一个结束标记处停止，后面重复字节被忽略 | 以 SHA256 校验为准，能解包不代表拼包正确 |
| Windows 解包超时 | 一次性 `extractall` 在 17.5 万文件上超过 300 秒被中断 | 小文件创建开销，不是压缩包损坏 | 按「目标不存在或大小不符才解」的规则续解（用 Python tarfile 遍历 member 逐个 extract 即可），剩余 24,124 个文件约 84 秒完成 |
| 站点 403 | Zenodo、Science、Aaronson 博客、知乎正文抓不到 | 反爬或区域限制 | 反复重试通常无效。用搜索摘要 + `notes/` 里注明出处的逐字转述；有替代镜像就走镜像 |
| 误传概念 | 早期二手报道说用 Nyman–Beurling 判据、3 条公理、Sutherland 当天编译 | 前两项是误传，第三项当时未证实 | 一手 PDF 写的是 Stepanov/theta lift；3 公理由 MIT `audit.log` 证实；Sutherland 由 MIT 验证页证实 |
| 两个项目混淆 | 把 Zenodo「The Cathedral」（约 15.8 万行、一个等价于 RH 的公理）当成同一证明 | 规模差两个数量级、公理口径相反、路线不同，关系未证实 | 分开处理，两者不能互相作为证据 |
| 大文件硬传 GitHub | 239MB 的 tar 无法推送 | GitHub 单文件 100MB 硬限；LFS 免费额度 10GB 存储 + 每月 10GB 流量 | 只发布索引与笔记，代码从 `alpo.ge` 直取 |
| GitHub API TLS 失败 | 退出码 13，`Client network socket disconnected before secure TLS connection was established` | 代理 CONNECT 返回 200，但 TLS 握手被断（`schannel: failed to receive handshake`） | 与凭据无关，属网络层 TLS 握手失败。网络恢复后再推 |
| 7.7GB 导出 | `solution.ndjson.zst` 解压 61.5GB，官方说需约 100GB 内存、70GB 磁盘 | 内核检查是 CPU + 内存 + 磁盘活 | 内存和磁盘有限的环境先取 47MB 的 challenge 跑通 statement comparison，再决定是否下导出 |
| GPU 加速幻想 | 想用 CUDA 并行加速内核检查 | Lean 内核是分支密集的符号树操作，算术强度极低；显存通常远小于解压后的 61.5GB，无法整体载入 | 不值得。提速靠内存、CPU 核数、单核性能与 NVMe |

## 3. 事实边界

- MIT 验证页记录的检查项见 REPORT.md 第四节；本仓库的整理工作止于资料抓取与静态定位。
- 形式化检查能说明的是「按所写形式陈述、依据所用内核、从三条标准公理推出」；它不说明 25 万页量级的证明在人类可理解意义上被掌握。四页说明里作者本人明确说自己尚未掌握全部论证，且只发布 Lean、没有可读正文。
- 20 步时间线是线索不是大纲：作者说其中部分描述只是对「slop」摘要的转述，可能毫无意义，且可能修订。
- `RiemannHypothesis` 用的是 Mathlib 自带定义；项目自定义的是 zeta 零点与 Dirichlet L 函数的四个命题。这里的「GRH」仅指 Dirichlet L 函数。

## 4. 文档指路

| 文件 | 用途 |
|---|---|
| `README.md` | 仓库入口与定位 |
| `REPORT.md` | 完整调研报告：一手引文、20 步时间线、验证状态、缺口、逆推清单 |
| `sources/SOURCE_INDEX.md` | 全部来源的 URL 与抓取状态 |
| `sources/PROOF_SKELETON.md` | 四层依赖链、模块规模、逆推线索 |
| `sources/skeleton-defs.txt` | 骨架定理定义位置扫描结果 |
| `notes/01-claims.md` | 一手声称与文献笔记 |
| `notes/02-lean-code.md` | 代码托管、结构与下载核查 |
| `notes/03-commentary.md` | HN、MathOverflow、博客与中文社区评论归档 |
| `notes/04-backfill.md` | 补漏扫描与逆推输入整理 |
