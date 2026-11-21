# 03 · 社区与专家评论（Anthropic / Levent Alpöge / Claude 证明黎曼猜想）

> 调研时间：2026-11-21（Asia/Shanghai）。事件日期：2026-11-20。
> 本文件为社区评论归档，引文按来源记录。所有「编译通过」「已被验证」等均为转述来源方的说法，非本文结论。
> 逐字英文原文保留；中文叙述为检索说明。每条尽量附平台、作者、时间、URL。
> 检索手段：Bing / Google / DuckDuckGo、Hacker News Algolia API、站点直取；部分站点（Reddit、Science.org、X/Twitter）被反爬拦截，已在文末「检索限制」列明。

---

## 〇、事件速览与核心事实（供评论定位）

- 原始声明：Levent Alpöge，《Remark》4 页 PDF，https://alpo.ge/remark.pdf ，2026-11-20。原话（Scientific American 引）：「I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis.」
- Lean 源码：14 个分卷 `https://alpo.ge/lean.tar.gz.part01` 至 `part14`（经社区合并，约 239 MB 压缩包）。
- MIT Andrew Sutherland 独立验证页：**https://math.mit.edu/~drew/grh/** （标题「RH Lean proof: verification files」，页面自述 "checks run with Claude Code"，最后更新 2026-11-20 22:05 UTC）。

### 重点：Lean 代码到底编译了什么 / 17M 行 vs 25 万页

来源：https://math.mit.edu/~drew/grh/ （Andrew Sutherland，MIT，2026-11-20，逐字英文）

> "The published Lean source compiles without errors, the five headline theorems depend only on the three standard axioms, every module has been replayed through the Lean kernel, two independently written kernels, nanoda and con-leche, accept the full exported proof, and lean4lean accepts every compiled module."

> 关键逐条（表格原文转录）：
> - "All 175,187 modules compile (Lean v4.33.1, Mathlib v4.33.1) — pass — zero errors; finished 16:43:21 UTC"
> - "#print axioms for the five theorems in Main.lean — pass — propext, Classical.choice, Quot.sound only (audit.log)"
> - "Mathlib files identical to the official cache — pass — 8,322 Mathlib and 378 dependency .olean files, byte for byte"
> - "leanchecker kernel replay of every module — pass — 175,187 of 175,187; zero rejections"
> - "nanoda (independent Rust kernel) on the full export — accepts — 27 min 42 s on 80 threads"
> - "con-leche master ... accepts — 1,468,418 declarations accepted in --verified mode; 3 min 15 s on 64 threads"
> - "lake comparator (Lean v4.36.0) end to end — running ... kernels in progress"

> **编译对象的精确定义**（页面 "What was verified" 一节，逐字）：
> "The theorem `riemannHypothesis : RiemannHypothesis` in the submission's Main.lean, where RiemannHypothesis is Mathlib's own definition (Mathlib/NumberTheory/LSeries/RiemannZeta.lean):
> ```
> def RiemannHypothesis : Prop :=
>   ∀ (s : ℂ) (_ : riemannZeta s = 0) (_ : ¬∃ n : ℕ, s = -2 * (n + 1)) (_ : s ≠ 1), s.re = 1 / 2
> ```
> and four further theorems, including the analogous statement for all Dirichlet L-functions. 'GRH' here means Dirichlet L-functions only."

> **逆向复现代价**（逐字）："The exported proof: output of lean4export on module Main" — `solution.ndjson.zst 7.7 GB`，"Unpacks to 61,570,595,788 bytes."；"With Lean v4.36.0 installed, nothing needs to be built. You need roughly 100 GB of RAM and 70 GB of disk."；"nanoda alone took about 28 minutes on 80 threads. The Lean kernels are single-threaded and take much longer."

> **Caveats（逐字，重要，直接回应「编译了证明的哪一部分」）**：
> "A passing check shows that the formal statements, as written in Lean against Mathlib's definitions, follow from the three standard axioms according to the kernels used. It relies on those kernels and on Mathlib's definitions being what they are intended to be."
> "The build ran on cloud machines without a sandbox, and compiled each file by calling lean directly with the same inputs lake supplies, not through lake build. A 77-module sample of the output is byte-identical to a real lake build; the full set has not been compared against one."

---

## 一、Hacker News（主帖）

**主帖**："Levent Alpöge claims proof of the Generalized Riemann Hypothesis [pdf]"，作者 iamronaldo，2026-11-20T14:16:21Z，2405 分 / 1601 条评论。
URL：https://news.ycombinator.com/item?id=50574968 ；原文链接 https://alpo.ge/remark.pdf
（完整评论树见 Hacker News item 50574968，可经 Algolia API 公开取回）

