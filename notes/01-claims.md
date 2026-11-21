# 官方／一手声称与文献调研笔记：Alpöge 声称与 Claude 合作证明黎曼猜想

调研执行时间：2026-11-21（Asia/Shanghai）。
调研角度：**官方／一手声称与文献本身**。
14 卷 Lean 源码的下载与结构核查见 `notes/02-lean-code.md`。所有「声称」「宣称」均按来源原样记录。

检索手段：Bing（8 组查询词）与 Google（6 组）检索、脚本抓页面正文、PDF 解析器提取文本层。中英文查询词均已覆盖。知乎两个页面返回 403，Hugging Face 与 anthropic.com/news 抓取 SSL 失败。

---

## 一、一手材料：Alpöge 个人网站四页说明

### 1.1 确切 URL 与获取状态

- 声明 PDF：**https://alpo.ge/remark.pdf**
- 作者主页：**https://alpo.ge/**（抓取成功）
- Lean 代码 14 卷（PDF 脚注 1 给出）：
  `https://alpo.ge/lean.tar.gz.part01` ～ `https://alpo.ge/lean.tar.gz.part14`
- 获取状态：`remark.pdf` **下载成功**，245114 字节，保存为 `remark.pdf`；`pdfplumber` 成功提取文本层，10986 字符（pdfplumber 提取；另一次用 pdfminer 提取得 12,222 字符，差异来自解析器对空格与数学符号的处理）。共 4 页（PDF 正文页码标注至「3」，第 4 页含脚注与时间线）。
- 文本层缺陷：PDF 提取时**词间空格大量丢失**（例如 `IbelieveIhave,workingwithClaude,discoveredaproof1 oftheRiemann`），数学符号上下标也被打散。下文逐字引文是**仅还原空格与断行、未改动任何用词与词序**的可读版；原始提取文本可按 AGENT.md 第 1 步重新生成，用于逐字核对。

### 1.2 开篇声称（逐字原文，第 1 页）

> "The Riemann Hypothesis is the classic problem in number theory. It asks for optimal control on the count of prime numbers of a given size. Its shadows have guided mathematicians for 150 years, and its many rhymes and reflections, even reaching physics, taught us the unity of our fields."

> "I believe I have, working with Claude, discovered a proof [footnote 1] of the Riemann Hypothesis. In fact, the method easily extends much further, resolving the so-called Generalized Riemann Hypothesis, and I expect, with more time, the Extended Riemann Hypothesis too. Claude and I have also formalized the arguments without assumptions. It is natural to feel that this is an end, in my case also a loss of a principal guiding light, but what I saw during the course of progress so amazed me that I believe it is one dawn across science of something far greater."

（该句与 PDF 原文逐字一致。）

### 1.3 关于「为何 AI 才能做到」与分工（逐字原文）

> "It was my secret in my student days that my plans were for this problem in decades, which meant then that I tried to become familiar with all tools that I could possibly recognize in the problem when it was time. Claude and I have had a great time together with such breadth in recent months, but this time Claude far outpaced me near the finish line. No, Claude flew, and danced, and sparkled near that finish line. I had us on the line of combining two quite different ideas present in a toy setting, one of which was already new and shocking, making very simple one of the shadows of Riemann over which we mathematicians felt most triumph. I guided us to the right situation analogous to that toy setting, and then Claude began to leave me in awe."

关键分工句：**「我」负责把玩具模型里的两个想法组合起来、并引导到与之类比的真实情形；Claude 在终点线前「飞、跳舞、闪光」（flew, danced, and sparkled），并补足「我」不掌握的高阶技术。**

### 1.4 证明策略与所用数学工具（逐字原文）

> "Tens of such occurred, but I will give one line which will suffice to make fellow mathematicians fall out of their chairs. In Stepanov's argument resolving Riemann over function fields one uses a divisor involving multiplicity. This argument is a descendant of Stepanov's, but multiplicity is hard to find over the integers, corresponding to something like differentiating whole numbers. The simple, new, Fourier-analytic ideas in the toy setting led us to examine chaining theta lifts if we were to have the same tricks recur but work in a natively automorphic manner. The line: for the von Mangoldt sum of length X, Claude's suggested length of chain of the theta lifts, in view of each mimicking multiplicity, was √X. (And yet, what else could it be?)"

这是全文最具体的「一行」：Stepanov 函数域论证的后裔；用**串联 theta lift** 在整数上模仿「重数」（multiplicity）；Claude 建议对长度 X 的 von Mangoldt 和取长度 **√X** 的 theta lift 链。

