# The Missing Lin(ni)k — Lean formalization

Lean proofs accompanying Paul Pollack's manuscript *The Missing Lin(ni)k*, an elementary proof of Linnik's theorem using Selberg symmetry formulas and quadratic-character anti-concentration.

**Linnik's theorem:** there is an absolute, effectively computable constant L such that, for every integer q ≥ 2 and every a coprime to q, some prime p ≡ a (mod q) satisfies p ≤ q^L.

The repository is distributed as a [source archive](missing-linnik-lean.zip). It contains the Lean source tree, the manuscript (`paper/main.tex`), build configuration, and verification records. The files visible at the repository root summarize that package. [Download the archive](missing-linnik-lean.zip) and extract it before using the source paths or commands below. Its SHA-256 checksum is in [missing-linnik-lean.zip.sha256](missing-linnik-lean.zip.sha256).

## Formalized results

All declarations in this table are in the namespace `MissingLinnik.CurrentPaper`.

| Result | Lean declaration | Source in the archive |
| --- | --- | --- |
| Linnik's theorem | `linnik` | `CurrentPaper.lean` |
| Effective quadratic-character anti-concentration | `proposition_two_effective` | `CurrentPaper.lean` |
| One-parameter triangular van Dijk bound | `one_parameter` | `OneParameter.lean` |
| Two-term symmetry estimate, with no upper cutoff on x | `symmetry_two_unbounded` | `CurrentPaper.lean` |
| Three-term symmetry estimate, with no upper cutoff on x | `symmetry_three_unbounded` | `CurrentPaper.lean` |
| Negative single and pair mass bound used to exclude positive character bias | `negative_mass_sign_bound` | `CurrentPaper.lean` |
| Rough-number sieve with integer parameters | `rough_numbers_sieve` | `SharpRough.lean` |
| Rough-number sieve with real h ≥ 18 | `rough_numbers_sieve_real_h` | `SharpRough.lean` |

The one-parameter theorem gives the explicit bound `(1/2 + 70000/t) log x` for `x ≥ f^(t²)` and integers `t ≥ 3`. The constants are not optimized. The sieve bounds have additive error `z^(3h)` and relative error `16 exp(32-h)` for integer h ≥ 17, or `16 exp(33-h)` for real h ≥ 18.

## Build and reproduce the checks

Install Lean through [elan](https://github.com/leanprover/elan), extract the source archive, and run these commands from the extracted directory:

```sh
lake exe cache get
lake build CurrentPaper SharpRough Audit
```

The configuration pins Lean **4.35.0-rc3** and mathlib commit **3f6737de4761ec7bf368491fe9faccc991ebd6ca**. See `lean-toolchain`, `lakefile.toml`, and `lake-manifest.json` in the archive.

To export the eight statements and compare their proofs, run Python in an environment supporting the pinned toolchain's Comparator sandbox:

```sh
python Verification/export_for_comparator.py
```

This script builds the challenge and solution modules, exports their dependencies, and invokes `lake comparator` with `comparator-current.json`.

## Verification

On **25 October 2026**, all **163 project Lean modules** compiled freshly against the cached, pinned dependencies. Fresh exports passed independent kernel replay with `leanchecker`, and the normal sandboxed Comparator accepted all **eight statements** listed above.

The recorded run used native Windows compilation and exports, followed by Comparator in Ubuntu WSL. It was not a clean Linux source rebuild. Build results, per-module logs, source and export hashes, and kernel and Comparator outputs are in `Verification/2026-10-25/` inside the archive.

The audited solution declarations depend only on `propext`, `Classical.choice`, and `Quot.sound`. Solution modules contain no unfinished proofs. `ChallengeCurrent.lean` contains eight intentional statement-only `sorry` placeholders for Comparator and is not imported by the solutions.

See [VERIFICATION.txt](VERIFICATION.txt) for the verification report and [formalization.yaml](formalization.yaml) for structured metadata.

## Relationship to the manuscript

The formalization checks the mathematical statements and proof ingredients described above. It is not a literal translation of every manuscript line, and Comparator checks Lean statements rather than interpreting TeX.

The proof uses a triangular determinant argument for anti-concentration, two- and three-term symmetry estimates, and a finite character argument assembling them into Linnik's theorem. The sign-exclusion step bounds negative single and pair mass directly. The sharper appendix sieve is proved separately from the sufficient sieve constants used in the main proof.

The precise correspondence has these conventions:

- The symmetry theorems use von Mangoldt weights and `B = 170k`. Prime-power estimates supply the connection to prime sums. A prime-only restatement for every integer B is not a separate formal theorem.
- The sieve allows real h ≥ 18 but retains integer z and the interval `0 ≤ n < N`. Conversions to arbitrary real z and the manuscript's positive-integer endpoint convention are not separately packaged.
- The determinant proof uses internal slope `1024t`. The character bridge uses a bounded integer representative rather than identifying the manuscript's exact fundamental discriminant.

`Verification/manuscript-alignment.md` in the archive explains these correspondences. Historical and bibliographical claims are outside the formal verification.

## Authorship and licensing

The paper is by Paul Pollack. The formalization was developed by OpenAI Codex at his request. No additional claim of human review is made. No distribution license has been specified for these sources; dependency licenses remain their respective owners'.