**副帖**：
- "The stochastic parrots have apparently proved the Riemann Hypothesis"，2026-11-20T20:03:07Z，223 分 / 199 评，https://news.ycombinator.com/item?id=50581084
- "Anthropic mathematician Levent Alpöge claims to prove Riemann Hypothesis"，https://news.ycombinator.com/item?id=50575483
- "2 hr delay between Alpöge posting the Riemann pdf and Kalshi market moving"，https://news.ycombinator.com/item?id=50583050
- "Unforced Navier-Stokes Singularity and the Generalized Riemann Hypothesis"，https://news.ycombinator.com/item?id=50578358

### 1A · 正面 / 信以为真

- **jrflo**，2026-11-20T14:38:51Z（id 50575244）：
  > "Insane if true! Would also be great for Alpoge after being scooped by OpenAI on Navier Stokes... Edit: just saw the Lean code is included in the footnote. Considering the author's reputation, I think it's fair to remove the qualifications from the post title, by the standards of today, it's proven."
- **jrflo**，2026-11-20T15:00:11Z（id 50575601）：
  > "This is true, but since there is a lean formalization the odds of that are much lower. Only the correctness of the problem statement needs to be verified, which I would assume is fairly easy for RH?"
- **dawdler-purge**，2026-11-20T23:20:57Z（id 50583778，引 Sutherland 验证页）：
  > "Given the multiple ways this proofs has been formally verified^[1], I think at the moment there is no any doubt that it is correct. > Andrew Sutherland (MIT), checks run with Claude Code. Page last updated 2026-11-20 22:05 UTC. ... [1]: https://math.mit.edu/~drew/grh/"
- **_flag**，2026-11-20T20:03:32Z（id 50581102）：
  > "...the fact that the Lean compiles is certainly going to be, in this setting, a more reliable assessment of the proof's correctness than the opinion of any expert, unless that opinion also comes with an identification of the bug being exploited."
- **NitpickLawyer**，2026-11-20T15:18:27Z（id 50575887）：
  > "...We've had 2 millennium prize problems fall this year, plus a lean proof for FLT ... The negativity and goalpost moving is getting tired."
- **ariwilson**，2026-11-20T14:58:10Z（id 50575564）：
  > "The way this is getting announced via a human mathematician humbly presenting a result is great, as opposed to the marketing influenced OpenAI post. I am still a bit sad though. I had hoped the Rieman hypothesis would fall mostly to a human mathematician rather than 60/40 AI/mathematician that my layman's read of that note interprets."

### 1B · 怀疑 / 质疑

- **hgoel**，2026-11-20T14:58:17Z / 15:12:38Z（id 50575567 / 50575783）：
  > "plenty of otherwise very prolific mathematicians have claimed to solve problems only to turn out to have been mistaken."
  > "I will remain skeptical until then. After the excitement of the Navier-Stokes solution, only for it to eventually seem to not really solve the full problem ... I'd rather wait and see if this proof also makes some assumptions that ultimately limit its impact."
- **well_ackshually**，2026-11-20T15:35:52Z（id 50576265）：
  > "No. A lean codebase that certifies that whatever definition of the Riemann hypothesis their LLM wrote is correct. I can write Lean that certifies that 2 + 2 = 5 is correct, by redefining every axiom about natural numbers on the way. Or abusing bugs in Lean. Stop believing Lean is the response to everything."
  （反驳见 latent-person id 50576497：人类独立写的 formal conjecture + 多内核交叉检查 + lake comparator。）
- **vintermann**，2026-11-20T15:29:29Z（id 50576140）：
  > "At what point do we start worrying that it's found a bug in Lean instead?"
- **nocturn9x**，2026-11-20T18:25:33Z（id 50579450）：
  > "Lean can and does have bugs. It compiling doesn't mean anything, because that doesn't tell you whether the proof actually encodes what the mathematician intended."
- **causal**，2026-11-20T16:28:58Z（id 50577262）：
  > "One real problem with depending on Lean is that if there are any real bugs, then sufficiently-large swarms of agents are guaranteed to eventually come up with proofs that depend on the bug."
- **rvz**，2026-11-20T16:04:37Z（id 50576817）：
  > "...multiple authorities in the field got together and verified that the proofs were correct (even after Wiles initially had an error.) ... So at best I'll keep as unverified until it is checked, unless you have read the 250,000 proof and checked that there is no issues or soundness bug with the lean proof."
- **iLoveOncall**（副帖），2026-11-20T21:54:24Z（id 50582810）：
  > "Nobody is ever reviewing and verifying 17M lines of code. Ever. This is just another hypothesis on top of the Riemann Hypothesis. We now have an Hypothetical proof of the Riemann Hypothesis, great. Breakthrough."
