# Anthropic / Claude 黎曼猜想声称 · 调研准备报告

> 生成时间：2026-11-21（Asia/Shanghai）。发布物：本仓库。
> 原文引文保留英文，中文来源保留中文；所有结论均标注出处。

## 一、任务与结论概览

调研任务：把 2026-11-20 前后关于「Anthropic 生成黎曼猜想证明」的消息汇成一整套可检索材料，重点覆盖声称、Lean 代码与网上评论，为后续校验和讲解做准备。本报告的定位是证明前的资料准备。

核心结论（均有一手来源）：

1. 这不是 Anthropic 官方公告，而是其雇员 **Levent Alpöge** 于 **2026-11-20** 在个人网站发布的四页说明（`https://alpo.ge/remark.pdf`）。
2. 声称内容：与 Claude 合作发现黎曼猜想证明；方法「容易推广」到广义黎曼猜想（GRH），并预期能推广到扩展黎曼猜想；并称已把论证形式化且「without assumptions」。
3. 唯一发布的证明材料是 **14 卷 Lean 源码**，解压后为 **175,187 个 `.lean` 文件、17,374,215 行**；没有人类可读的证明正文。Alpöge 自述可读文档超过 250,000 页，但那不是发布的证明。
4. MIT 的 **Andrew Sutherland** 建立了独立验证页（`https://math.mit.edu/~drew/grh/`），给出顶层定理原文、公理审计与多内核检查结果。
5. 社区反应分三派：信以为真、质疑（语义、内核 bug、可读性）、以及关于「AI 产出速度超过人类理解速度」的范式反思。

## 二、一手声称：四页说明逐字要点

PDF 已下载并抽取全文，`remark.pdf` 的文字提取层（12,222 字符，获取方式见 AGENT.md 第 1 步）。关键逐字原文：

- 声称句：「I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis.」「In fact, the method easily extends much further, resolving the so-called Generalized Riemann Hypothesis, and I expect, with more time, the Extended Riemann Hypothesis too. Claude and I have also formalized the arguments without assumptions.」
- 分工：「I guided us to the right situation analogous to that toy setting, and then Claude began to leave me in awe.」「Claude flew, and danced, and sparkled near that finish line.」
- 核心数学线索：Stepanov 在函数域上证明黎曼猜想的论证用到了「带重数的除子」，而整数上的重数「corresponding to something like differentiating whole numbers」；新想法是 Fourier 分析式的，导出「chaining theta lifts」，使每次 theta lift 模拟一次重数。Claude 建议的链长是 **√X**（X 为 von Mangoldt 求和长度）。
- 可读性问题：「The argument is incredibly long at the moment, and while its formalization is finished and it is routine to produce a huge, unreadable document containing sufficient detail...」；「I intend to do my best to understand and impart understanding of this argument...」（即作者本人**尚未掌握**全部论证）。
- 为何只发代码：「It being obviously inappropriate to attempt to post some massive unreadable travesty of 'slop', I decided to only present Lean.」
- 脚注 1 给出 14 卷下载地址；脚注 2 说 Claude 把 25 万页可读文档称为「GRH book」，作者看了 Theorem 1，开头只是平凡地按 σ>1/2、σ<1/2 分类讨论。

### 四页说明附的 20 步时间线（原文照录主题，日期为 PDF 原样）

