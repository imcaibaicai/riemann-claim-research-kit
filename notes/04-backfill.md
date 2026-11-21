# 04 — 补漏扫描与「从 Lean 逆推证明过程」输入清单

> 调研时间：2026-11-21（UTC+8）。
> 本文内容为来源搜集与资料整理。
> 所有引文均为逐字摘录（英文原文保留），并附确切 URL 与页面时间。时间戳以来源页面自述为准。
> 说明：本轮正文抓取以脚本直取为主；一手 PDF（remark.pdf）的内容另见 `notes/02-lean-code.md` 的文字层提取与 `notes/01-claims.md` 的逐字引文。

---

## 0. 一句话结论

四页说明、Lean 代码、媒体报道三路之外最大的遗漏是 **MIT Andrew Sutherland 的独立验证文件页**（`math.mit.edu/~drew/grh/`）——它把顶层定理原文、完整公理审计、 proof export 下载与多个独立内核的验证状态全部公开，正是「逆推证明过程」最硬的入口；此外 Clarín 与中文源给出了 20 条时间线的具体节点，Aaronson 博客与 OfficeChai 暴露了争议与滨把边界。

---

## 1. 遗漏来源清单（按重要性排序）

### 1.1 MIT Sutherland 独立验证文件页【最重要，一手】
- URL：`https://math.mit.edu/~drew/grh/`
- 页面自述时间：`Andrew Sutherland (MIT), checks run with Claude Code. Page last updated 2026-11-20 22:05 UTC.`
- 标题：`Verification files for the Lean proof of the Riemann Hypothesis`
- 逐字引文（Summary）：`The published Lean source compiles without errors, the five headline theorems depend only on the three standard axioms, every module has been replayed through the Lean kernel, two independently written kernels, nanoda and con-leche, accept the full exported proof, and lean4lean accepts every compiled module.`
- 这是四页说明与新闻报道均未直接给出的「验证侧一手记录」。含可下载 proof export（7.7 GB）、challenge export、公理审计、独立内核日志。

### 1.2 Scott Aaronson 博客《The stochastic parrots have apparently proved the Riemann Hypothesis》【评论/质疑】
- URL：`https://scottaaronson.blog/?p=10344`
- 时间：`Friday, November 20th, 2026 at 12:29 pm`，55 条评论（抓取时截至约 Comment #24）
- 逐字引文：`I woke up this morning to the news that Levent Alpöge says that he's gotten Anthropic models to prove the Generalized Riemann Hypothesis, producing 17.4 million (!!) lines of Lean code.`
- 关键质疑引文：`the Lean formal verification system still has issues, from kernel bugs (which have, in the past, allowed proofs of 0=1) to uncertain soundness of the underlying type theory... Until they are, a sufficiently motivated AI could almost certainly exploit them to prove a false statement! All the same, I see zero indications that that's happening right now.`
- 立场引文：`so far, not one of the AI results with Lean certificates has needed to be retracted... my bet is that the proof will stand.`
- 逆推相关引文：`Mathematicians of course won't be satisfied with 17 million lines of Lean. They'll be satisfied when, and only when, they're able to explain and distill and reconstruct the ideas for themselves.`
- 评论中的分支线索（均需另行核实）：
  - Comment #4 Noah Snyder 指向人类证明：`https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/`（标题含 hurried announcement、Alessandro Masullo、GRH）
  - Comment #15 BasicQuestion：`https://reddit.com/r/mathematics/comments/1y1b463/ravi_vakil_announces_fully_humancreated_grh_proof/`（Ravi Vakil 宣称纯人工 GRH 证明）
  - Comment #14 引 Connor Leahy：`https://www.youtube.com/watch?v=oiXjKblOszU`（Future of Life Institute，2026-11-20）

