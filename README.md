# Ince: periodic ODE groundwork in Lean 4

[![Lean verification](https://github.com/xyli9596/ince-ordinary-differential-equations-lean/actions/workflows/lean.yml/badge.svg)](https://github.com/xyli9596/ince-ordinary-differential-equations-lean/actions/workflows/lean.yml)

This small, standalone project formalizes elementary steps in E. L. Ince's
*Ordinary Differential Equations*, §15.7, printed pages 381–382.
It is **groundwork for Floquet theory**, not a formalization of the full Floquet
or Frobenius theorem, and not a verification of the entire book.

中文：独立整理 Ince §15.7 的周期平移与指数因子分解。当前只有下表中的辅助结论；
完整基本解组的存在性、一般 Jordan 链及正则奇点理论尚未完成。

## Book-to-Lean correspondence

All statements and proofs are in [IncePeriodic.lean](IncePeriodic.lean).
The section has prose derivations rather than numbered theorem labels; no
book theorem number is invented here.

| Book location | Lean declaration | Exact verified scope |
| --- | --- | --- |
| §15.7, p. 381, opening translation step | `solves_translate` | An actual solution of a scalar linear ODE with periodic coefficients remains a solution after translation by a period. |
| §15.7, p. 382, exponential factor removal | `periodic_factor`, `reconstruct_factor` | Given the translation eigenfunction identity, multiplication by the reciprocal exponential gives a periodic factor and the exact reconstruction. |
| §15.7, p. 382, simple multiplier calculation | `eigenfunction_factorization` | Given an **entire** eigenfunction, a nonzero period and nonzero multiplier, constructs an exponent and an **entire periodic** factor. The eigenfunction identity is an explicit hypothesis. |
| §15.7, p. 382, first repeated-root step | `periodic_of_jordan_step`, `entire_jordan_remainder` | If `p` is periodic and `q(z+h)=q(z)+p(z)`, then `q(z)-(z/h)p(z)` is periodic; it is entire when `p,q` are entire. This is the first two-function step only. |

The code allows an arbitrary nonzero complex period. The book normalizes the
period to a real number. The first Jordan calculation uses subtraction rather
than dividing by a possibly vanishing eigenfunction.

Not yet proved: construction of the finite-dimensional solution space and
its monodromy operator; existence of a Floquet basis; the full arbitrary-length
Jordan decomposition; descent to the punctured plane; the local analytic
and regular-singular results used in §15.23 and pp. 396–402. Statements requiring
these results must not cite this repository as a complete proof of them.

## Reproduce

Install Git, Python 3.9+ and [elan](https://github.com/leanprover/elan):

```sh
lake exe cache get
python3 verify.py --jobs 2
```

Alternatively `lake build` compiles the sources and the unified axiom audit.
The toolchain is Lean 4.33.0 and mathlib is pinned to
`db584cd6d46c92f209a44c0f1c829460d327499d` in `lake-manifest.json`.
No personal filesystem path is needed. The verifier recompiles both local
modules and checks the six named declarations against the allowed axioms
`propext`, `Classical.choice`, and `Quot.sound`; it rejects `sorry`, `admit`,
custom axioms and `native_decide` proof commands. Six declarations means six
code lemmas, not six complete book theorems.

[The recorded local verification](verification/verification.json) and the
GitHub Actions status provide separate evidence. A local pass does not imply
that the hosted Linux run has finished.

## Source and reuse

Book reference: E. L. Ince, *Ordinary Differential Equations*, Longmans, Green
and Co., 1926; Dover unabridged reprint. The printed pages were checked against
[this public scan](https://archive.org/details/ordinarydifferen029666mbp).
No book scan or paper PDF is distributed here.

Developed while formalizing X.-Y. Li, J. Wang and Z.-T. Wen,
“Representation of finite-order solutions to linear differential equations
with exponential-sum coefficients,” *Journal of Differential Equations* 423
(2025), 118–132, [doi:10.1016/j.jde.2024.12.035](https://doi.org/10.1016/j.jde.2024.12.035).
That paper's general representation theorems are not yet verified by this work.

The Lean code is Apache 2.0. OpenAI's ChatGPT and Codex assisted in proof
construction, debugging and documentation. This is independent of upstream
mathlib. Cite an exact commit for reproducibility; no DOI is claimed.

Related independent projects: [Levin](https://github.com/xyli9596/levin-entire-functions-lean)
and [Wasow](https://github.com/xyli9596/wasow-asymptotics-lean).