1. 10/6/26 Stepanov 行列式
2. 10/9/26 CM 曲线上惰性素数的 `D^p ≡ 0` 与 theta 导数整除性
3. 10/19/26+ `qN = M^k + R`，θ 的六重覆盖、CM 上的二次型、Poincaré 丛
4. 10/6/26+ `W(1_S) ≥ 0` 用行列式，解释为「重数一的 Stepanov」，bootstrap
5. 10/20/26 切换到 Λ 与 Guinand–Weil 符号、variance test function trick；迭代 Parseval k 次得平方筋品质，反号相消即 QED
6. 10/8/26 `W(1_(0,1] + (~Xη)·1_(0,1/X]) ≥ 0` 给出 > 1−η
7. 10/14/26 W(·) 的行列式含 GGP periods，交错权重
8. 11/15/26 全定酉群间的迭代 theta lifts `U(X0)→…→U(Xm)`，赢得重数 m，取 m 如 √X
9. 11/15/26 lift 范数、级数与 m 阶项消去，用 Chowla–Selberg、Weyl、Siegel mass
10. 11/15/26 m 重迭代 lift 整除性的下界
11. 10/6/26 类比有限域鸽巢的 Iwasawa tower
12. 11/15/26 seesaw：迭代 lift 的周期是独立于 Hecke 特征的 Bessel 周期（Rallis）
13. 10/6/26 Bost slope inequality
14. 11/15/26 每次 lift 的「成本」精算，先验停在 2/3，需要「division」
15. 11/15/26 跨 Hecke 特征粘合整数结构得到「gcd」，即额外平均
16. 10/14/26 relative trace formula，六重覆盖 GL5 的 µ 自守性
17. 11/15/26 对不能靠 twist 杀掉的 `H¹` 类，争取 Vandiver 的显著弱化
18. 11/15/26 余体积、theta 级数指数、lift 与形式同余的计算
19. 10/19/26 Iwasawa tower 的精确选择，用到 Behrend
20. 11/16/26 GRH

> 注意：作者本人说这些描述部分是「paraphrasing a 'slop' summary, which might make zero sense」，且可能编辑修订。它是**线索**，不是可靠证明大纲。

## 三、Lean 代码：已下载材料与完整性

| 项目 | 数值 |
|---|---|
| 下载源 | `https://alpo.ge/lean.tar.gz.part01` 至 `part14`（四页说明脚注 1） |
| 分卷大小 | part01 至 part13 各 18,000,000 字节，part14 为 5,275,554 字节 |
| 拼回包大小 | 239,275,554 字节 |
| 拼回包 SHA256 | `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc`（与 MIT 验证页完全一致） |
| 解包内容 | 175,190 个成员，其中 175,187 个 `.lean` 文件；解压约 1.03GB |
| 代码行数 | 17,374,215 行 |
| 工具链 | Lean v4.33.1、Mathlib v4.33.1（MIT 验证页记录） |

> 下载过程中 part14 因服务器单次只返回 5,275,554 字节被重复追加，按 MIT 官方字节数截断后重新拼包，SHA256 已与官方一致。解包出的源码未因此受损。

顶层结构：`lean/src/Main.lean`（5 个定理）、`lean/src/Statements.lean`（4 个命题）、`Supplements`、`Formalism`、`GroundTheory`、`Development`、`ReciprocityTheory`、`CompanionTheory`、`PositivityTheory`、`Lemmata`、`BuildAid`。各目录文件数与行数见 `sources/PROOF_SKELETON.md`。

## 四、独立验证状态（MIT Sutherland 页，原文汇总）

页面最后更新 2026-11-20 22:05 UTC。其摘要与检查项（逐条转述）：

- 175,187 个模块全部编译通过，0 错误（Lean v4.33.1 / Mathlib v4.33.1）。
- `#print axioms` 对五个顶层定理：`propext`、`Classical.choice`、`Quot.sound` only。
- 环境里共 15 条公理，全部在 Init；没有在 Lean 核心之外声明新公理。环境含 `sorryAx`，但顶层定理不依赖它。
- Mathlib 文件与官方缓存逐字节一致（8,322 个 Mathlib + 378 个依赖 `.olean`）。
- `leanchecker` 内核回放 175,187/175,187 通过。
- 独立的 Rust 内核 `nanoda` 接受完整导出；`con-leche`（master）以 `--verified` 模式接受 1,468,418 条声明；`con-leche --compare` 确认五个定理全部「justified by the solution」；`lean4lean` 通过 175,187/175,187 模块（其中 5 个因默认步数限制重跑后通过）。
- 仍在进行：Lean 默认/paranoid 内核对导出的回放、`lake comparator` 端到端、沙箱化导出与本次导出的 sha256 比对。
- 下载项：`solution.ndjson.zst` 7.7GB（解压 61.5GB，约 61,570,595,788 字节）——未下载；本次取得的是 47MB 的 `challenge.ndjson.zst` 及 MIT 页上的全部小文件。
- 官方提醒：检查只说明「按所写形式陈述、依据所用内核，从三条标准公理推出」；构建未沙箱化；77 个模块的抽样与真实 `lake build` 逐字节一致，但全集未比对。

