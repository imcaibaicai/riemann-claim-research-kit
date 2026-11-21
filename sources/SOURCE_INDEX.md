# 来源索引

> 状态：`已取得`＝正文或文件已实际抓取；`仅摘要`＝只在搜索结果里拿到片段；`受限`＝403、付费墙或反爬拦截。调研时间为 2026-11-21（UTC+8）。获取方式中的步骤编号对应 [AGENT.md](../AGENT.md)。

## 一、一手材料

| 来源 | URL | 状态 | 获取方式 | 关键内容 |
|---|---|---|---|---|
| Alpöge 四页说明 | `https://alpo.ge/remark.pdf` | 已取得 | AGENT.md 第 1 步 | 声称原文、Stepanov/theta lift/√X 策略、20 步时间线、14 卷代码地址 |
| Lean 源码 14 卷 | `https://alpo.ge/lean.tar.gz.part01` 至 `part14` | 已取得 | AGENT.md 第 2 至 4 步 | 239,275,554 字节，SHA256 与 MIT 页一致 |
| MIT Sutherland 验证页 | `https://math.mit.edu/~drew/grh/` | 已取得 | URL 直取 | 顶层定理原文、三公理审计、多内核验证、7.7GB proof export |
| MIT challenge 导出 | `https://math.mit.edu/~drew/grh/challenge.ndjson.zst` | 已取得 | AGENT.md 第 5 步 | 五个带 sorry 的命题，47MB |
| MIT 校验与日志 | `https://math.mit.edu/~drew/grh/{SHA256SUMS,audit.log,Audit.lean,comparator.json,nanoda.log,con-leche-*.log,project-oleans.sha256}` | 已取得 | URL 直取 | 公理清单、编译与内核检查结果 |

## 二、代码顶层文件

解包 14 卷后出现 `lean/src/`，按下列路径读取：

| 文件 | 内容 |
|---|---|
| `lean/src/Main.lean` | 五个顶层定理，12 行 |
| `lean/src/Statements.lean` | 四个自定义命题定义，16 行 |
| `lean/src/Supplements/Kernels/MainTheorems.lean` | `rhOfAll`、`allChi`、`claimAll`、`critAll` |
| `lean/src/Formalism/GammaFactorNonvanishing/CritLine.lean` | `rh_of_every`、`critLine`，经典归约部分 |
| `lean/src/Development/AutomorphicArithmetic/CharacterBoxes/MainConclusion.lean` | `everyCharacter`、`everyCharacter_of_row` |
| `lean/src/Development/AutomorphicArithmetic/Foundations/GaloisRepresentations/IntegralStructures/RowSrc.lean` | 核心 `galoisLocalGlobalStrong_recParameter` |

## 三、新闻报道

| 来源 | URL | 状态 |
|---|---|---|
| Scientific American（验证报道） | `https://www.scientificamerican.com/article/the-ai-riemann-hypothesis-proof-is-almost-certainly-true-independent-mathematicians-attest/` | 已取得 |
| Scientific American（首报） | `https://www.scientificamerican.com/article/anthropic-mathematician-claims-to-prove-riemann-hypothesis-most-famous-problem-in-all-of-math/` | 已取得 |
| Scientific American（双重声称） | `https://www.scientificamerican.com/article/dueling-claims-of-riemann-hypothesis-proofs-emerge/` | 已取得 |
| Scientific American（9 月前史） | `https://www.scientificamerican.com/article/ai-may-have-just-solved-a-million-dollar-math-problem-the-field-will-never-be-the-same/` | 已取得 |
| Scientific American（25 位菲尔兹奖得主） | `https://www.scientificamerican.com/article/25-winners-of-maths-nobel-prize-decry-the-ai-invasion-of-their-discipline/` | 已取得 |
| Nature | `https://www.nature.com/articles/d41586-026-03812-7` | 已取得 |
| New Scientist | `https://www.newscientist.com/article/2600134-anthropic-mathematician-claims-ai-assisted-proof-of-the-riemann-hypothesis/` | 已取得 |
| Science | `https://www.science.org/content/article/ai-may-have-solved-math-s-greatest-mystery-can-humans-understand-answer` | 受限 403 |
| New Straits Times | `https://www.nst.com.my/world/world/2026/11/1582339/...` | 已取得（正文少） |
| SBS | `https://news.sbs.co.kr/english/article.do?news_id=N1008856699` | 已取得 |
| Business Insider / Yahoo | `https://www.yahoo.com/news/science/articles/anthropic-mathematician-says-claude-helped-183716235.html` | 已取得 |
| Clarín（验证路线图） | `https://www.clarin.com/us-news/whats-next-if-the-riemann-hypothesis-is-solved-the-road-to-verifying-alpoge-and-claudes-proof-claim_0_1lB3dnmm5n.html` | 已取得 |
| 虎嗅 | `https://www.huxiu.com/article/4907330.html` | 已取得 |
| 果壳 / 网易（中文深度） | `https://m.163.com/dy/article/LC8JJRH505118OGM.html` | 已取得 |
| 搜狐 / DoNews | `https://m.sohu.com/coo/zmtq/1104385896_100106801` | 已取得 |
| 腾讯新闻 | `https://news.qq.com/rain/a/20261120A0GCJC00` | 已取得 |
| Zenodo「The Cathedral」 | `https://zenodo.org/records/20803093` | 受限 403 |

## 四、评论与讨论

| 平台 | URL | 状态 | 备注 |
|---|---|---|---|
| Hacker News 主帖 | `https://news.ycombinator.com/item?id=50574968` | 仅摘要 | 约 1601 条评论；逐字引文见 `notes/03-commentary.md` |
| MathOverflow | `https://mathoverflow.net/questions/518033/how-to-human-verify-enormous-ai-math-output` | 仅摘要 | 71 票，含「提取 0=1 有多难」 |
| Kevin Buzzard 博客 | `https://xenaproject.wordpress.com/2026/11/12/and-the-proof-is-checked-in-lean/` | 仅摘要 | 编译之外四问 |
| Scott Aaronson 博客 | `https://scottaaronson.blog/?p=10344` | 受限 403 | 转述见 `notes/04-backfill.md` |
| 知乎问题 | `https://www.zhihu.com/question/2107239235015263538` | 受限 403 | 718 万浏览、850 答；摘要见 `notes/03-commentary.md` |
| X / Reddit | 多条 | 受限 | 反爬；仅经媒体转述 |

## 五、检索记录

检索查询词与返回摘要保存在 `notes/` 的调研笔记末尾，可用于复查覆盖度。被反爬拦截的站点，逐字内容以各笔记里注明了出处的转述为准。