- **twotwoone**，2026-11-20（id 50581350，质疑 AI 公司动机）：
  > "...using 17 million lines of unverifiable Lean code, along with mathematicians, to mask their decline on the stock market."

### 1C · 「17M 行 / 25 万页 / 代码逆向」专条（重点）

- **djoldman**，2026-11-20T15:54:31Z（id 50576609，给出下载+统计命令）：
  > "These proofs really are quite huge. The proof is 1.3GB, ~15M lines of code: mkdir -p riemann_proof ... curl ... https://alpo.ge/lean.tar.gz.part$i ... 14 parts ... find lean -name '*.lean' | wc -l ..."
- **auntienomen**，2026-11-20T20:31:54Z（id 50581584）：
  > "250k pages is the 'natural language proof' that Alpoge chose not to publish because it was slop. The lean proof is 17M lines. That did get published, although good luck compiling it if you don't have access to 1/2 TB of RAM."
- **nathell**，2026-11-20T15:40:58Z（id 50576374）：
  > "17M+ lines, 1 GB of Lean in 175K+ files. Good luck of understanding all of that in one's lifetime."
- **jychang**，2026-11-21T01:54:58Z（id 50585062，**声称正在编译并审阅代码**）：
  > "175,187 modules... a lot of them are repetitive. It really seems he was rushed to publish and did not bother cleaning the code up at all. I am compiling the code and reviewing it, and there are 204 dead modules that are not referenced anywhere. And a LOT of repetitive code. I think this should not be 17 million lines, I bet this can easily be under 1 million lines of Lean with a little bit of cleanup."
- **GrollingGrakbor**，2026-11-20T18:03:44Z（id 50579112）：
  > "It's 17M according to the github repo. Which btw, is akin to showing someone the bible and saying 'Proof of God is in here. It may be 2000 pages and in Latin, which you can't read, but just believe me.'"
- **lossolo**，2026-11-20T20:02:21Z（id 50581069，**质疑 25 万页 ↔ 17M 行的语义映射**）：
  > "OK, so if I understand correctly, I formulate a proposition in natural language, then an LLM creates 250k pages of proof based on it, then it takes those 250k pages/lines of proof and creates 15 million lines of code to prove those 250k pages. Is that correct? If so, how can I be sure that my proposition is correctly translated into those 250,000 pages? Isn't it possible that the LLM can reward hack here ... it tries to change assumptions without telling me ..."
- **YeGoblynQueenne**，2026-11-20（id 50582438，引用 Kevin Buzzard 文章的 tl;dr）：
  > "It's not the proposition (i.e. the theorem) you need to worry about but the proof. ... 'this theorem is correct because it was checked in Lean'. The summary is: there is an important signal that can be extracted from Lean code, but if this code is AI-generated then you need to do some tests first."
- **less_less**，2026-11-20T17:01:43Z（id 50577948）：
  > "...the length of the proof doesn't much matter since Lean has checked it. You'd still have to check the theorem statement, which is not going to be nearly as easy as with FLT (Riemann uses analytic continuations etc) but it's much easier than checking a 250,000 page proof."
- **math_dandy**，2026-11-20T16:02:34Z（id 50576771，指出 GRH 声明在 Mathlib 两行定义 + 引 Buzzard）：
  > "The statement of the Riemann hypothesis is included in the Mathlib library ... See https://github.com/leanprover-community/mathlib4/blob/master/Mathlib/NumberTheory/LSeries/RiemannZeta.lean ... for the two-line statement."
- **hgoel**（补充"250k pages"提法，见 1B id 50575783）："if this proof is correct and can't be made much shorter than the current ~250k pages..."

### 1D · 方法论文艺评论（AI 是否可被人类理解 / 对数学界的影响）

- **puff_pastry**，2026-11-20T14:42:56Z（id 50575311，18 回复）：
  > "...this feels like we're moving away from the old world of 'science', where we could actually read an experiment, understand its steps, and, with enough time, possibly reproduce it. And we're back in the world of 'magic' and 'belief'. Yes, there are formal ways to prove that this is correct, but if the proof is so convoluted and hard to read, is it actually something a human will ever find the time to go through and understand? We're going to start delegating ... the understanding to AI ... Still, very strange dizzy feeling, much more than Navier-Stokes"
- **Waterluvian**，2026-11-20T14:53:36Z（id 50575484）：
  > "we're decoupling two related but distinct elements: the answer and the understanding. We have a big machine that tells us the answer is '42.' ... I'm just not convinced that we're swapping one world for the other."