> "An astounding amount of mathematics is present in the argument, far more than I had imagined would be necessary as a student. My intuition insists it will be massively simplified, likely immediately. But I hope in having the experience of seeing all of number theory's beauty brought together I am just one of the first, and those who love the mathematics will all be able to enjoy such transcendence in the future."

> "The argument is incredibly long at the moment, and while its formalization is finished and it is routine to produce a huge, unreadable document containing sufficient detail, in my view of my path I have just taken a step. I intend to do my best to understand and impart understanding of this argument and its certain simplifications, to speak about it, to write it properly."

> "In a time of revolution in mathematics I hope the argument provides a glimpse towards the miracles that lie ahead, and shows us the joy and wonder we are about to find in the simplest things."

时间线条目里出现的工具名（照抄，不做数学解释）：Stepanov determinant；D_p ≡ 0 on a CM curve；invariant derivation；qN = Mk + R；sixfold cover of GL5；Poincaré bundle；W_S(1/2) ≥ 0 via a determinant；Guinand-Weil；quadratic form；iterated theta lifts U(X_0) → U(X_1) → ··· → U(X_m)；totally definite unitary groups；π ↦ π ⊞ μ[t]；Chowla-Selberg；Weyl；Siegel mass；Rallis seesaw（Bessel period）；Bost slope inequality；relative trace formula；GGP periods；Iwasawa tower（Behrend）；Vandiver 弱化；K-groups of Z；covolumes；theta series' index。

### 1.5 脚注原文

- 脚注 1（PDF 第 1 页）：`https://alpo.ge/lean.tar.gz.part01,...,https://alpo.ge/lean.tar.gz.part14.`
- 脚注 2：「I of course amused myself, and hit 250,000+ pages.」
- 关于 GRH「book」的注释：「Claude cutely called it the GRH "book"—I looked at Theorem 1 and it began by noting that because the reals are totally ordered if σ ≠ 1/2 then σ > 1/2 or σ < 1/2.」
- 脚注 4：「The parts in which my vague intuition worked out due to Claude's technical mastery are of course less familiar to me today, and probably I'll have to edit some of these descriptions on reflection. The parts in which Claude is the expert and not me I am sort of paraphrasing a "slop" summary, which might make zero sense, and I apologize, though the Lean should suffice to clarify everything. I'm just too inexpert in the relevant stuff at the moment, I hope the reader sees the breadth of this argument!」
- 脚注 6：「This is still pretty strong!! I believe vanishing of a number of K-groups of Z follows, and cyclicity up to 100ish maybe? I had Lichtenbaum, so I knew we would have a lot of slack in fighting these K's.」
- 脚注 7：「This is the sort of trick that intuitively to me is obviously going to be unnecessary.」

### 1.6 关于披露时机与 Anthropic 领导层（逐字原文，第 2 页）

> "The precise timing is horrible. If I could have, I'd have finished this all much earlier. Hell, strictly before this age would have sure been nice. This all happened for me late Sunday night, and I messaged Dario, 'Hypothetically, if I were to have ...', eventually asking about my ability to announce. We went and figured it out and the fact is that there was a constraint to respect, though I did ask about progressively more bizarre ideas. This problem has been the sacred object in mathematics for me since my childhood, and I am very touchy about the treatment of great intellectual goals, though seriously imperfect. Our leadership and the few trusted mathematicians I consulted hold extremely similar views on the right things to do in these circumstances (it is occasionally possible for physicists and mathematicians to agree...), and I am clearly unilaterally responsible for feeling I just could not live if I waited even longer. There is an obvious cynical view, to do with the timing, but it is false, and I am pleased that I am in circumstances where it is obviously false."

要点：他称关键发现发生在「周日深夜」，随后给 Dario（即 Dario Amodei）发消息询问能否公布；提到「有一个必须遵守的约束（a constraint to respect）」「我们的领导层和少数受信任的数学家持有高度一致的看法」。**这是一手材料里唯一涉及 Anthropic 公司层面的表述，但它是个人叙述，不是公司公告。**

> "This puts us in the rather extraordinary situation this age has made possible, in which I am presenting the world with a complete argument before having mastered a massive neighbourhood around its mathematics. It being obviously inappropriate to attempt to post some massive unreadable travesty of 'slop', I decided to only present Lean."

### 1.7 致谢（逐字原文）

> "I have a lot of people I want to praise, and I will do a proper job in a proper writeup, alas pardon the accidental omissions—among many others, Manjul Bhargava, Kai Gill, Hamish Kerr, Max Leiter, John Ma, Tal Magdish, Chris Paika, Maksym Radziwiłł, Jascha Sohl-Dickstein, Peter Sarnak, Jarred Sumner, Amir Tajdini, and especially David Dohan, Ben Michel, Ralph Furman, Eric Price, and Cora Vinther, and my miraculous colleagues. I want them to be proud."

