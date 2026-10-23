# The Missing Lin(ni)k — Lean formalization

Lean proofs accompanying Paul Pollack's *The Missing Lin(ni)k*.

This repository contains the one-parameter triangular van Dijk argument for quadratic-character anti-concentration, its assembly into Linnik's theorem using the symmetry-formula argument, and a sharpened Selberg sieve estimate.

The complete source tree is provided in [missing-linnik-lean.zip](missing-linnik-lean.zip). Extract it before building; source paths below refer to the extracted directory.

## Main results

| Paper result | Lean declaration | Source |
| --- | --- | --- |
| Appendix 1: one-parameter bound | `MissingLinnik.CurrentPaper.one_parameter` | OneParameter.lean |
| Effective Proposition 2 | `MissingLinnik.CurrentPaper.proposition_two_effective` | CurrentPaper.lean |
| Theorem 1: Linnik's theorem | `MissingLinnik.CurrentPaper.linnik` | CurrentPaper.lean |
| Appendix 2: rough-number sieve | `MissingLinnik.CurrentPaper.rough_numbers_sieve` | SharpRough.lean |

The first theorem proves the explicit bound `(1/2 + 70000/t) log x` for `x >= f^(t^2)` and integers `t >= 3`. The constant is not optimized. The sieve theorem has relative error `16 exp(32-h)` and additive error `z^(3h)`.

## Build

Install Lean through elan, then run from this directory:

```sh
lake exe cache get
lake build CurrentPaper SharpRough Audit
```

The toolchain is pinned to Lean **4.35.0-rc3** and mathlib commit **3f6737de4761ec7bf368491fe9faccc991ebd6ca**. See `lean-toolchain`, `lakefile.toml`, and `lake-manifest.json`.

## Verification

The additions passed Lean elaboration and independent exported-proof kernel replay. A fresh sandboxed Comparator run accepted all four declarations above on 22 October 2026. Its output is in Verification/comparator-current.log.

The audited solution declarations depend only on `propext`, `Classical.choice`, and `Quot.sound`. There are no unfinished proofs in the solution modules. ChallengeCurrent.lean intentionally contains four statement-only `sorry` placeholders for Comparator; it is not imported by the solutions.

To reproduce the export comparison in an environment supporting the pinned toolchain's Comparator sandbox:

```sh
python Verification/export_for_comparator.py
```

This builds the challenge and solution, exports their dependencies, and invokes `lake comparator`. The recorded run used native Windows-generated exports and Ubuntu WSL's normal Comparator sandbox; it was not a clean Linux source rebuild. Export hashes are recorded in `Verification/export-sha256.txt`.

## Relation to the paper

The formalization reuses the prior triangular van Dijk proof's nonzero-minor, divisibility, and determinant-size arguments, improving the final slope error to linear. Internally it uses slope `1024t`, still with only one free parameter.

The main proof reuses the earlier symmetry-formula development and retains von Mangoldt weights until the final prime-power estimates. Its existing sufficient sieve constants remain in that proof. The sharper appendix sieve is proved separately.

The formal sieve uses integer parameters and the interval `0 <= n < N`. The paper uses asymptotic notation and positive integers `n <= t`; the routine real-parameter and endpoint conversions are not separately packaged here. This is verification of mathematical statements through the indicated proof ingredients, not a literal translation of every line or verification of historical and bibliographical claims.

See VERIFICATION.txt for details, formalization.yaml for structured metadata, and PRIOR_PROOF_MAP.txt for the reused modules. The latter describes the earlier project; `CurrentPaper.lean` replaces its final Green-route assembly with the triangular route.

## Authorship and licensing

The paper is by Paul Pollack. The formalization was developed by OpenAI Codex at his request, reusing the earlier project. No additional claim of human review is made. No distribution license has been specified for these sources; dependency licenses remain their respective owners'.

