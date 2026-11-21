# Levent Alpöge「黎曼猜想证明」配套 Lean 代码：托管、结构、可下载性与佐证

> 调研时间：2026-11-21（UTC+8）。一手代码已下载、解包并做关键字审计。

## 0. 一句话结论

代码确为公开可下载：`https://alpo.ge/lean.tar.gz.part01` 至 `part14`，拼回 239,275,554 字节、175,187 个 `.lean` 文件、17,374,215 行；解包后全库 **axiom / sorry / admit / unsafe / opaque 均为 0**，唯一外部依赖为 pin 到固定 revision 的 mathlib。Zenodo「The Cathedral」与本代码规模相差两个数量级，关系未证实（见第 7 节）。

---

## 1. 代码的确切托管位置与下载 URL

### 1.1 一手来源（权威）
- 声明 PDF：`https://alpo.ge/remark.pdf`（HTTP 200，`content-length: 245114`，`content-type: application/pdf`，Cloudflare `server: cloudflare`）。
- PDF 脚注 1 逐字原文（提取自 PDF 第 1 页）：
  > `1https://alpo.ge/lean.tar.gz.part01, . . . , https://alpo.ge/lean.tar.gz.part14.`
- 即 **14 个分卷**，完整 URL 模式：`https://alpo.ge/lean.tar.gz.partNN`，`NN = 01,02,…,14`。
- 托管方：Alpöge 个人域名 `alpo.ge`（无 GitHub / HuggingFace / Zenodo / 网盘 / 种子链接出现在声明中）。`Scientific American` 亦把其个人网站称作 statement 发布处。

### 1.2 逐卷 HEAD 探测
- `https://alpo.ge/lean.tar.gz.part01` → HTTP 200，`etag: "b116580303b65e25529dae6e694a3bab"`，`access-control-allow-origin: *`，`cache-control: public, max-age=0, must-revalidate`，`cf-cache-status: DYNAMIC`。
- `part02` → 200，`etag: "47a77f2884ae9185547ee36e4c9cf397"`。
- `part14` → 200，`etag: "8a60f30a51ada38b89908f1ab8fcd86c"`。
- Range 请求 `bytes=0-255` 被忽略（返回整卷），首字节十六进制 `1f 8b 08 00 00 00 00 00 00 ff` → 标准 **gzip** 流。

### 1.3 实际下载结果（全部 14 卷已落盘）
- part01–part13：各 **18,000,000 字节**；part14：**5,275,554 字节**。
- 拼接总大小 **239,275,554 字节**（≈228 MiB）。SHA256 前缀 `9631b4f0…` 与 MIT 独立验证页一致。
- 分卷保存为 `lean-parts/lean.tar.gz.part01..14`，拼回的压缩包为 `lean.tar.gz`（239,275,554 字节）。
- 注意：分卷是**原始字节切片**，part14 单独无 gzip 头，必须按序 `cat part01..part14 > lean.tar.gz` 后再解。

### 1.4 媒体复述的下载与体量（虎嗅，2026-11-21 01:02，作者「数字生命卡兹克」）
URL：`https://www.huxiu.com/article/4907330.html`
> 「PDF的脚注里，放了14个压缩包的链接。我把它们全部下了下来，压缩包229MB，解压之后1.4GB，一共175187个Lean文件，1737万行代码。」
> 「作为对比，九月份费马大定理的整个证明翻译成Lean，是1300万行。」

该报道的 229MB / 175187 文件 / 1737 万行与实测（239,275,554 字节 / 175,187 / 17,374,215）吻合。

---

## 2. 代码结构（来自解包后的真实文件）