### 1.8 关于 OpenAI 的致谢段（逐字原文，与前史直接相关）

> "I have benefited from the good taste of the people at OpenAI involved with various mathematical results, so I am quite happy to praise them too. The Ehrhart conjecture was one such tasteful choice some time ago; playing further and working out the equality case I saw accurate handling of solutions of the Monge-Ampère equation and immediately understood Navier-Stokes was going to fall. Similarly, their quiet inclusion of the Siegel argument, whose result was strictly dominated by the (2,3,6)-triangle group trick, again showed great taste; though I did not comment so publicly, I immediately recognized it as Stepanov without multiplicity, though it was only clear to me during this rather harebrained tornado through the whole of number theory that this problem would fall given enough all-nighters and physical toll when I had the qN = Mk + R sign-only proof of Weil and Deligne."

### 1.9 PDF 附的 20 条时间线（逐字照抄，日期原样）

导语：「So here is a rough timeline Claude provided after a thorough search.」并紧接着说明部分描述是对 Claude 说法的转述、可能需要修改。

1. 10/6/26: Stepanov determinant.
2. 10/9/26: D_p ≡ 0 on a CM curve for p inert in K, D the invariant derivation, and D a·θ(0)/Ω_a is divisible by √p_a + √p_a^3 + ··· − O(log a).
3. 10/19/26+: qN = Mk + R, using x_1·x_2 + ··· + x_{k−1}·x_k, wanting "automorphy" of N since μ is automorphic from θ on sixfold cover of GL5 as a period integral of the thetas, quadric has natural interpretation on corresponding CM e.g. C/Z[ω] as Weil, put Poincaré bundle on the edges to get θs.
4. 10/6/26+: W_S(1/2) ≥ 0 via a determinant, interpret as Stepanov with multiplicity one again; bootstrap!!
5. 10/20/26: definitively switch to Λ aka ∑_{n≤X} Λ(n)·((X/n) − 1) − X log X + (1 + γ)·X; Guinand-Weil had the sign, variance test function trick to make things look trace-formula-like (but how can I pigeonhole / Turán over n? X? it?), Mehta-like integral over twists or at infinity...
6. 10/8/26: W_{(0,1]}(1/2 + (∼Xη)·1_{(0,1/X]}) ≥ 0 gives > 1 − η.
7. 10/14/26: Determinants for W(·)'s containing the desired periods, indeed GGP periods, with values 0,1 meaning interlacing weights, or up to m non-interlacing ones.
8. 11/15/26: Iterated theta lifts U(X_0) → U(X_1) → ··· → U(X_m) between totally definite unitary groups, π ↦ π ⊞ μ_j[t], wins multiplicity m. Time to take m like √X...
9. 11/15/26: Norms of the lifts and level and cancellation of order-m terms with proven divisibility via Chowla-Selberg, Weyl, and Siegel mass.
10. 11/15/26: Sufficient lower bound on divisibility of the m-fold iterated lift.
11. 10/6/26: Iwasawa tower via analogy to pigeonhole on F_{q^n}/F_q.
12. 11/15/26: Seesaw: the period of the iterated lift is a Bessel period independent of the Hecke characters, Rallis.
13. 10/6/26: Bost slope inequality.
14. 11/15/26: precise computation of the "cost" of each lift, showing that one would stop at 2/3 a priori, attempt to get a main term and transfer constant to a function field or kill via twisting, need for "division".
15. 11/15/26: Glue together an integral structure across Hecke characters for a "gcd" i.e. extra averaging.
16. 10/14/26: Relative trace formula, seen in a different half-plane argument via sixfold cover of GL5 automorphy of μ and reducing via Cauchy-Schwartz to a bound on the norm of a suitably projected Poincaré series, approximate orthogonality of the columns of the integral structure.
17. 11/15/26: for the classes in H^1(E^o[ℓ](1), Q_ℓ/Z_ℓ(−e)) that we couldn't kill by twisting, fight for a significant weakening of Vandiver.
18. 11/15/26: computation of covolumes, theta series' index, congruences between lifts and forms.
19. 10/19/26: precise choice of the Iwasawa tower, specifically using Behrend!!
20. **11/16/26: GRH.**

（注：PDF 条目序号与时间顺序并不严格递增，原文如此，照抄。）

---

## 二、Anthropic 官方渠道核查

结论：**截至 2026-11-21，未检索到 Anthropic 官方博客、官方新闻稿或官方社媒就本次声称发表声明**。现有材料均指向这是一份个人声明。

证据：