- **layer8**，2026-11-20T15:17:57Z（id 50575874）：
  > "It would be sad if AI designed and built the Saturn V and we had it fly us to the moon without understanding the details of how it is built ... There is furthermore value for humans in going through the discovery process themselves..."
- **yewenjie**，2026-11-20T15:14:01Z（id 50575797，40 回复，引 Scott Aaronson「Mathocalypse」）：
  > "Experience has shown that, even now, there will still be people explaining in patronizing tones why none of this is real and none of it counts. If such people were capable of being impressed by anything that happens in the empirical world ... they would've already been impressed ... long before things had reached the point of an actual Mathocalypse."
- **z7**，2026-11-20T16:18:38Z（id 50577066，引 Terence Tao 一个月前访谈，https://www.youtube.com/watch?v=bAnwMdfcMVw @17:10）：
  > "Terence Tao a month ago: 'The AI companies have clearly... I'm sure they have secretly devoted much more time to the Riemann hypothesis than what they've disclosed. And they couldn't touch it. I mean something beyond current AI is needed. So maybe some combination of humans and AIs in 2050 or something will make further progress. Who knows?'"
- **teekert**，2026-11-20T15:40:27Z（id 50576362）：
  > "I heard Terence Tao say he had only looked at about 10 of the OpenAI proofs, found some good things, bad things, mistakes. But it will be his (and other's) understanding of the proofs that will give new insights."
- **ToucanLoucan**，2026-11-20T16:44:11Z（id 50577583）：
  > "Due to the nature of how an LLM works, it cannot make anything truly new, it ... can only recombine and remix things from it's training data. ... They are not intelligent, they are not conscious, and they cannot make New things."
- **FiberBundle**，2026-11-20T15:36:24Z（id 50576281）：
  > "...we have a domain that is solvable by reinforcement learning with verifiable rewards. It's very impressive ... but the techniques aren't transferable to the vast majority of other domains. ... It's that lean is able to massively shrink the search sp[ace]."
- **Jaxan**（id 50579715）与 **u1hcw9nx**（id 50579836）指向"纯人类证明"：https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/ ，并质疑 "Is this yet another case of AI company stealing almost ready proof from someone else?"

---

## 二、MathOverflow（专业社区，方法论主场）

**主问题**："How to 'human verify' enormous AI math output?"，提问者 Sam Hopkins（community wiki），2026-11-20，71 票 / 3k 浏览 / 3 回答。
URL：https://mathoverflow.net/questions/518033/how-to-human-verify-enormous-ai-math-output

> 正文（逐字）："Levent Alpöge has claimed a proof of the Riemann Hypothesis produced by Anthropic's Claude AI system. ... he says Claude produced 250,000 pages of naturally written mathematics. ... if presumably we as mathematicians are interested in human knowledge and human understanding, and not just confidence in the truth of some statement, at some point we will have to work to understand this proof at a human level. But the numbers mentioned make this seem totally unappealing, if not outright impossible ... Question: How should mathematicians approach understanding truly enormous AI outputs that (claim to) prove very significant results?"
> EDIT："an independent, non-AI proof of GRH by Alessandro Maria Masullo has also been announced on Ravi Vakil's blog."

**评论**（逐字）：
- **Asaf Karagila**（17 票）："The question is, really, how complicated is it to extract from the proof the part where 0=1."
- **Timothy Chow**（15 票）："Before popping the champagne, let's at least wait for some independent verification that the Lean proof doesn't exploit some obscure kernel bug or something stupid like that."
- **Thomas Bloom**（15 票，**直接质疑 25 万页是虚标**）："The 250k page count is likely a ridiculous inflation. I have heard from someone using Codex that they have gotten the proof down to <300 pages. Of course these are probably very dense, but I'd expect a 'reasonable' human-readable version would be <1,000 pages."
- **Francesco Polizzi**（28 票）："Once we used to quote Thurston and to say that 'The product of mathematics is clarity and understanding. Not theorems by themselves'. And now we are talking about 250000 pages and Lean code. What happened to us? Is this really the Mathematics we want?"