### 2.1 构建配置（逐字，来自 `lean/lakefile.toml`）
```
name = "tree"
srcDir = "src"
defaultTargets = ["BuildAid", "CompanionTheory", "Development", "Formalism", "GroundTheory", "GroundTheoryFields", "Lemmata", "PositivityTheory", "ReciprocityTheory", "Supplements", "Main"]
moreLeanArgs = ["--tstack=400000"]

[leanOptions]
autoImplicit = false
relaxedAutoImplicit = false
maxSynthPendingDepth = 3
pp.unicode.fun = true
maxHeartbeats = 4000000

[[require]]
name = "mathlib"
git = "https://github.com/leanprover-community/mathlib4"
rev = "0df444a360eaa60ab8c11dca51a86af692955474"

[[lean_lib]]
name = "Main"
roots = ["Statements", "Main"]
```
- `lean/lean-toolchain` 逐字：`leanprover/lean4:v4.33.1`
- `lean/lake-manifest.json`：唯一外部包 `mathlib`，rev `0df444a360eaa60ab8c11dca51a86af692955474`；另有继承的 `plausible`（rev `b7eb3304aeae834b12dda98993a37f6a41f6f0bb`）等。
- 11 个顶层库：`BuildAid`、`CompanionTheory`、`Development`、`Formalism`、`GroundTheory`、`GroundTheoryFields`、`Lemmata`、`PositivityTheory`、`ReciprocityTheory`、`Supplements`、`Main`。`GroundTheory` 下含 ArithmeticGeometry / AutomorphicForms / Brauer / FunctionField / HeightsAndTheta / IdeleRoots / PAdicHodge / PowerSubgroups / ProfiniteCohomology / TateCohomology。
- `BuildAid/D1.lean`–`D5.lean` 各约 160–185 字节，是极小的编译辅助文件。

### 2.2 一级目录文件数（part01 解出约 10,345 个带 PAX 全路径的条目）
`lean/src/BuildAid` 5；`lean/src/CompanionTheory` 2027；`lean/src/Development` 8310（其余卷含 Formalism/GroundTheory/…/Main）。

### 2.3 核心入口：`lean/src/Main.lean`（433 字节，逐字全文）
```
import Statements
import Supplements.Kernels.MainTheoremsAll

theorem riemannHypothesis : RiemannHypothesis := rhOfAll

theorem riemannZeta_eq_zero_re : RiemannZetaZerosOnLine := rhOfAll

theorem LFunction_ne_zero_of_half_lt_re : DirichletLNonvanishingRightOfHalf := allChi

theorem LFunction_eq_zero_re : DirichletLZerosOnLine := claimAll.2.1

theorem LFunction_eq_zero_re_of_isPrimitive : PrimitiveDirichletLZerosOnLine := critAll
```
**这五个顶层定理是「逆推证明过程」的关键入口**：RH 本体 (`riemannHypothesis`)、黎曼 zeta 零点在线 (`riemannZeta_eq_zero_re`)、Dirichlet L 函数在 Re>1/2 非零、Dirichlet L 零点在线、本原 Dirichlet L 零点在线。依赖的证明项分别为 `rhOfAll`、`allChi`、`claimAll.2.1`、`critAll`。注意 `RiemannHypothesis`（大写驼峰）本身未定义在 `Statements.lean`，应定义在 `Supplements.Kernels.MainTheoremsAll` 或其上游。

### 2.4 核心陈述：`lean/src/Statements.lean`（783 字节，逐字全文）
```
import Mathlib

def RiemannZetaZerosOnLine : Prop :=
  ∀ s : ℂ, riemannZeta s = 0 → (¬∃ n : ℕ, s = -2 * ((n : ℂ) + 1)) → s ≠ 1 → s.re = 1 / 2

def DirichletLNonvanishingRightOfHalf : Prop :=
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
    1 / 2 < s.re → (χ ≠ 1 ∨ s ≠ 1) → DirichletCharacter.LFunction χ s ≠ 0

def DirichletLZerosOnLine : Prop :=
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
    0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2

def PrimitiveDirichletLZerosOnLine : Prop :=
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N), χ.IsPrimitive →
    ∀ s : ℂ, 0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
```
注意零点陈述显式排除了平凡零点 `s = -2(n+1)` 与 `s = 1`。