1. AICoder 明确写：「**这是个人声明，不是 Anthropic 官方公告，也尚未被独立证实。**」
   URL：https://www.aicoder.com/zh/news/news-20261120-anthropic-mathematician-levent-alpoge-claims-riemann-hypothesis-grh-proof-with-claude-lean-4-formalization-unverified
2. Scientific American 与 New Scientist 的措辞均为「Anthropic mathematician」（Anthropic 的数学家），媒体把它当作作者身份描述，未引用 Anthropic 官方发言。
3. PDF 中作者自述已给 Dario Amodei 发消息、咨询领导层（见 1.6），但这是作者单方叙述，不构成公司背书。
4. `https://www.anthropic.com/news` 抓取时 SSL 握手失败（`UNEXPECTED_EOF_WHILE_READING`），未能直接核验其新闻列表；Bing 对 `site:anthropic.com Riemann hypothesis Claude proof` 的查询未返回任何相关官方页面（检索原始记录未随仓库发布）。此为本轮检索阴性结果。
5. Unite.AI 提到的是 **2026-08-10 Anthropic 关于 ζ 零点下界的先前工作报道**，不是本次声明：「Anthropic于2026年8月10日报道，Claude的一个尚未发布的研究版本将满足黎曼猜想的ζ零点比例的长期下界从41.6%提高到了67.2%；报道同时明确指出，Claude并未证明该猜想，而且预计这些技术无法导出证明。」不要与本次公告混淆。

---

## 三、权威英文媒体一手报道

### 3.1 Scientific American

- URL：https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/
- 标题：Anthropic mathematician claims to prove Riemann hypothesis, most famous problem in all of math
- 发布时间：**November 20, 2026**；作者 By Joseph Howlett，edited by Claire Cameron；标注「4 min read」
- 逐字引文：

> "Levent Alpöge, a mathematician and employee at artificial intelligence company Anthropic, claimed on Friday to have proven the most famous problem in all of mathematics using a large language model (LLM). 'I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis,' Alpöge wrote in a statement posted on his website."

> "The first chink in the problem's armor appeared in October, when one of the proofs in OpenAI's massive dump of math results made significant progress on making sense of the primes. Now the long struggle appears over."

（这是「2026 年 10 月 OpenAI 成果作为前史」的一处媒体出处，见第六节。）

> "Alpöge posted his Claude proof coded in the programming language Lean, which verifies the mathematical correctness of each argument. Issues can arise in Lean, but it shouldn't take long for experts to verify whether the hypothesis is properly coded, and if the proof is correct."

> "The announcement comes just a day after Anthropic's record-breaking debut on the stock market. Alpöge says his proof confirms the 'Generalized Riemann Hypothesis,' a gargantuan expansion on Riemann's initial guess that undergirds the entire mathematical universe with untold hidden structure."

> "'If the proof is correct and understood, this completes a dictionary between different branches of math where the connections were only partial,' says Hector Pasten, a mathematician at the Pontifical Catholic University of Chile, 'something like a Rosetta Stone.'"

### 3.2 New Scientist

- URL：https://www.newscientist.com/article/2600134-anthropic-mathematician-claims-ai-assisted-proof-of-the-riemann-hypothesis/
- 标题：Anthropic mathematician claims AI-assisted proof of the Riemann hypothesis
- 发布时间：**20 November 2026**；作者 By Matthew Sparkes
- 逐字引文：

> "A mathematician at AI company Anthropic claims to have discovered, and verified with formalisation, a proof of the Riemann hypothesis – one of the most infamous and thorny problems in all of mathematics."

> "'I believe I have, working with Claude, discovered a proof of the Riemann hypothesis,' writes Alpöge. 'It is natural to feel that this is an end, in my case a loss of a principal guiding light, but what I saw during the course of progress so amazed me that I believe it is one dawn across science of something far greater.'"

> "Levent Alpöge at Anthropic published a statement today claiming to have solved the problem with the aid of Anthropic's Claude AI."

> "Alpöge hasn't published a human-readable version of the proof, saying it would be 'obviously inappropriate to attempt to post some massive unreadable travesty of "slop"', though he says an unpublished version ran to more than 250,000 pages."

> "'Let's see if it compiles first,' says Kevin Buzzard at Imperial College London, reacting to the news. 'Might be a while before we find the relevant experts who are able to tell us the thrust of the argument, assuming it's all true, of course, which it probably will be because of course Alpöge will have [a] huge amount of egg on [his] face if the Lean code doesn't compile or is deficient in some way.'"

> "Alpöge says the proof may also help solve the generalised Riemann hypothesis (GRH), which widens out the concept to different properties of prime numbers... 'There are numerous results that are conditional on the generalised Riemann hypothesis, and now those are theorems!' says James McKee at Royal Holloway University of London."