### 1.3 Clarín 两篇深度报道（阿根廷，英文版）【策略 + 验证路线图】
- 报道一：`https://www.clarin.com/us-news/the-riemann-hypothesis-been-solved-anthropic-mathematician-levent-alpoge-claims-claude-cracked-maths-most-famous-problem_0_nQxBE3rUxX.html`（2026-11-20）
- 报道二（验证路线图）：`https://www.clarin.com/us-news/whats-next-if-the-riemann-hypothesis-is-solved-the-road-to-verifying-alpoge-and-claudes-proof-claim_0_1lB3dnmm5n.html`（2026-11-20）
- 策略引文（报道一）：`pointing to a strategy built around weighted prime sums (von Mangoldt sums) and chains of mathematical constructions called theta lifts.`
- 档案引文：`14 archive parts totaling roughly 239 megabytes and 175,187 source files written in Lean`
- Clay 引文：`Under Clay's official rules, a personal website post and a code archive do not qualify for the prize... As of Friday, Clay's official page still listed the problem as unsolved.`
- 250,000 页引文（报道二）：`In a footnote, he revealed that a full written version he generated ran past 250,000 pages—a massive dump he refused to publish, dismissing it as "slop".`
- 时间线引文（报道二）：`Dated entries begin on October 6, with several pivotal breakthroughs landing on Sunday, November 15—when Alpöge wrote that it "happened for me late Sunday night"—followed by a final November 16 entry reading simply "GRH".`
- 新质疑引文（报道二）：`In October 2026, researchers Alexander Bastounis, Fabian Circelli, and Anders C. Hansen warned in a preprint—citing an OpenAI Navier–Stokes claim—that Lean checks of AI translations can offer "no confidence" if a mismatch exists between a problem and its formalization.`

### 1.4 kingy.ai 综述（ Curtis Pyke，2026-11-20）【证据最全的二手汇总】
- URL：`https://kingy.ai/blog/levent-alpoge-claude-riemann-hypothesis-proof/`
- 引文（顶层定理原文）：`One line reads theorem riemannHypothesis : RiemannHypothesis := rhOfAll .`
- 引文（档案指纹）：`The joined archive contained 239,275,554 bytes and 175,187 Lean source files.`
- 引文（机制）：`a von Mangoldt sum counts prime powers with a weight tied to the underlying prime. For a sum of length X, Alpöge says Claude proposed using a chain of theta lifts of length √ X.`
- 引文（验证时点）：`Sutherland's 19:45 UTC report says the theorem statements and axioms passed in one Comparator run. It still lists the end-to-end verdict and a separate Lean-kernel replay of the exported proof as pending.`
- 姊妹篇（同日"两个证明声明"）：`https://kingy.ai/blog/two-riemann-proof-claims-timeline/`
- 67% 结果解释：`https://kingy.ai/blog/claude-riemann-hypothesis-67-percent-result/`

### 1.5 OfficeChai【Pool 边界 + 数学工具清单最细】
- URL：`https://officechai.com/ai/claimed-riemann-hypothesis-proof-made-with-claude-appears-on-anthropic-mathematician-levent-alpoges-website/`（2026-11-20）
- 引文（数学工具）：`The other steps draw on a wide range of modern number theory, including relative trace formulas, Iwasawa theory, Bessel periods and a weakened form of the Vandiver conjecture.`
- 引文（匿名/未分享）：`The four-page document, titled "Remark," is hosted at alpo.ge. It carries no author's name, and Alpöge has not shared it on social media or anywhere else.`
- 引文（黑箱担忧）：`A Lean-only release also raises the concern Terence Tao has voiced about AI proofs arriving as sealed black boxes... It also cuts against the call from 25 Fields medalists for slower, properly written-up announcements.`

### 1.6 MLQ.ai（2026-11-20 15:27）【逐条脚注索引】
- URL：`https://mlq.ai/news/anthropic-mathematician-levent-alpoge-posts-a-claimed-proof-of-the-riemann-hypothesis-made-with-claude/`
- 引文（方法）：`The approach, as he describes it, adapts Stepanov's method, which proved the Riemann hypothesis for curves over finite fields ("function fields") and relies on a kind of multiplicity that has no obvious counterpart over the whole numbers.`
- 引文（8 月前史反转）：`Anthropic said then that Claude's attempt at the full hypothesis had failed and that it did not expect those techniques to prove it.`