## 五、社区评论（三派归纳，不下判断）

逐字引文与平台细节在 `notes/03-commentary.md`（299 行）。要点：

- **信以为真**：Sutherland 编译后称「在这份证明里，也许最重要的一步，是造出了一台会思考的机器」；牛津 Fields 奖得主 James Maynard 认为若核实无误就是里程碑。
- **质疑**：Imperial 的 Kevin Buzzard 提醒「编译只说明每步在 Lean 里成立，还要确认证明的是不是黎曼猜想本身」，专家正检查两件事——是否悄悄改了定义、是否钻了 Lean 内核未知 bug；陶哲轩博客只说「消化这份证明需要一些时间」。HN 与 MathOverflow 主战场聚焦 17M 行/25 万页、内核 bug、以及「从机器导出里提取 0=1 有多难」。
- **范式反思**：人类可读性危机；同期出现纯人类的对手证明线索——AMS 会长 Ravi Vakil 披露 Stanford 的 Alessandro Masullo 可能数月前已独立做出类似证明且未用 AI，目前尚未同行复核。
- 中文讨论（知乎、果壳、华尔街见闻、新智元等）覆盖相同议题，知乎该问题约 718 万浏览、850 答。

## 六、未取得或受限的材料

- `science.org`、Zenodo、Scott Aaronson 博客、知乎正文、X/Reddit 原帖：403 或反爬，只拿到检索摘要与转述，见各笔记的「检索限制」节。
- `con-leche-bundled-4.36.log` 等 4 个小日志：`SHA256SUMS` 里列出但未单独抓取；不影响主体。
- 7.7GB 的 `solution.ndjson.zst`：未下载（需要约 100GB 内存、70GB 磁盘才能本地复核）。
- 25 万页人类可读文档：不存在于发布物中；Alpöge 明确只发布 Lean。
- 四页说明里的「3 条公理」「Sutherland 当天编译」在本轮一手材料中已由 MIT 页证实；但「Nyman–Beurling 判据」在本次一手材料中**没有出现**，实际策略是 Stepanov + theta lift，这一项出自早期二手报道，一手材料未证实。

## 七、校验输入清单

1. **顶层命题对齐**：按 AGENT.md 第 4 步解包后读 `lean/src/Main.lean` 与 `Statements.lean`，再对照第 5 步取得的 `challenge.ndjson.zst`（五个带 sorry 命题）与 MIT 页给出的 Mathlib 定义原文。
2. **逆推骨架**：按 `sources/PROOF_SKELETON.md` 的四层依赖链展开，核心入口是 `galoisLocalGlobalStrong_recParameter := galoisRepresentations_of_thetaLift UpperTheory.TP.K.tLiftK`。
3. **声称与实际代码比对**：把四页说明 20 步时间线里的术语（theta lift、Bessel 周期、Iwasawa tower、Bost 不等式、GGP 周期、Vandiver 弱化、Behrend）逐一去代码里定位对应文件，看时间线声称的路线与代码顶层依赖是否一致。
4. **公理与定义审计**：用 AGENT.md 第 5 步取得的 `audit.log` 核对三条公理；用 MIT 页 caveats 核对构建来源与沙箱性。
5. **评论证据**：需要原话时按 `sources/SOURCE_INDEX.md` 的 URL 回到原始平台，或查 `notes/` 中注明出处的引文。

以上仅为材料的检索入口，供后续核验取用。