> "'What is both unsettling and tremendously exciting, is the rapid progress in recent months towards automated proofs of significant mathematical results. It is both a wonderful time and a terrible time to be a research mathematician.'"

文中相关阅读链接（未展开）：「OpenAI has dumped 722 maths papers – now it must clean up the mess」。

### 3.3 Unite.AI（英文原站中文版页面）

- URL：https://www.unite.ai/zh-cn/levent-alpoges-unverified-riemann-claim-puts-claudes-lean-code-in-focus/
- 发布时间：**2026年11月20日**；作者 Jonas Reeve（页面注明作者为 AI 生成、编辑审阅）
- 逐字引文（中文页面照抄）：
  > 「Levent Alpöge于2026年11月20日表示，他相信自己与Claude发现了黎曼猜想的证明，以及一种解决广义黎曼猜想的方法。这位Anthropic数学家发布了一份个人声明，其中附有Lean代码链接，但没有指出任何对完整论证的独立验证。」
  > 「声明中由Claude搜索后提供的粗略时间线记载：2026年10月6日得到了Stepanov行列式；2026年11月15日进行了迭代theta提升及相关计算；2026年11月16日得出了广义结果。」
  > 「这套论证建立在Stepanov的函数域方法之上；该方法利用除子得到重数，而如何在整数上重现这一效果，是一个核心难题。傅里叶分析的思路使两人转向自守框架中的反复theta提升；Claude提出了一条长度为 X 的链，用于处理一个长度为 X 的底层求和。」

---

## 四、中文媒体报道（逐字摘录与时间）

### 4.1 腾讯新闻「赛博禅心」

- URL：https://news.qq.com/rain/a/20261120A0GCJC00
- 发布时间：**2026-11-20 22:55 发布于广东**
- 逐字引文：
  > 「刚刚，Anthropic 的数学家 Levent Alpöge 宣布，他和 Claude 一起找到了黎曼猜想的证明。证明代码已经放出，原始下载链接如下（其中的 x 替换为 01-14，共 14 个）：https://alpo.ge/lean.tar.gz.part[x]」
  > 「为了方便大家下载，我传到了 Github（版权归属原作者）：https://github.com/CocoSgt/riemann-hypothesis-lean」
  > 「而按 Alpöge 的说法，同一套方法能直接推到广义黎曼猜想（GRH），整个论证已经在 Lean 里完整形式化，没有任何额外假设；人类能读的版本还没写（Claude 生成的详细稿超过了 25 万页），所以这次只放出了 Lean 代码。」
  > 「这一切发生在周日深夜。他在发出来之前给 Dario 发消息问『假设我……』，确认自己能不能公布（有一个约束需要遵守）。他还补了一句：『有一种显而易见的犬儒解读，和时机有关，但它是错的。』」
  > 「Alpöge 说，光这一句话就足以让同行从椅子上摔下来（堪称眩晕瘫坐）」
  > 时间线摘要：「10 月 6 日：从 Stepanov 行列式、Iwasawa 塔与 Bost 斜率不等式起步；10 月 20 日：转向 von Mangoldt 函数的加权和；11 月 15 日：这一天标了七条……11 月 16 日：走通 GRH。」

### 4.2 腾讯新闻「AI寒武纪」

- URL：https://news.qq.com/rain/a/20261121A015UQ00
- 发布时间：**2026-11-21 01:02 发布于日本**
- 逐字引文：
  > 「个人网站声明：https://alpo.ge/remark.pdf」
  > 「困扰人类167年的数学圣杯，现在被完整的 形式化代码 攻克。」
  > 「他在个人网站上直接放出了拆分成14个压缩包的完整 Lean 工程文件，供全世界数学家下载查验。https://alpo.ge/lean.tar.gz.part01 ... https://alpo.ge/lean.tar.gz.part14.」
  > 「那份文档超过了25万页。」
  > 「麻省理工学院数学家Andrew Sutherland曾经谈到过这种场景。他表示，一个能够证明黎曼猜想的AI是令人敬畏的，如果这一天真的到来，数学家会不会丢掉工作，将是全人类最微不足道的烦恼。」
  > 「今年AI在数学领域的爆发早有征兆。7月，Alpöge就曾发布过Claude对雅可比猜想的求解方案。9月，围绕OpenAI的一系列数学研究成果，纽大数学家Tristan Buckmaster曾与OpenAI就纳维斯托克斯方程等研究归属产生争议，Alpöge也身处讨论中心。」

注意：该文提到 Sutherland 的是**他过去的一段评论**，并非「他编译了代码」。

### 4.3 新浪财经（转量子位）