### 1.7 中文来源（本次补充）
- 腾讯新闻《Anthropic的数学家宣称在Claude的帮助下证明了黎曼猜想》：
  - URL：`https://news.qq.com/rain/a/20261120A0GCJC00`，时间 `2026-11-20 22:55`，作者「赛博禅心」
  - 关键引文（20 步时间线的具体节点，其他英文源未逐日给出）：`10 月 6 日：从 Stepanov 行列式、 Iwasawa 塔 与 Bost 斜率不等式 起步 / 10 月 20 日：转向 von Mangoldt 函数的加权和 / 11 月 15 日：这一天标了七条：定号酉群之间的迭代 theta lift （赢得 m 重数，取 m ≈ √X）、提升的范数与整除性、 Rallis 跷跷板，以及各步"代价"的精确计算 / 11 月 16 日：走通 GRH`
  - 下载链接原文：`https://alpo.ge/lean.tar.gz.part[x]`（x 替换为 01-14）
  - 中文社区镜像：`https://github.com/CocoSgt/riemann-hypothesis-lean`（版权归属原作者）
- 新浪财经（转量子位）《黎曼猜想被证明？！Claude加持，34岁数学家放出Lean证明》：
  - URL：`https://finance.sina.com.cn/stock/t/2026-11-21/doc-inizmfew3857939.shtml`，时间 `2026年11月21日 00:06`
  - 引文（诗性描述，四页说明原文中文转述）：`临近答案的终局，Claude不再缓步求索，它振翅腾空，以起舞完成这场奔赴。`
  - 引文（前后史）：`将已知能够证明位于黎曼ζ函数临界线上的非平凡零点中，能够证明位于临界线上的比例下界从41.6%提高到67.2%。`
  - PDF 直链（量子位版带 utm）：`https://alpo.ge/remark.pdf?utm_source=chatgpt.com`

### 1.8 其他英文报道（补漏列出，未逐篇深抓）
- Scientific American（一手首报，Joseph Howlett，2026-11-20）：`https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/` — 引文：`"If the proof is correct and understood, this completes a dictionary between different branches of math where the connections were only partial," says Hector Pasten... "something like a Rosetta Stone."`；Sutherland 当日回应：`"This is indeed an historic moment," Sutherland tells me today after he had independently compiled Alpöge's result in Lean. "The world has changed..."`；编辑注：`Editor's Note (11/20/26): This is a breaking news story and will be updated.`
- The Verge：`https://www.theverge.com/`（Google 收录标题 `Anthropic mathematician claims to have solved prestigious ...`，未定位到精确文章 URL）
- Phys.org：`https://phys.org/`（标题 `Anthropic researcher says he and Claude proved the ...`）
- The Straits Times：`https://www.straitstimes.com/world/united-states/anthropic-researcher-says-ai-chatbot-claude-solved-maths-greatest-riddle`
- 拉美/意/西语系：Clarín（阿根廷，见 1.3）、`https://www.nacionale.com/en/articles/how-does-alpoge-say-claude-helped-riemann-hypothesis-proof-claim-timeline`（403，仅标题 `Claude 'flew' at the finish: Alpöge on its Riemann role`）、`https://pasqualepillitteri.it/en/news/35468/riemann-hypothesis-proof-claude-alpoge`
- Reddit 讨论串：`https://www.reddit.com/r/Physics/comments/1y154le/anthropic_claims_proof_of_the_riemann_hypothesis/`、`https://www.reddit.com/r/singularity/comments/1y14vcd/levent_alp%C3%B6ge_working_with_claude_claims_to_have/`、`https://www.reddit.com/r/EverythingScience/`（200+ 评论，12 小时前）、`https://www.reddit.com/r/mathematics/comments/1y1b463/...`（Vakil 人类证明）
- 其他综述：`https://felloai.com/riemann-hypothesis-solved/`、`https://medium.com/@adnanmasood/riemann-hypothesis-explained-claude-lean-proof-0cbe6a4ff681`、`https://eu.36kr.com/en/p/4078708897092744`（`Is the Riemann Hypothesis really proven by Claude? Fields ...`）、`https://mlq.ai/news/anthropic-mathematician-levent-alpoge-posts-a-claimed-proof-of-the-riemann-hypothesis-made-with-claude/`
- 视频/播客：Scientific American 的 1:46 短片（Google 视频结果显示发布在 Facebook/YouTube/Instagram，标题 `Riemann Hypothesis "solved" by Claude AI and Anthropic ...`）；Connor Leahy 访谈（见 1.2 Comment #14）。
- 多语种检索（日/德/法/俄/印）尝试过 `リーマン予想 Claude 証明 アルポゲ`、`Riemann-Vermutung Claude Beweis Alpöge`、`Hypothèse de Riemann Claude Alpöge`、`Гипотеза Римана Клод ...` 等查询词，未取得可引用的逐字引文，属未取得项。