### 2.5 典型文件路径示例（真实清单，取自 part01）
- `lean/src/BuildAid/D1.lean` … `D5.lean`
- `lean/src/CompanionTheory/AutomorphicTheory/BesselOrbitBounds/BesselMapUniqueness/BesselMapUniqGap.lean`（16561 字节）
- `lean/src/CompanionTheory/AutomorphicTheory/BesselWaveGrids/CoefOfOfWsp.lean`（18176）、`EigStrOfMem.lean`（1624）、`Bridge.lean`（7940）
- `lean/src/CompanionTheory/AutomorphicTheory/JacobiDiagonalBounds/Vander.lean`（1342）
- `lean/src/CompanionTheory/AutomorphicTheory/LocalBesselUniqueness/BesselModels/LocScalar.lean`（22833）、`LocalUniquenessReduction*.lean`（34834）
- `lean/src/Development/AbelianVarietyTheory/Proofs/DecoratedLatticeProofs/`：`LastSeeSawDatumExistsAtLevelPrimePlace.lean`（11993）、`LevelPrimeLawLastTwoStepsAllTuples.lean`（12309）、`LowerBlockDoubleCosetOperatorKeepsLevelCharacter.lean`（13628）、`LintegralBesselGroupUnfoldFundamentalDomain.lean`（4531）等，大量成对出现 `XxxStatement.lean` + `Xxx.lean`。
- `lean/src/CompanionTheory/RealGroups/BaseChange/ArchimedeanBaseChange/TensorCompletions/SkewFieldFrames/PFactorField.lean`
- `lean/src/CompanionTheory/HarishChandraModules/KFiniteAnalyticVectors/OrbitDerivatives/EtaleClosedPointLiftsSimpleRootsMonic.lean`
- `lean/src/GroundTheory/LocalFields/MainTheorem.lean`（3690 字节）
- `lean/src/Supplements/Kernels/MainTheorems.lean`（740）、`MainTheoremsAll.lean`（136）、`MainTheoremsCritical.lean`（319）
- 解包得到 175,190 个文件条目。

### 2.6 假设与缺口审计（对全部 175,187 个 `.lean` 逐字扫描）
| 关键字 | 命中文件数 | 说明 |
|---|---|---|
| `axiom` | 0 | 无任何 `axiom` 声明 |
| `sorry` | 0 | 无任何 `sorry` |
| `admit` | 0 | 无 |
| `unsafe` | 0 | 无 |
| `opaque` | 0 | 无 |
| `noncomputable` | 79954 | 常见于分析/选择，非证明缺口 |
| `Classical.choice` | 212 | 经典选择，非公理声明 |
| `constant` | 35 | 抽查命中均为 `PreconnectedSpace.constant` 之类普通标识符，非顶层假设 |

扫描结果为零命中。
结论（资料层面）：该代码库内**未声明任何 axiom、无 sorry**，与声明 PDF 中 "formalized the arguments without assumptions" 自述一致；也与 Andrew Sutherland 的「只用 3 条最基础标准公理」说法方向一致（「3 条标准公理」应指 Lean/Mathlib 自身的逻辑基础，而非本库新增公理）。
注意：Zenodo「The Cathedral」条目自述「one axiom equivalent to RH」，与本库 0 axiom 的记录不同，是判断两者关系的关键差异（见第 7 节）。

---

## 3. 声明 PDF 逐字要点（一手，2026-11-20 发布，抓取 2026-11-21）

URL：`https://alpo.ge/remark.pdf`
- 开篇逐字：
  > "I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis. In fact, the method easily extends much further, resolving the so-called Generalized Riemann Hypothesis, and I expect, with more time, the Extended Riemann Hypothesis too. Claude and I have also formalized the arguments without assumptions."
- 关于为何只放代码（逐字）：
  > "It being obviously inappropriate to attempt to post some massive unreadable travesty of 'slop', I decided to only present Lean."
- 页数脚注 2 逐字：
  > "I of course amused myself, and hit 250, 000+ pages. Claude cutely called it the GRH ... 'book' — I looked at Theorem 1 and it began by noting that 2 then σ > 1/2 because the reals are totally ordered if σ ≠ 1/2 or σ < 1/2."
- 方法论关键句（逐字）：
  > "for the von Mangoldt sum of length X, Claude's suggested length of chain of theta lifts, in view of each mimicking multiplicity, was √X. (And yet, what else could it be?)"
- 关于时间线与 Claude 转述：
  > "So here is a rough timeline Claude provided after a thorough search."
  > "The parts in which Claude is the expert and not me I am sort of paraphrasing a 'slop' summary, which might make zero sense, and I apologize, though the Lean should suffice to clarify everything."
- 自述致谢名单（逐字）：Manjul Bhargava, Kai Gill, Hamish Kerr, Max Leiter, John Ma, Tal Magdish, Chris Paika, Maksym Radziwiłł, Jascha Sohl-Dickstein, Peter Sarnak, Jarred Sumner, Amir Tajdini, and especially David Dohan, Ben Michel, Ralph Furman, Eric Price, and Cora Vinther。
- 提到与 OpenAI 722 篇论文中 "quiet inclusion of the Siegel argument" 的关系，并称其为 "Stepanov without multiplicity"；提到 "qN = M^k + R sign-only proof of Weil and Deligne"。