- URL：https://finance.sina.com.cn/stock/t/2026-11-21/doc-inizmfew3857939.shtml
- 发布时间：**2026年11月21日 00:06**，来源量子位
- 逐字引文：
  > 「刚刚，34岁的Anthropic数学家Levent Alpöge放出一份PDF文档，声称自己与Claude合作，找到了黎曼猜想（数学界最有名、悬赏100万美元的百年难题）的证明。」
  > 「Alpöge没有发表完整论文，选择公布了整套Lean形式化证明代码，分成14个压缩文件供外界下载。如果把它转成一份包含全部细节的文档，篇幅会超过25万页。」
  > 「Claude 还提出了一个关键参数：即对于长度为X的von Mangoldt求和，Theta提升链的长度应当达到√X。除此之外，整套论证涉及了相对迹公式、Iwasawa理论、Bessel周期等多个现代数论工具。」
  > 「不过要注意，Lean能够验证形式化系统中的逻辑推导，却无法自动保证研究者输入的定理定义准确对应黎曼猜想。因此，外部研究者还需要检查代码能否完整编译、使用了哪些公理，以及最终定理是否真的等价于黎曼猜想。」

### 4.4 搜狐（转机器之心）

- URL：https://www.sohu.com/a/1104307250_122014422
- 发布时间：**2026-11-21 00:27 发布于北京市**，来源机器之心
- 逐字引文：
  > 「刚刚，Anthropic 数学家 Levent Alpöge 在自己的个人网站上上传了一份 PDF 文件，该文件仅有 4 页，但却给出了一个足以载入史册的重磅成果——其宣称在 Claude 的帮助下，著名的黎曼猜想（Riemann Hypothesis）已经得到了证明，并给出了 Lean 代码。」
  > 「报告地址：https://alpo.ge/remark.pdf  Lean 证明：https://alpo.ge/lean.tar.gz.part01 直到 https://alpo.ge/lean.tar.gz.part14」
  > 「它包含成果声明、方法线索、致谢和研究时间线。真正承载完整论证的材料，被放在脚注指向的 14 个 Lean 压缩分卷中，整个文稿有 25 万页。」
  > 「15 日，最后一项标为『11/16/26：GRH』。这份由 Claude 整理的回顾，展示了作者所述的研究跨度，也意味着不能把这次工作包装成一次提示词、几小时内完成的解题。」
  > 「因此，『提供了 Lean 文件』『作者称形式化已完成』和『外界已独立验证』对应着不同的证据状态。本文阅读了声明，并核对了相关背景资料，未对这 14 个分卷组成的工程进行编译或证明审计。」

### 4.5 AICoder（结构化的技术核查汇总，信息密度最高）

- URL：https://www.aicoder.com/zh/news/news-20261120-anthropic-mathematician-levent-alpoge-claims-riemann-hypothesis-grh-proof-with-claude-lean-4-formalization-unverified
- 发布时间：**原文发布于 11-20 21:41（北京时间）**；页面标注来源账号 S Scott Armstrong @scottnarmstrong，推特原帖 https://x.com/scottnarmstrong/status/2123773651531305190
- 逐字引文：
  > 「11 月 20 日，Anthropic 员工、数学家 Levent Alpöge 在个人网站 alpo.ge 发布四页说明：『我相信我与 Claude 合作发现了黎曼猜想的证明』，并称方法可推广到广义黎曼猜想（GRH），论证已在无额外假设下完成形式化。他没有发布人类可读论文，只放出分 14 段的 Lean 4 源码包（约 228 MiB、17 万多个 .lean 文件）。目前社区镜像的静态检查未发现 sorry 或 axiom，但完整编译与公理检查仍在进行，结论尚未经独立验证。」
  > 「✓ 来源：Levent Alpöge 本人在 alpo.ge 发布的说明 remark.pdf 与 14 段 Lean 源码；《科学美国人》《新科学人》确认其为 Anthropic 员工」
  > 「✓ 声称范围：黎曼猜想（RH）与广义黎曼猜想（GRH），并预计扩展黎曼猜想也可推广；说明未注明具体 Claude 型号」
  > 「✓ 只发 Lean 4 形式化（toolchain v4.33.1，依赖固定版本 Mathlib），称未删减的人类可读版本超过 25 万页」
  > 「✓ Hugging Face 社区镜像静态检查：sorry、axiom、admit、native_decide 均为 0；完整 lake build 与 #print axioms 尚未完成」
  > 「✓ 目前仍是个人声明：Imperial College 的 Kevin Buzzard 表示『先看能不能编译』，人类专家审读尚待进行」
  > 「这是个人声明，不是 Anthropic 官方公告，也尚未被独立证实。」
  > 镜像地址：「🔗 Mirror of the Lean archive (unverified) huggingface.co/datasets/kzoacn/riemann-lean-archive」