---

## 2. 证明数学策略的具体描述（四页说明 + 转述逐字汇总）

> 只做摘录，不评价对错。四页说明标题为《Remark》，托管于 `https://alpo.ge/remark.pdf`（2026-11-20）。抓取时该 PDF 为二进制，正文依据 OfficeChai、kingy.ai、Clarín、腾讯、新浪、MLQ 六源交叉转述。

1. **总方法**：改编自**函数域（有限域上曲线）的 Stepanov 方法**，该方法靠构造具有高「重数」的辅助函数证明函数域 RH；迁移到整数时缺「对整数求导」的自然对应。（OfficeChai / MLQ / 腾讯 / 新浪）
2. **权重和**：对象是 **von Mangoldt 加权和**（对素数幂按底素数加权），长度为 X。（kingy.ai / Clarín / 腾讯）
3. **重数的替代**：用**长链 theta lifts（θ 提升）**替代导数，每次提升在定号酉群之间增加一单位重数；链长取 **√X**（即素数规模的平方根）。此关键参数由 Claude 提出。（OfficeChai / kingy.ai / 腾讯）
   - 腾讯逐字引文：`面对长度为 X 的 von Mangoldt 和 ， Claude 建议把这条链的长度取为 √X。Alpöge 说，光这一句话就足以让同行从椅子上摔下来（堪称眩晕瘫坐）`
4. **其他工具**：相对迹公式（relative trace formulas）、Iwasawa 理论、Bessel 周期、弱化版 Vandiver 猜想；时间线还提到 Stepanov 行列式、Iwasawa 塔、Bost 斜率不等式、Rallis 跷跷板（Rallis see-saw）。（OfficeChai / 腾讯）
5. **人类可读版本**：**未发布**。Claude 生成的非形式化详细稿超过 250,000 页，作者称其为 "slop" 拒绝发布；作者自述「在尚未掌握周边大量数学的情况下就展示了完整论证」，承诺后续简化、做报告、写正式论文，未给日期。（Clarín / MLQ / OfficeChai / 腾讯 / 新浪）
6. **版本层级**：已证 GRH（Dirichlet L 函数）；**Extended Riemann Hypothesis 列为「有更多时间时」的未来工作**。（MLQ / OfficeChai）
7. **20 步时间线**（由 Claude 汇编，作者warn 转述可能「完全说不通」）：起 2026-10-06，终 2026-11-16「GRH」；11-15 单日七条突破。（腾讯 / 新浪 / Clarín / kingy.ai）

---

## 3. ★ 逆推输入清单（为后续「从 Lean 逆推证明过程」整理）

> 性质说明：以下是**能拿到的输入材料的索引**，不是证明本身的解读。逐条附 URL；三角标记为拿到原文全文、方括号标记为拿到确切 URL 但未逐字抓取。

### (a) 定理陈述原文
1.【已拿到原文】Mathlib 中 `RiemannHypothesis` 的定义（MIT 页面逐字）：
   ```
   def RiemannHypothesis : Prop :=
     ∀ (s : ℂ) (_ : riemannZeta s = 0) (_ : ¬∃ n : ℕ, s = -2 * (n + 1)) (_ : s ≠ 1), s.re = 1 / 2
   ```
   URL：`https://math.mit.edu/~drew/grh/`（节 `What was verified`）
2.【已拿到原文】顶层定理名（MIT audit.log 逐字）：`riemannHypothesis : RiemannHypothesis`，声明于模块 `Main`；同模块另有 `riemannZeta_eq_zero_re`、`LFunction_ne_zero_of_half_lt_re`、`LFunction_eq_zero_re`、`LFunction_eq_zero_re_of_isPrimitive`。
   URL：`https://math.mit.edu/~drew/grh/audit.log`
