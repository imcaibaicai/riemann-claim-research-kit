# 逆推证明骨架

> 目的：回答「这段 Lean 代码到底编译的是哪一段证明」。以下全部来自解包后的真实源码与 MIT 验证页，按顶层依赖链逐层下探。

## 一、五个顶层定理（`lean/src/Main.lean`，全文 12 行）

```lean
import Statements
import Supplements.Kernels.MainTheoremsAll

theorem riemannHypothesis : RiemannHypothesis := rhOfAll
theorem riemannZeta_eq_zero_re : RiemannZetaZerosOnLine := rhOfAll
theorem LFunction_ne_zero_of_half_lt_re : DirichletLNonvanishingRightOfHalf := allChi
theorem LFunction_eq_zero_re : DirichletLZerosOnLine := claimAll.2.1
theorem LFunction_eq_zero_re_of_isPrimitive : PrimitiveDirichletLZerosOnLine := critAll
```

`RiemannHypothesis` 不是本项目自定义，而是 Mathlib 自身定义（`Mathlib/NumberTheory/LSeries/RiemannZeta.lean`）：

```lean
def RiemannHypothesis : Prop :=
  ∀ (s : ℂ) (_ : riemannZeta s = 0)
    (_ : ¬∃ n : ℕ, s = -2 * (n + 1)) (_ : s ≠ 1), s.re = 1 / 2
```

`lean/src/Statements.lean` 另定义四个命题：`RiemannZetaZerosOnLine`、`DirichletLNonvanishingRightOfHalf`、`DirichletLZerosOnLine`、`PrimitiveDirichletLZerosOnLine`。其中“GRH”在这里**仅指 Dirichlet L 函数**（MIT 验证页原话）。

## 二、依赖链逐层下探

第 1 层 `lean/src/Supplements/Kernels/MainTheorems.lean`：

```lean
theorem allChi : UpperTheory.EveryCharacter := UpperTheory.FinalTheorems.everyCharacter
theorem rhOfAll : RiemannHypothesis := Formalism.VW.rh_of_every allChi
theorem critAll ... := Formalism.VW.critLine allChi N χ hχ s h0 h1 hz
theorem claimAll : Claim.HalfPlane ∧ Claim.Strip ∧ Claim.Zeta :=
  ⟨allChi, Claim.strip allChi, Claim.zeta allChi⟩
```

第 2 层 `lean/src/Formalism/GammaFactorNonvanishing/CritLine.lean`：

- `Formalism.VW.rh_of_every` 把「所有 Dirichlet 特征在 Re>1/2 非零」经 `rh_of_right` 归约到 RH；对平凡特征用 `LFunction_modOne_eq`。
- `Formalism.VW.critLine` 用完备化 L 函数函数方程与 gamma 因子在 Re>0 非零，把右半平面的零点推到临界线。这一段是**经典论证**，与四页说明里「已经让人最有成就感的黎曼影子之一变得简单」相对应。
- 两个定理的完整源码各只有 32 行与 6 行，可直接通读。

第 3 层 `lean/src/Development/AutomorphicArithmetic/CharacterBoxes/MainConclusion.lean`：

```lean
theorem UpperTheory.FinalTheorems.everyCharacter_of_row
  (h : UpperTheory.GalStar.Row0b fun h rc => UpperTheory.Zero.LLC.IsRecT h rc)
  : UpperTheory.EveryCharacter :=
  UpperTheory.LiteratureTheorems.every_character_of_literature_of_vWin
    ⟨h, UpperTheory.Zero.LLC.recTExists, h⟩ (UpperTheory.One.vwin_of_row h)

theorem UpperTheory.FinalTheorems.everyCharacter : UpperTheory.EveryCharacter :=
  UpperTheory.FinalTheorems.everyCharacter_of_row
    UpperTheory.Gal.Int.galoisLocalGlobalStrong_recParameter
```

第 4 层（核心入口）`lean/src/Development/AutomorphicArithmetic/Foundations/GaloisRepresentations/IntegralStructures/RowSrc.lean`：

```lean
theorem galoisLocalGlobalStrong_recParameter : UpperTheory.GalStar.Row0b recT :=
  galoisRepresentations_of_thetaLift UpperTheory.TP.K.tLiftK
```

再往下就是约 17.4M 行的主体。**逆推时应从这里开始，按 `tLiftK`、`Row0b`、`recTExists`、`vwin_of_row`、`every_character_of_literature_of_vWin` 五个名字展开。** 批量定位结果见 `sources/skeleton-defs.txt`。

## 三、代码模块规模（脚本按顶层目录统计）

| 顶层目录 | 文件数 | 行数 |
|---|---|---|
| Development | 141928 | 12232663 |
| Formalism | 18254 | 3200748 |
| GroundTheory | 8507 | 1119138 |
| ReciprocityTheory | 3845 | 563016 |
| CompanionTheory | 2027 | 194218 |
| PositivityTheory | 449 | 55130 |
| Lemmata | 167 | 9232 |
| BuildAid | 5 | 20 |
| Supplements | 3 | 22 |
| 顶层两个文件 | 2 | 28 |
| 合计 | 175187 | 17374215 |

## 四、与四页说明时间线的对应关系（推断，未验证）

四页说明给出的 20 步时间线（原文见 AGENT.md 第 1 步取得的 PDF 文字层）里，可辨认的主题与代码顶层目录大致对应：`theta lift / automorphic` 对应 `Development/AutomorphicArithmetic` 与 `CompanionTheory`；`reciprocity / Galois` 对应 `ReciprocityTheory` 与 `Development/AutomorphicArithmetic/Foundations`；`Bessel` 对应 `CompanionTheory/AutomorphicTheory/Bessel*`；`positivity / W(1S)≥0` 对应 `PositivityTheory`；`Stepanov 行列式` 与 `qN = M^k + R` 散布在 `Development` 深处。**这只是命名层面的对应猜测，没有任何验证。**

## 五、四条逆推线索（供后续人工核对）

1. 顶层定理用的是 Mathlib 自己的 `RiemannHypothesis`，不是项目自定义；自定义的是 zeta 零点与 Dirichlet L 函数的四个命题。
2. 五个定理只依赖三条标准公理；环境里虽有 15 条公理（含 `sorryAx`），但顶层定理不依赖它们。见 AGENT.md 第 5 步取得的 `audit.log`。
3. 有一份 `challenge.ndjson.zst`（五个命题带 sorry，仅 import Mathlib）和一份 `solution.ndjson.zst`（7.7GB，未下载）。challenge 与 solution 的 statement matching 已由 con-leche `--compare` 检查——这是「代码编译的命题是否与声称一致」的直接证据来源。
4. MIT 页明确提醒：通过的检查只说明「按所写形式陈述、依据所用内核，从三条标准公理推出」，仍依赖内核与 Mathlib 定义的正确性；且构建未沙箱化，完整沙箱化导出比对尚未做。