### 4.6 知乎（仅拿到搜索摘要，页面 403）

- URL：https://www.zhihu.com/question/2107239235015263538
- 摘要（Bing 返回的逐字片段）：「近期，levent alpoge 公开了一份名为 remark.pdf`的文档（见 alpo.ge/remark.pdf），宣称自己与 Claude … 11 月 21 日 …」
- 另一条摘要含英文引：「"If there's a Lean proof of the Riemann hypothesis that's like 20 billion tokens long and nobody can read it, that's not a …"」——该片段上下文不完整，来源未确认。
- URL：https://zhuanlan.zhihu.com/p/2107263438028289049（量子位转载，403）；https://www.zhihu.com/question/2075142928394086068（关于 S^6 复结构的旧讨论，2026-09-10，非本事件）

---

## 五、事件时间线（精确汇总）

| 时间 | 事件 | 出处 |
|---|---|---|
| 2026-10-06 | PDF 时间线第 1 条「Stepanov determinant」；第 4、11、13 条也标此日 | `remark.pdf` |
| 2026-10-19 ～ 10-20 | qN = Mk + R；正式转向 Λ 加权和 | `remark.pdf` |
| 2026-11-15（周日）深夜 | 作者自述「This all happened for me late Sunday night」；集中 7 条：迭代 theta lift、Rallis seesaw、Vandiver 弱化等 | `remark.pdf` |
| 2026-11-16 | 时间线第 20 条「GRH」 | `remark.pdf` |
| 周日深夜之后 | 给 Dario Amodei 发消息询问能否公布；提到「有一个必须遵守的约束」 | `remark.pdf` |
| 2026-11-19 | Anthropic 上市（SciAm：公告「just a day after Anthropic's record-breaking debut on the stock market」） | SciAm |
| 2026-11-20（周五） | 个人网站发布 remark.pdf 与 14 卷 Lean 代码；SciAm："claimed on Friday"；New Scientist："published a statement today" | `remark.pdf`、SciAm、New Scientist |
| 2026-11-20 21:41 北京时间 | AICoder 发布 | aicoder.com |
| 2026-11-20 22:55（广东） | 腾讯新闻「赛博禅心」 | news.qq.com |
| 2026-11-20 | Scientific American（November 20, 2026）、New Scientist（20 November 2026）、Unite.AI（2026年11月20日） | 见第三节 |
| 2026-11-21 00:06 | 新浪财经转量子位 | finance.sina.com.cn |
| 2026-11-21 00:27（北京） | 搜狐转机器之心 | sohu.com |
| 2026-11-21 01:02（日本） | 腾讯新闻「AI寒武纪」 | news.qq.com |

时间线的内部张力（如实记录，不做判断）：PDF 说「周日深夜」完成，「精确时机很糟糕」，且需遵守某项约束；对外发布在 11-20（周五）。SciAm 把发布与 Anthropic 上市次日联系起来。

---

## 六、2026 年 10 月 OpenAI 数学成果（突破前史）出处

找到的相关一手／权威出处：

1. **Scientific American**（2026-11-20）：「The first chink in the problem's armor appeared in October, when one of the proofs in OpenAI's massive dump of math results made significant progress on making sense of the primes.」
   URL：https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/
2. **New Scientist** 相关阅读标题：「OpenAI has dumped 722 maths papers – now it must clean up the mess」（作为侧栏链接出现，未展开全文）。
   URL：https://www.newscientist.com/article/2600134-anthropic-mathematician-claims-ai-assisted-proof-of-the-riemann-hypothesis/
3. **Alpöge 自己的致谢段**（`remark.pdf`，见 1.8）点名了 OpenAI 那批成果的具体内容：Ehrhart 猜想、Monge-Ampère 方程等号情形、Navier-Stokes、Siegel argument、(2,3,6)-triangle group trick。
4. **腾讯「AI寒武纪」**（2026-11-21 01:02）补充前史：「9月，围绕OpenAI的一系列数学研究成果，纽大数学家Tristan Buckmaster曾与OpenAI就纳维斯托克斯方程等研究归属产生争议，Alpöge也身处讨论中心。」并提到 7 月 Alpöge 发布过 Claude 对雅可比猜想的求解方案。

**未找到**：OpenAI 官方博客就该批次成果的准确原始 URL。Bing 对 `OpenAI October 2026 mathematics results Ehrhart conjecture Navier Stokes`、`OpenAI math breakthroughs October 2026 AI proofs announcement` 的查询只返回通用导航页（检索原始记录未随仓库发布），Google 返回脚本页无法解析，DuckDuckGo-html 返回 0 条。**「722 篇数学论文」这一具体数字目前只有 New Scientist 侧栏标题这一处来源，本轮未取得第二来源。** 另注意区分：aicoder 页面提到的「Google Research Antigravity /teamwork 证明 2013 年 Courtade–Kumar 猜想（5000 万行 Lean 4）」是 Google 而非 OpenAI 的成果，两者属于不同公司的成果。

Anthropic 前一阶段的 ζ 零点工作（2026-08-10，下界 41.6% → 67.2%，两个 Claude Code 会话、3100 万输出 token、650 个初始想法、约 60 个子代理、Alpöge 与 Ralph Furman 检查、Brian Conrey 与 Dan Goldston 审阅论文）来自 Unite.AI 的转述，原始 Anthropic 官方 URL 亦未取得。

---

## 七、声称的数学内容与公理问题

- **RH 精确形式**：非平凡零点实部 = 1/2（腾讯「赛博禅心」：「黎曼 ζ 函数的非平凡零点全都落在实部为 1/2 的那条直线上」）。
- **GRH**：声称同一方法已解决，PDF 时间线第 20 条「11/16/26: GRH」。
- **Extended Riemann Hypothesis**：只是「I expect, with more time」（期望），**未声称已完成**。
- **「无假设形式化」**：PDF 原文「Claude and I have also formalized the arguments without assumptions」；中文译为「论证已在无额外假设下完成形式化」。
- **公理列表**：**PDF 中没有 axiom 列表**。「只调用 3 条最基础标准公理」这一说法在本次所有检索到的中英文材料中**均未出现**，本轮检索未找到该说法的一手出处。
- 关于公理的**唯一**具体说法来自 aicoder 转述的社区静态检查：「Hugging Face 社区镜像静态检查：sorry、axiom、admit、native_decide 均为 0；完整 lake build 与 #print axioms 尚未完成」。这是**静态文本检查结果**，不是 `#print axioms` 的公理清单，也不等于编译通过。
- Lean 版本：toolchain v4.33.1、依赖固定版本 Mathlib（来源：aicoder）。
- 术语提醒：机器之心指出「Lean 官方文档也明确区分两个问题：一个命题是否获得了有效证明，以及这个命题表达的是否正是人们想证明的东西」——这是转述内容。