3.【已拿到 URL】独立验证项目的 5 个 challenge 声明所在文件：
   - `https://github.com/Deicyde/riemann-hypothesis/blob/main/src/Statements.lean`（声明：`RiemannZetaZerosOnLine`、`DirichletLNonvanishingRightOfHalf`、`DirichletLZerosOnLine`、`PrimitiveDirichletLZerosOnLine`）
   - `https://github.com/Deicyde/riemann-hypothesis/blob/main/src/Main.lean`（顶层行逐字：`theorem riemannHypothesis : RiemannHypothesis := rhOfAll`）
4.【已拿到 URL】可机器读取的定理陈述导出（带 `sorry`、仅 import Mathlib）：`https://math.mit.edu/~drew/grh/challenge.ndjson.zst`（47 MB，解压 350,862,970 bytes，unpacked sha256 `bd484ac9886a96f26b1d9a7a59ebd45b7a70d102c015d4f31f9302bbdd6ab2b9`）
5.【已拿到原文】MIT 对 GRH 范围的界定逐字：`and four further theorems, including the analogous statement for all Dirichlet L-functions. "GRH" here means Dirichlet L-functions only.`
   URL：`https://math.mit.edu/~drew/grh/`

### (b) 公理 / 假设清单
1.【已拿到原文】五个顶层定理的公理依赖（MIT audit.log 逐字）：`'riemannHypothesis' depends on axioms: [propext, Classical.choice, Quot.sound]`，其余四个同样。
   URL：`https://math.mit.edu/~drew/grh/audit.log`
2.【已拿到原文】环境中的全部 15 条公理（MIT audit.log 逐字）：`(Classical.choice, Init.Prelude), (Lean.ofReduceBool, Init.Core), (Lean.ofReduceNat, Init.Core), (Lean.trustCompiler, Init.Core), (Quot.lcInv, Init.Prelude), (Quot.sound, Init.Core), (isScalarObj, Init.Prelude), (lcAny, Init.Prelude), (lcCast, Init.Prelude), (lcErased, Init.Prelude), (lcProof, Init.Prelude), (lcUnreachable, Init.Prelude), (lcVoid, Init.Prelude), (propext, Init.Core), (sorryAx, Init.Prelude)`；MIT Summary 逐字：`No axioms declared outside Lean core` — `15 axioms in the environment, all in Init`。
3.【已拿到 URL】生成公理审计的 Lean 文件：`https://math.mit.edu/~drew/grh/Audit.lean`
4. 注：`sorryAx` 在环境公理表中出现是因为 challenge export 含 `sorry`；这不等于提交证明使用 sorry，需查 solution export 区分。

### (c) 代码依赖结构线索
1.【已拿到原文，出处同 1.1】版本信息逐字：`Lean v4.33.1 (commit 819816b2); Mathlib tag v4.33.1 (0df444a3)`、`lean4export tag v4.33.0 (15f6055e)`、Judge 用 `Lean v4.36.0`。
2. 规模：175,187 modules 全部编译（`zero errors; finished 16:43:21 UTC`）；audit.log 统计 `modules imported: 186200; constants: 2701132; theorems: 2084256`。
3. 源档案：`lean.tar.gz`，14 部分合并，`239,275,554 bytes`，sha256 `9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc`；镜像 `https://github.com/Deicyde/riemann-hypothesis` 除 Comparator 文件外逐字节相同。
4. 导出清单/目标表：Deicyde 仓的 `export-targets.txt`；`https://math.mit.edu/~drew/grh/comparator.json`；`https://math.mit.edu/~drew/grh/SHA256SUMS`；`https://math.mit.edu/~drew/grh/project-oleans.sha256`（29 MB，每个 .olean 的 sha256）。
5. 完整 proof export（逆推的物料本体）：`https://math.mit.edu/~drew/grh/solution.ndjson.zst`，7,699,933,588 bytes，解压 61,570,595,788 bytes，compressed sha256 `61a8938ad5f5e57397c1c0e3b954916af546d9004ade01c3555744a681b05381`。
6. 独立内核/工具链（用于交叉核对 import 图）：`https://github.com/digama0/lean4lean`（67f63b0c）、`https://github.com/leanprover/con-leche`（master cd2eceab / compare 分支 5dc36dc1，含 `--compare` 实现 Comparator 的 statement matching）。
7. 作者原始 14 卷下载：`https://alpo.ge/lean.tar.gz.part01` ~ `https://alpo.ge/lean.tar.gz.part14`；第三方镜像 `https://github.com/CocoSgt/riemann-hypothesis-lean`。