### 3.1 PDF 中的 20 步时间线（逐字，日期为 2026 年，MM/DD/YY）
1. 10/6/26: Stepanov determinant.
2. 10/9/26: Dp ≡ 0 on a CM curve for p inert in K, D the invariant derivation…
3. 10/19/26+: qN = M^k + R, using x1·x2 + … + xk−1·xk, wanting "automorphy" of N …
4. 10/6/26+: W(1S) ≥ 0 via a determinant, interpret as Stepanov with multiplicity one again; bootstrap!!
5. 10/20/26: definitively switch to Λ …
6. 10/8/26: W(1(0,1] + (∼X η)·1(0,1/X]) ≥ 0 gives > 1 − η.
7. 10/14/26: Determinants for W(·)'s containing the desired periods, indeed GGP periods …
8. 11/15/26: Iterated theta lifts U(X0) → U(X1) → … → U(Xm) between totally definite unitary groups, π ↦ π ⊞ µj[t], wins multiplicity m. Time to take m like √X …
9. 11/15/26: Norms of the lifts and level and cancellation of order-m terms with proven divisibility via Chowla-Selberg, Weyl, and Siegel mass.
10. 11/15/26: Sufficient lower bound on divisibility of the m-fold iterated lift.
11. 10/6/26: Iwasawa tower via analogy to pigeonhole on Fqn/Fq.
12. 11/15/26: Seesaw: the period of the iterated lift is a Bessel period independent of the Hecke characters, Rallis.
13. 10/6/26: Bost slope inequality.
14. 11/15/26: precise computation of the "cost" of each lift, showing that one would stop at 2/3 a priori …
15. 11/15/26: Glue together an integral structure across Hecke characters for a "gcd" i.e. extra averaging.
16. 10/14/26: Relative trace formula, seen in a different half-plane argument via sixfold cover of GL5 automorphy of µ …
17. 11/15/26: for the classes in H¹(…, Qℓ/Zℓ(−e)) that we couldn't kill by twisting, fight for a significant weakening of Vandiver.
18. 11/15/26: computation of covolumes, theta series' index, congruences between lifts and forms.
19. 10/19/26: precise choice of the Iwasawa tower, specifically using Behrend!!
20. 11/16/26: GRH.

该时间线是代码 `CompanionTheory/AutomorphicTheory/*`（Bessel/LocalBesselUniqueness/JacobiDiagonalBounds）与 `Development/AbelianVarietyTheory/Proofs/DecoratedLatticeProofs/*`（Hecke 特征、level prime、trace formula）两大目录的数学对应物。

### 3.2 已保存的一手文本
PDF 文字层按 AGENT.md 第 1 步重新生成（282 行）。

---

## 4. Andrew Sutherland 等人编译检查的原始报道

### 4.1 Scientific American（2026-11-20，作者 Joseph Howlett，编辑 Claire Cameron）
URL：`https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/`
逐字引文：
> "Alpöge posted his Claude proof coded in the programming language Lean, which verifies the mathematical correctness of each argument. Issues can arise in Lean, but it shouldn't take long for experts to verify whether the hypothesis is properly coded, and if the proof is correct."
> "'This is indeed an historic moment,' Sutherland tells me today after he had independently compiled Alpöge's result in Lean. 'The world has changed.... I know there will be a wide range of reactions in our community, but to me this is a human triumph, and a day for celebration.'"
> "Sutherland says that it's worth celebrating the human work behind the development of Lean and LLMs—and, for that matter, the millennia of mathematical achievements upon which today's advance rests."
> "'There is much work to be done to understand the mathematical details of the proof,' he says. 'And I can't wait to dig into them. But perhaps the single most important step in the proof was building a machine that can think.'"
同文另有其旧年引语：
> "An AI that can prove the Riemann hypothesis is not one I'd want to meet. If that happens, mathematicians having jobs will be the least of our problems."

同页推荐阅读（未逐篇抓取，URL 需从站内再取）："The AI Riemann hypothesis proof is almost certainly true, independent mathematicians attest"（Clara Moskowitz）、"Dueling claims of Riemann hypothesis proofs emerge"（Joseph Howlett）、"What does a solution to the Riemann hypothesis mean for you?"（Lee Billings）。

### 4.2 虎嗅报道中的其他编译者（2026-11-21 01:02）
URL：`https://www.huxiu.com/article/4907330.html`
> 「不过已经有人在开始跑了，数学家Elliot Glazer说，他已经在编译，要跑好几个小时，他估计Lean本身出bug的概率不到1%。」
> 「曼彻斯特大学的数论学家Thomas Bloom说，Lean社区的专家们正在查。他还吐槽了一句，说不知道哪个更让他震惊，是AI证明了黎曼猜想，还是这个证明写成人能读的版本，要超过25万页。」