---

## 八、未能确认的项

1. **四页说明 PDF 正文**：已下载并提取全文（本节一、完整引文），此项**不是**「找到 URL 但抓不到正文」。
2. **Anthropic 官方声明／博客／社媒**：未找到。检索阴性，且 anthropic.com/news 抓取 SSL 失败。若有官方推文，可能在 @AnthropicAI 或 Dario Amodei 的 X 账号，本次未核验。
3. **「只调用 3 条最基础标准公理」**：未找到任何出处；PDF 无 axiom 列表。
4. **Nyman–Beurling 判据**：用 `Nyman Beurling criterion Alpoge Claude Riemann proof` 检索，返回结果全是作曲家 Michael Nyman 与化学家 May Nyman 的噪声，**未找到与本事件相关的任何来源**。本判据未找到与本事件相关的一手来源。
5. **MIT Andrew Sutherland 在公告当天编译代码**：未找到一手佐证。检索到的 Sutherland 相关内容只是腾讯「AI寒武纪」引用的他**过去的评论**（「一个能够证明黎曼猜想的AI是令人敬畏的……」），并非编译记录。PDF 致谢名单里也没有 Sutherland。
6. **GitHub 镜像**：https://github.com/CocoSgt/riemann-hypothesis-lean （由腾讯「赛博禅心」给出，未打开核验）。
7. **Hugging Face 镜像**：https://huggingface.co/datasets/kzoacn/riemann-lean-archive （抓取时 SSL 失败，内容来自 aicoder 转述）。
8. **X 帖**：https://x.com/scottnarmstrong/status/2123773651531305190 （未打开核验）。
9. **知乎**：两个页面 403，只有 Bing 摘要。
10. **OpenAI 十月成果官方 URL**：未找到；「722 篇」只有 New Scientist 侧栏一处来源。

---

## 九、资料获取方式

- 四页说明 PDF：`https://alpo.ge/remark.pdf`，文字层提取见 AGENT.md 第 1 步
- 14 卷 Lean 源码：`https://alpo.ge/lean.tar.gz.part01` 至 `part14`，下载与解包见 AGENT.md 第 2 至 4 步
- 新闻报道与社区讨论的 URL：`sources/SOURCE_INDEX.md`
- 评论逐字引文：`notes/03-commentary.md`