### (d) 任何「证明大纲」文字
1. 四页说明 PDF：`https://alpo.ge/remark.pdf`（标题《Remark》，2026-11-20，无署名、作者未在社媒分享；内容见第 2 节转述）。
2. MIT 页面「Caveats」段（逐字可见，描述验证边界）：`The build ran on cloud machines without a sandbox, and compiled each file by calling lean directly with the same inputs lake supplies, not through lake build.` URL：`https://math.mit.edu/~drew/grh/`
3. 前文 2.1~2.7 的策略描述（von Mangoldt + theta lift 链 √X + Stepanov + 迹公式/Iwasawa/Bessel/Vandiver）即为目前唯一公开的大纲级文字；作者承诺的「人类可读版本」**尚不存在**。

### (e) 8 月前史论文（同一方法谱系，逆推时可作为"早期版本"对照）
- arXiv：`https://arxiv.org/abs/2608.13637`，作者 Levent Alpöge 与 Ralph Furman（Google 收录标注 2026-08-13）；主题：临界线上的零点比例/单纯性下界 41.6% → 67.25%。
- Anthropic 官方研究页（2026-08-10，更新 08-13）：`https://www.anthropic.com/research/riemann-zeta`；MLQ 给出的另一个 URL 形式 `https://www.anthropic.com/riemann-zeta`（抓取时连接关闭）。
- 67% 结果解释：`https://kingy.ai/blog/claude-riemann-hypothesis-67-percent-result/`
- 2026-10 OpenAI「The Quasi-Riemann Hypothesis」：收录于 722 篇手稿 dump；OfficeChai 引 Rutgers 的 Alex Kontorovich：`it would be an instant Fields Medal if a human had done it`。

---

## 4. 关键数字交叉比对（哪些来源说哪些、有无矛盾）

| 数字 | 出处与逐字说法 | 一致性判定 |
|---|---|---|
| 17,000,000 行 | Aaronson：`17.4 million (!!) lines of Lean code`；同文后文 `17 million lines of Lean` | 仅 Aaronson 一手给出；为「行数」。与「175,187 文件」不是同一维度，勿混。 |
| 14 卷 | Clarín `14 archive parts`；MLQ `14 compressed archive parts`；kingy `all 14 files`；腾讯/新浪 `14 个压缩文件`；MIT `the 14 parts from alpo.ge joined` | **完全一致**，五源以上交叉确认。 |
| 250,000 页 | Clarín `full written version... ran past 250,000 pages`；MLQ `informal, AI-generated write-up ran past 250,000 pages`；OfficeChai `informal, AI-generated draft material ran past 250,000 pages`；腾讯/新浪 `超过 25 万页` | 指**未发布的人类可读详细稿**，非 Lean 代码本身；报道偶有混用，需注意。 |
| 175,187 文件 | MIT `All 175,187 modules compile`；Clarín `175,187 source files`；kingy `175,187 Lean source files`；腾讯/新浪无此数 | **一致**（同一维度：文件/模块数）。 |
| 3 条公理 | MIT `depend only on the three standard axioms` + audit.log 三元组 `propext, Classical.choice, Quot.sound`；kingy `only Lean's three standard axioms` | **一致且有一手日志**。注意区别于「环境共 15 条 Init 公理」。 |
| 504 文件 / 158,000 行（Cathedral） | **本轮检索未找到独立来源交叉印证**；四页说明与主流报道均未提此数 | **待核实**：2026-06 Zenodo"The Cathedral"Lean 项目的数字本次未获独立来源印证。 |
| 239 MB / 239,275,554 bytes | Clarín `roughly 239 megabytes`；kingy 与 MIT 均为精确值 `239,275,554 bytes` | **一致**，精确值来自 MIT/kingy。 |
| 67.25% / 67.2% / 67% | Clarín `67.25%`；kingy `67.25%`；MLQ `67.2%`；新浪 `67.2%`；OfficeChai `41% to 67%` | 67.25 与 67.2 为同一结果的不同精度；起点 41.6%（Clarín/MLQ/新浪）与 41%（OfficeChai/kingy）并存，属四舍五入差异，非实质矛盾。 |