**回答 1**（Timothy Chow，31 票，转述 Lean Zulip 上 Chris Birkbeck 贴出的 Fable 高阶摘要）——**这是"从代码逆推证明过程"的直接材料**：
> "RH is deduced from 'no Dirichlet L-function vanishes for Re(s) > 1/2'. This step is short and standard. ... deduced from three things: a growth bound on certain Galois cohomology groups ... a covolume bound coming from arithmetic geometry; and statements about theta integrals on unitary groups, together with local Langlands for GL_n. I have not yet understood how these combine to exclude zeros. ... It genuinely uses the existence of Galois representations attached to regular algebraic, polarized cuspidal automorphic representations of GL_n over CM fields ... base change, Jacquet–Shalika, Clozel's purity, Bernstein–Zelevinsky, the main theorem of CM, Deligne–Ribet and more. ... Those Galois representations are not assumed. The project derives them from a geometric construction ... As far as I can tell this is not the route in the literature. ... There are no axioms, so each of these has to be proved inside the project."
> "Anyway, the point is, interrogating a frontier model in this fashion seems to be the only feasible way to get started understanding the proof."
> 附 convex hull hypothesis 启发式："a large proportion of the proof consists of, or can be interpreted to be, 'known ideas.' Probably the first order of business is to tease out the 'well-known ideas' from the new 'not-already-well-known' ideas."

**评论中对摘要的"人话"翻译**（**Will Sawin**）：
> "The proof involves defining certain lattices in a vector space of automorphic forms. ... This lattice is endowed with a metric by period integrals."；"In fact the proof defines two lattices ... the ratio of their covolumes is just the index of one lattice in the other, which is a product over primes of a p-adic index. One lower bounds the p-adic index for certain primes and thus upper bounds a count of primes. This upper bound then implies GRH."；"This summary arose from a brief conversation with ChatGPT. It is somewhat imprecise: There are not just two lattices ... I should have said the metric is the Petersson metric."
> **Malik Younsi**："Does this 'summary' even make any sense to anyone?"

**回答 2**（Will Sawin，28 票）：
> "For proofs of big conjectures, mathematicans should attain human understanding mainly by working together. Different people should read different parts of the proof, understand them, and write down what they understand or communicate it orally. ... people will have to become increasingly comfortable writing expositions of parts of proofs..."

**回答 3**（Stanley Yao Xiao，社会层面）：
> "a plan to adapt graduate, and possibly undergraduate curricula, so that future mathematicians will be accustomed to a world where gigantic, computer (AI)-assisted proofs come into being, with the human author(s) only vaguely able to explain them. ... Develop a professional environment ... which rewards and facilitates production of high quality, human readable artifacts ... have funding for meetings or even thematic programs focused on digesting such proofs ... Funding should be sought from the AI companies that undoubtedly benefited from this..."

**评论**（**akkapi**）："In an interview, Tristan Buckmaster expressed that LLMs seem to be more capable of interpreting outputs from other LLMs than humans. ... anyone without frontier level AI access is rapidly being left behind, both in terms of creation as well as understanding..."

---

## 三、Kevin Buzzard 的方法论长文（被 HN 多处引用，专家直接观点）

**标题**：""…and the proof is checked in Lean""，作者 xenaproject（Kevin Buzzard），2026-11-12。
URL：https://xenaproject.wordpress.com/2026/11/12/and-the-proof-is-checked-in-lean/

> 核心摘要（逐字）："there is an important signal that can be extracted from Lean code, but if this code is AI-generated then you need to do some tests first."
> 四问：Q1 "Does the Lean statement match the informal statement?"；Q2 "does the code compile?"；Q3 "does the code prove the theorem as stated?"；Q4 "does the code exploit a bug in Lean's official kernel?"
> **编译代价（逐字）**："When Anthropic formalized a proof of Fermat's Last Theorem in Lean, they asked me to take a look at the 13 million lines of code which they had generated. I downloaded it ... attempted to compile it; I very quickly ran out of memory. ... they gave me temporary access to a machine with 96 cores and 500 gigs of ram. I could then compile the code."
> **"编译 ≠ pdf 正确"（逐字）**："Even if the Lean code compiles and passes all these tests ... this does not mean that any corresponding pdf also contains a correct proof! The Navier–Stokes lost in translation paper demonstrates the opposite ... some experts have been using AI to informalise the Lean proof back into English in order to read the argument, rather than ploughing through the slop in the pdf."
> 结论："'It's true because it's checked in Lean' can be made to make sense. But one has to do some work beyond 'the Lean code compiles'."
> 反方评论（coolio / Rabuni）："How is it not circular to use Lean to prove that a Lean kernel is consistent?"；"'However, this does not matter.' Is a very strong statement ... I do not see any guarantee that even any finite number of different verifier kernels could not have joint bugs."

---

## 四、"纯人类对手证明"（同期发生的对比性评论背景）

**Ravi Vakil 博客**（注意：这是 Ravi Vakil 的代数几何博客 Math 216，**不是** Terence Tao 博客；HN 上多条评论误以为是陶哲轩）：
标题 "Hurried Announcement: Alessandro Masullo GRH"，2026-11-20，https://math216.wordpress.com/2026/11/20/hurried-announcement-alessandro-masullo-grh/
> 逐字："Alessandro Masullo has a proposed proof of GRH, and Brian Conrad has been looking closely at it for months and gave extensive feedback which is being incorporated, and it is so far looking good. **It is all human, with no AI.** The hope was to bring Alessandro to Stanford for a year to polish the writing and then announce it after running it by enough people. Now it seems best to put out the current draft. I hope people quickly check it, formalize, fill any small gaps if they occur."