### 4.3 SciAm 页面中一条链接的错位（资料性发现）
SciAm 正文把 `https://x.com/__alpoge__/status/2079028340955197566` 标注为 "Claude-generated solution"，但该 URL 实际打开是 **2026-07-20 的雅可比猜想反例帖**（逐字："Jacobian conjecture is false, thanks to my good friend Achill for asking about this…"，附映射 `((1+xy)^3 z + y²(1+xy)(4+3xy), y + 3x(1+xy)² z + 3xy²(4+3xy), 2x − 3x²y − x³z): ℂ³→ℂ³`，Jacobian = −2）。抓取时阅读量 4,012.8 万。即该链接并非 RH 公告帖，引用时需小心。

### 4.4 MIT 独立验证页
URL：`https://math.mit.edu/~drew/grh/`
含五个顶层定理、三公理审计、7.7GB proof export；其 SHA256 前缀 `9631b4f0…` 与本次下载拼接结果一致。该页是公理数量的权威出处。

---

## 5. 可下载性结论

- **能下载**。一手直链可用， Cloudflare 允许跨源（`access-control-allow-origin: *`），HEAD 与 GET 均 200，无登录、无付费墙。
- 下载方式见 AGENT.md 第 2 至 4 步（bash／WSL 语法）。得到 `lean/` 目录，入口 `lean/src/Main.lean`。
- 体量：下载 239,275,554 字节；解压后约 1.03GB；媒体称解压后 1.4GB。
- 结构：175,190 个 tar 条目，其中 175,187 个 `.lean` 文件、17,374,215 行代码。11 个顶层库，入口 `Main`。

---

## 6. 与背景线索的对照

- 「1700 万行 Lean 代码 / 14 卷 / 25 万页可读文本」：14 卷与 17,374,215 行实测吻合；25 万页来自 PDF 脚注 2 作者自述 "hit 250, 000+ pages"。
- 「3 条最基础标准公理」：本库内 0 axiom 声明，与"无新增假设"一致；"3 条公理"的具体所指应查 MIT 验证页的三公理审计（第 4.4 节）。
- 「MIT 的 Andrew Sutherland 在公告当天编译检查」：SciAm 逐字确认 "after he had independently compiled Alpöge's result in Lean"。

---

## 7. Zenodo「The Cathedral」与本次代码的关系（**未证实，仅记录**）

- 相关线索：`https://zenodo.org/records/20803093`，题名 "The Cathedral: A Machine-Verified Architecture for the Riemann Hypothesis"，日期 2026-06-22，Lean 4 + Mathlib，504 个文件约 15.8 万行，一个等价于 RH 的公理，crown path 上零 sorry，Nyman–Beurling L² 距离判据。
- 本次未取得该记录正文（站点 403），下列为基于已有证据的客观差异清单：
  1. 规模差两个数量级：Cathedral 504 文件 / 15.8 万行；alpo.ge 代码 175,187 文件 / 1737 万行。
  2. 时间差约 5 个月：Cathedral 2026-06-22；Alpöge 公告 2026-11-20（PDF 时间线起于 10/6）。
  3. 公理口径相反：Cathedral 自述「一个等价于 RH 的公理」；alpo.ge 代码实测 0 axiom。
  4. 数学路线不同：Cathedral 为 Nyman–Beurling L² 距离判据；Alpöge 时间线为 Stepanov 行列式 / theta lifts / Iwasawa tower / trace formula 的自守路线。
  5. 作者未确认：Cathedral 的作者信息需读 Zenodo 记录页；Alpöge 声明的致谢名单中未出现 Cathedral 相关署名。
- 如需定性，可读取 Zenodo 记录的 creator / description 与文件清单，与第 2 节的目录名交叉比对。

---

## 8. 资料获取方式

- 四页说明 PDF：`https://alpo.ge/remark.pdf`，提取方式见 AGENT.md 第 1 步
- 14 卷源码与拼包校验：AGENT.md 第 2 至 3 步
- 解包与入口文件：AGENT.md 第 4、6 步
- MIT 校验文件：AGENT.md 第 5 步
- 关键字审计：对全部 175,187 个 `.lean` 逐个扫描，axiom / sorry / admit / unsafe / opaque 命中均为 0