---

## 5. 当前（2026-11-21）验证状态最新进展

1. **是否已有正式接受：没有。** Clay 官方 Riemann Hypothesis 页面 `https://www.claymath.org/millennium/riemann-hypothesis/` 抓取时仍显示 `Unsolved`，并写 `This has been checked for the first 10,000,000,000,000 solutions.`；Clarín、MLQ、kingy 三家均记载「as of Friday（11-20）仍列 unsolved」。
2. **Clay 获奖流程（最早 2028 年底）**：Clarín `A two-year waiting period` + `Formal publication` + `Global consensus`；规则页 `https://www.claymath.org/millennium-problems/rules/`。Clay 不接受直接投稿。
3. **Sutherland 验证进度（截至其页面 2026-11-20 22:05 UTC）**：
   - 已完成 pass：175,187 模块全编译、五定理公理审计、无核外公理、Mathlib 文件与官方 cache 逐字节一致、`leanchecker` 逐模块 replay（约 18:17 UTC 完成）、challenge export 与 Deicyde CI 匹配（sha256 `bd484ac9…b2b9`）、nanoda 独立 Rust 内核接受全量 export（19:05:45 UTC，27 分 42 秒 / 80 线程，日志 `https://math.mit.edu/~drew/grh/nanoda.log`）、con-leche master 接受 1,468,418 declarations（21:19:04 UTC）、con-leche `--compare` 五定理全部 justified（21:19:12 UTC）、lean4lean 通过 175,187/175,187（约 21:55 UTC；其中 5 个模块因默认步数限制超时、提高 fuel 后通过）。
   - 仍 running：`Lean default kernel and Lean paranoid kernel on the export`（单线程）；`lake comparator (Lean v4.36.0) end to end`——其 statement comparison 与 axiom check 已在单次运行中通过，kernels 进行中。
   - declined/N.A.：与 Lean v4.36.0 捆绑的旧版 con-leche/con-ron（exit 2，因不支持某个 reflexive inductive type，非拒绝）；捆绑 lean4lean 无法读 v4.33 core（"unknown constant 'Decidable.intro'"）。
4. **新质疑**：Aaronson 的 Lean 内核 bug / 类型论可靠性（1.2）；Bastounis–Circelli–Hansen 预印本「no confidence」形式化错配（1.3）；Tao 的黑箱担忧与 25 位 Fields 奖得主联合倡议（1.5）；同日出现**第二个 GRH 声明**（Ravi Vakil 纯人工证明、Alessandro Masullo 人类证明），使「谁先证明」成为新争议点（1.2、1.4）。
5. **机构声明**：Anthropic 截至 11-20 美东上午未发官方公司声明（Scientific American / MLQ / OfficeChai 均载）；四页说明无署名、作者未在社媒分享（OfficeChai）。
6. **后续更新声明**：Scientific American 编辑注 `This is a breaking news story and will be updated.`；Sutherland 页面 `this page will be updated as the remaining checks finish`。两者都是持续更新的活页面，这两个页面持续更新。
7. **背景线索核对**：2026-10 OpenAI 722 篇手稿（含 "Quasi-Riemann Hypothesis"）作为前史，被 OfficeChai、MLQ、Scientific American 一致记载，与 2026-11 主流报道的 722 篇手稿口径一致。

---

## 6. 未完成项与后续建议
1. **多语种逐字引文**（日/德/法/俄/印）未取得。
2. **Cathedral（Zenodo 2026-06）504 文件 / 158,000 行**未找到独立来源。
3. **四页说明 PDF 原文逐字**：本轮仅取到二手转述；PDF 原件在 `https://alpo.ge/remark.pdf`，文字层提取方式见 AGENT.md 第 1 步。
4. **GitHub issue / 代码许可证与署名细节**：Deicyde 镜像仓库的 LICENSE、commit 作者、issue 讨论因抓取失败未取证。
5. 标注 [已拿到 URL] 的条目表示链接确定但正文未逐字读取。