HN 引述（**lysace**，2026-11-20T18:50:23Z，id 50579827）对 reddit 讨论的转引：
> "'It is all human, with no AI' Also: It's 940 pages. ... 'Rooting for this, but can't help but feel it's just false proof #14,828, but the more promising one that came out recently, and that we're clinging to it as the last hope before it turns out Anthropic got it.' 'Who is Alessandro Masullo? ... a data scientist who does not seem to have the background for a non-AI proof of GRH.' 'And we are supposed to believe that RH ... has just been solved by a mathematician with no publication history or academic presence, without the help of new technologies, at the same time as a new technology credibly becomes capable of doing [it]'"
> 指向 Reddit 讨论：https://www.reddit.com/r/math/comments/1y1b6l8/hurried_announcement_alessandro_masullo_grh/ （该帖直取被拦，此处为 HN 二手转引）
**另**：MathOverflow 关联问题 "Human proof of GRH. Post by Ravi Vakil including current state of the long manuscript"（侧栏链接，score 1）。

---

## 五、媒体报道中的专家直接引语（逐字英文 + 出处）

1. **Levent Alpöge**（声明原话），Scientific American，2026-11-20，Joseph Howlett：
   URL https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/
   > "I believe I have, working with Claude, discovered a proof of the Riemann Hypothesis."

2. **Andrew Sutherland**（MIT），Scientific American 专文《The million-dollar math problem hardly anyone is trying to solve》，2026-05-19，Joseph Howlett（**在 11-20 事件中被 HN 用户 1313ed01 id 50580888 引用**）：
   URL https://www.scientificamerican.com/article/the-riemann-hypothesis-is-a-million-dollar-math-problem-hardly-anyone-is-trying-to-solve/
   > "'An AI that can prove the Riemann hypothesis is not one I'd want to meet,' said Andrew Sutherland, a number theorist at the Massachusetts Institute of Technology. 'If that happens, mathematicians having jobs will be the least of our problems.'"
   > 事件当天 Sutherland 实际编译/验证的逐字结果见本文件第〇节（https://math.mit.edu/~drew/grh/）。

3. **Scientific American 报道标题（正面派）**："The AI Riemann hypothesis proof is almost certainly true, independent mathematicians attest"，作者 Clara Moskowitz，2026-11-20，
   URL https://www.scientificamerican.com/article/the-ai-riemann-hypothesis-proof-is-almost-certainly-true-independent-mathematicians-attest/
   > 可见开头（付费墙前）逐字："Mathematicians are now almost certain the earth-shattering artificial-intelligence-created proof of the Riemann hypothesis is real."
   > （注：付费墙后正文未能取得；"independent mathematicians attest" 系标题与导语口径，具体具名引语未验证。）

4. **Scientific American 另两篇（背景/对比）**：
   - "Dueling claims of Riemann hypothesis proofs emerge"，Joseph Howlett，https://www.scientificamerican.com/article/dueling-claims-of-riemann-hypothesis-proofs-emerge/
   - "What does a solution to the Riemann hypothesis mean for you?"，Lee Billings，https://www.scientificamerican.com/article/what-the-riemann-hypothesis-proof-really-means-about-ai-and-the-future/

5. **Science 报道（未能直取全文）**：标题 "AI may have solved math's greatest mystery. Can humans understand the answer?"；站点 science.org 被 Cloudflare 拦截（"正在进行安全验证 ... Ray ID ..."），未取得逐字引语。

6. **Steven Strogatz（康奈尔）"黎曼猜想是数学界的白鲸记 / Moby Dick"**：背景资料给出的表述；本次多引擎检索未找到可核验的原始逐字出处与 URL，HN 评论树中亦无 "Strogatz/white whale" 命中。**标注为「未核验」**，不写入逐字引语。

7. **Peter Sarnak 表态**：HN 用户 **_flag**（id 50577599，2026-11-20T16:44:51Z）："Sarnak weighing in is completely irrelevant. There is a lean formalisation, and it either compiles or it doesn't. (And the statement of RH is well established so there is no issue there.)" —— 说明当日确有围绕 Sarnak 表态的争论，但本次未取得 Sarnak 本人逐字原话，标注「未核验」。

8. **Terence Tao 访谈（一个月前，HN 转引）**：见 1D（YouTube https://www.youtube.com/watch?v=bAnwMdfcMVw @17:10）。Tao 博客 terrytao.wordpress.com 搜索 "Riemann hypothesis Claude" 无本次事件新帖，最近相关为 2026-10-22 客座帖 "Simple and critical zeros of the Riemann zeta-function"（https://terrytao.wordpress.com/2026/10/22/ ）。

---

## 六、中文社区（知乎 / 腾讯 / 新浪 / 爱范儿 / 量子位）

### 6.1 知乎问题「数学家称与 AI 合作证明了黎曼猜想，证明稿超 25 万页，黎曼猜想真的被解决了吗？」
URL https://www.zhihu.com/question/2107239235015263538 ；关注 3,015 / 浏览 7,183,913 / 850 个回答 / 88 条评论（抓取时）。抓取时间 2026-11-21。

- **答主 张凯羿**（知势榜科学工程领域成长力榜，11-21 08:25，赞同 2006）：
  > "Astra 审查通过了，这份 Lean 证明的就是黎曼猜想（考虑到 Claude 参与了证明，这何尝不是一种同行评议）此外 MIT 的 Sutherland 通过了独立验证 verification files 现在问题只剩下我们对 Lean 和 Mathlib 的信心了！"
  > 同一回答内的编译直播记录（11-21 01:26—09:10）："正在扩容存储，现在进行了约30%，占用了360GB，预计现在1TB刚好不够用，希望机器不崩溃。" → "扩容到2TB成功。" → "用时约7小时11分钟，这份Lean形式化证明通过了编译！那么剩下的两个问题在于核验语义，也就是确认形式化证明的对象就是黎曼猜想，以及确保这份证明中没有恰好触发这一版本Lean的bug" → "都是Slop就发lean了 二十五万页的论文就不发了"
  （这段中文直播与 Sutherland 页、HN 的 "17M 行/1-2TB 内存" 说法互相印证"编译代价"主题。）
- **答主 罄竹**（11-20 23:41，赞同 1858）：
  > "冷笑话：黎曼32岁提出黎曼猜想，39就去世了。说个雷霆点的假设你把这份证明打印出来穿越回到过去在黎曼提出猜想的当天交给他，25w页他读到死都读不完"
- 知乎站内热搜显示：「AI证明黎曼猜想」411 万热度；相关热搜「数学家完成NS方程无外力证明」336 万。

### 6.2 知乎专栏（量子位转载）
URL https://zhuanlan.zhihu.com/p/2107263438028289049 ，发布于 2026-11-21 00:06，30 人赞同，10 条评论。文章本体（叙述 Cloude " flew, and danced, and sparkled near that finish line"、25 万页、14 个 Lean 分卷、√X 链长、Theta Lifts 等）。
评论区（逐字，按平台作者/时间）：
- 转世灵童叁（3 小时前·河南）："这里地方很大，我的证明写的下"（**怀疑/嘲讽派**）
- 油面筋塞肉（4 小时前·上海）："如果是真的，那么真的震惊瘫坐了，以前那些都不如这个"（正面）
- Cedric聊AI（3 小时前·上海）："虽然不知道有什么用，但是我知道一定有应用场景，AI实实在在的在推动人类前沿了"（正面）
- sizeof（2 小时前·北京）："检验AI，推进AI，就是用途之一"（方法论）
- hotland（5 小时前·北京）："如果黎曼猜想真能搞定，也只有这种履历才可能了"
- 微与之（10 小时前·北京）："不理解的证明，放出来有什么意义呢？"（**方法论/理解性质疑**）
- 港深亻子（9 小时前·中国香港）："转人工"

### 6.3 腾讯新闻（赛博禅心，2026-11-20 22:55，广东）
URL https://news.qq.com/rain/a/20261120A0GCJC00 。正文标题「我们，已迈过奇点」；提到犬儒解读：「有一种显而易见的犬儒解读，和时机有关，但它是错的」（Alpöge 语）；致谢含 Bhargava、Peter Sarnak、Maksym Radziwiłł。
评论区（43 条，节选逐字）：
- ☠鬥戰於天☠（广西，7 小时前，**贝叶斯派/怀疑**）："该事件为'完整正确证明广义黎曼猜想'的概率仅约12%，为'取得关键阶段性进展、存在可修补漏洞'的概率约58%，为'存在本质错误、无法通过同行评议'的概率约30%。目前事件处于'个人声明阶段'，尚未进入学术共同体验证的核心环节，远未达到'已攻克'的确定性水平。"
- 柏沙（山西）："这要真证明了黎曼猜想，那菲尔兹没跑了，黎曼猜想这是数论皇冠上的明珠。"
- xhell（上海）："这个黎曼猜想对于数学的影响是非常巨大的，不如相对论对物理的影响但也绝对可以比较，这可是划时代的成果"
- 田微 / 元宝（AI 回复）："你来证明黎曼猜想@元宝" / "这题有点超纲，我这点算力连ζ函数的零头都算不明白..."
- 陆小风（上海）："已经毫无意义了。证明出来还是证明不出来都没有了意义。"
- 莺飞（广东）："刘慈欣有篇小说《朝闻道》，看来真的是'只有想不到的，没有做不到的'"

### 6.4 新浪财经（转载量子位同名文章）
URL https://finance.sina.com.cn/stock/t/2026-11-21/doc-inizmfew3857939.shtml ，标题「黎曼猜想被证明？！Claude加持，34岁数学家放出Lean证明」（内容与 6.2 同源，8701 字节；评论以页面渲染为准）。

### 6.5 爱范儿（相关背景，非评论主场）
URL https://www.ifanr.com/1690375 ，标题「刚刚，Anthropic 敲钟上市！史上最大IPO却让硅谷吵翻了」——事件与 Anthropic IPO 舆论叠加的中文背景。

### 6.6 其他中文视频/聚合线索（未逐页取证）
Bing 中文结果出现 bilibili、52pojie 等讨论（如 https://www.52pojie.cn/thread-2116353-1-1.html ），本次未逐页核验，标注「待核」。

---

## 七、三派观点归纳（仅归类，不下判断）

**正面 / 信以为真**：HN jrflo、dawdler-purge（引 Sutherland 验证）、_flag、NitpickLawyer；知乎张凯羿（Astra+Sutherland 双重验证，编译 7 小时）；量子位评论区油面筋塞肉、Cedric聊AI；腾讯柏沙、xhell；SciAm 标题口径 "almost certainly true, independent mathematicians attest"。核心论据：Lean 编译通过 + Mathlib 定义 + 多内核交叉验证 + 作者履历。

**怀疑 / 质疑**：HN hgoel、well_ackshually（2+2=5 类比）、vintermann、nocturn9x、causal（内核 bug）、iLoveOncall（17M 无人能审）、twotwoone（动机质疑）；MathOverflow Asaf Karagila「提取 0=1 有多难」、Timothy Chow「先别开香槟」、Thomas Bloom「25 万页虚标」；量子位「这里地方很大，我的证明写的下」；腾讯☠鬥戰於天☠（12% 概率）。焦点：语义映射是否正确、是否真证明 GRH、内核 bug、AI 贡献与时机动机。

**方法论文艺评论**：HN puff_pastry（科学→魔法）、Waterluvian（答案 vs 理解）、layer8（土星五号）、Aronson「Mathocalypse」引用者、ToucanLoucan、FiberBundle；MathOverflow Polizzi（Thurston「产物是清晰与理解」）、Timothy Chow 的 Fable 逆推摘要 + Will Sawin 格论人话翻译 + Stanley Yao Xiao（改课程/赞助"消化证明"计划）；Buzzard「编译之外还要做四问」；量子位微与之「不理解的证明放出来有什么意义」；知乎罄竹「25 万页读到死也读不完」。核心：AI 产出的可理解性、人类如何分工消化、对数学教育与共同体的冲击。

---

## 八、检索限制与未完成项

- **X/Twitter**：多引擎检索未取得可直接引用的专家/大众帖固定 URL；相关表态以 HN/MathOverflow 二手转引为主（Tao 访谈 YouTube、Sarnak 争论、Aaronson 引语）。标注「未核验原帖」。
- **Reddit**：r/math 帖 https://www.reddit.com/r/math/comments/1y1b6l8/ 页面与 .json 均被拦截（old.reddit 显示 "Blocked"，www 返回 222 字节空壳）。仅经 HN lysace 评论取得四条二手转引（见第四节）。
- **Science.org**：Cloudflare 安全验证拦截，标题已知但无逐字引语。
- **Strogatz「白鲸记」、Sarnak 本人原话**：多引擎未找到可核验逐字出处，标注「未核验」，本次无独立证据。
- **SciAm 付费墙**：三篇 11-20 文章仅取得导语，具名专家引语（除 Sutherland 旧文、Alpöge 声明）在墙后未取得。
- **中文果壳/网易/搜狐**：本轮未发现可核验的讨论帖 URL（Bing 中文结果泛化），仅覆盖知乎/腾讯/新浪/爱范儿/量子位。标注「待补」。

### 原始材料获取

HN 完整评论树可经 Algolia API 按 item id 50574968 取回；其他页面与讨论的 URL 见 `sources/SOURCE_INDEX.md`。
